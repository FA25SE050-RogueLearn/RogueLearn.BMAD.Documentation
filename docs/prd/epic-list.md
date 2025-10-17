# **Epic List**

## **Phase 1: Core Player MVP**
*Focus: Establish the fundamental single-player experience.*

### **Epic: Onboarding & Academic Data**
*Goal: Seamless entry into a curriculum-aligned journey with optional academic data enrichment.*
*   **FR1:** Account creation with 3-step "Character Creation" (Route selection, Class selection, Skill-based roadmap selection).
*   **FR2:** Optional academic document upload with FPTU verification to enrich skill tree and Arsenal features.
*   **FR3:** System maintains comprehensive predefined curriculum/syllabus database with optional document intake for personalization.

### **Epic: AI Curriculum & Career Alignment**
*Goal: Transform selected curriculum into a clear academic route and bridge to career specialization.*
*   **FR4:** AI analyzes selected curriculum to identify academic route and structure primary quest line.
*   **FR4A:** AI gap analysis versus roadmap.sh specialization, generating supplementary quests.

### **Epic: Skill & Stats Foundation**
*Goal: Establish core AI-driven skills, stats, and quest scaffolding from academic data.*
*   **FR5:** AI calculates user skill points (1–10 scale) aligned to FPTU scoring.
*   **FR6:** AI suggests related classes aligned to selected route (smaller major).
*   **FR7:** Initial character stats influenced by uploaded academic data (GPA, transcripts, schedules).
*   **FR8:** AI populates skill tree with acquired knowledge based on academic data.
*   **FR9:** AI automatically creates quest lines organized into chapters (semesters) with FPTU calendar integration.

### **Epic: Dashboard, Skill Tree & Arsenal**
*Goal: Visualize progress, manage knowledge, and link notes to skills.*
*   **FR10:** Personal dashboard ("Character Sheet") showing progress, stats, dynamic skill tree, and generated quest line.
*   **FR11:** Skill tree visualized as an interconnected mind map.
*   **FR12:** Skill nodes display skill level, linked Arsenal notes, and completed/upcoming quests.
*   **FR13:** Visualization highlights relationships and missing skills needed to reach goals.
*   **FR14:** Arsenal rich text editor with Notion-like functionality.
*   **FR15:** Link skill tree nodes to Arsenal notes with visual indicators.

### **Epic: Boss Fights & Leaderboards**
*Goal: Make learning engaging and measurable through Unity-based challenges.*
*   **FR16:** Gamified mock exams (Unity WebGL Boss Fights) with MCQs, feedback, and animations.
*   **FR17:** Difficulty levels per question award more points and affect mechanics.
*   **FR18:** Total Boss Fight score calculated and contributes to skill progression.
*   **FR19:** Comprehensive leaderboards (class-specific, events, overall platform).

### **Epic: Dynamic Quest & Notifications**
*Goal: Keep the quest line adaptive and users informed.*
*   **FR20:** Dynamic main quest line with AI-driven changes to uncompleted tasks.
*   **FR21:** Notification system for quest changes and suggested learning paths.

### **Epic: Browser Extension Integration (FPTU & Arsenal)**
*Goal: Capture and organize web/portal content to drive learning automation.*
*   **FR22:** Browser extension with comprehensive FPTU portal integration to scan, extract, and synchronize academic documents.
*   **FR23:** Automatic organization of extracted FPTU/web content into Arsenal with intelligent categorization.
*   **FR24:** Contextual popup on text highlight showing relevant notes and quest actions.

---

## **Phase 2: Social & Extension MVP**
*Focus: Introduce multiplayer social learning and collaborative workflows.*

### **Epic: Party & Collaboration (Study Groups)**
*Goal: Turn learning into a collaborative, team-based effort.*
*   **FR25:** Create and manage a Party with configurable rules and permissions.
*   **FR26:** Invite friends directly when creating a Party.
*   **FR27:** Browse other users, view relevant stats, and send invitations.
*   **FR28:** Set a Party to open for anyone to join.
*   **FR29:** Invite registered users and assign granular member permissions.
*   **FR30:** Shared resource space ("Party Stash") with role-based permissions (view/edit/comment).
*   **FR31:** Party Leader configures per-member permissions in Party Stash.

### **Epic: Meetings & Summaries**
*Goal: Coordinate study sessions and capture outcomes.*
*   **FR32:** Schedule, organize, and manage study meetings (titles, descriptions, types, scheduling).
*   **FR33:** Built-in meeting recording capabilities (manual entry, audio transcription, automated capture). Browser extension is for web content capture only.
*   **FR34:** AI-generated meeting summaries with key points, action items, next steps, and resources.

### **Epic: Notifications (Platform)**
*   **FR35:** Email and mobile push notifications with user-configurable preferences.

### **Epic: Boss Fight (Co-op & Frontend Integration)**
*Goal: Cooperative Boss Fights with robust networking and UI integration.*
*   **FR35A:** Start/manage co-op Boss Fight sessions with configurable rules.
*   **FR35B:** Implement question-driven 2D gameplay loop with Answer Stations, Team Charges, Power Play windows, and real-time scoring/leaderboards.
*   **FR35C:** Unity Netcode for GameObjects with Unity Relay (WSS), join-code flow, server-authoritative validation.
*   **FR35D:** Next.js embeds Unity WebGL module with JS ↔ Unity bridge for session control, auth, and result reporting.

---

## **Phase 3: Simplified Educator Toolkit & Competition Platform**

### **Epic: Basic Guild Management (Verified Lecturer Gated)**
*Goal: Provide essential tools for guild creation, materials sharing, and progress tracking.*
*   **FR36:** Create a Guild (Players or Verified Lecturers). Governance (elective curation, curriculum imports) is owned by Game Master.
*   **FR37:** Players use Guild Master-uploaded materials to adjust/supplement personal quest lines.
*   **FR38:** Guild Master dashboard for aggregated, anonymized course progress.
*   **FR39:** Basic in-app notifications for quest updates and announcements.

### **Epic: Code Battle & Competitive Events**
*Goal: Enable real-time coding challenges and guild-based competitions.*
*   **FR43:** Real-time code battle execution environment (multi-language, automated testing, scoring, spectator mode).
*   **FR44:** Guild-based competitive events with room assignment, ranking calculations, and lifecycle management.
*   **FR45:** Advanced social learning mechanics within competitive contexts (peer challenges, collaboration, mentorship).

### **Epic: Event Management & Administration Platform**
*Goal: Tools to create, schedule, administer, and approve events with Game Master oversight.*
*   **FR46:** Comprehensive event creation and management tools for educators/guild masters.
*   **FR47:** Streamlined approval workflow with Game Master administrative authority.

### **Epic: Academic Integration (FPTU, Quest Memory & Recovery)**
*Goal: Robust academic verification and continuity across semesters.*
*   **FR48:** FPTU student verification via document analysis and portal integration.
*   **FR49:** Quest memory and history tracking for cross-semester continuity.
*   **FR51:** Adaptive quest generation for failed students with recovery pathways.

### **Epic: Admin-Owned Educational Governance**
*Goal: Centralized governance for elective content and official curricula.*
*   **FR52:** Elective Library curation and approval (multi-source intake, audit-logged workflows, tagging/versioning, controlled publication).
*   **FR53:** University Curriculum import and administration (manual JSON import in MVP), version management, mappings, and reporting.

### **Epic: Objective System, Knowledge Graph & Rewards**
*Goal: Define objectives, verify projects, and drive progression through contextual rewards.*
*   **FR54:** Objective Categorization.
*   **FR55:** Objective Completion Criteria (Adjusted).
*   **FR56:** Automated Project Verification.
*   **FR57:** Skill Knowledge Graph.
*   **FR58:** Reward Cascade.
*   **FR59:** Contextual Unlocks.
*   **FR60:** Workspace-Specific Interfaces.

*Note: Advanced educator features (AI-assisted quest generation, complex analytics, and comprehensive Game Master tools) are post-MVP to keep focus on core player validation.*

---

## **Phase 4: Enhanced Browser Extension Integration**
*Focus: Advanced portal integration and content management.*

### **Epic: Enhanced Browser Extension Integration**
*   **FR50:** Comprehensive FPTU portal integration for automated academic data extraction and quest synchronization.

---

## **Future Phases: Post-MVP Expansion**

### **Phase 5: Marketplace & Economy** (Future)
- User-driven marketplace for learning resources
- Creator economy and monetization features
- AI-powered content curation and recommendations
- Virtual currency and trading systems

*These features will be considered for future development phases after successful MVP validation and user feedback.*