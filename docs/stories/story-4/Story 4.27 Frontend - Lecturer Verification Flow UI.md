### **Story 4.27: Frontend - Lecturer Verification Flow UI**
- **Status:** Done
- **Ownership:** Frontend Team (TBD)
- **Target Deadline:** TBD

**As a** user who is a lecturer,
**I want** a clear way to submit a verification request and track its status,
**so that** I can get verified and access lecturer capabilities; and **as an** admin,
**I want** a dashboard to approve/decline lecturer requests,
**so that** platform roles remain trustworthy.

#### **Context & References**
- Backend verification API: `docs/stories/story-4/Story 4.26 Backend - Lecturer Verification Requests API.md`.
- Roles foundation: `docs/stories/story-1/1.4.user-role-management.md`.
- Dashboard & user profiles: `docs/stories/story-4/Story 4.21 Frontend - Dashboard & User Profiles Enhancements.md`.

#### **Scope**
- Account creation completes first; lecturer verification is prompted immediately after sign-up (onboarding), not during registration.
- If the user selects "I am a Lecturer" during registration, proceed to create the account, then direct them to the verification prompt.
- User dashboard CTA: "Are you a Lecturer?" opens the request form for later completion.
- Request form collects email (pre-filled), staff ID (required), screenshot (optional upload or URL).
- My Requests list shows statuses and decline reasons.
- Admin dashboard tab for lecturer requests with list, detail (screenshot preview), approve, and decline (reason required).
- After form submission, user enters `Pending` state for lecturer verification; lecturer features gated until approved.
- Session/claims refresh after approval to reflect `Verified Lecturer` role in UI.

---

#### **Acceptance Criteria**
1. **CTA & form**
   - A visible CTA (button/link) "Are you a Lecturer?" appears in the user dashboard/profile area.
   - Clicking opens a form with fields `{ email (pre-filled, read-only), staffId (required), screenshot (optional file or URL) }`.
   - On submit, call `POST /api/lecturer-verification/requests` with JSON or `multipart/form-data`; show success state and navigate to My Requests.
   - Account creation completes first. If "I am a Lecturer" was selected during registration, post-signup onboarding prompts the user to submit lecturer details; the pending request is created only after the form submission.

2. **My Requests list**
   - A page or tab lists all of the current user’s requests with `status` badges: `Pending`, `Approved`, `Declined`.
   - Declined requests surface the `reason` string; Pending shows submitted timestamp.
   - Loading, empty, and error states are present and accessible.

3. **Admin dashboard tab**
   - Under `/admin/lecturer-requests`, show a list of requests with filters by `status` and search by user.
   - Selecting a request opens a detail view showing email, staff ID, and screenshot preview when provided.
   - Approve action performs `POST /api/admin/lecturer-verification/requests/{id}/approve`; Decline opens a modal requiring `reason` and calls the decline endpoint.
   - UI updates list/detail states immediately on success; errors show inline/toast messages.

4. **Role reflection**
   - After approval, the UI refreshes user session/claims (e.g., re-fetch `/api/users/me`) so the `Verified Lecturer` badge/permissions appear without a full logout.
   - Lecturer-gated features become available; non-lecturers remain gated.

5. **Accessibility & UX**
   - Keyboard navigation, focus management, ARIA labels for form and table interactions.
   - Consistent skeleton loaders and empty-state messaging.

---

#### **Dev Notes**
- Components: build with Shadcn/UI and Tailwind; reuse dashboard layout patterns.
- Data fetching: stale-while-revalidate and optimistic UI for approve/decline.
- Upload: support `multipart/form-data` file upload for screenshot with preview and retry.
- Errors: friendly messages for conflicts (existing pending request) and validation failures.
- Security: hide admin tab for non-admins; guard routes and actions client-side and server-side.

- #### **Routes & Components**
- Routes
  - Onboarding: `/onboarding/lecturer-verification` (post-signup prompt to complete verification).
  - User: `/dashboard/lecturer-verification` (CTA entry) and `/dashboard/lecturer-verification/requests` (My Requests).
  - Admin: `/admin/lecturer-requests` (list) and `/admin/lecturer-requests/:id` (detail).
- Components
  - `LecturerCTA`, `LecturerRequestForm`, `MyLecturerRequestsList`.
  - `AdminLecturerRequestsTable`, `LecturerRequestDetail`, `ApproveDeclinePanel`.

#### **API Alignment**
- Create request: `POST /api/lecturer-verification/requests` with `{ email, staffId, screenshotUrl? }` or `multipart/form-data` file.
- My requests: `GET /api/lecturer-verification/requests`.
- Admin list/detail: `GET /api/admin/lecturer-verification/requests[/{id}]`.
- Approve: `POST /api/admin/lecturer-verification/requests/{id}/approve`.
- Decline: `POST /api/admin/lecturer-verification/requests/{id}/decline` (reason required).

#### **Tasks / Subtasks**
- [x] Add CTA and route scaffold in dashboard; wire form to create endpoint (AC 1)
- [x] Implement My Requests list with statuses and decline reasons (AC 2)
- [x] Build admin list with filters and detail; approve/decline interactions (AC 3)
- [x] Refresh session/claims after approval; show lecturer badge/gates (AC 4)
- [x] Accessibility, loaders, empty/error states pass (AC 5)

#### **Testing**
- Unit: form validation, status badge rendering, admin modal enforcement of reason.
- Integration: create flow → appears in My Requests; admin approve/decline; client-side gating.
- E2E: user submit → admin approve → user sees Approved and lecturer features; decline path shows reason.

#### **Definition of Done**
- Lecturer verification flow implemented end-to-end in the UI.
- Admin can review and approve/decline with required reason; states sync correctly.
- User session reflects role changes promptly; accessibility validated; tests pass.