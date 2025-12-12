### **UC-01: Personalized Learning Journey & Collaborative Gaming Challenge**

This use case demonstrates the "Master Template Instantiation" model. The student receives a personalized difficulty track derived from their academic history, without waiting for real-time AI generation.

**Actor:** Player (Student)

---

## Transactions

1.  **Onboard and Choose Career Path**
    *   **Action:** Student A logs in and navigates to the onboarding screen.
    *   **Action:** Student A selects a **Curriculum Program** (e.g., "Software Engineering") and a **Class Specialization** (e.g., ".NET/React Fullstack").
    *   **System Process:** The system updates the `user_profiles` table, linking the user to specific `classes` and `curriculum_programs`.

2.  **Sync Academic Record (The Personalization Trigger)**
    *   **Action:** Student A pastes their FAP academic record HTML.
    *   **System Process (Analysis):** The `ProcessAcademicRecordCommandHandler` parses the HTML using the `FapExtractionPlugin`.
    *   **System Process (Linking):** The system executes `GenerateQuestLine`. It iterates through the student's subjects (e.g., **PRF192**).
    *   **System Process (Difficulty Resolution):** The `QuestDifficultyResolver` analyzes the student's history.
        *   *Scenario:* Student A failed PRF192 previously.
        *   *Result:* The system creates a `UserQuestAttempt` for PRF192 with `AssignedDifficulty = 'Supportive'`.

3.  **Student Views Personalized Dashboard**
    *   **Display:** The dashboard shows the computed GPA and the "Programming Fundamentals (PRF192)" Quest.
    *   **Visual Cue:** The quest is marked with a "Supportive Track" badge, indicating tailored content for reinforcement.
    *   **Action:** Student A clicks "Start Quest".

4.  **Student Plays Quest (Master Template Instantiation)**
    *   **Action:** The system loads the Quest Map.
    *   **System Process:** The `UserQuestProgressController` queries the **Master Quest** steps. It filters the content, returning *only* the **Supportive** difficulty variant steps (generated previously by Admin).
    *   **Outcome:** The load is instant; no AI generation occurs at this moment.

5.  **Weekly Activity Completion (Reading & Knowledge Check)**
    *   **Action:** Student A opens "Module 1: C Basics".
    *   **Action:** Student A completes a **Reading** activity (enriched with a tutorial URL) and a **Knowledge Check** (formative assessment).
    *   **System Process:** The `UpdateQuestActivityProgress` handler marks these activities as complete in `user_quest_step_progress`.

6.  **Summative Assessment (The 70% Rule)**
    *   **Action:** Student A attempts the **Module Quiz** (Summative Assessment).
    *   **Constraint:** The `ActivityValidationService` enforces that the quiz cannot be marked complete unless the score is **≥ 70%**.
    *   **Action:** Student A scores 80%.
    *   **System Process:**
        *   The step is marked as `Completed`.
        *   The system triggers an `IngestXpEventCommand`.
        *   **XP Distribution:** 400 XP is awarded and distributed to the specific `UserSkills` (e.g., "C Programming") based on the `subject_skill_mappings`.

7.  **Transition to Collaborative Boss Fight**
    *   **Trigger:** Upon completing the module, the system unlocks the "Memory Allocation Boss Fight".
    *   **Action:** Student A invites Student B (from their Guild) to a real-time lobby.
    *   **System Process:** The system initializes a `GameSession` with a `RelayJoinCode`.

8.  **Collaborative Gameplay**
    *   **Action:** Students A and B fight the boss using the Unity client.
    *   **System Process:** The game server reports the match result to `SubmitUnityMatchResult`.
    *   **Outcome:** Both students receive bonus XP, and the activity is marked complete in their respective learning paths.

