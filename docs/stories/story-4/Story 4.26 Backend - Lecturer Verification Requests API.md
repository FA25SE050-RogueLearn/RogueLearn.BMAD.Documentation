### **Story 4.26: Backend - Lecturer Verification Requests API**
- **Status:** Done
- **Ownership:** Backend Team (TBD)
- **Target Deadline:** TBD

**As an** aspiring lecturer,
**I want** to submit a verification request and have admins review and approve/decline it,
**so that** my account can be granted the `Verified Lecturer` role once validated.

#### **Context & References**
- Roles and RBAC foundation: `docs/stories/story-1/1.4.user-role-management.md` (Verified Lecturer role exists).
- Admin UI scaffolding patterns: `docs/stories/story-4/Story 4.2 Scaffold Admin Event Management UI.md`.
- Auth/claims refresh considerations: `docs/stories/story-4/Story 4.23 Authentication - Google Sign-In Integration (Supabase).md`.

#### **Scope**
- Persistence model and endpoints for lecturer verification requests (manual KYC style).
- Admin review endpoints: list, detail with screenshot preview, approve (assign role), decline (with reason).
- Idempotency and constraints to avoid duplicate pending requests per user.
- Account flagged `lecturerVerificationStatus: pending|approved|declined`; lecturer features gated until approved.
- Audit logging and events for admin actions.

---

#### **Acceptance Criteria**
1. **Lecturer submits and views requests**
   - `POST /api/lecturer-verification/requests` creates a request for the authenticated user.
     - Body requires `{ email: string, staffId: string }` and optionally `{ screenshotUrl?: string }`.
     - Supports `multipart/form-data` with file field `screenshot` as an alternative to `screenshotUrl`.
     - Server validates `email` against the authenticated account and stores the provided `staffId` and optional screenshot.
     - Returns `{ requestId, status: "pending" }`.
   - `GET /api/lecturer-verification/requests` returns all requests for the current user, including `status`, `reason` (if declined), and timestamps.
   - Registration variation: if a user selects "I am a Lecturer" during sign-up/onboarding, the system auto-creates a `pending` request using the account email and prompts for staff ID and optional screenshot; sets `lecturerVerificationStatus: pending` on the user profile.

2. **Admin listing and detail**
   - `GET /api/admin/lecturer-verification/requests?status=&userId=&page=&size=` returns paginated list with filters for status and user.
   - `GET /api/admin/lecturer-verification/requests/{requestId}` returns request detail including submitted payload and user basics.

3. **Approve with role assignment**
   - `POST /api/admin/lecturer-verification/requests/{requestId}/approve` sets status to `approved`, assigns the `Verified Lecturer` role to the request's user atomically, and updates `lecturerVerificationStatus` to `approved`.
   - Response `204 No Content`; idempotent approval returns `204`.

4. **Decline with required reason**
   - `POST /api/admin/lecturer-verification/requests/{requestId}/decline` sets status to `declined` with mandatory body `{ reason: string }` and updates `lecturerVerificationStatus` to `declined`.
   - Response `204 No Content`; the reason is persisted and visible to the user.

5. **Constraints & validation**
   - Only one `pending` request per user at a time; creating a new request when one is `pending` returns `409 Conflict` with guidance.
   - Users with an `approved` request cannot create new requests; users with a `declined` request may create a new one.
   - No reliance on email pattern/domain; the verified email is the authenticated account email.

6. **Security & authorization**
   - Lecturer endpoints require authenticated user.
   - Admin endpoints require `Admin` role; audit entries recorded for approve/decline actions.

7. **Events & notifications**
   - On approve/decline, publish an event (`LecturerVerificationApproved|Declined`) for UI notification and audit streams, and send a transactional email to the lecturer’s account email.
   - Email content includes action outcome (`Approved` or `Declined`), timestamp, and if declined, the `reason`. Subjects: `Your Lecturer Verification was Approved` / `Your Lecturer Verification was Declined`.

8. **Testing**
   - Unit tests: validators, constraints, role assignment handler.
   - Integration tests: endpoints, RBAC checks, idempotency on approve, required decline reason.

---

#### **API Contracts (Draft)**

Commands
- `POST /api/lecturer-verification/requests`
  - Request (JSON): `{ "email": "user@example.com", "staffId": "FPT-12345", "screenshotUrl": "https://.../proof.png" }`
  - Request (multipart): `email=...`, `staffId=...`, `screenshot=<file>`
  - Response: `{ "requestId": "r-001", "status": "pending" }`

- `POST /api/admin/lecturer-verification/requests/{requestId}/approve`
  - Request: `{ "note": "Verified via HR portal" }`
  - Response: `204 No Content`

- `POST /api/admin/lecturer-verification/requests/{requestId}/decline`
  - Request: `{ "reason": "Insufficient documentation" }`
  - Response: `204 No Content`

Queries
- `GET /api/lecturer-verification/requests`
  - Response: `[{ "id": "r-001", "email": "user@example.com", "staffId": "FPT-12345", "status": "pending", "submittedAt": "2025-11-25T10:00:00Z", "screenshotUrl": "https://.../proof.png" }]`

- `GET /api/admin/lecturer-verification/requests?status=pending&page=1&size=20`
  - Response: `{ "items": [{ "id": "r-001", "userId": "u-123", "status": "pending", "institution": "University of Example" }], "page": 1, "size": 20, "total": 1 }`

---

#### **Data Model (Minimum)**
- `LecturerVerificationRequest { id, userId, email, staffId, screenshotUrl?, status: "pending|approved|declined", reason?, approvedBy?, declinedBy?, submittedAt, updatedAt }`
- `User { id, email, lecturerVerificationStatus: "pending|approved|declined" }` (field added/updated)

#### **Security & RBAC**
- JWT auth required for user endpoints; `Admin` role required for admin endpoints.
- Approve path performs atomic transaction: update request status + assign role via internal call to UserService roles API.

#### **Tasks / Subtasks**
- [x] Create DB migration and EF model for `LecturerVerificationRequest` and `User.lecturerVerificationStatus` (AC 1, 5, 8)
- [x] Implement user endpoints: create and list-my with JSON and multipart support (AC 1)
- [x] Implement admin endpoints: list, detail with screenshot preview, approve, decline (AC 2–4)
- [x] Enforce constraints/idempotency; required decline reason; no email-domain reliance (AC 5)
- [x] Integrate role assignment on approve via UserService and update `lecturerVerificationStatus` (AC 3)
- [x] Add audit logging and publish approve/decline events (AC 7)
- [x] Send transactional emails on approve/decline using the platform mailer with templates (AC 7)
- [x] Unit and integration tests (AC 8)

#### **Testing**
- Unit: validators, constraint checks, role assignment.
- Integration: RBAC, approve idempotency, decline reason enforcement, email dispatch via mocked provider.
- E2E (service-level): create request → admin approve → role present; create request → admin decline → reason visible.

#### **Definition of Done**
- Endpoints implemented and secured; constraints enforced.
- Approvals assign `Verified Lecturer` role atomically; declines require and store reason.
- Users can view their request statuses; admin can manage all requests.
- Tests pass; audit logs and events present; emails sent to lecturer on approve/decline.