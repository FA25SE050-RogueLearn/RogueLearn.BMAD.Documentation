### **Story 4.21: Frontend - Dashboard & User Profiles Enhancements**
- **Status:** Proposed
- **Ownership:** Frontend Team (TBD)
- **Target Deadline:** TBD

**As a** user,
**I want** improved fetching of other users’ profiles and an enhanced dashboard with refined user tab and profile modal,
**so that** I can explore and connect with others effectively.

#### **Context & References**
- Aligns with user directory and profile flows in docs/front-end-spec.
- APIs: User profile endpoints in docs/fullstack-architecture/service-apis/user-service-api.md.

#### **Scope**
- Better data fetching: caching, skeletons, and resilient error handling for other users’ profile data.
- View other user’s profile: dedicated view or modal, accessible from lists/search.
- Readjust user tab: reorganize information and add a profile modal for quick inspection.

---

#### **Acceptance Criteria**
1. **Fetching improvements**
   - Requests for other users’ profiles use caching and show skeleton loaders; failures show helpful guidance.
   - Rate limits and retry logic avoid hammering the API; loading states are predictable.

2. **View other user’s profile**
   - From user lists/search, a "View Profile" action opens a profile view or modal.
   - View includes avatar, basic bio, achievements/skills, parties/guilds membership summaries, and privacy-respecting fields.
   - Deep links supported via route (e.g., `/users/:userId`) or modal state.

3. **User tab readjustment**
   - The dashboard’s user tab presents refined sections; a modal allows quick inspection without navigation.
   - Keyboard navigation and accessible labels are present.

4. **Error handling & accessibility**
   - Consistent inline/toast errors; focus management; readable contrast and ARIA roles.

---

#### **Dev Notes**
- Cache: use client-side caching with stale-while-revalidate; memoize computed summaries.
- Privacy: honor visibility flags; never expose PII beyond policy.
- Modal: share a common profile component between full page and modal.

#### **API Alignment**
- Profiles: `GET /api/users/me` for the current user’s profile and aggregates used by the dashboard and user tab.

#### **Tasks / Subtasks**
- [ ] Implement caching/skeleton/error handling for profile fetches (AC 1)
- [ ] Add "View Profile" entry point and component/modal (AC 2)
- [ ] Readjust dashboard user tab and integrate modal (AC 3)
- [ ] Accessibility & error handling pass (AC 4)

#### **Testing**
- Unit: profile component rendering and states.
- Integration: list→profile view/modal; caching behaviors.
- E2E: navigate dashboard → open user tab → view other profile → back.

#### **Definition of Done**
- Profile viewing is smooth and resilient; dashboard user tab refined.
- Accessibility validated; tests pass.