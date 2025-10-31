### **Story 4.5: Backend - Implement Party Management API (CQRS, Roles, Admin Endpoints)**
- **Status:** Done
- **Ownership:** Minh Anh (Backend), with support from Phuc
- **Target Deadline:** Next Weekend

**As a** Party Leader,
**I want** secure Party Management APIs that follow CQRS and enforce role-based access,
**so that** I can create and manage my study party (members and resources) reliably.

#### **Context & References**
- Based on UC-03: Creating and Managing Study Party.
- This API will be consumed by the basic Party UI in Story 4.4.
- Place this domain under the Social/Party bounded context in the SocialService (or equivalent).

#### **Scope**
- Implement CQRS-style command handlers and query handlers for the Party domain.
- Assign “Party Leader” role to the party creator.
- Use `/api/admin/...` routes for any endpoint that requires party admin permission (Party Leader or Platform Admin).
- Cover UC-03 transactions: create/configure party, invite/join, upload/categorize resources.

---

#### **Acceptance Criteria**
1. **CQRS structure in codebase**
   - Party domain uses separate command handlers and query handlers.
   - Queries read from the query model/read store (can be the same DB for now, but separated logically).

2. **Role application on creation**
   - `POST /api/parties` (CreateParty): Authenticated user creates a party.
   - On success, the creator receives the Party Leader role for that party (e.g., claim: `role=PartyLeader; scope=party:{partyId}`).
   - Response returns `partyId` and initial party snapshot.

3. **Admin-protected endpoints use `/api/admin/...`**
   - The following endpoints must be under `/api/admin/` and require Party Leader (of the target party) or Platform Admin:
     - `PUT /api/admin/parties/{partyId}/settings` (ConfigureParty: name, subjects, privacy, max members).
     - `POST /api/admin/parties/{partyId}/invite` (InviteMember: by userId or email).
     - `POST /api/admin/parties/{partyId}/members/{memberId}/remove` (RemoveMember).
     - `POST /api/admin/parties/{partyId}/transfer-leadership` (TransferLeadership: to an existing member).
     - `DELETE /api/admin/parties/{partyId}` (DeleteParty).

4. **Member/Invitee endpoints (non-admin)**
   - `POST /api/parties/{partyId}/invitations/{invitationId}/accept` (AcceptInvitation: invitee joins party).
   - `POST /api/parties/{partyId}/resources` (UploadMaterial: subject, description, file metadata or URL). Leader can upload; members may upload if privacy allows.
   - `GET /api/parties/me` returns the current party overview for the authenticated user.

5. **Queries for UI and dashboards**
   - `GET /api/parties/{partyId}` returns party overview (name, subjects, privacy, leader, member count, invite code if caller is leader).
   - `GET /api/parties/{partyId}/members` returns member list (id, displayName, roles, joinedAt).
   - `GET /api/parties/{partyId}/invitations` returns pending invitations (admin-only).
   - `GET /api/parties/{partyId}/resources?subject=` returns resources (name, subject, uploader, createdAt).
   - `GET /api/parties/{partyId}/dashboard` returns key metrics (member count, invitations pending/accepted, resources uploaded).

6. **Security & authorization**
   - JWT-based authentication; verify membership and role claims on each call.
   - Admin endpoints under `/api/admin/...` require `PartyLeader` of that `partyId` or `PlatformAdmin`.
   - Non-admin endpoints require authenticated users; some require membership of the target party.
   - Invite acceptance requires that the caller matches the invitation target and that the invite is valid.

7. **Notifications**
   - Notify invitees on `InviteMember`.
   - Notify party members on `UploadMaterial`.

8. **Data model (minimum)**
   - `Party { id, name, subjects[], privacy, maxMembers, leaderId, inviteCode, createdAt }`
   - `PartyMember { partyId, userId, roles[], joinedAt, status }`
   - `Invitation { id, partyId, targetUserId|email, status, expiresAt }`
   - `Resource { id, partyId, subject, title, url|fileId, description, uploaderId, createdAt }`
9. **Testing**
    - Unit tests for command handlers and query handlers.
    - Integration tests for endpoints (authz checks, idempotency on repeated commands like invite).
    - Scenario tests that traverse UC-03 transactions end-to-end.

---

#### **API Contracts (Draft)**

Commands
- `POST /api/parties`
  - Request: `{ "name": "PRJ-HJF-SBA Party", "subjects": ["PRJ301","HJF101","SBA401"], "privacy": "invite_only", "maxMembers": 8 }`
  - Response: `{ "partyId": "...", "roleGranted": "PartyLeader" }`

- `PUT /api/admin/parties/{partyId}/settings`
  - Request: `{ "name": "...", "subjects": ["..."], "privacy": "invite_only|public", "maxMembers": 8 }`
  - Response: `204 No Content`

- `POST /api/admin/parties/{partyId}/invite`
  - Request: `{ "targets": [{"userId": "u2"},{"email": "student@example.com"}], "message": "Join our party" }`
  - Response: `{ "invitationIds": ["i1","i2", ...] }`

- `POST /api/parties/{partyId}/invitations/{invitationId}/accept`
  - Request: none (caller must match invitation target)
  - Response: `204 No Content`


- `POST /api/parties/{partyId}/resources`
  - Request: `{ "subject": "PRJ301", "title": "Assignment Solutions", "url": "https://...", "description": "Cheat sheets and solutions" }`
  - Response: `{ "resourceId": "r1" }`

- `POST /api/admin/parties/{partyId}/members/{memberId}/remove`
  - Request: `{ "reason": "inactive" }`
  - Response: `204 No Content`

- `POST /api/admin/parties/{partyId}/transfer-leadership`
  - Request: `{ "toUserId": "u2" }`
  - Response: `204 No Content`

- `DELETE /api/admin/parties/{partyId}`
  - Request: none
  - Response: `204 No Content`

Queries
- `GET /api/parties/{partyId}`
- `GET /api/parties/{partyId}/members`
- `GET /api/parties/{partyId}/invitations`
- `GET /api/parties/{partyId}/resources?subject=PRJ301`
- `GET /api/parties/{partyId}/dashboard`
- `GET /api/parties/me`

---

#### **Dev Notes**
- Follow the existing service conventions (controllers -> handlers -> domain/services -> repositories).
- Prefer idempotent command handling where reasonable (e.g., re-sending the same invite).
- Enforce member limits and privacy rules.
- Use opaque `inviteCode` (GUID or short hash) for join-by-code flows if needed by UI.
- Instrument audit logs for admin actions under `/api/admin/...`.
- Rate-limit invites to prevent spam.
- For now, a single DB is acceptable; later we can separate a read model for scalable queries.

#### **Definition of Done**
- All endpoints above implemented and secured (JWT + role checks).
- Unit and integration tests pass; UC-03 scenario tests cover the end-to-end flow.
- API docs and Postman/Insomnia collection updated.
- Story 4.4 UI can successfully create a party, view it, invite, join, and upload a resource.