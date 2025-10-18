# PRD ↔ SRS Traceability Matrix

Purpose: Map PRD requirements to SRS sections and IDs for clear traceability across Business Rules (BR), Use Cases (UC), Screens/Functions, and NFRs.

## Phase 1 – Core Player MVP
- PRD: Onboarding & Academic Data (FR1–FR7)
  - SRS Business Rules: BR-001, BR-002, BR-003, BR-006
  - SRS Use Cases: UC-001, UC-002
  - SRS Screens/Functions: Character Creation Wizard; Import Academic Document (pop-up); Learning Progress
  - SRS NFRs: Accessibility, Security (verification privacy), Performance (onboarding task times)

- PRD: AI Curriculum & Career Alignment (FR4A)
  - SRS Business Rules: BR-005
  - SRS Use Cases: UC-004
  - SRS Screens/Functions: Quest Line Interface; Quest Detail View
  - SRS NFRs: Observability (AI success metrics), Performance (generation cadence)

- PRD: Skill/Stats Foundation (FR7–FR13)
  - SRS Business Rules: BR-006, BR-007
  - SRS Use Cases: UC-003
  - SRS Screens/Functions: Skill Tree Visualization; Dashboard stats
  - SRS NFRs: Performance (Skill Tree interactions), Accessibility (complex graph ARIA)

- PRD: Dashboard/Skill Tree/Arsenal (FR8, FR14–FR15)
  - SRS Business Rules: BR-007, BR-008
  - SRS Use Cases: UC-002, UC-003
  - SRS Screens/Functions: Main Dashboard; Skill Tree Visualization; Arsenal
  - SRS NFRs: Accessibility; Performance; Browser Compatibility

- PRD: Boss Fights (FR16–FR18)
  - SRS Business Rules: BR-009
  - SRS Use Cases: UC-009
  - SRS Screens/Functions: Boss Fight Arena
  - SRS NFRs: WebGL optimization budgets; Reliability (scoring precision)

- PRD: Leaderboards (FR19)
  - SRS Business Rules: BR-010
  - SRS Use Cases: UC-008 (guild context) / general leaderboard behaviors
  - SRS Screens/Functions: Leaderboard
  - SRS NFRs: Availability; Observability (ranking recalculations)

- PRD: Dynamic Quests & Notifications (FR20, FR39)
  - SRS Business Rules: BR-011, BR-018
  - SRS Use Cases: UC-004
  - SRS Screens/Functions: Quest Line Interface; In-app notifications
  - SRS NFRs: Reliability (24h rollback), Observability (notification delivery)

## Phase 2 – Browser Extension
- PRD: Browser Extension, FPTU portal integration (FR23–FR24, FR50)
  - SRS Business Rules: BR-012
  - SRS Use Cases: UC-007
  - SRS Screens/Functions: Arsenal (content organization), notifications
  - SRS NFRs: Security (token handling), Privacy (content extraction)

## Phase 3 – Educator & Game Master Toolkit
- PRD: Guild Creation & Lecturer Features (FR36–FR38)
  - SRS Business Rules: BR-014
  - SRS Use Cases: UC-008
  - SRS Screens/Functions: Guild Master Dashboard; Lecturer analytics views
  - SRS NFRs: Privacy (anonymization), Security (RBAC)

## Phase 3 – Event Management & Code Battle
- PRD: Code Arena & Competitive Events (FR43–FR47)
  - SRS Business Rules: BR-015
  - SRS Use Cases: UC-010
  - SRS Screens/Functions: Code Arena; Event Creation Wizard; Leaderboards (event category)
  - SRS NFRs: Performance (real-time feedback), Availability (event uptime)

## Cross-Cutting Requirements
- PRD: Notifications, Logging, Data Architecture, Content Delivery (FR39–FR42)
  - SRS Business Rules: BR-018
  - SRS Use Cases: Supporting flows across UC-001..UC-017
  - SRS Screens/Functions: Non-screen functions (Notification services, Logging, Sync)
  - SRS NFRs: Security; Observability; Availability

## Academic Integration
- PRD: FPTU Verification, Quest Memory, Recovery (FR49, FR51)
  - SRS Business Rules: BR-017
  - SRS Use Cases: UC-001, UC-002
  - SRS Screens/Functions: Import Academic Document; Learning Progress Interface
  - SRS NFRs: Privacy; Reliability

## Objective System, Knowledge Graph & Rewards
- PRD: Objective types, completion criteria, verification, knowledge graph, reward cascade, contextual unlocks (FR54–FR60)
  - SRS Business Rules: BR-016
  - SRS Use Cases: UC-002, UC-004, UC-009, UC-010
  - SRS Screens/Functions: Mission Control; Code Arena; Quest Detail View
  - SRS NFRs: Observability (reward ledger), Security (verification)

## Notes & References
- SRS Business Rules table: docs/srs/1-overall-description.md#12-business-rules
- SRS Use Cases table: docs/srs/2-user-requirements.md
- SRS Screens & Functions: docs/srs/3-functional-requirements.md
- SRS NFRs: docs/srs/4-non-functional-requirements.md
- PRD Requirements: docs/prd/requirements.md; PRD Epics: docs/prd/epic-list.md