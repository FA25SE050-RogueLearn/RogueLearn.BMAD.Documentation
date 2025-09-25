# **Main Flows Documentation**

## **Overview**
This document outlines the critical main flows for the RogueLearn platform, focusing on the most essential user journeys and system processes that deliver core value to students and educators. Each flow describes the actors involved, the process steps, and the expected outputs.

---

## **Flow 1: Student Onboarding & Character Creation**

### **Description**
The foundational flow where new users create their academic gaming profile and establish their learning journey within the RogueLearn ecosystem.

### **Actors**
- **Primary:** New Student User
- **Secondary:** AI System, Authentication Service

### **Process**
1. User accesses RogueLearn platform
2. User initiates account creation process
3. System presents "Character Creation" interface
4. User selects their "Class" (career goal - e.g., Full-Stack Developer, Data Scientist)
5. User selects their "Route" (current academic path - e.g., Software Engineering, Computer Science)
6. User uploads academic documents (GPA, transcripts, timetable, examination schedule)
7. AI System processes uploaded documents to influence character stats
8. System generates initial skill tree based on academic data
9. User completes profile setup and receives welcome quest

### **Outputs**
- Fully configured user account with academic profile
- Personalized character stats influenced by real academic performance
- Initial skill tree populated with existing knowledge
- Welcome quest line generated
- Personal dashboard ("Character Sheet") ready for use

### **Success Criteria**
- User successfully creates account within 10 minutes
- Academic documents are properly parsed and integrated
- Initial skill tree reflects user's academic background
- User receives first quest within the onboarding flow

---

## **Flow 2: The Core AI Learning Loop (Ingestion, Generation & Adaptation)**

### **Description**
This is the central, dynamic engine of the RogueLearn platform. It combines content ingestion (from either file uploads or a browser extension), AI-powered generation of personalized learning paths, and a continuous, adaptive feedback loop that refines the experience based on user progress.

### **Actors**
- **Primary:** Student User, AI Adaptation Engine
- **Secondary:** Syllabus Processing Service, Quest Generation Engine, Browser Extension, Progress Analytics, Notification System

### **Process**
1.  **Content Ingestion (Triggers)**: The loop is initiated in one of two ways:
    *   **File Upload**: A user uploads a course syllabus or other academic document (PDF, DOCX).
    *   **Browser Extension**: A user captures educational content from a university portal or academic website.
2.  **AI-Powered Generation**: The backend begins an asynchronous process to:
    *   Ingest and parse the document content.
    *   Identify the course structure, key topics, and learning objectives.
    *   Generate an interconnected skill tree based on the content.
    *   Create a personalized "main quest line" aligned with the learning objectives.
    *   Establish prerequisites and dependencies between skills and quests.
    *   Populate the user's "Arsenal" with the processed content and notify the user of completion.
3.  **Continuous Adaptation (Feedback Loop)**: Once the user begins interacting with the generated quests and content, the system continuously:
    *   Monitors user interactions, performance, and engagement across all features.
    *   Identifies patterns, including areas of struggle or accelerated learning.
    *   The Adaptive Engine dynamically adjusts quest difficulty and modifies learning paths.
    *   The Skill Tree visualization is updated in real-time to reflect progress.
    *   Personalized recommendations for improvement or new challenges are generated.
    *   The feedback loop constantly refines the personalization algorithms to optimize the learning journey.

### **Outputs**
- A dynamic, interconnected skill tree that evolves with the user.
- A personalized quest line that adapts to the user's performance.
- Real-time progress visualization and analytics.
- Automated notifications for quest updates, achievements, and recommendations.
- A continuously optimized and personalized learning experience.

### **Success Criteria**
- Content ingestion (upload or capture) is accurately parsed over 95% of the time.
- The initial generated skill tree and quest line cover 90%+ of the source material's topics.
- Adaptive adjustments demonstrate a measurable improvement in user learning outcomes and engagement.
- The entire loop feels seamless to the user, from content input to personalized feedback.

---

## **Flow 3: Gamified Assessment & Boss Fight Experience**

### **Description**
The interactive assessment system that transforms traditional exams into engaging Unity-based gaming experiences with real-time feedback and scoring.

### **Actors**
- **Primary:** Student User
- **Secondary:** Unity WebGL Engine, Assessment System, Scoring Algorithm

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
- **Secondary:** Meeting Recording System, AI Summary Generator, Notification Service

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
- **Secondary:** Student Players (Guild Members), Analytics Engine, Content Management System

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

## **Flow 6: Event Management & Code Battle Competition**

### **Description**
The competitive programming system that enables Guild Masters and System Admins to create code battle events where guilds compete against each other. This flow manages event creation, approval workflows, participant registration, and real-time competitive coding battles with comprehensive scoring and leaderboards.

### **Actors**
- **Primary:** Guild Master A (Event Creator), Admin (Event Approver), Guild Master B (Participating Guild Leader)
- **Secondary:** Guild Members (Students), Code Battle Service, Social Service, Database, Analytics Engine

### **Process**
1. **Event Creation & Approval Phase:**
   - Admin imports code problems into the system with difficulty levels and test cases
   - Guild Master A initiates event creation through the guild management interface
   - System presents event configuration options (number of easy/medium/hard questions, points per question, start/end times, participation rules)
   - Guild Master A configures event parameters and submits event creation request
   - Admin receives notification of pending event approval request
   - Admin reviews event configuration and approves or rejects the request
   - Upon approval, system finalizes event creation and makes it available for registration

2. **Event Setup & Guild Registration:**
   - System randomly selects code problems based on Guild Master A's configured difficulty distribution
   - Code Battle Service creates event infrastructure and prepares judging environment
   - System broadcasts approved event announcement to all eligible guilds
   - Guild Master B (and other guild leaders) review available events
   - Guild Master B registers their guild for participation in the approved event
   - System validates guild eligibility and confirms registration
   - Code Battle Service coordinates with Social Service to verify guild information and member counts

3. **Competition Execution:**
   - Code Battle Service initializes real-time battle rooms for all registered guilds
   - System distributes the randomly selected code problems to all participants simultaneously
   - Guild members from participating guilds submit code solutions through the battle interface
   - Code Battle Service evaluates submissions against test cases in real-time
   - System updates individual participant leaderboards and guild aggregate scores
   - Real-time notifications keep all participants informed of current standings and progress

4. **Results & Analytics:**
   - System calculates final individual and guild rankings based on scoring algorithm
   - Analytics Engine processes comprehensive performance data from the competition
   - System updates guild reputation scores and individual member achievements
   - Final results are published to all participating guild dashboards and community leaderboards
   - Post-event analysis provides insights and recommendations for future events

### **Outputs**
- Configured competitive events with approved code problems
- Real-time battle environments with secure code execution
- Individual participant leaderboards with detailed scoring
- Guild-based leaderboards showing collective performance
- Performance analytics and achievement tracking
- Event history and statistical reports

### **Success Criteria**
- Event creation and approval process completed within 24 hours
- Code problem selection accurately reflects configured difficulty distribution
- Real-time battle system supports concurrent participants without performance degradation
- Scoring algorithm provides fair and accurate ranking based on solution correctness and speed
- Guild leaderboards accurately aggregate member performance
- System maintains 99.9% uptime during active competitions
- Post-event analytics provide actionable insights for participants and organizers

---

## **Integration Points**

### **Cross-Flow Dependencies**
- **Onboarding → Core AI Loop:** Character creation data and initial document uploads trigger the AI learning loop.
- **Core AI Loop → Assessment:** The generated quest line includes and leads to "Boss Fight" assessments.
- **Assessment → Core AI Loop:** Assessment results feed back into the adaptive learning part of the loop.
- **Social Learning → Guild Management:** Party activities can contribute to Guild analytics and community health.
- **Guild Management → Event Management:** Guild Masters can create competitive events for their communities.
- **Event Management → Assessment:** Code battle competitions provide alternative assessment mechanisms.
- **Event Management → Social Learning:** Competitive events foster guild collaboration and peer learning.

### **Data Flow Architecture**
- **User Profile Service:** Central repository for character data and preferences.
- **AI Processing Pipeline:** Handles all content ingestion and adaptive generation.
- **Real-time Synchronization:** Ensures consistent state across all user interactions.
- **Analytics Engine:** Processes data from all flows for insights and recommendations.
- **Notification Hub:** Coordinates alerts and updates across all system components.

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