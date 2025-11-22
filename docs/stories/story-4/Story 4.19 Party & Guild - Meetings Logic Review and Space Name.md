### **Story 4.19: Party & Guild — Meetings Management Logic Review and Space Name**
- **Status:** Proposed
- **Ownership:** Frontend + Backend (TBD)
- **Target Deadline:** TBD

**As a** leader/member,
**I want** consistent meetings behavior across Party and Guild with a visible space name,
**so that** scheduling, listing, and joining meetings are predictable.

#### **Context & References**
- Builds on: Story 6.1 Meetings Management — Google Meet API Integration and Story 6.4 Guilds Meetings Management — Google Meet API Integration.
- Routes: `/party/:partyId/meetings`, `/community/guilds/:guildId/meetings`.
- APIs: Meetings endpoints in docs/fullstack-architecture/service-apis (guild/party associations).

#### **Scope**
- Logic audit: unify statuses, time handling, and permission gates between Party and Guild.
- `spaceName` addition: capture/display a human-readable space name for each meeting.
- Create/edit flows updated to include `spaceName`; default from Meet API if available.
- UI tables show `spaceName` alongside title/time/status.

---

#### **Acceptance Criteria**
1. **Unified lifecycle**
   - Statuses: Scheduled → Active → Ended Processing → Completed; same transitions across Party and Guild.
   - Time zones handled consistently; UTC storage, local display.
   - Permission gating yields predictable UI (organizer actions vs. attendee view).

2. **Meeting metadata fields**
   - Meeting DTO includes `meetingCode: string` and `spaceName: string`; `spaceName` defaults from Google Meet `space.config.title` when available; backfill strategy defined for existing meetings.
   - UI lists show a "Space Name" column in Party and Guild meetings tables and support sorting by `spaceName`.

3. **Auth & tokens**
   - Supabase OAuth configured with `access_type: 'offline'` and `prompt: 'consent'`.
   - Callback route captures `provider_refresh_token` and persists it to `User.refreshToken`.

4. **Create/edit forms**
   - Forms include `spaceName` input; when created via Google Meet, auto-fill from API where possible.
   - Validation and error states visible; non-blocking retries for transient errors.

5. **API endpoints (data storage)**
   - `POST /api/meetings`: Upsert meeting metadata (`meetingCode`, `spaceName`, `status`, times, links).
   - `POST /api/meetings/{meetingId}/participants`: Upsert participants captured after meeting end.
   - `POST /api/meetings/{meetingId}/artifacts`: Process artifacts (e.g., transcript files) and summarize.
   - `POST /api/meetings/{meetingId}/summary`: Create/update meeting summary content.
   - `GET /api/meetings/{meetingId}`: Retrieve meeting details.
   - `GET /api/meetings/party/{partyId}` and `GET /api/meetings/guild/{guildId}`: List meetings with `spaceName`, `meetingCode`, and `status`.

6. **Frontend states**
   - Group dashboard: `ACTIVE` shows Join/End; `ENDED_PROCESSING` shows processing message; `COMPLETED` shows summary; Sync Transcript available after 10 minutes.

---

#### **Dev Notes**
- Google Cloud: enable Google Meet API and Google Drive API; configure OAuth Consent Screen with scopes:
  - `.../auth/meetings.space.created`
  - `.../auth/meetings.space.settings`
  - `.../auth/drive.readonly`
- Supabase & Frontend Auth: configure login with `access_type: 'offline'` and `prompt: 'consent'`; create a callback route to capture `provider_refresh_token` and save it to the User table.
 - Data (.NET + Supabase Postgres): models
  - User: `RefreshToken: string`
  - Group: `Name`, `LeaderId`
  - Meeting: `MeetingCode`, `SpaceName`, `MeetingLink`, `Status`, `Transcript`, `Summary`, `StartedAt`, `EndedAt`
  - Participant: `DisplayName`, `JoinTime`, `LeaveTime`, `MeetingId`
- Lifecycle statuses enum `MeetingStatus`: `SCHEDULED`, `ACTIVE`, `ENDED_PROCESSING`, `COMPLETED`.
- Google Meet: map `space.config.title` to `spaceName` when available.
- Drive sync: query Drive by `name contains '{meetingCode}'`, export transcript; optionally send to LLM for summary.
- Error handling: clear messages for permission denial, token issues, and missing artifacts; accessible feedback (keyboard navigation, labeled fields, screen-reader announcements).
- Frontend data layer: use `src/api/meetingsApi.ts` for storage and retrieval; update types in `@/types/meetings` to include `meetingCode`, `spaceName`, and `MeetingStatus`.

##### Database Enum Scripts

```sql
CREATE TYPE meeting_status AS ENUM ('SCHEDULED','ACTIVE','ENDED_PROCESSING','COMPLETED');
```

```csharp
public enum MeetingStatus
{
  SCHEDULED,
  ACTIVE,
  ENDED_PROCESSING,
  COMPLETED
}
```

```ts
export enum MeetingStatus {
  SCHEDULED = 'SCHEDULED',
  ACTIVE = 'ACTIVE',
  ENDED_PROCESSING = 'ENDED_PROCESSING',
  COMPLETED = 'COMPLETED'
}
```

##### .NET Model Samples

```csharp
public class Meeting
{
  public Guid Id { get; set; }
  public string MeetingCode { get; set; }
  public string SpaceName { get; set; }
  public string MeetingLink { get; set; }
  public MeetingStatus Status { get; set; }
  public string Transcript { get; set; }
  public string Summary { get; set; }
  public DateTime? StartedAt { get; set; }
  public DateTime? EndedAt { get; set; }
}
```

```csharp
public class Participant
{
  public Guid Id { get; set; }
  public Guid MeetingId { get; set; }
  public string DisplayName { get; set; }
  public DateTime? JoinTime { get; set; }
  public DateTime? LeaveTime { get; set; }
}
```

#### **API Alignment**
- Meetings
  - Upsert: `POST /api/meetings` via `meetingsApi.upsertMeeting(payload)`.
  - Participants: `POST /api/meetings/{meetingId}/participants` via `meetingsApi.upsertParticipants(meetingId, participants)`.
  - Artifacts: `POST /api/meetings/{meetingId}/artifacts` via `meetingsApi.processArtifactsAndSummarize(meetingId, artifacts)`.
  - Summary: `POST /api/meetings/{meetingId}/summary` via `meetingsApi.createOrUpdateSummary(meetingId, content)`.
  - Details: `GET /api/meetings/{meetingId}` via `meetingsApi.getMeetingDetails(meetingId)`.
  - Lists: `GET /api/meetings/party/{partyId}` and `GET /api/meetings/guild/{guildId}` via `meetingsApi.getPartyMeetings(partyId)` / `meetingsApi.getGuildMeetings(guildId)`.

#### **Tasks / Subtasks**
- [ ] Configure Google Cloud + OAuth scopes; Supabase offline consent and callback
 - [x] Add .NET models and Supabase Postgres migrations (`meetingCode`, `spaceName`, `MeetingStatus`)
- [ ] Implement backend routes for upsert, participants, artifacts, summary, and details
- [ ] Update Party/Guild meetings UI lists and forms with `spaceName` and `meetingCode`
- [ ] Implement dashboard states and actions for lifecycle statuses
- [ ] Error handling and accessibility pass across flows
- [ ] Wire frontend data to `src/api/meetingsApi.ts`; update `@/types/meetings`

#### **Testing**
 - Unit: DTO mapping; .NET models; form validation.
- Integration: upsert/participants/artifacts/summary endpoints; list rendering with `spaceName` and `meetingCode`.
- E2E: create meeting → join/end → wait 10 min → sync transcript via artifacts → view summary across Party/Guild; edit `spaceName` and verify consistency.

#### **Definition of Done**
 - Supabase refresh tokens captured and stored; migrations applied via SQL/EF.
- Status lifecycle unified across Party and Guild; `spaceName` visible, sortable, and editable.
- Endpoints implemented and verified using `src/api/meetingsApi.ts`; transcripts synced; summaries present when enabled.
- Tests pass; docs updated.