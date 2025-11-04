# Frontend Implementation Stories — Guild Management (Learning Community — Flow 4)

This document focuses on Guild-related UI for Flow 4. It provides actionable user stories with acceptance criteria, UI scope, data/state expectations, and error handling.

## Scope and Assumptions
- Platform: Web SPA using the design tokens and patterns from docs/front-end-spec.
- Routes and components align with the visual and screen specifications.
- API paths are placeholders and must be replaced with actual endpoints under docs/fullstack-architecture/service-apis/.
- Accessibility (WCAG AA) and responsiveness are required.

---

## G-FE-001 — Guild Directory and Search
As a Student, I want to browse and search guilds so I can find communities that match my goals and language focus.

Acceptance Criteria
- Given I open “Guilds” via top navigation, when the page loads, then I see a paginated list/grid of guilds with name, description, member count, tags, and join status.
- Given I type in the search box, when I submit or pause, then the list filters by name/description/tags.
- Given I apply filters (language, capacity, eligibility), when I change filter chips, then results update with server-backed queries.
- Given a guild is at capacity, then I see a “Full” badge and a “Join Waitlist” action (if enabled).
- Given my profile does not meet eligibility rules, then the card shows “Not eligible” with a tooltip explaining why.

UI Scope
- Route: /guilds
- Components: GuildList, GuildCard, FilterBar, SearchInput, Pagination, Loading/Skeleton, EmptyState

State & Data
- Client state: query { text, tags, language, capacity }, pageable { page, size }, sort { name|members|created }
- Data shape: Guild { id, name, description, tags[], language, capacity, memberCount, eligibilityStatus, canApply }

APIs
- GET /guilds?query=&tags=&language=&capacity=&page=&size=&sort=

Error Handling
- Network error: inline retry + toast.
- Empty results: “No guilds found” guidance.

Analytics
- Track search term, filters used, card clicks.

---

## G-FE-002 — Guild Detail and Join Request
As a Student, I want to open a guild page and submit a join request so officers can review me.

Acceptance Criteria
- Given I click a guild card, then I see the detail page with description, rules, tags, officers, and member stats.
- Given I meet eligibility, then I see a “Request to Join” button that opens a modal with motivation textarea and optional screening questions.
- Given I submit a valid request, then I see a success state and my request becomes “Pending Review”.
- Given I do not meet eligibility, then the join button is disabled with a clear reason.

UI Scope
- Route: /guilds/:guildId
- Components: GuildHeader, RulesPanel, OfficersList, JoinRequestModal, SuccessState

State & Data
- Data shape: GuildDetail { id, name, description, rules[], officers[], tags[], eligibilityStatus }
- Form state: { motivation: string, answers: Record<string,string> }

APIs
- GET /guilds/:id
- POST /guilds/:id/join-requests { motivation, answers }

Error Handling
- Validation: motivation required, screening answers required when presented.
- Server errors: show reasons (already applied, capacity reached) and guidance.

Analytics
- Track request opens, submissions, validation failures.

---

## G-FE-003 — Guild Membership Home
As a Guild Member, I want a guild home view so I can see announcements, events, and parties in my guild.

Acceptance Criteria
- Given I am a member, when I open my guild, then I see announcements, upcoming events, and parties with quick actions.
- Given I have role Leader/Officer, then I see management shortcuts (create event, moderate members, create party).

UI Scope
- Route: /guilds/:guildId/home
- Components: AnnouncementFeed, EventsList, PartiesList, RoleBadges, QuickActions

APIs
- GET /guilds/:id/home

Error Handling
- Permission gating: hide/disable features not allowed for my role.

Analytics
- Track engagement (clicks on announcements, events, parties).

---

## G-FE-004 — Notifications (Guild)
As a Student, I want timely guild notifications so I don’t miss invites and decisions.

Acceptance Criteria
- Given I receive a guild invite or decision, then I see a bell icon count and an inbox listing notifications by type and unread status.
- Given I click a notification, then I’m taken to the relevant screen; unread becomes read.

UI Scope
- Global Components: NotificationBell, NotificationInbox

APIs
- GET /notifications?context=guild
- POST /notifications/:id/read

Error Handling
- Inbox fetch errors: retry and show partial offline cache if available.

Analytics
- Track notification open rate and click-through.

---

## G-FE-005 — Responsive Design and Accessibility (Guild)
As a Student, I want guild screens to work on any device and be accessible.

Acceptance Criteria
- Screens adapt to mobile/tablet/desktop breakpoints; critical actions remain reachable.
- Keyboard navigation covers interactive controls; focus states visible; ARIA roles set.
- Color contrast and font sizes meet AA standards.

References
- docs/front-end-spec/screen-specifications.md
- docs/front-end-spec/visual-design-system.md

---

## G-FE-006 — Error and Edge Case Handling (Guild)
As a Student, I want consistent handling of guild-specific issues.

Acceptance Criteria
- Capacity reached: request disabled or waitlist shown.
- Eligibility failed: disabled join button with clear explanation.
- Permission denied: gated UI with explanation and next steps.
- Offline mode (read-only): cached directory and detail (if available), with sync banner when back online.

UI Scope
- Global error boundary components, toasts, banners.

APIs
- N/A

Analytics
- Track error occurrences and retry success.