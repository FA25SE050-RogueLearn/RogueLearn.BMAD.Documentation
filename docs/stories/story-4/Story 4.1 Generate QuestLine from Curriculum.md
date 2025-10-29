### **Story 4.1: Backend - Generate QuestLine from Curriculum**
-   **Status:** To Do
-   **Ownership:** An (Backend), with support from Minh Anh
-   **Target Deadline:** End of Weekend

**As a** Game Master (Admin),
**I want** to trigger the generation of a QuestLine from a pre-existing curriculum version via an API endpoint,
**so that** the system can transform the academic blueprint into an interactive learning path for students.

#### **Acceptance Criteria**
1.  A new `POST /api/admin/quest-lines/generate-from-curriculum` endpoint is created in the **Quest Service**.
2.  The endpoint accepts a JSON body containing a `curriculumVersionId`.
3.  The service fetches the full curriculum structure (including subjects and terms) from the **User Service** using the provided `curriculumVersionId`.
4.  If the curriculum version is found, the service generates the corresponding `LearningPath` (QuestLine), `Quests`, and `QuestSteps` in its own database.
5.  The endpoint returns the ID of the newly created `LearningPath`.
6.  If the `curriculumVersionId` is invalid or not found, the endpoint returns a `404 Not Found`.
7.  The endpoint is protected and only accessible by users with the 'Game Master' role.

#### **Dev Notes**
*   **Architectural Guidance:** This task is central to the `QuestService`'s purpose as the "Generation Engine." You will need to implement a secure, internal HTTP client within the `QuestService` to call the `UserService` and fetch the academic data created in Story 2.1.
*   **Idempotency:** Ensure that re-running this generation process for the same `curriculumVersionId` updates the existing `LearningPath` rather than creating duplicates.