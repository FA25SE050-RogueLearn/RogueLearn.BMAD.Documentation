# Story 4.11: WebGL Clients + Headless Server (Relay)  Implementation Checklist

## Status
In Progress

## Ownership
* **Unity Game/Netcode:** Tan
* **Infra/Deployment:** Tan / DevOps

## Target Deadline
TBD (align with multiplayer milestone)

## Story
**As a** developer,
**I want** to run an authoritative headless server that advertises a Unity Relay join code,
**so that** WebGL clients can connect securely over wss and play multiplayer.

## Acceptance Criteria
1. A Linux headless (UNITY_SERVER) build auto-starts and logs a Relay join code on launch.
2. WebGL clients can enter a join code and connect via Relay (wss) and reach gameplay.
3. Server-authoritative systems (QuizManager, ReadyStation auto-ready) work with multiple clients.
4. WebGL builds hide/disable Host UI; clients see only the Join-by-code flow.
5. A basic deployment pipeline exists (headless server Docker image -> cloud -> logs available).

## Dependencies
- Unity 2022.3 LTS
- Packages: netcode.gameobjects (1.11.0), transport (2.0.2), services.core, services.authentication, services.relay (1.2.0)
- HTTPS hosting for WebGL (to allow wss)

## Tasks / Subtasks
- [ ] Packages: Verify/pin `com.unity.services.core`, `com.unity.services.authentication`, `com.unity.services.relay`, `com.unity.transport` 2.x, `com.unity.netcode.gameobjects`.
- [ ] Server Bootstrap (UNITY_SERVER): Add `Assets/Scripts/Network/ServerBootstrap.cs` to init services, create Relay allocation, configure `UnityTransport` (dtls), and `StartHost()`.
- [ ] Scenes & NetworkManager: Ensure gameplay scene(s) and a configured `NetworkManager` are included in server build.
- [ ] WebGL Join Flow: Add UI to input join code; init services; `JoinAllocationAsync(joinCode)`; configure `UnityTransport` for **wss**; `StartClient()`.
- [ ] Platform Gating: Hide/disable Host UI in WebGL builds; expose only Join.
- [ ] Deployment: Build Linux headless, wrap in Docker, deploy to cloud (e.g., Azure Container Apps). Capture join code from logs.
- [ ] Verification: Connect 2+ WebGL clients; validate movement sync, ReadyStation auto-ready, question/answer flow; clean disconnects and session end.
- [ ] Observability: Add basic structured logs or health endpoint for orchestration.

## Notes
- Headless must run `StartHost()` to bind to Relay as host. NGO `StartServer()` alone does not publish a Relay endpoint.
- Serve WebGL over HTTPS so browsers allow wss transport.

## References
- Full guide: `docs/unity-content/headless-server-bootstrap-relay.md`
- Architecture overview: `docs/fullstack-architecture/unity-architecture.md` (Implementation Update  Headless Host via Relay)

### Orchestration & Lifecycle Tasks
- [ ] Add server lifecycle states (Starting, WaitingForPlayers, InMatch, Finalizing, Closing) and auto-shutdown after match.
- [ ] Integrate Lobby or a minimal allocator endpoint to publish join code to clients.
- [ ] Define late-join policy (allow/deny) and enforce MaxConnections.
- [ ] Add structured logs for join code, player count, match start/end.
- [ ] Document scaling plan (one container per room) in `unity-content/room-lifecycle-and-orchestration.md`.

### Match Results Logging Tasks
- [ ] Implement ServerMatchRecorder on server to capture players, questions, attempts, and scoreboard.
- [ ] File sink: write `<LOG_ROOT>/matches/<date>/match_<matchId>.json` with schema from `unity-content/match-results-logging.md`.
- [ ] Env-config: RESULTS_SINK, RESULTS_LOG_ROOT, STORE_RAW_QUESTION_TEXT.
- [ ] Acceptance: run a match with 2 WebGL clients; verify JSON file contents and correctness tallies.
- [ ] Optional: HTTP sink to backend (RESULTS_HTTP_URL, RESULTS_HTTP_TOKEN).

### Results Persistence Tasks
- [ ] Define MatchResult/QuestionEvent schema in `4.12.match-results-api.md` (draft created).
- [ ] Add server-side posting on Finalizing using UnityWebRequest (HTTPS) with retries + idempotency.
- [ ] Configure `RESULTS_API_BASE_URL` and `RESULTS_API_KEY` in server environment.
- [ ] Decide whether to store full question text/options or only hashes + pack references.
- [ ] Add structured logs for results posting and error handling.
- [ ] Write an integration test: run a match with 2 WebGL clients, verify one record stored in DB.
