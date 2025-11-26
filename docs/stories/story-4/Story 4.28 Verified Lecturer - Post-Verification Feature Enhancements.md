### **Story 4.28: Verified Lecturer - Post-Verification Feature Enhancements**
- **Status:** Proposed
- **Ownership:** Frontend + Backend Teams (TBD)
- **Target Deadline:** TBD

**As a** Verified Lecturer,
**I want** a dedicated educator toolkit and streamlined workflows immediately available after approval,
**so that** I can create guilds, support students, share materials, run assignments, and view anonymized analytics effectively.

#### **Context & References**
- Verification flow and role assignment: `docs/stories/story-4/Story 4.26 Backend - Lecturer Verification Requests API.md`, `docs/stories/story-4/Story 4.27 Frontend - Lecturer Verification Flow UI.md`.
- Guild UI and role gating patterns: `docs/stories/story-4/Story 4.20 Frontend - Guild UI Refinements (Posts, Dashboard, Info).md`, `docs/stories/story-4/Story 4.15 Frontend - Guild Role Management UI and Promotion Flow.md`, `docs/stories/story-4/Story 4.6 Backend - Implement Guild Management API (CQRS, Roles, Admin Endpoints).md`.
- Requirements: `docs/prd/epic-list.md#basic-guild-management-verified-lecturer-gated`, `docs/prd/epic-details.md` (Guild Creation, Guild Dashboard & Analytics), `docs/prd/user-features.md` (Verified Lecturer).
- Service APIs: `docs/fullstack-architecture/service-apis/social-service-api.md` (Guilds, Announcements, Members), `docs/fullstack-architecture/service-apis/event-service-api.md`.
- Automated project verification for assignments: `docs/prd/requirements.md` (FR56).

#### **Scope**
- Role reflection and educator workspace:
  - Upon approval, surface a `Verified Lecturer` badge on profile and guild pages; show a `Lecturer Workspace` entry in the main navigation without requiring logout.
  - Workspace home summarizes guilds you lead, upcoming office hours, active assignments, and recent announcements.
- Streamlined guild creation:
  - Provide a simplified guild creation wizard tailored for educators with prefilled templates (name, description, route focus) and immediate access on approval.
  - Verified Lecturers bypass non-educator restrictions; creation is available from the workspace and dashboard.
- Announcements:
  - Create and schedule guild announcements with pin/unpin and lock controls; posts visible to guild members with moderation actions gated by role.
- Materials & reading lists:
  - Upload and manage educator materials (links/files) organized by module/topic; attach tags aligned to route/class taxonomy. Materials remain guild-scoped (no global publishing).
- Assignments (LocalProject) with automated verification:
  - Create assignments with title, description, due date, target groups (entire guild or subset), and verification rules mapped to FR56 (structure checks, patterns, score threshold).
  - Students submit repo/zip; system verifies and records pass/fail with feedback; aggregated results appear in the educator dashboard.
- Office hours scheduling:
  - Schedule recurring or one-off office hours with timezones, capacity limits, and join links; integrate meeting links (Google Meet per Story 6.1/6.4) where available.
- Educator analytics (anonymized):
  - Display aggregated, anonymized guild progress: completion rates by route topics, struggle hotspots, assignment outcomes, and participation metrics; export CSV.
- Security & RBAC:
  - All features gated to `Verified Lecturer`; non-lecturers see upsell prompts; Admin retains governance for platform-level publishing.

---

#### **Acceptance Criteria**
1. Role reflection and workspace
   - After approval, the UI shows `Verified Lecturer` badge and a `Lecturer Workspace` navigation group.
   - Claims refresh occurs automatically (re-fetch `/api/users/me`) so visibility updates without logout.

2. Streamlined guild creation
   - A `Create Guild` CTA appears in the workspace; the wizard supports educator templates and immediate creation via API.
   - Post-creation, redirect to guild dashboard with educator tools enabled.

3. Announcements management
   - Educators can compose, schedule, pin/unpin, and lock announcements; members can view and comment subject to moderation.
   - Permissions enforced client/server; error and empty states present.

4. Materials & reading lists
   - Educators can add links/files with tags, reorder items, and attach to modules/topics; items are visible to guild members.
   - No global publish actions; Admin governance remains separate.

5. Assignments with automated verification
   - Educators can create assignments targeting guild members; students submit repo/zip.
   - System verifies per FR56 (existence, structure, patterns, score threshold); results show pass/fail and feedback.
   - Educator dashboard aggregates assignment outcomes with filters/date ranges.

6. Office hours scheduling
   - Educators can schedule office hours with recurring patterns and capacity; members can register and receive reminders.
   - Meeting links generated/attached (Google Meet integration aligned with Story 6.1/6.4).

7. Educator analytics (anonymized)
   - Dashboard shows anonymized aggregates: completion rates, struggle hotspots, assignment pass rates, participation metrics; export CSV.
   - Data privacy constraints observed; no PII exposure beyond policy.

8. Security, accessibility, performance
   - RBAC gating enforced on routes and API; keyboard navigation, ARIA; SWR caching and skeleton loaders; errors toast/inline.

---

#### **Dev Notes**
- UI: Build workspace using existing dashboard layout with Shadcn/UI and Tailwind; reuse guild components where possible.
- Data: Stale-while-revalidate for educator dashboards; memoize aggregates; batched fetch where supported.
- Privacy: Anonymization/aggregation for analytics per SRS; guild-scoped materials only.
- Verification: Integrate FR56 service to evaluate assignment submissions; store verification results with feedback.
- Security: Client-side guards plus server-side RBAC checks; hide actions for non-lecturers.

#### **API Alignment**
- Guilds: `POST /guilds`, `GET /guilds/{guildId}`, `GET /guilds/{guildId}/members` (social-service-api.md).
- Announcements: `POST /guilds/{guildId}/announcements` (social-service-api.md).
- Materials (guild-scoped): Draft endpoints, e.g., `POST /guilds/{guildId}/materials`, `GET /guilds/{guildId}/materials`.
- Assignments: Draft endpoints, e.g., `POST /guilds/{guildId}/assignments`, `POST /guilds/{guildId}/assignments/{id}/submissions`; verification integrates FR56.
- Office hours: Draft endpoints, e.g., `POST /guilds/{guildId}/office-hours`, `GET /guilds/{guildId}/office-hours`.
- Educator analytics: `GET /guilds/{guildId}/analytics` for anonymized aggregates and CSV export.

#### **Routes & Components**
- Routes
  - `/lecturer/workspace` (home), `/lecturer/workspace/guilds/create`, `/lecturer/workspace/analytics`.
  - Guild pages add educator tabs: `Announcements`, `Materials`, `Assignments`, `Office Hours`.
- Components
  - `LecturerWorkspaceHome`, `GuildAnnouncementsPanel`, `GuildMaterialsManager`, `GuildAssignmentsPanel`, `OfficeHoursScheduler`, `EducatorAnalyticsDashboard`.

#### **Tasks / Subtasks**
- [ ] Add Verified badge, workspace nav, and claims refresh (AC 1)
- [ ] Implement educator guild creation wizard (AC 2)
- [ ] Build announcements management UI and RBAC gating (AC 3)
- [ ] Build materials & reading lists manager (AC 4)
- [ ] Implement assignments with FR56 integration and results (AC 5)
- [ ] Implement office hours scheduling with reminders (AC 6)
- [ ] Implement anonymized educator analytics + CSV export (AC 7)
- [ ] Accessibility, performance, and security gating pass (AC 8)

#### **Testing**
- Unit: workspace visibility, forms validation, RBAC rendering.
- Integration: guild creation flow; announcements, materials, assignments, office hours endpoints; FR56 verification.
- E2E: approve lecturer → workspace appears → create guild → post announcements/materials → assign LocalProject → students submit → verification results aggregate → schedule office hours → view analytics.

#### **Definition of Done**
- Verified Lecturers gain immediate access to educator workspace with streamlined guild creation.
- Announcements, materials, assignments with verification, office hours, and anonymized analytics implemented and gated.
- Accessibility validated; privacy preserved; tests pass.