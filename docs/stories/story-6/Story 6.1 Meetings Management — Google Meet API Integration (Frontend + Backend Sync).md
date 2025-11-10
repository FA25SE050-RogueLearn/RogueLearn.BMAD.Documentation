# Story 6.1 — Meetings Management with Google Meet API (Frontend integration + Backend data sync)

Summary
- As a Student or Party Member, I want to schedule, view, and review my meetings, so that I can collaborate and keep records of participants and meeting artifacts (recordings, transcripts).
- As a System, I want to ingest meeting metadata, participants, and artifacts from Google Meet, so that RogueLearn can surface attendance, notes, and follow-ups across parties/guilds.

Goals
- Implement meetings management UI that calls Google Meet REST API from the frontend (authenticated user).
- Create backend endpoints to persist meeting spaces, conferences, participants, and artifacts into our platform database.
- Support listing past conferences, viewing participants, and linking to artifacts (recordings/transcripts) when available.

Non-Goals
- Real-time in-call features (chat, reactions, live moderation) beyond what Meet provides.
- Building our own video stack; we rely on Google Meet.
- Storing raw recordings/transcript files in RogueLearn; we store metadata/links and optionally summaries.

Scope & Assumptions
- Frontend is a Next.js SPA with Google Identity Services for OAuth 2.0.
- Users sign in with their Google Workspace account; scopes are requested only as needed.
- Organization uses Google Workspace; Meet API is enabled in the Cloud project.
- Domain policies may restrict artifacts access; we respect scope limitations.

User Flows
1) Create meeting space
- Given I’m authenticated, when I click “New Meeting”, then the app calls Meet API spaces.create and returns a meeting URI.
- I can set basic space settings (access, moderation) and auto-artifact preferences if allowed.

2) List my past conferences
- Given I open “Meetings”, then I see a paginated list of conference records associated with my account.
- Selecting a conference shows participants, participant sessions, and available artifacts (recordings/transcripts).

3) Sync to RogueLearn backend
- Given I view a conference, then the frontend posts meeting metadata, participants, and artifacts to our backend to persist in our DB.
- Attendance summaries and artifact links are available in the Party/Guild dashboards.

Acceptance Criteria
Frontend
- Google OAuth flow implemented via Google Identity Services to obtain access tokens for Meet scopes.
- Create meeting space via Meet API v2.spaces.create with configurable SpaceConfig (accessType, entryPointAccess, moderation restrictions). The returned meetingUri is displayed and copyable.
- List past conferences via GET https://meet.googleapis.com/v2/conferenceRecords with pagination and filter options.
- For a selected conference:
  - List participants: GET /v2/{parent=conferenceRecords/*}/participants, with filter and paging.
  - List participant sessions: GET /v2/{parent=conferenceRecords/*/participants/*}/participantSessions.
  - List artifacts:
    - Recordings: GET /v2/{parent=conferenceRecords/*}/recordings; display Drive exportUri.
    - Transcripts: GET /v2/{parent=conferenceRecords/*}/transcripts; list transcript entries via /transcripts.entries.
- Error handling: token expiration, insufficient scope, permission denied; UI shows actionable messages.

Backend
- Provide secure endpoints to persist meeting data (JWT auth, rate limiting):
  - POST /api/meet/spaces: body { spaceId, meetingUri, config, createdByUserId } → upsert meet_spaces.
  - POST /api/meet/conferences: body { conferenceId, spaceId, startedAt, endedAt } → upsert meet_conferences.
  - POST /api/meet/conferences/{conferenceId}/participants: array of ParticipantDto → upsert meet_participants.
  - POST /api/meet/conferences/{conferenceId}/artifacts: array of ArtifactDto → upsert meet_artifacts (recording/transcript metadata, exportUri).
  - Optional: POST /api/meet/conferences/{conferenceId}/summary: body { notes, actionItems } → create meeting_summaries.
- Persist minimal PII; store Google user IDs when provided; avoid emails unless necessary.
- Idempotency keys supported per conferenceId when syncing.

API Contracts (Backend DTOs)
- MeetSpaceDto: { id, meetingUri, config: { accessType, entryPointAccess, moderationRestrictions, autoArtifactConfig? }, createdByUserId, createdAt }
- ConferenceDto: { id, spaceId, startTime, endTime }
- ParticipantDto: { id, conferenceId, displayName, userId?, type: [signedin|anonymous|phone], earliestStartTime?, latestEndTime? }
- ParticipantSessionDto: { id, participantId, deviceType, startTime, endTime }
- ArtifactDto: { id, conferenceId, type: [recording|transcript], state, exportUri, driveFileId?, docsDocumentId? }

Data Model Additions (proposed)
- meet_spaces(id PK, meeting_uri, config_json, created_by_user_id, created_at)
- meet_conferences(id PK, space_id FK, start_time, end_time)
- meet_participants(id PK, conference_id FK, display_name, user_id, type, earliest_start_time, latest_end_time)
- meet_participant_sessions(id PK, participant_id FK, device_type, start_time, end_time)
- meet_artifacts(id PK, conference_id FK, type, state, export_uri, drive_file_id, docs_document_id)
- meeting_summaries(id PK, conference_id FK, content, created_at)

OAuth & Scopes
- Required scopes vary by operation and ownership:
  - Create/manage spaces created by your app: https://www.googleapis.com/auth/meetings.space.created
  - Read metadata about any space the user has access to: https://www.googleapis.com/auth/meetings.space.readonly
  - Access artifacts (recordings/transcripts): https://www.googleapis.com/auth/meeting
- Consent screen and OAuth client must be configured. For setup steps (enable API, configure OAuth consent, create OAuth client, install libraries), see Google’s quickstart for Meet API in Node.js (Node.js quickstart — Google Meet: https://developers.google.com/workspace/meet/api/guides/quickstart/nodejs).
- Note: When using service accounts server-side, domain-wide delegation and impersonation may be required for certain admin flows; frontend uses user credentials.

Security & Compliance
- HTTPS only; store minimal PII.
- Respect data retention: transcript entries may be available for 30 days; recordings/transcripts retained in Drive per policy.
- Handle permission errors: list() may return conferences only where the user is the organizer unless scopes permit broader access.

Error Handling
- Token expired → prompt re-auth.
- Permission denied → explain scope/ownership limits (e.g., spaces created by another app may require read-only scope or organizer access).
- Artifact not ready → show processing state and retry affordance.

Dev Notes
- Frontend calling patterns (example, pseudo-code):

  1) Acquire token via GIS:
  - const token = await google.accounts.oauth2.initTokenClient({ client_id, scope: '…', callback }).requestAccessToken();

  2) List conferences:
  - fetch('https://meet.googleapis.com/v2/conferenceRecords?pageSize=25', { headers: { Authorization: `Bearer ${token}` } })

  3) List participants:
  - fetch(`https://meet.googleapis.com/v2/conferenceRecords/${confId}/participants`, { headers: { Authorization: `Bearer ${token}` } })

  4) List recordings:
  - fetch(`https://meet.googleapis.com/v2/conferenceRecords/${confId}/recordings`, { headers: { Authorization: `Bearer ${token}` } })

  5) Sync to backend:
  - await axios.post('/api/meet/conferences', conferenceDto)
  - await axios.post(`/api/meet/conferences/${confId}/participants`, participants)
  - await axios.post(`/api/meet/conferences/${confId}/artifacts`, artifacts)

- UI routes: /meetings, /meetings/:conferenceId
- Respect API limits; paginate and debounce queries.

Definition of Done
- Frontend:
  - OAuth sign-in implemented; tokens acquired for required scopes.
  - Users can create meeting spaces and copy meeting URIs.
  - Users can list past conferences, view participants and artifacts.
  - Clear error messages for permission/scope issues.
- Backend:
  - Endpoints exist and persist data for spaces, conferences, participants, artifacts.
  - Idempotent upsert behavior; JWT auth enforced; tests for DTO validation.
- Documentation:
  - README or dev docs updated with environment variables and scopes.
  - Postman/Insomnia examples for backend endpoints.

References
- Google Meet REST API overview and resources (participants, participant sessions, recordings, transcripts, conference records, spaces): https://developers.google.com/workspace/meet/api/guides/overview and https://developers.google.com/meet/api/reference/rest/v2
- Work with participants: https://developers.google.com/workspace/meet/api/guides/participants
- Work with conferences: https://developers.google.com/workspace/meet/api/guides/conferences
- Work with artifacts: https://developers.google.com/workspace/meet/api/guides/artifacts
- Configure meeting spaces and settings (including scopes for settings/artifacts): https://developers.google.com/workspace/meet/api/guides/meeting-spaces-configuration
- Quickstart (setup, enabling API, OAuth consent, client creation): https://developers.google.com/workspace/meet/api/guides/quickstart/nodejs