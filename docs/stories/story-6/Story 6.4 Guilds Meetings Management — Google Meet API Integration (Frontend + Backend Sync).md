# Story 6.4 — Guilds Meetings Management with Google Meet API (Frontend integration + Backend data sync)

Summary
- As a Guild Member, I want to schedule, view, and review guild meetings, so that our community can collaborate and keep records of participants and meeting artifacts (recordings, transcripts).
- As a System, I want to ingest meeting metadata, participants, and artifacts from Google Meet, so that RogueLearn can surface attendance, notes, and follow-ups across guild dashboards.

Goals
- Implement guild-level meetings UI that calls Google Meet REST API from the frontend (authenticated user).
- Use existing backend endpoints to persist meetings, participants, and artifacts, and associate meetings to a `guildId`.
- Support listing past conferences, viewing participants, and linking to artifacts (recordings/transcripts) when available.

Non-Goals
- Real-time in-call features beyond what Meet provides.
- Building our own video stack; we rely on Google Meet.
- Storing raw recordings/transcript files in RogueLearn; we store metadata/links and optionally summaries.

Scope & Assumptions
- Frontend is a Next.js SPA with Google Identity Services (GIS) for OAuth 2.0.
- Users sign in with their Google Workspace account; scopes are requested only as needed.
- Organization uses Google Workspace; Meet API enabled in the Cloud project.
- Domain policies may restrict artifacts access; we respect scope limitations.

User Flows
1) Create guild meeting space
- Given I’m authenticated and inside a guild context, when I click “New Meeting”, then the app calls `spaces.create` (Meet API) and returns a meeting URI.
- I can set basic space settings (access/moderation) and store the join URL on the meeting record tied to the guild.

2) List my guild’s past conferences
- Given I open “Guild Meetings”, then I see a paginated list of meeting records associated with the guild.
- Selecting a conference shows participants, participant sessions, and available artifacts (recordings/transcripts).

3) Sync to RogueLearn backend (guild)
- Given I view a conference, then the frontend posts meeting metadata, participants, and artifacts to our backend to persist in our DB.
- Attendance summaries and artifact links are available in the Guild dashboards.

Acceptance Criteria
Frontend
- OAuth sign-in implemented via GIS to obtain access tokens for Meet scopes.
- Create meeting space via Meet API `v2.spaces.create` with configurable `SpaceConfig` (accessType, entryPointAccess, moderation restrictions). The returned `meetingUri` is displayed and copyable.
- List past conferences via `GET https://meet.googleapis.com/v2/conferenceRecords` with pagination and filter options.
- For a selected conference:
  - List participants: `GET /v2/{parent=conferenceRecords/*}/participants`.
  - List participant sessions: `GET /v2/{parent=conferenceRecords/*/participants/*}/participantSessions`.
  - List artifacts:
    - Recordings: `GET /v2/{parent=conferenceRecords/*}/recordings`; display Drive `exportUri`.
    - Transcripts: `GET /v2/{parent=conferenceRecords/*}/transcripts`; list transcript entries via `/transcripts.entries`.
- Error handling: token expiration, insufficient scope, permission denied; UI shows actionable messages.

Backend
- Use secure endpoints to persist meeting data (JWT auth, rate limiting):
  - `POST /api/meetings` — upsert meeting metadata (include guild association in payload).
  - `POST /api/meetings/{meetingId}/participants` — upsert participants.
  - `POST /api/meetings/{meetingId}/artifacts` — upsert artifacts (recordings/transcripts metadata, exportUri).
  - Optional: `POST /api/meetings/{meetingId}/summary` — create or update meeting summary.
- Persist minimal PII; store Google user IDs when provided; avoid emails unless necessary.
- Idempotency (server-side) for repeated syncs by conferenceId.

API Contracts (Frontend DTOs)
- MeetingDto (guild-aware): supports `guildId` along with `partyId` for association.
- MeetingParticipantDto: maps signed-in/anonymous users to platform users when possible.
- ArtifactInputDto: describes transcript and recording metadata (exportUri, docsDocumentId, etc.).

Data Model Notes
- Meetings persist as platform records (meeting metadata) with linkage to guilds via `guildId` in the meeting DTO.
- Conference/participants/artifacts are synced as per Story 6.1’s proposed meet_* tables; guild association flows through the meeting linkage.

OAuth & Scopes
- Required scopes vary by operation and ownership:
  - Create/manage spaces created by your app: `https://www.googleapis.com/auth/meetings.space.created`
  - Read metadata about any space the user has access to: `https://www.googleapis.com/auth/meetings.space.readonly`
  - Access artifacts (recordings/transcripts): `https://www.googleapis.com/auth/meeting`
- Consent screen and OAuth client configured; see Google’s quickstart (Node.js).

UI Routes
- `/community/guilds/:guildId/meetings` — Guild meetings list and details.

Dev Notes (Frontend integration)
1) Acquire token via GIS:
```ts
const token = await google.accounts.oauth2
  .initTokenClient({ client_id, scope: '…', callback })
  .requestAccessToken();
```

2) Create space and upsert meeting:
```ts
const created = await googleMeetApi.createSpace(token, { config: {} });
const dto = { organizerId, guildId, title, scheduledStartTime, scheduledEndTime, meetingLink: created.meetingUri };
await meetingsApi.upsertMeeting(dto);
```

3) List conferences and artifacts:
```ts
await googleMeetApi.listConferenceRecords(token, { pageSize: 25 });
await googleMeetApi.listParticipants(token, conferenceId);
await googleMeetApi.listTranscripts(token, conferenceId);
```

4) Sync to backend (guild):
```ts
await meetingsApi.upsertParticipants(meetingId, participants);
await meetingsApi.processArtifactsAndSummarize(meetingId, artifacts);
```

5) Guild meetings listing:
```ts
await meetingsApi.getGuildMeetings(guildId);
```

Definition of Done
- Frontend:
  - OAuth sign-in implemented; tokens acquired for required scopes.
  - Users can create guild meeting spaces and copy meeting URIs.
  - Users can list past guild conferences, view participants and artifacts.
  - Clear error messages for permission/scope issues.
- Backend:
  - Endpoints persist data for meetings, participants, artifacts; meetings can be associated to a guild.
  - Idempotent upsert behavior; JWT auth enforced; DTO validation.
- Documentation:
  - Updated with environment variables and scopes.
  - Postman/Insomnia examples for backend endpoints.

References
- Story 6.1 (technology overview and flows): `docs/stories/story-6/Story 6.1 Meetings Management — Google Meet API Integration (Frontend + Backend Sync).md`
- Google Meet REST API overview and resources: https://developers.google.com/workspace/meet/api/guides/overview
- Participants: https://developers.google.com/workspace/meet/api/guides/participants
- Conferences: https://developers.google.com/workspace/meet/api/guides/conferences
- Artifacts: https://developers.google.com/workspace/meet/api/guides/artifacts