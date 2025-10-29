# Room Lifecycle & Orchestration (Unity Headless + Relay)

## Overview
We use a **single-room per process** model: each headless server process creates one Relay allocation (one join code) and hosts one match. Clients connect via join code over wss.

## Control Plane Options
- Manual (dev): start server, read join code from logs, share code.
- Lobby-driven: server registers join code in Unity Lobby; clients discover and auto-join.
- Backend allocator: web backend spins a server container, reads join code (logs/endpoint), returns code to clients.

## Reference Flow (Allocator + Lobby)
1) Player requests "Start Match" in the web app.
2) Backend calls Allocator  spins a server container (Unity headless).
3) Server boots  creates Relay allocation  gets join code  posts join code to Lobby.
4) Backend returns Lobby or join code to clients.
5) Clients connect via wss and enter gameplay.
6) On match end, server posts results to backend and exits; container is recycled.

## Server States
- Starting  WaitingForPlayers  InMatch  Finalizing  Closing

## Capacity & Limits
- MaxConnections set per allocation (e.g., 20). Enforce on server to avoid overload.
- Late-join policy configurable: allow/deny joins after match start.

## Shutdown Policy
- Exit on match completion or when all clients disconnect for X seconds (idle timeout).

## Security
- Only expose join codes via Lobby or authenticated API.
- Consider short TTL for codes and rate limiting joins.

## Observability
- Emit structured logs: join code, player count, match start/end, errors.
- Optional health endpoint: returns room state for allocator.

## Results & Audit
During Finalizing, the server should compile a match summary and post it to the backend for storage and review.

Minimum fields: `match_id`, timestamps, players, question events (ids + correctness), and either content hashes or full text for audit.

Reliability: retries with backoff, idempotent by `match_id`.

Security: API key/JWT, HTTPS only.
