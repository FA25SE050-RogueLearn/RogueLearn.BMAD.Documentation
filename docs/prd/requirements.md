# **Requirements**

### **Functional - Phase 1: Core Student MVP**
*Focus: Establish the fundamental single-player experience.*

1.  **FR1 (User):** The system must allow a new user to create an account and onboard via a "Character Creation" flow. This includes selecting a "Class" (career goal, e.g., Full-Stack Developer) and a "Route" (current academic path, e.g., Software Engineering).
2.  **FR2 (User):** During character creation or from their profile, users must be able to manually upload academic documents such as their GPA, transcripts, timetable, and examination schedule.
3.  **FR3 (System):** The system must be able to ingest user-uploaded documents (e.g., PDF, DOCX) representing a course syllabus.
4.  **FR4 (AI):** The system's AI must parse the uploaded curriculum to identify the user's chosen route and detect potential smaller majors within the curriculum (e.g., .NET specialization within Software Engineering).
5.  **FR5 (AI):** The AI must provide suggestions for smaller major choices based on curriculum analysis.
6.  **FR6 (AI):** After user selection of smaller major, the AI must suggest related classes aligned to that route.
7.  **FR7 (AI):** The system's AI must use the uploaded academic data (GPA, transcripts, schedules) to influence the character's initial stats.
8.  **FR8 (AI):** The AI must populate the skill tree with acquired knowledge based on uploaded academic data.
9.  **FR9 (AI):** The AI must automatically create tasks or events in the quest line based on uploaded schedules.
10. **FR10 (User):** Users must have a personal dashboard ("Character Sheet") that displays their progress, stats (influenced by academic data), a dynamic and interconnected skill tree, and the generated quest line.
11. **FR11 (System):** The skill tree must be visualized as an interconnected mind map with nodes representing individual skills.
12. **FR12 (System):** Each skill tree node must display its skill level, contributed notes from the user's Arsenal, and completed/upcoming quests that contribute to that skill.
13. **FR13 (System):** The skill tree visualization must show relationships between skills and highlight missing skills needed to reach the user's goal.
14. **FR14 (User):** The user's personal study notes ("Arsenal") must be a rich text editor with Notion-like functionalities, allowing for flexible content creation and organization.
15. **FR15 (System):** The skill tree must be able to display links to relevant notes within the user's "Arsenal" for each skill node, with visual indicators showing which Arsenal items contribute to each skill's development.
16. **FR16 (System):** The system must generate gamified mock exams ("Boss Fights") as Unity-based 2D interactive experiences with multiple choice questions, visual feedback, and engaging animations based on upcoming tests input by the player.
17. **FR17 (System):** Each Boss Fight question must have an assigned difficulty level, with higher difficulty questions awarding more points and triggering more challenging visual boss mechanics.
18. **FR18 (System):** The total Boss Fight score must be calculated based on correctly answered questions, weighted by their difficulty levels, with real-time score updates and visual progress indicators in the Unity game interface.
19. **FR19 (System):** The system must feature leaderboards that display player rankings, both within specific "Classes", for PvP events, and overall.
20. **FR20 (AI):** The main quest line must be dynamic, allowing for AI-driven changes to uncompleted tasks based on user preferences or new information.
21. **FR21 (System):** The system must have a notification system to inform users of changes to their quest line and suggest new learning paths.

### **Functional - Phase 2: Social & Extension MVP**
*Focus: Introduce multiplayer and external integration features.*

22. **FR22 (Browser Extension):** The system must provide a browser extension to scan and extract academic documents and information (e.g., syllabus, GPA, timetables, exams, curriculum) from university portals and other web pages.
23. **FR23 (Browser Extension):** The browser extension must automatically organize extracted information and web content into the user's "Arsenal" (notes), categorizing it appropriately.
24. **FR24 (Browser Extension):** When a user highlights text on a webpage, the extension must trigger a popup that displays relevant notes from their "Arsenal" based on the highlighted keywords.
25. **FR25 (User):** A user must be able to create a "Party" (study group) with configurable rules and permissions.
26. **FR26 (User):** When creating a party, a user must have the option to invite friends directly.
27. **FR27 (User):** When creating a party, a user must have the option to browse a list of other users (strangers), view their relevant stats, and send invitations.
28. **FR28 (User):** When creating a party, a user must have the option to set the party to be open for anyone to join.
29. **FR29 (User):** A Party Leader must be able to invite other registered users to their Party and assign granular permissions to each member.
30. **FR30 (User):** Party members must have access to a shared resource space ("Party Stash") with role-based permissions: view-only access, edit permissions, or comment-only permissions on shared Arsenal items (notes).
31. **FR31 (User):** The Party Leader must be able to configure permissions for each member individually in the Party Stash.
32. **FR32 (Player):** Players must be able to schedule, organize, and manage study meetings within their party, including setting meeting titles, descriptions, types (Study Session, Project Discussion, Exam Prep, General Meeting), and scheduling details.
33. **FR33 (Player):** The browser extension must be able to capture and record meeting content during party study sessions, including participant discussions, shared resources, and key decisions, with support for multiple recording methods (manual entry, audio transcription, automated capture).
34. **FR34 (AI/Player):** The system must generate comprehensive meeting summaries from recorded content, including executive summaries, key discussion points, action items with assignments, next steps, resources mentioned, study materials covered, and unresolved questions, with options for both AI-generated and manual/collaborative summary creation.

### **Functional - Phase 3: Educator & Admin Toolkit**
*Focus: Empower educators and administrators with tools to manage and enhance the learning experience.*

35. **FR35 (Guild Master):** Guild Masters (Lecturers or Players) must be able to create "Guilds" and upload their own course materials.
36. **FR36 (Player):** Players (Students) in a Guild must be able to use Guild Master-uploaded materials to adjust or supplement their personal quest lines.
37. **FR37 (Guild Master):** A Guild Master must have a dashboard to view aggregated, anonymized progress for all Players in their course ("Guild").
38. **FR38 (Guild Master):** The dashboard must highlight topics or 'boss fights' where a significant percentage of the class is struggling.
39. **FR39 (Guild Master):** A Guild Master must be able to upload reference materials and use an AI assistant to generate draft 'side quests'.
40. **FR40 (Guild Master):** A Guild Master must be able to review, edit, and assign these AI-generated quests to their entire course.
41. **FR41 (Guild Master):** A Guild Master must be able to design and publish custom skill tree overlays for their course, which players can view alongside their personal tree to compare progress and priorities.
42. **FR42 (Guild Master):** A Guild Master must be able to create custom "Boss Fights", specifying the required knowledge nodes, a challenge description, optional rewards, and deadlines.
43. **FR43 (Guild Master):** A Guild Master must be able to assign "Lore Fragments" to individual course topics. These fragments are short narrative descriptions that thematically connect academic concepts to the game world.
44. **FR44 (Guild Master):** Guild Masters can merge course modules or knowledge nodes to generate "hybrid quests" or composite topics using a "Curriculum Alchemy" tool.
45. **FR45 (Guild Master):** A Guild Master can define and activate "Guild Buffs", which grant temporary stat boosts or rewards for students if collective achievements are met (e.g., 90% quiz completion).
46. **FR46 (Guide):** A Guide (Tutor) must be able to be assigned to a specific Player for a specific course by a Guild Master.
47. **FR47 (Guide):** Upon assignment, a Guide (as a Party Leader) must have read-only access to the assigned Player's progress and notes for that course only.
48. **FR48 (Guide):** A Guide must be able to create and assign custom, non-graded 'training drills' to their assigned Player.
49. **FR49 (Guide):** A Guide must be able to send "Mentor Reflections", which are motivational or advisory notes (text, voice, or video) tied to specific parts of the student's quest line or skill tree.
50. **FR50 (Guide):** A Guide must be able to create and assign "Training Grounds" — short, focused drills targeting a single micro-skill (e.g., recursion, essay structure, grammar rules).
51. **FR51 (Guide):** A Guide must be able to view "Burnout Forecasts" for their assigned students, which predict disengagement based on quest abandonment, repeated failures, or low Arsenal activity.
52. **FR52 (Guide):** A Guide must have read-only access to a student's "Arsenal Loadout" and may leave inline feedback or suggest links between notes and missing skill nodes.
53. **FR53 (Guide):** Guides must be able to choose a "Mentor Archetype" (e.g., Sage, Trickster, Warrior), which influences the tone of their default advice and unlocks thematic dialogue or avatar cosmetics.
54. **FR54 (Game Master):** A Game Master (System Admin) must have access to a non-academic back-end interface to monitor application health.
55. **FR55 (Game Master):** A Game Master must be able to trigger "Global Events", such as bonus XP days, randomized mini-boss challenges, or knowledge-based invasions that affect all players or Guilds.
56. **FR56 (Game Master):** A Game Master must be able to schedule and configure "Event Scripts" — timed platform-wide challenges or coordinated PvP events with specific participation conditions.
57. **FR57 (Game Master):** A Game Master must be able to moderate "PvP Disputes" and manage community-reported abuse, including banning or muting users, flagging inappropriate content, or rolling back unfair scores.
58. **FR58 (Game Master):** A Game Master must have access to a comprehensive analytics dashboard to monitor system health and user engagement.
59. **FR59 (Game Master):** The analytics dashboard must track the popularity of quests, marketplace items, and other features.
60. **FR60 (Game Master):** The analytics dashboard must provide user cohort analysis, viewing data on class/route choices, completion rates, and other user segments.
61. **FR61 (Game Master):** The analytics dashboard must monitor key performance indicators for the overall system.
62. **FR62 (Game Master):** The Game Master must be able to publish "Seasonal Lore Updates" that expand the world map, unlock new skill paths, introduce new factions or threats, and set the stage for the upcoming semester.

### **Functional - Phase 4: Living Ecosystem & Social**
*Focus: Enhance the platform with advanced AI, social, and competitive features.*

63. **FR63 (Player):** Players must be able to create and join "Study Parties" — temporary groups for collaborative learning sessions with shared whiteboards, voice chat, and synchronized quest progress.
64. **FR64 (Player):** Players must be able to participate in "Knowledge Duels" — real-time, competitive quizzes or problem-solving challenges against other players, with XP and cosmetic rewards.
65. **FR65 (Player):** Players must be able to engage in "Peer Teaching" sessions where they can create and share mini-lessons or tutorials, earning "Mentor XP" and unlocking teaching-related achievements.
66. **FR66 (Player):** Players must be able to access a "Global Leaderboard" that ranks players by various metrics (XP, quest completion, peer teaching contributions, etc.) with privacy controls.
67. **FR67 (Player):** Players must be able to participate in "Seasonal Events" — limited-time challenges or themed learning experiences that offer exclusive rewards and foster community engagement.
68. **FR68 (Player):** Players must be able to join "Learning Circles" — persistent, topic-focused communities where players can discuss concepts, share resources, and collaborate on projects.
69. **FR69 (Player):** Players must be able to create and customize their "Learning Space" — a virtual environment where they can display achievements, organize resources, and invite others for study sessions.
70. **FR70 (AI):** The system's AI must be able to proactively scan a Player's 'Arsenal' and suggest the creation of 'spells' (study aids).
71. **FR71 (AI):** The system must provide a feature to ingest audio/video recordings of online classes and generate text summaries.
72. **FR72 (AI):** The system must have a predictive analytics module that can identify Players who are at risk of falling behind.
73. **FR73 (Guild Master):** The system must feature a "Guild-vs-Guild" competition mode, which can be initiated by the Guild Master.
74. **FR74 (System):** The system must include a personal achievements/badges system.
75. **FR75 (Player):** PvP events can be based on coding challenges related to the user's quest line (e.g., system design, CSS battles, algorithm battles, design patterns).

### **Functional - Phase 5: Marketplace & Economy**
*Focus: Introduce a user-driven economy for sharing and monetizing knowledge.*

76. **FR76 (Player):** The system must include a "Marketplace" where users can upload and share high-quality study materials.
77. **FR77 (Player):** The Marketplace must include a rating and review system for shared content.
78. **FR78 (System):** The marketplace must use an in-game currency that players can acquire through various in-app activities like daily tasks and boss events.
79. **FR79 (Player):** Players must be able to trade items (e.g., notes, documents) with other players using the in-game currency. The platform will also support transactions with real money, and sellers can determine the value of their products or negotiate prices.
80. **FR80 (AI):** The system's AI must be able to review, rate, and elevate user-generated notes or study materials to the shared "Eternal Codex," making them publicly searchable and globally accessible.
81. **FR81 (AI):** The system's AI must be able to curate "Knowledge Packs" (bundles of notes and resources) for specific themes, subjects, or exams, and publish them in the Marketplace.
82. **FR82 (AI):** The system's AI must be able to tag materials with "meta-skills" (e.g., critical thinking, synthesis, memorization) to better inform AI search and Arsenal suggestions.



### **Success Metrics (KPIs)**

To measure the success of the RogueLearn MVP, we will track the following Key Performance Indicators (KPIs):

*   **Weekly Active Users (WAU)**: The number of unique users who interact with the platform in a given week. This is a primary indicator of user engagement and retention.
*   **Average Number of Quests Completed per User per Week**: This metric will help us understand how effectively users are engaging with the core functionality of the platform.
*   **Party Creation Rate**: The number of new parties created per week. This will indicate the adoption of the social learning feature.
*   **User Retention Rate**: The percentage of users who return to the platform week after week. This is a critical measure of the platform's value and stickiness.
*   **User Feedback Score**: We will collect user feedback through surveys and other mechanisms to gauge user satisfaction and identify areas for improvement. A Net Promoter Score (NPS) or a similar metric will be used.

### **MVP Validation Approach**

To validate the RogueLearn MVP, we will use a combination of qualitative and quantitative methods:

*   **User Interviews**: We will conduct in-depth interviews with a small group of target users to gather feedback on the platform's usability, features, and overall value proposition.
*   **Surveys**: We will use surveys to collect quantitative data on user satisfaction, feature usage, and other key metrics.
*   **A/B Testing**: We will use A/B testing to compare different versions of the platform and identify the most effective design and features.
*   **Analytics**: We will use analytics tools to track user behavior and identify patterns and trends. This will help us understand how users are interacting with the platform and where we can make improvements.

### Non-Functional Requirements (All Phases)

1.  **NFR1 (Responsiveness):** The application must be a responsive web application, providing an optimal viewing and interaction experience across a range of devices (desktops, tablets, and mobile phones). All functionality must be accessible and usable on screen widths from 320px to 1920px.
2.  **NFR2 (Authentication):** The system must use Clerk for user authentication, ensuring secure and reliable user management.
3.  **NFR3 (Backend Technology):** The system must use .NET 8 and Golang for all backend services, leveraging its performance and security features.
4.  **NFR4 (Services Communication):** System's services must use RabbitMQ to communicate.
5.  **NFR5 (Frontend Technology):** The system must use Next.js (version 14 or later) for the frontend application to enable server-side rendering and a fast user experience.
6.  **NFR6 (Data Storage):** The system must use Supabase for database and file storage, configured for scalability and reliability.
7.  **NFR7 (Payment Integration):** The system must integrate with Stripe for future subscription and payment processing, with the initial implementation being a placeholder or a lightweight integration.
8.  **NFR8 (Performance):** Core API endpoints (including user authentication, quest generation, and data retrieval) must respond in under 500ms under a normal load of 100 concurrent users.
9.  **NFR9 (Frontend Performance):** The web application's core pages (Dashboard, Skill Tree, Quest Log) must achieve a Google Lighthouse performance score of 85 or higher on both mobile and desktop.
10. **NFR10 (Availability):** The system shall be designed to maintain at least 99.5% uptime, excluding scheduled maintenance windows. Maintenance windows will be limited to a maximum of 4 hours per month and scheduled outside of peak usage times (9 AM - 9 PM local time).
11. **NFR11 (Data Backup & Recovery):** All user data stored in Supabase must have a point-in-time recovery (PITR) backup plan with a recovery point objective (RPO) of 1 hour and a recovery time objective (RTO) of 4 hours.
12. **NFR12 (Security):** The application must implement security best practices to mitigate risks from the OWASP Top 10, including secure data handling, input validation, and protection against injection attacks.
13. **NFR13 (Data Privacy):** The system must be designed with data privacy as a priority, complying with GDPR principles. It must include a configurable data retention policy allowing for automated data deletion upon user request or after a defined period of inactivity.
14. **NFR14 (Access Control):** The system must implement a robust Role-Based Access Control (RBAC) system to ensure users can only access data and functionality appropriate for their role (e.g., Student, Guild Master, Guide, Game Master).
15. **NFR15 (Scalability):** The architecture must support horizontal scaling for both the backend services and the database to handle a 50% increase in user traffic over a 3-month period without significant performance degradation. AI/ML workloads must be managed in a cost-effective manner, using serverless functions or dedicated instances as appropriate.
