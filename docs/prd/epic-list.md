# **Epic List**

## **Phase 1: Core Player MVP**
*Focus: Establish the fundamental single-player experience.*

### **Epic: User Onboarding & Profile Management**
*Goal: Create a seamless and engaging entry point for new users, allowing them to set up their curriculum-based learning journey and career specialization.*
*   **FR1:** User account creation and curriculum-based onboarding ("Character Creation") with Route (Curriculum) and Class (Roadmap.sh) selection.
*   **FR2:** Enhanced academic document upload with FPTU student verification and academic calendar integration for verified students.
*   **FR3:** Personal dashboard ("Character Sheet") displaying curriculum progress and career specialization alignment.
*   **FR48:** FPTU Student Verification System for enhanced academic integration and personalized quest generation.

### **Epic: Core AI & Quest Generation**
*Goal: Leverage AI to transform curriculum content into personalized learning paths and bridge academic-industry gaps through roadmap.sh integration.*
*   **FR4:** Curriculum analysis and academic route identification for primary quest line generation.
*   **FR4A:** Gap analysis between curriculum and roadmap.sh career specialization with supplementary quest generation.
*   **FR5:** AI-powered curriculum-based "main quest line" generation following academic progression.
*   **FR6:** AI analysis of curriculum structure, prerequisites, and learning objectives.
*   **FR7:** AI generation of skill tree from curriculum topics and career specialization requirements.
*   **FR8:** AI influence on character stats from academic performance data.
*   **FR9:** Enhanced AI creation of integrated learning paths with FPTU-specific quest memory (FR49) and adaptive generation for failed students (FR51).
*   **FR10:** Dynamic, AI-driven updates to quest lines based on curriculum changes and industry trends.
*   **FR11:** Notification system for quest changes, curriculum updates, and new supplementary learning paths.
*   **FR49:** Quest Memory & Continuity System for maintaining learning progress across semesters.
*   **FR51:** Adaptive Quest Generation for Failed Students with personalized recovery pathways.

### **Epic: Core Business Logic & Scoring Systems**
*Goal: Implement the fundamental business rules, algorithms, and scoring mechanisms that drive the gamification experience.*

*   **FR3:** Skill point calculation algorithm: `skill_points = (difficulty_level × base_points × performance_multiplier)`
*   **FR6:** XP conversion system: `xp_gained = skill_points × 10`
*   **FR7:** Boss Fight scoring algorithm: `score = (correct_answers / total_questions) × time_bonus × difficulty_multiplier`
*   **FR8:** Dynamic difficulty adjustment based on performance patterns
*   **FR9:** Leaderboard ranking algorithms (weekly/monthly/category-specific)
*   **FR10:** Achievement and milestone calculation systems

### **Epic: Skill Tree & Knowledge Management**
*Goal: Provide tools for students to organize their knowledge, visualize academic progress, and manage personal learning resources enhanced by user documents.*
*   **FR12:** Dynamic and interconnected skill tree based on curriculum structure and career specialization.
*   **FR13:** Rich text editor for study notes and personal learning resources ("Arsenal").
*   **FR14:** Linking skill tree nodes to notes in "Arsenal" with user document integration.
*   **FR15:** Skill tree node completion tracking enhanced by uploaded achievement documents.
*   **FR16:** Prerequisites and dependencies between skill nodes following curriculum progression.
*   **FR17:** Visual progress indicators for skill mastery with personalized achievements from user documents.

### **Epic: Gamification & Assessment**
*Goal: Make learning engaging and measurable through Unity-based interactive challenges and progress tracking.*
*   **FR18:** Gamified mock exams ("Boss Fights").
*   **FR19:** Leaderboards.
*   **FR20:** Simplified Guild Creation for Verified Lecturers.
*   **FR21:** Guild Master basic dashboard.
*   **FR22:** Guild Master dashboard highlights struggling topics.
*   **FR23:** Guild Master upload basic reference materials.
*   **FR24:** Guild Master create simple announcements.

---

## **Phase 2: Social & Extension MVP**
*Focus: Introduce multiplayer and external integration features with enhanced FPTU capabilities.*

### **Epic: Social & Collaboration (Parties)**
*Goal: Transform learning from an isolated activity into a collaborative, team-based effort.*

*   **FR21:** Party creation and management system (2-8 members per party)
*   **FR22:** Party leadership and role management with transfer protocols
*   **FR23:** Shared resource space ("Party Stash") with permission controls
*   **FR24:** Meeting scheduling and organization tools
*   **FR25:** AI-powered meeting summaries and action item extraction
*   **FR26:** Collaborative study session coordination

### **Epic: Browser Extension**
*   Enhanced FPTU portal integration with real-time academic data synchronization (FR50).
*   Scan and extract academic documents from web pages.
*   Automatic organization of extracted information into "Arsenal".
*   Contextual note display on text highlight.
*   Real-time academic calendar and grade monitoring for FPTU students.

---

## **Phase 3: Simplified Educator Toolkit & Competition Platform**

### **Epic 3.1: Basic Guild Management**
**Functional Requirements:** FR21-FR24
**Description:** Essential tools for Guild Masters to create and manage educational guilds with basic progress tracking, simple document sharing, and communication features. Focus on core educational value without complex administrative overhead.

### **Epic 3.2: Verified Lecturer System**
**Functional Requirements:** FR18, FR19, FR20
**Description:** Academic credential verification system that enables enhanced guild creation privileges and credibility indicators for qualified educators, maintaining educational quality while keeping the system simple and focused.

### **Epic 3: Advanced Social Learning & Code Battle Competition**
**Functional Requirements:** FR19 (Enhanced), FR45, FR46, FR47
**Description:** Comprehensive competitive learning platform featuring real-time code battles, guild-based events, and advanced social learning mechanics. Includes live coding competitions, peer-to-peer challenges, leaderboard systems, and collaborative problem-solving environments that transform learning into engaging competitive experiences.

**Key Features:**
*   Real-time code battle execution and judging system
*   Guild-based competitive events with room assignment systems
*   Live peer-to-peer coding challenges and knowledge duels
*   Advanced party management with competitive coordination
*   Global and guild-specific leaderboards with ranking systems
*   Collaborative problem-solving and team-based competitions
*   Real-time spectator mode for educational observation
*   Performance analytics and skill progression tracking

### **Epic 3: Event Management & Administration Platform**
**Functional Requirements:** FR48, FR49
**Description:** Comprehensive event creation and management system enabling educators and guild masters to organize, schedule, and administer competitive learning events. Provides Game Master administrative oversight with highest authority, streamlined approval workflows, and event lifecycle management for maintaining educational quality and platform integrity.

**Key Features:**
*   Event creation wizard with templates and customization options
*   Streamlined approval workflow (Guild Master → Game Master) with Game Master highest authority
*   Event scheduling and calendar integration
*   Participant registration and capacity management
*   Event monitoring and real-time administration tools
*   Post-event analytics and performance reporting
*   Event archival and historical tracking
*   Game Master administrative dashboard for platform oversight

*Note: Advanced educator features (AI-assisted quest generation, complex analytics, skill tree overlays, and comprehensive game master tools) have been moved to post-MVP to focus on core player experience validation.*

---

## **Phase 4: Enhanced Browser Extension Integration**
*Focus: Advanced web integration and content management.*

### **Epic: Enhanced Browser Extension Integration**
*   Web content extraction and organization
*   Contextual learning assistance during browsing
*   Seamless integration with Arsenal knowledge base
*   Cross-platform synchronization
*   Advanced web page analysis and content categorization
*   Smart content recommendations based on study goals

---

## **Future Phases: Post-MVP Expansion**

### **Phase 5: Marketplace & Economy** (Future)
- User-driven marketplace for learning resources
- Creator economy and monetization features
- AI-powered content curation and recommendations
- Virtual currency and trading systems

*These features will be considered for future development phases after successful MVP validation and user feedback.*
### **Epic 3.3: Admin-Owned Educational Governance**
**Functional Requirements:** FR52, FR53
**Description:** Centralized educational governance owned by Game Master (System Admin) to ensure quality and consistency across elective content and official curriculum. Includes curated elective library management and university curriculum import/administration with strict RBAC-controlled publication.

**Key Features:**
* Elective library curation and approval workflow (draft → review → approved → published)
* Tagging, versioning, and visibility controls aligned to skill nodes
* Curated elective packs with institutional/public visibility options
* University curriculum import with mapping, validation, and version activation
* Scheduled imports, rollback capabilities, and audit-logged governance actions
* Read-only reporting for Guild Masters; publication authority restricted to Admin