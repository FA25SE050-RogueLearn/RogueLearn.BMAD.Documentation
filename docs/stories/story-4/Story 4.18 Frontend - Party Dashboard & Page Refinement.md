### **Story 4.18: Frontend - Party Dashboard & Page Refinement**
- **Status:** Done
- **Ownership:** Frontend Team (TBD)
- **Target Deadline:** TBD

**As a** party member,
**I want** a cleaner dashboard, a way to discover public parties, and a unified party page layout,
**so that** I can manage and join parties more efficiently.

#### **Context & References**
- Builds on: Story 4.4 Implement Basic Party Management UI and Story 4.9 Frontend Implementation Stories — Party Management.
- Routes: `/party` and `/party/:partyId`.
- APIs: Parties endpoints in docs/fullstack-architecture/service-apis/user-service-api.md.

#### **Scope**
- Dashboard refactor: remove "Party Activities" card; emphasize key stats, invites, and quick actions.
- Discover parties: a discover section using `GET /api/parties` to list all parties; private parties appear without a join button and show "Invitation Only".
- PartyId layout refactor: combine components into a single large page with anchored sections.
- Information modal: explain party features and role capabilities.

---

#### **Acceptance Criteria**
1. **Dashboard adjustments**
   - "Party Activities" card removed.
   - Show "My Parties" list, pending invites, and quick actions (Create Party, Invite Member).
   - Loading skeletons and empty states are present and accessible.

2. **Discover parties**
   - A Discover tab/section lists parties from `GET /api/parties` with filter bar (text, tags, capacity).
   - Cards show emblem, name, description, member count, visibility; private parties do not render a join button and show "Invitation Only".
   - Server-backed pagination and sorting are supported via query params on `/api/parties`.

3. **Unified partyId layout**
   - A single-page layout with a persistent header and anchored sections (Dashboard, Stash, Scheduler, Live Meeting).
   - Scrollspy or sticky nav for quick jumps; sections lazy-load content.
   - Accessibility: keyboard navigation across sections; focus management.

4. **Information modal**
   - An "Info" action in header opens a modal explaining features and role-based capabilities.
   - Accessible (focus trap, labeled controls, ESC to close).

5. **Error handling & performance**
   - Clear toasts and inline messages; predictable retry handles network issues.
   - Render performance meets expectations with memoization and virtualization where applicable.

---

#### **Dev Notes**
- Reuse existing Party components; compose into single-page layout with anchor nav.
- Introduce a discover data source in `partiesApi` (public parties listing) if missing.
- Ensure deep links (e.g., `#stash`) work and preserve state; consider URL query sync for filters.

#### **API Alignment**
- Parties
  - Listing endpoint: `GET /api/parties` returns public and private parties; UI gates join for private ones.
  - Join endpoint respects role and capacity; responses drive UI enable/disable.

#### **Tasks / Subtasks**
- [x] Refactor dashboard and remove Activities card (AC 1)
- [x] Implement Discover public parties section (AC 2)
- [x] Refactor `/party/:partyId` into unified anchored layout (AC 3)
- [x] Add party info modal with role guidance (AC 4)
- [x] Error handling and performance pass (AC 5)

#### **Testing**
- Unit: dashboard components, discover filters/search, anchor nav.
- Integration: join flow from Discover; deep link navigation.
- E2E: navigate dashboard → discover → join party → view unified page.

#### **Definition of Done**
- Dashboard simplified; discover section functional; unified party page in place.
- Role guidance available; performance and accessibility validated.