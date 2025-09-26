# Unity Architecture

This page centralizes the Unity game client architecture for RogueLearn, complementing the PRD Technical Guidance and the Fullstack Architecture views.

- Unity Version: 2022.3 LTS (pinned)
- Primary Role: Interactive "Boss Fight" experiences
- Primary Targets: WebGL (client), Linux Headless (dedicated server)
- Related: See PRD Unity Multiplayer — Phased Plan for delivery sequencing and exit criteria

References
- PRD: ../prd/technical-guidance.md#unity-game-client--multiplayer--phased-plan
- Components: ./components.md
- Frontend Integration: ./frontend-architecture.md
- Tech Stack: ./tech-stack.md
- API Spec (Game endpoints): ./api-specification.md

## Runtime & Build Targets

- WebGL Client
  - Hosted on Supabase Storage + CDN
  - Structure (Unity default): Build.loader.js, Build.framework.js, Build.data, Build.wasm
  - Embedded in Next.js via react-unity-webgl; see Frontend Integration
  - Auth/Init: JWT passed via JS bridge to fetch/start sessions

- Linux Headless (Authoritative Server)
  - Target: Linux x86_64, headless (batchmode, nographics)
  - Containerized for Azure Container Apps or Azure Container Instances
  - Lifecycle: session-oriented processes (spin-up per match or pooled workers)

## Multiplayer Topology by Phase (Summary)

- Phase 1 — Single-player WebGL
  - No networking; all logic client-side; results posted to backend
- Phase 2 — Client-hosted (UGS Lobby + Relay with Netcode for GameObjects)
  - Host is also a client; small parties (<= 12) for study co-op and duels
  - Relay provides WSS transport; Lobby handles discovery/join code; NGO for state sync
  - Host migration plan for resilience
- Phase 3 — Dedicated Authoritative Server
  - Unity headless instance holds source of truth; clients are thin
  - Target up to ~20 players per instance (tune during testing)

Details, deliverables, and exit criteria are maintained in the PRD: ../prd/technical-guidance.md#unity-game-client--multiplayer--phased-plan

## JS Bridge & API Boundaries

- Embedding
  - Next.js component wraps WebGL build and initializes via Unity loader
  - React listens to Unity events (e.g., GameOver) and forwards to backend
- Initialization
  - Frontend obtains a JWT (Supabase) and session token, passes to Unity via SendMessage / event plumbing
- Core API Endpoints
  - Start Session: POST /game/sessions (returns sessionId, seed, questions)
  - Complete Session: POST /game/sessions/{sessionId}/complete (score, breakdown)
  - See ./api-specification.md for schemas and security

## Unity Gaming Services (UGS) Configuration (Phase 2)

- Lobby
  - Region selection (auto/nearest), join codes, metadata for session type
- Relay
  - Secure WSS transport; allocation per session; QoS data for region choice
- Netcode for GameObjects
  - Host mode for small groups; snapshot/Delta sync configured per entity type
  - Interest management and tick rate tuned for academic mini-games

## Dedicated Server Architecture (Phase 3)

- Process Model
  - One process per match (simplest), or a multi-room server with a room scheduler
- Deployment Targets
  - Azure Container Apps (ACA) or Container Instances; horizontal scale via KEDA-like triggers
- Session Lifecycle
  - Matchmaking -> allocate server -> pass connection info -> run -> finalize -> persist results
- Observability
  - Health endpoints, structured logs, minimal metrics (tick rate, RTT, dropped packets)

## CI/CD (Unity)

- CI
  - GitHub Actions builders for WebGL and Linux Headless
  - Cache Library/ artifacts where possible to reduce build time
  - Deterministic versions (Unity hash + project version) for reproducibility
- CD
  - WebGL
    - Upload to Supabase Storage under versioned path: games/boss-fight/<game_version>/
    - Set Cache-Control headers (immutable with versioned paths)
  - Headless
    - Build Docker image tagged with unity-<unity_version>-game-<game_version>
    - Push to registry; deploy via ACA workflow files

## Asset Delivery & CDN

- Game builds and assets stored in Supabase Storage and served via CDN
- Versioned directories to enable cache-busting by path
- Static compression where supported (WASM streaming, Brotli for JS)

## Performance & Memory Budgets (Guidance)

- WebGL
  - Initial load target: <= 15–20 MB (optimize textures, strip unused code)
  - Runtime memory budget: start at 256–512 MB; validate on low-memory devices
  - Frame-time target for mini-games: 16–33 ms (60–30 FPS range)
- Networking
  - P2 NGO: tune tick rate and message sizes to remain stable on average student WiFi
  - P3 server: validate player count and fairness before increasing cap

## Security & Fairness

- P1 (Client-side): server validates scoring ranges and question integrity
- P2: host migration and basic tamper checks; sensitive calculations remain server-validated where possible
- P3: authoritative simulation on server; clients provide inputs only; anti-exploit checks in server loop

## Versioning & LTS Strategy

- Unity 2022.3 LTS pinned; upgrade only between verified patch versions
- Package versions locked; keep NGO/UGS packages within supported matrix
- Semantic versioning for game content and protocol changes; backward-compat gates where needed

## Architecture Diagrams

### Phase 2 — Client-hosted (UGS Lobby + Relay + NGO)
High-level topology where one player also acts as the host; discovery through Lobby, transport via Relay, state sync with Netcode for GameObjects.

```mermaid
flowchart LR
  subgraph Clients
    C1[Client A]:::client
    C2[Client B]:::client
    C3[Client C]:::client
  end

  L[UGS Lobby]:::ugs
  R[UGS Relay (WSS)]:::ugs
  H[Host Client (NGO Host)]:::host

  C1 -- "Join via code" --> L
  C2 -- "Join via code" --> L
  C3 -- "Join via code" --> L
  L -. "Join data / host info" .- C1
  L -. "Join data / host info" .- C2
  L -. "Join data / host info" .- C3

  C1 -->|WSS| R
  C2 -->|WSS| R
  C3 -->|WSS| R
  R -->|WSS| H

  %% NGO state sync occurs over Relay between clients and host
  classDef client fill:#E6F7FF,stroke:#1890FF,color:#000;
  classDef ugs fill:#FFF7E6,stroke:#FA8C16,color:#000;
  classDef host fill:#F6FFED,stroke:#52C41A,color:#000;
```

### Phase 3 — Dedicated Authoritative Server
High-level topology where an authoritative headless server runs the simulation; clients are thin and connect directly to the server after allocation.

```mermaid
flowchart LR
  subgraph Clients
    C1[Client A]:::client
    C2[Client B]:::client
    C3[Client C]:::client
  end

  MM[Matchmaker / Allocator]:::svc
  DS[Unity Dedicated Server Headless]:::server
  API[(Backend API / Persistence)]:::svc

  C1 -->|Match request| MM
  C2 -->|Match request| MM
  MM -->|Server endpoint + token| C1
  MM -->|Server endpoint + token| C2

  C1 -->|NGO transport| DS
  C2 -->|NGO transport| DS

  DS -->|Results / telemetry| API

  classDef client fill:#E6F7FF,stroke:#1890FF,color:#000;
  classDef server fill:#F6FFED,stroke:#52C41A,color:#000;
  classDef svc fill:#FFF7E6,stroke:#FA8C16,color:#000;

## CurriculumPack Delivery Flow (Unity)

This section details how the Unity client consumes vetted AI-generated content packs via backend APIs — using ephemeral, in-memory generation (no pack persisted).

1) Start session
- Call POST /game/sessions with questId (and optionally subjectId/seed/provider hints).
- Backend creates GameSession (status: InProgress), generates CurriculumPack in-memory, and returns { sessionId, pack, gameBuildUrl }.

2) Launch client and sync server
- Initialize Unity WebGL embed and pass { sessionId, pack } via the JS bridge.
- Game Server obtains the same ephemeral pack from Quests Service by sessionId for server-authoritative validation.

3) Play & submit
- Unity renders items from the ephemeral pack; server validates answers against its copy.
- On completion, call POST /game/sessions/{sessionId}/complete with score and progressData; ephemeral pack is discarded.

Integration notes
- Determinism: Use session-bound ephemeral pack across client and server during the match; no DB snapshot required.
- Caching: Cache only in memory for the session; invalidate on session end.
- Security: Never accept raw AI output client-side; only consume backend-vetted packs provided via session start.

## Ephemeral Pack Consumption & Provider Abstraction

To support dynamic generation per attempt without persistence, the Unity client uses a provider abstraction.

```csharp
public interface IPackProvider {
  Task<CurriculumPack> GetPackAsync(GameSession session);
}

public sealed class RemoteEphemeralPackProvider : IPackProvider {
  private readonly IApiClient _api; // wraps API Gateway calls
  public RemoteEphemeralPackProvider(IApiClient api) { _api = api; }

  public async Task<CurriculumPack> GetPackAsync(GameSession session) {
    // Preferred flow: POST /game/sessions/ephemeral returning { session, pack }
    var result = await _api.StartEphemeralSessionAsync(new {
      questId = session.quest_id,
      seed = session.seed,
      packType = "BossFightQuestionPack",
      providerPreference = "Gemini",
      fallbackEnabled = true
    });
    return result.pack; // vetted by backend JSON Schema
  }
}

public sealed class LocalFallbackPackProvider : IPackProvider {
  public Task<CurriculumPack> GetPackAsync(GameSession session) {
    // Minimal offline pack for resilience
    var pack = new CurriculumPack {
      meta = new CurriculumPackMeta { version = "1.0.0", source = "Seed", ephemeral = true },
      items = new [] {
        new CurriculumQuestion { id = "q1", type = "MCQ", text = "2+2?", options = new[]{ new CurriculumOption{ id="a", text="3"}, new CurriculumOption{ id="b", text="4"} }, correctAnswers = new[]{ "b" }, timeLimitSec = 20 }
      }
    };
    return Task.FromResult(pack);
  }
}
```

Provider selection & fallback semantics
- Preferred: RemoteEphemeralPackProvider using POST /game/sessions/ephemeral
- If remote fails or validation errors occur, switch to LocalFallbackPackProvider for continuity
- Log providerInfo (chosenProvider, fallbackUsed) for analytics; expose in HUD if desired

Frontend bridge
- The web app obtains JWT and passes session bootstrap info to Unity
- Unity calls the provider to fetch the ephemeral pack, plays the boss fight, and posts completion via /game/sessions/{sessionId}/complete

API references
- POST /game/sessions/ephemeral
- POST /game/sessions/{sessionId}/complete

Data model reference
- See BossFightQuestionPack specialization and ephemeral flag in Data Models