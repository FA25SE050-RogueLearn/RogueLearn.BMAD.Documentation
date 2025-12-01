### **Story 4.29: Admin — Role Revocation Post‑Revocation Effects (Party Leader, Guild Master, Verified Lecturer)**
- **Status:** Proposed
- **Ownership:** Game Master (Admin), Backend + Frontend Teams
- **Target Deadline:** TBD

**As a** Game Master (Admin),
**I want** to revoke user roles with clear, safe, and auditable post‑revocation outcomes,
**so that** party/guild leadership, educator capabilities, and permissions remain consistent without orphaning responsibilities.

#### **Context & References**
- Role management endpoints and safeguards: `docs/stories/story-4/Story 4.8 Backend - Implement Role Management for Party and Guild (CQRS, RBAC, Admin Endpoints).md:35–55`
- Leadership cannot be revoked via generic role endpoints; use leadership transfer: `docs/stories/story-4/Story 4.8 Backend - Implement Role Management for Party and Guild (CQRS, RBAC, Admin Endpoints).md:26–27`
- Service supports `TransferLeadership`: `docs/stories/story-6/Story 6.3 User Services gRPC Endpoints Implementation.md:157–165`
- Verification affects guild privileges and event hosting: `mermaid-diagrams/erd/conceptual/social-service-erd.md:99`
- Role deletions restricted to prevent orphaned assignments: `mermaid-diagrams/erd/conceptual/user-service-erd.md:73`
- Verified Lecturer role and gating: `docs/stories/story-1/1.4.user-role-management.md:17,75`
- Screen authorization coverage: `docs/srs/3-functional-requirements.md:65–81`

#### **Scope**
- Define precise system behavior after revoking roles for Party Leader, Guild Master, and Verified Lecturer.
- Enforce continuity of ownership (no orphaned leadership), immediate claims refresh, audit logging, and user notifications.
- Gate educator features when Verified Lecturer is revoked; require leadership transfer to restore advanced guild capabilities.

#### **Post‑Revocation Outcomes**
**Party Leader**
- Direct revoke attempt returns `422 Unprocessable Entity` with guidance to use party leadership transfer.
- After transfer:
  - Old leader loses `PartyLeader`; retains other roles (e.g., `PartyModerator`) if present, otherwise becomes `PartyMember`.
  - New leader gains `PartyLeader`; party admin actions remain uninterrupted.
- Immediate actions: claims/session refresh; audit log entries for transfer and role change; notifications to all party members.

**Guild Master**
- Direct revoke attempt returns `422 Unprocessable Entity`; use guild leadership transfer.
- After transfer:
  - Old master loses `GuildMaster` and becomes `GuildMember` or `GuildModerator` per existing roles.
  - New master inherits all guild administration capabilities.
- Immediate actions: claims refresh; audit entries; notifications to guild members; pending approvals/publishing gated to new master.

**Verified Lecturer**
- On revocation:
  - Remove educator badge and hide Lecturer Workspace; educator features disappear without logout after claims refresh.
  - Block educator‑gated creation/publishing flows.
  - If the user is a `GuildMaster`, enforce restricted guild operations until a `GuildMaster` holds `Verified Lecturer`:
    - Set `requires_approval = true` for new member intake.
    - Lock configuration changes; freeze `maxMembers` at its current configured value if it has exceeded 50 members.
    - Invitations and join requests policy:
      - Allow inviting/join requests only while `memberCount < min(maxMembers, 50)`.
      - When `memberCount >= 50`, disallow all invitations and join requests.
  - Immediate actions: claims refresh; audit entries; notifications to the user and affected guilds explaining capability changes.
  - Restoration: when leadership is transferred and the `GuildMaster` becomes a `Verified Lecturer`, restore normal guild operations and unlock settings per policy then set `requires_approval = false`.

#### **Cross‑Cutting Behaviors**
- **Claims Refresh:** Re‑fetch user claims (e.g., `GET /api/users/me`) or reissue tokens to reflect role changes in UI.
- **Security & Safeguards:** Enforce “cannot revoke leader/master via generic endpoints” and prevent members from having zero roles while still in a group.
- **Auditability:** Structured audit logs for every revocation and leadership transfer with actor, target, context, and timestamp.
- **Notifications:** In‑app notices for leadership changes and educator status changes, aligned to platform notification rules.

#### **Acceptance Criteria**
1. Party Leader Revocation Attempt
   - Given an admin calls party role revoke with `PartyLeader`, when constraints are checked, then respond `422` and suggest `TransferLeadership`.
2. Party Leadership Transfer
   - Given an admin transfers leadership to a valid member, when transfer completes, then the target becomes `PartyLeader`, former leader loses the claim, claims refresh occurs, audit records are created, and members receive notifications.
3. Guild Master Revocation Attempt
   - Given an admin calls guild role revoke with `GuildMaster`, when constraints are checked, then respond `422` and suggest `TransferLeadership`.
4. Guild Leadership Transfer
   - Given an admin transfers guild leadership, when completed, then new master gains admin capabilities, former master is demoted, claims refresh and audit logs occur, and members are notified.
5. Verified Lecturer Revocation
   - Given an admin revokes `Verified Lecturer`, when claims update, then educator badge/workspace disappear and educator features are gated; if the user is `GuildMaster`, set `requires_approval = true`, lock settings (freeze `maxMembers`), and allow invitations/join requests only while `memberCount < min(maxMembers, 50)`; if `memberCount >= 50`, block invitations and join requests entirely until leadership is transferred and the `GuildMaster` is a `Verified Lecturer`.
6. Claims & UI Reflection
   - Given any role change, when the frontend re‑fetches user claims, then role‑gated UI updates without requiring logout.
7. Idempotency & Validation
   - Given a role is already absent, when revoke is called, then return `204 No Content` without change; ensure at least one role remains for active group membership.

#### **API Contracts**
- Role revoke (group contexts): `POST /api/admin/parties/{partyId}/roles/revoke`, `POST /api/admin/guilds/{guildId}/roles/revoke` — leader/master constrained.
- Leadership transfer: exposed via service API (`TransferLeadership`) in user/social services.
- Claims refresh: `GET /api/users/me` pattern used post change to reflect updated permissions.

#### **Non‑Functional Requirements**
- Consistency: role changes propagate across UIs within 2 seconds via claims refresh.
- Reliability: no orphaned leadership or memberships; transactional integrity for transfers.
- Auditability: 100% revocations and transfers appear in structured audit logs.
- Security: authorization checks on all endpoints; leadership transfer restricted to valid members under constraints.

#### **Definition of Done**
- Leadership revocation attempts are blocked with actionable errors; transfers succeed via dedicated APIs.
- Verified Lecturer revocation removes educator capabilities immediately and gates affected guild features until a Verified `GuildMaster` is assigned.
- Claims refresh verified in UI flows; notifications sent to impacted users and groups.
- Audit logs present for all changes; tests cover revocation attempts, transfers, educator gating, idempotency, and claims/UI reflection.