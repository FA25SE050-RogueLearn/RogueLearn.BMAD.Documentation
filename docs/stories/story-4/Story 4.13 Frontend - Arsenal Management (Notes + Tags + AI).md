# Story 4.13  Frontend: Arsenal Management (Notes & Tags with AI)

## Overview
The Arsenal is the learners personal knowledge hub. It enables capturing rich notes, organizing with tags, and leveraging AI to suggest tags, summarize, outline, and enhance content. This story defines the frontend implementation built on existing Notes and Tags APIs, with planned integration of BlockNote and BlockNote AI for a Notion-like rich text experience.

## Goals
- Provide an intuitive, fast, and reliable note-taking experience with rich text editing.
- Streamline organization via tags, filters, and search.
- Integrate AI assistance for tag suggestions and in-editor actions (summarize, rewrite, expand).
- Support uploads (PDF/DOCX/TXT) to generate note content and AI tag suggestions.
- Optionally expose public notes with shareable links.

## Non-Goals
- Backend implementation of additional AI models beyond tag suggestion endpoints.
- Complex collaboration features (multi-user editing, comments).
- Advanced semantic search (may be a future story).

## Personas
- Student: Captures class notes, reading summaries, and ideas; wants quick search and meaningful organization.
- Self-learner: Organizes resources, snippets, and principles into tagged collections; values AI suggestions.

## Scope
- Route: `/arsenal` under Next.js App Router.
- Tabs: Notes and Tags (Templates/Snippets optional).
- Editor: BlockNote rich text editor with BlockNote AI actions.
- API Clients: `src/api/notesApi.ts`, `src/api/tagsApi.ts` (align routes with backend before release).

## UX Outline
- Notes Tab:
  - Toolbar: search (debounced), tag filter, sort (updatedAt/title), New Note, Upload file.
  - List: card or table view; title, tags, updatedAt, public badge; actions (edit, delete).
  - Editor Drawer/Modal: BlockNote editor, auto-save, public toggle, tags side panel.
  - AI Assist Panel: suggest tags from note content; commit selected suggestions.
- Tags Tab:
  - List: users tags with search.
  - Create Tag: quick add with validation.
  - Attach/Detach: attach existing or create-and-attach from within the note editor sidebar.
- Optional Templates/Snippets:
  - Templates: start notes from predefined structures (lecture, reading summary, flashcards).
  - Snippets: save and insert code/text snippets into the editor.

## Epics & User Stories

### Epic 1: Notes Management
1. View my notes
   - As a user, I can see my notes in a paginated, searchable list.
   - Acceptance:
     - Uses `notesApi.getMyNotes()`.
     - Search input filters list (debounced). Empty state shows Create Note.
2. Filter and sort notes
   - As a user, I can filter by tags and sort by updated date or title.
   - Acceptance:
     - Tag filter applies to the list (client-side or via query once supported).
     - Sort options: updatedAt desc, title asc.
3. Create a note (rich editor)
   - As a user, I can create a note with title and rich content using BlockNote.
   - Acceptance:
     - New Note opens an editor; Save calls `notesApi.create(payload)`.
     - Auto-save (debounced throttle) calls `notesApi.update(id, payload)` after initial create.
4. Edit a note (rich editor)
   - As a user, I can edit title/content and toggle public/private.
   - Acceptance:
     - Loads via `notesApi.getById(id)`; updates via `notesApi.update(id, payload)`.
     - Public toggle reflected in UI (badge).
5. Delete a note
   - As a user, I can delete a note with confirmation.
   - Acceptance:
     - Confirmation modal; call `notesApi.remove({ id, authUserId })`.
     - Optimistic UI with rollback on error.

### Epic 2: AI-Assisted Notes & Tags
1. Suggest tags for existing note content
   - As a user, I can request AI tag suggestions and apply selected suggestions.
   - Acceptance:
     - Suggest Tags button calls `notesApi.suggestTags({ noteId, authUserId, maxTags? })`.
     - Shows `confidence`, optional `reason`. Apply invokes `notesApi.commitTagSelections({ noteId, selectedTagIds, newTagNames? })`.
2. Create note with AI tags from raw text
   - As a user, I can paste raw text and let AI suggest tags, optionally auto-apply.
   - Acceptance:
     - Calls `notesApi.createWithAiTagsFromText({ authUserId, title?, rawText, maxTags?, applySuggestions? })`.
     - Navigates to the editor for the returned `noteId`.
3. Upload file to create note with AI tags
   - As a user, I can upload a file to generate note content and AI tag suggestions.
   - Acceptance:
     - Calls `notesApi.createWithAiTagsFromUpload(formData)` with file + options.
     - Navigates to editor with returned `noteId`.
4. In-editor AI tools
   - As a user, I can summarize, rewrite, expand, and outline content using BlockNote AI.
   - Acceptance:
     - AI actions accessible as toolbar/slash commands; results inserted with undo support.
     - Progress indicator and error messages displayed.

### Epic 3: Tag Management
1. View my tags
   - As a user, I can view all my tags and search by name.
   - Acceptance:
     - Calls `tagsApi.getMyTags()`; optional client-side search.
2. Create a tag
   - As a user, I can create new tags.
   - Acceptance:
     - Validates non-empty, dedupe by name; calls `tagsApi.create({ authUserId, name })`.
3. Attach/detach tags to notes
   - As a user, I can attach existing tags or create-and-attach to a note.
   - Acceptance:
     - Attach: `tagsApi.attachToNote({ noteId, authUserId, tagId })`.
     - Create-and-attach: `tagsApi.createAndAttach({ noteId, authUserId, name })`.
     - Detach: `tagsApi.removeFromNote({ noteId, authUserId, tagId })`.

### Epic 4 (Optional): Templates & Snippets
1. Templates
   - As a user, I can start from predefined templates to speed note creation.
   - Acceptance:
     - Template picker populates BlockNote blocks.
2. Snippet library
   - As a user, I can save reusable text/code snippets and insert them into notes.
   - Acceptance:
     - Snippet palette inserts content at BlockNote cursor.

## API References (Frontend Types & Clients)
- Notes: `src/types/notes.ts`; client `src/api/notesApi.ts`
  - List: `getMyNotes()`
  - Detail: `getById(id)`
  - Create: `create(payload)`
  - Update: `update(id, payload)`
  - Delete: `remove({ id, authUserId })`
  - AI Tagging:
    - Suggest: `suggestTags(request)`
    - Suggest from upload: `suggestTagsFromUpload(formData)`
    - Commit: `commitTagSelections(command)`
    - Create with AI (text/upload): `createWithAiTagsFromText`, `createWithAiTagsFromUpload`
- Tags: `src/types/tags.ts`; client `src/api/tagsApi.ts`
  - My tags: `getMyTags()`
  - Create: `create(command)`
  - For note: `getTagsForNote(noteId)`
  - Attach/Detach: `attachToNote`, `removeFromNote`
  - Create-and-attach: `createAndAttach(command)`

## Endpoint Alignment
Please verify frontend vs backend route alignment before release.
- Backend (NotesController.cs sample):
  - AI suggest (text): `POST /api/ai/tagging/suggest`
  - AI suggest (upload): `POST /api/ai/tagging/suggest-upload`
  - Commit tag selections: `POST /api/ai/tagging/commit`
  - Create with AI (text): `POST /api/notes/create-with-ai-tags`
  - Create with AI (upload): `POST /api/notes/create-with-ai-tags/upload`
- Frontend (current `notesApi`):
  - Uses `/api/notes/ai/...` paths for suggest/commit/create-with-tags.
Action: Align `notesApi` to the backend routes in the deployed environment.

## Technical Plan
- Pages & Components:
  - `/app/arsenal/page.tsx` with tabs: Notes, Tags (optional Templates/Snippets)
  - `NotesToolbar`, `NotesList`, `NoteEditorModal` (BlockNote), `AIAssistPanel`
  - `TagList`, `TagCreateForm`, `TagAttachWidget`
- Editor & Content:
  - Store BlockNote content as JSON or HTML consistently.
  - Auto-save via debounced updates; preserve unsaved drafts in localStorage.
- Uploads:
  - Use `FormData` with multipart/form-data; client-side validation (size/type).
- AI Config:
  - Provide keys via env (e.g., `BLOCKNOTE_AI_KEY` or provider key).
  - Graceful error handling and loading states.
- Performance & Accessibility:
  - Virtualize long lists, debounce searches, keyboard shortcuts, ARIA labels.

## Tasks (MVP)
1. Create `/app/arsenal/page.tsx` and scaffold tabs.
2. Implement Notes tab (toolbar, list, editor modal with auto-save).
3. Implement AI tag suggestion UI and commit flow.
4. Implement file upload create-with-AI flow.
5. Implement Tags tab (list, create, attach/detach).
6. Validate route alignment and adjust `notesApi` if needed.
7. Add basic tests (unit + Playwright flows).

## Testing
- Unit: components render, editor auto-save triggers update with correct payload.
- Integration: API clients return expected shapes; attach/detach flows update UI.
- E2E (Playwright):
  - Create/edit/delete note.
  - AI tag suggest + commit (mock responses acceptable).
  - Upload-and-create with AI tags.
  - Tag create-and-attach and detach.

## Future Enhancements
- Public note sharing with shareable URLs.
- Collections for grouping notes by topic/course.
- Favorites/pin to top.
- AI semantic search across notes with snippet previews.
- Backlinks and knowledge graph view.
