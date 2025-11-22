### **Story 4.25: Frontend - Post Comments & Likes UI**
- **Status:** Proposed
- **Ownership:** Frontend Team (TBD)
- **Target Deadline:** TBD

**As a** guild/party member,
**I want** to read, write, and react to comments on posts with reliable feedback,
**so that** discussions are easy to follow and engagement is clear.

#### **Context & References**
- Backend posts authoring and moderation: `docs/stories/story-4/Story 4.7 Backend - Implement Guild Post Management API (CQRS, Moderation, Admin Endpoints).md`.
- Frontend posts/dashboard refinements: `docs/stories/story-4/Story 4.20 Frontend - Guild UI Refinements (Posts, Dashboard, Info).md`.
- Backend comments & likes: `docs/stories/story-4/Story 4.24 Backend - Post Comments & Likes Management.md`.
- Routes: `/community/guilds/:guildId/posts/:postId`, `/community/parties/:partyId/posts/:postId`.

#### **Scope**
- Comment threads: display nested replies with pagination or incremental "load more".
- Compose & reply: inline composer with validation; edit and soft delete own comments.
- Likes & reactions: like/unlike toggle with optimistic counts; optional emoji reaction panel.
- Moderation awareness: show locked indicators; hide/unhide controls for admins; reflect soft-deleted content.
- Accessibility & UX: skeleton loaders, focus management, keyboard navigation, ARIA labels.

---

#### **Acceptance Criteria**
1. **Thread display**
   - Post detail renders comments with nesting up to at least one reply level; deeper replies can collapse/expand.
   - Soft-deleted comments show a placeholder (e.g., "Comment removed") and omit content.
   - Pagination or incremental loading is supported for long threads.

2. **Compose, edit, delete**
   - Inline composer supports new comment and reply; validation errors are clearly shown.
   - Users can edit or soft delete their own comments unless locked.
   - Locked posts or comments disable compose/edit/delete actions with an explanatory message.

3. **Likes & reactions**
   - Like/unlike updates counts optimistically and rolls back on failure.
   - Reaction counts appear on post detail and in lists; if emoji reactions are enabled, show a compact breakdown.

4. **Permissions & moderation**
   - Actions are gated by membership; non-members cannot comment/like.
   - Admins (GuildMaster/Moderator) can hide/unhide comments; UI reflects hidden state.

5. **Loading, errors, and accessibility**
   - Skeleton loaders for comments and counts; empty states are friendly.
   - Errors use consistent inline messaging or toasts; keyboard navigation and ARIA roles are present.

---

#### **Dev Notes**
- Use a stale-while-revalidate pattern for comments and reaction counts; memoize computed aggregates.
- Support deep links to a specific comment via URL param (e.g., `?commentId=c-123`) with auto-scroll and focus.
- Apply consistent component patterns shared across guild and party post pages.

#### **API Alignment**
- Comments: `POST/PUT/DELETE/GET /api/guilds/{guildId}/posts/{postId}/comments[...]`.
- Likes: `POST/DELETE /api/guilds/{guildId}/posts/{postId}/like`; optional emoji reactions under `/reactions`.
- Party posts mirror guild endpoints: `/api/parties/{partyId}/posts/...`.
- Post detail and list endpoints expose `comment_count` and `reaction_counts`.

#### **Tasks / Subtasks**
- [ ] Implement thread components with nesting, collapse, and pagination (AC 1)
- [ ] Build inline composer with reply/edit/soft delete and validation (AC 2)
- [ ] Add like/unlike and optional emoji reaction UI with optimistic updates (AC 3)
- [ ] Gate actions by membership and admin roles; reflect lock/hidden states (AC 4)
- [ ] Integrate loaders, empty/error states, and accessibility patterns (AC 5)

#### **Testing**
- Unit: thread rendering, composer validation, optimistic like/unlike interactions.
- Integration: membership gating, admin hide/unhide, lock state behavior.
- E2E: open post → scroll/load comments → reply/edit/delete → like/unlike → admin hide/unhide.

#### **Definition of Done**
- Comments and likes UI implemented for guild and party posts.
- Counts and thread states accurate and resilient to API failures.
- Accessibility validated; unit/integration/E2E tests pass.