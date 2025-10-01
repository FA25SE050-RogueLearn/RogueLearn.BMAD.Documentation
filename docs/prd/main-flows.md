# **Main Flows Documentation**

## **Overview**
This document outlines the critical main flows for the RogueLearn platform, focusing on the most essential user journeys and system processes that deliver core value to students and educators. Each flow describes the actors involved, the process steps, and the expected outputs.

---

## **Flow 1: Student Onboarding & Character Creation**

### **Description**
The foundational flow where new users create their academic gaming profile and establish their learning journey within the RogueLearn ecosystem.

### **Actors**
- **Primary:** New Student User
- **Secondary:** AI System, User Service

### **Process**
1. User accesses RogueLearn platform
2. User initiates account creation process
3. System presents "Character Creation" interface
4. User selects their "Route" (academic curriculum - e.g., Software Engineering curriculum, Computer Science program)
5. User selects their "Class" (career specialization from roadmap.sh - e.g., Full-Stack Developer, DevOps Engineer)
6. User optionally uploads academic documents for skill tree and arsenal enhancement only
7. AI System performs gap analysis between selected curriculum and career specialization
8. System generates curriculum-based quest line with supplementary roadmap.sh content
9. User completes profile setup and receives curriculum-aligned welcome quest

### **Outputs**
- Fully configured user account with curriculum-career profile
- Curriculum-based quest line structure with roadmap.sh supplements
- Gap analysis results identifying industry skill gaps
- Enhanced skill tree visualization (if documents uploaded)
- Personal dashboard ("Character Sheet") with Route/Class integration
- Welcome quest aligned with curriculum progression

### **Success Criteria**
- User successfully creates account within 10 minutes
- Curriculum and career specialization are properly selected and integrated
- Gap analysis identifies relevant industry skills missing from curriculum
- Enhanced skill tree reflects academic background (if documents uploaded)
- User receives first curriculum-aligned quest within the onboarding flow

---

## **Flow 2: The Core AI Learning Loop (Ingestion, Generation & Adaptation)**

### **Description**
This is the central, dynamic engine of the RogueLearn platform. It combines curriculum-based quest generation with roadmap.sh integration, AI-powered gap analysis, and a continuous adaptive feedback loop that refines the learning experience based on user progress and industry alignment.

### **Actors**
- **Primary:** Student User, AI Adaptation Engine
- **Secondary:** Quest Service, Gap Analysis Engine, Curriculum Parser

### **Process**
1.  **Curriculum-First Generation**: The primary quest generation process:
    *   System analyzes the selected academic curriculum structure
    *   AI generates main quest line based on curriculum progression and requirements
    *   Course sequences, prerequisites, and academic milestones are mapped to quest dependencies
    *   Primary skill tree is established from curriculum learning objectives
2.  **Gap Analysis & Roadmap Integration**: Supplementary content generation:
    *   AI performs gap analysis between curriculum and selected roadmap.sh career specialization
    *   Missing industry skills, technologies, and competencies are identified
    *   Supplementary quests are generated from roadmap.sh content to fill identified gaps
    *   Integration strategy balances curriculum requirements with career preparation
3.  **Optional Document Enhancement**: If user uploads academic documents:
    *   Documents enhance skill tree visualization and arsenal management only
    *   Personal academic progress is reflected in character progression
    *   Documents do not impact quest generation or curriculum structure
4.  **Continuous Adaptation (Feedback Loop)**: Dynamic optimization process:
    *   Monitors user interactions, performance, and engagement across curriculum and career tracks
    *   Identifies learning patterns and areas requiring additional support
    *   Adaptive Engine adjusts quest difficulty and refines gap analysis accuracy
    *   Real-time updates to skill tree visualization and progress tracking
    *   Personalized recommendations balance academic success with career readiness

### **Outputs**
- Curriculum-based main quest line with clear academic progression
- Gap analysis results identifying industry skill deficiencies
- Supplementary quest content from roadmap.sh integration
- Enhanced skill tree visualization (with optional document integration)
- Real-time progress tracking across academic and career preparation tracks
- Automated notifications for curriculum milestones and career skill development
- Continuously optimized learning experience balancing academic and industry requirements

### **Success Criteria**
- Curriculum analysis accurately maps 95%+ of academic requirements to quest structure
- Gap analysis identifies relevant industry skills with 90%+ accuracy
- Supplementary roadmap.sh content integrates seamlessly with curriculum progression
- Document enhancement (when used) improves skill tree visualization without disrupting quest generation
- Adaptive adjustments demonstrate measurable improvement in both academic performance and career readiness

---

## **Flow 3: Gamified Assessment & Boss Fight Experience**

### **Description**
The interactive assessment system that transforms traditional exams into engaging Unity-based gaming experiences with real-time feedback and scoring.

### **Actors**
- **Primary:** Student User
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
The community-focused system that enables a **Guild Master** (a Verified Lecturer or an experienced Student) to create an educational guild, share resources, and monitor the anonymized, aggregated progress of guild members to provide targeted support and foster a healthy learning environment.

### **Actors**
- **Primary:** Guild Master
- **Secondary:** Student Players (Guild Members), Social Service

### **Process**
1. Guild Master creates a new Guild with a specific subject focus.
2. Students discover and join the Guild.
3. Guild Master uploads reference materials and curated resources for the members.
4. System aggregates anonymized student progress data from all members.
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
- Educator interventions show a measurable improvement in student outcomes.
- Student engagement with Guild resources exceeds 70%.

---

## **Flow 6: Enhanced Event Management & Code Battle Competition**

### **Description**
The comprehensive competitive programming system that enables Guild Masters and System Admins to create, manage, and execute system-wide code battle events where participating guilds compete through room-based assignments. This enhanced flow includes advanced event lifecycle management, multi-tier approval workflows, real-time spectator capabilities, and guild-based ranking systems with detailed analytics and post-event reporting.

### **Actors**
- **Primary:** 
  - Guild Master A (Event Creator)
  - User (Student) - Guild members participating in competitions
  - Guild Master B (Participating Guild Leader)
  - Admin (Event Approver & Platform Overseer)
  - **Spectators** - Non-participating users watching live competitions
- **Secondary:** 
  - Web Interface (User interaction layer)
  - API Gateway (Request routing and authentication)
  - **Event Service** (Event lifecycle and approval workflows)
  - **Social Service** (Manages guild information and membership)
  - **Real-time Communication Service** (Live updates and spectator features)
  - Database (Data persistence layer)

### **Enhanced Process**
1. **Event Creation & Multi-Tier Approval Phase:**
   - Admin imports code problems into Code Battle Service with difficulty levels, test cases, and solution templates
   - Guild Master A initiates event creation through enhanced Event Management Service interface
   - **Event Configuration Wizard**: Template-based setup with customizable parameters (event type, scoring rules, time limits, guild eligibility criteria)
   - Event Management Service validates configuration and creates draft event with unique identifier
   - **Multi-tier Approval Workflow**: Guild Master review → Platform Administrator approval → Event activation
   - **Approval Dashboard**: Admins review events with automated validation checks and manual oversight capabilities
   - **Emergency Controls**: Rapid event suspension protocols and real-time moderation tools
   - Upon approval, Event Management Service coordinates with Room Assignment Service for guild member distribution

2. **Enhanced Event Setup & Registration:**
   - Room Assignment Service prepares room allocation system for participating guild members
   - Event Management Service broadcasts approved events with rich metadata and preview capabilities
   - **Guild Registration**: Entire guilds register for system-wide events with member eligibility validation
   - **Calendar Integration**: Automatic scheduling with timezone management and conflict detection
   - **Pre-event Preparation**: Practice rounds, guild coordination tools, and strategy planning interfaces

3. **Real-time Competition Execution:**
   - Room Assignment Service distributes guild members across available battle rooms for balanced competition
   - Code Battle Service initializes secure, containerized execution environments for all participants
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
- **Approval Workflow Performance**: Multi-tier approval completion within 24 hours with 100% automated validation accuracy
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
- **Enhanced Event Management → Assessment:** Code battle competitions provide alternative assessment mechanisms with real-time skill validation.
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
- **Code Battle Completion:** >80% of event participants complete at least one code challenge.
- **Adaptation Success:** Personalized adjustments increase retention by >30%.

### **Overall Platform Health**
- **Weekly Active Users (WAU):** Primary engagement metric
- **User Retention Rate:** Week-over-week user return rate
- **Learning Outcome Improvement:** Measurable academic performance gains
- **Platform Satisfaction:** Net Promoter Score (NPS) >50
- **System Performance:** <500ms response time for core operations