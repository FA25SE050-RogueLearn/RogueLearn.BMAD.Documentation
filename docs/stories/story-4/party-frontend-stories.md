# Frontend Implementation Stories — Party Management (Learning Community — Flow 4)

This document focuses on Study Party-related UI for Flow 4. It provides actionable user stories with acceptance criteria, UI scope, data/state expectations, and error handling.

## Scope and Assumptions
- Platform: Web SPA using the design tokens and patterns from docs/front-end-spec.
- Routes and components align with the visual and screen specifications.
- API paths are placeholders and must be replaced with actual endpoints under docs/fullstack-architecture/service-apis/.
- Accessibility (WCAG AA) and responsiveness are required.

---

## P-FE-001 — Create Study Party
As a Student, I want to create a study party so I can collaborate with peers around specific topics.

Acceptance Criteria
- Given I click “Create Party” from guild or global actions, then a modal/form appears with name, description, visibility (guild-only or private), topics/tags, and rules.
- Given I provide a valid name and description, when I submit, then the party space is created and I’m set as Party Leader.
- Given visibility is guild-only, then the party appears under the guild’s parties list; otherwise in my profile’s parties.

UI Scope
- Route: /parties/new
- Components: PartyCreateForm, VisibilitySelector, TagSelector, RulesEditor

State & Data
- Form state: { name, description, visibility: "GuildOnly"|"Private", tags[], rules[] }
- Party: { id, leaderId, guildId?, visibility, createdAt }

APIs
- POST /parties { name, description, visibility, tags, rules, guildId? }

Error Handling
- Validation: name required; description min length.
- Server: duplicate name within guild -> suggest alternative.

Analytics
- Track party creation success/failure.

---

## P-FE-002 — Invite Members to Party
As a Party Leader, I want to invite members so my party can onboard collaborators.

Acceptance Criteria
- Given I open the party members tab, then I can search students by name or username and send invites.
- Given I invite a member, then an in-app notification and email (if enabled) is sent; the invite appears with status Pending.
- Given an invitee accepts, then status becomes Accepted and they are added to the member list.

UI Scope
- Route: /parties/:partyId/members
- Components: MemberList, InvitePanel, SearchAndSelect, InviteStatusBadge

State & Data
- Invite: { id, partyId, inviteeUserId, status: "Pending"|"Accepted"|"Declined"|"Expired" }

APIs
- POST /parties/:id/invitations { inviteeUserId }
- POST /parties/invitations/:inviteId/respond { action: "accept"|"decline" }

Error Handling
- Capacity full: show message and disable invites.
- Invalid invite link: show “Invite expired or invalid”.

Analytics
- Track invites sent, accept/decline rates.

---

## P-FE-003 — Materials Library (Files and Links)
As a Party Member, I want to share and browse materials so we can learn collaboratively.

Acceptance Criteria
- Given I open the Materials tab, then I can upload files, add links, and tag content by topic.
- Given I add or update material, then it appears in the list with versioning and tags.
- Given I filter by tag or type, then the list updates accordingly.

UI Scope
- Route: /parties/:partyId/materials
- Components: MaterialList, UploadButton, LinkAddForm, TagFilter, VersionBadge

State & Data
- Material: { id, type: "file"|"link", title, url?, fileMeta?, tags[], version, createdBy, createdAt }

APIs
- GET /parties/:id/materials
- POST /parties/:id/materials { type, title, url?, file }
- PUT /parties/materials/:materialId { title?, tags? }

Error Handling
- Upload failures: show retry & max size guidance.
- Malicious link checks: show warning if flagged.

Analytics
- Track uploads, downloads, link clicks.

---

## P-FE-004 — Schedule and Meeting Links
As a Party Leader, I want to schedule recurring meetings and share join links so members can attend regularly.

Acceptance Criteria
- Given I open the Schedule tab, then I can create one-off or weekly recurring events with title, date/time, duration, and meeting link.
- Given I create an event, then all members receive a notification; the calendar displays upcoming sessions.
- Given I edit or cancel an event, then updates propagate to members with notifications.

UI Scope
- Route: /parties/:partyId/schedule
- Components: CalendarView, EventFormModal, RecurrenceSelector, EventCard

State & Data
- Event: { id, title, startAt, endAt, recurrence?: "none"|"weekly", meetingLink, notes? }

APIs
- GET /parties/:id/events
- POST /parties/:id/events { title, startAt, endAt, recurrence, meetingLink }
- PUT /parties/events/:eventId
- DELETE /parties/events/:eventId

Error Handling
- Timezone conflicts: show user-local time and party-default timezone.
- Invalid meeting link: validation and inline warning.

Analytics
- Track event creation, edits, cancellations.

---

## P-FE-005 — Live Notes and Action Items
As a Party Member, I want live notes with action items so meetings capture decisions and follow-ups.

Acceptance Criteria
- Given a meeting is active, then members can add notes and mark action items with assignees and due dates.
- Given the meeting ends, then a summary doc is generated and accessible from the Meeting History.
- Given I update an action item, then the dashboard reflects completion status.

UI Scope
- Route: /parties/:partyId/notes
- Components: NotesEditor (collab-ready), ActionItemList, AssignSelector, SummaryView

State & Data
- Note: { id, content, authorId, createdAt }
- ActionItem: { id, title, assigneeId, dueAt, status: "Open"|"Done" }
- Summary: { id, meetingId, content }

APIs
- GET /parties/:id/notes
- POST /parties/:id/notes
- GET /parties/:id/action-items
- POST /parties/:id/action-items
- PUT /parties/action-items/:id

Error Handling
- Conflict resolution: last-write-wins with visual merge hints (basic); advanced collab via backend if supported.

Analytics
- Track note additions, action item completion.

---

## P-FE-006 — Notifications (Party)
As a Student, I want timely party notifications so I don’t miss invites, events, and summaries.

Acceptance Criteria
- Given I receive an invite or event update, then I see a bell icon count and an inbox listing notifications by type and unread status.
- Given I click a notification, then I’m taken to the relevant screen; unread becomes read.

UI Scope
- Global Components: NotificationBell, NotificationInbox

APIs
- GET /notifications?context=party
- POST /notifications/:id/read

Error Handling
- Inbox fetch errors: retry and show partial offline cache if available.

Analytics
- Track notification open rate and click-through.

---

## P-FE-007 — Party Dashboard and Engagement Widgets
As a Party Leader, I want a dashboard so I can understand engagement and outcomes.

Acceptance Criteria
- Given I open the Party Dashboard, then I see attendance rate, meeting frequency, shared materials count, topic coverage, action items completed, and study time per member.
- Given I change date ranges or filters, then widgets update consistently.

UI Scope
- Route: /parties/:partyId/dashboard
- Components: MetricsGrid, AttendanceChart, MaterialsCounter, TopicCoverageChart, ActionItemsChart, TimeSpentChart, DateRangePicker

APIs
- GET /parties/:id/metrics?range=…

Error Handling
- No data: show placeholders and guidance to schedule or share.

Analytics
- Track widget interactions and filter usage.

---

## P-FE-008 — Roles and Permissions (Leader/Member)
As a Party Leader, I want to manage roles and permissions so the party is organized and safe.

Acceptance Criteria
- Given I am Leader, then I can promote Members to Co-Leaders and revoke roles.
- Given I am Member, then restricted actions (e.g., removing others) are hidden/disabled.

UI Scope
- Route: /parties/:partyId/settings/roles
- Components: RoleTable, RoleBadge, PromoteDemoteButtons

APIs
- GET /parties/:id/roles
- POST /parties/:id/roles/update { userId, role }

Error Handling
- Permission errors: clearly surfaced with guidance.

Analytics
- Track role changes.

---

## P-FE-009 — Moderation and Rules
As a Party Leader, I want to set rules and moderate members so the environment remains constructive.

Acceptance Criteria
- Given I open Party Settings, then I can edit rules (markdown support), set visibility, and remove members with optional reason.
- Given I remove a member, then the member sees a notification with reason (if provided).

UI Scope
- Route: /parties/:partyId/settings
- Components: RulesEditor, VisibilityToggle, MemberModerationList

APIs
- PUT /parties/:id { rules, visibility }
- POST /parties/:id/members/:userId/remove { reason? }

Error Handling
- Confirmation modals for destructive actions.

Analytics
- Track rule edits and moderation actions.

---

## P-FE-010 — Attendance Tracking and Check-in
As a Party Member, I want to check in to meetings so attendance is tracked.

Acceptance Criteria
- Given a scheduled meeting with check-in token, then I can check in within the event window; my attendance is recorded.
- Given I miss the window, then the check-in is disabled with a clear message.

UI Scope
- Route: /parties/:partyId/events/:eventId
- Components: CheckInPanel, AttendanceList, TokenStatus

APIs
- POST /parties/events/:eventId/check-in { token }
- GET /parties/events/:eventId/attendance

Error Handling
- Invalid/expired token: show error and guidance.

Analytics
- Track check-in rates.

---

## P-FE-011 — Responsive Design and Accessibility (Party)
As a Student, I want party screens to work on any device and be accessible.

Acceptance Criteria
- Screens adapt to mobile/tablet/desktop breakpoints; critical actions remain reachable.
- Keyboard navigation covers interactive controls; focus states visible; ARIA roles set.
- Color contrast and font sizes meet AA standards.

References
- docs/front-end-spec/screen-specifications.md
- docs/front-end-spec/visual-design-system.md

---

## P-FE-012 — Error and Edge Case Handling (Party)
As a Student, I want consistent handling of party-specific issues.

Acceptance Criteria
- Capacity reached: invites disabled; show waitlist and notification opt-in.
- Invalid invite links: recovery options (request new invite, contact leader).
- Permission denied: gated UI with explanation and next steps.
- Offline mode: read-only caches for materials and schedule; sync banner when back online.

UI Scope
- Global error boundary components, toasts, banners.

APIs
- N/A

Analytics
- Track error occurrences and retry success.