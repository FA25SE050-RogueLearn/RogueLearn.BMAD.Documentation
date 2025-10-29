### **Story 4.2: Frontend - Scaffold Admin Event Management UI**
-   **Status:** To Do
-   **Ownership:** Duong (Frontend)
-   **Target Deadline:** End of Weekend

**As a** Game Master (Admin),
**I want** to see and interact with the user interface for creating new events and viewing pending approvals,
**so that** I can provide early feedback on the user experience and the team can prepare for backend integration.

#### **Acceptance Criteria**
1.  A new set of pages for event management is created in the Next.js application under `/admin/events`.
2.  A multi-step form for creating a new event is built, based on the `Event Creation Wizard` wireframes. The form does not need to submit to the backend; it can `console.log` the data for now.
3.  A page is created to display a list of pending events, based on the `Event Approval Dashboard` wireframe.
4.  All UI components are built using Shadcn/UI and Tailwind CSS.
5.  The data for the pending events list is sourced from a **local mock JSON file**. This data must match the API contract from the `GET /api/admin/events/pending` endpoint developed in Story 3.1.
6.  The components have basic unit tests to ensure they render correctly with mock data.

#### **Dev Notes**
*   **Architectural Guidance:** This story is frontend-only and can be developed in parallel.
*   **API Dependency:** Use a mock data file (`src/mocks/pending-events.json`) to simulate the API response from the `EventService`. This decouples your work from the backend's progress. Ensure the mock data structure aligns with what Minh Anh is building in Story 3.1.
