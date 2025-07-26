# **Requirements**

### **Functional - Phase 1 (MVP - Student Experience)**

1.  **FR1 (User):** The system must allow a new user to create an account and onboard via a "Character Creation" flow. This includes selecting a "Class" (career goal, e.g., Full-Stack Developer) and a "Route" (current academic path, e.g., Software Engineering).
2.  **FR2 (User):** During character creation or from their profile, users must be able to manually upload academic documents such as their GPA, transcripts, timetable, and examination schedule.
3.  **FR3 (System):** The system must be able to ingest user-uploaded documents (e.g., PDF, DOCX) representing a course syllabus.
4.  **FR4 (AI):** The system's AI must parse the uploaded syllabus and, based on the user's selected "Class" and "Route," generate a personalized "main quest line" that connects their current studies to their career goal.
5.  **FR5 (AI):** The system's AI must use the uploaded academic data (GPA, transcripts, schedules) to influence the character's initial stats, populate the skill tree with acquired knowledge, and automatically create tasks or events in the quest line based on schedules.
6.  **FR6 (User):** Users must have a personal dashboard ("Character Sheet") that displays their progress, stats (influenced by academic data), a dynamic and interconnected skill tree, and the generated quest line.
7.  **FR7 (System):** The skill tree must visually represent the knowledge a user has acquired, showing the relationships between different skills and how they contribute to the user's final goal. It should also highlight missing skills needed to reach that goal.
8.  **FR8 (User):** The user's personal study notes ("Arsenal") must be a rich text editor with Notion-like functionalities, allowing for flexible content creation and organization.
9.  **FR9 (System):** The skill tree must be able to display links to relevant notes within the user's "Arsenal" for each skill node.
10. **FR10 (System):** The system must be able to generate gamified mock exams ("Boss Fights") based on the topics in the quest line and the user's notes. These events should include descriptions of the challenge and the specific skills (knowledge) required to succeed.
11. **FR11 (Browser Extension):** The system must provide a browser extension to scan and extract academic documents and information (e.g., syllabus, GPA, timetables, exams, curriculum) from university portals and other web pages.
12. **FR12 (Browser Extension):** The browser extension must automatically organize extracted information and web content into the user's "Arsenal" (notes), categorizing it appropriately.
13. **FR13 (Browser Extension):** When a user highlights text on a webpage, the extension must trigger a popup that displays relevant notes from their "Arsenal" based on the highlighted keywords.
14. **FR14 (User):** A user must be able to create a "Party" (study group) with configurable rules and permissions.
15. **FR15 (User):** When creating a party, a user must have the option to:
    - Invite friends directly.
    - Browse a list of other users (strangers), view their relevant stats, and send invitations.
    - Set the party to be open for anyone to join.
16. **FR16 (User):** A Party Leader must be able to invite other registered users to their Party.
17. **FR17 (User):** Party members must have access to a shared resource space ("Party Stash") to post notes and links.
18. **FR18 (System):** The system must feature leaderboards that display player rankings, both within specific "Classes", for PvP events, and overall.
19. **FR19 (AI):** The main quest line must be dynamic, allowing for AI-driven changes to uncompleted tasks based on user preferences or new information.
20. **FR20 (System):** The system must have a notification system to inform users of changes to their quest line and suggest new learning paths.

### **Functional - Phase 2 (Educator & Admin Toolkit)**

21. **FR21 (Guild Master):** Guild Masters (Lecturers or Players) must be able to create "Guilds" and upload their own course materials.
22. **FR22 (Player):** Players (Students) in a Guild must be able to use Guild Master-uploaded materials to adjust or supplement their personal quest lines.
23. **FR23 (Guild Master):** A Guild Master must have a dashboard to view aggregated, anonymized progress for all Players in their course ("Guild").
24. **FR24 (Guild Master):** The dashboard must highlight topics or 'boss fights' where a significant percentage of the class is struggling.
25. **FR25 (Guild Master):** A Guild Master must be able to upload reference materials and use an AI assistant to generate draft 'side quests'.
26. **FR26 (Guild Master):** A Guild Master must be able to review, edit, and assign these AI-generated quests to their entire course.
27. **FR27 (Guild Master):** A Guild Master must be able to design and publish custom skill tree overlays for their course, which players can view alongside their personal tree to compare progress and priorities.
28. **FR28 (Guild Master):** A Guild Master must be able to create custom "Boss Fights", specifying the required knowledge nodes, a challenge description, optional rewards, and deadlines.
29. **FR29 (Guild Master):** A Guild Master must be able to assign "Lore Fragments" to individual course topics. These fragments are short narrative descriptions that thematically connect academic concepts to the game world.
30. **FR30 (Guild Master):** Guild Masters can merge course modules or knowledge nodes to generate "hybrid quests" or composite topics using a "Curriculum Alchemy" tool.
31. **FR31 (Guild Master):** A Guild Master can define and activate "Guild Buffs", which grant temporary stat boosts or rewards for students if collective achievements are met (e.g., 90% quiz completion).
32. **FR32 (Guide):** A Guide (Tutor) must be able to be assigned to a specific Player for a specific course by a Guild Master.
33. **FR33 (Guide):** Upon assignment, a Guide (as a Party Leader) must have read-only access to the assigned Player's progress and notes for that course only.
34. **FR34 (Guide):** A Guide must be able to create and assign custom, non-graded 'training drills' to their assigned Player.
35. **FR35 (Guide):** A Guide must be able to send "Mentor Reflections", which are motivational or advisory notes (text, voice, or video) tied to specific parts of the student’s quest line or skill tree.
36. **FR36 (Guide):** A Guide must be able to create and assign "Training Grounds" — short, focused drills targeting a single micro-skill (e.g., recursion, essay structure, grammar rules).
37. **FR37 (Guide):** A Guide must be able to view "Burnout Forecasts" for their assigned students, which predict disengagement based on quest abandonment, repeated failures, or low Arsenal activity.
38. **FR38 (Guide):** A Guide must have read-only access to a student's "Arsenal Loadout" and may leave inline feedback or suggest links between notes and missing skill nodes.
39. **FR39 (Guide):** Guides must be able to choose a "Mentor Archetype" (e.g., Sage, Trickster, Warrior), which influences the tone of their default advice and unlocks thematic dialogue or avatar cosmetics.
40. **FR40 (Game Master):** A Game Master (System Admin) must have access to a non-academic back-end interface to monitor application health.
41. **FR41 (Game Master):** A Game Master must be able to trigger "Global Events", such as bonus XP days, randomized mini-boss challenges, or knowledge-based invasions that affect all players or Guilds.
42. **FR42 (Game Master):** A Game Master must be able to schedule and configure "Event Scripts" — timed platform-wide challenges or coordinated PvP events with specific participation conditions.
43. **FR43 (Game Master):** A Game Master must be able to moderate "PvP Disputes" and manage community-reported abuse, including banning or muting users, flagging inappropriate content, or rolling back unfair scores.
44. **FR44 (Game Master):** The Game Master must be able to publish "Seasonal Lore Updates" that expand the world map, unlock new skill paths, introduce new factions or threats, and set the stage for the upcoming semester.

### **Functional - Phase 3 (Living Ecosystem & Social)**

45. **FR45 (AI):** The system's AI must be able to proactively scan a Player's 'Arsenal' and suggest the creation of 'spells' (study aids).
46. **FR46 (AI):** The system must provide a feature to ingest audio/video recordings of online classes and generate text summaries.
47. **FR47 (AI):** The system must have a predictive analytics module that can identify Players who are at risk of falling behind.
48. **FR48 (Guild Master):** The system must feature a "Guild-vs-Guild" competition mode, which can be initiated by the Guild Master.
49. **FR49 (System):** The system must include a personal achievements/badges system.
50. **FR50 (Player):** PvP events can be based on coding challenges related to the user's quest line (e.g., system design, CSS battles, algorithm battles, design patterns).

### **Functional - Phase 4 (Marketplace & Economy)**

51. **FR51 (Player):** The system must include a "Marketplace" where users can upload and share high-quality study materials.
52. **FR52 (Player):** The Marketplace must include a rating and review system for shared content.
53. **FR53 (System):** The marketplace must use an in-game currency that players can acquire through various in-app activities like daily tasks and boss events.
54. **FR54 (Player):** Players must be able to trade items (e.g., notes, documents) with other players using the in-game currency. The platform will also support transactions with real money, and sellers can determine the value of their products or negotiate prices.
55. **FR55 (AI):** The system's AI must be able to review, rate, and elevate user-generated notes or study materials to the shared “Eternal Codex,” making them publicly searchable and globally accessible.
56. **FR56 (AI):** The system's AI must be able to curate "Knowledge Packs" (bundles of notes and resources) for specific themes, subjects, or exams, and publish them in the Marketplace.
57. **FR57 (AI):** The system's AI must be able to tag materials with "meta-skills" (e.g., critical thinking, synthesis, memorization) to better inform AI search and Arsenal suggestions.

### **Success Metrics (KPIs)**

To measure the success of the QuestLearn MVP, we will track the following Key Performance Indicators (KPIs):

*   **Weekly Active Users (WAU)**: The number of unique users who interact with the platform in a given week. This is a primary indicator of user engagement and retention.
*   **Average Number of Quests Completed per User per Week**: This metric will help us understand how effectively users are engaging with the core functionality of the platform.
*   **Party Creation Rate**: The number of new parties created per week. This will indicate the adoption of the social learning feature.
*   **User Retention Rate**: The percentage of users who return to the platform week after week. This is a critical measure of the platform's value and stickiness.
*   **User Feedback Score**: We will collect user feedback through surveys and other mechanisms to gauge user satisfaction and identify areas for improvement. A Net Promoter Score (NPS) or a similar metric will be used.

### **MVP Validation Approach**

To validate the QuestLearn MVP, we will use a combination of qualitative and quantitative methods:

*   **User Interviews**: We will conduct in-depth interviews with a small group of target users to gather feedback on the platform's usability, features, and overall value proposition.
*   **Surveys**: We will use surveys to collect quantitative data on user satisfaction, feature usage, and other key metrics.
*   **A/B Testing**: We will use A/B testing to compare different versions of the platform and identify the most effective design and features.
*   **Analytics**: We will use analytics tools to track user behavior and identify patterns and trends. This will help us understand how users are interacting with the platform and where we can make improvements.

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