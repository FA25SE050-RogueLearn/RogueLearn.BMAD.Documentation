# **Goals and Background Context**

### **Goals**

- To launch a viable MVP that demonstrates the core player and collaborative learning loops.
- To create a highly engaging, gamified player experience that increases motivation and reduces exam anxiety by transforming academic milestones into epic "Boss Fights" and learning modules into a visualized, interconnected "Skill Tree."
- To provide a browser extension that seamlessly integrates with the player's digital environment, automatically capturing and organizing learning materials from across the web into a personal "Arsenal."
- To establish a clear, AI-driven learning path for students that aligns their academic curriculum (Route) with their career specialization from roadmap.sh (Class) through intelligent gap analysis and supplementary quest generation.
- To empower students to enhance their learning journey by uploading achievement documents and project portfolios, which will dynamically influence their skill tree visualization and personal arsenal management.
- To allow students to select their curriculum ("Route") and career specialization ("Class") to generate a curriculum-based main quest line supplemented with industry-relevant content from roadmap.sh.
- To enable peer-to-peer collaborative study through an intuitive "Party" system.
 - Post-MVP: Validate a multi-tiered subscription model for future monetization (explicitly excluded from MVP scope).

### **Background Context**

University students often face significant challenges with motivation and connecting their current studies to future career goals. The lack of a clear, engaging pathway can lead to procrastination and anxiety. RogueLearn addresses this by transforming the educational journey into an interactive RPG. It allows players to choose their academic curriculum as their "Route" (foundational quest line) and select a career specialization from roadmap.sh as their "Class" (supplementary career-focused content). The system provides a structured, AI-powered curriculum-based "main quest line" where exams become "Boss Fights," and knowledge acquisition is visualized as a "Skill Tree." Players can enhance their experience by uploading achievement documents and project portfolios to personalize their skill tree and arsenal management. The AI performs intelligent gap analysis between curriculum and career requirements, generating supplementary quests to bridge academic learning with industry needs. This shows players exactly how their learning connects and contributes to their ultimate career goal. A companion browser extension further enhances this by automatically capturing relevant learning materials from the web, organizing them into the player's "Arsenal." This turns passive learning into an active and meaningful adventure, making studying more effective and building a foundation for a rich academic ecosystem involving lecturers, tutors, and peer groups.

### **Business Opportunity**

The student learning market is crowded with single-purpose tools—note-taking apps, course platforms, and social communities—that fragment the learning experience across motivation, organization, and progress tracking. RogueLearn creates a cohesive, engaging layer on top of the existing academic environment by fusing gamified progression (Route, Class, Boss Fights, Skill Tree) with personal knowledge management (Arsenal) and lightweight collaboration (Party).

Environment of use:
- Individual university students, bootcamp learners, and self-study groups using a web application and companion browser extension.
- Campus and remote/hybrid learning contexts that require cross-device continuity and privacy-aware sharing.

Comparative evaluation (see "Existing Systems"):
- Notion/Obsidian/Roam: Excellent personal knowledge management, but lack curriculum-to-career mapping, gamified progression, and cross-service learning loops.
- Duolingo/Khan Academy/Codecademy: Strong progression and learning paths, but focus on proprietary content rather than integrating a student's own curriculum and captured materials.
- Discord and study communities: High engagement socially, but limited structure for mastery tracking, shared study assets, and formalized quests.

Why RogueLearn is attractive:
- Integrates a student's curriculum (Route) and chosen specialization (Class) into a unified, visual progression with clear milestones and feedback loops.
- Connects personal notes (Arsenal) directly to skill nodes and quests, turning captured materials into actionable learning steps.
- Enables collaborative study via Party Stash while preserving data ownership and privacy boundaries.
- Provides AI-driven gap analysis between academic requirements and career demands to generate supplementary quests.

Problems not well solved today without RogueLearn:
- Connecting university coursework to real career paths in a personalized, visual, and motivating way.
- Turning fragmented notes and resources into structured learning artifacts linked to progression and assessments.
- Lightweight, privacy-respecting collaboration that shares study assets without losing ownership or exposing personal data.
- Seamless capture from the browser into a structured Arsenal tied to skills and quests, rather than generic bookmarks.

Fit with market trends and strategic direction:
- AI-assisted personalization, micro-learning, and dynamic learning paths.
- Gamification as a proven engagement lever, applied thoughtfully to academic mastery rather than only habit loops.
- Privacy-first architectures with clear RBAC and service boundaries (aligned to Supabase Auth/Storage and per-service databases).
- Interoperability-first design that can integrate with campus systems and external content sources.

### **Software Product Vision**

For university students who want a clear, motivating way to plan and conquer their academic journey while connecting it to real career goals, RogueLearn is a gamified, AI-assisted learning platform that transforms curricula into quest lines, exams into Boss Fights, and knowledge into a visual Skill Tree. It unifies personal notes captured from the web (Arsenal), structured progression (Route and Class), and lightweight collaboration (Party) into a single experience across web app and browser extension.

Unlike generic note-taking tools, standalone course platforms, or unstructured social communities, RogueLearn integrates a student's own curriculum and captured materials into actionable quests with mastery tracking and AI-driven guidance. This reduces anxiety, increases engagement, and turns everyday study into a meaningful adventure—grounded in practical architectures, privacy-by-design, and an MVP-first scope that prioritizes core loops before advanced collaboration and analytics.

### **User Research & Validation Framework**

#### **Target User Validation**
To ensure RogueLearn addresses real user needs, we will conduct comprehensive user research before and during development:

**Pre-Development Research (Required before Phase 1):**
- **User Interviews:** Conduct 8-12 structured interviews with university students across different academic disciplines
- **Problem Validation:** Validate core assumptions about player motivation challenges and learning preferences
- **Solution Validation:** Test initial concept reception and gather feedback on gamification approach
- **Competitive Analysis:** Interview users about current study tools and identify gaps in existing solutions

**Research Methodology:**
1. **Participant Recruitment:** Target students aged 18-25 across STEM and non-STEM disciplines
2. **Interview Structure:** 45-60 minute sessions covering current study habits, motivation challenges, and technology preferences
3. **Key Research Questions:**
   - How do students currently organize and track their learning progress?
   - What are the primary sources of academic stress and motivation loss?
   - How do students connect current coursework to career goals?
   - What role does social learning play in their academic success?
   - How do they currently use digital tools for studying?

**Validation Criteria:**
- **Problem-Solution Fit:** 70%+ of interviewed students express strong interest in gamified learning approach
- **Feature Prioritization:** Validate which core features (skill tree, quest system, social features) resonate most
- **Usability Preferences:** Understand preferred interaction patterns and UI expectations
 - **Pricing Sensitivity (Post-MVP):** Gauge willingness to pay for premium features (for post-MVP planning; not an MVP validation goal)

**Ongoing Validation (During Development):**
- **Weekly User Testing:** Test core features with 3-5 users every 2 weeks during development
- **A/B Testing Framework:** Test different approaches to key interactions (onboarding, quest completion, social features)
- **Analytics Integration:** Track user behavior patterns and engagement metrics from day one
- **Feedback Loops:** Implement in-app feedback collection and regular user surveys

**Success Metrics for Validation:**
- **User Interview Completion:** Complete 8+ user interviews before development begins
- **Concept Validation:** Achieve 70%+ positive response to core gamification concept
- **Feature Validation:** Identify top 3 most valuable features through user feedback
- **Usability Validation:** Achieve 80%+ task completion rate in early prototype testing

#### **Risk Mitigation Through Research**
- **Assumption Documentation:** Clearly document all assumptions about user behavior and preferences
- **Pivot Criteria:** Define specific metrics that would trigger feature pivots or scope changes
- **Continuous Learning:** Establish regular research cadence to validate new features and improvements
- **User Advisory Board:** Recruit 3-5 engaged users to provide ongoing feedback throughout development

### **Existing Systems**

[Add the systems which might help solve the problems listed above or the systems you can learn/refer features for the current system design]

#### **Notion**
- Description: All-in-one workspace with a rich block editor, databases, and templates for organizing knowledge.
- Link: https://www.notion.so/
- System Actors: Individual students, teams.
- Key Features: Rich blocks, relations, tags, powerful search, web clipper.
- Pros: Polished UX, flexible organization, quick onboarding.
- Cons: Proprietary, limited offline-first behavior, complex advanced features.
- Learnings/Adoption (MVP): Use Notion-like editor patterns; implement CRUD, tagging, filters, and linking to skills/quests (FR14, FR15).

#### **Obsidian**
- Description: Local markdown-based knowledge base with bidirectional links and graph view.
- Link: https://obsidian.md/
- System Actors: Individuals.
- Key Features: Markdown notes, backlinks, graph visualization, plugin ecosystem.
- Pros: Offline-first, extensible, user ownership of data.
- Cons: Not collaborative by default, local-first sync complexity.
- Learnings/Adoption (MVP): Adopt tagging and backlinks concepts; reserve graph visualization for post-MVP.

#### **Roam Research**
- Description: Networked thought note-taking emphasizing bidirectional linking and daily notes.
- Link: https://roamresearch.com/
- System Actors: Individuals.
- Key Features: Backlinks-first workflow, graph of ideas, blocks referencing.
- Pros: Excellent idea linking, deep knowledge graphs.
- Cons: Subscription cost, proprietary, steeper learning curve.
- Learnings/Adoption (MVP): Consider backlinks as a future enhancement beyond basic tags.

#### **Outline (Open-source)**
- Description: Open-source team knowledge base for documentation and wiki-style content.
- Link: https://www.getoutline.com/
- System Actors: Teams.
- Key Features: Hierarchical docs, search, collections, access control.
- Pros: Self-hosted, open-source, mature architecture.
- Cons: Operational overhead, not tailored for personal study notes UX.
- Learnings/Adoption (MVP): Reference architectural patterns for knowledge bases and access control.

#### **Joplin (Open-source)**
- Description: Open-source note-taking app with sync, tagging, and web clipper.
- Link: https://joplinapp.org/
- System Actors: Individuals.
- Key Features: Markdown, tags, notebooks, clipper, encryption.
- Pros: Open-source, privacy-friendly, cross-platform.
- Cons: Less polished UI than commercial tools.
- Learnings/Adoption (MVP): Use clipper/tagging flows to guide Arsenal capture (FR23, FR24).

#### **Duolingo**
- Description: Gamified language learning platform with strong progression loops.
- Link: https://www.duolingo.com/
- System Actors: Learners.
- Key Features: XP, streaks, leagues, bite-sized lessons.
- Pros: Highly engaging gamification, clear progression.
- Cons: Gamification can overshadow depth, not academic-focused.
- Learnings/Adoption (MVP): Minimal gamification (XP, achievements) to support motivation; avoid heavy mechanics.

#### **Khan Academy**
- Description: Free educational content with mastery-based progression.
- Link: https://www.khanacademy.org/
- System Actors: Students, teachers.
- Key Features: Mastery tracking, practice exercises, structured courses.
- Pros: Clear mastery model, pedagogically sound.
- Cons: Less game-like engagement, limited social loops.
- Learnings/Adoption (MVP): Use mastery/progression clarity to inform Skill Catalog and achievements.

#### **Codecademy**
- Description: Interactive coding education with sequenced lessons and projects.
- Link: https://www.codecademy.com/
- System Actors: Learners.
- Key Features: Step-based modules, hints, quizzes, paths.
- Pros: Clear sequencing, interactive steps.
- Cons: Proprietary content and platform constraints.
- Learnings/Adoption (MVP): Map quests to step-based flows and learning paths in Quests Service.

#### **Discord**
- Description: Real-time community and collaboration platform.
- Link: https://discord.com/
- System Actors: Parties, guilds, communities.
- Key Features: Channels, roles, threads, reactions, voice.
- Pros: Strong social engagement, flexible permissions.
- Cons: Not purpose-built for study notes, potential noise.
- Learnings/Adoption (MVP): Inform Party/Guild roles, permissions, and lightweight collaboration.

#### **Notion Web Clipper**
- Description: Browser extension for saving web content to Notion.
- Link: https://www.notion.so/web-clipper
- System Actors: Individuals.
- Key Features: Quick-save, tagging, URL/title metadata capture.
- Pros: Simple capture UX, reliable.
- Cons: Limited processing/enrichment out of the box.
- Learnings/Adoption (MVP): Design Arsenal capture with highlight → tag → save flow; add offline queue/retry.

#### **Otter.ai**
- Description: AI-powered meeting transcription and summarization.
- Link: https://otter.ai/
- System Actors: Study groups, classes.
- Key Features: Transcripts, summaries, speaker detection.
- Pros: High-quality transcription and summaries.
- Cons: Cost, privacy concerns for sensitive meetings.
- Learnings/Adoption (MVP): Start with transcript storage + basic summary; defer advanced analytics.

#### **LeetCode / Codeforces**
- Description: Competitive programming platforms for practice and contests.
- Links: https://leetcode.com/ / https://codeforces.com/
- System Actors: Competitors, learners.
- Key Features: Timed challenges, scoring, leaderboards.
- Pros: Strong engagement, clear difficulty progression.
- Cons: Competitive focus may not fit curriculum; fairness concerns.
- Learnings/Adoption (MVP): Implement scoring and basic leaderboards; expand events later.

#### **Supabase (Auth & Storage)**
- Description: Backend-as-a-service with PostgreSQL, Auth, Storage.
- Link: https://supabase.com/
- System Actors: Platform developers, end users via apps.
- Key Features: JWT auth, RBAC, email templates, storage buckets, presigned uploads.
- Pros: Fast integration, modern tooling, Postgres-native.
- Cons: Vendor dependency, quotas on free tiers.
- Learnings/Adoption (MVP): Use Supabase Auth/Storage per architecture; enforce consistent RBAC across services (see Story 1.2).

### **Change Log**

| **Date** | **Version** | **Description** | **Author** |
| --- | --- | --- | --- |
| *Initial Draft* | 1.0 | Initial PRD creation, expanded to include Phases 1, 2, & 3. | John, PM |
| September 10, 2025 | 1.1 | Refined scope to focus on Software Engineering students. Removed "Guide" role and clarified "Lecturer" into a "Verified" status for the "Guild Master" role. Generalized competitions to an "Events" system. | John, PM |
| *Current* | 2.0 | **MENTOR-DRIVEN PIVOT:** Significantly simplified scope based on mentor feedback. Removed Phases 4-5 (60% of features) and streamlined Phase 3 to focus on core MVP validation. Enhanced Verified Lecturer system while maintaining gamified learning focus. | John, PM |
| January 2025 | 2.1 | **USER RESEARCH ENHANCEMENT:** Added comprehensive user research and validation framework to address validation gaps identified in PM checklist review. | John, PM |