### **UC-01: Personalized Learning Journey & Collaborative  Gaming Challenge**

This use case combines your core value proposition: using a student's real academic data to create a personalized game, which then flows into a collaborative challenge to reinforce learning.

**Actor:** Player (Student)

---

## Transactions

1.  **Onboard and Sync Academic Record**
    *   Student A logs into RogueLearn and navigates to the onboarding or dashboard page.
    *   They are prompted to connect their academic profile. Student A clicks "Connect to FAP".
    *   The system instructs the user on how to copy the full HTML of their academic record page.
    *   Student A pastes the raw HTML into a text area and clicks "Sync My Record".

2.  **System Analyzes Transcript and Generates Path**
    *   The backend receives the HTML, cleans it, and uses an AI plugin to extract all subjects, grades, and statuses.
    *   The system creates the `student_semester_subjects` records, forming a definitive "Student Gradebook".
    *   Based on the gradebook, the system generates the user's high-level Learning Path, creating shell `quests` for every subject in their curriculum.

3.  **Student Views Personalized Dashboard**
    *   The dashboard displays Student A's calculated GPA and a visual overview of their progress.
    *   The system highlights subjects with a "Not Passed" status (e.g., PRJ301).
    *   An AI-powered recommendation appears: **"You struggled with PRJ301. Start the 'Agile Quest' to master the fundamentals."**
    *   Student A clicks "Start Agile Quest".

4.  **Student Plays Solo Quest Chapter**
    *   The system loads the "Agile Quest" game, showing the chapter list.
    *   Student A clicks "Start Chapter 1: Scrum Fundamentals".
    *   The backend triggers the `GenerateQuestStepsCommandHandler` on-demand, creating detailed, interactive steps for Chapter 1.
    *   Student A engages with the content, answering quiz questions and completing reading tasks.

5.  **Student Completes Chapter and Receives XP**
    *   Upon completing the final step, the system awards XP (+150) and updates the user's skill progression for "Scrum" and "Project Planning".
    *   The user's progress for the PRJ301 quest is updated to 10% complete.

6.  **System Recommends a Collaborative Boss Fight**
    *   A new recommendation appears: **"You've mastered the basics. Challenge the 'PRJ301 Final Exam Simulator' boss fight with a friend to prepare for the final exam!"**
    *   Student A clicks "Find Teammate".

7.  **Student A Invites Student B to the Boss Fight**
    *   The system shows a lobby screen. Student A invites Student B, who is also in their guild.
    *   Student B receives and accepts the invitation. Both players are placed in the boss fight room and ready up.

8.  **Players Collaborate to Defeat the Boss**
    *   The boss fight begins, consisting of multiple rounds: a synchronized quiz, a collaborative sprint planning problem, and a rapid-fire scenario challenge.
    *   The players use the built-in chat to coordinate their answers.

9.  **Boss Fight Completion and Rewards**
    *   The team successfully completes the challenges and defeats the boss.
    *   The system displays a victory screen and distributes rewards: both students earn XP (+450), level up, and unlock the "Dragon Slayer" achievement.

10. **System Updates All Profiles and Suggests Next Steps**
    *   The `student_semester_subjects` record for PRJ301 is updated for both students, showing improved mastery scores.
    *   The dashboard now shows new recommendations, such as **"Create a Study Party to continue collaborating"** or **"Join the 'FCode - Java' Guild to find more teammates."**
