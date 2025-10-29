### **Story 4.4: Frontend - Implement Basic Party Management UI**
-   **Status:** To Do
-   **Ownership:** Duong (Frontend), once UC-05 scaffolding is stable.
-   **Target Deadline:** Next Weekend

**As a** Player,
**I want** to create and manage Parties through a basic UI (create, view members, get invite code, join, leave),
**so that** I can begin collaborating with friends.

#### **Acceptance Criteria**
1.  Pages are created under `/social/party/`:
    *   `/social/party/create`: A simple form to create a new party.
    *   `/social/party/me`: Displays the user's current party, member list, and an invite code.
    *   `/social/party/join`: A form to join a party using an invite code.
2.  UI components (`PartyCreateForm`, `PartyOverview`, `MemberList`, `JoinByCodeForm`) are created using Shadcn/UI.
3.  The UI components are wired to the Party Management APIs created by Phuc in Story 4.1 (`POST /api/parties`, `GET /api/parties/{id}`, etc.).
4.  Role-specific actions (e.g., "Get Invite Code" for the leader) are conditionally rendered in the UI.
5.  Component tests verify rendering and basic form interactions.
