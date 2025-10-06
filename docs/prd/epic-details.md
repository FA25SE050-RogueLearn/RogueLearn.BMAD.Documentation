# **Epic Details**

This document provides detailed user stories for each epic, aligned with the phased rollout plan.

---

### **Epic: Core Business Logic & Scoring Systems**
*Goal: Implement the fundamental business rules, algorithms, and scoring mechanisms that drive the gamification experience.*

#### **Story: Skill Point Calculation Engine**
**As a** system developer, **I want** to implement the core skill point calculation algorithm, **so that** player performance is consistently and fairly quantified across all learning activities.

*   **Acceptance Criteria:**
    *   Skill point calculation follows exact formula: `skill_points = (difficulty_level × base_points × performance_multiplier)`
    *   Difficulty levels are standardized: Easy (1.0), Medium (1.5), Hard (2.0), Expert (2.5)
    *   Base points are configurable per quest type: Knowledge Check (10), Boss Fight (25), Project (50)
    *   Performance multiplier ranges from 0.5 (poor) to 2.0 (exceptional) based on accuracy and completion time
    *   Algorithm handles edge cases: partial completion, time penalties, bonus achievements
    *   Calculation results are logged for audit and debugging purposes
    *   Real-time calculation updates user progress immediately upon quest completion

#### **Story: XP Conversion and Leveling System**
**As a** player, **I want** my skill points to convert to experience points with clear leveling progression, **so that** I can track my overall advancement and unlock new capabilities.

*   **Acceptance Criteria:**
    *   XP conversion follows exact formula: `xp_gained = skill_points × 10`
    *   Level progression uses exponential curve: `level = floor(sqrt(total_xp / 100))`
    *   Level thresholds are clearly defined: Level 1 (100 XP), Level 2 (400 XP), Level 3 (900 XP), etc.
    *   Level-up triggers unlock new features: skill tree nodes, quest types, social features
    *   XP gains are displayed with visual feedback and celebration animations
    *   Historical XP tracking maintains complete progression history
    *   Level benefits are clearly communicated to users upon advancement

#### **Story: Boss Fight Scoring Algorithm**
**As a** player, **I want** Boss Fight challenges to be scored fairly based on accuracy, speed, and difficulty, **so that** my performance reflects my true mastery level.

*   **Acceptance Criteria:**
    *   Scoring follows exact formula: `score = (correct_answers / total_questions) × time_bonus × difficulty_multiplier`
    *   Time bonus calculation: `time_bonus = max(0.5, min(2.0, (time_limit - time_taken) / time_limit + 1))`
    *   Difficulty multipliers: Easy (1.0), Medium (1.3), Hard (1.6), Expert (2.0)
    *   Minimum score threshold for quest completion: 60% for Easy, 70% for Medium, 80% for Hard, 90% for Expert
    *   Partial credit system awards points for partially correct multiple-choice answers
    *   Score breakdown is displayed post-challenge with detailed performance analysis
    *   High scores (>90%) trigger bonus XP rewards and achievement unlocks

#### **Story: Dynamic Difficulty Adjustment Engine**
**As a** player, **I want** the system to automatically adjust quest difficulty based on my performance patterns, **so that** I'm consistently challenged without being overwhelmed.

*   **Acceptance Criteria:**
    *   Performance tracking analyzes last 10 completed quests for trend identification
    *   Success rate thresholds trigger adjustments: >85% success increases difficulty, <60% decreases difficulty
    *   Difficulty adjustments are gradual: maximum one level change per adjustment cycle
    *   Adjustment notifications explain reasoning and provide opt-out mechanism
    *   Performance metrics include: completion time, accuracy, retry attempts, help usage
    *   Difficulty changes affect quest generation, not retroactively applied to existing quests
    *   Manual difficulty override available with performance impact warnings

#### **Story: Leaderboard Ranking Algorithms**
**As a** player, **I want** to see my ranking compared to peers across different categories and timeframes, **so that** I can gauge my progress and stay motivated through friendly competition.

*   **Acceptance Criteria:**
    *   Weekly leaderboard calculation: `weekly_score = sum(skill_points_earned_this_week) × consistency_multiplier`
    *   Monthly leaderboard includes cumulative progress and improvement metrics
    *   Category-specific rankings: by subject, skill type, quest difficulty, party performance
    *   Consistency multiplier rewards regular activity: `consistency = min(2.0, days_active_this_week / 7 + 0.5)`
    *   Ranking updates occur daily at midnight UTC with historical tracking
    *   Privacy controls allow users to opt-out of public rankings while maintaining personal tracking
    *   Leaderboard displays top 100 globally, top 20 in user's institution, and user's relative position

---

## **Phase 1: Core Player MVP**
*Focus: Establish the fundamental single-player experience.*

### **Epic: User Onboarding & Profile Management**
*Goal: Create a seamless and engaging entry point for new users, allowing them to set up their curriculum-based learning profiles with career specialization integration.*

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
**As a** prospective player, **I want** to create an account with email verification, **so that** I can access the RogueLearn platform securely.

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
**As a** new user, **I want** to choose my career specialization (Class) from roadmap.sh options, **so that** my learning path includes industry-relevant skills that complement my academic curriculum.

*   **Acceptance Criteria:**
    *   Career specialization options are sourced from roadmap.sh with detailed descriptions
    *   Class selection includes associated industry skills, technologies, and career progression paths
    *   Each specialization shows gap analysis preview with curriculum integration benefits
    *   Class selection impacts supplementary quest generation and skill tree enhancement
    *   Class information is stored in user profile with modification capability
    *   Class-specific career guidance and industry insights are provided

#### **Story: Academic Route Configuration**
**As a** new user, **I want** to select my academic curriculum (Route) and learning preferences, **so that** the system can generate my primary quest line based on curriculum requirements.

*   **Acceptance Criteria:**
    *   Curriculum selection includes university programs, course sequences, and academic milestones
    *   Learning pace preferences (intensive, moderate, relaxed) are configurable
    *   Study schedule preferences (morning, afternoon, evening, weekend) are captured
    *   Academic difficulty preference and challenge level are selectable
    *   Route configuration drives primary quest generation and curriculum-based skill tree
    *   Route can be modified later with impact assessment on quest progression provided

#### **Story: Document Upload and Validation**
**As a** new user, **I want** to optionally upload my academic documents for skill tree and arsenal enhancement, **so that** my learning visualization is personalized without affecting quest generation.

*   **Acceptance Criteria:**
    *   File upload supports PDF, DOCX, TXT formats with 10MB size limit for academic documents only
    *   Drag-and-drop interface with progress indicators and error handling
    *   Document validation checks for academic content relevance (transcripts, schedules, assignments)
    *   Multiple document upload capability for different courses and academic records
    *   Secure file storage with encryption and access controls
    *   Upload status tracking with retry capability for failed uploads
    *   Clear messaging that documents enhance visualization only, not quest generation
    *   Document processing focuses on skill tree enhancement and arsenal management features

#### **Story: Initial Character Sheet Generation**
**As a** new user, **I want** to see my personalized character sheet after onboarding completion, **so that** I can visualize my curriculum-career learning persona with Route/Class integration.

*   **Acceptance Criteria:**
    *   Character sheet displays user name, selected Class (career specialization), and Route (curriculum)
    *   Initial stats are calculated based on curriculum requirements and career specialization alignment
    *   Character avatar/image is generated or selected during onboarding
    *   Skill tree structure is initialized with curriculum-based progression and career enhancement nodes
    *   Character sheet includes progress tracking elements (XP, level, achievements) for both academic and career tracks
    *   Character sheet is accessible from main dashboard with edit capabilities for Route/Class modifications

#### **Story: Onboarding Completion and Dashboard Transition**
**As a** new user, **I want** to smoothly transition from onboarding to the main dashboard, **so that** I can begin my curriculum-based learning journey with career specialization integration.

*   **Acceptance Criteria:**
    *   Onboarding completion celebration/animation provides positive reinforcement
    *   Dashboard tutorial highlights curriculum progression and career skill development features
    *   Initial quest recommendations are generated based on selected curriculum and gap analysis results
    *   Quick start guide is accessible for immediate curriculum-aligned learning activities
    *   Onboarding can be revisited to modify Route/Class settings with impact assessment
    *   User feedback collection on curriculum-career integration experience for continuous improvement

---

### **Epic: Core AI & Quest Generation**
*Goal: Leverage AI to transform academic curriculum into personalized learning paths with career specialization integration through gap analysis and roadmap.sh enhancement, enhanced with FPTU-specific capabilities for verified students.*

#### **Story: AI Curriculum Processing & Quest Generation**
**As a** student, **I want** the system to process my academic curriculum with AI, **so that** a comprehensive learning path is created with career specialization integration.

*   **Acceptance Criteria:**
    *   AI service processes selected curriculum (Route) to extract course structure and learning objectives
    *   System generates curriculum-based quest chapters aligned with academic modules and progression
    *   Quest difficulty progression follows academic curriculum pacing and requirements
    *   Prerequisites and dependencies are automatically identified from curriculum structure
    *   Generated quests include clear learning objectives tied to academic outcomes
    *   AI processing status is communicated with progress indicators for curriculum analysis

#### **Story: FPTU Student Verification Engine**
**As an** FPTU student, **I want** the system to verify my academic status and documents, **so that** I receive university-specific quest generation and academic calendar integration.

*   **Acceptance Criteria:**
    *   System implements FR48 verification process for FPTU student documents (Student ID, transcripts, enrollment certificates)
    *   Document verification uses OCR and pattern recognition to validate FPTU-specific formats
    *   Verified status unlocks FPTU-specific features: academic calendar sync, quest memory, portal integration
    *   Verification failure provides clear feedback and alternative verification methods
    *   Academic standing (current semester, GPA, failed courses) is extracted and stored securely
    *   Verification status influences quest generation algorithms and difficulty adjustments
    *   System maintains verification audit trail for compliance and debugging

#### **Story: Quest Memory & Continuity System**
**As an** FPTU student, **I want** the system to remember my quest history across semesters, **so that** I have continuity in my learning progression and avoid repetitive content.

*   **Acceptance Criteria:**
    *   System implements FR49 quest memory to track completed quests, performance patterns, and learning preferences
    *   Quest history includes: completion status, performance scores, time spent, difficulty preferences, help usage
    *   Semester transitions maintain quest continuity with <5% content repetition
    *   Memory system influences future quest generation to avoid redundancy and build on previous learning
    *   Students can review their complete quest history with performance analytics and progress trends
    *   Quest memory integrates with academic calendar to align with university semester structure
    *   System provides semester-based progress reports showing quest completion and skill development

#### **Story: Academic Calendar Integration**
**As an** FPTU student, **I want** my quest generation to align with the university academic calendar, **so that** my learning path synchronizes with course schedules and exam periods.

*   **Acceptance Criteria:**
    *   System integrates with FPTU academic calendar to extract semester dates, exam periods, and course schedules
    *   Quest generation aligns with university timeline: 10 quests per semester, paced according to course progression
    *   Exam preparation quests are automatically generated 2-3 weeks before official exam dates
    *   System adjusts quest difficulty and pacing during exam periods and academic breaks
    *   Academic milestones (midterms, finals, project deadlines) trigger specialized quest types
    *   Calendar integration updates automatically when university schedules change
    *   Students receive notifications for upcoming academic deadlines with related quest recommendations

#### **Story: AI-Powered Gap Analysis & Roadmap Integration**
**As a** student, **I want** the system to analyze gaps between my curriculum and career specialization, **so that** supplementary quests from roadmap.sh enhance my learning path.

*   **Acceptance Criteria:**
    *   AI performs gap analysis between selected Route (curriculum) and Class (career specialization)
    *   System identifies missing industry skills not covered in academic curriculum
    *   Roadmap.sh content is integrated to fill identified gaps with relevant career skills
    *   Supplementary quests are generated from roadmap.sh to complement curriculum-based learning
    *   Gap analysis results are visualized showing curriculum coverage vs. industry requirements
    *   Integration maintains academic priority while enhancing career readiness

#### **Story: Dynamic Quest Adaptation with Curriculum Focus**
**As a** student, **I want** the system to adapt quest difficulty based on curriculum requirements and my performance, **so that** my learning experience aligns with academic goals while building career skills.

*   **Acceptance Criteria:**
    *   AI analyzes performance patterns within curriculum-based quest progression
    *   Quest difficulty adjusts based on academic requirements and individual performance
    *   Additional practice quests are generated for curriculum topics requiring reinforcement
    *   Career enhancement quests from roadmap.sh are unlocked based on curriculum mastery
    *   Learning path modifications prioritize curriculum completion with career skill integration
    *   Performance analytics inform ongoing curriculum-focused quest generation and career enhancement

---

### **Epic: Skill Tree & Knowledge Management**
*Goal: Provide tools for students to visualize curriculum-based progression and organize knowledge with career specialization enhancement through optional document integration.*

#### **Story: The 'Arsenal' - Enhanced Note Management**
**As a** student, **I want** to create, import, and organize my study notes to enhance my skill tree visualization, **so that** I have a centralized 'arsenal' of knowledge that complements my curriculum-career learning path.

*   **Acceptance Criteria:**
    *   Rich text editor supports formatting, links, images, and code snippets for comprehensive note-taking
    *   Note organization system with folders, tags, and search capabilities for efficient knowledge management
    *   Import functionality for existing notes from common formats to enhance skill tree nodes
    *   Note linking system connects content to curriculum-based skill tree nodes and career enhancement areas
    *   Collaborative sharing options for study groups and parties within curriculum context
    *   Version history and backup functionality for note preservation and academic continuity

#### **Story: Building the Knowledge Arsenal with Curriculum Integration**
**As a** student, **I want** to create and link my personal study notes ("Arsenal") to curriculum concepts and career skills, **so that** I can build a centralized knowledge base that enhances my Route/Class learning progression.

*   **Acceptance Criteria:**
    *   The "Arsenal" provides a rich text editor for creating notes that enhance curriculum understanding
    *   Users can link notes to curriculum-based skill tree nodes and career specialization areas
    *   When viewing a skill tree node, users can access all linked Arsenal notes for enhanced learning
    *   Arsenal content enhances AI-generated quiz questions and "Boss Fight" challenges based on curriculum
    *   Note organization reflects Route (curriculum) and Class (career) structure for better navigation
    *   Arsenal integration supports both academic requirements and career skill development

#### **Story: Curriculum-Based Skill Tree Visualization & Navigation**
**As a** student, **I want** to visualize my curriculum progression and career skill development through an interactive skill tree, **so that** I can understand my Route/Class learning path and academic progress.

*   **Acceptance Criteria:**
    *   Interactive skill tree displays curriculum-based nodes with clear academic progression states
    *   Navigation allows filtering by Route (curriculum) requirements and Class (career) enhancements
    *   Each node shows associated curriculum quests, career supplements, and learning outcomes
    *   Skill tree reflects gap analysis results with curriculum coverage and career enhancement areas
    *   Visual indicators distinguish between curriculum requirements and career specialization content
    *   Progress tracking shows both academic achievement and career readiness development

*   **Acceptance Criteria:**
    *   Interactive skill tree displays nodes with clear visual states (locked, available, completed)
    *   Navigation allows zooming, panning, and filtering by categories or progress
    *   Each node shows associated quests, prerequisites, and learning outcomes
    *   Progress indicators show completion percentage and mastery levels
    *   Skill tree updates in real-time as quests are completed
    *   Mobile-responsive design ensures accessibility across devices

#### **Story: Visualizing Curriculum-Career Skill Progression**
**As a** student, **I want** to see my skill tree update in real-time as I complete curriculum quests and career enhancement activities, **so that** I get immediate visual feedback on my Route/Class progression toward academic and career goals.

*   **Acceptance Criteria:**
    *   Completing curriculum-based quests visually updates corresponding academic skill nodes
    *   Career enhancement activities from roadmap.sh update career specialization nodes
    *   Unlocking prerequisite skills makes next curriculum skills available following academic progression
    *   User interface clearly shows pathway from curriculum foundations to career specialization skills
    *   Gap analysis results are reflected in skill tree with curriculum coverage and career enhancement indicators
    *   Progress visualization distinguishes between academic requirements and career development achievements

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
    *   Progressive difficulty scaling adjusts boss mechanics based on player performance

#### **Story: Boss Fight Gameplay & Results Visualization**
**As a** student, **I want** to complete Unity boss fight challenges and see detailed results with visual progress indicators, **so that** I can track my character progression and learning outcomes.

*   **Acceptance Criteria:**
    *   Unity interface displays real-time score updates and progress bars
    *   Victory/defeat animations play based on final performance
    *   Detailed results screen shows question-by-question breakdown with explanations
    *   XP and skill points are awarded based on performance and difficulty
    *   Results are synchronized with backend for skill tree and leaderboard updates
    *   Achievement unlocks and badges are displayed for milestone completions

#### **Story: Engaging with Curriculum Quests and Career Assessments**
**As a** student, **I want** to complete my curriculum-based quests through interactive challenges, including gamified "Boss Fights", **so that** I can master academic knowledge while building career skills in a motivating way.

*   **Acceptance Criteria:**
    *   Users can select and start curriculum quests directly from their dashboard or skill tree
    *   Curriculum quests include gamified quizzes based on academic content ("knowledge checks")
    *   Major curriculum milestones are validated with interactive "Boss Fights" (Unity-based assessments)
    *   Career enhancement quests from roadmap.sh integration provide industry-focused challenges
    *   Unity WebGL client launches seamlessly from curriculum and career quest contexts
    *   Quest completion updates both academic progress and career specialization advancement
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
**As a** student, **I want** to create a 'Party' and become its 'Party Leader', **so that** I can organize a study group with clear size limits and management capabilities.

*   **Acceptance Criteria:**
    *   Party creation interface allows setting name, description, and privacy settings
    *   Party Leader role is automatically assigned to creator with management privileges
    *   Party size is strictly enforced: minimum 2 members, maximum 8 members
    *   Party size validation prevents joining when at capacity with clear error messaging
    *   Party settings include study focus areas, meeting schedules, and collaboration preferences
    *   Party deletion requires confirmation and notifies all members 24 hours in advance
    *   Leadership transfer protocol: requires current leader initiation and new leader acceptance
    *   Party analytics track member engagement, attendance rates, and group progress metrics

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
**As a** party member, **I want** access to a shared space for notes and resources with granular permission controls, **so that** we can collaborate effectively while maintaining content security.

*   **Acceptance Criteria:**
    *   Shared repository for notes, documents, and study materials with 1GB storage limit per party
    *   Version control system tracks changes with contributor attribution and rollback capability
    *   Permission system implements three levels: View-only, Contributor, Editor
    *   Party Leader can assign permissions per member and per content type
    *   Integration with individual Arsenal systems allows one-click sharing to party stash
    *   Search functionality includes full-text search across all shared content
    *   Content organization supports folders, tags, and custom categories
    *   Collaborative editing supports real-time multi-user editing with conflict resolution
    *   Content deletion requires confirmation and creates 30-day recovery window
    *   Activity log tracks all stash interactions for accountability and audit purposes

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
*Goal: Integrate the learning experience directly into the student's existing web-based academic workflow, with enhanced FPTU portal integration for verified students.*

#### **Story: Extension for Document Extraction**
**As a** student, **I want** a browser extension that can extract academic info from my university portal, **so that** I can easily import it into RogueLearn.

*   **Acceptance Criteria:**
    *   Extension identifies and extracts structured academic data from supported university portals
    *   Automated extraction of syllabus, schedules, grades, and assignment information
    *   Secure data transmission to RogueLearn platform with user consent
    *   Support for major Learning Management Systems (Canvas, Blackboard, Moodle)
    *   Manual extraction tools for unsupported systems
    *   Data validation and preview before import to platform

#### **Story: FPTU Portal Integration**
**As an** FPTU student, **I want** the browser extension to automatically extract and synchronize my academic data from the FPTU portal, **so that** my quest generation stays current with my university enrollment and performance.

*   **Acceptance Criteria:**
    *   Extension implements FR50 comprehensive FPTU portal integration with automated data extraction
    *   Real-time synchronization of course enrollments, grades, schedules, and academic standing
    *   Automatic detection of FPTU portal pages and seamless data extraction without manual intervention
    *   Secure authentication integration with FPTU portal login credentials
    *   Data extraction includes: current semester courses, completed courses, GPA, failed courses, upcoming exams
    *   Extracted data directly feeds into quest generation algorithms and academic calendar integration
    *   Synchronization occurs within 24 hours of university data updates
    *   Extension provides status indicators for sync success/failure and data freshness

#### **Story: Real-time Academic Data Sync**
**As an** FPTU student, **I want** the browser extension to continuously monitor my FPTU portal for changes, **so that** my RogueLearn experience automatically adapts to my current academic situation.

*   **Acceptance Criteria:**
    *   Extension monitors FPTU portal for academic changes: new course registrations, grade updates, schedule modifications
    *   Automatic quest line adjustments when course enrollments change (add/drop courses)
    *   Real-time academic standing updates trigger appropriate quest difficulty and recovery pathway adjustments
    *   Failed course detection automatically initiates FR51 adaptive recovery quest generation
    *   Extension maintains sync history and provides detailed logs of all academic data changes
    *   Background synchronization operates without disrupting normal browsing activities
    *   Sync conflicts are resolved with user notification and manual override options
    *   Integration with quest memory system ensures academic changes don't disrupt learning continuity

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

## **Phase 3: Simplified Educator Toolkit & Competition Platform**
*Focus: Enhanced social learning, competitive programming, and comprehensive event management.*

### **Epic 3: Advanced Social Learning & Code Battle Competition**
*Goal: Create a comprehensive competitive programming environment with real-time code battles, guild-based events with room assignments, and advanced social learning mechanics that enhance student engagement and skill development.*

#### **Story: Real-time Code Battle Environment**
**As a** student, **I want** to participate in real-time code battles with secure execution environments, **so that** I can compete with peers while solving programming challenges in a fair and monitored setting.

*   **Acceptance Criteria:**
    *   Real-time code battle system supports multiple programming languages (Python, JavaScript, Java, C#)
    *   Secure containerized execution environment prevents malicious code and ensures fair play
    *   Live code editor with syntax highlighting and basic debugging capabilities
    *   Real-time synchronization shows opponent progress and submission status
    *   Automated test case validation with immediate feedback on solution correctness
    *   Battle timer with countdown display and automatic submission at time expiry
    *   Performance metrics tracking including execution time and memory usage
    *   Anti-cheat mechanisms including code similarity detection and execution monitoring

#### **Story: Guild-based Event System**
**As a** Guild Master, **I want** to organize system-wide competitive events for participating guilds, **so that** I can foster healthy competition between guilds and identify top-performing guild communities.

*   **Acceptance Criteria:**
    *   Event creation wizard for system-wide guild competitions with customizable parameters
    *   Guild registration system allowing entire guilds to participate in events
    *   Automated room assignment system distributing guild members across battle rooms
    *   Real-time guild ranking system calculating total points from all guild members
    *   Flexible scheduling system with timezone support and automated reminders
    *   Spectator mode allowing guild members to watch their teammates' battles
    *   Guild analytics dashboard showing member participation and collective performance
    *   Automated prize distribution system with guild-based rewards and recognition
    *   Integration with guild leaderboards and collective achievement systems

#### **Story: Enhanced Social Learning Mechanics**
**As a** student, **I want** advanced social learning features that help me learn from competitions, **so that** I can improve my skills through peer interaction and collaborative analysis.

*   **Acceptance Criteria:**
    *   Post-battle code review system with peer commenting and rating capabilities
    *   Solution explanation feature allowing winners to share their approach
    *   Collaborative problem-solving sessions with shared whiteboards and real-time editing
    *   Mentorship matching system connecting experienced students with beginners
    *   Study group formation tools based on skill level and learning objectives
    *   Knowledge sharing forums with topic-based discussions and expert contributions
    *   Peer tutoring scheduling system with integrated video conferencing
    *   Achievement badges for teaching, mentoring, and community contribution activities

#### **Story: Advanced Competition Analytics**
**As a** student, **I want** detailed analytics on my competitive performance, **so that** I can identify areas for improvement and track my skill development over time.

*   **Acceptance Criteria:**
    *   Comprehensive performance dashboard showing win/loss ratios, skill progression, and ranking trends
    *   Detailed battle history with code submissions, execution metrics, and opponent analysis
    *   Skill assessment reports identifying strengths and weaknesses across different problem types
    *   Comparative analysis showing performance relative to peers and historical data
    *   Learning path recommendations based on competition results and skill gaps
    *   Progress tracking with visual charts and milestone celebrations
    *   Export capabilities for portfolio development and academic record keeping
    *   Integration with external platforms (GitHub, LinkedIn) for professional profile enhancement

---

### **Epic 3: Event Management & Administration Platform**
*Goal: Provide comprehensive event management capabilities with multi-tier approval workflows, automated scheduling, and advanced administrative tools for creating, managing, and analyzing competitive programming events and educational activities.*

#### **Story: Comprehensive Event Creation Wizard**
**As a** Guild Master, **I want** an intuitive event creation wizard with advanced configuration options, **so that** I can efficiently set up complex competitive events with detailed specifications and requirements.

*   **Acceptance Criteria:**
    *   Step-by-step wizard interface guides users through event configuration process
    *   Event type selection supports multiple formats (tournaments, workshops, lectures, code battles)
    *   Advanced scheduling system with timezone support, recurring events, and conflict detection
    *   Customizable participant criteria including skill level, guild membership, and prerequisites
    *   Problem set configuration with difficulty distribution and topic selection
    *   Prize and reward system setup with automated distribution rules
    *   Registration management with capacity limits, waitlists, and approval requirements
    *   Integration with external calendar systems and notification platforms

#### **Story: Multi-tier Approval Workflow System**
**As a** Game Master, **I want** a streamlined approval workflow for Guild Master event requests, **so that** I can ensure quality control and proper resource allocation while maintaining efficient processing times with my highest administrative authority.

*   **Acceptance Criteria:**
    *   Game Master approval interface with customizable criteria and review assignments
    *   Automated validation checks for event feasibility, resource availability, and policy compliance
    *   Real-time notification system for Guild Master event requests and approval status updates
    *   Detailed review interface with commenting, revision requests, and approval tracking
    *   Direct Game Master event creation with automatic approval for all event types
    *   Audit trail maintaining complete history of approval decisions and modifications
    *   Bulk approval capabilities for routine or pre-approved Guild Master event types
    *   Integration with Game Master administrative dashboards and reporting systems

#### **Story: Advanced Event Lifecycle Management**
**As an** event organizer, **I want** comprehensive tools to manage events throughout their entire lifecycle, **so that** I can ensure smooth execution and optimal participant experience.

*   **Acceptance Criteria:**
    *   Pre-event preparation tools including participant communication and resource allocation
    *   Real-time event monitoring with live participant tracking and system performance metrics
    *   Dynamic event modification capabilities for schedule changes and emergency adjustments
    *   Automated participant management including check-in, progress tracking, and support escalation
    *   Live spectator management with viewing permissions and engagement features
    *   Post-event processing including result compilation, feedback collection, and analytics generation
    *   Event archival system with searchable history and performance benchmarking
    *   Integration with learning management systems for academic credit and progress tracking

#### **Story: Comprehensive Analytics & Reporting Dashboard**
**As a** Guild Master, **I want** detailed analytics and reporting capabilities for my events, **so that** I can measure success, identify improvement opportunities, and demonstrate educational value.

*   **Acceptance Criteria:**
    *   Real-time event analytics dashboard showing participation rates, engagement metrics, and performance trends
    *   Detailed participant analysis including skill development tracking and learning outcome assessment
    *   Comparative reporting across multiple events with trend identification and benchmarking
    *   Customizable report generation with export capabilities for academic and administrative purposes
    *   Automated insight generation highlighting key findings and recommendations
    *   Integration with institutional reporting systems and accreditation requirements
    *   Predictive analytics for event planning and resource optimization
    *   ROI analysis demonstrating educational impact and resource utilization efficiency

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

## **Phase 4: Specialization & Career Preparation**
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
**As a** Game Master, **I want** to create and trigger global events with highest administrative authority, **so that** I can enhance user engagement across the platform.

*   **Acceptance Criteria:**
    *   Event creation tools for platform-wide challenges and competitions with Game Master highest authority
    *   Game Master approval workflow for Guild Master created events with streamlined review process
    *   Automatic approval system for all Game Master created events (standard and custom)
    *   Game Master dashboard for managing Guild Master event approval queue and platform oversight
    *   Scheduling system for timed events with automatic triggers and Game Master override capabilities
    *   Reward distribution system for event participants with Game Master administrative control
    *   Event performance tracking and analytics with comprehensive Game Master reporting
    *   Communication tools for event announcements and updates with Game Master broadcast authority
    *   Integration with user progression systems for seamless experience under Game Master oversight

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
    *   The UI displays live session status indicators for all party members
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
    *   Skill-based matchmaking is used for fair competition
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
    *   Ranking system is implemented based on duel performance.
#### **Story: Elective Library Curation & Approval (Admin-Owned)**
**As a** Game Master (System Admin), **I want** to curate, vet, and approve elective content before publication, **so that** the Elective Library maintains academic quality and aligns with the skill-tree.

*   **Acceptance Criteria:**
    *   Multi-source intake (faculty submissions, trusted repos, community proposals)
    *   Review workflow with audit logs and role-based approvals
    *   Tagging and versioning aligned to curriculum routes and skill nodes
    *   Publication queue with rollback capability
    *   Guild Masters and Verified Lecturers can request elective additions but cannot publish directly

#### **Story: University Curriculum Import & Administration (Admin-Owned)**
**As a** Game Master (System Admin), **I want** to manage curriculum imports, versions, and mappings, **so that** platform quests and routes remain synchronized with official academic structures.

*   **Acceptance Criteria:**
    *   Scheduled imports (e.g., per semester) with supported formats (CSV/API)
    *   Version management with effective dates and archival
    *   Route/program mapping with prerequisite validation
    *   Reporting exports (CSV/PDF) for academic oversight
    *   Change logs with approval controls