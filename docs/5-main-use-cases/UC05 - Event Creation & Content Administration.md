### **UC-05: System Administration & Content Governance**

This use case details how the "Master Templates" are created. This is the **Factory** that produces the content consumed in UC-01.

**Actor:** System Admin (Game Master)

---

## Transactions

1.  **Curriculum Import (Structure Layer)**
    *   **Action:** Admin uploads the "K20 Software Engineering" curriculum file.
    *   **System Process:** The `ImportCurriculumCommandHandler` creates the skeleton `Subject` entities (e.g., PRF192, PRN211) and their relationships.

2.  **Syllabus Ingestion (Data Layer)**
    *   **Action:** Admin uploads the specific Syllabus HTML for "PRF192".
    *   **System Process:** The `ImportSubjectFromTextCommandHandler`:
        *   Extracts learning outcomes and session schedules via AI.
        *   Enriches sessions with **Live Web Search** URLs (e.g., finding a GeeksForGeeks article for "Pointers").
        *   Saves the `Subject.Content` JSONB blob.

3.  **Master Quest Generation (Content Layer)**
    *   **Context:** The subject PRF192 exists but has no interactive Quest Steps.
    *   **Action:** Admin clicks **"Generate Quest Content"**.
    *   **System Process (The Engine):** The `GenerateQuestStepsCommandHandler` executes:
        1.  **Topic Grouping:** The `TopicGrouperService` bundles the 30 syllabus sessions into logical "Modules" (e.g., "Variables", "Control Flow", "Pointers").
        2.  **3-Lane Generation:** The `QuestGenerationPlugin` calls the AI *once* per module to generate **Three Parallel Tracks**:
            *   **Standard:** The core curriculum.
            *   **Supportive:** Extra scaffolding and analogies (consumed by Student A in UC-01).
            *   **Challenging:** Advanced edge cases.
    *   **Outcome:** The `quest_steps` table is populated with all variants, ready for instantiation.

4.  **Content Editing & Quality Control**
    *   **Context:** Admin notices a generated quiz question in the "Standard" track is ambiguous.
    *   **Action:** Admin opens the **Quest Step Editor** (React Admin Dashboard).
    *   **Action:** Admin modifies the JSON content of the step.
    *   **System Process:** `UpdateQuestStepContentCommandHandler` saves the changes.
    *   **Impact:** All students currently on the "Standard" track immediately see the corrected question.

5.  **Feedback Resolution (Maintenance Layer)**
    *   **Context:** A student reports a broken link in PRF192 via the "Report Issue" button.
    *   **Action:** Admin views the **Feedback Dashboard**.
    *   **Action:** Admin clicks "Resolve", fixes the URL in the Master Template, and the system marks the `UserQuestStepFeedback` as resolved.