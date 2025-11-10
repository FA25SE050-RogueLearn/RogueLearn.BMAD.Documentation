# Story 6.2 — Party Stash Management (Adjusted to current endpoints + BlockNote collaboration)

Overview
- As a Party Member, I want to view shared notes in the Party Stash, so I can access collective knowledge.
- As a Party Leader, I want to create, update, and remove stash items, so I can curate content for the party.
- As a Note Owner, I want to share my public Arsenal notes to a party stash, so others can benefit from them.

What changed in this adjusted story
- Aligns with current backend endpoints in PartiesController.cs.
- Aligns with current frontend API services in src/api/partiesApi.ts and types in src/types/parties.ts.
- Uses BlockNote collaborative editing via Yjs provider as per BlockNote Collaboration docs.

Technology and Contracts
- Frontend: Next.js App Router. Party routes include /party/:partyId/stash and /party/:partyId/stash/:stashItemId.
- Editor: BlockNote for stash item content with real-time collaboration using Yjs providers.
- Collaboration reference: BlockNote collaboration feature using Yjs providers (WebRTC, y-websocket, PartyKit, Liveblocks, etc.). See BlockNote docs: https://www.blocknotejs.org/docs/features/collaboration
- Backend: ASP.NET Core, JWT-secured, role-based authorization.

Backend Endpoints (current, per PartiesController.cs)
- POST /api/parties/{partyId}/stash
  - AuthZ: PartyLeaderOnly
  - Body: AddPartyResourceRequest { title: string; content: Record<string, unknown>; tags: string[] }
  - Response: 200 OK with PartyStashItemDto
- GET /api/parties/{partyId}/stash
  - AuthZ: authenticated party member
  - Response: 200 OK with PartyStashItemDto[]
- GET /api/parties/{partyId}/stash/{stashItemId}
  - Response: 200 OK with PartyStashItemDto or 404 Not Found
- PUT /api/parties/{partyId}/stash/{stashItemId}
  - AuthZ: PartyLeaderOnly
  - Body: UpdatePartyResourceRequest { title?: string; content?: Record<string, unknown>; tags?: string[] }
  - Response: 204 No Content
- DELETE /api/parties/{partyId}/stash/{stashItemId}
  - AuthZ: PartyLeaderOnly
  - Response: 204 No Content
- Admin variants exist under /api/admin/parties/... for add/update/delete with AdminOnly.

Frontend API services (current, per src/api/partiesApi.ts)
- partiesApi.addResource(partyId, payload: AddPartyResourceRequest) => ApiResponse<PartyStashItemDto>
- partiesApi.getResources(partyId) => ApiResponse<PartyStashItemDto[]>
- partiesApi.getResourceById(partyId, stashItemId) => ApiResponse<PartyStashItemDto | null>
- partiesApi.updateResource(partyId, stashItemId, payload: UpdatePartyResourceRequest) => Promise<void>
- partiesApi.deleteResource(partyId, stashItemId) => Promise<void>
- Admin: partiesApi.admin.addResource/updateResource/deleteResource

Notes vs Stash Content
- Current NoteDto uses content?: string | null (plain text or HTML).
- PartyStashItemDto.content uses Record<string, unknown> (BlockNote JSON document).
- When sharing a note to stash, the frontend converts the note content to BlockNote’s document JSON and passes it via AddPartyResourceRequest.content. If provenance is desired before backend support for originalNoteId in requests, include it in tags (e.g., ["source:note:<noteId>"]).

Roles & Permissions (current enforcement)
- Member: view stash list and items; cannot create/update/delete.
- Party Leader: can create, update, and delete stash items via the party endpoints.
- Admin: can manage stash items via the admin endpoints.

User Flows (aligned with current endpoints)
1) Share Arsenal Note to Party Stash
- Precondition: Note is public (client-side check using NoteDto.isPublic). Display tooltip and disable action if not public.
- Action: User clicks “Share to Party Stash” from a note.
- UI: Modal lets user select target party and set title/tags for the stash item.
- Request: POST /api/parties/{partyId}/stash with AddPartyResourceRequest { title, content: BlockNote JSON, tags }.
- Response: 200 OK with PartyStashItemDto (id, partyId, title, content, tags, sharedByUserId, timestamps). If using tags for provenance: include source:note:<noteId>.

2) View Party Stash
- Route: /party/:partyId/stash loads partiesApi.getResources(partyId).
- UI: List/grid of stash items with title, sharedByUser, sharedAt/updatedAt. Show a badge if tags include source:note:<id> and link to the original note (if still public).
- Permissions: Members can view, Leaders/Admins see actions to create/edit/delete.

3) Edit Party Stash (Party Leader or Admin)
- Route: /party/:partyId/stash/:stashItemId loads partiesApi.getResourceById.
- UI: If user is Member, render a read-only BlockNote viewer. If Party Leader/Admin, render BlockNote collaborative editor and allow title/tags editing.
- Request: PUT /api/parties/{partyId}/stash/{stashItemId} with UpdatePartyResourceRequest.
- Response: 204 No Content; the UI refreshes or updates local state accordingly.

4) Remove Party Stash (Party Leader or Admin)
- Action: Click “Delete” on a stash item, confirm.
- Request: DELETE /api/parties/{partyId}/stash/{stashItemId}.
- Response: 204 No Content; item disappears from the list.

Collaboration Editing with BlockNote + Yjs
- We enable BlockNote’s collaboration feature.
- Example setup (using a provider such as y-webrtc for development or y-websocket/Liveblocks/PartyKit for production):

```ts
import * as Y from "yjs";
import { WebrtcProvider } from "y-webrtc"; // dev-only, replace in prod
import { useCreateBlockNote } from "@blocknote/react";

// For each stash item editor instance:
const doc = new Y.Doc();
const roomId = `yjs:party:${partyId}:stash:${stashItemId}`; // room naming convention
const provider = new WebrtcProvider(roomId, doc);

const editor = useCreateBlockNote({
  collaboration: {
    provider,
    fragment: doc.getXmlFragment("document-store"),
    user: { name: currentUserName, color: userColor },
    showCursorLabels: "activity",
  },
});
```

- See BlockNote docs for provider options and production setups: https://www.blocknotejs.org/docs/features/collaboration
- Persisting content: periodically snapshot the Yjs state to UpdatePartyResourceRequest.content, or save on blur/interval when collaboration is active.

Acceptance Criteria (updated)
- Share action is disabled when note.isPublic === false; displays helper tooltip.
- Party stash index shows list with optional search/filter client-side; backend currently returns full list without pagination.
- Detail page renders read-only viewer for Members; collaborative editor for Leaders/Admins.
- Presence cursors and real-time updates are visible when two or more authorized editors open the same stash item.
- Errors show clear messages: permission denied (403), item not found (404), collaboration provider unavailable (fallback to non-collab mode).

Error Handling & Edge Cases
- If collaboration provider fails to connect, show a banner and allow non-collaborative editing with local autosave.
- If original note becomes private or is deleted, keep the stash item; badge indicates the source may no longer be public (when provenance tags are used).
- Update endpoint returns 204; ensure UI updates without relying on response body.

Implementation Notes
- Use partiesApi.* methods for all stash operations; rely on ApiResponse wrappers where provided.
- Role detection: use backend role endpoints if needed, or rely on server responses/403 to gate UI.
- Consider feature flag for collaboration provider choice (e.g., LIVEBLOCKS_API_KEY or YJS_SERVER_URL).
- Suggested room naming: yjs:party:{partyId}:stash:{stashItemId}.

Postman/Docs
- Document example requests/responses for the stash endpoints listed above.
- Note that admin variants exist under /api/admin/parties/... and require AdminOnly.

Definition of Done
- Frontend: Share flow (client-side public check), stash list and detail views, role-based UI gates, collaborative editor working with chosen provider, graceful fallback when provider is down.
- Backend: Uses current PartiesController endpoints and role checks; no additional fields required beyond AddPartyResourceRequest/UpdatePartyResourceRequest.
- Documentation: This story updated to reflect current endpoints and collaboration approach; environment variables for provider documented.