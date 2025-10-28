# **API Specification**

This document presents RogueLearn's feature-level API overview and points to the service-specific OpenAPI specifications. The architecture has been consolidated to provide a unified API experience while maintaining clear feature boundaries.

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

### Core Application Features — User Service (Consolidated)
Spec: `./service-apis/user-service-api.md`

The User Service provides a unified API for all core application functionality:

#### User Profile & Academic Management
- User profile: retrieve and update current profile.
- Academic enrollments: list/create enrollments, manage status.
- Syllabus upload: upload and process syllabus documents against an enrollment.

Representative APIs:
- `GET /profiles/me`, `PATCH /profiles/me`
- `GET /enrollments`, `POST /enrollments`
- `POST /enrollments/{enrollmentId}/upload-syllabus`

#### Social & Collaboration Features
- Parties: create, list, invite, manage membership.
- Guilds: create, list, invite, achievements, membership.
- Messaging: party/guild messages and reactions.
- Social events: organize and participate in community events.

Representative APIs:
- `GET /parties`, `POST /parties`
- `GET /guilds`, `POST /guilds`
- `GET /messages`, `POST /messages`
- `GET /social-events`, `POST /social-events`

#### Quest & Skill Management
- Quests: list and create quests and learning paths.
- Curriculum packs: manage AI-vetted content packs.
- Game sessions: start, track, and complete quest sessions.
- Skills: manage skill trees and user skill progression.

Representative APIs:
- `GET /quests`, `POST /quests`
- `GET /learning-paths`, `POST /learning-paths`
- `GET /curriculum-packs`, `POST /curriculum-packs`
- `GET /skills`, `GET /skill-trees`

#### Meeting & Collaboration Management
- Meetings: list, create, and retrieve meetings.
- Participants: manage and view meeting participants.
- Transcripts & summaries: AI-powered processing and retrieval.

Representative APIs:
- `GET /meetings`, `POST /meetings`
- `GET /meetings/{meetingId}`
- `GET /meetings/{meetingId}/transcripts`, `GET /meetings/{meetingId}/summaries`

#### Notes & Content Management
- Notes: create, update, and manage user-generated content.
- Tags: organize content with tagging system.
- Content linking: associate notes with quests and skills.

Representative APIs:
- `GET /notes`, `POST /notes`
- `GET /tags`, `POST /tags`
- `POST /notes/{noteId}/link-quests`

### Code Battles & Competitions — Event Service (Specialized)
Spec: `./service-apis/event-service-api.md`

The Event Service maintains specialized functionality for competitive programming:

- Problems & test cases: list problems, fetch test cases.
- Rooms & events: create rooms, join/leave, track room players.
- Submissions & leaderboards: submit solutions, view rankings.
- Duels: challenge users to 1v1 battles.

Representative APIs:
- `GET /languages`, `GET /problems`
- `GET /rooms`, `POST /rooms`
- `POST /submissions`, `GET /leaderboards`
- `POST /duels/challenge`

## **Service Communication Patterns**

### Internal User Service Communication
- **Direct Database Access**: All User Service features can access shared database directly
- **Transactional Operations**: Cross-domain operations (e.g., quest completion affecting social stats) can use database transactions
- **Real-time Updates**: SignalR hubs provide real-time communication across all User Service features

### User Service ↔ Event Service Communication
- **API-based Integration**: Event Service communicates with User Service via REST APIs
- **Soft References**: Event Service maintains `auth_user_id` references to User Service entities
- **Event-driven Updates**: Azure Service Bus handles asynchronous communication for code evaluation results

## **Cross-Service Considerations**
- Identity: `auth_user_id` is the canonical link between services for authenticated users.
- Authorization: Resource ownership and roles enforced within User Service; Event Service validates permissions via User Service APIs.
- Data flows: Event Service coordinates with User Service for user data, guild information, and achievement triggers.

## **Next Steps**
- For detailed request/response formats, refer to each service spec under `./service-apis/`.
- Integrate these specs into client/server code generation pipelines as needed.