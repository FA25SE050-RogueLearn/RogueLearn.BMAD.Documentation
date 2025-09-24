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

## **Flow 2: AI-Powered Quest Generation & Learning Path Creation**

### **Description**
The core AI-driven process that transforms academic syllabi into personalized, gamified learning experiences with dynamic quest lines and skill progression.

### **Actors**
- **Primary:** Student User, AI System
- **Secondary:** Syllabus Processing Service, Quest Generation Engine

### **Process**
1. User uploads course syllabus document (PDF, DOCX)
2. AI System ingests and parses syllabus content
3. AI identifies course structure, topics, and learning objectives
4. AI analyzes user's academic background and career goals
5. AI generates interconnected skill tree from syllabus topics
6. AI creates personalized "main quest line" aligned with course progression
7. AI establishes prerequisites and dependencies between skill nodes
8. System populates quest line with tasks based on uploaded schedules
9. AI continuously monitors progress and adjusts quest difficulty
10. System sends notifications for quest updates and new learning paths

### **Outputs**
- Dynamic, interconnected skill tree visualization
- Personalized main quest line with scheduled tasks
- Skill node prerequisites and dependency mapping
- Automated quest adjustments based on progress
- Notification system for learning path updates

### **Success Criteria**
- Syllabus is accurately parsed within 2 minutes
- Generated skill tree covers 90%+ of syllabus topics
- Quest line aligns with user's academic schedule
- Dynamic adjustments improve learning outcomes

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
The educator-focused system that enables verified lecturers to create educational guilds, monitor student progress, and provide targeted support through data-driven insights.

### **Actors**
- **Primary:** Guild Master (Verified Lecturer)
- **Secondary:** Student Players, Analytics Engine, Content Management System

### **Process**
1. Verified Lecturer creates new Guild with course information
2. Students join Guild and link their academic progress
3. Guild Master uploads reference materials and course resources
4. System aggregates anonymized student progress data
5. Analytics engine identifies struggling topics and performance patterns
6. Dashboard highlights areas where significant percentages struggle
7. Guild Master creates targeted announcements and interventions
8. System tracks effectiveness of educator interventions
9. Guild Master adjusts course materials based on data insights
10. Continuous monitoring provides ongoing feedback loop

### **Outputs**
- Configured Guild with enrolled students
- Anonymized progress analytics and performance insights
- Targeted intervention recommendations
- Shared course materials and resources
- Communication channels for announcements and support

### **Success Criteria**
- Guild setup completed within 15 minutes
- Analytics accurately identify struggling areas within 24 hours
- Educator interventions show measurable improvement in student outcomes
- Student engagement with Guild resources exceeds 70%

---

## **Flow 6: Browser Extension Integration & Content Capture**

### **Description**
The seamless integration system that captures academic content from web sources and automatically organizes it within the user's learning ecosystem.

### **Actors**
- **Primary:** Student User
- **Secondary:** Browser Extension, Content Processing Engine, Arsenal Organization System

### **Process**
1. User browses university portal or academic websites
2. Browser extension automatically detects academic content
3. User highlights relevant text or triggers manual capture
4. Extension extracts and processes academic documents and information
5. Content processing engine categorizes and tags captured information
6. System automatically organizes content into user's Arsenal
7. AI links captured content to relevant skill tree nodes
8. Extension displays contextual Arsenal notes when user highlights text
9. System maintains synchronization across devices and platforms
10. User receives notifications about newly captured and organized content

### **Outputs**
- Automatically captured and organized academic content
- Contextual note display system for highlighted text
- Synchronized Arsenal across all user devices
- AI-powered content categorization and linking
- Seamless integration with existing learning workflows

### **Success Criteria**
- Content capture accuracy exceeds 95% for supported formats
- Automatic categorization correctly classifies 90%+ of content
- Contextual notes display within 2 seconds of text highlighting
- Cross-device synchronization occurs within 30 seconds

---

## **Flow 7: Real-time Progress Tracking & Adaptive Learning**

### **Description**
The continuous monitoring and adaptation system that tracks user progress across all learning activities and dynamically adjusts the learning experience for optimal outcomes.

### **Actors**
- **Primary:** Student User, AI Adaptation Engine
- **Secondary:** Progress Analytics, Notification System, Skill Tree Manager

### **Process**
1. System continuously monitors user interactions across all platform features
2. Progress analytics engine processes learning activity data
3. AI identifies patterns in user performance and engagement
4. System detects areas of struggle or accelerated learning
5. Adaptive engine adjusts quest difficulty and learning paths
6. Skill tree visualization updates to reflect current progress
7. System generates personalized recommendations for improvement
8. Notification system alerts user to important progress milestones
9. AI suggests optimal study schedules based on performance patterns
10. Continuous feedback loop refines personalization algorithms

### **Outputs**
- Real-time progress visualization and analytics
- Dynamically adjusted learning paths and quest difficulty
- Personalized improvement recommendations
- Optimized study schedule suggestions
- Milestone notifications and achievement tracking

### **Success Criteria**
- Progress tracking updates in real-time with <1 second latency
- Adaptive adjustments improve learning outcomes by 25%+
- User engagement with recommendations exceeds 60%
- Personalization accuracy improves over time with usage

---

## **Integration Points**

### **Cross-Flow Dependencies**
- **Onboarding → Quest Generation:** Character creation data feeds into AI quest generation
- **Quest Generation → Assessment:** Generated quests include Boss Fight assessments
- **Assessment → Progress Tracking:** Assessment results update skill tree and progress analytics
- **Social Learning → Guild Management:** Party activities contribute to Guild analytics
- **Browser Extension → All Flows:** Captured content enhances all learning activities
- **Progress Tracking → All Flows:** Adaptive insights influence all user experiences

### **Data Flow Architecture**
- **User Profile Service:** Central repository for character data and preferences
- **AI Processing Pipeline:** Handles syllabus parsing, quest generation, and adaptations
- **Real-time Synchronization:** Ensures consistent state across all user interactions
- **Analytics Engine:** Processes data from all flows for insights and recommendations
- **Notification Hub:** Coordinates alerts and updates across all system components

---

## **Success Metrics**

### **Flow-Specific KPIs**
- **Onboarding Completion Rate:** >85% of users complete character creation
- **Quest Engagement:** Average 5+ quests completed per user per week
- **Assessment Participation:** >90% of users engage with Boss Fight assessments
- **Social Learning Adoption:** >40% of users join or create parties
- **Guild Effectiveness:** Educator interventions improve outcomes by >20%
- **Content Capture Usage:** >60% of users actively use browser extension
- **Adaptation Success:** Personalized adjustments increase retention by >30%

### **Overall Platform Health**
- **Weekly Active Users (WAU):** Primary engagement metric
- **User Retention Rate:** Week-over-week user return rate
- **Learning Outcome Improvement:** Measurable academic performance gains
- **Platform Satisfaction:** Net Promoter Score (NPS) >50
- **System Performance:** <500ms response time for core operations