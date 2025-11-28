### **Story 4.21: Frontend - Dashboard & User Profiles Enhancements**
- **Status:** Proposed
- **Ownership:** Frontend Team (TBD)
- **Target Deadline:** TBD

**As a** user,
**I want** improved fetching of other users’ profiles and an enhanced dashboard with refined user tab and profile modal,
**so that** I can explore and connect with others effectively.

#### **Context & References**
- Aligns with user directory and profile flows in docs/front-end-spec.
- APIs: User profile endpoints in docs/fullstack-architecture/service-apis/user-service-api.md.

#### **Scope**
- Better data fetching: caching, skeletons, and resilient error handling for other users’ profile data.
- View other user’s profile: dedicated view or modal, accessible from lists/search.
- Readjust user tab: reorganize information and add a profile modal for quick inspection.
 - User Profile Modal: includes `Settings` and `Lecturer Verification` panels informed by Story 4.27.
   - Settings: account preferences (e.g., display name, notifications) and quick links to profile editing.
   - Lecturer Verification: status badge (Pending/Approved/Declined), CTA to open verification form, link to My Requests.

---

#### **Acceptance Criteria**
1. **Fetching improvements**
   - Requests for other users’ profiles use caching and show skeleton loaders; failures show helpful guidance.
   - Rate limits and retry logic avoid hammering the API; loading states are predictable.

2. **View other user’s profile**
   - From user lists/search, a "View Profile" action opens a profile view or modal.
   - View includes avatar, basic bio, achievements/skills, parties/guilds membership summaries, and privacy-respecting fields.
   - Deep links supported via route (e.g., `/users/:userId`) or modal state.

3. **User tab readjustment**
   - The dashboard’s user tab presents refined sections; a modal allows quick inspection without navigation.
   - Keyboard navigation and accessible labels are present.

4. **Profile modal with Settings & Lecturer Verification**
   - Modal opens from user avatar/user tab and contains tabs: `Profile`, `Settings`, `Lecturer Verification`.
   - Settings tab shows basic account preferences and links; changes persist via appropriate APIs.
   - Lecturer Verification tab displays current status badge (Pending/Approved/Declined), decline reason when applicable, and a CTA that opens the verification form or navigates to `/dashboard/lecturer-verification`.
   - After admin approval, re-fetch `/api/users/me` so modal reflects `Verified Lecturer` status without logout.

5. **Error handling & accessibility**
   - Consistent inline/toast errors; focus management; readable contrast and ARIA roles.

---

#### **Dev Notes**
- Cache: use client-side caching with stale-while-revalidate; memoize computed summaries.
- Privacy: honor visibility flags; never expose PII beyond policy.
 - Modal: share a common profile component between full page and modal; implement tabs for Settings and Lecturer Verification.
 - Verification integration: reuse endpoints and flow from Story 4.27; ensure session/claims refresh after approval.

#### **API Alignment**
- Profiles: `GET /api/users/me` for the current user’s profile and aggregates used by the dashboard and user tab.
 - Lecturer Verification: `POST/GET /api/lecturer-verification/requests[...]` and admin endpoints per Story 4.27.

#### **Tasks / Subtasks**
- [ ] Implement caching/skeleton/error handling for profile fetches (AC 1)
- [ ] Add "View Profile" entry point and component/modal (AC 2)
- [ ] Readjust dashboard user tab and integrate modal (AC 3)
- [ ] Build Profile Modal with Settings and Lecturer Verification tabs; wire verification CTA/status (AC 4)
- [ ] Accessibility & error handling pass (AC 5)

#### **Testing**
- Unit: profile component rendering and states.
 - Integration: list→profile view/modal; caching behaviors; lecturer verification tab interactions.
 - E2E: navigate dashboard → open Profile Modal → switch to Lecturer Verification → open request form → submit → reflect Pending → admin approves → modal shows Verified.

#### **Definition of Done**
- Profile viewing is smooth and resilient; dashboard user tab refined.
 - Profile Modal includes Settings and Lecturer Verification tabs; status and CTA are functional.
 - Accessibility validated; tests pass.

---

### **Story 4.21.B: Backend — Full User Info Endpoint for Dashboard**
- **Status:** Proposed
- **Ownership:** Backend Team (TBD)
- **Target Deadline:** TBD

**As a** user,
**I want** a single endpoint that returns my complete dashboard data (profile, auth summary, and related records),
**so that** the dashboard can render quickly without multiple sequential API calls.

#### **Context & References**
- Aligns with consolidated `User Service` responsibilities in `mermaid-diagrams/ecosystem/services-ecosystem.md`.
- Data relationships per `mermaid-diagrams/erd/physical/user-service-physical.md` and `docs/fullstack-architecture/service-databases/merged-backend-database.md`.
- Frontend consumption tied to Story 4.21 (Dashboard & Profiles) and Story 4.27 (Lecturer Verification status reflection).

#### **Scope**
- Provide aggregated endpoints:
  - `GET /api/users/me/full` — returns the current session user’s full info.
  - `GET /api/users/{auth_user_id}/full` — admin-only or with explicit permission; respects privacy policies.
- Response includes:
  - `profile` — canonical record from `user_profiles`.
  - `auth` — safe subset from `auth.users` (id, email, email_verified, created_at, last_sign_in_at, user_metadata).
  - `relations` — arrays for junctions and related aggregates tied to `user_profiles`.
  - `counts` — optional counters for heavy collections to support preview UIs.
- Query options:
  - `include`: comma-separated list to control related collections (e.g., `roles,skills,achievements,academic,parties,guilds,meetings,notes,notifications`).
  - `page[size]`, `page[number]`: pagination for large relations (notes, achievements, meetings).
  - `fields[entity]`: sparse fieldsets per collection to reduce payload size.

#### **Acceptance Criteria**
1. **Data completeness**
   - Returns `profile` with fields: `auth_user_id`, `username`, `email`, `first_name`, `last_name`, `class_id`, `route_id`, `level`, `experience_points`, `profile_image_url`, `onboarding_completed`, `created_at`, `updated_at`.
   - Returns `auth` subset for the same user: `id`, `email`, `email_verified`, `created_at`, `last_sign_in_at`, `user_metadata`.
2. **Relations coverage (junctions and key related tables)**
   - `user_roles` (role assignments with `role_id`, `assigned_at`; include `roles.name`).
   - Academic: `student_enrollments` and `student_term_subjects`.
   - Skills & progress: `user_skills`, `user_quest_progress`.
   - Achievements: `user_achievements` with joined `achievements` metadata.
   - Social: `party_members` and `guild_members` (include party/guild summaries).
   - Meetings: `meeting_participants` (include meeting summaries where available).
   - Notes: `notes` plus `note_tags`, `note_skills`, `note_quests`.
   - Notifications: `notifications` (latest 20 by default).
   - Lecturer Verification: latest `lecturer_verification_requests` record (if any).
3. **Filtering & pagination**
   - `include` controls which relations are returned; default includes: `roles,skills,achievements,academic`.
   - Pagination is available for `notes`, `achievements`, `meetings`, `notifications` with sensible defaults.
4. **Performance**
   - P95 response time ≤ 400ms for typical users; P50 ≤ 200ms.
   - Uses batched queries and efficient joins; consider read-model or caching for heavy aggregates.
5. **Security & privacy**
   - Auth required. `GET /api/users/{auth_user_id}/full` restricted to admin or the same user.
   - Sensitive fields (password hashes, tokens, PII beyond policy) are excluded.
   - Honors visibility flags; private notes or hidden memberships not included unless actor has permission.
6. **Error handling**
   - Standardized errors: `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `429 Too Many Requests`, `500 Internal Server Error`.
   - Clear messages and codes; rate limiting applied.

#### **Request & Response Examples**

Request (current user):
```http
GET /api/users/me/full?include=roles,skills,achievements,academic,parties,guilds,meetings,notes,notifications&page[size]=20&page[number]=1
Authorization: Bearer <jwt>
```

Response (example):
```json
{
  "profile": {
    "auth_user_id": "8aa40a58-ef31-4b43-9a3a-bf84f4a4f3c6",
    "username": "minh.anh",
    "email": "minh.anh@example.com",
    "first_name": "Minh",
    "last_name": "Anh",
    "class_id": "c-101",
    "route_id": "r-202",
    "level": 7,
    "experience_points": 13450,
    "profile_image_url": "https://cdn.roguelearn.com/avatars/u-8aa4.png",
    "onboarding_completed": true,
    "created_at": "2025-01-15T10:05:22Z",
    "updated_at": "2025-11-24T09:44:10Z"
  },
  "auth": {
    "id": "8aa40a58-ef31-4b43-9a3a-bf84f4a4f3c6",
    "email": "minh.anh@example.com",
    "email_verified": true,
    "created_at": "2025-01-15T09:59:03Z",
    "last_sign_in_at": "2025-11-24T09:40:01Z",
    "user_metadata": { "provider": "supabase", "roles": ["Student"] }
  },
  "relations": {
    "user_roles": [{ "role_id": "role-student", "assigned_at": "2025-01-15T10:05:22Z", "role": { "name": "Student" } }],
    "student_enrollments": [{ "id": "enr-1", "curriculum_version_id": "cv-2024", "status": "Active" }],
    "student_term_subjects": [{ "id": "ts-1", "enrollment_id": "enr-1", "subject_id": "sub-101", "status": "Completed", "grade": "A" }],
    "user_skills": [{ "id": "us-1", "skill_name": "React", "level": 3, "experience_points": 820 }],
    "user_quest_progress": [{ "id": "uqp-77", "quest_id": "q-501", "status": "InProgress" }],
    "user_achievements": [{ "achievement_id": "ach-10", "earned_at": "2025-10-01T12:00:00Z", "achievement": { "name": "Quest Novice" } }],
    "party_members": [{ "party_id": "p-101", "role": "Member", "joined_at": "2025-03-12T08:10:00Z" }],
    "guild_members": [{ "guild_id": "g-88", "role": "Member", "joined_at": "2025-04-01T14:20:00Z" }],
    "meeting_participants": [{ "meeting_id": "m-33", "join_time": "2025-11-21T16:00:00Z" }],
    "notes": [{ "id": "n-901", "title": "Binary Search Trees", "created_at": "2025-06-14T10:30:00Z" }],
    "note_tags": [{ "note_id": "n-901", "tag_id": "t-11" }],
    "note_skills": [{ "note_id": "n-901", "skill_id": "skill-101" }],
    "note_quests": [{ "note_id": "n-901", "quest_id": "q-501" }],
    "notifications": [{ "id": "notif-700", "type": "Achievement", "title": "New Badge", "is_read": false, "created_at": "2025-11-24T09:50:00Z" }],
    "lecturer_verification_requests": [{ "id": "lv-9", "status": "Pending", "submitted_at": "2025-11-20T07:00:00Z" }]
  },
  "counts": {
    "notes": 42,
    "achievements": 12,
    "meetings": 5,
    "notifications_unread": 3
  }
}
```

#### **Dev Notes**
- Implement as a read-model aggregator in the `User Service`; prefer composable repository queries over N+1 patterns.
- Use database views or materialized views where helpful; consider caching hot paths with TTL.
- Respect cross-service boundaries: for Meeting summaries, use lightweight DTOs via internal client or read replica.
- Expose `include` as the primary mechanism to keep payloads constrained; default to essential relations.

#### **API Alignment**
- New endpoints under consolidated User Service: `/api/users/me/full` and `/api/users/{auth_user_id}/full`.
- Frontend consumer: Dashboard and Profile Modal (Story 4.21) with SWR-style caching and skeletons.
- RBAC: Admins may request other users’ `full` data; standard users may only request `me/full`.

#### **Testing**
- Unit: aggregator query composition and DTO mapping for each relation.
- Integration: verify `include` filters, pagination behavior, and RBAC enforcement.
- Contract: freeze response schema for `profile`, `auth`, and top-level `relations` keys.
- Performance: measure response times under realistic relation sizes; ensure P95 ≤ 400ms.

#### **Definition of Done**
- Endpoints implemented with schema-documented responses and error codes.
- Dashboard consumes `me/full` to reduce request fan-out; UI performance improved.
- RBAC and privacy rules enforced; tests green.