### **Story 4.20: Frontend - Guild UI Refinements (Posts, Dashboard, Info)**
- **Status:** Proposed
- **Ownership:** Frontend Team (TBD)
- **Target Deadline:** TBD

**As a** guild member,
**I want** improved post management and a richer dashboard with clear guidance,
**so that** I can participate more effectively in my guild.

#### **Context & References**
- Builds on: Story 4.10 Guild Frontend Stories and Story 4.15 Frontend - Guild Role Management UI and Promotion Flow.
- Routes: `/community/guilds/:guildId`, `/community/guilds/:guildId/posts`.
- APIs: Guild posts endpoints in docs/front-end spec and service-apis.

#### **Scope**
- Post management UI refactor: streamlined compose/edit, moderation actions, filtering and sorting.
- Dashboard enhancements: summary tiles for posts, events, parties; cross-tab insights with deep links.
- Information modal: guild overview, features, and role-based capabilities.

---

#### **Acceptance Criteria**
1. **Posts UI**
   - Compose/edit forms include title, content, tags; validation errors are visible.
   - Moderation actions (pin/lock/approve) gated by role; non-author posts cannot be edited by members.
   - List supports sort (newest, most liked) and tag filters; empty and error states present.

2. **Dashboard expansions**
   - Dashboard shows recent announcements, upcoming events, and active parties with quick links.
   - Tiles link to their respective tabs/pages; skeleton loaders and accessibility in place.

3. **Information modal**
   - An "Info" action opens a modal summarizing features and roles (Member, Officer, Master) with capabilities.
   - Accessible control patterns; non-blocking.

4. **Error handling & accessibility**
   - Inline errors and toasts; keyboard navigation across cards and actions; ARIA labels.

---

#### **Dev Notes**
- Reuse `GuildPostsPage` and related components; consolidate actions and clarify permissions.
- Dashboard tiles consume existing endpoints; cache and memoize.
- Modal content should be maintainable and localized where applicable.

#### **API Alignment**
- Posts: `GET/POST/PUT/DELETE` endpoints; admin pin/lock/approve routes per Story 4.10.
- Dashboard: `/api/guilds/:guildId/dashboard` composite endpoint or individual fetches.

#### **Tasks / Subtasks**
- [ ] Refactor post management UI and moderation gating (AC 1)
- [ ] Add dashboard tiles and deep links (AC 2)
- [ ] Add guild info modal (AC 3)
- [ ] Error handling & accessibility pass (AC 4)

#### **Testing**
- Unit: post forms and moderation actions; dashboard tiles.
- Integration: tag filters and sorting; role-based visibility.
- E2E: view dashboard → open posts → moderate → verify state.

#### **Definition of Done**
- Posts UI streamlined and permission-aware; dashboard informative.
- Info modal present; accessibility validated; tests pass.