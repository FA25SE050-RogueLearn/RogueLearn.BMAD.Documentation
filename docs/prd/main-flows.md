# **Main Flows Documentation**

## **Overview**
This document outlines the critical main flows for the RogueLearn platform, focusing on the most essential user journeys and system processes that deliver core value to students and educators. Each flow describes the actors involved, the process steps, and the expected outputs.

---

## **Flow 1: Enhanced Player Onboarding & Character Creation with FPTU Integration**

### **Description**
The foundational flow where new users create their academic gaming profile and establish their learning journey within the RogueLearn ecosystem, with specialized FPTU student verification and academic integration capabilities.

### **Actors**
- **Primary:** New Player User (FPTU Student or General User)
- **Secondary:** AI System, User Service, FPTU Portal Integration Service

### **Process**
1. User accesses RogueLearn platform
2. User initiates account creation process
3. System presents "Character Creation" interface
4. User selects their "Route" (academic curriculum - e.g., Software Engineering curriculum, Computer Science program)
5. User selects their "Class" (career specialization from roadmap.sh - e.g., Full-Stack Developer, DevOps Engineer)
6. **FPTU Student Verification Path** (if applicable):
   - User uploads FPTU-specific documents (Student ID, transcripts, enrollment certificates)
   - System validates FPTU student status through FR48 verification process
   - System integrates with FPTU portal for real-time academic standing verification
   - Academic calendar synchronization and semester timeline integration
7. **General User Path** (if not FPTU student):
   - User optionally uploads academic documents for skill tree and arsenal enhancement only
8. AI System performs gap analysis between selected curriculum and career specialization
9. **FPTU-Enhanced Quest Generation** (for verified FPTU students):
   - System generates curriculum-based quest line with FPTU academic calendar integration
   - Quest chapters align with FPTU semester structure and university deadlines
   - Academic standing influences quest difficulty and recovery pathways
10. **Standard Quest Generation** (for general users):
    - System generates curriculum-based quest line with supplementary roadmap.sh content
11. User completes profile setup and receives curriculum-aligned welcome quest

### **Outputs**
- Fully configured user account with curriculum-career profile
- **FPTU Students**: University-verified academic status and calendar-synchronized quest structure
- **General Users**: Curriculum-based quest line structure with roadmap.sh supplements
- Gap analysis results identifying industry skill gaps
- Enhanced skill tree visualization (if documents uploaded)
- Personal dashboard ("Character Sheet") with Route/Class integration
- **FPTU Integration**: Real-time academic status monitoring and quest synchronization
- Welcome quest aligned with curriculum progression and academic calendar (FPTU students)

### **Success Criteria**
- User successfully creates account within 10 minutes
- Curriculum and career specialization are properly selected and integrated
- **FPTU Students**: Academic verification completes successfully with >95% accuracy
- Gap analysis identifies relevant industry skills missing from curriculum
- Enhanced skill tree reflects academic background (if documents uploaded)
- **FPTU Students**: Quest generation aligns with university calendar and academic standing
- User receives first curriculum-aligned quest within the onboarding flow

---

## **Flow 2: FPTU-Integrated AI Learning Loop (Enhanced Quest Generation & Academic Synchronization)**

### **Description**
This is the central, dynamic engine of the RogueLearn platform enhanced with FPTU-specific capabilities. It combines curriculum-based quest generation with roadmap.sh integration, AI-powered gap analysis, quest memory systems, and real-time FPTU academic synchronization for a comprehensive adaptive learning experience.

### **Actors**
- **Primary:** Player User (FPTU Student or General User), AI Adaptation Engine
- **Secondary:** Quest Service, Gap Analysis Engine, Curriculum Parser, FPTU Portal Integration Service, Quest Memory System

### **Process**
1.  **Enhanced Curriculum-First Generation**: The primary quest generation process with FPTU integration:
    *   System analyzes the selected academic curriculum structure
    *   **FPTU Students**: AI integrates real-time FPTU academic calendar and course schedules
    *   **FPTU Students**: System leverages FR49 quest memory to reference previous semester performance
    *   AI generates main quest line based on curriculum progression and requirements
    *   Course sequences, prerequisites, and academic milestones are mapped to quest dependencies
    *   **FPTU Students**: Quest chapters align with university semester structure (10 quests per semester)
    *   Primary skill tree is established from curriculum learning objectives
2.  **FPTU-Enhanced Gap Analysis & Roadmap Integration**: Supplementary content generation:
    *   AI performs gap analysis between curriculum and selected roadmap.sh career specialization
    *   **FPTU Students**: Analysis incorporates university-specific course content and academic performance data
    *   Missing industry skills, technologies, and competencies are identified
    *   Supplementary quests are generated from roadmap.sh content to fill identified gaps
    *   **FPTU Students**: Integration strategy balances university requirements with career preparation
    *   **FPTU Students**: Failed course recovery quests generated via FR51 adaptive pathways
3.  **Enhanced Document Processing & Academic Verification**: Multi-tier document handling:
    *   **FPTU Students**: Documents undergo FR48 verification for academic status validation
    *   **FPTU Students**: Verified documents directly influence quest generation and academic calendar sync
    *   **General Users**: Documents enhance skill tree visualization and arsenal management only
    *   **FPTU Students**: Real-time portal integration via FR50 for automated data extraction
    *   Personal academic progress is reflected in character progression
4.  **Advanced Continuous Adaptation (Feedback Loop)**: Dynamic optimization with memory:
    *   **FPTU Students**: System maintains comprehensive quest history via FR49 for semester continuity
    *   Monitors user interactions, performance, and engagement across curriculum and career tracks
    *   **FPTU Students**: Tracks academic standing changes and adjusts quest difficulty accordingly
    *   Identifies learning patterns and areas requiring additional support
    *   **FPTU Students**: Adaptive Engine provides specialized recovery pathways for failed courses
    *   Adaptive Engine adjusts quest difficulty and refines gap analysis accuracy
    *   Real-time updates to skill tree visualization and progress tracking
    *   **FPTU Students**: Automated synchronization with university deadlines and exam schedules
    *   Personalized recommendations balance academic success with career readiness

### **Outputs**
- **FPTU Students**: University-synchronized quest line with real-time academic calendar integration
- **General Users**: Curriculum-based main quest line with clear academic progression
- **FPTU Students**: Quest memory continuity across semesters with performance-based adaptation
- Gap analysis results identifying industry skill deficiencies
- **FPTU Students**: Specialized recovery quests for failed courses and academic remediation
- Supplementary quest content from roadmap.sh integration
- Enhanced skill tree visualization (with optional document integration)
- **FPTU Students**: Real-time academic status monitoring and automatic quest adjustments
- Real-time progress tracking across academic and career preparation tracks
- **FPTU Students**: Automated notifications for university deadlines and academic milestones
- Continuously optimized learning experience balancing academic and industry requirements

### **Success Criteria**
- Curriculum analysis accurately maps 95%+ of academic requirements to quest structure
- **FPTU Students**: Academic calendar synchronization maintains >98% accuracy with university systems
- **FPTU Students**: Quest memory system provides seamless semester transitions with <5% content repetition
- Gap analysis identifies relevant industry skills with 90%+ accuracy
- **FPTU Students**: Failed course recovery quests demonstrate >80% success rate in academic remediation
- Supplementary roadmap.sh content integrates seamlessly with curriculum progression
- **FPTU Students**: Real-time portal integration updates academic data within 24 hours of university changes
- Document enhancement (when used) improves skill tree visualization without disrupting quest generation
- **FPTU Students**: Academic standing verification maintains >95% accuracy with university records
- Adaptive adjustments demonstrate measurable improvement in both academic performance and career readiness

---

## **Flow 3: Gamified Assessment & Boss Fight Experience**

### **Description**
The interactive assessment system that transforms traditional exams into engaging Unity-based gaming experiences with real-time feedback and scoring.

### **Actors**
- **Primary:** Player User
- **Secondary:** Unity WebGL Engine, Quest Service

### **Process**
1. User accesses upcoming exam preparation from quest line
2. System generates "Boss Fight" based on exam topics and difficulty
3. Unity WebGL interface loads with boss character and game mechanics
4. User engages with multiple-choice questions presented as combat actions
5. System provides real-time visual feedback for correct/incorrect answers
6. Boss mechanics adapt based on question difficulty and user performance
7. Scoring algorithm calculates weighted scores based on difficulty levels
8. System updates skill tree progress based on assessment results
9. User receives post-assessment feedback and improvement recommendations
10. Results are integrated into leaderboards and progress tracking

### **Outputs**
- Interactive Unity-based assessment experience
- Real-time performance feedback and scoring
- Updated skill tree progress and mastery levels
- Leaderboard rankings and competitive metrics
- Personalized improvement recommendations

### **Success Criteria**
- Boss Fight loads within 5 seconds on standard browsers
- Assessment accurately reflects exam difficulty and topics
- User engagement increases compared to traditional testing
- Performance data integrates seamlessly with skill progression

---

## **Flow 4: Social Learning & Party Collaboration**

### **Description**
The collaborative learning system that enables students to form study groups ("Parties") with shared resources, meeting coordination, and AI-powered collaboration tools.

### **Actors**
- **Primary:** Party Leader, Party Members
- **Secondary:** Meeting Service

### **Process**
1. User creates a new "Party" (study group) with configurable settings
2. Party Leader sets permissions and invites other students
3. Invited users join party and gain access to shared "Party Stash"
4. Party members schedule study meetings with titles, types, and descriptions
5. System provides built-in meeting recording capabilities that can be activated through the meeting interface
6. Browser extension captures external web resources only (not meeting content)
7. AI system processes meeting recordings and generates comprehensive summaries
8. System creates action items with assignments and next steps
9. Party members collaborate on shared Arsenal items with role-based permissions
10. Notification system keeps all members updated on party activities
11. Progress tracking shows collective party achievements and individual contributions

### **Outputs**
- Configured party with member permissions and shared resources
- Scheduled study meetings with automated reminders
- AI-generated meeting summaries with action items
- Collaborative Arsenal with version control and permissions
- Party progress tracking and achievement metrics

### **Success Criteria**
- Party creation completed within 3 minutes
- Meeting summaries capture 95%+ of key discussion points
- Shared resources improve collaborative learning outcomes
- Party engagement metrics show sustained activity

---

## **Flow 5: Guild Management & Educator Dashboard**

### **Description**
The community-focused system that enables a **Guild Master** (a Verified Lecturer or an experienced Player) to create an educational guild, share resources, and monitor the anonymized, aggregated progress of guild members to provide targeted support and foster a healthy learning environment.

### **Actors**
- **Primary:** Guild Master
- **Secondary:** Student Players (Guild Members), Social Service

### **Process**
1. Guild Master creates a new Guild with a specific subject focus.
2. Students discover and join the Guild.
3. Guild Master uploads reference materials and curated resources for the members.
4. System aggregates anonymized player progress data from all members.
5. The Analytics Engine identifies common struggling topics and performance patterns within the guild.
6. The Guild Master's dashboard highlights these areas, enabling them to provide targeted support.
7. Guild Master creates announcements or shares supplementary materials to address common challenges.
8. The system tracks the effectiveness of these interventions by monitoring subsequent performance trends.

### **Outputs**
- A configured Guild with enrolled students.
- Anonymized progress analytics and community performance insights.
- Shared course materials and resources for guild members.
- Communication channels for announcements and support.

### **Success Criteria**
- Guild setup is completed within 15 minutes.
- Analytics accurately identify struggling areas for the community within 24 hours of sufficient data aggregation.
- Educator interventions show a measurable improvement in player outcomes.
- Player engagement with Guild resources exceeds 70%.

---

## **Flow 6: Enhanced Event Management & Competitive Programming Competition**

### **Description**
The comprehensive competitive programming system that enables Guild Masters and System Admins to create, manage, and execute system-wide competitive programming events where participating guilds compete through room-based assignments. This enhanced flow includes advanced event lifecycle management, multi-tier approval workflows, real-time spectator capabilities, and guild-based ranking systems with detailed analytics and post-event reporting.

### **Actors**
- **Primary:** 
  - Guild Master A (Event Creator)
  - User (Player) - Guild members participating in competitions
  - Guild Master B (Participating Guild Leader)
  - Game Master (Admin) (Event Approver & Platform Overseer)
  - **Spectators** - Non-participating users watching live competitions
- **Secondary:** 
  - Web Interface (User interaction layer)
  - API Gateway (Request routing and authentication)
  - **Event Service** (Event lifecycle and approval workflows)
  - **Social Service** (Manages guild information and membership)
  - **Real-time Communication Service** (Live updates and spectator features)
  - Database (Data persistence layer)

### **Enhanced Process**
1. **Event Creation & Streamlined Approval Phase:**
   - Game Master imports code problems into Event Service with difficulty levels, test cases, and solution templates
   - Guild Master A initiates event creation through enhanced Event Management Service interface
   - **Event Configuration Wizard**: Template-based setup with customizable parameters (event type, scoring rules, time limits, guild eligibility criteria)
   - Event Management Service validates configuration and creates draft event with unique identifier
   - **Streamlined Approval Workflow**: Guild Master event requests → Game Master approval with highest administrative authority
   - **Game Master Dashboard**: Game Masters review Guild Master events with automated validation checks and administrative oversight capabilities
   - **Emergency Controls**: Rapid event suspension protocols and real-time moderation tools under Game Master authority
   - Upon approval, Event Management Service coordinates with Room Assignment Service for guild member distribution

2. **Enhanced Event Setup & Registration:**
   - Room Assignment Service prepares room allocation system for participating guild members
   - Event Management Service broadcasts approved events with rich metadata and preview capabilities
   - **Guild Registration**: Entire guilds register for system-wide events with member eligibility validation
   - **Calendar Integration**: Automatic scheduling with timezone management and conflict detection
   - **Pre-event Preparation**: Practice rounds, guild coordination tools, and strategy planning interfaces

3. **Real-time Competition Execution:**
   - Room Assignment Service distributes guild members across available battle rooms for balanced competition
   - Event Service initializes secure, containerized execution environments for all participants
   - **Live Spectator Mode**: Real-time viewing capabilities with commentary support and audience interaction
   - **Multi-language Support**: Comprehensive programming language options with standardized testing frameworks
   - **Real-time Guild Rankings**: Live ranking updates based on total points accumulated by all guild members
   - **Collaborative Features**: Guild coordination tools with communication channels and shared progress tracking
   - **Performance Monitoring**: System health tracking with automatic scaling and load balancing

4. **Guild Performance & Management:**
   - Room Assignment Service manages room transitions and member redistribution as needed
   - **Live Broadcasting**: Event streaming with integrated commentary and guild-focused analysis features
   - **Dynamic Scoring**: Guild ranking calculations based on cumulative member performance across all rooms
   - **Intervention Tools**: Real-time moderation, technical support, and dispute resolution capabilities

5. **Comprehensive Results & Analytics:**
   - **Advanced Analytics**: Detailed performance statistics, participation tracking, and engagement metrics
   - **Post-event Reporting**: Comprehensive outcome analysis with actionable insights and recommendations
   - **Achievement System**: Automated badge and reward distribution based on performance and participation
   - **Knowledge Sharing**: Post-battle code review sessions and solution explanation features
   - **Feedback Collection**: Participant and spectator feedback with sentiment analysis and improvement suggestions

### **Enhanced Outputs**
- **Comprehensive Event Management**: End-to-end event lifecycle with automated workflows and approval processes
- **Guild-based Event Systems**: System-wide events with room assignment management and guild ranking calculations
- **Real-time Competition Environments**: Secure, scalable code execution with live monitoring and performance tracking
- **Enhanced Guild Rankings**: Multi-dimensional guild ranking systems with collective performance tracking and analytics
- **Live Spectator Experience**: Real-time viewing capabilities with commentary, analysis, and audience interaction features
- **Detailed Analytics & Reporting**: Comprehensive performance metrics, engagement tracking, and actionable insights
- **Achievement & Recognition Systems**: Automated badge distribution and skill validation through competitive performance
- **Knowledge Sharing Platforms**: Post-event code review, solution explanations, and collaborative learning opportunities

### **Enhanced Success Criteria**
- **Event Creation Efficiency**: Average event setup time reduced to <15 minutes with 95% success rate using configuration wizard
- **Approval Workflow Performance**: Streamlined Game Master approval completion within 12 hours with 100% automated validation accuracy
- **Competition Participation**: 70% of active guild members participate in monthly competitions with <10% drop-off rate
- **Real-time System Performance**: Support for 100+ concurrent participants with <2s code execution latency and 99.9% uptime
- **Spectator Engagement**: Average of 50+ spectators per major event with <500ms update latency and 85% retention rate
- **Learning Outcomes**: 40% improvement in problem-solving skills measured through pre/post competition assessments
- **Community Building**: 60% increase in guild activity and collaboration following competitive events
- **User Satisfaction**: >4.2/5.0 satisfaction score for competition experience with detailed feedback analysis
- **Knowledge Sharing**: 80% of participants engage with post-event learning materials and code reviews
- **Room Assignment Efficiency**: Automated room distribution with 99.9% accuracy and balanced guild member allocation
- **Analytics Utilization**: 90% of Guild Masters use competition analytics for member development planning
- **Security Compliance**: Zero critical security incidents in code execution environment with comprehensive audit trails

---

## **Integration Points**

### **Cross-Flow Dependencies**
- **Onboarding → Core AI Loop:** Character creation data and initial document uploads trigger the AI learning loop.
- **Core AI Loop → Assessment:** The generated quest line includes and leads to "Boss Fight" assessments.
- **Assessment → Core AI Loop:** Assessment results feed back into the adaptive learning part of the loop.
- **Social Learning → Guild Management:** Party activities can contribute to Guild analytics and community health.
- **Guild Management → Enhanced Event Management:** Guild Masters can create comprehensive competitive events with advanced configuration options.
- **Enhanced Event Management → Assessment:** Competitive programming competitions provide alternative assessment mechanisms with real-time skill validation.
- **Enhanced Event Management → Social Learning:** Competitive events foster guild collaboration, peer learning, and knowledge sharing through post-event analysis.
- **Enhanced Event Management → Core AI Loop:** Competition performance data feeds into personalized learning recommendations and skill gap analysis.

### **Enhanced Data Flow Architecture**
- **User Profile Service:** Central repository for character data, preferences, and competitive performance history.
- **AI Processing Pipeline:** Handles all content ingestion, adaptive generation, and competition-based learning insights.
- **Real-time Synchronization:** Ensures consistent state across all user interactions, including live competition updates.
- **Analytics Engine:** Processes data from all flows for insights, recommendations, and competitive performance analysis.
- **Notification Hub:** Coordinates alerts and updates across all system components, including real-time competition notifications.
- **Event Management Pipeline:** Orchestrates event lifecycle, approval workflows, and tournament coordination.
- **Competition Analytics Service:** Specialized processing for competitive performance metrics and skill validation.

---

## **Success Metrics**

### **Flow-Specific KPIs**
- **Onboarding Completion Rate:** >85% of users complete character creation.
- **Core AI Loop Engagement:** >70% of users successfully process content and start a quest within 48 hours.
- **Assessment Participation:** >90% of users engage with Boss Fight assessments.
- **Social Learning Adoption:** >40% of users join or create parties.
- **Guild Effectiveness:** Educator interventions improve outcomes by >20%.
- **Event Management Engagement:** >60% of active guilds participate in competitive events monthly.
- **Competitive Programming Event Completion:** >80% of event participants complete at least one code challenge.
- **Adaptation Success:** Personalized adjustments increase retention by >30%.

### **Overall Platform Health**
- **Weekly Active Users (WAU):** Primary engagement metric
- **User Retention Rate:** Week-over-week user return rate
- **Learning Outcome Improvement:** Measurable academic performance gains
- **Platform Satisfaction:** Net Promoter Score (NPS) >50
- **System Performance:** <500ms response time for core operations