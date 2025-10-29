# Headless Server Bootstrap with Unity Relay (WebGL Clients)

Status: Adopted � We will run an authoritative headless server (host-mode) using Unity Relay so WebGL clients can join via join code over secure WebSockets (wss).

Why
- Browsers cannot host servers (no UDP/listening sockets). WebGL must be Client-only.
- Unity Relay provides a secure transport path: WebGL clients connect over wss; the server runs as a Relay host.
- Netcode for GameObjects (NGO) handles state sync; server authority keeps gameplay fair.

Key Decisions
- Dedicated Server runs as **Host** (even headless) to bind to Relay. NGO `StartServer()` does not publish a Relay endpoint.
- Use `UNITY_SERVER` build with `Application.isBatchMode` guard to auto-start the server.
- Relay allocation exposes a **join code** that clients use to connect.

Package Requirements (unity/Packages/manifest.json)
- com.unity.netcode.gameobjects: 1.11.0
- com.unity.transport: 2.0.2
- com.unity.services.core: latest (verified for 2022.3 LTS)
- com.unity.services.authentication: latest
- com.unity.services.relay: 1.2.0

Server Bootstrap Implementation
A static bootstrap runs before scene load on headless builds and:
1) Initializes Unity Services
2) Signs in anonymously
3) Creates a Relay allocation and logs the join code
4) Configures UnityTransport with RelayServerData (dtls)
5) Starts Host (headless)

Code snippet (located at `Assets/Scripts/Network/ServerBootstrap.cs` in the game repo):

```csharp
#if UNITY_SERVER
using Unity.Netcode;
using Unity.Netcode.Transports.UTP;
using Unity.Services.Core;
using Unity.Services.Authentication;
using Unity.Services.Relay;
using Unity.Services.Relay.Models;
using Unity.Networking.Transport.Relay;
using UnityEngine;

[RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.BeforeSceneLoad)]
static async void InitializeHeadlessServer(){
  await UnityServices.InitializeAsync();
  await AuthenticationService.Instance.SignInAnonymouslyAsync();
  var allocation = await RelayService.Instance.CreateAllocationAsync(20);
  var joinCode = await RelayService.Instance.GetJoinCodeAsync(allocation.AllocationId);
  Debug.Log($"Relay Join Code: {joinCode}");
  var transport = (UnityTransport)NetworkManager.Singleton.NetworkConfig.NetworkTransport;
  transport.SetRelayServerData(new RelayServerData(allocation, "dtls"));
  NetworkManager.Singleton.StartHost();
}
#endif
```

Build & Deploy (Linux Headless)
- Build target: Linux x86_64, **Server Build** checkbox enabled (produces headless binary)
- Scenes: Include gameplay scene(s) in Build Settings (server should spawn into gameplay immediately)
- Environment: Run with `-batchmode -nographics` (Unity defaults for server build)
- Container: Wrap in Docker image for Azure Container Apps / Instances. Mount log volume or stream to stdout.

Runtime Behavior
- On start, server logs: `Relay Join Code: XXXXX`
- Clients use this join code to connect via Relay
- If Relay init fails, server logs the exception and exits (restart policy recommended)

WebGL Client Flow (overview)
- Initialize Unity Services; sign in anonymously
- Join Relay with code; configure UnityTransport for **wss**
- StartClient()
- Host/Server state remains authoritative (Quiz, ReadyStation, Boss logic)

Operational Notes
- Serve WebGL builds over HTTPS to allow wss connections
- Prefer region selection near users for lower latency
- Capture join code for orchestration (expose via logs or simple HTTP endpoint if needed)

Acceptance Check
- With the server running, at least two WebGL clients can join using the join code
- Movement/animations replicate correctly; ReadyStation auto-ready logic behaves server-authoritatively
- Session completes; results can be posted to backend

## Room Model & Lifecycle

There are two common ways to host matches ("rooms") with the headless server:

1) Single-room per process (recommended initially)
- Each headless server process creates exactly ONE Relay allocation (one join code) at startup.
- That allocation represents a single match/room with a fixed MaxConnections (e.g., 20).
- Lifecycle:
  - Boot  create allocation  log join code  wait for players (Ready Station majority)  run match  finalize  shutdown.
- Pros: simplest; easy to scale horizontally; clean isolation per match.
- Ops: use a container orchestrator to spin up one container per match request and to terminate after match ends.

2) Multi-room per process (advanced; usually avoided)
- A single Unity process handles multiple matches concurrently.
- Would require multiple NetworkManager/Transport instances and separate Relay allocations per room, or a custom multiplexing layer.
- NGO is primarily designed for one active NetworkManager per process; multi-room increases complexity and risk.
- Recommendation: avoid; scale with single-room-per-process containers instead.

### How players get a room
- Without Lobby: the server prints a join code on startup; clients enter this code in the WebGL UI and connect.
- With Lobby/Allocator (preferred): a control-plane service (matchmaker/allocator) starts a server, retrieves the join code (e.g., via logs or a small HTTP endpoint), registers it in a Lobby, and returns the lobby/join code to clients.

### Join Code distribution patterns
- Lobby-based: Server posts join code to Lobby; clients discover/join via Lobby.
- Backend API: Server exposes join code via a small health/metadata endpoint; web backend returns it to the frontend.
- Manual (dev): Read logs in the server console and paste into the client.

### Lifecycle states (example)
- Starting: server initializing, creating Relay allocation.
- WaitingForPlayers: join code published; waiting for Ready Station majority.
- InMatch: questions/combat running; authoritative state.
- Finalizing: match ends; post results (optional).
- Closing: disconnect clients; call `Application.Quit()` to terminate the container.

### Auto-shutdown
- Recommended: when all clients disconnect or after match completes, the server should exit to free resources (or restart to serve a new match).

### Scaling & Regions
- One container per room  horizontal scaling via orchestrator.
- Choose Relay region closest to your users for lower latency.
- Optionally pre-warm a small pool of servers to reduce cold-start time.

## Match Result Persistence
- The headless server records match results server-side to validate which player played and which questions they answered.
- See `unity-content/match-results-logging.md` for logging guidance and request examples.
- See `docs/4.12.match-results-api.md` for cross-service schemas, idempotency guidance, and security.
- Default (dev): File sink with per-match JSON under a logs directory; optional HTTP sink to a backend.

## Match Results Persistence (Server-side)

To verify generated questions and track player performance, the **headless server** should post match results to a backend database.

Recommended approach:
- Server-authoritative: The headless server records question events and posts a summary at match end.
- Transport: Use HTTPS (UnityWebRequest) with an API key or JWT for auth.
- Reliability: Implement retry with backoff; mark payloads with `match_id` for idempotency.
- Privacy: Only store needed fields; consider hashing sensitive content.

### Event fields (per question)
- sequence: incremental index within the match.
- player_id: Unity Authentication ID or your platform ID.
- question_id: e.g., `packId:itemId` or generator ID.
- question_pack_id: source pack ID (if applicable).
- generator_seed: deterministic seed (for generated content), optional.
- prompt_text or prompt_hash: store full text or a hash for audit.
- options: array of `{id, text}` or `{id, text_hash}`.
- correct_option_id: canonical correct choice.
- chosen_option_id: players chosen choice.
- is_correct: boolean.
- answered_at: ISO timestamp.
- latency_ms: time from show to answer.

### Match summary fields
- match_id: UUID.
- relay_join_code: for traceability.
- region: Relay region used.
- started_at, ended_at: ISO timestamps.
- players: array of `{player_id, display_name?, joined_at, left_at, score, accuracy}`.
- questions_total, correct_total.

### Example payload (summary + events)
```json
{
  "match_id": "b4f6b2e1-2a1d-4df7-9f5c-1a9c3a10ad01",
  "relay_join_code": "ABCD",
  "region": "us-central",
  "started_at": "2025-10-28T12:34:56Z",
  "ended_at": "2025-10-28T12:47:05Z",
  "players": [
    {"player_id":"auth:12345","display_name":"Alice","joined_at":"2025-10-28T12:35:10Z","left_at":"2025-10-28T12:47:05Z","score":12,"accuracy":0.8}
  ],
  "question_events": [
    {
      "sequence":1,
      "player_id":"auth:12345",
      "question_id":"pack1:Q42",
      "question_pack_id":"pack1",
      "prompt_hash":"sha256:...",
      "options":[{"id":"opt1","text_hash":"sha256:..."},{"id":"opt2","text_hash":"sha256:..."}],
      "correct_option_id":"opt2",
      "chosen_option_id":"opt1",
      "is_correct":false,
      "answered_at":"2025-10-28T12:36:01Z",
      "latency_ms":1532
    }
  ],
  "questions_total": 10,
  "correct_total": 8
}
```

### Implementation notes
- Use environment variables:
  - `QUESTS_API_BASE`: e.g., `https://api.roguelearn.local/quests`
  - `RESULTS_API_KEY`: secret key for server-to-server auth.
  - Optional: Bearer JWT for client-originated events
- Post on Finalizing (match end). Optionally stream events during gameplay if needed.
- Add structured logs: posted payload size, status codes, error messages.
 - Idempotency:
   - Results: `Idempotency-Key: result::<sessionId>`
   - Events: `Idempotency-Key: event::<sessionId>-<sequence>`

### Adoption notes
- Identity: include both `player_id` (Unity/NGO) and `auth_user_id` (platform UUID) when known
- Content storage: static packs may be hashed; dynamic content should store full text for audit
- Retention: detailed events 30–90 days; summaries long-term
- Late joins: record join/leave timestamps per player; summarize partial participation
