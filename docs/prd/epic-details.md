# **Epic Details**

This document provides detailed user stories for each epic, aligned with the phased rollout plan.

---

## **Phase 1: Core Student MVP**
*Focus: Establish the fundamental single-player experience.*

### **Epic: User Onboarding & Profile Management**
*Goal: Create a seamless and engaging entry point for new users, allowing them to set up their accounts and profiles.*

#### **Story: Project Infrastructure Setup**
**As a** Developer, **I want** to establish the foundational project infrastructure with proper CI/CD pipelines, **so that** we have a reliable development and deployment foundation.

*   **Acceptance Criteria:**
    *   Git repositories are created for frontend (Next.js) and backend (.NET) with proper branching strategy
    *   Basic "Hello World" applications are deployable and accessible via health check endpoints
    *   CI/CD pipeline triggers on commits with automated testing (minimum 80% code coverage)
    *   Environment-specific configuration management is implemented (dev, staging, prod)
    *   Docker containerization is set up for both frontend and backend
    *   Basic monitoring and logging infrastructure is configured

#### **Story: Authentication Service Integration**
**As a** Developer, **I want** to integrate Supabase Authentication with proper error handling, **so that** users can securely access the platform.

*   **Acceptance Criteria:**
    *   The Supabase Auth Helpers SDK (`@supabase/auth-helpers-nextjs`) is properly configured in the Next.js frontend.
    *   Backend API routes are protected by middleware that validates JWTs issued by Supabase.
    *   Authentication state (e.g., session, user) is managed effectively in the frontend using Supabase's provided hooks or context providers.
    *   Session persistence and token refresh are handled automatically by the Supabase SDK.
    *   Supabase's built-in rate limiting for authentication endpoints is enabled and configured.
    *   Proper error messages are displayed to the user for authentication failures (e.g., invalid credentials, email already in use).
    *   A PostgreSQL trigger is created in the Supabase database to synchronize new users from `auth.users` to the public `UserProfiles` table.
    
#### **Story: User Registration Flow**
**As a** prospective student, **I want** to create an account with email verification, **so that** I can access the RogueLearn platform securely.

*   **Acceptance Criteria:**
    *   Registration form validates email format and password strength (8+ chars, mixed case, numbers)
    *   Email verification is required before account activation
    *   Duplicate email registration is prevented with clear error messaging
    *   User agreement and privacy policy acceptance is required
    *   Registration success triggers welcome email with next steps
    *   Failed registration attempts are logged for security monitoring

#### **Story: User Login and Session Management**
**As a** registered user, **I want** to log in securely with session persistence, **so that** I can access my personalized learning dashboard.

*   **Acceptance Criteria:**
    *   Login form supports email/password authentication with "Remember Me" option
    *   Failed login attempts are rate-limited (5 attempts, then 15-minute lockout)
    *   Successful login redirects to appropriate page (dashboard or onboarding)
    *   Session timeout is configured (24 hours with refresh capability)
    *   "Forgot Password" functionality is available with secure reset flow
    *   Login activity is logged for security auditing

#### **Story: Route Protection and Authorization**
**As a** system administrator, **I want** proper route protection implemented, **so that** user data and functionality are secured appropriately.

*   **Acceptance Criteria:**
    *   Protected routes require valid authentication tokens
    *   Unauthenticated access redirects to login with return URL preservation
    *   Role-based access control is implemented for different user types
    *   API endpoints validate user permissions before processing requests
    *   Unauthorized access attempts are logged and monitored
    *   Session expiry handling gracefully prompts for re-authentication

#### **Story: Onboarding Flow Initialization**
**As a** new user, **I want** to be guided through a structured onboarding process, **so that** I can set up my learning profile effectively.

*   **Acceptance Criteria:**
    *   Onboarding status is tracked in user profile (not_started, in_progress, completed)
    *   Multi-step progress indicator shows current step and overall completion
    *   Users can save progress and return to complete onboarding later
    *   Skip options are available for optional steps with clear consequences
    *   Onboarding completion triggers character sheet generation
    *   Analytics track onboarding completion rates and drop-off points

#### **Story: Academic Profile Setup**
**As a** new user, **I want** to define my academic profile with institution and program details, **so that** the system can provide relevant content and connections.

*   **Acceptance Criteria:**
    *   Institution selection from searchable database with "Add New" option
    *   Program/major selection with custom entry capability
    *   Academic year/level selection (Freshman, Sophomore, Junior, Senior, Graduate)
    *   Study preferences configuration (learning style, time availability, goals)
    *   Profile information is validated and saved to user database
    *   Profile completeness score is calculated and displayed

#### **Story: Character Class Selection**
**As a** new user, **I want** to choose my character class based on career goals, **so that** my learning path aligns with my professional aspirations.

*   **Acceptance Criteria:**
    *   Predefined class options are presented with detailed descriptions and career paths
    *   Class selection includes associated skills, strengths, and typical quest types
    *   Custom class creation option for unique career goals
    *   Class selection impacts initial skill tree structure and available quests
    *   Class information is stored in user profile with modification capability
    *   Class-specific onboarding tips and resources are provided

#### **Story: Academic Route Configuration**
**As a** new user, **I want** to define my academic route and learning preferences, **so that** the system can customize my educational journey.

*   **Acceptance Criteria:**
    *   Route selection includes academic focus areas and specializations
    *   Learning pace preferences (intensive, moderate, relaxed) are configurable
    *   Study schedule preferences (morning, afternoon, evening, weekend) are captured
    *   Difficulty preference and challenge level are selectable
    *   Route configuration affects quest generation and skill tree progression
    *   Route can be modified later with impact assessment provided

#### **Story: Document Upload and Validation**
**As a** new user, **I want** to upload my course syllabus and academic documents, **so that** the AI can create my personalized learning path.

*   **Acceptance Criteria:**
    *   File upload supports PDF, DOCX, TXT formats with 10MB size limit
    *   Drag-and-drop interface with progress indicators and error handling
    *   Document validation checks for academic content and readability
    *   Multiple document upload capability for different courses
    *   Secure file storage with encryption and access controls
    *   Upload status tracking with retry capability for failed uploads
    *   Document processing queue with estimated completion times

#### **Story: Initial Character Sheet Generation**
**As a** new user, **I want** to see my personalized character sheet after onboarding completion, **so that** I can visualize my learning persona and initial stats.

*   **Acceptance Criteria:**
    *   Character sheet displays user name, selected class, and academic route
    *   Initial stats are calculated based on profile information and assessments
    *   Character avatar/image is generated or selected during onboarding
    *   Skill tree structure is initialized with locked/unlocked nodes
    *   Character sheet includes progress tracking elements (XP, level, achievements)
    *   Character sheet is accessible from main dashboard with edit capabilities

#### **Story: Onboarding Completion and Dashboard Transition**
**As a** new user, **I want** to smoothly transition from onboarding to the main dashboard, **so that** I can begin my learning journey immediately.

*   **Acceptance Criteria:**
    *   Onboarding completion celebration/animation provides positive reinforcement
    *   Dashboard tutorial highlights key features and navigation elements
    *   Initial quest recommendations are generated based on uploaded documents
    *   Quick start guide is accessible for immediate learning activities
    *   Onboarding can be revisited to modify profile settings
    *   User feedback collection on onboarding experience for continuous improvement

---

### **Epic: Core AI & Quest Generation**
*Goal: Leverage AI to transform academic documents into a personalized and dynamic learning path.*

#### **Story: AI Syllabus Processing & Quest Generation**
**As a** student, **I want** the system to process my syllabus with an AI service, **so that** a meaningful learning path is created.

*   **Acceptance Criteria:**
    *   AI service processes uploaded syllabus documents to extract course structure
    *   System generates quest chapters aligned with course modules and learning objectives
    *   Quest difficulty progression follows academic curriculum pacing
    *   Prerequisites and dependencies are automatically identified and configured
    *   Generated quests include clear learning objectives and success criteria
    *   AI processing status is communicated to users with progress indicators

#### **Story: AI-Powered Curriculum Transformation**
**As a** Student, **I want** the system to automatically generate a new chapter in my Main Quest from each of my FPT course syllabi, **so that** my academic requirements are transformed into a structured learning path.

*   **Acceptance Criteria:**
    *   When a syllabus is processed, a new "Quest Chapter" is created and added to the user's Main Quest Line.
    *   The AI correctly parses key topics, assignments, and deadlines from the syllabus to generate individual `Quests`.
    *   Each generated `Quest` is linked to one or more `Skills` on the user's Skill Tree.
    *   The system establishes the correct prerequisite dependencies between quests based on the curriculum's structure.

#### **Story: Dynamic Quest Adaptation**
**As a** student, **I want** the system to adapt quest difficulty and content based on my performance, **so that** my learning experience remains challenging but achievable.

*   **Acceptance Criteria:**
    *   AI analyzes performance patterns to identify strengths and weaknesses
    *   Quest difficulty automatically adjusts based on success rates and completion times
    *   Additional practice quests are generated for struggling areas
    *   Advanced challenges are unlocked for mastered topics
    *   Learning path modifications are explained to users with reasoning
    *   Performance analytics inform ongoing quest generation and refinement

---

### **Epic: Skill Tree & Knowledge Management**
*Goal: Provide tools for students to organize their knowledge and visualize their academic progress.*

#### **Story: The 'Arsenal' - Note Management**
**As a** student, **I want** to create, import, and organize my study notes, **so that** I have a centralized 'arsenal' of knowledge.

*   **Acceptance Criteria:**
    *   Rich text editor supports formatting, links, images, and code snippets
    *   Note organization system with folders, tags, and search capabilities
    *   Import functionality for existing notes from common formats
    *   Note linking system connects content to skill tree nodes
    *   Collaborative sharing options for study groups and parties
    *   Version history and backup functionality for note preservation

#### **Story: Building the Knowledge Arsenal**
**As a** Student, **I want** to create and link my personal study notes ("Arsenal") to the concepts I'm learning, **so that** I can build a centralized, interconnected knowledge base for my career.

*   **Acceptance Criteria:**
    *   The "Arsenal" provides a rich text editor for creating and organizing study materials.
    *   Users can create a link from any note to one or more nodes on their Skill Tree.
    *   When viewing a Skill Tree node, the user can easily see and access all linked Arsenal notes.
    *   The content within these notes is used by the AI as the source material for generating quiz questions and "Boss Fight" challenges.

#### **Story: Skill Tree Visualization & Navigation**
**As a** student, **I want** to visualize my skill progression through an interactive skill tree, **so that** I can understand my learning path and progress.

*   **Acceptance Criteria:**
    *   Interactive skill tree displays nodes with clear visual states (locked, available, completed)
    *   Navigation allows zooming, panning, and filtering by categories or progress
    *   Each node shows associated quests, prerequisites, and learning outcomes
    *   Progress indicators show completion percentage and mastery levels
    *   Skill tree updates in real-time as quests are completed
    *   Mobile-responsive design ensures accessibility across devices

#### **Story: Visualizing Skill Progression**
**As a** Student, **I want** to see my Skill Tree update in real-time as I complete quests, **so that** I get immediate visual feedback on my progress toward my career goal.

*   **Acceptance Criteria:**
    *   Completing a quest that awards a skill visually updates the corresponding node on the Skill Tree.
    *   Unlocking a prerequisite skill makes the next skill in the dependency chain available to be learned.
    *   The user interface clearly shows the pathway from completed foundational skills towards the more advanced skills required for their chosen career Class.

---

### **Epic: Gamification & Assessment**
*Goal: Make learning engaging and measurable through Unity-based interactive challenges and progress tracking.*

#### **Story: Unity Boss Fight System Setup**
**As a** developer, **I want** to integrate Unity WebGL builds into the Next.js frontend, **so that** students can experience immersive 2D boss fight challenges.

*   **Acceptance Criteria:**
    *   Unity 2022.3 LTS project is created with WebGL build target configured
    *   Unity WebGL builds are integrated into Next.js pages with proper loading states
    *   JavaScript-Unity communication bridge is established for data exchange
    *   Unity game state synchronizes with backend APIs for progress tracking
    *   WebGL builds are optimized for browser performance and mobile compatibility
    *   Error handling and fallback mechanisms are implemented for Unity loading failures

#### **Story: Interactive Boss Fight Challenge Generation**
**As a** student, **I want** the system to generate Unity-based 'Boss Fight' challenges with visual feedback and animations, **so that** I can test my knowledge in an engaging interactive format.

*   **Acceptance Criteria:**
    *   AI generates multiple choice questions based on uploaded course materials
    *   Unity 2D boss characters are created with different visual themes per subject
    *   Question difficulty levels trigger corresponding boss attack patterns and animations
    *   Real-time visual feedback shows correct/incorrect answers with particle effects
    *   Boss health decreases with correct answers, increases with incorrect ones
    *   Progressive difficulty scaling adjusts boss mechanics based on student performance

#### **Story: Boss Fight Gameplay & Results Visualization**
**As a** student, **I want** to complete Unity boss fight challenges and see detailed results with visual progress indicators, **so that** I can track my character progression and learning outcomes.

*   **Acceptance Criteria:**
    *   Unity interface displays real-time score updates and progress bars
    *   Victory/defeat animations play based on final performance
    *   Detailed results screen shows question-by-question breakdown with explanations
    *   XP and skill points are awarded based on performance and difficulty
    *   Results are synchronized with backend for skill tree and leaderboard updates
    *   Achievement unlocks and badges are displayed for milestone completions

#### **Story: Engaging with Core Quests and Assessments**
**As a** Student, **I want** to complete my academic quests through a variety of interactive challenges, including gamified "Boss Fights", **so that** I can master foundational knowledge in a motivating way.

*   **Acceptance Criteria:**
    *   Users can select and start quests directly from their dashboard or Skill Tree.
    *   Quests for theoretical knowledge include gamified quizzes ("knowledge checks").
    *   Major milestones or topics are validated with interactive "Boss Fights" (Unity-based assessments).
    *   The Unity WebGL client is successfully integrated and can be launched from a quest.
    *   The "Boss Fight" includes a 2D boss character, multiple-choice questions generated by the AI, real-time visual feedback, and a scoring mechanism that affects the boss's health.
    *   Successfully completing quests and boss fights awards Experience Points (XP).

#### **Boss Fight Networking by Phase (Unity)**
- **Phase 1 (MVP):** Single-player WebGL; no networking; focus on fast load and reliable results posting to backend.
- **Phase 2 (UGS Multiplayer):** Client-hosted sessions via Unity Lobby + Relay (WSS) using NGO (join-code flow); validate ≤12 players initially; host migration plan documented.
- **Phase 3 (Dedicated Server):** Linux headless authoritative server (Dockerized) with WSS via reverse proxy; target up to 20 players; fairness and anti-exploit guarantees.

Reference: See PRD Technical Guidance → Unity Game Client & Multiplayer — Phased Plan.

---

## **Phase 2: Social & Extension MVP**
*Focus: Introduce multiplayer and external integration features.*

### **Epic: Social & Collaboration (Parties)**
*Goal: Transform learning from an isolated activity into a collaborative, team-based effort.*

#### **Story: Party Creation & Management**
**As a** student, **I want** to create a 'Party' and become its 'Party Leader', **so that** I can organize a study group.

*   **Acceptance Criteria:**
    *   Party creation interface allows setting name, description, and privacy settings
    *   Party Leader role is automatically assigned to creator with management privileges
    *   Maximum party size limits are enforced (configurable, default 8 members)
    *   Party settings include study focus areas, meeting schedules, and collaboration preferences
    *   Party deletion and transfer of leadership capabilities are available
    *   Party analytics track member engagement and group progress

#### **Story: Party Invitations & Membership**
**As a** Party Leader, **I want** to invite other students to my party, **so that** we can study together.

*   **Acceptance Criteria:**
    *   Invitation system supports email invites and direct platform invitations
    *   Invitation status tracking shows pending, accepted, and declined invitations
    *   Join request system allows students to request party membership
    *   Member approval process with automatic and manual acceptance options
    *   Member removal capabilities with appropriate notifications
    *   Role assignment system for different member responsibilities

#### **Story: Shared 'Party Stash'**
**As a** party member, **I want** access to a shared space for notes and resources, **so that** we can easily collaborate.

*   **Acceptance Criteria:**
    *   Shared repository for notes, documents, and study materials
    *   Version control system tracks changes and contributor history
    *   Permission system controls read/write access for different content types
    *   Integration with individual Arsenal systems for easy sharing
    *   Search and organization tools for shared content discovery
    *   Collaborative editing capabilities for shared documents

#### **Story: Forming a Study Party**
**As a** student, **I want** to create or join a 'Party' with other students, **so that** I can collaborate, share knowledge, and study together.

*   **Acceptance Criteria:**
    *   A user can create a "Party" and become its "Party Leader."
    *   The Party Leader can invite other students to their party.
    *   Party members gain access to a shared resource space ("Party Stash") where they can share items from their personal Arsenal.

#### **Story: Meeting Scheduling & Organization**
**As a** Party Leader, **I want** to schedule and organize study meetings within my party, **so that** we can coordinate collaborative learning sessions effectively.

*   **Acceptance Criteria:**
    *   Party Leader can create new meetings with title, description, and agenda.
    *   Meeting types can be selected: Study Session, Project Discussion, Exam Prep, or General Meeting.
    *   Date, time, and duration can be set for meetings.
    *   Meeting invitations are automatically sent to all party members.
    *   Party members can accept, decline, or mark as tentative for meeting invitations.
    *   Meeting details are visible in the party dashboard and individual calendars.

#### **Story: Meeting Content Recording & Capture**
**As a** party member, **I want** the system to provide built-in meeting recording capabilities during our study sessions, **so that** we can preserve important discussions and decisions for future reference.

*   **Acceptance Criteria:**
    *   System provides built-in meeting recording functionality that can be activated during party meetings.
    *   Recording functionality is integrated into the meeting interface and does not require external tools.
    *   Multiple recording methods are supported: manual entry, audio transcription, and automated capture.
    *   Participants can activate/deactivate recording features through the meeting interface.
    *   Content capture includes discussions, shared resources, and key decisions.
    *   Recording permissions and privacy controls are clearly defined within the system.
    *   Raw content is stored securely and linked to the specific meeting.
    *   Browser extension is used only for capturing external web resources, not meeting content.
    *   Recording can be paused, resumed, and stopped by authorized party members.

#### **Story: AI-Generated Meeting Summaries & Action Items**
**As a** party member, **I want** to receive comprehensive AI-generated summaries of our meetings with clear action items, **so that** we can stay organized and follow up on important tasks and decisions.

*   **Acceptance Criteria:**
    *   AI processes recorded meeting content to generate structured summaries.
    *   Summary includes executive overview, key discussion points, and decisions made.
    *   Action items are automatically extracted with clear assignments and deadlines.
    *   Next steps and follow-up actions are identified and organized.
    *   Resources mentioned during the meeting are catalogued and linked.
    *   Study materials covered are mapped to relevant skill tree nodes.
    *   Unresolved questions are highlighted for future discussion.
    *   Summary can be reviewed, edited, and approved by party members before sharing.
    *   Action items can be assigned to specific party members with notifications.

#### **Story: Collaborative Meeting Management**
**As a** Party Leader, **I want** to schedule, organize, and conduct study meetings within my party, **so that** we can coordinate collaborative learning sessions and projects effectively.

*   **Acceptance Criteria:**
    *   The Party Leader can create new meetings with a title, description, and agenda.
    *   Meeting invitations are automatically sent to all party members who can RSVP.
    *   The system provides built-in meeting recording capabilities (e.g., audio transcription, manual notes).
    *   After a meeting, the AI processes the recorded content to generate a structured summary, including key discussion points and action items.
    *   Action items can be assigned to specific party members with notifications.

---

### **Epic: Browser Extension**
*Goal: Integrate the learning experience directly into the student's existing web-based academic workflow.*

#### **Story: Extension for Document Extraction**
**As a** student, **I want** a browser extension that can extract academic info from my university portal, **so that** I can easily import it into RogueLearn.

*   **Acceptance Criteria:**
    *   Extension identifies and extracts structured academic data from supported university portals
    *   Automated extraction of syllabi, schedules, grades, and assignment information
    *   Secure data transmission to RogueLearn platform with user consent
    *   Support for major Learning Management Systems (Canvas, Blackboard, Moodle)
    *   Manual extraction tools for unsupported systems
    *   Data validation and preview before import to platform

#### **Story: Contextual Note Access**
**As a** student, **I want** the extension to show me my relevant notes when I highlight text on a webpage, **so that** I can quickly access my knowledge.

*   **Acceptance Criteria:**
    *   Text highlighting triggers contextual popup with relevant Arsenal notes
    *   Real-time search of personal knowledge base based on highlighted content
    *   Preview of related notes with links to full content in platform
    *   Option to create new notes pre-populated with highlighted text and source
    *   Integration with skill tree to show related learning objectives
    *   Privacy controls ensure notes are only accessible to the user

#### **Story: Installing the Knowledge Capture Extension**
**As a** student, **I want** a browser extension that helps me capture and access information, **so that** my learning is seamlessly integrated with my web browsing.

*   **Acceptance Criteria:**
    *   The browser extension can extract academic information from university portals or other web pages and send it to RogueLearn for processing.
    *   When a user highlights text on a webpage, the extension triggers a popup that displays relevant notes from their "Arsenal" based on the highlighted keywords.

---

## **Phase 3: Specialization & Career Preparation**
*Focus: Advanced curriculum, portfolio development, and career readiness.*

### **Epic: The Specialization Chapter**
*Goal: Enable the student to commit to a specific career path by choosing a Specialization Track and mastering an advanced, industry-aligned curriculum that includes collaborative project work.*

#### **Story: Choosing the Specialization Track**
**As a** Student entering my senior years, **I want** to be presented with a clear choice of Specialization Tracks, **so that** I can formally commit to the advanced chapters of my main career quest.

*   **Acceptance Criteria:**
    *   Upon completing the core curriculum quests (Phase 1), the user is presented with a "Choose Your Specialization" interface.
    *   The system offers a curated list of available tracks (e.g., ".NET Development," "React Frontend Development," "Azure DevOps").
    *   Each track displays a summary of the key skills they will learn, potential job titles, and a preview of the advanced quests.
    *   The user's choice appends the corresponding advanced quest chapters to their existing "Main Quest Line."
    *   This choice is visually represented on their Skill Tree, unlocking the advanced branches related to their chosen track.

#### **Story: Mastering the Specialization Curriculum**
**As a** Student on a Specialization Track, **I want** my new quests to be a blend of my advanced FPT curriculum and industry best practices, **so that** I am learning skills that are directly relevant to the job market.

*   **Acceptance Criteria:**
    *   Quests within this epic are generated from a combination of the student's advanced FPT syllabus and the corresponding roadmap from roadmap.sh.
    *   These quests are more complex and application-focused than those in Phase 1.
    *   Assessments ("Boss Fights") for this epic are more challenging and mimic real-world technical problems.

#### **Story: The First Portfolio Project Quest**
**As a** Student on a Specialization Track, **I want** to be guided through building my first complete portfolio project, **so that** I have a tangible asset to showcase to potential employers.

*   **Acceptance Criteria:**
    *   The system provides a "Project Quest" with a clear brief and a set of requirements for a small but complete application (e.g., "Build a To-Do List API using .NET").
    *   The quest is structured as a checklist of professional tasks: setting up a Git repository, structuring the project, implementing core features, writing unit tests, and creating a `README.md`.
    *   To complete the quest, the user must submit the public URL to their finished project on GitHub.
    *   The system validates that a valid GitHub URL has been provided to mark the quest as "Complete." (Note: Automated code grading is out of scope for this story).

#### **Story: Advanced Algorithmic Challenges (Code Battles)**
**As a** Student preparing for my career, **I want** to tackle interview-style algorithmic challenges relevant to my specialization, **so that** I can build my problem-solving profile.

*   **Acceptance Criteria:**
    *   The platform's "Code Battles" feature is integrated as a core quest type within Specialization Tracks.
    *   These quests present LeetCode-style problems that are relevant to the user's chosen career (e.g., array manipulation for game dev, string parsing for web dev).
    *   The user submits their code, which is automatically judged against a set of test cases.
    *   Successful completion of these challenges contributes to a visible "Problem-Solving" stat on the user's public profile, similar to a LeetCode profile.

---

### **Epic: Career Preparations - The Final Gauntlet**
*Goal: Bridge the final gap between being a skilled student and becoming a compelling job candidate. This epic focuses on creating tangible career assets, preparing for the hiring process, and engaging with the broader professional community.*

#### **Story: The Capstone Portfolio Project**
**As a** graduating Student, **I want** to be guided through building a comprehensive capstone project, **so that** I have a high-quality, impressive application to showcase to employers during job interviews.

*   **Acceptance Criteria:**
    *   The quest provides the student with a *choice* of several professional-grade project briefs relevant to their Specialization Track (e.g., a "Social Media API in .NET," a "Real-time Dashboard in React").
    *   The project requires the integration of multiple advanced skills learned throughout their Main Quest Line.
    *   The quest follows the guided checklist model, including tasks for planning, implementation, testing, and creating a detailed, professional `README.md` file.
    *   Completion requires the user to submit the public URL to their finished project on GitHub.

#### **Story: Professional Branding & Resume Refinement**
**As a** job-seeking Student, **I want** a quest that guides me through refining my resume and online professional profiles, **so that** my personal brand is polished and appealing to recruiters.

*   **Acceptance Criteria:**
    *   The quest provides a checklist of tasks for improving a technical resume, including sections for skills, projects, and experience.
    *   The quest includes tasks for optimizing the user's GitHub and LinkedIn profiles.
    *   Tasks prompt the user to link their new capstone project and their RogueLearn profile (which showcases their Code Battle stats and completed skills).
    *   Quest completion is self-reported by the user checking off all tasks.

#### **Story: The Technical Interview Gauntlet**
**As a** Student preparing for interviews, **I want** to face a series of challenging "Boss Fights" and "Code Battles" that simulate a real technical interview, **so that** I can build confidence and prove my problem-solving skills under pressure.

*   **Acceptance Criteria:**
    *   The system generates a special "Interview Gauntlet" quest line that combines relevant Code Battle challenges (for algorithms) and Boss Fights (for conceptual knowledge).
    *   The challenges are sourced from a pool of questions commonly asked in technical interviews for the user's chosen career path.
    *   The system tracks performance metrics (speed, accuracy) and provides feedback on areas for improvement.
    *   Successfully completing the gauntlet unlocks a special "Interview Ready" achievement that can be displayed on the user's profile.

#### **Story: Guild Creation & Community Engagement**
**As a** Student, **I want** to create or join a "Guild," **so that** I can engage with a larger community, share my knowledge, and learn from experienced peers or Verified Lecturers.

*   **Acceptance Criteria:**
    *   Any user can create a Guild focused on a specific subject or technology.
    *   A verification process exists for educators to become "Verified Lecturers," granting them a badge of credibility.
    *   Guild Masters (either students or Verified Lecturers) can upload course materials and post announcements.
    *   A Guild Master dashboard provides aggregated, anonymized progress data to identify topics where members are struggling.

#### **Story: The Networking Quest**
**As a** Student entering the job market, **I want** a social quest that encourages and guides me to network with professionals in my field, **so that** I can build industry connections and learn about job opportunities.

*   **Acceptance Criteria:**
    *   The quest provides a checklist of actionable networking tasks (e.g., "Attend a local tech meetup," "Connect with 5 engineers from target companies on LinkedIn," "Conduct one informational interview").
    *   The quest provides light guidance and resources on how to approach professional networking.
    *   Quest completion is self-reported.

---

## **Phase 4: Educator & Admin Toolkit**
*Focus: Empower educators and administrators with tools to manage and enhance the learning experience.*

### **Epic: Guild Master Toolkit**
*Goal: Provide educators with tools to create, manage, and enhance courses as interactive "Guilds".*

#### **Story: Guild Creation & Course Material Upload**
**As a** Guild Master, **I want** to create a Guild and upload course materials, **so that** I can establish an interactive learning environment for my students.

*   **Acceptance Criteria:**
    *   Guild creation wizard guides educators through setup process
    *   Course material upload supports multiple formats (PDF, DOCX, video, audio)
    *   Content organization tools for structuring lessons and modules
    *   Integration with existing LMS systems for content import
    *   Preview and publishing workflow for course materials
    *   Student enrollment management with invitation and registration systems

#### **Story: Guild Dashboard & Analytics**
**As a** Guild Master, **I want** to view aggregated progress data for all students in my Guild, **so that** I can identify areas where students are struggling.

*   **Acceptance Criteria:**
    *   Comprehensive analytics dashboard showing student progress and engagement
    *   Identification of common struggle points across quest completion data
    *   Individual student progress tracking with intervention recommendations
    *   Comparative analytics showing class performance trends
    *   Exportable reports for academic record keeping
    *   Real-time alerts for students requiring additional support

---

### **Epic: Game Master (Admin) Toolkit**
*Goal: Provide system administrators with tools to manage and monitor the platform.*

#### **Story: System Monitoring & Analytics**
**As a** Game Master, **I want** to access a comprehensive analytics dashboard, **so that** I can monitor system health and user engagement.

*   **Acceptance Criteria:**
    *   Platform-wide analytics showing user acquisition, retention, and engagement metrics
    *   System performance monitoring with alerts for critical issues
    *   Content analytics tracking quest completion rates and difficulty assessment
    *   User behavior analysis to identify optimization opportunities
    *   Revenue and subscription metrics for business intelligence
    *   Data export capabilities for detailed analysis and reporting

#### **Story: Global Event Management**
**As a** Game Master, **I want** to create and trigger global events, **so that** I can enhance user engagement across the platform.

*   **Acceptance Criteria:**
    *   Event creation tools for platform-wide challenges and competitions
    *   Scheduling system for timed events with automatic triggers
    *   Reward distribution system for event participants
    *   Event performance tracking and analytics
    *   Communication tools for event announcements and updates
    *   Integration with user progression systems for seamless experience

---

## **Phase 5: Advanced Social & Collaboration Features**
*Focus: Enhanced social learning and competitive features.*

### **Epic: Advanced Social Learning & Competition**
*Goal: Enhance social interaction, competition, and collaborative learning to increase engagement.*

#### **Story: Advanced Party Management Tools**
**As a** party leader, **I want** advanced management tools for my study group, **so that** I can coordinate complex study sessions and track member progress effectively.

*   **Acceptance Criteria:**
    *   Party analytics dashboard showing member engagement and progress
    *   Advanced scheduling tools with calendar integration
    *   Customizable party roles and permissions system
    *   Party performance metrics and goal tracking
    *   Automated reminders and notifications for party activities

#### **Story: Real-time Study Session Coordination**
**As a** party member, **I want** real-time coordination tools during study sessions, **so that** I can collaborate effectively with my study group.

*   **Acceptance Criteria:**
    *   Live session status indicators for all party members
    *   Real-time shared whiteboard and note-taking tools
    *   Voice/video integration for remote study sessions
    *   Screen sharing capabilities for collaborative work
    *   Session recording and playback functionality

#### **Story: Peer-to-Peer Learning System**
**As a** student, **I want** to engage in peer teaching and learning, **so that** I can both help others and deepen my own understanding.

*   **Acceptance Criteria:**
    *   Peer tutoring matching system based on skills and needs
    *   Knowledge sharing rewards and recognition system
    *   Peer review and feedback mechanisms
    *   Collaborative problem-solving tools
    *   Peer teaching session scheduling and management

#### **Story: Competitive Learning Challenges**
**As a** student, **I want** to participate in competitive learning challenges, **so that** I can test my knowledge against peers and stay motivated.

*   **Acceptance Criteria:**
    *   Tournament-style knowledge competitions
    *   Skill-based matchmaking for fair competition
    *   Real-time leaderboards and rankings
    *   Achievement badges and rewards system
    *   Team-based challenges and competitions

#### **Story: Knowledge Duels**
**As a** student, **I want** to challenge other students to knowledge duels, **so that** I can test my understanding in a fun, competitive format.

*   **Acceptance Criteria:**
    *   One-on-one challenge system with topic selection
    *   Real-time duel interface with timer and scoring
    *   Duel history and statistics tracking
    *   Spectator mode for other students to watch
    *   Ranking system based on duel performance

---

### **Epic: Enhanced Browser Extension Integration**
*Goal: Provide seamless web integration and contextual learning assistance.*

#### **Story: Advanced Web Content Extraction**
**As a** student, **I want** the browser extension to intelligently extract and organize web content, **so that** I can build my knowledge base efficiently while browsing.

*   **Acceptance Criteria:**
    *   AI-powered content analysis and categorization
    *   Automatic tagging and metadata extraction
    *   Smart content summarization and key point extraction
    *   Integration with existing Arsenal organization system
    *   Bulk content processing and organization tools

#### **Story: Contextual Learning Assistance**
**As a** student, **I want** contextual learning assistance while browsing, **so that** I can connect new information to my existing knowledge.

*   **Acceptance Criteria:**
    *   Real-time content analysis and related knowledge suggestions
    *   Popup displays of relevant Arsenal notes and connections
    *   Skill tree integration showing how content relates to learning goals
    *   Difficulty assessment and learning recommendations
    *   Progress tracking for web-based learning activities

#### **Story: Cross-Platform Synchronization**
**As a** student, **I want** seamless synchronization across all my devices, **so that** my learning progress and content are always up-to-date.

*   **Acceptance Criteria:**
    *   Real-time sync of Arsenal content across devices
    *   Offline capability with sync when connection restored
    *   Conflict resolution for simultaneous edits
    *   Progress synchronization across web and mobile platforms
    *   Backup and restore functionality for all user data

---

## **Phase 6: Marketplace & Economy**
*Focus: Introduce a user-driven economy for sharing and monetizing knowledge.*

### **Epic: Marketplace**
*Goal: Create a platform for students to share and monetize their knowledge.*

#### **Story: Marketplace Creation & Content Upload**
**As a** student, **I want** to upload and share my study materials in the Marketplace, **so that** I can help others and potentially earn rewards.

*   **Acceptance Criteria:**
    *   Content upload system supporting various formats and media types
    *   Quality assessment and moderation workflow for uploaded content
    *   Pricing and monetization options for content creators
    *   Content categorization and tagging system for discoverability
    *   Creator analytics showing content performance and earnings
    *   Revenue sharing system with transparent fee structure

#### **Story: Content Rating & Review**
**As a** student, **I want** to rate and review content in the Marketplace, **so that** I can help others find quality materials.

*   **Acceptance Criteria:**
    *   5-star rating system with detailed review capabilities
    *   Review moderation system to ensure quality and appropriateness
    *   Helpful/unhelpful voting system for reviews
    *   Creator response system for addressing feedback
    *   Review aggregation and display system showing overall ratings
    *   Verified purchase requirement for leaving reviews

---

### **Epic: AI-Powered Curation**
*Goal: Use AI to curate and enhance user-generated content.*

#### **Story: AI Content Review & Elevation**
**As a** platform administrator, **I want** the AI to review and elevate high-quality user content, **so that** the best materials are easily accessible to all users.

*   **Acceptance Criteria:**
    *   AI quality assessment algorithm analyzing content value and accuracy
    *   Automated content categorization and skill mapping
    *   Featured content promotion system highlighting exceptional materials
    *   Quality score system influencing search rankings and visibility
    *   Automated plagiarism and duplicate content detection
    *   Content improvement suggestions generated by AI analysis

#### **Story: Knowledge Pack Creation**
**As a** student, **I want** the AI to curate themed bundles of study materials, **so that** I can access comprehensive resources for specific topics or exams.

*   **Acceptance Criteria:**
    *   AI-generated content bundles based on learning objectives and skill requirements
    *   Personalized knowledge pack recommendations based on user progress and goals
    *   Bundle pricing and packaging options with bulk discounts
    *   Quality assurance ensuring content coherence and completeness
    *   Regular updates to knowledge packs based on curriculum changes
    *   User feedback integration for continuous improvement of curation algorithms

---

## **Post-Capstone Epics (Future Vision)**
*Focus: These features represent the long-term vision for the RogueLearn platform, to be developed after the core career-readiness loop has been validated.*

### **Epic: Enhanced Browser Extension**
*Goal: Fully integrate the RogueLearn experience into the student's daily web-based academic workflow, making knowledge capture and contextual access seamless and intelligent.*

#### **Story: Extension for Document Extraction**
**As a** student, **I want** a browser extension that can extract academic info from my university portal, **so that** I can easily import it into RogueLearn.

*   **Acceptance Criteria:**
    *   The extension can identify and extract structured data like syllabus, GPA, timetables, and exam schedules from supported university portals.
    *   Extracted information is sent to the RogueLearn backend for AI processing to generate or update quests.
    *   The user is notified within the extension when the import and processing are complete.
    *   The extension provides a user-friendly interface for mapping and confirming extracted data fields.

#### **Story: Contextual Note Access**
**As a** student, **I want** the extension to show me my relevant notes when I highlight text on a webpage, **so that** I can quickly access my knowledge.

*   **Acceptance Criteria:**
    *   Highlighting text on any webpage triggers a non-intrusive popup or sidebar.
    *   The extension performs a real-time search of the user's "Arsenal" based on the highlighted keywords.
    *   Relevant notes are displayed in the popup with links to open them fully within the platform.
    *   The user can create a new note pre-populated with the highlighted text and source URL.

#### **Story: Advanced Web Content Extraction**
**As a** student, **I want** the browser extension to intelligently extract and organize web content, **so that** I can build my knowledge base efficiently while browsing.

*   **Acceptance Criteria:**
    *   The extension uses AI-powered content analysis to categorize captured web pages (e.g., Article, Tutorial, Reference).
    *   The system performs automatic tagging and metadata extraction on saved content.
    *   A "Smart Summarization" feature generates key points from long articles.
    *   Content is integrated directly into the user's existing Arsenal organization system.

#### **Story: Contextual Learning Assistance**
**As a** student, **I want** contextual learning assistance while browsing, **so that** I can connect new information to my existing knowledge.

*   **Acceptance Criteria:**
    *   The extension analyzes the content of the current page in real-time to suggest relevant notes from the user's Arsenal.
    *   The UI displays links to related Skill Tree nodes, showing how the current content fits into their learning goals.
    *   The extension can assess the difficulty of an article and provide learning recommendations.
    *   Time spent on relevant educational websites is tracked as part of the user's progress.

#### **Story: Cross-Platform Synchronization**
**As a** student, **I want** seamless synchronization across all my devices, **so that** my learning progress and content are always up-to-date.

*   **Acceptance Criteria:**
    *   Notes and content captured via the browser extension are available in real-time on the main web application.
    *   The extension has offline capabilities, caching captured content and syncing it when a connection is restored.
    *   The system handles conflict resolution for simultaneous edits on different platforms.
    *   Progress made on the web app (e.g., completing a quest) is reflected in extension-based recommendations.

### **Epic: Advanced Social & Collaboration Features**
*Goal: Deepen the collaborative learning experience with more powerful tools for teamwork, peer-to-peer engagement, and friendly competition.*

#### **Story: Advanced Party Management Tools**
**As a** party leader, **I want** advanced management tools for my study group, **so that** I can coordinate complex study sessions and track member progress effectively.

*   **Acceptance Criteria:**
    *   A party analytics dashboard is available, showing member engagement and progress metrics.
    *   Advanced scheduling tools are provided with calendar integration options.
    *   The system supports customizable party roles and permissions.
    *   Party leaders can set and track performance metrics and goals for the group.
    *   The system sends automated reminders and notifications for party activities.

#### **Story: Real-time Study Session Coordination**
**As a** party member, **I want** real-time coordination tools during study sessions, **so that** I can collaborate effectively with my study group.

*   **Acceptance Criteria:**
    *   The UI displays live session status indicators for all party members.
    *   The platform includes a real-time shared whiteboard and collaborative note-taking tools.
    *   Voice/video integration is available for remote study sessions.
    *   Screen sharing capabilities are provided for collaborative work.
    *   Sessions can be recorded and played back later.

#### **Story: Peer-to-Peer Learning System**
**As a** student, **I want** to engage in peer teaching and learning, **so that** I can both help others and deepen my own understanding.

*   **Acceptance Criteria:**
    *   A peer tutoring matching system is available, based on skills and needs.
    *   The system includes a rewards and recognition system for knowledge sharing.
    *   Peer review and feedback mechanisms are implemented.
    *   The platform provides collaborative problem-solving tools.

#### **Story: Competitive Learning Challenges**
**As a** student, **I want** to participate in competitive learning challenges, **so that** I can test my knowledge against peers and stay motivated.

*   **Acceptance Criteria:**
    *   The platform supports tournament-style knowledge competitions.
    *   Skill-based matchmaking is used for fair competition.
    *   Real-time leaderboards and rankings are displayed during events.
    *   An achievement badges and rewards system is in place for winners.
    *   The system supports team-based challenges and competitions.

#### **Story: Knowledge Duels**
**As a** student, **I want** to challenge other students to knowledge duels, **so that** I can test my understanding in a fun, competitive format.

*   **Acceptance Criteria:**
    *   A one-on-one challenge system is available with topic selection.
    *   The duel interface is real-time with a timer and scoring.
    *   The system tracks and displays duel history and statistics.
    *   A spectator mode is available for other students to watch duels.
    *   A ranking system is implemented based on duel performance.