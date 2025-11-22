### **Story 4.17: Frontend - Arsenal Management Refinement (v1.2)**
- **Status:** Done
- **Ownership:** Frontend Team (TBD)
- **Target Deadline:** TBD

**As a** student,
**I want** to embed images in my notes, organize notes via a folder hierarchy, and have clear guidance with previews and audio while editing,
**so that** my Arsenal is richer, easier to navigate, and more supportive during study.

#### **Context & References**
- Builds on: Story 4.13 Frontend - Arsenal Management (Notes + Tags + AI) and Story 4.16 Frontend - Arsenal Management Refinement (v1.1).
- Editor: BlockNote; design patterns per docs/front-end-spec.
- APIs: Notes/Arsenal endpoints in docs/fullstack-architecture/service-apis/user-service-api.md.

#### **Scope**
- Image upload for BlockNote: paste/drag/upload images; upload to Supabase Storage; render inline in editor with accessibility.
- Storage bucket: create and use `notes-media`; store files under `/authUserId/file` categorization.
- Folder hierarchy layout based on tags: left sidebar shows accordion groups by tag; each accordion lists the user’s notes with that tag; persist note `folderPath` derived from tag grouping when applicable.
- Information modal: Arsenal features overview and quick tips; accessible modal tied to the editor layout.
- NoteId enhancements: sidebar preview of previous notes; embedded music player for focused sessions.

---

#### **Acceptance Criteria**
1. **BlockNote image upload**
   - Editor supports paste and file upload for images with an optional alt-text field.
   - Uploads route through Supabase Storage bucket `notes-media`; assets stored under `/authUserId/file` with predictable naming and metadata linked to the note.
   - While uploading, a placeholder appears; failure shows a retry affordance and clear error.
   - Inline images render responsively in the editor and lists.

2. **Folder hierarchy UI (tags-based)**
   - Left sidebar shows accordion groups for each tag owned by the user, plus “All Notes”, “Untagged”, and “Recently Edited”.
   - Notes appear under their tag accordions; drag-and-drop between tag groups updates tags (and `folderPath` when used).
   - Persist tag/group assignments via `notesApi.update`; when backend lacks support, changes are cached locally and reconciled.

3. **Arsenal information modal**
   - A top-right "Info" action opens a modal explaining features: notes, tags, AI, sharing, and keyboard tips.
   - Modal is accessible (focus trap, ESC to close, labeled controls) and non-blocking.

4. **NoteId sidebar + music player**
   - Editor screen includes a sidebar showing recent/previous notes with title and updatedAt; selecting navigates to that note.
   - An embedded audio player supports play/pause/seek/volume; remembers the last track and position.
   - Player never auto-plays; persists across navigation within the Arsenal area.

5. **Error handling & accessibility**
   - All flows show clear error messages and toasts; keyboard navigation and ARIA labels are present.

---

#### **Dev Notes**
- Storage: prefer Supabase Storage or configured assets service; avoid embedding large base64; enforce size limits and progressive uploads.
- Metadata: canonical `folderPath: string[]` stored with notes; plan for backend alignment/migration if necessary.
- Audio: use the native `<audio>` element; maintain a small playlist from settings or curated list; debounce state persist.
- Performance: memoize editor blocks; avoid full doc re-render on image insert; lazy-load previews.

#### **API Alignment**
- Notes/Arsenal
  - `GET /api/notes/me` to list the current user’s notes; `PUT /api/notes/{noteId}` to update metadata (tags, folderPath).
  - `GET /api/tags/me` to list tags owned by the user for accordion grouping.
- Storage
  - Supabase Storage bucket `notes-media` with path schema `/authUserId/file`; access via signed URLs or public-read per policy.

##### **Supabase SQL: notes-media bucket and policies**
```sql
insert into storage.buckets (id, name, public, file_size_limit, allowed_mime_types)
values (
  'notes-media',
  'notes-media',
  true,
  10485760,
  null
)
on conflict (id) do nothing;

alter table storage.objects enable row level security;

grant usage on schema storage to authenticated, anon;
grant select on storage.buckets to authenticated, anon;
grant select on storage.objects to authenticated, anon;
grant insert, update, delete on storage.objects to authenticated;

drop policy if exists "notes_media_public_read" on storage.objects;
create policy "notes_media_public_read" on storage.objects
  for select
  using (
    bucket_id = 'notes-media'
  );

drop policy if exists "notes_media_insert_own_folder" on storage.objects;
create policy "notes_media_insert_own_folder" on storage.objects
  for insert to authenticated
  with check (
    bucket_id = 'notes-media'
    and auth.uid() is not null
    and owner = auth.uid()
    and (
      name like auth.uid()::text || '/%'
      or name = auth.uid()::text
    )
  );

drop policy if exists "notes_media_update_own_or_admin" on storage.objects;
create policy "notes_media_update_own_or_admin" on storage.objects
  for update to authenticated
  using (
    bucket_id = 'notes-media'
    and (
      (
        owner = auth.uid()
        and (
          name like auth.uid()::text || '/%'
          or name = auth.uid()::text
        )
      )
      or public.jwt_has_role('Game Master')
    )
  )
  with check (
    bucket_id = 'notes-media'
    and (
      (
        owner = auth.uid()
        and (
          name like auth.uid()::text || '/%'
          or name = auth.uid()::text
        )
      )
      or public.jwt_has_role('Game Master')
    )
  );

drop policy if exists "notes_media_delete_own_or_admin" on storage.objects;
create policy "notes_media_delete_own_or_admin" on storage.objects
  for delete to authenticated
  using (
    bucket_id = 'notes-media'
    and (
      (
        owner = auth.uid()
        and (
          name like auth.uid()::text || '/%'
          or name = auth.uid()::text
        )
      )
      or public.jwt_has_role('Game Master')
    )
  );
```

#### **Tasks / Subtasks**
- [x] Implement image upload with storage integration (AC 1)
- [x] Implement folder tree sidebar with metadata persistence (AC 2)
- [x] Implement Arsenal info modal (AC 3)
- [x] Implement NoteId sidebar previews and audio player (AC 4)
- [x] Accessibility & error handling pass (AC 5)

#### **Testing**
- Unit: image upload handlers; folder create/rename/move; audio controls.
- Integration: editor saves with embedded images; metadata persists; sidebar previews load correctly.
- E2E: create note → upload image → move to folder → play audio → reload and verify persistence.

#### **Definition of Done**
- Image embedding works reliably; folder UI intuitive; info modal present.
- Previews/music stable across navigation; accessibility checks pass.
- API alignment documented; tests green (unit/integration/E2E).