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
> Canonical use cases sourced from docs/detailed-use-cases.md.

| ID | Use Case | Actors | Use Case Description | Source PRD |
|----|----------|--------|----------------------|------------|
| UC-001 | User Registration and Curriculum-Career Onboarding | Player (Student) | Register and complete the 3-step Character Creation flow (Curriculum → Career Path → Skill-based roadmap). | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| UC-002 | Academic Document Enhancement and Skill Tree Integration | Player (Student) | Upload academic documents to enhance skill tree visualization, Arsenal personalization, and enable FPTU verification. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data), [Dashboard, Skill Tree & Arsenal](../prd/requirements.md#dashboard-skill-tree--arsenal) |
| UC-003 | Curriculum-Based Skill Tree Visualization and Career Navigation | Player (Student) | View interactive skill tree mapped to curriculum; explore prerequisites, dependencies, and career navigation. | [Dashboard, Skill Tree & Arsenal](../prd/requirements.md#dashboard-skill-tree--arsenal) |
| UC-004 | AI-Powered Curriculum Quest Generation and Career Enhancement | Player (Student) | Generate primary quest line from curriculum and supplementary gap quests aligned to career path. | [AI Curriculum & Career Alignment](../prd/requirements.md#ai-curriculum--career-alignment), [Dynamic Quest & Notifications](../prd/requirements.md#dynamic-quest--notifications) |
| UC-005 | Curriculum-Based Party Creation and Career Collaboration | Player (Student) | Create a party, invite members or open for join; configure study/collaboration rules. | [Party & Collaboration Features](../prd/requirements.md#party--collaboration-features) |
| UC-006 | Meeting Scheduling and Management | Party Members | Schedule and manage study meetings; record content and generate summaries. | [Meetings & Summaries](../prd/requirements.md#meetings--summaries) |
| UC-007 | Browser Extension Integration | Player (Student) | Extract academic info/web content; organize into Arsenal; provide context-aware suggestions linked to quests. | [Browser Extension Integration (FPTU & Arsenal)](../prd/requirements.md#browser-extension-integration-fptu--arsenal) |
| UC-008 | Guild Management System | Guild Master, Verified Lecturer | Create and manage guilds; share materials; basic analytics and announcements; educational governance retained by Admin. | [Basic Guild Management (Verified Lecturer Gated)](../prd/epic-list.md#basic-guild-management-verified-lecturer-gated) |
| UC-009 | Curriculum-Career Boss Fight Assessment System | Player (Student) | Attempt boss fights (WebGL), receive readiness assessments, and convert scores to skill XP. | [Boss Fights & Leaderboards](../prd/requirements.md#boss-fights--leaderboards) |
| UC-010 | Collaborative Study Sessions | Party Members | Conduct collaborative study sessions with shared objectives and resource sharing. | [Party & Collaboration Features](../prd/requirements.md#party--collaboration-features) |
| UC-011 | Event Management with Game Master Approval | Guild Master, Game Master (Admin) | Create and manage competitive events via wizard; submit for admin approval when required. | [Event Platform (Code Arena & Guild Events)](../prd/requirements.md#event-platform-code-arena--guild-events) |
| UC-012 | Code Battle Participation | Player (Student) | Participate in real-time code battles; submit solutions with automated scoring and live rankings. | [Event Platform (Code Arena & Guild Events)](../prd/requirements.md#event-platform-code-arena--guild-events) |
| UC-013 | Real-time Notifications and Updates | Player (Student) | Receive in-app/email/push notifications with configurable preferences for quest and system updates. | [Dynamic Quest & Notifications](../prd/requirements.md#dynamic-quest--notifications), [Notifications](../prd/requirements.md#notifications) |
| UC-014 | Performance Analytics and Monitoring | Verified Lecturer, Game Master (Admin) | Visualize performance analytics and monitor progress across students/classes. | [Non-Functional Requirements (All Phases)](../prd/requirements.md#non-functional-requirements-all-phases), [Cross-Cutting Requirements](../prd/requirements.md#cross-cutting-requirements-all-phases) |
| UC-015 | System Integration and API Management | Game Master (Admin), System | Manage system integrations, APIs, logging, and synchronization. | [Cross-Cutting Requirements (All Phases)](../prd/requirements.md#cross-cutting-requirements-all-phases) |
| UC-016 | Elective Library Curation & Approval (Admin-Owned) | Game Master (Admin) | Curate, review, approve, and publish elective content aligned to skill nodes. | [Admin & Content Curation](../prd/requirements.md#admin--content-curation) |
| UC-017 | University Curriculum Import & Administration (Admin-Owned) | Game Master (Admin) | Import curriculum via JSON (MVP), manage versions/activations, and reporting exports. | [Academic Integration (FPTU, Quest Memory & Recovery)](../prd/requirements.md#academic-integration-fptu-quest-memory--recovery) |