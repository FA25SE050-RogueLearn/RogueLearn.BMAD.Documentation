### **UC-05: System Administration & Content Governance**

This use case details the "God Mode" operations performed by the System Admin (Game Master) to build and maintain the academic game world and competitive infrastructure.

**Actor:** System Admin (Game Master)

---

## Transactions

1.  **Curriculum "Big Bang" Import (Foundation Layer)**
    *   **Context:** The university releases a new curriculum PDF ("K20 Software Engineering").
    *   **Action:** Admin navigates to "Curriculum Management" and uploads the raw file.
    *   **System Process:** The `ImportCurriculumCommandHandler` triggers the **Curriculum Extraction AI**. It parses the text to create the program structure and empty `Subject` entities for all 50+ courses.
    *   **Outcome:** The academic skeleton is created instantly.

2.  **Syllabus & Resource Enrichment (Detail Layer)**
    *   **Context:** The Admin needs to populate "CSD201 - Data Structures" with actual learning content.
    *   **Action:** Admin selects "CSD201" and uploads its specific Syllabus HTML.
    *   **System Process:**
        *   **Extraction:** The AI extracts learning outcomes, session schedules, and assessment weights.
        *   **Enrichment:** The system performs a live web search to find and link valid, non-paywalled tutorial links (e.g., GeeksForGeeks) for every session topic.
    *   **Outcome:** The subject is now fully populated with a structured learning path.

3.  **Competitive Problem Bank Generation (Event Layer)**
    *   **Context:** Admin wants to stock the "Code Battle" problem bank with new challenges for upcoming guild wars.
    *   **Action:** Admin navigates to "Code Battle Management" -> "Generate Problems".
    *   **Input:** Admin selects a topic (e.g., "Dynamic Programming") and Difficulty (e.g., "Hard").
    *   **System Process:** The AI generates a set of **LeetCode-style Coding Challenges**:
        *   **Problem Statement:** "Climbing Stairs with Variable Steps."
        *   **Boilerplate Code:** C# method signature.
        *   **Test Cases:** A JSON array of 10 hidden test cases (Input/Output pairs) for the auto-grader.
        *   **Reference Solution:** An optimal solution for validation.
    *   **Action:** Admin reviews the problems, edits the test cases if needed, and saves them to the global **Problem Bank**.
    *   **Outcome:** These problems are now available for Guild Masters to select when creating Code Battle events.

4.  **Career Class & Roadmap Definition (Strategic Layer)**
    *   **Action:** Admin creates a new Class: ".NET Backend Developer".
    *   **Input:** Admin uploads a "Roadmap.sh" PDF for Backend Development.
    *   **System Process:** The AI extracts the visual tree nodes (C# -> SQL -> APIs).
    *   **Action:** Admin maps university subjects to these nodes (e.g., "PRN211" maps to "Basic C#").
    *   **Outcome:** Students can visualize how their academic subjects contribute to their career goals.

5.  **Skill Tree & Dependency Governance (RPG Layer)**
    *   **Action:** Admin navigates to "Skill Management".
    *   **Action:** Admin defines "Data Structures" as a prerequisite for "Algorithms".
    *   **Action:** Admin links "CSD201" to the "Data Structures" master skill in the `subject_skill_mappings` table.
    *   **Outcome:** This configuration drives the user's Skill Tree visualization and XP progression logic.

6.  **Quest Feedback & Quality Control (Maintenance Layer)**
    *   **Context:** Users report a bug in a standard quest quiz question.
    *   **Action:** Admin checks the "Feedback Dashboard" and sees a heat map of errors for "CSD201 - Week 3".
    *   **Action:** Admin opens the Quest Step Editor, corrects the JSON content, and marks the feedback as "Resolved".
    *   **System Process:** All affected users receive a notification that the issue has been fixed.