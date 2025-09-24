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

---

### **Epic: Skill Tree & Knowledge Management**
*Goal: Provide tools for students to organize their knowledge and visualize their academic progress.*

#### **Story: The 'Arsenal' - Note Management**
**As a** student, **I want** to create, import, and organize my study notes, **so that** I have a centralized 'arsenal' of knowledge.

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

#### **Story: Party Invitations & Membership**
**As a** Party Leader, **I want** to invite other students to my party, **so that** we can study together.

#### **Story: Shared 'Party Stash'**
**As a** party member, **I want** access to a shared space for notes and resources, **so that** we can easily collaborate.

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

**Acceptance Criteria:**
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

---

### **Epic: Browser Extension**
*Goal: Integrate the learning experience directly into the student's existing web-based academic workflow.*

#### **Story: Extension for Document Extraction**
**As a** student, **I want** a browser extension that can extract academic info from my university portal, **so that** I can easily import it into RogueLearn.

#### **Story: Contextual Note Access**
**As a** student, **I want** the extension to show me my relevant notes when I highlight text on a webpage, **so that** I can quickly access my knowledge.

---

## **Phase 3: Educator & Admin Toolkit**
*Focus: Empower educators and administrators with tools to manage and enhance the learning experience.*

### **Epic: Guild Master Toolkit**
*Goal: Provide educators with tools to create, manage, and enhance courses as interactive "Guilds".*

#### **Story: Guild Creation & Course Material Upload**
**As a** Guild Master, **I want** to create a Guild and upload course materials, **so that** I can establish an interactive learning environment for my students.

#### **Story: Guild Dashboard & Analytics**
**As a** Guild Master, **I want** to view aggregated progress data for all students in my Guild, **so that** I can identify areas where students are struggling.

---

### **Epic: Game Master (Admin) Toolkit**
*Goal: Provide system administrators with tools to manage and monitor the platform.*

#### **Story: System Monitoring & Analytics**
**As a** Game Master, **I want** to access a comprehensive analytics dashboard, **so that** I can monitor system health and user engagement.

#### **Story: Global Event Management**
**As a** Game Master, **I want** to create and trigger global events, **so that** I can enhance user engagement across the platform.

---

## **Phase 4: Advanced Social & Collaboration Features**
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

## **Phase 5: Marketplace & Economy**
*Focus: Introduce a user-driven economy for sharing and monetizing knowledge.*

### **Epic: Marketplace**
*Goal: Create a platform for students to share and monetize their knowledge.*

#### **Story: Marketplace Creation & Content Upload**
**As a** student, **I want** to upload and share my study materials in the Marketplace, **so that** I can help others and potentially earn rewards.

#### **Story: Content Rating & Review**
**As a** student, **I want** to rate and review content in the Marketplace, **so that** I can help others find quality materials.

---

### **Epic: AI-Powered Curation**
*Goal: Use AI to curate and enhance user-generated content.*

#### **Story: AI Content Review & Elevation**
**As a** platform administrator, **I want** the AI to review and elevate high-quality user content, **so that** the best materials are easily accessible to all users.

#### **Story: Knowledge Pack Creation**
**As a** student, **I want** the AI to curate themed bundles of study materials, **so that** I can access comprehensive resources for specific topics or exams.