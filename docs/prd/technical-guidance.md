# **Technical Guidance**

### **Core Architecture**

#### **Repository Structure**
- **Multi-Repo Strategy:** Separate repositories for frontend (`roguelearn-web`) and backend (`roguelearn-api`)
- **Shared Package:** Private NPM package (`@roguelearn/shared-types`) for TypeScript interfaces
- **Documentation Repo:** Centralized documentation repository (`roguelearn-docs`)
- **Extension Repo:** Separate repository for browser extension (`roguelearn-extension`)

#### **Service Architecture (Simplified for MVP)**
- **Frontend:** Next.js 14+ with App Router, TypeScript, Tailwind CSS
- **Backend:** .NET 8 Web API with Clean Architecture pattern
- **Database:** PostgreSQL 15+ with Entity Framework Core
- **Cache:** Redis for session management and API response caching
- **File Storage:** Local file system for MVP (Azure Blob Storage for production)
- **AI Services:** OpenAI GPT-4 API (simplified quest generation only)

**Unity Integration for Boss Fights:**
- Unity WebGL for interactive boss fight experiences
- Seamless browser integration with web-native UI
- Unity-to-web communication for progress tracking

#### **Technology Stack Constraints (MVP Focused)**
- **Node.js:** Version 18+ (LTS) for frontend development
- **.NET:** Version 8.0+ for backend services
- **Database:** PostgreSQL 15+ (required for JSON operations and user data)
- **Browser Support:** Chrome 90+, Firefox 88+, Safari 14+, Edge 90+ (standard web technologies)
- **Mobile:** Responsive design targeting iOS 14+ and Android 10+

**Removed from MVP:**
- Unity Engine requirements (no WebGL dependencies)
- Advanced browser compatibility requirements for game features

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

**Removed from MVP:**
- Unity WebGL integration and game communication
- Complex 2D boss fight mechanics
- Advanced game state synchronization

- **Backend Architecture:**
  - Clean Architecture with CQRS pattern
  - Repository pattern with Unit of Work
  - Dependency injection with built-in .NET DI container
  - AutoMapper for object-to-object mapping
  - FluentValidation for request validation

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
- **Unit Tests:**
  - Frontend: Jest + React Testing Library (>80% coverage)
  - Backend: xUnit + Moq + FluentAssertions (>85% coverage)
  - Shared: TypeScript type testing with tsd

- **Integration Tests:**
  - API integration tests using WebApplicationFactory
  - Database integration tests with test containers
  - External service integration tests with WireMock

- **End-to-End Tests:**
  - Playwright for cross-browser testing
  - Critical user journeys: registration, quest creation, skill tree navigation
  - Performance testing with Lighthouse CI

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

#### **Simplified AI-driven Quest Generation**
- **Risk:** Basic document parsing and quest generation accuracy
- **Technical Constraints:**
  - OpenAI API rate limits: 3,500 requests/minute for GPT-4
  - Token limits: 8,192 tokens per request (input + output)
  - Cost considerations: ~$0.03 per 1K tokens for GPT-4
- **Mitigation Strategy:**
  - Simple document text extraction (no OCR for MVP)
  - Basic prompt templates for quest generation
  - Implement response validation and retry logic
  - Cache processed results to minimize API calls
  - Use GPT-3.5-turbo as primary model for cost optimization
  - Simple loading states instead of streaming responses

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

**Removed from MVP:**
- Unity Editor and WebGL build requirements
- Game development tools and extensions

#### **Local Development Constraints**
- **Port Allocation:** Frontend (3000), Backend (5000), Database (5432), Redis (6379)
- **Environment Variables:** Use .env.local for sensitive data
- **Hot Reload:** Enabled for both frontend and backend development
- **HTTPS:** Required for browser extension testing

### **MVP Technical Scope Summary**

#### **Included in MVP**
- Next.js frontend with basic gamification UI
- .NET 8 backend with clean architecture
- PostgreSQL database with Entity Framework
- Redis caching for sessions and API responses
- OpenAI GPT-3.5-turbo for basic quest generation
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