### **Story 4.24: Backend - Post Comments & Likes Management**
- **Status:** Proposed
- **Ownership:** Backend Team (TBD)
- **Target Deadline:** TBD

**As a** guild member,
**I want** to comment on posts and express likes,
**so that** conversations are structured and engagement is measurable across communities.

#### **Context & References**
- Complements Story 4.7 Guild Post Management API (`docs/stories/story-4/Story 4.7 Backend - Implement Guild Post Management API (CQRS, Moderation, Admin Endpoints).md`).
- Builds on Story 4.20 Frontend - Guild UI Refinements (`docs/stories/story-4/Story 4.20 Frontend - Guild UI Refinements (Posts, Dashboard, Info).md`).
- Social Service domain (or consolidated under User Service per architecture notes).

#### **Scope**
- Commenting: CRUD for post comments with threaded replies and soft delete.
- Reactions: simple "like" as default, extensible to emoji reactions.
- Metrics: aggregate counts for comments and reactions returned with post queries.
- Moderation: admin controls for hiding/removing comments and viewing reports.
- Notifications: publish events on new comments and likes.

---

#### **Acceptance Criteria**
1. **Comment endpoints (guild and party posts)**
   - `POST /api/guilds/{guildId}/posts/{postId}/comments` body `{ content, parentCommentId? }` creates a comment (optional reply via `parentCommentId`).
   - `PUT /api/guilds/{guildId}/posts/{postId}/comments/{commentId}` updates own comment content (not when post or comment is locked by admin).
   - `DELETE /api/guilds/{guildId}/posts/{postId}/comments/{commentId}` soft-deletes own comment; admins can hard-delete via `/api/admin/...`.
   - `GET /api/guilds/{guildId}/posts/{postId}/comments?page=&size=&sort=` returns paginated comments with reply nesting and author basics.
   - Equivalent endpoints exist for party posts under `/api/parties/{partyId}/posts/...`.

2. **Like/reaction endpoints**
   - `POST /api/guilds/{guildId}/posts/{postId}/like` idempotently likes a post for the authenticated member; unique per user.
   - `DELETE /api/guilds/{guildId}/posts/{postId}/like` removes the like.
   - Optional emoji reactions: `POST /api/guilds/{guildId}/posts/{postId}/reactions` body `{ emoji }`; unique per `(postId, userId, emoji)`; `DELETE` removes.
   - Reaction counts included in `GET /api/guilds/{guildId}/posts/{postId}` and list queries.

3. **Query model enhancements**
   - Post list/detail responses include `comment_count` and `reaction_counts` (e.g., `{ like: number, emojis: { ":thumbsup:": number } }`).
   - Comment list includes `reply_count` and `is_deleted` flags for soft-deleted comments; content omitted when deleted.

4. **Security & authorization**
   - JWT authentication required; verify membership for guild/party actions.
   - Respect post `locked` state (authors cannot comment or react when locked by admin).
   - Admin endpoints under `/api/admin/...` for comment hide/unhide, hard-delete, and bulk moderation.

5. **Notifications & audits**
   - Publish events for new comments and likes to notify post authors and subscribers.
   - Audit logs record admin moderation actions under `/api/admin/...`.

6. **Data model (minimum)**
   - `PostComment { id, postId, authorId, content, parentCommentId?, isDeleted, isLocked, createdAt, updatedAt }`
   - `PostReaction { id, postId, userId, type: "like"|"emoji", emoji?, reactedAt }`
   - `PostMetrics { postId, commentCount, likeCount, emojiCounts: JSONB }` (optional denormalized table for fast queries).

7. **Testing**
   - Unit tests for comment and reaction handlers.
   - Integration tests for endpoints (auth checks, idempotent likes, soft delete).
   - Scenario tests: create post → comment → reply → like/unlike → moderate.

---

#### **API Alignment**
- Guild posts: extend endpoints from Story 4.7 to include comments and reactions.
- Party posts: mirror guild endpoints under `/api/parties/{partyId}/posts`.
- Query responses: include metric aggregates in list/detail for UI consumption.

#### **Tasks / Subtasks**
- [ ] Implement comment endpoints and thread model (AC 1)
- [ ] Implement like/unlike and optional emoji reactions (AC 2)
- [ ] Enhance post queries with aggregate metrics (AC 3)
- [ ] Enforce security, lock states, and admin moderation (AC 4)
- [ ] Notifications and audit logging integration (AC 5)
- [ ] Unit, integration, and scenario tests (AC 7)

#### **Testing**
- Unit: handlers and validators.
- Integration: role/membership checks; idempotent likes; soft-deleted visibility.
- E2E: navigate to post → add comment → add reply → like/unlike → admin hide.

#### **Definition of Done**
- Commenting and reactions available for guild and party posts.
- Post list/detail include accurate comment and reaction counts.
- Moderation endpoints secure; notifications published; tests pass.