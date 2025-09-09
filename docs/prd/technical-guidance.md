# **Technical Guidance**

### **Core Architecture**

#### **Repository Structure**
- **Multi-Repo Strategy:** Separate repositories for frontend (`roguelearn-web`) and backend (`roguelearn-api`)
- **Shared Package:** Private NPM package (`@roguelearn/shared-types`) for TypeScript interfaces
- **Documentation Repo:** Centralized documentation repository (`roguelearn-docs`)
- **Extension Repo:** Separate repository for browser extension (`roguelearn-extension`)

#### **Service Architecture**
- **Frontend:** Next.js 14+ with App Router, TypeScript, Tailwind CSS
- **Game Engine:** Unity 2022.3 LTS with WebGL build target for boss fight mechanics
- **Backend:** .NET 8 Web API with Clean Architecture pattern
- **Database:** PostgreSQL 15+ with Entity Framework Core
- **Cache:** Redis for session management and API response caching
- **File Storage:** Azure Blob Storage or AWS S3 for document uploads
- **AI Services:** OpenAI GPT-4 API with fallback to Azure OpenAI

#### **Technology Stack Constraints**
- **Node.js:** Version 18+ (LTS) for frontend development
- **.NET:** Version 8.0+ for backend services
- **Unity:** Version 2022.3 LTS for game development with WebGL support
- **Database:** PostgreSQL 15+ (required for advanced JSON operations)
- **Browser Support:** Chrome 90+, Firefox 88+, Safari 14+, Edge 90+ (WebGL 2.0 required)
- **Mobile:** Responsive design targeting iOS 14+ and Android 10+ with WebGL compatibility

### **Technical Decision Framework**

All significant technical decisions will be evaluated against the following criteria, in order of priority:

1.  **User Experience & Performance:** Does the choice lead to a fast, responsive, and intuitive user experience?
2.  **Developer Experience & Velocity:** Is the technology easy to work with, well-documented, and does it enable the team to build and iterate quickly?
3.  **Scalability & Cost-Effectiveness:** Can the solution scale to meet projected user growth without incurring prohibitive costs?
4.  **Security & Reliability:** Is the technology secure, stable, and does it have a strong track record?
5.  **Ecosystem & Community:** Is there a strong community and a healthy ecosystem of libraries and tools around the technology?

### **Implementation Considerations**

#### **Development Patterns & Standards**
- **Frontend Architecture:**
  - Component-based architecture with React Server Components
  - Unity WebGL integration with JavaScript bridge for game communication
  - Custom hooks for state management and API calls
  - Atomic design methodology for component organization
  - Strict TypeScript configuration with no implicit any
  - ESLint + Prettier for code formatting and quality

- **Unity Game Architecture:**
  - 2D boss fight mechanics with educational quiz integration
  - WebGL build optimization for browser performance
  - JavaScript-Unity communication via Unity WebGL API
  - Game state synchronization with backend APIs
  - Responsive UI design for various screen sizes

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
  - CDN integration for static assets

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
  - **Frontend:** Vercel with custom domain and CDN
  - **Unity WebGL:** Static hosting via CDN with CORS configuration for game assets
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

### **High-Risk Areas & Mitigation**

#### **AI-driven Data Processing**
- **Risk:** Complex document parsing and quest generation accuracy
- **Technical Constraints:**
  - OpenAI API rate limits: 3,500 requests/minute for GPT-4
  - Token limits: 8,192 tokens per request (input + output)
  - Cost considerations: ~$0.03 per 1K tokens for GPT-4
- **Mitigation Strategy:**
  - Implement document preprocessing pipeline with OCR fallback
  - Create structured prompt templates with few-shot examples
  - Implement response validation and retry logic
  - Cache processed results to minimize API calls
  - Implement fallback to GPT-3.5-turbo for cost optimization
  - Use streaming responses for better user experience

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

#### **Dynamic Skill Tree Visualization**
- **Risk:** Performance issues with large knowledge graphs
- **Technical Constraints:**
  - Browser rendering limits: ~1000 nodes for smooth interaction
  - Memory constraints on mobile devices
  - Real-time updates without full re-renders
- **Mitigation Strategy:**
  - Implement virtualization for large graphs (only render visible nodes)
  - Use WebGL-based rendering (Three.js or similar)
  - Implement progressive loading with lazy evaluation
  - Cache graph layouts and use incremental updates
  - Provide simplified view options for mobile devices

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
  - Use CDN for static asset delivery
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

#### **Required Tools & Versions**
- **Node.js:** 18.17.0+ (use nvm for version management)
- **.NET SDK:** 8.0.100+
- **Unity:** 2022.3 LTS with WebGL build support
- **Docker:** 24.0+ with Docker Compose
- **PostgreSQL:** 15.3+ (or Docker container)
- **Redis:** 7.0+ (or Docker container)
- **Git:** 2.40+ with conventional commit hooks

#### **IDE Configuration**
- **VS Code Extensions:** ESLint, Prettier, C# Dev Kit, Docker
- **Unity Editor:** 2022.3 LTS with WebGL build module installed
- **JetBrains Rider:** Recommended for .NET development
- **Database Tools:** pgAdmin 4 or DBeaver for PostgreSQL management

#### **Local Development Constraints**
- **Port Allocation:** Frontend (3000), Backend (5000), Database (5432), Redis (6379)
- **Environment Variables:** Use .env.local for sensitive data
- **Hot Reload:** Enabled for both frontend and backend development
- **HTTPS:** Required for browser extension testing