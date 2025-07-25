# **Requirements**

### **Functional - Phase 1 (MVP)**

1. **FR1:** The system must allow a new user to create an account and onboard via a "Character Creation" flow. This includes selecting a "Class" (career goal, e.g., Full-Stack Developer) and a "Route" (current academic path, e.g., Software Engineering).
2. **FR1.1:** During character creation or from their profile, users must be able to upload academic documents such as their GPA, transcripts, timetable, and examination schedule.
2. **FR2:** The system must be able to ingest user-uploaded documents (e.g., PDF, DOCX) representing a course syllabus.
3. **FR3:** The system's AI must parse the uploaded syllabus and, based on the user's selected "Class" and "Route," generate a personalized "main quest line" that connects their current studies to their career goal.
4. **FR3.1:** The system's AI must use the uploaded academic data (GPA, transcripts, schedules) to influence the character's initial stats, populate the skill tree with acquired knowledge, and automatically create tasks or events in the quest line based on schedules.
5. **FR4:** Users must have a personal dashboard ("Character Sheet") that displays their progress, stats (influenced by academic data), a dynamic and interconnected skill tree, and the generated quest line.
6. **FR4.1:** The skill tree must visually represent the knowledge a user has acquired, showing the relationships between different skills and how they contribute to the user's final goal. It should also highlight missing skills needed to reach that goal.
5. **FR5:** The user's personal study notes ("Arsenal") must be a rich text editor with Notion-like functionalities, allowing for flexible content creation and organization.
6. **FR5.1:** The skill tree must be able to display links to relevant notes within the user's "Arsenal" for each skill node.
7. **FR6:** The system must be able to generate gamified mock exams ("Boss Fights") based on the topics in the quest line and the user's notes. These events should include descriptions of the challenge and the specific skills (knowledge) required to succeed.
8. **FR6.1:** The system must provide a browser extension to scan and extract university-related information (timetables, exams, curriculum) and web content (videos, articles, etc.).
9. **FR6.2:** The browser extension must automatically organize extracted information and web content into the user's "Arsenal" (notes), categorizing it appropriately.
10. **FR6.3:** When a user highlights text on a webpage, the extension must trigger a popup that displays relevant notes from their "Arsenal" based on the highlighted keywords.
11. **FR7:** A user must be able to create a "Party" (study group) with configurable rules and permissions.
12. **FR7.1:** When creating a party, a user must have the option to:
    - Invite friends directly.
    - Browse a list of other users (strangers), view their relevant stats, and send invitations.
    - Set the party to be open for anyone to join.
8. **FR8:** A Party Leader must be able to invite other registered users to their Party.
9. **FR9:** Party members must have access to a shared resource space ("Party Stash") to post notes and links.
13. **FR10:** The system must feature leaderboards that display player rankings, both within specific "Classes" and overall.
14. **FR11:** The system must allow for the creation of "Guilds" where lecturers can upload their own course materials.
15. **FR11.1:** Students in a Guild must be able to use lecturer-uploaded materials to adjust or supplement their personal quest lines.
16. **FR12:** The main quest line must be dynamic, allowing for AI-driven changes to uncompleted tasks based on user preferences or new information.
17. **FR12.1:** The system must have a notification system to inform users of changes to their quest line and suggest new learning paths.

### **Functional - Phase 2 (Educator's Toolkit)**

1. **FR10 (Lecturer):** A Lecturer must have a "Guild Master" dashboard to view aggregated, anonymized progress for all students in their course ("Guild").
2. **FR11 (Lecturer):** The dashboard must highlight topics or 'boss fights' where a significant percentage of the class is struggling.
3. **FR12 (Lecturer):** A Lecturer must be able to upload reference materials and use an AI assistant to generate draft 'side quests'.
4. **FR13 (Lecturer):** A Lecturer must be able to review, edit, and assign these AI-generated quests to their entire course.
5. **FR14 (Tutor):** A Tutor must be able to be assigned to a specific student for a specific course by a Lecturer.
6. **FR15 (Tutor):** Upon assignment, a Tutor must have read-only access to the assigned student's progress and notes for that course only.
7. **FR16 (Tutor):** A Tutor must be able to create and assign custom, non-graded 'training drills' to their assigned student.
8. **FR17 (System Admin):** A System Admin must have access to a non-academic back-end interface to monitor application health.

### **Functional - Phase 3 (Living Ecosystem)**

1. **FR18 (AI):** The system's AI must be able to proactively scan a student's 'Arsenal' and suggest the creation of 'spells' (study aids).
2. **FR19 (AI):** The system must provide a feature to ingest audio/video recordings of online classes and generate text summaries.
3. **FR20 (AI):** The system must have a predictive analytics module that can identify students who are at risk of falling behind.
4. **FR21 (Social):** The system must feature a "Guild-vs-Guild" competition mode.
5. **FR22 (Social):** The system must include a personal achievements/badges system.
6. **FR23 (Marketplace):** The system must include a "Marketplace" where users can upload and share high-quality study materials.
7. **FR24 (Marketplace):** The Marketplace must include a rating and review system for shared content.

### **Non Functional (All Phases)**

1. **NFR1:** The application must be a responsive web application.
2. **NFR2:** The system must use Clerk for user authentication.
3. **NFR3:** The system must use .NET for all backend services.
4. **NFR4:** The system must use Next.js for the frontend application.
5. **NFR5:** The system must use Supabase for database and file storage.
6. **NFR6:** The system must integrate with Stripe for future subscription management.
7. **NFR7:** Core API endpoints must respond in under 500ms under normal load.
8. **NFR8:** The web application's core pages must achieve a Google Lighthouse performance score of 85 or higher.
9. **NFR9:** The system shall be designed to maintain at least 99.5% uptime.
10. **NFR10:** All user data stored in Supabase must have a point-in-time recovery (PITR) backup plan.
11. **NFR11:** The application must implement basic security best practices (OWASP Top 10).
12. **NFR12:** The system must be designed with data privacy as a priority and include a configurable data retention policy.
13. **NFR13 (Security):** The system must implement a robust Role-Based Access Control (RBAC) system.
14. **NFR14 (Scalability):** The architecture must support scalable and cost-effective infrastructure for intensive AI/ML workloads.