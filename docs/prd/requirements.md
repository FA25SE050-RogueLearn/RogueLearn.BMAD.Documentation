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
*Focus: Empower educators and administrators with tools.*

35. **FR35 (Guild Master):** Any user (Student or Verified Lecturer) can create a "Guild."
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
46. **FR46 (Game Master):** A Game Master (System Admin) must have access to a non-academic back-end interface to monitor application health.
47. **FR47 (Game Master):** A Game Master must be able to trigger "Global Events", such as bonus XP days, randomized mini-boss challenges, or knowledge-based invasions that affect all players or Guilds.
48. **FR48 (Game Master):** A Game Master must be able to schedule and configure "Event Scripts" — timed platform-wide challenges or coordinated PvP events with specific participation conditions.
49. **FR49 (Game Master):** A Game Master must be able to moderate "PvP Disputes" and manage community-reported abuse, including banning or muting users, flagging inappropriate content, or rolling back unfair scores.
50. **FR50 (Game Master):** A Game Master must have access to a comprehensive analytics dashboard to monitor system health and user engagement.
51. **FR51 (Game Master):** The analytics dashboard must track the popularity of quests, marketplace items, and other features.
52. **FR52 (Game Master):** The analytics dashboard must provide user cohort analysis, viewing data on class/route choices, completion rates, and other user segments.
53. **FR53 (Game Master):** The analytics dashboard must monitor key performance indicators for the overall system.
54. **FR54 (Game Master):** The Game Master must be able to publish "Seasonal Lore Updates" that expand the world map, unlock new skill paths, introduce new factions or threats, and set the stage for the upcoming semester.

### **Functional - Phase 4: Living Ecosystem & Social**
*Focus: Enhance the platform with advanced AI, social, and competitive features.*

55. **FR55 (Player):** Players must be able to create and join "Study Parties" — temporary groups for collaborative learning sessions with shared whiteboards, voice chat, and synchronized quest progress.
56. **FR56 (Player):** Players must be able to participate in "Knowledge Duels" — real-time, competitive quizzes or problem-solving challenges against other players, with XP and cosmetic rewards.
57. **FR57 (Player):** Players must be able to engage in "Peer Teaching" sessions where they can create and share mini-lessons or tutorials, earning "Mentor XP" and unlocking teaching-related achievements.
58. **FR58 (Player):** Players must be able to access a "Global Leaderboard" that ranks players by various metrics (XP, quest completion, peer teaching contributions, etc.) with privacy controls.
59. **FR59 (Player):** Players must be able to participate in "Seasonal Events" — limited-time challenges or themed learning experiences that offer exclusive rewards and foster community engagement.
60. **FR60 (Player):** Players must be able to join "Learning Circles" — persistent, topic-focused communities where players can discuss concepts, share resources, and collaborate on projects.
61. **FR61 (Player):** Players must be able to create and customize their "Learning Space" — a virtual environment where they can display achievements, organize resources, and invite others for study sessions.
62. **FR62 (AI):** The system's AI must be able to proactively scan a Player's 'Arsenal' and suggest the creation of 'spells' (study aids).
63. **FR63 (AI):** The system must provide a feature to ingest audio/video recordings of online classes and generate text summaries.
64. **FR64 (AI):** The system must have a predictive analytics module that can identify Players who are at risk of falling behind.
65. **FR65 (Guild Master):** The system must feature a "Guild-vs-Guild" competition mode, which can be initiated by the Guild Master.
66. **FR66 (System):** The system must include a personal achievements/badges system.
67. **FR67 (Player):** PvP events can be based on coding challenges related to the user's quest line (e.g., system design, CSS battles, algorithm battles, design patterns).

### **Functional - Phase 5: Marketplace & Economy**
*Focus: Introduce a user-driven economy for sharing and monetizing knowledge.*

68. **FR68 (Player):** The system must include a "Marketplace" where users can upload and share high-quality study materials.
69. **FR69 (Player):** The Marketplace must include a rating and review system for shared content.
70. **FR70 (System):** The marketplace must use an in-game currency that players can acquire through various in-app activities like daily tasks and boss events.
71. **FR71 (Player):** Players must be able to trade items (e.g., notes, documents) with other players using the in-game currency. The platform will also support transactions with real money, and sellers can determine the value of their products or negotiate prices.
72. **FR72 (AI):** The system's AI must be able to review, rate, and elevate user-generated notes or study materials to the shared "Eternal Codex," making them publicly searchable and globally accessible.
73. **FR73 (AI):** The system's AI must be able to curate "Knowledge Packs" (bundles of notes and resources) for specific themes, subjects, or exams, and publish them in the Marketplace.
74. **FR74 (AI):** The system's AI must be able to tag materials with "meta-skills" (e.g., critical thinking, synthesis, memorization) to better inform AI search and Arsenal suggestions.