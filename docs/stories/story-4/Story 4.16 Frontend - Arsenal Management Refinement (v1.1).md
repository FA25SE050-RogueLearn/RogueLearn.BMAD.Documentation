### **Story 4.16: Frontend - Arsenal Management Refinement (v1.1)**
- **Status:** Done
- **Ownership:** Frontend Team (TBD)
- **Target Deadline:** TBD

**As a** student,
**I want** a fast, resilient Arsenal (Notes & Tags with AI) experience with clear error handling and sharing,
**so that** I can manage my knowledge without interruptions and collaborate via Party Stash when needed.

#### **Context & References**
- Builds on the initial implementation in: Story 4.13 Frontend - Arsenal Management (Notes + Tags + AI).
- Aligns sharing behavior with: Story 6.2 Party Stash Management  Share Arsenal Notes with Real-time Collaboration.
- Contracts: docs/fullstack-architecture/service-apis/user-service-api.md (Notes/Arsenal), Parties endpoints in Social/Party domain.

#### **Scope**
- Performance: virtualized notes list, memoization, debounced search.
- Reliability: autosave improvements, offline queue with background retry.
- API alignment: verify/adjust 
notesApi, tagsApi, partiesApi routes to current backend contracts.
- UX polish: predictable AI tag suggestion flows, tag management clarity, accessible keyboard navigation.
- Sharing: public note toggle + link copy; "Share to Party Stash" modal for public notes only.

---

#### **Acceptance Criteria**
1. **Notes list performance**
   - List uses virtualization (render only visible rows).
   - Debounced search and immediate tag filter application.
   - Client-side sort (updatedAt desc, title asc) with backend query support when available.
   - Empty state and skeleton loaders for fetching.
   - Network failures show actionable message and retry.

2. **Editor autosave + offline queue**
   - Autosave uses debounce/throttle with visible "Saved" status.
   - If offline or request fails, changes persist locally and enter a retry queue.
   - On reconnect, queued updates sync in order (last-write-wins with warning banner).
   - Local draft recovery available after reload.

3. **Tag management UX**
   - In-editor sidebar supports attach/detach with undo-friendly actions.
   - Creating tags validates non-empty and de-duplicates by normalized case.
   - Errors display clear copy; global tag list reflects changes.

4. **AI tag suggestions alignment**
   - Suggest Tags surface shows loading, confidence scores, optional rationale.
   - Apply commits selected suggestions; failures are non-blocking with retry.
   - Frontend AI endpoints aligned to backend routes (text/upload/commit) per user-service-api.md.

5. **Public note sharing (optional)**
   - Public toggle updates note state; a public badge appears in lists and editor.
   - Copy link button provides shareable URL.
   - Policy violations or 403 show clear error and keep local state consistent.

6. **Share to Party Stash (public notes only)**
   - "Share to Party Stash" action appears only when note is public.
   - Modal collects party selection, stash title, and tags.
   - POST to /api/parties/{partyId}/stash with BlockNote JSON content.
   - Success shows confirmation and link to /party/:partyId/stash.

7. **Accessibility and keyboard navigation**
   - Keyboard shortcuts for editor basics; list and toolbar navigable via keyboard.
   - ARIA labels and visible focus states are present.
   - Color contrast meets guidelines; toasts/alerts are announced.

---

#### **Dev Notes**
- Content format: decide on canonical storage (BlockNote JSON vs HTML). Align 
otesApi payloads and readers consistently.
- Offline queue: use IndexedDB or localStorage to persist unsynced edits; implement exponential backoff on retries.
- Parties integration: only share public notes. Optionally include provenance in tags (e.g., source:note:<noteId>).
- Error handling: unify with typed ApiResponse; surface clear toasts and non-blocking retries.
- Performance: memoize list items; avoid full document re-renders on small changes; cap autosave frequency.
- Accessibility: manage focus on dialog open/close; provide ARIA for controls; ensure keyboard-first navigation.

#### **API Alignment**
- Notes/Arsenal (User Service)
  - Verify getMyNotes, getById, create, update, 
emove routes and response shapes.
  - AI endpoints: suggest-from-text, suggest-from-upload (multipart/form-data), and commit selections.
- Parties Stash
  - Use partiesApi.addResource/getResources/getResourceById/updateResource/deleteResource.
  - Role enforcement handled by backend; UI gates based on responses (403) and role hints where available.

#### **Tasks / Subtasks**
- [x] Virtualized notes list + memoization (AC 1)
- [x] Debounced search & tag filters; client-side sort (AC 1)
- [x] Autosave + offline queue with local persistence and retry (AC 2)
- [x] Tag sidebar: attach/detach/create with validation and clear errors (AC 3)
- [x] AI suggest/apply flows with aligned endpoints and error states (AC 4)
- [x] Public toggle + link copy (AC 5)
- [x] "Share to Party Stash" modal + API integration (AC 6)
- [x] Accessibility pass: labels, focus mgmt, contrast, shortcuts (AC 7)
- [x] Update 
otesApi/	agsApi/partiesApi to current backend routes (API Alignment)
- [ ] Unit, integration, and E2E tests (Testing)

#### **Testing**
- Unit
  - Virtualized list renders only visible items; search/filter debounce works.
  - Autosave debounce triggers update and respects offline state; draft restore works.
  - Tag create/attach/detach handles errors; list updates accordingly.
- Integration
  - 
otesApi/	agsApi/partiesApi aligned to backend; payloads/responses validated.
  - AI suggest/apply state changes verified.
- E2E
  - Create/edit/delete note with offline simulation and recovery.
  - Toggle public and copy link; anonymous view if implemented.
  - Share to Party Stash; item appears in party stash list.
  - Keyboard navigation and screen reader checks.

#### **Definition of Done**
- Measurable performance improvements on large lists.
- Autosave resilient to connectivity issues; queued edits sync on reconnect.
- Tags/AI flows have clear loading/error states; endpoints verified.
- Public note sharing and share-to-stash work reliably with permissions enforced.
- Accessibility pass complete.
- Documentation updated and API alignment noted.
- Tests pass (unit/integration/E2E).
