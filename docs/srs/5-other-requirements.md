# 5. Other Requirements

> Provide other system requirements (messages list), glossary of abbreviations, references, and open questions.

## 5.1 Common Requirements

### 5.1.1 Cross-Platform Compatibility
- **Web Standards Compliance**: Support for modern web standards (HTML5, CSS3, ES6+)
- **Browser Support**: Latest 2 versions of Chrome, Edge, Firefox; Safari latest
- **Responsive Design**: Mobile-first approach with breakpoints for tablet and desktop
- **Progressive Web App (PWA)**: Offline capabilities and app-like experience

### 5.1.2 Data Management
- **Data Validation**: Client-side and server-side validation for all user inputs
- **Data Backup**: Automated daily backups with integrity checks
- **Data Migration**: Version-controlled database schema migrations
- **Data Archival**: Configurable retention policies for different data types

### 5.1.3 Integration Standards
- **API Design**: RESTful APIs following OpenAPI 3.0 specification
- **Authentication**: OAuth 2.0 and JWT token-based authentication
- **Third-party Integration**: Standardized connectors for FPTU, GitHub, roadmap.sh
- **Webhook Support**: Event-driven notifications for external systems

### 5.1.4 Monitoring and Logging
- **Application Monitoring**: Real-time performance metrics and health checks
- **Error Tracking**: Centralized error logging with stack traces and context
- **Audit Trails**: Comprehensive logging of user actions and system changes
- **Analytics**: User behavior tracking with privacy compliance

### 5.1.5 Development Standards
- **Code Quality**: Automated linting, testing, and code coverage requirements
- **Documentation**: Inline code documentation and API documentation
- **Version Control**: Git-based workflow with branching strategies
- **Deployment**: CI/CD pipelines with automated testing and deployment

## 5.2 Messages List

| ID | Severity | Context | Message | Suggested Action | Source PRD |
|----|----------|---------|---------|------------------|------------|
| MSG-1 | Info | Onboarding | Character created successfully. | Proceed to Dashboard. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-2 | Warning | Onboarding | Please complete all required steps to continue. | Fill missing selections and retry. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-3 | Success | Document Upload | Upload completed. Verification in progress. | You will be notified when verification finishes. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-4 | Error | Document Upload | Upload failed. Please retry. | Check network, file size/type, and retry. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-5 | Warning | Document Upload | Invalid file type. Accepted: PDF, PNG, JPG. | Convert file to a supported format and try again. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-6 | Success | Verification | Verification successful. University flows unlocked. | Continue with semester planning. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-7 | Warning | Verification | Unverified status limits features. | Provide required documents or contact support. | [Onboarding & Academic Data](../prd/requirements.md#onboarding--academic-data) |
| MSG-8 | Info | Quest Generation | New quest line generated from curriculum. | Review quests and start the first objective. | [AI Curriculum & Career Alignment](../prd/requirements.md#ai-curriculum--career-alignment) |
| MSG-9 | Error | Quest Generation | Unable to generate quests at this time. | Try again later or adjust specialization. | [AI Curriculum & Career Alignment](../prd/requirements.md#ai-curriculum--career-alignment) |
| MSG-10 | Success | Quest Attempt | Objective completed. XP awarded. | Continue to next objective or view rewards. | [Objective System, Knowledge Graph & Rewards](../prd/requirements.md#objective-system-knowledge-graph--rewards) |
| MSG-11 | Error | Quest Attempt | Submission error. Please review and retry. | Fix validation errors and resubmit. | [Objective System, Knowledge Graph & Rewards](../prd/requirements.md#objective-system-knowledge-graph--rewards) |
| MSG-12 | Info | Boss Fight | Assessment started. Time is running. | Focus on tasks; pause not available in exam mode. | [Boss Fights & Leaderboards](../prd/requirements.md#boss-fights--leaderboards) |
| MSG-13 | Error | Boss Fight | Network interruption detected. Attempt auto-saved. | Reconnect to resume; avoid closing the browser. | [Boss Fights & Leaderboards](../prd/requirements.md#boss-fights--leaderboards) |
| MSG-14 | Warning | Code Arena | Tests failing. Review output and fix. | Inspect failing cases; rerun tests. | [Event Platform (Code Arena & Guild Events)](../prd/requirements.md#event-platform-code-arena--guild-events) |
| MSG-15 | Error | Code Arena | Compilation error. | Fix syntax or dependency issues and retry. | [Event Platform (Code Arena & Guild Events)](../prd/requirements.md#event-platform-code-arena--guild-events) |
| MSG-16 | Info | Leaderboard | Rankings updated. | View detailed score breakdown. | [Boss Fights & Leaderboards](../prd/requirements.md#boss-fights--leaderboards) |
| MSG-17 | Warning | Event Wizard | Scheduling conflict detected. | Choose a different time or venue. | [Event Platform (Code Arena & Guild Events)](../prd/requirements.md#event-platform-code-arena--guild-events) |
| MSG-18 | Success | Event Wizard | Event created successfully. | Share registration link with participants. | [Event Platform (Code Arena & Guild Events)](../prd/requirements.md#event-platform-code-arena--guild-events) |
| MSG-19 | Info | Guild Management | Invitation sent. | Await recipient acceptance. | [Basic Guild Management (Verified Lecturer Gated)](../prd/epic-list.md#basic-guild-management-verified-lecturer-gated) |
| MSG-20 | Error | Guild Management | Action not permitted: insufficient role. | Request Verified Lecturer or Guild Master privileges. | [Basic Guild Management (Verified Lecturer Gated)](../prd/epic-list.md#basic-guild-management-verified-lecturer-gated) |
| MSG-21 | Success | Browser Extension | Content saved to Arsenal. | Open Arsenal to organize and tag. | [Browser Extension Integration](../prd/requirements.md#browser-extension-integration-fptu--arsenal) |
| MSG-22 | Warning | Rate Limiting | Too many requests. Please wait and try again. | Slow down and retry after a few seconds. | [Security & Abuse Prevention](../prd/requirements.md#security--abuse-prevention) |
| MSG-23 | Error | Authentication | Session expired. Please log in again. | Reauthenticate to continue. | [User Roles & Authentication](../prd/user-roles.md#authentication) |

## 5.3 Glossary of Abbreviations

### Technical Abbreviations
- **API**: Application Programming Interface
- **ARIA**: Accessible Rich Internet Applications
- **ASTC**: Adaptive Scalable Texture Compression
- **AZ**: Availability Zone
- **BR**: Business Rule
- **CI/CD**: Continuous Integration/Continuous Deployment
- **CLS**: Cumulative Layout Shift
- **CSS**: Cascading Style Sheets
- **DPI**: Dots Per Inch
- **ERD**: Entity Relationship Diagram
- **ETC2**: Ericsson Texture Compression 2
- **FPS**: Frames Per Second
- **FR**: Functional Requirement
- **GDPR**: General Data Protection Regulation
- **HSTS**: HTTP Strict Transport Security
- **HTML**: HyperText Markup Language
- **HTTP**: HyperText Transfer Protocol
- **JS**: JavaScript
- **JWT**: JSON Web Token
- **KLOC**: Thousand Lines of Code
- **LCP**: Largest Contentful Paint
- **LMS**: Learning Management System
- **MTBF**: Mean Time Between Failures
- **MTTR**: Mean Time To Recovery
- **NFR**: Non-Functional Requirement
- **OAuth**: Open Authorization
- **OWASP**: Open Web Application Security Project
- **P1**: Priority 1 (Critical)
- **PII**: Personally Identifiable Information
- **PRD**: Product Requirements Document
- **PWA**: Progressive Web App
- **RBAC**: Role-Based Access Control
- **RPO**: Recovery Point Objective
- **RTO**: Recovery Time Objective
- **SLO**: Service Level Objective
- **SRS**: Software Requirements Specification
- **TLS**: Transport Layer Security
- **TTI**: Time To Interactive
- **UC**: Use Case
- **UI**: User Interface
- **WCAG**: Web Content Accessibility Guidelines
- **WebGL**: Web Graphics Library
- **XP**: Experience Points

### Domain-Specific Abbreviations
- **FPTU**: FPT University
- **MSG**: Message (identifier prefix)

## 5.4 References
- Unity WebGL documentation
- Internal Architecture docs (service databases, interfaces)
- [WCAG 2.1 AA Guidelines](https://www.w3.org/WAI/WCAG21/AA/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [OpenAPI 3.0 Specification](https://swagger.io/specification/)
- [OAuth 2.0 RFC](https://tools.ietf.org/html/rfc6749)
- [GDPR Compliance Guidelines](https://gdpr.eu/)

## 5.5 Open Questions
- Should we integrate additional LMS providers beyond GitHub and roadmap.sh?
- What restrictions do we impose for non-verified students when joining guilds/parties?
- What is the data retention policy for academic documents and quest submissions?
- How should we handle multi-language support for international students?
- What are the specific requirements for mobile app versions (iOS/Android)?
- Should we implement real-time collaboration features in the Arsenal workspace?
- What level of customization should be allowed for guild-specific quest templates?

## 5.6 Glossary (authoritative from docs/prd/glossary.md)

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
- Game Master (Admin): Platform administrator with ownership University Curriculum administration.
- Verified Lecturer: Educator status granted after verification, enabling enhanced privileges for events/content moderation.
- Learning Path: Sequence of quests curated to guide learners through a topic with clear progression checkpoints.
- Assessment: Formal evaluation component tied to quests (e.g., quizzes, assignments, boss fights) with submissions.
- Roadmap.sh: External reference for specialization tracks used to inform curriculum alignment and skill taxonomy.
- FPTU: FPT University; source of academic data, verification, and curriculum structures.
- Vector Index: Embedding-powered index that supports semantic search and content recommendation across notes/resources.
- Telemetry: Analytics and event tracking for performance, engagement, and reliability metrics.
- Unity WebGL: Technology used to render interactive boss fights and game-like experiences in the browser.