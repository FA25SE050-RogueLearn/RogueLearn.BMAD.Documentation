# 1. Overall Description

## 1.1 Product Overview
> Gives the overall description about the product with some introduction and the context diagram. The context diagram presents the boundary and connections between the system you’re developing and everything else. This identifies external entities (terminators) outside the system that interface to it in some way, as well as data, control, and material flows between the terminators and the system.

RogueLearn is a gamified learning platform that aligns university curriculum with career roadmaps. It converts academic progress into quests, skill trees, and boss fights, supporting students with AI-driven personalization and integrations (e.g., FPTU portal, roadmap.sh).

Context Diagram (Mermaid)
```mermaid
flowchart LR
    subgraph RogueLearn System
      UI[Web/App UI]
      Auth[Authentication]
      Quest[Quest Generator]
      Skill[Skill Tree Service]
      Notes[Arsenal/Notes]
      Exam[Boss Fight (WebGL)]
      DB[(Database)]
      UI --> Auth
      Auth --> Quest
      Auth --> Skill
      Auth --> Notes
      Quest --> DB
      Skill --> DB
      Notes --> DB
      Exam --> DB
    end

    Student[Student User] -->|Login/Use| UI
    Instructor[Instructor/Advisor] -->|Review/Feedback| UI
    Admin[Administrator] -->|Manage/Configure| UI

    FPTU[FPTU Portal/API] -->|Academic Data| Quest
    Roadmap[roadmap.sh API] -->|Career Data| Quest
    GitHub[GitHub OAuth/API] -->|Project Links| Notes
    Analytics[Telemetry/Monitoring] -->|Usage Metrics| DB
```

## 1.2 Business Rules
> Key business rules derived from PRD requirements.md. Each rule references its authoritative PRD FR source.

| BR-ID | Rule | Source (PRD FR) | Notes |
|-------|------|------------------|-------|
| BR-001 | Character Creation (3-step) is mandatory before unlocking the dashboard: 1) Route (Curriculum), 2) Class (Roadmap.sh specialization), 3) Skill-based roadmap generation. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) | Curriculum-first approach; roadmap.sh content supplements gaps. |
| BR-002 | Academic document upload is optional and influences skill tree visualization and Arsenal personalization; verified FPTU documents impact quest generation and calendar integration. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data), [Dashboard, Skill Tree & Arsenal](../prd/requirements.md#dashboard-skill-tree--arsenal) | Supported docs include transcripts, schedules, ID; enables FPTU verification flows. |
| BR-003 | System maintains predefined curriculum/syllabus database for supported routes/subjects. | [Admin & Content Curation](../prd/requirements.md#admin--content-curation) | Authoritative source for quest objective generation and skill mapping. |
| BR-004 | Primary quest line is generated directly from curriculum analysis and organized by semester; integrates the FPTU academic calendar. | [AI Curriculum & Career Alignment](../prd/requirements.md#ai-curriculum--career-alignment) | Confidence scores; prerequisite-based unlocking; 10 quests per 4-month semester. |
| BR-005 | AI gap analysis generates supplementary quests to bridge curriculum with selected career path (roadmap.sh). | [AI Curriculum & Career Alignment](../prd/requirements.md#ai-curriculum--career-alignment) | Quarterly refresh; confidence thresholds ≥85%/≥90%. |
| BR-006 | Initial stats baseline and ongoing adjustments are derived from GPA/transcript analysis. | [Skill & Stats Foundation](../prd/requirements.md#skill--stats-foundation) | GPA→base stat formula; grade-weighted contributions to skill categories. |
| BR-007 | Skill tree is populated and leveled from academic data; shows prerequisites/dependencies; supports knowledge decay and cross-skill synergies. | [Dashboard, Skill Tree & Arsenal](../prd/requirements.md#dashboard-skill-tree--arsenal) | Tiered nodes; force-directed layout; decay 10%/semester after 12 months; +5% synergy. |
| BR-008 | Arsenal is a Notion-like workspace; notes link to skill nodes and display contribution indicators. | [Dashboard, Skill Tree & Arsenal](../prd/requirements.md#dashboard-skill-tree--arsenal) | Rich-text editor; bidirectional links between skills and notes. |
| BR-009 | Boss Fights are Unity WebGL exams with difficulty tiers; scores convert to skill XP using defined distribution rules. | [Boss Fights & Leaderboards](../prd/requirements.md#boss-fights--leaderboards) | Difficulty scaling, time bonus, XP distribution 60/25/15 across related skills. |
| BR-010 | Leaderboards include global/class/major/guild/event categories; real-time updates for active events, batch for quests/skills; seasonal resets. | [Boss Fights & Leaderboards](../prd/requirements.md#boss-fights--leaderboards) | Update targets: ≤5s during events; weekly social, daily quest/skill. |
| BR-011 | Main quest line is dynamically adjustable based on performance, schedule, preferences, and academic updates, with 24-hour rollback. | [Dynamic Quest & Notifications](../prd/requirements.md#dynamic-quest--notifications) | Active vs pending modification rules; immutable completed quests. |
| BR-012 | Browser extension organizes academic/web content into Arsenal and suggests context-aware actions; integrates with FPTU portal for real-time sync. | [Browser Extension Integration (FPTU & Arsenal)](../prd/requirements.md#browser-extension-integration-fptu--arsenal) | Extraction, categorization, micro-quests, deadline sync, grade notifications. |
| BR-013 | Co-op Boss Fight sessions follow allowed configuration ranges and server-authoritative rules; React Unity WebGL bridge enables session control and reporting. | [Boss Fight (Co-op & Frontend Integration)](../prd/requirements.md#boss-fight-co-op--frontend-integration) | NGO + Relay join code; JS↔Unity events; latency tolerance 100–200 ms; graceful fallback. |
| BR-014 | Guilds can be created by Players or Verified Lecturers; Verified Lecturers get enhanced analytics/quest tools; Admin retains publishing/governance. | [Basic Guild Management (Verified Lecturer Gated)](../prd/epic-list.md#basic-guild-management-verified-lecturer-gated) | Creation requirements, size tiers, dashboards, announcements; access restrictions. |
| BR-015 | Competitive event platform includes Code Arena and Guild Events with event wizard, room assignment, scoring, and analytics. | [Event Platform (Code Arena & Guild Events)](../prd/requirements.md#event-platform-code-arena--guild-events) | Supported languages; sandbox limits; guild ranking by total points; templates and rules. |
| BR-016 | Objective types map to workspace UIs; completion criteria vary by type; automated project verification; knowledge graph drives micro-objectives; reward cascade and contextual unlocks. | [Objective System, Knowledge Graph & Rewards](../prd/requirements.md#objective-system-knowledge-graph--rewards) | Mission Control, Code Arena, verification score, event pipeline and unlock banners. |
| BR-017 | Quest memory preserves continuity across semesters; failed courses trigger adaptive recovery quests. | [Academic Integration (FPTU, Quest Memory & Recovery)](../prd/requirements.md#academic-integration-fptu-quest-memory--recovery) | History database, remediation pathways, reintegration support. |
| BR-018 | Core platform capabilities: notifications, structured logging, real-time sync/data versioning, optimized content delivery. | [Cross-Cutting Requirements (All Phases)](../prd/requirements.md#cross-cutting-requirements-all-phases) | Cross-cutting across all phases. |