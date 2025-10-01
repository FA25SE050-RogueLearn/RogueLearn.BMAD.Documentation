### **Functional - Phase 1: Core Student MVP**
*Focus: Establish the fundamental single-player experience.*

1.  **FR1 (User):** The system must allow a new user to create an account and onboard via a 3-step "Character Creation" flow: (1) Route selection (Curriculum), (2) Class selection (Roadmap.sh specialization), and (3) Skill-based roadmap selection.
    
    **Business Logic Specifications:**
    - **Step 1 - Route Selection (Curriculum)**: User selects their academic curriculum which serves as the foundational quest line, including:
      - Software Engineering curriculum
      - Computer Science curriculum  
      - Information Technology curriculum
      - Data Science curriculum
      - Other supported university programs
    - **Step 2 - Class Selection (Roadmap.sh Specialization)**: Career-focused specializations from roadmap.sh that complement and fill gaps in the curriculum:
      - Backend Developer
      - Frontend Developer  
      - Full-Stack Developer
      - DevOps Engineer
      - Data Analyst
      - AI Engineer
      - Game Developer
      - Mobile Developer
    - **Step 3 - Skill-based Roadmap**: System generates integrated learning path combining curriculum requirements with roadmap.sh career specialization
    - **Curriculum-First Approach**: Academic curriculum forms the primary quest line structure, with roadmap.sh providing supplementary career-focused content
2.  **FR2 (User):** During character creation or from their profile, users may optionally upload academic documents to enhance their skill tree visualization and arsenal management features.
    
    **Business Logic Specifications:**
    - **Limited Scope**: Document upload is restricted to skill tree and arsenal management enhancement only
    - **Supported Documents**: Course completion certificates, project portfolios, coding achievements, competition results
    - **Enhanced Features**: Uploaded documents provide:
      - More detailed skill tree node completion tracking
      - Personalized arsenal item unlocks based on demonstrated competencies
      - Achievement badges and progress indicators
    - **No Quest Generation Impact**: User documents do not influence main quest line generation (curriculum-driven)
    - **Skill Tree Enhancement**: Documents help populate and validate skill progression within the curriculum framework
    - **Arsenal Personalization**: Documents unlock specialized tools, resources, and achievements in user's personal arsenal
3.  **FR3 (System):** The system must maintain a comprehensive database of predefined curriculum and syllabus content for all supported academic routes and subjects, with optional document upload capability for enhanced personalization.
4.  **FR4 (AI):** The system's AI must analyze the selected curriculum to identify the user's academic route and structure the primary quest line based on curriculum requirements and academic progression.
    
    **Business Logic Specifications:**
    - **Curriculum Analysis**: AI processes selected academic curriculum to extract:
      - Core subject requirements and prerequisites
      - Semester/term progression structure
      - Learning objectives and competency frameworks
      - Assessment and milestone requirements
    - **Quest Line Generation**: System creates primary quest structure directly from curriculum:
      - Course-based quest objectives aligned with syllabus content
      - Sequential progression following academic calendar
      - Prerequisite-based quest unlocking system
      - Assessment-driven milestone achievements
    - **Academic Route Mapping**: System maps curriculum to supported academic routes:
      - Software Engineering programs
      - Computer Science degrees
      - Information Technology programs
      - Data Science programs
    - **Confidence Scoring**: Each curriculum analysis includes confidence score (0-100%); scores <80% trigger manual review
    - **Supported Curriculum Formats**: PDF syllabus, structured course catalogs, academic program descriptions
5.  **FR4A (AI):** The system's AI must perform gap analysis between the selected curriculum and roadmap.sh career specialization to generate supplementary quests that bridge academic learning with industry requirements.
    
    **Business Logic Specifications:**
    - **Gap Identification**: AI compares curriculum content against selected roadmap.sh career path to identify:
      - Missing industry-standard technologies and frameworks
      - Practical skills not covered in academic coursework
      - Current industry trends and emerging technologies
      - Professional development competencies
    - **Supplementary Quest Generation**: System creates additional quests to fill identified gaps:
      - Technology-specific learning modules (e.g., React, Docker, AWS)
      - Industry best practices and methodologies
      - Professional certification preparation
      - Real-world project simulations
    - **Integration Strategy**: Supplementary quests are woven into curriculum timeline:
      - Aligned with relevant academic courses when possible
      - Scheduled during academic breaks or lighter course loads
      - Prioritized based on career path relevance and industry demand
    - **Dynamic Updates**: Gap analysis refreshes quarterly to incorporate:
      - Updated roadmap.sh content and industry trends
      - User progress and demonstrated competencies
      - Employer feedback and job market requirements
    - **Confidence Thresholds**: Gap analysis requires ≥85% confidence in curriculum mapping and ≥90% confidence in roadmap.sh alignment
6.  **FR5 (AI):** The AI must calculate user skill points on a 1-10 scale based on FPT University scoring system and convert academic performance into relevant skill categories.
    
    **Business Logic Specifications:**
    - **FPT Scoring Integration**: Direct mapping from FPT University's 1-10 grading scale to skill point calculations
    - **Subject Score Conversion**: Each subject score (1-10) directly translates to skill points in relevant categories
    - **Skill Point Distribution**: 
      - Score 9-10: 95-100 skill points in related areas
      - Score 8-8.9: 80-94 skill points in related areas  
      - Score 7-7.9: 70-79 skill points in related areas
      - Score 6-6.9: 60-69 skill points in related areas
      - Score 5-5.9: 50-59 skill points in related areas
      - Score 1-4.9: 10-49 skill points in related areas
    - **Skill Category Mapping**: Subject scores contribute to relevant skill trees (Programming, Mathematics, Design, etc.)
    - **Performance Tracking**: System tracks score trends and adjusts skill point allocation accordingly
6.  **FR6 (AI):** After user selection of smaller major, the AI must suggest related classes aligned to that route.
7.  **FR7 (AI):** The system's AI must use the uploaded academic data (GPA, transcripts, schedules) to influence the character's initial stats.
    
    **Business Logic Specifications:**
    - **GPA Conversion**: GPA (1-10 scale) maps to base stats using formula: Base Stat = (GPA/10.0) × 100, minimum 25 points
    - **Transcript Analysis**: Completed courses contribute +5 points per course to related skill categories (max 200 points per category)
    - **Grade Weighting**: A grades = +10 points, B grades = +7 points, C grades = +4 points, D/F grades = +1 point to relevant skills
    - **Stat Categories**: Intelligence (academic performance), Experience (completed courses), Potential (GPA trend analysis)
8.  **FR8 (AI):** The AI must populate the skill tree with acquired knowledge based on uploaded academic data.
    
    **Business Logic Specifications:**
    - **Course-to-Skill Mapping**: Each completed course maps to 2-5 skill nodes based on course content analysis and predefined curriculum mappings
    - **Skill Level Calculation**: Skill level = (Course Grade × Credit Hours × Relevance Score) / 10, capped at level 100
    - **Prerequisite Chain Recognition**: System identifies course prerequisites and creates skill dependencies (e.g., Calculus I → Calculus II → Differential Equations)
    - **Knowledge Decay**: Skills not reinforced within 12 months decrease by 10% per semester (minimum level 20% of original)
    - **Cross-Skill Synergies**: Related skills provide +5% bonus when multiple skills in same domain exceed level 50
9.  **FR9 (AI):** The AI must automatically create quest lines organized into quest chapters (semesters) with structured quests based on course syllabus.
    
    **Business Logic Specifications:**
    - **Quest Chapter Structure**: Each semester (4 months) = 1 quest chapter containing exactly 10 quests representing the first 10 weeks of study
    - **Quest Generation Timeline**: 
      - Standard subjects (4-month duration): 10 quests covering full semester content
      - Short subjects (2-month duration): 10 quests total, but objectives only generated for first 5 quests of that specific subject
    - **Objective Creation**: Each quest contains smaller objectives for each enrolled subject, generated from course syllabus content
    - **Manual Assignment Integration**: 
      - Users manually input assignments and in-class progress tests
      - AI processes manual inputs to create corresponding objectives in the appropriate quest (based on timing/week)
      - System matches assignment deadlines to quest weeks automatically
    - **Syllabus-Based Content**: Quest objectives derived from predefined curriculum and syllabus stored in system database
    - **Semester Timing**: 
      - Weeks 1-10: Active quest period with learning objectives
      - Weeks 11-12 (Month 3): Exam preparation period
      - Month 4: Retake period for failed courses (no new quests generated)
10. **FR10 (User):** Users must have a personal dashboard ("Character Sheet") that displays their progress, stats (influenced by academic data), a dynamic and interconnected skill tree, and the generated quest line.
11. **FR11 (System):** The skill tree must be visualized as an interconnected mind map with nodes representing individual skills.
    
    **Business Logic Specifications:**
    - **Node Hierarchy**: Skills organized in 4 tiers: Foundation (Level 1-25), Intermediate (26-50), Advanced (51-75), Expert (76-100)
    - **Connection Rules**: Skills connect based on academic prerequisites, logical dependencies, and domain relationships
    - **Visual Indicators**: 
      - Unlocked skills: Full color with progress bar
      - Locked skills: Grayed out with lock icon and prerequisite requirements
      - In-progress skills: Pulsing animation with current XP/next level ratio
    - **Interconnection Logic**: Each skill can have 1-5 prerequisites and unlock 1-8 dependent skills
    - **Layout Algorithm**: Force-directed graph with clustering by domain (Programming, Mathematics, Design, etc.)
12. **FR12 (System):** Each skill tree node must display its skill level, contributed notes from the user's Arsenal, and completed/upcoming quests that contribute to that skill.
13. **FR13 (System):** The skill tree visualization must show relationships between skills and highlight missing skills needed to reach the user's goal.
14. **FR14 (User):** The user's personal study notes ("Arsenal") must be a rich text editor with Notion-like functionalities, allowing for flexible content creation and organization.
15. **FR15 (System):** The skill tree must be able to display links to relevant notes within the user's "Arsenal" for each skill node, with visual indicators showing which Arsenal items contribute to each skill's development.
16. **FR16 (System):** The system must generate gamified mock exams ("Boss Fights") as Unity-based interactive experiences (WebGL) with multiple choice questions, visual feedback, and engaging animations based on upcoming tests input by the player.
17. **FR17 (System):** Each Boss Fight question must have an assigned difficulty level, with higher difficulty questions awarding more points and triggering more challenging visual boss mechanics.
    
    **Business Logic Specifications:**
    - **Difficulty Levels**: Easy (1-2 points), Medium (3-4 points), Hard (5-7 points), Expert (8-10 points)
    - **Point Calculation**: Base points × difficulty multiplier × time bonus (1.5x if answered in first 25% of time limit)
    - **Boss Mechanics Scaling**:
      - Easy: Basic attacks, 100 HP, simple animations
      - Medium: Special abilities, 200 HP, combo attacks
      - Hard: Multiple phases, 350 HP, environmental hazards
      - Expert: Advanced AI patterns, 500 HP, dynamic difficulty adjustment
    - **Question Distribution**: 40% Easy, 30% Medium, 20% Hard, 10% Expert per exam
    - **Adaptive Difficulty**: System adjusts question difficulty based on user's skill level and recent performance
18. **FR18 (System):** The total Boss Fight score must be calculated based on correctly answered questions, weighted by their difficulty levels, with real-time score updates and visual progress indicators in the Unity game interface. Earned points must contribute to the skill tree progression system.
    
    **Business Logic Specifications:**
    - **Point-to-Skill Conversion**: Boss Fight points convert to skill tree experience using formula: Skill XP = (Boss Points × Subject Relevance Score × 0.8)
    - **Skill Tree Contribution Rules**:
      - Points distribute across 2-4 related skill nodes based on question topics
      - Primary skill (most relevant): 60% of earned points
      - Secondary skills (moderately relevant): 25% of earned points  
      - Tertiary skills (loosely relevant): 15% of earned points
    - **Subject Mapping**: Each Boss Fight question maps to specific skill categories (Programming, Mathematics, Design, etc.)
    - **Bonus Multipliers**:
      - Perfect score (100%): +20% skill XP bonus
      - First attempt completion: +15% skill XP bonus
      - Consecutive wins: +5% per consecutive victory (max +25%)
    - **Skill Level Progression**: Each skill level requires exponentially more XP: Level N requires (N × 100) XP points
    - **Cross-Skill Synergies**: Completing Boss Fights in related subjects provides +10% XP bonus to connected skill nodes
19. **FR19 (System):** The system must feature comprehensive leaderboards that display player rankings, both within specific "Classes", for competitive events including real-time code battles, and overall platform performance.
    
    **Business Logic Specifications:**
    - **Ranking Algorithm**: Overall rank = (40% Quest Completion Score + 30% Skill Tree Progress + 20% Boss Fight Performance + 10% Social Contribution)
    - **Score Calculations**:
      - Quest Completion Score: (Completed Quests / Total Available Quests) × 1000 + Quality Bonus (0-200 points)
      - Skill Tree Progress: Sum of all skill levels × skill tier multiplier (Foundation=1x, Intermediate=2x, Advanced=3x, Expert=4x)
      - Boss Fight Performance: Average score across all attempts with recency weighting (recent fights weighted 2x)
      - Social Contribution: Party/Guild participation points + peer ratings + mentoring activities
    - **Leaderboard Categories**:
      - Global: All users across all classes and majors
      - Class-Specific: Users within same role-based roadmap (Backend, Frontend, etc.)
      - Major-Specific: Users within same smaller major (ReactJS/NodeJS, Game Development, etc.)
      - Guild/Party: Rankings within specific social groups
      - **Event-Specific**: Real-time rankings for active code battles and guild-based competitive events
      - **Guild Events**: Guild rankings based on total points accumulated by all guild members in system-wide events
    - **Code Battle Integration**:
      - Real-time score updates during active competitions
      - Performance metrics including solution efficiency, code quality, and completion time
      - Head-to-head comparison displays for direct competitions
      - Historical performance tracking across multiple battle types
    - **Ranking Tiers**: Bronze (0-999), Silver (1000-2499), Gold (2500-4999), Platinum (5000-7499), Diamond (7500+)
    - **Real-time Updates**: Leaderboard positions update within 5 seconds of score changes during active events
    - **Update Frequency**: Real-time for Boss Fights, daily batch updates for Quest/Skill progress, weekly for Social Contribution
    - **Seasonal Resets**: Quarterly leaderboard resets with legacy achievement preservation
20. **FR20 (AI):** The main quest line must be dynamic, allowing for AI-driven changes to uncompleted tasks based on user preferences or new information.
    
    **Business Logic Specifications:**
    - **Dynamic Adjustment Triggers**: 
      - Performance-based: <60% completion rate triggers easier alternatives
      - Schedule changes: New deadlines automatically adjust quest timelines
      - Preference updates: User feedback (too easy/hard) modifies future quest difficulty
      - Academic updates: New course additions/drops trigger quest line regeneration
    - **Modification Rules**:
      - Active quests: Can adjust deadline and difficulty, cannot change core objectives
      - Pending quests: Full modification allowed including content and structure changes
      - Completed quests: Immutable but can influence future quest generation
    - **AI Learning**: System tracks user completion patterns and adjusts future quest generation accordingly
    - **Rollback Protection**: Users can revert quest changes within 24 hours of modification
21. **FR21 (System):** The system must have a notification system to inform users of changes to their quest line and suggest new learning paths.

### **Functional - Phase 2: Social & Extension MVP**
*Focus: Introduce multiplayer and external integration features.*

22. **FR22 (Browser Extension):** The system must provide a browser extension to scan and extract academic documents and information (e.g., syllabus, GPA, timetables, exams, curriculum) from university portals and other web pages.
23. **FR23 (Browser Extension):** The browser extension must automatically organize extracted information and web content into the user's "Arsenal" (notes), categorizing it appropriately.
24. **FR24 (Browser Extension):** When a user highlights text on a webpage, the extension must trigger a popup that displays relevant notes from their "Arsenal" based on the highlighted keywords.
25. **FR25 (User):** A user must be able to create a "Party" (study group) with configurable rules and permissions.
    
    **Business Logic Specifications:**
    - **Party Size Limits**: Minimum 2 members, maximum 8 members for optimal collaboration
    - **Permission Levels**: 
      - Party Leader: Full control (invite/remove members, modify settings, delete party)
      - Co-Leader: Moderate control (invite members, manage shared resources, cannot delete party)
      - Member: Basic access (view content, contribute to discussions, complete shared quests)
    - **Leadership Transfer Rules**:
      - When Party Leader leaves: Leadership automatically transfers to oldest member (by join time)
      - If oldest member is inactive (>7 days): Leadership transfers to most active Co-Leader
      - If no Co-Leaders exist: Leadership transfers to most active Member, automatically promoted to Leader
      - Emergency transfer: Any Co-Leader can initiate leadership vote if Leader inactive >30 days
    - **Configurable Rules**:
      - Study schedule requirements (minimum hours per week: 0-20)
      - Academic performance thresholds (minimum GPA: 2.0-4.0)
      - Communication preferences (required check-ins: daily/weekly/none)
      - Resource sharing policies (open/restricted/approval-required)
    - **Auto-Management**: Inactive members (>14 days) automatically moved to "inactive" status with limited permissions
26. **FR26 (User):** When creating a party, a user must have the option to invite friends directly.
27. **FR27 (User):** When creating a party, a user must have the option to browse a list of other users (strangers), view their relevant stats, and send invitations.
28. **FR28 (User):** When creating a party, a user must have the option to set the party to be open for anyone to join.
29. **FR29 (User):** A Party Leader must be able to invite other registered users to their Party and assign granular permissions to each member.
30. **FR30 (User):** Party members must have access to a shared resource space ("Party Stash") with role-based permissions: view-only access, edit permissions, or comment-only permissions on shared Arsenal items (notes).
    
    **Business Logic Specifications:**
    - **Storage Limits**: 500MB per party (scales with party size: +100MB per member beyond 4)
    - **File Type Support**: Documents (PDF, DOCX, TXT), Images (PNG, JPG, SVG), Code files (common extensions), Links/bookmarks
    - **Permission Matrix**:
      - View-Only: Can read content, download files, cannot modify or comment
      - Comment-Only: Can read and add comments/annotations, cannot edit original content
      - Edit: Full access to modify, delete, and organize shared content
    - **Version Control**: Automatic versioning for edited documents with 30-day history retention
    - **Access Logs**: Track who accessed/modified what content with timestamps for accountability
    - **Content Organization**: Folder structure with tags, search functionality, and content categorization by subject/course
31. **FR31 (User):** The Party Leader must be able to configure permissions for each member individually in the Party Stash.
32. **FR32 (Player):** Players must be able to schedule, organize, and manage study meetings within their party, including setting meeting titles, descriptions, types (Study Session, Project Discussion, Exam Prep, General Meeting), and scheduling details.
33. **FR33 (System/Player):** The system must provide built-in meeting recording capabilities during party study sessions that capture participant discussions, shared resources, and key decisions, with support for multiple recording methods (manual entry, audio transcription, automated capture) that can be activated/deactivated by participants. The browser extension serves only as a content capture tool for external web resources and does not handle meeting recording functionality.
34. **FR34 (AI/Player):** The system must generate comprehensive meeting summaries from recorded content, including executive summaries, key discussion points, action items with assignments, next steps, resources mentioned, study materials covered, and unresolved questions, with options for both AI-generated and manual/collaborative summary creation.
35. **FR35 (System):** The notification system must support email notifications and push notifications for mobile devices with user-configurable preferences.

### **Functional - Phase 3: Educator & Admin Toolkit**
*Focus: Empower educators and administrators with tools.*

36. **FR36 (Guild Master):** Any user (Student or Verified Lecturer) can create a "Guild." Verified Lecturers receive additional features and capabilities within the system.
    
    **Business Logic Specifications:**
    - **Guild Creation Requirements**:
      - Students: Minimum level 25 in any skill, 30+ completed quests, active for 60+ days
      - Verified Lecturers: No restrictions, immediate guild creation access
    - **Guild Size Scaling**: 
      - Tier 1: 10-50 members (basic features)
      - Tier 2: 51-200 members (advanced analytics, custom quests)
      - Tier 3: 201+ members (enterprise features, API access)
    - **Guild Management Tools**:
      - Member recruitment and approval workflows
      - Bulk communication and announcement systems
      - Progress tracking and analytics dashboards
      - Custom quest creation and assignment capabilities
    - **Guild Progression**: Guilds earn experience through member activities and unlock new features at milestone levels
    - **Lecturer-Specific Features**:
      - **Advanced Analytics Dashboard**: Detailed student progress tracking, performance analytics, and learning pattern insights
      - **Custom Quest Creation**: Ability to create and assign specialized quests with custom objectives and deadlines
      - **Bulk Student Management**: Import/export student data, batch operations for large classes
      - **Assessment Integration**: Create and manage custom Boss Fights with tailored difficulty and question sets
      - **Resource Library Access**: Access to premium educational content and templates for quest creation
      - **Priority Support**: Dedicated support channel and faster response times for technical issues
      - **Advanced Reporting**: Generate detailed reports on student engagement, completion rates, and skill development
      - **Curriculum Mapping Tools**: Advanced tools to map course content to skill trees and quest objectives
    - **Access Restrictions**: Lecturer status provides feature access only; lecturers cannot view student notes, personal quests, or private data outside of party/guild rules
    - **Verification Requirements**: Lecturer status requires institutional email verification and periodic re-verification (annually)
    - **Verified Lecturer Process**: The system must provide a verification process for Guild Masters to become "Verified Lecturers" through academic credential validation, enabling enhanced guild creation privileges and credibility indicators
    - **Streamlined Guild Creation**: Verified Lecturers must be able to create guilds with streamlined processes, basic document sharing capabilities, and simple member management tools focused on educational content delivery
37. **FR37 (Player):** Players (Students) in a Guild must be able to use Guild Master-uploaded materials to adjust or supplement their personal quest lines.
38. **FR38 (Guild Master):** A Guild Master must have a basic dashboard to view aggregated, anonymized progress for all Players in their course ("Guild").
    
    **Business Logic Specifications:**
    - **Dashboard Features**: The dashboard must highlight topics or 'boss fights' where a significant percentage of the class is struggling
    - **Reference Materials**: A Guild Master must be able to upload basic reference materials and share them with guild members
    - **Communications**: A Guild Master must be able to create simple announcements and communications for their guild

### **Phase 4: Advanced Social & Collaboration Features**

**Social Learning & Competition:**
- Advanced party management and collaboration tools
- Real-time study session coordination
- Peer-to-peer learning and knowledge sharing
- Competitive learning challenges and tournaments

**Browser Extension Integration:**
- Web content extraction and organization
- Contextual learning assistance
- Seamless integration with Arsenal knowledge base
- Cross-platform synchronization

### **Cross-Cutting Requirements (All Phases)**
*Focus: Core system capabilities that support all phases.*

39. **FR39 (System):** The system must implement basic in-app notifications for quest updates and system announcements.
40. **FR40 (System):** The system must implement structured logging for debugging and basic performance monitoring.
41. **FR41 (System):** The system must implement comprehensive data architecture supporting real-time synchronization and data versioning.
42. **FR42 (System):** The platform must support scalable content delivery with optimized asset loading.

### **Phase 3: Event Management & Code Battle Requirements**
*Focus: Competitive learning platform with comprehensive event management.*

43. **FR43 (System):** The system must provide a real-time code battle execution environment supporting multiple programming languages with automated testing, scoring, and live spectator capabilities.
    
    **Technical Specifications:**
    - **Supported Languages**: JavaScript, Python, Java, C#, Go, TypeScript (extensible architecture)
    - **Execution Environment**: Secure sandboxed containers with resource limits (CPU: 2 cores, Memory: 512MB, Timeout: 30s)
    - **Real-time Features**: Live code sharing, real-time compilation feedback, instant test result updates
    - **Judging System**: Automated test case execution with correctness, efficiency, and code quality scoring
    - **Spectator Mode**: Real-time viewing of competitor progress with syntax highlighting and execution status
    - **Battle Types**: Head-to-head duels, team competitions, time-based challenges, algorithmic problem solving
    - **Performance Metrics**: Solution correctness (40%), execution efficiency (30%), code quality (20%), completion time (10%)

44. **FR44 (System):** The system must support system-wide guild-based competitive events with room assignment management, guild ranking calculations, and comprehensive event lifecycle management.
    
    **Guild Event Features:**
    - **Event Structure**: System-wide events exclusively for participating guilds with no bracket tournaments
    - **Guild Participation**: Guild-based registration, member assignment to battle rooms, cross-guild competitions
    - **Room Management**: Automated room assignment for guild members, balanced distribution across battle rooms
    - **Ranking System**: Final rankings calculated based on total points accumulated by all guild members
    - **Scheduling System**: Flexible time slot management, timezone handling, automated reminders
    - **Prize System**: Guild-based rewards, collective XP bonuses, achievement unlocks, guild leaderboard recognition
    - **Event Analytics**: Guild performance statistics, member participation tracking, engagement metrics
    - **Live Broadcasting**: Event streaming capabilities with guild-focused commentary and audience interaction

45. **FR45 (System):** The system must implement advanced social learning mechanics including peer-to-peer challenges, collaborative problem-solving, and mentorship integration within competitive environments.
    
    **Social Learning Features:**
    - **Peer Challenges**: Direct challenge system between users with customizable problem sets
    - **Collaborative Battles**: Team-based coding challenges requiring coordination and communication
    - **Mentorship Integration**: Mentor-student pairing for guided competitive learning experiences
    - **Knowledge Sharing**: Post-battle code review sessions, solution explanation features
    - **Study Groups**: Competitive study sessions with shared objectives and group scoring
    - **Skill Matching**: Intelligent pairing based on skill levels, learning objectives, and compatibility
    - **Progress Tracking**: Individual and group progress analytics with improvement recommendations

46. **FR46 (System):** The system must provide comprehensive event creation and management tools enabling educators and guild masters to design, schedule, and administer competitive learning events.
    
    **Event Management Capabilities:**
    - **Event Creation Wizard**: Template-based event setup with customizable parameters and rules
    - **Event Types**: Code battles, hackathons, study competitions, skill assessments, collaborative projects
    - **Scheduling Tools**: Calendar integration, recurring event support, timezone management, conflict detection
    - **Participant Management**: Registration systems, capacity limits, prerequisite checking, team formation
    - **Content Management**: Problem set creation, test case definition, resource allocation, difficulty scaling
    - **Monitoring Dashboard**: Real-time event oversight, participant tracking, issue resolution tools
    - **Analytics & Reporting**: Event performance metrics, participant feedback collection, outcome analysis

47. **FR47 (System):** The system must implement a streamlined approval workflow for event management ensuring educational quality and platform integrity through Game Master administrative oversight.
    
    **Approval Workflow Features:**
    - **Game Master Authority**: Game Masters have the highest administrative authority and can create events with automatic approval for all event types
    - **Streamlined Approval Process**: 
      - Game Master Created Events: Direct creation and activation with full administrative authority
      - Guild Master Event Requests: Require Game Master approval for quality assurance and educational oversight
    - **Game Master Permissions**: Full event lifecycle management including creation, modification, deletion, and approval authority over all platform events
    - **Quality Assurance**: Automated content validation, educational standard compliance checking for all event requests
    - **Review Dashboard**: Administrative interface for Game Masters to review Guild Master event requests with batch processing capabilities
    - **Approval Criteria**: Educational value assessment, technical feasibility validation, resource availability check
    - **Feedback System**: Rejection reasoning, improvement suggestions, resubmission workflows for Guild Master requests
    - **Audit Trail**: Complete approval history, decision tracking, accountability logging
    - **Administrative Controls**: Game Masters can suspend events, moderate content, and respond to incidents with full authority



### **Success Metrics (KPIs) with Specific Targets**

#### **Primary Success Metrics**

**User Engagement & Retention**
*   **Weekly Active Users (WAU)**: Target 70% of registered users active weekly by Month 3
    - Baseline: 0 users (new platform)
    - Month 1 Target: 40% of registered users
    - Month 2 Target: 55% of registered users  
    - Month 3 Target: 70% of registered users
    - Success Threshold: >60% sustained for 4 consecutive weeks

*   **User Retention Rate**: Target 40% weekly retention by Month 2
    - Week 1 Retention: 60% (users return after first week)
    - Week 2 Retention: 50% (users return after second week)
    - Week 4 Retention: 40% (users return after fourth week)
    - Month 3 Retention: 35% (users return after 3 months)
    - Critical Threshold: <25% weekly retention triggers intervention

*   **Session Duration**: Target average 25 minutes per session
    - Baseline Target: 15 minutes (Month 1)
    - Growth Target: 20 minutes (Month 2)
    - Optimal Target: 25 minutes (Month 3+)
    - Quality Indicator: >80% of sessions include quest completion

#### **Core Feature Adoption Metrics**

**Quest & Learning Engagement**
*   **Average Number of Quests Completed per User per Week**: Target 8-12 quests weekly
    - New User Target: 3-5 quests in first week
    - Active User Target: 8-12 quests weekly by Month 2
    - Power User Target: 15+ quests weekly
    - Completion Rate: >75% of started quests completed

*   **AI Quest Generation Success Rate**: Target >85% user satisfaction
    - Technical Success: >95% successful generation (no errors)
    - Content Quality: >80% user rating of "helpful" or "excellent"  
    - Relevance Score: >85% content relevance to uploaded documents
    - Time to Generation: <30 seconds for 90% of requests
    
    **Business Logic Specifications:**
    - **Success Rate Calculation**: (Successful generations / Total generation attempts) × 100
    - **Quality Scoring Algorithm**: 
      - User feedback weight: 40% (5-star rating system)
      - Content relevance weight: 35% (AI semantic analysis)
      - Completion rate weight: 25% (% of generated quests completed)
    - **Performance Thresholds**:
      - Green: >85% success rate, >4.0/5.0 average rating
      - Yellow: 70-85% success rate, 3.5-4.0 average rating  
      - Red: <70% success rate, <3.5 average rating (triggers immediate review)
    - **Relevance Scoring**: Cosine similarity between document content and generated quest objectives (minimum 0.75 threshold)

*   **Document Upload & Processing**: Target 90% successful processing
    - Upload Success Rate: >95% of files uploaded successfully
    - Processing Success Rate: >90% of files processed without errors
    - Processing Time: <2 minutes for 80% of documents
    - User Satisfaction: >85% satisfied with generated content

#### **Social & Collaboration Metrics**

**Party & Social Features**
*   **Party Creation Rate**: Target 25% of active users create or join parties
    - Month 1: 10% of users engage with party features
    - Month 2: 18% of users in active parties
    - Month 3: 25% of users in active parties
    - Party Retention: >60% of parties remain active after 2 weeks
    
    **Business Logic Specifications:**
    - **Active Party Definition**: Party with ≥2 members and ≥1 activity (shared resource, meeting, or quest) in past 14 days
    - **Engagement Calculation**: (Users in active parties / Total active users) × 100
    - **Party Health Metrics**:
      - High Activity: >5 interactions per week per member
      - Moderate Activity: 2-5 interactions per week per member
      - Low Activity: <2 interactions per week per member (at-risk status)
    - **Retention Triggers**: Automated engagement prompts when party activity drops below threshold for 7+ days

*   **Collaborative Learning Engagement**: Target 40% participation in social features
    - Party Quest Completion: >70% of party quests completed collaboratively
    - Peer Interaction: Average 5 meaningful interactions per user per week
    - Knowledge Sharing: >30% of users share notes or insights weekly
    - Guild Participation: >15% of users join guilds by Month 3

#### **Technical Performance Metrics**

**System Performance & Reliability**
*   **Page Load Performance**: Target <2 seconds initial load, <1 second navigation
    - Critical Pages (Dashboard, Quest): <1.5 seconds (P95)
    - Secondary Pages: <2.5 seconds (P95)
    - Mobile Performance: <3 seconds on 3G connection
    - Lighthouse Score: >85 for core pages

*   **API Response Times**: Target <500ms for 95% of requests
    - Authentication: <200ms (P95)
    - Quest Generation: <5 seconds (P95)
    - Data Retrieval: <300ms (P95)
    - File Upload: <10 seconds for 10MB files

*   **System Availability**: Target 99.5% uptime
    - Monthly Uptime: >99.5% (≤3.6 hours downtime)
    - Incident Response: <15 minutes for critical issues
    - Recovery Time: <30 minutes for service restoration
    - Planned Maintenance: <4 hours monthly, off-peak hours

#### **Business & Growth Metrics**

**User Acquisition & Conversion**
*   **User Registration Conversion**: Target 15% of visitors register
    - Landing Page Conversion: >12% visitor-to-signup rate
    - Onboarding Completion: >80% complete initial setup
    - First Quest Completion: >70% complete first quest within 48 hours
    - Feature Discovery: >60% use 3+ core features in first week

*   **User Feedback & Satisfaction**: Target NPS >50, CSAT >4.2/5
    - Net Promoter Score (NPS): Target >50 (excellent)
    - Customer Satisfaction (CSAT): Target >4.2/5.0
    - Feature Satisfaction: >80% rate core features as "useful" or "very useful"
    - Support Satisfaction: >90% satisfied with support interactions

#### **Educational Impact Metrics**

**Learning Effectiveness**
*   **Learning Progress Tracking**: Target measurable skill advancement
    - Skill Tree Progression: Average 2-3 skills advanced per week
    - Knowledge Retention: >70% correct answers on review quests after 1 week
    - Concept Mastery: >60% users demonstrate mastery in tracked subjects
    - Study Habit Formation: >50% users maintain consistent weekly study patterns

*   **Academic Integration Success**: Target seamless workflow integration
    - Document Processing Success: >90% of academic documents processed successfully
    - Browser Extension Usage: >40% of users actively use extension weekly
    - Study Session Effectiveness: >75% report improved study organization
    - Time to Value: <10 minutes from document upload to first quest

#### **Content Quality & Moderation Metrics**

**Content & Community Health**
*   **Content Quality Scores**: Target >85% high-quality content
    - AI Content Quality: >85% user rating of "good" or "excellent"
    - User-Generated Content: >80% meets quality standards
    - Moderation Efficiency: <4 hours response time for flagged content
    - Community Guidelines Compliance: >95% content complies with policies

### **Baseline Measurement & Tracking Strategy**

#### **Data Collection Framework**
- **Analytics Platform**: Google Analytics 4 + custom event tracking
- **User Behavior Tracking**: Mixpanel or Amplitude for detailed user journeys
- **Performance Monitoring**: Application Performance Monitoring (APM) tools
- **A/B Testing Platform**: Optimizely or similar for feature experimentation
- **Survey Tools**: In-app surveys + email campaigns for qualitative feedback

#### **Reporting & Review Cadence**
- **Daily Monitoring**: Critical system metrics and user activity
- **Weekly Reviews**: Feature adoption, user engagement, and performance trends
- **Monthly Analysis**: Comprehensive KPI review and goal assessment
- **Quarterly Planning**: Strategic adjustments based on metric performance
- **Annual Evaluation**: Long-term trend analysis and goal setting

#### **Success Thresholds & Intervention Triggers**
- **Green Zone**: Metrics meeting or exceeding targets (continue current strategy)
- **Yellow Zone**: Metrics 10-20% below targets (implement optimization tactics)
- **Red Zone**: Metrics >20% below targets (immediate intervention required)
- **Critical Alerts**: System availability <99%, user satisfaction <3.5/5, or security incidents

### **MVP Validation Approach**

To validate the RogueLearn MVP, we will use a combination of qualitative and quantitative methods:

*   **User Interviews**: We will conduct in-depth interviews with a small group of target users to gather feedback on the platform's usability, features, and overall value proposition.
*   **Surveys**: We will use surveys to collect quantitative data on user satisfaction, feature usage, and other key metrics.
*   **A/B Testing**: We will use A/B testing to compare different versions of the platform and identify the most effective design and features.
*   **Analytics**: We will use analytics tools to track user behavior and identify patterns and trends. This will help us understand how users are interacting with the platform and where we can make improvements.

### Non-Functional Requirements (All Phases)

1.  **NFR1 (Responsiveness):** The application must be a responsive web application, providing an optimal viewing and interaction experience across a range of devices (desktops, tablets, and mobile phones). All functionality must be accessible and usable on screen widths from 320px to 1920px.
2.  **NFR2 (Authentication):** The system must use **Supabase Authentication** for user authentication, ensuring a secure and reliable user management system integrated with the primary data platform.
3.  **NFR3 (Backend Technology):** The system must use .NET 9 and Golang for all backend services, leveraging its performance and security features.
4.  **NFR4 (Services Communication):** Services communicate via RESTful HTTP for inter-service calls, with real-time updates delivered through SignalR; no centralized message broker is used in the MVP.
5.  **NFR5 (Frontend Technology):** The system must use Next.js (version 14 or later) for the frontend application to enable server-side rendering and a fast user experience.
6.  **NFR6 (Data Storage):** The system must use Supabase for database and file storage, configured for scalability and reliability.
7.  **NFR7 (Unity Integration):** Boss Fight interactive experiences must be implemented using Unity WebGL to ensure rich interactive gameplay while maintaining browser compatibility.
8.  **NFR8 (Payment Integration):** The system must integrate with Stripe for future subscription and payment processing, with the initial implementation being a placeholder or a lightweight integration.
9.  **NFR9 (Performance):** Core API endpoints (including user authentication, quest generation, and data retrieval) must respond in under 500ms under a normal load of 100 concurrent users.
10. **NFR10 (Frontend Performance):** The web application's core pages (Dashboard, Skill Tree, Quest Log) must achieve a Google Lighthouse performance score of 85 or higher on both mobile and desktop.
11. **NFR11 (Availability):** The system shall be designed to maintain at least 99.5% uptime, excluding scheduled maintenance windows. Maintenance windows will be limited to a maximum of 4 hours per month and scheduled outside of peak usage times (9 AM - 9 PM local time).
12. **NFR12 (Data Backup & Recovery):** All user data stored in Supabase must have a point-in-time recovery (PITR) backup plan with a recovery point objective (RPO) of 1 hour and a recovery time objective (RTO) of 4 hours.
13. **NFR13 (Security):** The application must implement security best practices to mitigate risks from the OWASP Top 10, including secure data handling, input validation, and protection against injection attacks.
14. **NFR14 (Data Privacy):** The system must be designed with data privacy as a priority, complying with GDPR principles. It must include a configurable data retention policy allowing for automated data deletion upon user request or after a defined period of inactivity.
15. **NFR15 (Access Control):** The system must implement a robust Role-Based Access Control (RBAC) system to ensure users can only access data and functionality appropriate for their role (e.g., Student, Guild Master, Game Master).
16. **NFR16 (Scalability):** The architecture must support horizontal scaling for both the backend services and the database to handle a 50% increase in user traffic over a 3-month period without significant performance degradation. AI/ML workloads must be managed in a cost-effective manner, using serverless functions or dedicated instances as appropriate.
17. **NFR17 (Microservices Architecture):** The system must be built using a microservices architecture with clearly defined service boundaries, independent deployment capabilities, and fault isolation to ensure system resilience and maintainability.
18. **NFR18 (API Design):** All services must expose RESTful APIs with comprehensive OpenAPI documentation, consistent error handling, rate limiting, and versioning support to enable seamless integration and future extensibility.
19. **NFR19 (Real-time Communication):** The system must support real-time communication through WebSocket connections for live notifications, collaborative features, and multiplayer interactions with automatic reconnection and message queuing capabilities.
20. **NFR20 (Content Management):** The system must implement a flexible content management system supporting rich media uploads, automatic transcoding, content versioning, and efficient content delivery with appropriate caching strategies.
21. **NFR21 (Integration Capabilities):** The platform must provide webhook support, third-party API integrations, and plugin architecture to enable seamless integration with external learning management systems, university portals, and educational tools.
22. **NFR22 (Mobile Responsiveness):** The system must provide native mobile app capabilities through progressive web app (PWA) technology with offline functionality, push notifications, and device-specific optimizations for iOS and Android platforms.
23. **NFR23 (Analytics & Monitoring):** The system must implement comprehensive telemetry, distributed tracing, and business intelligence capabilities with real-time dashboards, automated alerting, and data export functionality for institutional reporting.
24. **NFR24 (Compliance & Accessibility):** The platform must comply with WCAG 2.1 AA accessibility standards, support multiple languages and localization, and maintain compliance with educational data privacy regulations including FERPA and COPPA where applicable.