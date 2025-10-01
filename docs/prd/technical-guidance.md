# **Technical Guidance**

### **Core Architecture**

#### **Repository Structure**
- **Multi-Repo Strategy:** Separate repositories for frontend (`roguelearn-web`) and backend (`roguelearn-api`)
- **Shared Package:** Private NPM package (`@roguelearn/shared-types`) for TypeScript interfaces
- **Documentation Repo:** Centralized documentation repository (`roguelearn-docs`)
- **Extension Repo:** Separate repository for browser extension (`roguelearn-extension`)

#### **Service Architecture (Enhanced for Phase 3)**
- **Frontend:** Next.js 14+ with App Router, TypeScript, Tailwind CSS
- **Backend Core:** .NET 9 Web API with Clean Architecture pattern
- **Database:** PostgreSQL 15+ with Entity Framework Core
- **Cache:** Redis for session management and API response caching
- **File Storage:** Local file system for MVP (Supabase Storage for production)
- **AI Services:** Gemini API via internal AI Proxy service (simplified quest generation only)

**Phase 3 Enhanced Services:**
- **Event Management Service:** .NET 9 microservice for event creation, scheduling, and lifecycle management
- **Code Battle Service:** Go microservice with Docker containerization for secure code execution
- **Tournament Service:** Go microservice for bracket management and competitive tournament coordination
- **Real-time Communication Service:** SignalR hubs for live event updates, spectator mode, and real-time notifications
- **Approval Workflow Service:** Go microservice for multi-tier event approval and administrative oversight

**Unity Game Client & Multiplayer:**
- Unity 2022 LTS; Netcode for GameObjects (NGO)
- Primary client: WebGL; optional Windows/macOS desktop for debugging
- Multiplayer P2: Unity Lobby + Relay (WSS) join-code flow; client-hosted sessions
- **Phase 3 Enhancement**: Dedicated headless server (Linux) for code battle execution and tournament management
- Unity-to-web communication for progress/state UI and telemetry
- **Code Battle Integration**: Real-time code execution environment with live spectator capabilities

#### **Technology Stack Constraints (MVP Focused)**
- **Node.js:** Version 18+ (LTS) for frontend development
- **.NET:** Version 9.0+ for backend services
- **Database:** PostgreSQL 15+ (required for JSON operations and user data)
- **Browser Support:** Chrome 90+, Firefox 88+, Safari 14+, Edge 90+ (standard web technologies)
- **Mobile:** Post-MVP Phase - Responsive design targeting iOS 14+ and Android 10+

**Included in MVP:**
- Unity Engine requirements (WebGL client build target)
- Unity single-player core loop integration (Boss Fights) for MVP
- UGS services (Authentication, Lobby, Relay) introduced in Phase 2; ensure WebGL WSS compatibility
- Browser compatibility for WebGL build and WSS

#### Unity Game Client & Multiplayer — Phased Plan
- Phase 1 (MVP): Single-player WebGL boss-fight loop
  - Deliverables: Embedded Unity WebGL build, JS ↔ Unity bridge for session start/stop and results, basic analytics/telemetry, 2D boss mechanics, 30 Hz sim locally
  - Constraints: No networking; focus on fast load and smooth web integration
  - Exit criteria: Stable WebGL load in target browsers, results posted to backend, >95% successful session start rate, TTFMP within acceptable budget
- Phase 2 (Networking via UGS): Client-hosted sessions with Lobby + Relay + NGO
  - Deliverables: Anonymous Auth, Lobby create/join (join-code), Relay allocation/join (WSS), NGO gameplay with ServerRpc/ClientRpc and NetworkVariables for small, high-read fields; error handling + retry
  - Capacity target: Small-group co-op; start with ≤12 players to validate stability on WebGL + Relay
  - Exit criteria: Join flow reliability (>95%), stable 30 Hz sim with 15–20 Hz network send, acceptable jitter/packet loss handling, host migration plan documented
- Phase 3 (Enhanced Competition & Event Platform): Authoritative Linux server with code battle and event management
  - Deliverables: Headless build (Dockerized), TLS/WSS via reverse proxy, authoritative simulation + reconciliation, interest management, metrics/observability
  - **Code Battle Integration**: Real-time code execution environment, live spectator mode, tournament bracket management
  - **Event Management**: Scheduled event coordination, multi-tier approval workflows, guild-based competitions
  - Capacity target: Up to 20 players (Guild vs Guild) with fairness and anti-exploit guarantees, plus unlimited spectators
  - Exit criteria: Load test at 20 concurrent players per match with target tick stability, automated deploy + health checks, backfill/match-recovery flows, successful code battle execution with <2s latency

### **Technical Decision Framework**

All significant technical decisions will be evaluated against the following criteria, in order of priority:

1.  **User Experience & Performance:** Does the choice lead to a fast, responsive, and intuitive user experience?
2.  **Developer Experience & Velocity:** Is the technology easy to work with, well-documented, and does it enable the team to build and iterate quickly?
3.  **Scalability & Cost-Effectiveness:** Can the solution scale to meet projected user growth without incurring prohibitive costs?
4.  **Security & Reliability:** Is the technology secure, stable, and does it have a strong track record?
5.  **Ecosystem & Community:** Is there a strong community and a healthy ecosystem of libraries and tools around the technology?

### **Implementation Considerations**

#### **Development Patterns & Standards**
- **Frontend Architecture (Simplified for MVP):**
  - Component-based architecture with React Server Components
  - Custom hooks for state management and API calls
  - Atomic design methodology for component organization
  - Strict TypeScript configuration with no implicit any
  - ESLint + Prettier for code formatting and quality
  - Basic gamification UI components (progress bars, badges, simple animations)
  - Responsive design for cross-device compatibility

**Included for MVP:**
- Unity WebGL integration (single-player core loop); NGO networking introduced in Phase 2 (ServerRpc/ClientRpc, NetworkVariables)
- Simple 2D boss fight mechanics and core game loop
- Basic game state synchronization (30 Hz sim; 15–20 Hz network send; client interpolation)

- **Backend Architecture:**
  - Clean Architecture with CQRS pattern
  - Repository pattern with Unit of Work
  - Dependency injection with built-in .NET DI container
  - AutoMapper for object-to-object mapping
  - FluentValidation for request validation

**Phase 3 Enhanced Backend Services:**
- **Event Management Service Architecture:**
  - Domain-driven design with event sourcing for audit trails
  - Saga pattern for complex event lifecycle orchestration
  - Background services for scheduled event processing
  - Integration with external calendar systems (Google Calendar, Outlook)

- **Code Battle Service Architecture:**
  - Containerized execution environment with Docker isolation
  - Queue-based job processing with Hangfire for code execution
  - Real-time WebSocket connections for live code battle updates
  - Security sandboxing with resource limits and timeout controls

- **Tournament Service Architecture:**
  - Bracket generation algorithms with single/double elimination support
  - Real-time tournament state management with SignalR
  - Integration with leaderboard systems for ranking updates
  - Automated tournament progression and result processing

#### **API Design Standards**
- **RESTful API:** Following REST principles with proper HTTP verbs
- **Versioning:** URL-based versioning (e.g., `/api/v1/`)
- **Response Format:** Consistent JSON response structure with metadata
- **Rate Limiting:** 1000 requests/hour for authenticated users, 100/hour for anonymous
- **Pagination:** Cursor-based pagination for large datasets
- **Error Handling:** RFC 7807 Problem Details for HTTP APIs

#### **Security Implementation**
- **Authentication:** JWT tokens with refresh token rotation
- **Authorization:** Role-based access control (RBAC) with claims
- **Data Protection:** AES-256 encryption for sensitive data at rest
- **API Security:** CORS configuration, HTTPS enforcement, security headers
- **Input Validation:** Server-side validation for all user inputs
- **SQL Injection Prevention:** Parameterized queries and ORM usage

#### **Testing Strategy**

##### **Unit Testing Framework**
- **Frontend:** Jest + React Testing Library (>80% coverage)
  - Component testing with mock data and user interactions
  - Hook testing for custom React hooks
  - Utility function testing with edge cases
  - State management testing (Redux/Zustand)

- **Backend:** xUnit + Moq + FluentAssertions (>85% coverage)
  - Service layer testing with dependency injection
  - Repository pattern testing with in-memory databases
  - Business logic validation and edge case handling
  - Authentication and authorization testing

- **Shared:** TypeScript type testing with tsd
  - API contract validation between frontend and backend
  - Type safety verification for shared models
  - Interface compliance testing

##### **Integration Testing Strategy**

**API Integration Testing**
- **Framework:** WebApplicationFactory + TestContainers
- **Scope:** End-to-end API workflow validation
- **Test Scenarios:**
  - User authentication flow (registration, login, token refresh)
  - Document upload and AI processing pipeline
  - Quest generation and completion workflows
  - Party creation and collaboration features
  - Skill tree progression and achievement unlocking
  - Browser extension API endpoints

**Database Integration Testing**
- **Framework:** TestContainers with PostgreSQL
- **Scope:** Data persistence and retrieval validation
- **Test Scenarios:**
  - CRUD operations for all entities (User, Quest, Party, Guild)
  - Complex queries with joins and aggregations
  - Transaction rollback and consistency validation
  - Migration testing and schema validation
  - Performance testing for large datasets

**External Service Integration Testing**
- **Framework:** WireMock + Docker Compose
- **Scope:** Third-party service interaction validation
- **Test Scenarios:**
  - **AI Services:** Gemini API quest generation with various document types
  - **Authentication:** Supabase integration with different user scenarios
  - **File Storage:** Upload/download workflows with different file sizes
  - **Email Services:** Notification delivery and template rendering
  - **Payment Processing:** Stripe webhook handling and subscription management

**Cross-Service Integration Testing**
- **Framework:** Docker Compose + Newman (Postman CLI)
- **Scope:** Multi-service workflow validation
- **Test Scenarios:**
  - **User Onboarding Flow:** Registration → Email verification → Profile setup → First quest
  - **Learning Workflow:** Document upload → Processing → Quest generation → Completion → Progress tracking
  - **Social Features:** Party creation → Member invitation → Collaborative quest → Achievement sharing
  - **Browser Extension:** Content capture → Processing → Arsenal integration → Quest suggestion

**Unity WebGL Integration Testing**
- **Framework:** Playwright + Unity Test Framework
- **Scope:** Game client and web platform integration
- **Test Scenarios:**
  - Unity WebGL loading and initialization in browser
  - JavaScript-Unity communication bridge functionality
  - Boss fight game state synchronization with backend
  - Performance testing under various browser conditions
  - Error handling for Unity loading failures
  - Mobile browser compatibility testing

##### **End-to-End Testing Framework**

**Browser Testing**
- **Framework:** Playwright for cross-browser testing
- **Coverage:** Chrome, Firefox, Safari, Edge
- **Test Scenarios:**
  - Complete user journeys from registration to quest completion
  - Responsive design validation across device sizes
  - Accessibility compliance testing (WCAG 2.1 AA)
  - Performance testing with Lighthouse CI integration

**Performance & Load Testing**
- **Framework:** Artillery.js + K6 for load testing
- **Scope:** System performance under realistic load
- **Test Scenarios:**
  - **Concurrent Users:** 100-500 simultaneous users
  - **AI Processing Load:** Multiple document uploads and quest generations
  - **Database Performance:** Complex queries under load
  - **Real-time Features:** WebSocket connections and party synchronization
  - **Unity WebGL Performance:** Multiple concurrent game sessions

##### **Integration Test Environment Strategy**

**Test Environment Configuration**
- **Staging Environment:** Production-like setup with test data
- **Integration Test Environment:** Isolated environment for automated testing
- **Local Development:** Docker Compose with all services
- **CI/CD Pipeline:** Automated test execution on every pull request

**Test Data Management**
- **Seed Data:** Consistent test datasets for reproducible results
- **Test User Accounts:** Pre-configured users with different roles and permissions
- **Mock External Services:** Controlled responses for third-party integrations
- **Database Cleanup:** Automated cleanup between test runs

**Continuous Integration Testing Pipeline**
- **Pre-commit Hooks:** Unit tests and linting
- **Pull Request Validation:** Integration tests and code coverage
- **Staging Deployment:** End-to-end tests and performance validation
- **Production Deployment:** Smoke tests and health checks

##### **Quality Assurance & Monitoring**

**Test Reporting & Analytics**
- **Coverage Reports:** Detailed code coverage with branch analysis
- **Test Results Dashboard:** Real-time test execution status
- **Performance Metrics:** Response time trends and bottleneck identification
- **Failure Analysis:** Automated failure categorization and alerting

**Integration Health Monitoring**
- **Service Health Checks:** Continuous monitoring of all integrations
- **API Response Time Tracking:** Performance degradation alerts
- **Error Rate Monitoring:** Integration failure pattern analysis
- **Dependency Status:** Third-party service availability tracking

**Testing Best Practices**
- **Test Isolation:** Each test runs independently without side effects
- **Deterministic Results:** Consistent test outcomes across environments
- **Fast Feedback:** Test execution time optimization (<10 minutes for full suite)
- **Maintainable Tests:** Clear test structure and documentation

#### **Performance Optimization**
- **Frontend:**
  - Code splitting with dynamic imports
  - Image optimization with Next.js Image component
  - Bundle analysis and tree shaking
  - Service Worker for offline functionality
  - Basic static asset serving

- **Backend:**
  - Database query optimization with EF Core query analysis
  - Response caching with Redis
  - Background job processing with Hangfire
  - Connection pooling and async/await patterns
  - API response compression

#### **Game Networking & Project Structure **
- Assembly definitions: Core (game logic), Client (presentation), Net (NGO & messages)
- Scenes: Bootstrap, Main Menu (mode selector), SinglePlayer, MultiplayerClient, DedicatedServerBootstrap
- RPC Audit: Convert authoritative actions to ServerRpc; broadcast via ClientRpc; NetworkVariables for small, high-read fields
- Tick/Rate: 30 Hz simulation; 15–20 Hz network send; client interpolation and simple reconciliation
- Join Flow: UGS Auth (anonymous) → Lobby create/join → Relay allocate/join → NGO start client/host
- Error Handling: Surface join errors (codes), retry with backoff

#### **Deployment Strategy**
- **CI/CD Pipeline (GitHub Actions):**
  - Automated testing on pull requests
  - Security scanning with CodeQL
  - Dependency vulnerability scanning
  - Automated deployment to staging on main branch merge
  - Manual approval required for production deployment

- **Infrastructure:**
  - **Frontend:** Vercel with custom domain
  - **Unity WebGL:** Static hosting with CORS configuration for game assets
  - **Backend:** Docker containers on Azure Container Apps or AWS Fargate
  - **Database:** Managed PostgreSQL (Azure Database or AWS RDS)
  - **Cache:** Managed Redis (Azure Cache or AWS ElastiCache)
  - **Monitoring:** Application Insights or CloudWatch
  - **UGS (Phase 2):** Unity Authentication (anonymous), Lobby, Relay configured for WSS
  - **Dedicated Server (Phase 3):** Linux headless build, Dockerized, reverse proxy (Caddy) providing TLS and WSS upgrades; health endpoint

- **Environment Configuration:**
  - **Development:** Local development with Docker Compose
  - **Staging:** Mirrors production with test data
  - **Production:** High availability with auto-scaling

#### **Data Management**
- **Database Design:**
  - Normalized schema with appropriate indexes
  - Soft deletes for audit trails
  - Database migrations with Entity Framework
  - Connection string encryption

- **File Handling:**
  - Maximum file size: 10MB per document
  - Supported formats: PDF, DOCX, TXT, MD
  - Virus scanning before processing
  - Automatic cleanup of temporary files

- **Backup Strategy:**
  - Daily automated database backups
  - Point-in-time recovery capability
  - Cross-region backup replication
  - Monthly backup restoration testing

### **High-Risk Areas & Mitigation (MVP Focused)**

#### **AI-Driven Quest Generation Implementation Strategy**

##### **Core AI Architecture**
- **Primary AI Service:** Gemini API via internal AI Proxy service
- **Model Selection:** Gemini-1.5-Flash for cost-efficiency and speed
- **Fallback Model:** Gemini-1.5-Pro for complex documents when Flash fails
- **Processing Pipeline:** Document → Text Extraction → Content Analysis → Quest Generation → Validation → Storage

##### **Document Processing Workflow**
1. **Document Upload & Validation**
   - File type validation (PDF, DOCX, TXT, MD)
   - Size limits: 10MB max per document
   - Virus scanning before processing
   - Metadata extraction (title, author, creation date)

2. **Text Extraction Strategy**
   - **PDF:** Use PDF.js for client-side extraction, fallback to server-side with pdf2pic
   - **DOCX:** Server-side processing with mammoth.js
   - **TXT/MD:** Direct text processing
   - **OCR Fallback:** Tesseract.js for image-heavy PDFs (Phase 2)

3. **Content Preprocessing**
   - Text chunking (max 4000 tokens per chunk for Gemini context limits)
   - Content sanitization and formatting
   - Key concept extraction using NLP preprocessing
   - Document structure analysis (headers, sections, bullet points)

##### **Quest Generation Pipeline**

**Phase 1: Content Analysis**
```
Input: Processed document chunks
Process: 
- Identify learning objectives
- Extract key concepts and relationships
- Determine difficulty level
- Map to skill tree categories
Output: Structured content analysis
```

**Phase 2: Quest Template Selection**
```
Quest Types:
- Knowledge Check: Multiple choice, true/false
- Application: Scenario-based problems
- Synthesis: Connect concepts across documents
- Boss Fight: Gamified knowledge application
```

**Phase 3: Quest Generation**
```
Prompt Template Structure:
- System context (learning objectives, user level)
- Document content (chunked and formatted)
- Quest type specification
- Output format requirements (JSON schema)
- Difficulty calibration parameters
```

**Phase 4: Validation & Quality Assurance**
```
Automated Validation:
- JSON schema compliance
- Content relevance scoring (0.7+ threshold)
- Difficulty appropriateness check
- Duplicate detection
Manual Review Triggers:
- Low confidence scores (<0.6)
- User feedback flags
- Content moderation alerts
```

##### **Fallback Workflows & Error Handling**

**Level 1: API Failure Handling**
```
Primary: Gemini-1.5-Flash
├── Rate limit exceeded → Exponential backoff (2^n seconds, max 60s)
├── Token limit exceeded → Document chunking + parallel processing
├── API timeout → Retry with Gemini-1.5-Pro
└── Service unavailable → Fallback to pre-generated quest templates
```

**Level 2: Content Processing Failures**
```
Document parsing failure:
├── PDF extraction fails → Try alternative parser (pdf-parse)
├── Text quality too low → Request user to upload better quality
├── Content too short → Combine with related documents
└── No extractable content → Suggest manual content input
```

**Level 3: Quest Quality Failures**
```
Generated quest quality < threshold:
├── Retry with enhanced prompt context
├── Switch to simpler quest template
├── Use human-curated fallback questions
└── Flag for manual review and improvement
```

**Level 4: System Degradation**
```
Complete AI service failure:
├── Serve cached/pre-generated quests
├── Enable manual quest creation tools
├── Provide study guide generation instead
└── Graceful degradation message to users
```

##### **Performance & Cost Optimization**

**Caching Strategy**
- **Document Analysis Cache:** 7 days TTL for processed documents
- **Quest Template Cache:** 30 days TTL for generated quest patterns
- **User Context Cache:** 24 hours TTL for personalization data
- **API Response Cache:** 1 hour TTL for similar content requests

**Cost Management**
- **Token Optimization:** Compress prompts, remove redundant context
- **Batch Processing:** Group similar documents for efficiency
- **Smart Retry Logic:** Avoid unnecessary API calls on permanent failures
- **Usage Monitoring:** Track costs per user, implement usage caps
- **Model Selection:** Use Flash for 80% of requests, Pro for complex cases only

**Performance Targets**
- **Quest Generation Time:** <30 seconds for standard documents
- **Concurrent Processing:** Support 10 simultaneous generations
- **Cache Hit Rate:** >60% for repeat content patterns
- **API Success Rate:** >95% with fallback workflows

##### **Quality Assurance & Monitoring**

**Real-time Monitoring**
- API response times and error rates
- Quest generation success/failure rates
- User satisfaction scores (thumbs up/down)
- Content quality metrics (relevance, difficulty)

**Quality Metrics**
- **Relevance Score:** AI-generated content alignment with source material
- **Difficulty Calibration:** Match between intended and actual difficulty
- **Engagement Rate:** User completion rates for generated quests
- **Accuracy Rate:** Correctness of generated questions and answers

**Continuous Improvement**
- **Feedback Loop:** User ratings → prompt refinement
- **A/B Testing:** Compare different prompt strategies
- **Model Fine-tuning:** Collect high-quality examples for future training
- **Human Review:** Weekly review of flagged content

##### **Security & Privacy Considerations**

**Data Protection**
- **Document Encryption:** AES-256 for stored documents
- **API Communication:** TLS 1.3 for all AI service calls
- **Content Sanitization:** Remove PII before AI processing
- **Audit Logging:** Track all AI interactions for compliance

**Privacy Safeguards**
- **Data Minimization:** Only send necessary content to AI services
- **Retention Limits:** Delete processed content after 30 days
- **User Consent:** Clear opt-in for AI processing
- **Data Locality:** Ensure compliance with regional data laws

##### **Technical Risk Mitigation**
- **Risk:** Basic document parsing and quest generation accuracy
- **Technical Constraints:**
  - **Gemini API quotas and rate limits** as provided by the platform
  - **Model token/context limits** per selected Gemini model
  - **Cost considerations** managed with quota monitoring and caching strategies
- **Enhanced Mitigation Strategy:**
  - Multi-tier fallback system with 4 levels of degradation
  - Comprehensive caching to reduce API dependency
  - Quality scoring and validation at each step
  - Human oversight for edge cases and quality assurance
  - Cost monitoring with automatic usage caps and alerts

#### **Browser Extension Security**
- **Risk:** Security vulnerabilities and cross-site scripting attacks
- **Technical Constraints:**
  - Manifest V3 requirements (service workers, limited permissions)
  - Content Security Policy restrictions
  - Cross-origin request limitations
- **Mitigation Strategy:**
  - Implement Content Security Policy with strict directives
  - Use message passing instead of direct DOM manipulation
  - Sanitize all scraped content before processing
  - Implement certificate pinning for API communications
  - Regular security audits and penetration testing
  - Minimal permission requests (activeTab, storage only)

#### **Basic Skill Tree Display**
- **Risk:** Simple performance issues with skill tree rendering
- **Technical Constraints:**
  - Browser rendering limits for basic tree structures
  - Mobile device compatibility
- **Mitigation Strategy:**
  - Use simple HTML/CSS tree structure with basic animations
  - Implement responsive design for mobile devices
  - Cache skill tree data to minimize API calls
  - Use progressive enhancement for visual effects

**Advanced Features Included:**
- Dynamic visualization with interactive knowledge graphs
- Enhanced rendering and user interactions
- Real-time collaborative skill tree updates

#### **Scalability & Performance**
- **Risk:** System performance degradation under load
- **Technical Constraints:**
  - Database connection pool limits (100 connections max)
  - Memory usage limits in serverless environments
  - Cold start latency for serverless functions
- **Mitigation Strategy:**
  - Implement horizontal scaling with load balancers
  - Use database read replicas for query optimization
  - Implement circuit breaker pattern for external services
  - Use efficient static asset delivery
  - Implement graceful degradation for non-critical features

### **Technology-Specific Constraints**

#### **Next.js Limitations**
- **Server Components:** Limited client-side interactivity
- **Bundle Size:** Target <250KB initial bundle size
- **SEO Requirements:** Server-side rendering for public pages
- **Deployment:** Vercel function timeout limits (10s for hobby, 60s for pro)

#### **.NET Core Constraints**
- **Memory Usage:** Target <512MB per container instance
- **Startup Time:** <5 seconds for container cold starts
- **Request Timeout:** 30 seconds maximum for API endpoints
- **Concurrent Connections:** 1000 per instance maximum

#### **PostgreSQL Considerations**
- **Connection Limits:** 100 concurrent connections per database
- **Query Timeout:** 30 seconds maximum per query
- **Index Strategy:** Composite indexes for complex queries
- **JSON Operations:** Use JSONB for better performance

#### **Redis Caching Strategy**
- **Memory Limit:** 1GB for development, 4GB for production
- **TTL Strategy:** 1 hour for user sessions, 24 hours for static data
- **Eviction Policy:** LRU (Least Recently Used)
- **Persistence:** RDB snapshots every 15 minutes

### **Development Environment Setup**

#### **Required Tools & Versions (MVP Simplified)**
- **Node.js:** 18.17.0+ (use nvm for version management)
- **.NET SDK:** 8.0.100+
- **Docker:** 24.0+ with Docker Compose
- **PostgreSQL:** 15.3+ (or Docker container)
- **Redis:** 7.0+ (or Docker container)
- **Git:** 2.40+ with conventional commit hooks

#### **IDE Configuration**
- **VS Code Extensions:** ESLint, Prettier, C# Dev Kit, Docker
- **JetBrains Rider:** Recommended for .NET development
- **Database Tools:** pgAdmin 4 or DBeaver for PostgreSQL management


#### **Local Development Constraints**
- **Port Allocation:** Frontend (3000), Backend (5000), Database (5432), Redis (6379)
- **Environment Variables:** Use .env.local for sensitive data
- **Hot Reload:** Enabled for both frontend and backend development
- **HTTPS:** Required for browser extension testing

### **MVP Technical Scope Summary**

#### **Included in MVP**
- Next.js frontend with basic gamification UI
- .NET 9 backend with clean architecture
- PostgreSQL database with Entity Framework
- Redis caching for sessions and API responses
- Gemini API (via AI Proxy) for basic quest generation
- Browser extension with basic content scraping
- Simple skill tree display with HTML/CSS
- Basic user authentication and authorization
- File upload with local storage

#### **Comprehensive MVP Features (All Phases 1-4)**
- Unity WebGL integration for boss fight experiences
- Interactive skill tree visualization with dynamic updates
- Social collaboration features including party system
- Browser extension for content extraction and organization
- Educator toolkit with guild management capabilities
- Real-time synchronization and collaborative features
- Core AI-powered quest generation and personalization

#### **Future Development (Post-MVP)**
- Marketplace and creator economy features
- Enterprise analytics and institutional integrations
- Advanced AI features (proactive assistance, predictive analytics)
- Multi-region deployment and advanced scaling

**Rationale:** The expanded MVP scope provides a complete learning experience including social collaboration and Unity integration, validating the full gamified learning hypothesis while maintaining focused development priorities.