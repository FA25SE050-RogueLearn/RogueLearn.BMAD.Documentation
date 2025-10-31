### **Story 4.6: Backend - Implement Guild Management API (CQRS, Roles, Admin Endpoints)**
- **Status:** Done
- **Ownership:** Minh Anh (Backend), with support from Phuc
- **Target Deadline:** Next Weekend

**As a** Guild Master,
**I want** secure Guild Management APIs that follow CQRS and enforce role-based access,
**so that** I can create and manage my guild (members and settings) reliably.

#### **Context & References**
- Social/Guild domain within the SocialService (or equivalent).
- Will be consumed by future Guild UI and by Event Service for guild validation (see Story 4.3).

#### **Scope**
- Implement CQRS-style command handlers and query handlers for the Guild domain (no domain events required).
- Assign “GuildMaster” role to the guild creator.
- Use `/api/admin/...` routes for endpoints requiring guild admin permission (GuildMaster or PlatformAdmin).
- Cover flows: create/configure guild, invite/join, member management (remove, transfer leadership), leave, delete.

---

#### **Acceptance Criteria**
1. **CQRS structure in codebase**
   - Guild domain uses separate command handlers and query handlers.
   - Queries read from the query model/read store (can share DB initially but keep logical separation).

2. **Role application on creation**
   - `POST /api/guilds` (CreateGuild): Authenticated user creates a guild.
   - On success, the creator receives the GuildMaster role for that guild (e.g., claim: `role=GuildMaster; scope=guild:{guildId}`).
   - Response includes `guildId` and initial guild snapshot.

3. **Admin-protected endpoints use `/api/admin/...`**
   - Endpoints under `/api/admin/` require GuildMaster (of the target guild) or PlatformAdmin:
     - `PUT /api/admin/guilds/{guildId}/settings` (ConfigureGuild: name, description, privacy, maxMembers).
     - `POST /api/admin/guilds/{guildId}/invite` (InviteMember: by userId or email).
     - `POST /api/admin/guilds/{guildId}/members/{memberId}/remove` (RemoveMember).
     - `POST /api/admin/guilds/{guildId}/transfer-leadership` (TransferLeadership: to an existing member).
     - `DELETE /api/admin/guilds/{guildId}` (DeleteGuild).

4. **Member/Invitee endpoints (non-admin)**
   - `POST /api/guilds/{guildId}/invitations/{invitationId}/accept` (AcceptInvitation: invitee joins guild).
   - `POST /api/guilds/{guildId}/leave` (LeaveGuild: authenticated member leaves the guild; disallow if GuildMaster and there are other members unless leadership is transferred).
   - `GET /api/guilds/me` returns the current guild overview for the authenticated user.

5. **Queries for UI and dashboards**
   - `GET /api/guilds/{guildId}` returns guild overview (name, description, privacy, master, member count, invite code if caller is master).
   - `GET /api/guilds/{guildId}/members` returns member list (id, displayName, roles, joinedAt).
   - `GET /api/guilds/{guildId}/invitations` returns pending invitations (admin-only).
   - `GET /api/guilds/{guildId}/dashboard` returns key metrics (member count, invitations pending/accepted).

6. **Security & authorization**
   - JWT-based authentication; verify membership and role claims on each call.
   - Admin endpoints under `/api/admin/...` require `GuildMaster` of that `guildId` or `PlatformAdmin`.
   - Non-admin endpoints require authenticated users; some require membership of the target guild.
   - Invite acceptance requires the caller matches the invitation target and that the invite is valid.

7. **Notifications**
   - Notify invitees on `InviteMember`.
   - Notify guild members on membership changes (remove/transfer-leadership).

8. **Data model (minimum)**
   - `Guild { id, name, description, privacy, maxMembers, masterId, inviteCode, createdAt }`
   - `GuildMember { guildId, userId, roles[], joinedAt, status }`
   - `Invitation { id, guildId, targetUserId|email, status, expiresAt }`

9. **Testing**
   - Unit tests for command handlers and query handlers.
   - Integration tests for endpoints (authz checks, idempotency on repeated invites, leadership transfer guards).
   - Scenario tests that traverse common flows end-to-end (create, invite, accept, transfer, leave, delete).

---

#### **API Contracts (Draft)**

Commands
- `POST /api/guilds`
  - Request: `{ "name": "Elite Coders", "description": "Guild for C# enthusiasts", "privacy": "invite_only", "maxMembers": 50 }`
  - Response: `{ "guildId": "...", "roleGranted": "GuildMaster" }`

- `PUT /api/admin/guilds/{guildId}/settings`
  - Request: `{ "name": "...", "description": "...", "privacy": "invite_only|public", "maxMembers": 50 }`
  - Response: `204 No Content`

- `POST /api/admin/guilds/{guildId}/invite`
  - Request: `{ "targets": [{"userId": "u2"},{"email": "student@example.com"}], "message": "Join our guild" }`
  - Response: `{ "invitationIds": ["i1","i2", ...] }`

- `POST /api/guilds/{guildId}/invitations/{invitationId}/accept`
  - Request: none (caller must match invitation target)
  - Response: `204 No Content`

- `POST /api/admin/guilds/{guildId}/members/{memberId}/remove`
  - Request: `{ "reason": "inactive" }`
  - Response: `204 No Content`

- `POST /api/admin/guilds/{guildId}/transfer-leadership`
  - Request: `{ "toUserId": "u2" }`
  - Response: `204 No Content`

- `POST /api/guilds/{guildId}/leave`
  - Request: none
  - Response: `204 No Content`

- `DELETE /api/admin/guilds/{guildId}`
  - Request: none
  - Response: `204 No Content`

Queries
- `GET /api/guilds/{guildId}`
- `GET /api/guilds/{guildId}/members`
- `GET /api/guilds/{guildId}/invitations`
- `GET /api/guilds/{guildId}/dashboard`
- `GET /api/guilds/me`

---

#### **Dev Notes**
- Follow service conventions (controllers -> handlers -> domain/services -> repositories).
- Prefer idempotent handling for repeatable actions (e.g., sending the same invite).
- Enforce member limits and privacy rules.
- Use opaque `inviteCode` (GUID or short hash) for join-by-code flows if needed by UI.
- Instrument audit logs for admin actions under `/api/admin/...`.

#### **Definition of Done**
- All endpoints above implemented and secured (JWT + role checks).
- Unit and integration tests pass; scenario tests cover end-to-end guild lifecycle.
- API docs and Postman/Insomnia collection updated.
- Event Service (Story 4.3) can validate guild role via this service.