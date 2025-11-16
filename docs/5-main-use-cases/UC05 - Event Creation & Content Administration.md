### **UC-05: System & Content Administration**

This use case covers the essential backend tasks performed by an Admin to manage the platform's academic content.

**Actor:** System Admin

---

## Transactions

1.  **Admin Accesses the Content Management Dashboard**
    *   The Admin logs into the Admin Panel and navigates to "Content Management".
    *   They see an overview of all curricula, subjects, and their content status.

2.  **Admin Imports a New Curriculum**
    *   The university releases a new curriculum ("K20 Software Engineering"). The Admin receives the raw text file.
    *   They upload the file. The `ImportCurriculumCommandHandler` runs, using an AI plugin to parse the text, create new `subjects` records (with null content), and link them to a new "K20" `curriculum_program`.

3.  **Admin Imports a Detailed Syllabus for a Subject**
    *   The Admin receives an updated syllabus for the "PRJ301" subject.
    *   They find "PRJ301" in the subject list and upload the raw HTML of the new syllabus.
    *   The `ImportSubjectFromTextCommandHandler` runs, using an AI plugin to parse the detailed content (learning outcomes, session topics) and updates the `content` field for the "PRJ301" `subjects` record.

4.  **Admin Curates the Official Skill Mappings**
    *   The Admin navigates to the "Skill Mappings" page for the "PRJ301" subject.
    *   Based on the new syllabus, they link a new official skill, "Advanced Agile Metrics", to the subject. This is saved in the `subject_skill_mappings` table.

5.  **Admin Creates a New Master Skill**
    *   While curating, the Admin decides a new core skill is needed for the whole platform.
    *   They go to the master "Skills" catalog and create a new skill named "Cloud Cost Management" with a description and tier level.

6.  **Admin Manages Game Content Mappings**
    *   The system flags that the "Agile Quest" is mapped to an older version of the PRJ301 syllabus.
    *   The Admin reviews a comparison of the old vs. new syllabus content.

7.  **Admin Triggers AI-Powered Content Generation**
    *   The Admin clicks "Generate Content Updates with AI".
    *   They configure the AI to create new chapters for the new topics ("DevOps Integration") and modify existing chapters.
    *   The AI generates a new JSON structure for the game with updated chapters, challenges, and quizzes. The Admin reviews and approves the content.