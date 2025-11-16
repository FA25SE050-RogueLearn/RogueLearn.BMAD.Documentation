### **Story 4.14: Frontend - Role-Based Management for Party Screens**
- **Status:** Done
- **Ownership:** Frontend Team
- **Target Deadline:** TBD

**As a** Party member,
**I want** the party UI to adapt to my role (Leader, CoLeader, Member),
**so that** I only see and can perform actions appropriate to my permissions.

#### **Roles**
- **Leader:** Full management authority for the party.
- **CoLeader:** Shared management authority, excluding Leader removal/demotion, leadership transfer, and disband.
- **Member:** Participation rights with limited management.

#### **Acceptance Criteria**
1. On `/party/[partyId]`, actions visible and enabled reflect the current user role (Leader, CoLeader, Member).
2. Leader capabilities: invite/revoke invites, remove members, promote/demote CoLeaders, transfer leadership, edit party details, manage stash items, manage meetings, disband party.
3. CoLeader capabilities: invite/revoke invites, remove members (excluding Leader), promote Member to CoLeader (policy-guarded), edit party details, manage stash items, manage meetings; cannot demote/remove Leader, transfer leadership, or disband.
4. Member capabilities: view party info, use stash items per policy, attend meetings, leave party; cannot invite/remove/promote/demote, edit settings, or disband.
5. UI gates applied consistently across `PartyHeader`, `InvitationManagement`, `PartyMembersList` actions, `PartyStash`, `MeetingScheduler`, `LiveMeeting`, and `PartySettings`.
6. Role changes reflect immediately in visible actions without page reload (reactive state).
7. Unauthorized actions do not render; if invoked (e.g., deep-link), the UI shows a clear permission message and blocks the action.
8. Component and integration tests verify role gating for critical actions and tabs.
9. Explore Parties: non-members do not see party views; remove non-member explore views entirely.
10. Party Dashboard: party description is displayed for all roles.

#### **UI Scope**
- Route: `/party/[partyId]` using `DashboardLayout`.
- Components (from Party frontend stories):
  - `PartyDetailPageClient` fetches party by id and hydrates role state.
  - `PartyDetailClient` renders tabs: Dashboard, Stash, Scheduler, Live Meeting.
    - Dashboard tab shows party description beneath header.
  - `PartyHeader` shows party name/emblem and role-gated primary actions (Invite, Leave, Disband).
  - `PartyMembersList` shows members with role-badges and role-gated per-member actions.
  - `InvitationManagement` shows pending invites and role-gated controls.
  - `PartySettings` shows editable fields for Leader/CoLeader only.
- Explore Parties: remove views for non-members; do not render party previews/details for parties the current user is not a member of.

#### **Role Gate Pattern**
- Introduce a `RoleGate` utility to conditionally render based on role.
  - Props: `requireAny: Role[]` and `requireAll: Role[]`.
  - Use at button-level (actions) and tab-level (management views).
- Introduce a `usePartyRole(partyId)` hook to read roles for the current user and subscribe to changes.
- Centralize role constants: `Leader`, `CoLeader`, `Member`.

#### **Data & APIs**
- Read members with roles: `GET /api/parties/{partyId}/members` (roles embedded per backend story).
- Read explicit roles: `GET /api/parties/{partyId}/members/{memberId}/roles`.
- Derive `memberId` from auth context; cache role state in page-level store for reuse across components.

#### **Error Handling & UX**
- Permission-denied: show concise toast/banner explaining required role and hide action controls.
- Loading & failure states: render skeletons and retry affordance; fall back to minimal view if role fetch fails.
- Role changed mid-session: UI updates immediately; revoked permissions hide actions without breaking layout.
- Explore Parties empty state: if no member parties exist, show empty state with guidance, without exposing non-member party views.

#### **Edge Cases**
- Multiple roles: treat `Leader`/`CoLeader` as superseding `Member`.
- No role in party: show only join/leave options.
- Leader leaving: require leadership transfer confirmation before allowing leave.
- Explore Parties: users not in any party see no party views; only empty state is shown.

#### **Telemetry**
- Track visibility of gated actions and permission-denied events to inform UX improvements.

#### **Testing**
- Unit tests for `RoleGate` logic (`requireAny`, `requireAll`).
- Integration tests on `/party/[partyId]` verifying visible actions per role and that blocked actions do not render.
- Integration tests for Explore Parties verifying non-member views are removed.
- Integration tests verifying party description is visible on Dashboard for all roles.

#### **Definition of Done**
- Role-aware UI gates implemented across party screens and actions.
- Current user role fetched and cached; UI updates dynamically on change.
- Unauthorized actions hidden and blocked; clear error messaging present.
- Tests pass for gating and interactions; telemetry wired.
- Documentation references the endpoints and role behavior consistently across party components.
- Explore Parties page no longer renders views for non-members.
- Party description is displayed on the Dashboard tab.