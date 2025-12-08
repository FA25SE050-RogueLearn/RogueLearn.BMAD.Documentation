### **UC-04: Study Party Collaboration**

This use case details the creation and management of small, focused study groups for collaborative learning, featuring AI-powered resource organization.

**Actor:** Party Leader (Student A), Party Member (Student B)

---

## Transactions

1.  **Student A Creates a Study Party**
    *   After playing a challenging quest, Student A gets a suggestion: "Create a study party!"
    *   They click the suggestion, name the party "PRJ - HJF - SBA Party", and link the relevant subjects.

2.  **Party Leader Invites Members**
    *   As the Party Leader, Student A invites their friend, Student B, by searching for their username.

3.  **Members Share Materials via Smart Notes**
    *   Student B accepts the invitation and navigates to the party's "Stash" (Resources).
    *   Student B uploads a raw lecture file (e.g., `PRJ301_Architecture_Notes.pdf`) to create a new Note.
    *   **System Action (AI):** The system processes the file text and triggers the **AI Tagging Plugin**.
    *   **System Action (AI):** The AI analyzes the content and suggests relevant tags: `Java Web`, `MVC Pattern`, `DTOs`, `DAO`.
    *   Student B accepts the suggested tags and saves the note. The note is now indexed and searchable in the Party Stash.

4.  **Party Leader Schedules a Recurring Meeting**
    *   Student A navigates to the "Schedule" channel and sets up a recurring weekly study session, including a meeting link and agenda.

5.  **System Sends Automated Reminders**
    *   The system sends calendar invites and push notifications to all members before the scheduled meeting.

6.  **Members Join and Conduct the Study Session**
    *   Members join the meeting link at the scheduled time.
    *   They use RogueLearn's shared digital whiteboard feature to collaboratively work through a problem, referencing the AI-tagged notes from the Stash.

7.  **Party Leader Ends the Session and Submits a Summary**
    *   After the meeting, Student A clicks "End Meeting".
    *   The system prompts for a summary. Student A inputs raw meeting notes.
    *   **System Action (AI):** The system summarizes the meeting notes and auto-tags them (e.g., `Meeting`, `Week 5`, `Review`).
    *   The summary is saved to the party dashboard, updating the group's progress.