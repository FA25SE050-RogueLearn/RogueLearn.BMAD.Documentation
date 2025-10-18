# 5. Other Requirements

> Provide other system requirements (messages list), glossary of abbreviations, references, and open questions.

## 5.1 Messages List

| ID | Severity | Context | Message | Suggested Action | Source PRD |
|----|----------|---------|---------|------------------|------------|
| MSG-ONB-001 | Info | Onboarding | Character created successfully. | Proceed to Dashboard. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-ONB-002 | Warning | Onboarding | Please complete all required steps to continue. | Fill missing selections and retry. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-UPL-001 | Success | Document Upload | Upload completed. Verification in progress. | You will be notified when verification finishes. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-UPL-002 | Error | Document Upload | Upload failed. Please retry. | Check network, file size/type, and retry. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-UPL-003 | Warning | Document Upload | Invalid file type. Accepted: PDF, PNG, JPG. | Convert file to a supported format and try again. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-VRF-001 | Success | Verification | Verification successful. University flows unlocked. | Continue with semester planning. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-VRF-002 | Warning | Verification | Unverified status limits features. | Provide required documents or contact support. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-QUE-001 | Info | Quest Generation | New quest line generated from curriculum. | Review quests and start the first objective. | [AI Curriculum & Career Alignment](../prd/requirements.md#ai-curriculum--career-alignment) |
| MSG-QUE-002 | Error | Quest Generation | Unable to generate quests at this time. | Try again later or adjust specialization. | [AI Curriculum & Career Alignment](../prd/requirements.md#ai-curriculum--career-alignment) |
| MSG-QST-001 | Success | Quest Attempt | Objective completed. XP awarded. | Continue to next objective or view rewards. | [Objective System, Knowledge Graph & Rewards](../prd/requirements.md#objective-system-knowledge-graph--rewards) |
| MSG-QST-002 | Error | Quest Attempt | Submission error. Please review and retry. | Fix validation errors and resubmit. | [Objective System, Knowledge Graph & Rewards](../prd/requirements.md#objective-system-knowledge-graph--rewards) |
| MSG-BF-001 | Info | Boss Fight | Assessment started. Time is running. | Focus on tasks; pause not available in exam mode. | [Boss Fights & Leaderboards](../prd/requirements.md#boss-fights--leaderboards) |
| MSG-BF-002 | Error | Boss Fight | Network interruption detected. Attempt auto-saved. | Reconnect to resume; avoid closing the browser. | [Boss Fights & Leaderboards](../prd/requirements.md#boss-fights--leaderboards) |
| MSG-COD-001 | Warning | Code Arena | Tests failing. Review output and fix. | Inspect failing cases; rerun tests. | [Event Platform (Code Arena & Guild Events)](../prd/requirements.md#event-platform-code-arena--guild-events) |
| MSG-COD-002 | Error | Code Arena | Compilation error. | Fix syntax or dependency issues and retry. | [Event Platform (Code Arena & Guild Events)](../prd/requirements.md#event-platform-code-arena--guild-events) |
| MSG-LBD-001 | Info | Leaderboard | Rankings updated. | View detailed score breakdown. | [Boss Fights & Leaderboards](../prd/requirements.md#boss-fights--leaderboards) |
| MSG-EVT-001 | Warning | Event Wizard | Scheduling conflict detected. | Choose a different time or venue. | [Event Platform (Code Arena & Guild Events)](../prd/requirements.md#event-platform-code-arena--guild-events) |
| MSG-EVT-002 | Success | Event Wizard | Event created successfully. | Share registration link with participants. | [Event Platform (Code Arena & Guild Events)](../prd/requirements.md#event-platform-code-arena--guild-events) |
| MSG-GLD-001 | Info | Guild Management | Invitation sent. | Await recipient acceptance. | [Basic Guild Management (Verified Lecturer Gated)](../prd/epic-list.md#basic-guild-management-verified-lecturer-gated) |
| MSG-GLD-002 | Error | Guild Management | Action not permitted: insufficient role. | Request Verified Lecturer or Guild Master privileges. | [Basic Guild Management (Verified Lecturer Gated)](../prd/epic-list.md#basic-guild-management-verified-lecturer-gated) |
| MSG-BEX-001 | Success | Browser Extension | Content saved to Arsenal. | Open Arsenal to organize and tag. | [Browser Extension Integration](../prd/requirements.md#browser-extension-integration-fptu--arsenal) |
| MSG-OFF-001 | Warning | Offline Mode | Operating offline. Some features are limited. | Continue with cached content; sync when online. | [Operational Requirements](../prd/operational-requirements.md#offline-behavior--caching) |
| MSG-RAT-001 | Warning | Rate Limiting | Too many requests. Please wait and try again. | Slow down and retry after a few seconds. | [Security & Abuse Prevention](../prd/requirements.md#security--abuse-prevention) |
| MSG-AUTH-001 | Error | Authentication | Session expired. Please log in again. | Reauthenticate to continue. | [User Roles & Authentication](../prd/user-roles.md#authentication) |
| MSG-API-001 | Error | External API | FPTU API unavailable. | Retry later; system will degrade gracefully. | [Integration: FPTU](../prd/integration-requirements.md#fptu-academic-portal) |

## 5.2 Glossary of Abbreviations
- FR: Functional Requirement
- NFR: Non-Functional Requirement
- UI: User Interface
- API: Application Programming Interface
- PRD: Product Requirements Document
- ERD: Entity Relationship Diagram

## 5.3 References
- FPTU Academic Portal/API
- roadmap.sh APIs & content
- GitHub OAuth/API
- Unity WebGL documentation
- Internal Architecture docs (service databases, interfaces)

## 5.4 Open Questions
- Should we integrate additional LMS providers beyond GitHub and roadmap.sh?
- What restrictions do we impose for non-verified students when joining guilds/parties?
- What is the data retention policy for academic documents and quest submissions?

---

## 5.5 Glossary (authoritative from docs/prd/glossary.md)

- Arsenal: The note-taking workspace where students create, organize, and link notes to skills and quests.
- Character Creation: The onboarding flow where a student selects curriculum, specialization, and initial roadmap.
- XP (Experience Points): Numeric progression awarded for completing quests, boss fights, and learning milestones.
- Skill Tree: Graph of interrelated skills with prerequisites and levels that represent the student's growth.
- Quest: A structured learning activity with objectives, steps, and resources derived from syllabus or roadmap gaps.
- Boss Fight: A mock exam or capstone assessment delivered via WebGL, used to benchmark mastery and grant XP.
- Party: A small study group that collaborates on quests and shares resources; managed by a Party Leader.
- Guild: A larger community that aggregates several parties; managed by a Guild Master.
- Party Leader: Role that manages party membership, events, and collaborative activities.
- Guild Master: Role that manages guild membership, invitations, and guild achievements.
- Game Master (Admin): Platform administrator with ownership of Elective Library and University Curriculum administration.
- Verified Lecturer: Educator status granted after verification, enabling enhanced privileges for events/content moderation.
- Learning Path: Sequence of quests curated to guide learners through a topic with clear progression checkpoints.
- Assessment: Formal evaluation component tied to quests (e.g., quizzes, assignments, boss fights) with submissions.
- Roadmap.sh: External reference for specialization tracks used to inform curriculum alignment and skill taxonomy.
- FPTU: FPT University; source of academic data, verification, and curriculum structures.
- Vector Index: Embedding-powered index that supports semantic search and content recommendation across notes/resources.
- Telemetry: Analytics and event tracking for performance, engagement, and reliability metrics.
- Unity WebGL: Technology used to render interactive boss fights and game-like experiences in the browser.