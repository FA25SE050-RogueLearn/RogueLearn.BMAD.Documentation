# 2. User Requirements

## 2.1 System Actors
> An actor is a person (or sometimes another software system or a hardware device) that interacts with the system to perform a use case. Questions to identify actors: Who is notified when something occurs? Who provides information/services to the system? Who helps the system respond/complete a task?

Description of system actors:

| # | Actor | Description |
|---|-------|-------------|
| 1 | Player (Student) | Primary user who onboards, views skill tree, completes quests, attempts boss fights, and manages personal notes; may become Party Leader or Guild Master based on actions. |
| 2 | Party Leader | Player with leadership role over a Party; invites members, manages roles, schedules activities, and can start group Boss Fights. |
| 3 | Guild Master | Owner/admin of a Guild; manages membership, roles, guild page, events, and moderation. |
| 4 | Verified Lecturer (Special Status) | Instructor verified by Game Master; can create and manage Guilds and official events, review student progress, and provide feedback. |
| 5 | Game Master (Admin) | Platform administrator; owns Elective Library and University Curriculum; configures system, verifies lecturers, and moderates community/leaderboards. |
| 6 | AI Engine (System) | Generates quests and recommendations, performs curriculum-roadmap gap analysis, calculates stats and skill mapping. |
| 7 | FPTU Portal/API (External) | Supplies academic calendar, course data, and student verification endpoints. |
| 8 | roadmap.sh API (External) | Provides specialization roadmaps used for supplementary quest generation. |
| 9 | GitHub OAuth/API (External) | Enables linking of projects and repositories to Arsenal notes and skill evidence. |

## 2.2 Use Cases
> A use case describes a sequence of interactions between the system and an external actor that results in an outcome of value. Names should be verb + object and clearly indicate the value delivered.

| ID | Use Case | Actors | Use Case Description |
|----|----------|--------|----------------------|
| 01 | Onboard Player | Player (Student) | Complete 3-step character creation (Curriculum → Specialization → Roadmap) and initialize profile/stat baseline. |
| 02 | Upload Academic Documents | Player (Student) | Upload transcripts, schedules, and IDs to influence stats, unlock quests, and enable FPTU verification. |
| 03 | View Skill Tree | Player (Student) | Explore interconnected skills, see prerequisites/dependencies, and track progress and contributions from notes. |
| 04 | Generate Quest Line | Player (Student), AI Engine | Produce semester-structured quests from syllabus and calendar; adapt based on progress and preferences. |
| 05 | Attempt Boss Fight | Player (Student) | Take gamified mock exams (WebGL), earn points, and convert results to skill XP. |
| 06 | View Leaderboard | Player (Student) | Browse rankings by class/major/guild and inspect scoring breakdowns. |
| 07 | Manage University Curriculum | Game Master (Admin) | Maintain predefined curriculum and syllabus data for supported programs and subjects. |
| 08 | Review Student Progress | Verified Lecturer | Inspect student dashboards and provide feedback/suggestions to improve outcomes. |
| 09 | Create Party | Player (Student) | Create a Party, invite members, and set initial roles. |
| 10 | Manage Party | Party Leader | Manage membership, roles, schedules, party stash, and start party Boss Fights. |
| 11 | Create Guild | Player (Student), Verified Lecturer | Create a Guild and set up governance; Verified Lecturers can create official/endorsed Guilds. |
| 12 | Manage Guild | Guild Master | Manage membership, roles, guild page, events, and moderation. |
| 13 | Create Guild Event | Verified Lecturer | Create and publish official Guild/Community events with attendance and prerequisites. |
| 14 | Moderate Community | Guild Master, Game Master (Admin) | Review/flag content, ban or mute members, and enforce community rules. |
| 15 | Verify Lecturer | Game Master (Admin) | Review verification submissions and grant Verified Lecturer status. |
| 16 | Manage Elective Library | Game Master (Admin) | Curate and publish elective skill paths, courses, and resources. |
| 17 | Link Project Evidence | Player (Student), GitHub OAuth/API | Attach repositories and project artifacts to Arsenal notes and skill nodes. |
| 18 | Start Group Boss Fight | Party Leader, Player (Student) | Initiate and coordinate a group Boss Fight session; track outcomes and reward distribution. |