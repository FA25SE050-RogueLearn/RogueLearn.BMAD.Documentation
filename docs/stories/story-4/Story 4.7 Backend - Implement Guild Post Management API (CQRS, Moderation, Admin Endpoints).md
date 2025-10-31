### **Story 4.7: Backend - Implement Guild Post Management API (CQRS, Moderation, Admin Endpoints)**
- **Status:** Done
- **Ownership:** Minh Anh (Backend), with support from Phuc
- **Target Deadline:** Next Weekend

**As a** Guild Member,
**I want** to create and manage posts within my guild,
**and as a** Guild Master/Moderator,
**I want** to moderate, pin, and control visibility of posts,
**so that** the guild has a reliable communication channel.

#### **Context & References**
- Social/Guild domain within the SocialService.
- Complements Story 4.6 (Guild Management API).

#### **Scope**
- Implement CQRS-style command handlers and query handlers for Guild Posts (no domain events required).
- Authoring: create, edit, delete own posts.
- Admin actions under `/api/admin/...`: pin/unpin, lock/unlock, approve/reject (if moderation enabled), force-delete.
- Simple attachments and tags support; optional reactions can be added later.

---

#### **Acceptance Criteria**
1. **CQRS structure in codebase**
   - Guild Post domain uses separate command handlers and query handlers.
   - Queries read from the query model/read store (may share DB initially, but keep logical separation).

2. **Authoring permissions**
   - Any authenticated guild member can create posts in the guild.
   - Authors can edit or delete their own posts unless a post is locked by admin.

3. **Admin-protected endpoints use `/api/admin/...`**
   - Endpoints under `/api/admin/` require GuildMaster or Moderator (of target guild) or PlatformAdmin:
     - `POST /api/admin/guilds/{guildId}/posts/{postId}/pin`
     - `POST /api/admin/guilds/{guildId}/posts/{postId}/unpin`
     - `POST /api/admin/guilds/{guildId}/posts/{postId}/lock`
     - `POST /api/admin/guilds/{guildId}/posts/{postId}/unlock`
     - `POST /api/admin/guilds/{guildId}/posts/{postId}/approve`
     - `POST /api/admin/guilds/{guildId}/posts/{postId}/reject`
     - `DELETE /api/admin/guilds/{guildId}/posts/{postId}` (force-delete)

4. **Member endpoints (non-admin)**
   - `POST /api/guilds/{guildId}/posts` (CreatePost)
   - `PUT /api/guilds/{guildId}/posts/{postId}` (EditPost)
   - `DELETE /api/guilds/{guildId}/posts/{postId}` (DeleteOwnPost)

5. **Queries for UI**
   - `GET /api/guilds/{guildId}/posts?tag=&authorId=&pinned=&search=` returns paginated list.
   - `GET /api/guilds/{guildId}/posts/{postId}` returns post details.
   - `GET /api/guilds/{guildId}/posts/pinned` returns pinned posts.

6. **Security & authorization**
   - JWT-based authentication; verify guild membership for all non-admin actions.
   - Admin endpoints under `/api/admin/...` require `GuildMaster` or `Moderator` of that `guildId` (or `PlatformAdmin`).
   - Respect `locked` state (authors cannot edit/delete when locked by admin).

7. **Notifications**
   - Notify guild members on new posts and on admin actions (pin/lock/approve/reject) where appropriate.

8. **Data model (minimum)**
   - `GuildPost { id, guildId, authorId, title, content, tags[], attachments[], status: "published|pending|rejected", pinned: bool, locked: bool, createdAt, updatedAt }`
   - `GuildPostAttachment { id, postId, url|fileId, name, size, mimeType }`

9. **Testing**
   - Unit tests for command handlers and query handlers.
   - Integration tests for endpoints (authz checks, lock/pin/moderation behavior).
   - Scenario tests covering authoring and moderation flows.

---

#### **API Contracts (Draft)**

Commands
- `POST /api/guilds/{guildId}/posts`
  - Request: `{ "title": "Sprint Planning", "content": "Planning notes...", "tags": ["planning"], "attachments": [{ "url": "https://...", "name": "agenda.pdf" }] }`
  - Response: `{ "postId": "p1" }`

- `PUT /api/guilds/{guildId}/posts/{postId}`
  - Request: `{ "title": "...", "content": "...", "tags": ["..."], "attachments": [ ... ] }`
  - Response: `204 No Content`

- `DELETE /api/guilds/{guildId}/posts/{postId}`
  - Request: none (author only unless admin)
  - Response: `204 No Content`

- `POST /api/admin/guilds/{guildId}/posts/{postId}/pin`
  - Request: none
  - Response: `204 No Content`

- `POST /api/admin/guilds/{guildId}/posts/{postId}/unpin`
  - Request: none
  - Response: `204 No Content`

- `POST /api/admin/guilds/{guildId}/posts/{postId}/lock`
  - Request: none
  - Response: `204 No Content`

- `POST /api/admin/guilds/{guildId}/posts/{postId}/unlock`
  - Request: none
  - Response: `204 No Content`

- `POST /api/admin/guilds/{guildId}/posts/{postId}/approve`
  - Request: `{ "note": "Looks good" }`
  - Response: `204 No Content`

- `POST /api/admin/guilds/{guildId}/posts/{postId}/reject`
  - Request: `{ "reason": "Off-topic" }`
  - Response: `204 No Content`

- `DELETE /api/admin/guilds/{guildId}/posts/{postId}`
  - Request: none
  - Response: `204 No Content`

Queries
- `GET /api/guilds/{guildId}/posts?tag=&authorId=&pinned=&search=&page=1&size=20`
- `GET /api/guilds/{guildId}/posts/{postId}`
- `GET /api/guilds/{guildId}/posts/pinned`

---

#### **Dev Notes**
- Reuse authentication and guild membership checks from Story 4.6.
- Enforce content size limits and attachment validation.
- Provide consistent pagination and filtering.
- Instrument audit logs for admin actions under `/api/admin/...`.

#### **Definition of Done**
- All endpoints above implemented and secured (JWT + role checks and membership verification).
- Unit and integration tests pass; scenario tests cover authoring and moderation.
- API docs and Postman/Insomnia collection updated.
- Guild UI can create, edit, delete posts, and admins can pin/lock/approve/reject.