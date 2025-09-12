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
16. **FR16 (System):** The system must generate gamified mock exams ("Boss Fights") as Unity-based interactive experiences with multiple choice questions, visual feedback, and engaging animations based on upcoming tests input by the player.
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
35. **FR35 (System):** The notification system must support email notifications and push notifications for mobile devices with user-configurable preferences.

### **Functional - Phase 3: Educator & Admin Toolkit**
*Focus: Empower educators and administrators with tools.*

36. **FR36 (Guild Master):** Any user (Student or Verified Lecturer) can create a "Guild."
37. **FR37 (Player):** Players (Students) in a Guild must be able to use Guild Master-uploaded materials to adjust or supplement their personal quest lines.

### **Enhanced User Roles & Verification**

18. **FR18 (Verified Lecturer):** The system must provide a verification process for Guild Masters to become "Verified Lecturers" through academic credential validation, enabling enhanced guild creation privileges and credibility indicators.
19. **FR19 (Enhanced Document Upload):** During character creation or profile management, users must be able to upload academic credentials including transcripts, certifications, and teaching qualifications, with secure storage and verification workflows.
20. **FR20 (Simplified Guild Creation):** Verified Lecturers must be able to create guilds with streamlined processes, basic document sharing capabilities, and simple member management tools focused on educational content delivery.

## **Phase 3: Simplified Educator Toolkit**

21. **FR21 (Guild Master):** A Guild Master must have a basic dashboard to view aggregated, anonymized progress for all Players in their course ("Guild").
22. **FR22 (Guild Master):** The dashboard must highlight topics or 'boss fights' where a significant percentage of the class is struggling.
23. **FR23 (Guild Master):** A Guild Master must be able to upload basic reference materials and share them with guild members.
24. **FR24 (Guild Master):** A Guild Master must be able to create simple announcements and communications for their guild.

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

25. **FR25 (System):** The system must implement basic in-app notifications for quest updates and system announcements.
26. **FR26 (System):** The system must implement structured logging for debugging and basic performance monitoring.
27. **FR27 (System):** The system must implement comprehensive data architecture supporting real-time synchronization and data versioning.
28. **FR28 (System):** The platform must support scalable content delivery with optimized asset loading.



### **Success Metrics (KPIs)**

To measure the success of the RogueLearn MVP, we will track the following Key Performance Indicators (KPIs):

*   **Weekly Active Users (WAU)**: The number of unique users who interact with the platform in a given week. This is a primary indicator of user engagement and retention.
*   **Average Number of Quests Completed per User per Week**: This metric will help us understand how effectively users are engaging with the core functionality of the platform.
*   **Party Creation Rate**: The number of new parties created per week. This will indicate the adoption of the social learning feature.
*   **User Retention Rate**: The percentage of users who return to the platform week after week. This is a critical measure of the platform's value and stickiness.
*   **User Feedback Score**: We will collect user feedback through surveys and other mechanisms to gauge user satisfaction and identify areas for improvement. A Net Promoter Score (NPS) or a similar metric will be used.

### **MVP Validation Approach**

To validate the RogueLearn MVP, we will use a combination of qualitative and quantitative methods:

*   **User Interviews**: We will conduct in-depth interviews with a small group of target users to gather feedback on the platform's usability, features, and overall value proposition.
*   **Surveys**: We will use surveys to collect quantitative data on user satisfaction, feature usage, and other key metrics.
*   **A/B Testing**: We will use A/B testing to compare different versions of the platform and identify the most effective design and features.
*   **Analytics**: We will use analytics tools to track user behavior and identify patterns and trends. This will help us understand how users are interacting with the platform and where we can make improvements.

### Non-Functional Requirements (All Phases)

1.  **NFR1 (Responsiveness):** The application must be a responsive web application, providing an optimal viewing and interaction experience across a range of devices (desktops, tablets, and mobile phones). All functionality must be accessible and usable on screen widths from 320px to 1920px.
2.  **NFR2 (Authentication):** The system must use Clerk for user authentication, ensuring secure and reliable user management.
3.  **NFR3 (Backend Technology):** The system must use .NET 8 and Golang for all backend services, leveraging its performance and security features.
4.  **NFR4 (Services Communication):** System's services must use RabbitMQ to communicate.
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