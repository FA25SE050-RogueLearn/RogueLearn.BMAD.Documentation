### **Story 4.15: Frontend - Guild Role Management UI and Promotion Flow**
- **Status:** Draft
- **Ownership:** Frontend Team
- **Target Deadline:** TBD

**As a** Guild member,
**I want** the guild UI to adapt to my role (GuildMaster, Officer, Veteran, Recruit),
**so that** I only see and can perform actions appropriate to my permissions.

#### **Roles**
- **GuildMaster:** Full administrative access; assign/revoke any role; transfer leadership; configure settings; invite/remove members; pin/lock/approve posts; create events/parties; delete guild.
- **Officer:** Moderate members and posts; invite users; approve/decline join requests; pin/lock/approve posts; create events/parties; cannot transfer leadership or delete guild.
- **Veteran:** Create posts; edit/delete own posts; join events/parties; mentor recruits; no role management.
- **Recruit:** View guild home and posts; react/comment (if enabled); join events/parties; posting limited by feature flags.

#### **Acceptance Criteria**
1. On `/community/guilds/:guildId`, visible actions reflect current user role; unauthorized actions do not render.
2. On `/community/guilds/:guildId/home`, `QuickActions` show admin shortcuts for GuildMaster/Officer; Veterans see create/join; Recruits see view/join.
3. On `/community/guilds/:guildId/posts`, GuildMaster/Officer can Pin/Lock/Approve; Veterans can Create/Edit/Delete own posts; Recruits view and optionally comment/react.
4. On `/community/guilds/:guildId/manage`, only GuildMaster/Officer can access; `RoleManagementTab` and `SettingsManagementTab` are visible to GuildMaster only.
5. Member actions: GuildMaster can Promote/Demote, Assign/Remove roles, Remove from guild; Officer can Promote to Veteran, Demote to Recruit, Remove non-officer members.
6. Role changes call admin endpoints and reflect immediately after success; stale role state on 403 triggers a role re-fetch.
7. Deep-link to protected tabs shows a permission message and safe navigation.
8. Component and integration tests verify gating across critical actions and tabs.

#### **UI Scope**
- Route: `/community/guilds/:guildId`, `/community/guilds/:guildId/home`, `/community/guilds/:guildId/posts`, `/community/guilds/:guildId/manage`.
- Components:
  - `GuildHeader` shows role badges and gated primary actions.
  - `GuildTabs` conditionally shows `Manage` based on role.
  - `RoleBadges` displays the current role.
  - `QuickActions` renders role-specific shortcuts.
  - `MemberManagementTab`, `RoleManagementTab`, `SettingsManagementTab` gated appropriately.
  - `InviteMembersPanel`, `JoinRequestList` visible to GuildMaster/Officer.
  - `PostList`, `PostCard`, `CreatePostForm` render permitted actions per role.
  - `ConfirmDialog` confirms role changes and removals.

#### **Role Gate Pattern**
- Introduce a `RoleGate` utility to conditionally render based on role.
  - Props: `requireAny: Role[]`, `requireAll: Role[]`, and optional `exclude: Role[]`.
- Introduce a `useGuildRoles(guildId)` hook to read roles for the current user and subscribe to changes; refresh on 403.
- Centralize role constants: `GuildMaster`, `Officer`, `Veteran`, `Recruit`.

#### **Data & APIs**
- Read members with roles: `GET /api/guilds/:guildId/members` (roles embedded).
- Read explicit roles: `GET /api/guilds/:guildId/members/:memberId/roles`.
- Admin role ops: `POST /api/admin/guilds/:guildId/roles/assign`, `POST /api/admin/guilds/:guildId/roles/revoke`.
- Admin member ops: `POST /api/admin/guilds/:guildId/members/{memberId}/remove`, `POST /api/admin/guilds/:guildId/invite`.

#### **Error Handling & UX**
- Permission-denied: concise toast/banner explaining required role; hide action controls.
- Loading & failure states: render skeletons and retry affordance; fall back to minimal view if role fetch fails.
- Role changed mid-session: UI updates dynamically; revoked permissions hide actions without breaking layout.
- Protected deep-links: show `Access restricted` message and a safe back link.

#### **Edge Cases**
- Multiple roles: treat GuildMaster/Officer as superseding Veteran/Recruit.
- No role in guild: show only non-privileged views and join/leave options.
- Leadership transfer: require confirmation before allowing GuildMaster to leave.
- Minimum officer policy: block demotion below policy minimum with messaging.

#### **Telemetry**
- Track gating visibility, permission-denied events, and successful privileged actions.

#### **Testing**
- Unit tests for `RoleGate` logic (`requireAny`, `requireAll`, `exclude`).
- Integration tests across guild routes verifying visible actions per role and that blocked actions do not render.
- Tests verifying posts actions per role and home `QuickActions` per role.

#### **Definition of Done**
- Role-aware UI gates implemented across guild pages and actions.
- Current user role fetched and cached; UI updates dynamically on change.
- Unauthorized actions hidden and blocked; clear error messaging present.
- Tests pass for gating and interactions; telemetry wired.
- Documentation references endpoints and role behavior consistently across guild components.