### **Story 4.30: Browser Extension — Notes Creation & Search**
- **Status:** Proposed
- **Ownership:** Browser Extension + Frontend + Backend
- **Target Deadline:** TBD

**As a** student using the browser extension,
**I want** to create notes from web content and search my notes quickly,
**so that** I can capture learning insights in context and retrieve them later across pages and topics.

#### **Context & References**
- Contextual note UI wireframes: `docs/front-end-spec/browser-extension-wireframes.md:186`, `docs/front-end-spec/browser-extension-wireframes.md:224`, `docs/front-end-spec/browser-extension-wireframes.md:237`
- Arsenal note management epic: `docs/prd/epic-details.md:293`
- Notes schema and tagging: `docs/fullstack-architecture/service-databases/merged-backend-database.md:316`
- Extension storage and search indexing guidance: `docs/prd/integration-requirements.md:168`
- Out-of-scope semantic search reference: `docs/prd/requirements.md:950`

#### **Scope**
- Implement in-extension note creation (page-level and selection-based) with tags and contextual metadata.
- Add in-extension search with filters (text, tags, URL/domain, date) and relevance ranking.
- Support offline-first capture; synchronize with backend when online.
- Respect content security, permissions, and user privacy.

#### **Functional Requirements**
- **Note Creation**
  - Create a note from the extension popup or context menu.
  - Capture context: current URL, page title, and optional selected text.
  - Fields: optional title, body (basic formatting: bold/italic/links), tags, and optional link to learning path.
  - Autosave draft and prevent data loss on popup close.
  - Tagging system: add/remove tags, suggest recent tags.
  - Save associates note to the page and, if selection exists, store a robust selection anchor; gracefully degrade when DOM changes.
- **Search**
  - Full-text search across note titles and body; prefix and substring matches.
  - Filters: tags, URL/domain, date range.
  - Sort: newest, most relevant.
  - Show matched snippets with highlighted query terms.
  - Open note in read/edit mode; navigate back to the page if available.
- **Sync & Storage**
  - Local IndexedDB for notes and search index; background sync to backend endpoints.
  - Conflict resolution: last-write-wins, with local versioning to avoid silent overwrites.
  - Offline mode: queue saves; mark synced status; retry with exponential backoff.
- **Permissions & Security**
  - Permissions: `activeTab`, `storage`, `contextMenus`; avoid excessive host permissions.
  - Sanitize pasted HTML and links; strip scripts.
  - Respect CORS and CSP for extension requests; use bearer auth.
- **Accessibility & UX**
  - Keyboard shortcuts for save, search, and tag navigation.
  - Screen reader-friendly labels and focus management (WCAG 2.1 AA).
  - Clear empty-state for search; debounced input; result count.

#### **Acceptance Criteria**
1. **Create Note (Page-level)**
   - Given the extension popup is open on any page, when I enter a title/body and click Save, then a note is created with URL and page title context stored, and I see a success confirmation.
2. **Create Note (Selection-based)**
   - Given I select text on a page and open the popup, when I add a note and save, then the note includes the selected text context; selection anchor is persisted for revisit.
3. **Tagging**
   - Given I add tags during creation or edit, when I save, then tags persist and appear as filters in search.
4. **Autosave & Draft Recovery**
   - Given I am typing a note, when I close the popup or navigate away, then my draft is preserved and restored on next open until explicitly discarded.
5. **Local Search**
   - Given I have multiple notes, when I enter a query and apply filters (tags/URL/domain/date), then results are ranked and show snippets with highlighted terms.
6. **Offline-first**
   - Given I am offline, when I save a note, then it persists locally and shows “Pending sync”; when back online, it syncs automatically and the status updates to “Synced”.
7. **Conflict Handling**
   - Given a note was edited on another device, when I sync, then the local note resolves via last-write-wins and I see an “Updated remotely” indicator if overwritten.
8. **Performance**
   - Given I type in the search box, when results update, then the first results appear within 200ms for up to 1,000 local notes.
9. **Privacy & Security**
   - Given I paste content with formatting, when the note is saved, then unsafe HTML is removed; links are preserved; no scripts or embedded trackers are stored.
10. **Accessibility**
   - Given I navigate with keyboard only, when I create and search notes, then all controls are reachable and labeled; search results are announced to assistive tech.

#### **API Contracts**
- GET /api/notes/me?search= — Fetch authenticated user's notes (owner-only).
- GET /api/notes/public?search= — Fetch public notes (anonymous allowed).
- GET /api/notes/{id:guid} — Fetch note by ID; returns only public or owned note.
- DELETE /api/notes/{id:guid} — Delete note by ID (owner-only).
- POST /api/notes/upload-and-create — Create note from uploaded file (multipart: file ).
- POST /api/notes/create-with-ai-tags — Create note with AI tag suggestions (JSON: raw text/title fields).
- POST /api/notes/create-with-ai-tags/upload — Create note from file with AI tagging (multipart: file , options).
- POST /api/ai/tagging/suggest — Suggest tags from raw text or existing note (JSON).
- POST /api/ai/tagging/suggest-upload — Suggest tags from uploaded file (multipart: file ).
- POST /api/ai/tagging/commit — Commit selected tags to a note (JSON).
- Authentication via JWT; CORS for extension origin; rate limiting applied.

Code References

- upload-and-create in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:41
- GET /me in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:68
- GET /public in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:86
- GET /{id:guid} in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:102
- POST /api/notes in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:138
- PUT /{id:guid} in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:158
- DELETE /{id:guid} in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:178
- POST /api/ai/tagging/suggest in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:201
- POST /api/ai/tagging/suggest-upload in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:224
- POST /api/ai/tagging/commit in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:256
- POST /api/notes/create-with-ai-tags in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:276
- POST /api/notes/create-with-ai-tags/upload in RogueLearn.User/src/RogueLearn.User.Api/Controllers/NotesController.cs:299

#### **Data Model**
- `notes`: `id`, `auth_user_id`, `title`, `content`, `is_public`, `created_at`, `updated_at` — `docs/fullstack-architecture/service-databases/merged-backend-database.md:316`
- `tags`, `note_tags` — many-to-many for tag associations.

#### **Non-Functional Requirements**
- **Performance:** local search under 200ms for 1k notes; index updates in under 100ms.
- **Reliability:** no data loss on popup close; background sync retries until success.
- **Security:** sanitize input; enforce auth; comply with CSP; no overbroad host permissions.
- **Observability:** track creation, edit, sync success/failure, search usage.

#### **Cross‑Cutting Behaviors**
- IndexedDB-backed local note store and inverted index for search; debounce input and incremental index updates.
- Background sync triggered on network restore; sync status badges in UI.
- Context anchors save both URL and text quote; fall back gracefully when DOM changes.

#### **Definition of Done**
- Note creation and search flows implemented in the extension with offline support.
- Local search indexing and filters deliver fast, relevant results.
- Notes sync to backend with conflict policy and clear statuses.
- Accessibility and security audits pass; telemetry events captured.
- QA covers create/edit/tag, offline/online transitions, search filters, conflict scenarios, and sanitization.

#### **Out of Scope**
- OCR and attachments full-text search; semantic search across notes — `docs/prd/requirements.md:950`
- Collaborative note editing and sharing.
- Advanced rich-text features beyond basic formatting and links.