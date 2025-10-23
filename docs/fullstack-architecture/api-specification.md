# **API Specification**

This document presents RogueLearn’s feature-level API overview and points to the service-specific OpenAPI specifications. The monolithic API has been sharded into dedicated service APIs to better reflect clear boundaries, scalability, and ownership.

## **How to Use This Document**
- Start here to understand feature areas and which service owns them.
- Follow the links to each service API spec for detailed schemas, request/response models, and endpoints.
- All service APIs follow common conventions outlined below.

## **Common Conventions**
- API standard: OpenAPI `3.0.0` across all service specs.
- Authentication: All endpoints assume `BearerAuth` with a JWT from Supabase unless explicitly noted otherwise.
- Error model: `Error { code, message }` returned on failure.
- IDs: All resource identifiers (e.g., `userId`, `questId`, `partyId`) are UUIDs.
- Versioning: External base URL versioning (e.g., `https://api.roguelearn.com/v1`); internal service evolution uses additive changes.
- Pagination: Cursor or limit/offset where applicable; see each service spec for details.

## **Role-Based Access Control (RBAC)**
- Admin scope: Endpoints prefixed with `/admin` require the `Game Master` role and will be denied for non-admin users.
- Standard users: Access is limited to their own resources unless otherwise delegated by party/guild roles.
- Service-local roles: Each service defines local roles (e.g., Party Leader, Guild Master, Meeting Organizer) which gate specific actions.

## **Feature → Service Mapping**

### Profiles & Academic Management — User Service
Spec: `./service-apis/user-service-api.md`
- User profile: retrieve current profile.
- Academic enrollments: list/create enrollments, manage status.
- Syllabus upload: upload and process syllabus documents against an enrollment.

Representative APIs:
- `GET /profiles/me`
- `GET /enrollments`, `POST /enrollments`
- `POST /enrollments/{enrollmentId}/upload-syllabus`

### Social & Collaboration — Social Service
Spec: `./service-apis/social-service-api.md`
- Parties: create, list, invite, manage membership.
- Guilds: create, list, invite, achievements, membership.
- Messaging: party/guild messages and reactions.

Representative APIs:
- `GET /parties`, `POST /parties`
- `GET /guilds`, `POST /guilds`
- `GET /messages`, `POST /messages`

### Quests & Curriculum Packs — Quests Service
Spec: `./service-apis/quests-service-api.md`
- Quests: list and create quests and learning paths.
- Curriculum packs: manage AI-vetted content packs.
- Game sessions: start, track, and complete quest sessions.

Representative APIs:
- `GET /quests`, `POST /quests`
- `GET /learning-paths`, `POST /learning-paths`
- `GET /curriculum-packs`, `POST /curriculum-packs`

### Meetings & AI Summaries — Meeting Service
Spec: `./service-apis/meeting-service-api.md`
- Meetings: list, create, and retrieve meetings.
- Participants: manage and view meeting participants.
- Transcripts & summaries: AI-powered processing and retrieval.

Representative APIs:
- `GET /meetings`, `POST /meetings`
- `GET /meetings/{meetingId}`
- `GET /meetings/{meetingId}/transcripts`, `GET /meetings/{meetingId}/summaries`

### Code Battles & Leaderboards — Event Service
Spec: `./service-apis/code-battle-service-api.md`
- Problems & test cases: list problems, fetch test cases.
- Rooms & events: create rooms, join/leave, track room players.
- Submissions & leaderboards: submit solutions, view rankings.
- Duels: challenge users to 1v1 battles.

Representative APIs:
- `GET /languages`, `GET /problems`
- `GET /rooms`, `POST /rooms`
- `POST /submissions`, `GET /leaderboards`
- `POST /duels/challenge`

## **Cross-Service Considerations**
- Identity: `auth_user_id` is the canonical link across services for authenticated users.
- Authorization: Resource ownership and roles enforced per service (e.g., party leader, guild officer, meeting organizer).
- Data flows: Features may coordinate across services (e.g., social events linking to code battle rooms); keep interactions via well-defined APIs.

## **Next Steps**
- For detailed request/response formats, refer to each service spec under `./service-apis/`.
- Integrate these specs into client/server code generation pipelines as needed.