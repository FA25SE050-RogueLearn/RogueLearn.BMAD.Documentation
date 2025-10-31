### **Story 4.8: Backend - Implement Role Management for Party and Guild (CQRS, RBAC, Admin Endpoints)**
- **Status:** Done
- **Ownership:** Minh Anh (Backend), with support from Phuc
- **Target Deadline:** Next Weekend

**As an** Admin (Party Leader or Guild Master),
**I want** to assign and revoke roles for members in my party or guild,
**so that** we can enforce permissions (moderation, resource management) consistently.

#### **Context & References**
- Builds on Party Management (Story 4.5) and Guild Management (Story 4.6).
- No domain events required; maintain CQRS separation for commands and queries.
- Roles are stored on membership records (PartyMember.roles[], GuildMember.roles[]).

#### **Scope**
- Define a minimal, consistent RBAC for Party and Guild contexts.
- Implement admin endpoints under `/api/admin/...` to manage member roles.
- Enforce safeguards (e.g., cannot revoke Leader/Master role; leadership transfer uses dedicated endpoints from 4.5/4.6).

---

#### **Role Model (Initial)**
- Party Roles: `PartyLeader` (implicit for creator), `PartyModerator`, `PartyMember`.
- Guild Roles: `GuildMaster` (implicit for creator), `GuildModerator`, `GuildMember`.
- Constraints:
  - `PartyLeader` and `GuildMaster` cannot be revoked via role management; use `transfer-leadership` endpoints.
  - A member must have at least one role (Member) when part of the group.
  - Duplicate role assignment is idempotent and should not create duplicates.

#### **Acceptance Criteria**
1. **CQRS structure**
   - Separate command handlers and query handlers for Role Management (PartyRoles, GuildRoles).
   - Queries read from a logically separated read model.

2. **Admin endpoints use `/api/admin/...`**
   - Party:
     - `POST /api/admin/parties/{partyId}/roles/assign` assigns a role to a member.
     - `POST /api/admin/parties/{partyId}/roles/revoke` revokes a role from a member.
   - Guild:
     - `POST /api/admin/guilds/{guildId}/roles/assign` assigns a role to a member.
     - `POST /api/admin/guilds/{guildId}/roles/revoke` revokes a role from a member.
   - Authorization:
     - Party endpoints require `PartyLeader` (of {partyId}) or `PlatformAdmin`.
     - Guild endpoints require `GuildMaster` (of {guildId}) or `PlatformAdmin`.

3. **Member endpoints (read-only)**
   - Party: `GET /api/parties/{partyId}/members/{memberId}/roles` returns role list (self or admin viewing).
   - Guild: `GET /api/guilds/{guildId}/members/{memberId}/roles` returns role list.

4. **Validation & safeguards**
   - Target member must exist and belong to the specified party/guild.
   - Only supported roles can be assigned/revoked (enum validated).
   - Cannot revoke `PartyLeader` or `GuildMaster` using these endpoints.
   - When revoking `PartyMember`/`GuildMember`, ensure at least one role remains or the operation is blocked (use member removal endpoints to fully remove a member).
   - Idempotency: assigning an already-present role or revoking a non-present role should be no-ops with `204 No Content`.

5. **Queries for admin UI**
   - Party: `GET /api/parties/{partyId}/members` includes each member's roles.
   - Guild: `GET /api/guilds/{guildId}/members` includes each member's roles.

6. **Security & authorization**
   - JWT-based authentication; verify membership and admin role claims.
   - Audit log entries are recorded for all role changes.

7. **Data model (updates)**
   - PartyMember.roles[] stores strings from the Party role enum.
   - GuildMember.roles[] stores strings from the Guild role enum.
   - Optional audit record: `RoleChange { id, context: "party|guild", contextId, actorId, targetMemberId, action: "assign|revoke", role, createdAt }`.

8. **Testing**
   - Unit tests for role command handlers and validators.
   - Integration tests for endpoints (authz checks, idempotency, safeguards on Leader/Master).
   - Scenario tests: assign moderator, revoke moderator, view roles.

---

#### **API Contracts (Draft)**

Commands
- `POST /api/admin/parties/{partyId}/roles/assign`
  - Request: `{ "memberId": "m1", "role": "PartyModerator" }`
  - Response: `204 No Content`

- `POST /api/admin/parties/{partyId}/roles/revoke`
  - Request: `{ "memberId": "m1", "role": "PartyModerator" }`
  - Response: `204 No Content`

- `POST /api/admin/guilds/{guildId}/roles/assign`
  - Request: `{ "memberId": "m1", "role": "GuildModerator" }`
  - Response: `204 No Content`

- `POST /api/admin/guilds/{guildId}/roles/revoke`
  - Request: `{ "memberId": "m1", "role": "GuildModerator" }`
  - Response: `204 No Content`

Queries
- `GET /api/parties/{partyId}/members/{memberId}/roles`
- `GET /api/guilds/{guildId}/members/{memberId}/roles`
- `GET /api/parties/{partyId}/members` (roles embedded)
- `GET /api/guilds/{guildId}/members` (roles embedded)

---

#### **Dev Notes**
- Centralize role enums and validation in a shared RBAC module within SocialService.
- Consider short-lived token re-issue after role changes, or rely on backend role checks per request.
- Ensure consistent error responses for invalid role operations (e.g., `422 Unprocessable Entity`).
- Instrument structured audit logs for admin actions.

#### **Definition of Done**
- All role management endpoints implemented and secured.
- Tests pass (unit, integration, scenarios).
- API docs and Postman/Insomnia collection updated.
- Party and Guild UIs (future) can read and reflect roles in member lists.