### **UC-03: End-to-End Code Battle Event**

This use case demonstrates the full lifecycle of a competitive Code Battle, from creation and approval to participation and conclusion.

**Actors:** Guild Master (User X), System Admin, Player (Student A)

---

## Transactions

1.  **Guild Master Defines a New Code Battle Event**
    *   User X, the Guild Master, navigates to "Guild Management" and clicks "Create New Event".
    *   They fill out the creation form with the event's title ("Spring Java Championship 2025"), date, rules, and entry fee (e.g., 200k VND).

2.  **Guild Master Configures Competition Structure**
    *   They specify the structure: 5 rooms for 100 total participants, a set of 8 problems with varying difficulty, and the eligible guilds ("FCode-Java", "SE Coders", etc.).
    *   They define the prize pool, allocating rewards for the top finishers.

3.  **Guild Master Submits Event for Approval**
    *   After reviewing a summary, the Guild Master clicks "Submit for Admin Approval".
    *   The system creates a draft event record with a "Pending Approval" status and notifies the admin team.

4.  **Admin Reviews and Approves the Event**
    *   A System Admin logs into the Admin Panel and sees the pending event.
    *   The Admin validates all details, ensuring the budget is sound and there are enough problems in the Problem Bank for the requested topics.
    *   Satisfied, the Admin clicks "Approve and Publish".

5.  **System Publishes the Event**
    *   The system changes the event's status to "Active".
    *   It automatically creates the public event page and sends notifications to all eligible students from the invited guilds.

6.  **Player Registers for the Event**
    *   Student A, a member of an eligible guild, receives the notification and registers for the event, paying the entry fee.

7.  **Player Competes in the Event**
    *   On event day, Student A joins the competition lobby and is assigned to a room.
    *   The event starts, and the system presents coding problems one by one. Student A writes their code in a web IDE and submits their solution.

8.  **System Auto-Grades the Solution**
    *   The backend receives the code and executes it against hidden test cases.
    *   It returns the result instantly ("8/10 Test Cases Passed"), and updates the live leaderboard.

9.  **Competition Ends and Rewards are Distributed**
    *   When the timer expires, the system finalizes all scores and displays the official leaderboard.
    *   Prizes and XP are automatically distributed to the winners and participants based on their final ranking.

10. **Guild Master Views Post-Event Dashboard**
    *   After the event, User X can view a detailed analytics dashboard showing participation rates, total revenue, prize payout, and the average score of their guild members.
