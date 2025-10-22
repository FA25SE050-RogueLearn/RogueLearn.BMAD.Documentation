# 4. Non-Functional Requirements

## 4.1 External Interfaces
> Information to ensure the system communicates properly with users and external elements.

- User Interface: Responsive web (desktop/mobile), accessible per WCAG 2.1 AA, consistent with platform design system.
- Unity WebGL (Boss Fight): Embed via iframe/container with standardized messaging for score and state sync.

## 4.2 Quality Attributes
> List required system characteristics specification.

### 4.2.1 Usability
- Training time: Normal users become productive within 30 minutes; power users within 15 minutes.
- Familiarity: Interfaces align with common study tools; provides tooltips and inline help.
- Accessibility: Conform to WCAG 2.1 AA; keyboard navigation and screen reader support.
- Task times: 3-step onboarding completed within 5 minutes under normal network conditions.

### 4.2.2 Reliability
- Availability: ≥ 99.5% monthly, excluding planned maintenance.
- MTBF: ≥ 500 hours for core services.
- MTTR: ≤ 2 hours for P1 incidents.
- Accuracy: Boss Fight scoring precision within ±1 point; skill XP conversion validated against rules.
- Defect Rates: Critical ≤ 0.1/KLOC; Significant ≤ 0.5/KLOC; Minor ≤ 1/KLOC.
- Bug Classification: Critical = data loss or complete inability to use core functionality.
- Recovery Objectives: RPO ≤ 15 minutes; RTO ≤ 60 minutes for core user flows.

### 4.2.3 Performance
- Response time (95th percentile):
  - Dashboard load ≤ 1500 ms
  - Skill Tree interactions ≤ 500 ms
  - Quest Detail load ≤ 1000 ms
- Throughput: Support ≥ 50 transactions/sec sustained for read-heavy operations.
- Capacity: Support ≥ 10,000 active users; scalable to 100,000 registered accounts.
- Resource utilization: Server memory ≤ 70% average; CPU ≤ 60% average under normal load.

- Loading performance (Front-end Spec):
  - Initial page load LCP ≤ 2.5 s on 4G, mid-tier mobile; TTI ≤ 3.5 s; CLS ≤ 0.1.
  - Route navigation (client-side) ≤ 800 ms for cached views; ≤ 1200 ms cold.
  - Progressive loading: code-splitting and prefetch for Dashboard widgets; critical CSS inlined.

- Network & caching budgets:
  - Initial JS ≤ 200 KB (gzipped) on Dashboard; CSS ≤ 80 KB; lazy-load non-critical images.
  - Cache-Control: static assets max-age=7d with content hashing; HTML no-cache; API TTL per data freshness (≤ 5 min for dashboard stats).
  - Service Worker: offline cache for Arsenal and Quest Detail; background sync queue for submissions.
  - HTTP/2 or HTTP/3 enabled; Brotli compression for text; preconnect/dns-prefetch to core API domains.
- Unity WebGL (Boss Fight) optimization:
  - Lazy load game bundle; initial payload ≤ 50 MB, deferred assets afterwards.
  - Target FPS: desktop ≥ 60; low-end devices ≥ 30; frame pacing within ±10 ms.
  - Memory budget ≤ 512 MB; texture compression (ASTC/ETC2); canvas scaling for DPI.
  - Input latency ≤ 100 ms; network join/reconnect ≤ 1500 ms.
  - Graceful suspend/resume; watchdogs for fatal WebGL context loss with recovery.

### 4.2.4 Security
- OWASP Top 10 mitigations: input validation, output encoding, CSRF protections where applicable, secure auth/session handling.
- Authentication & Authorization: JWT validation, RBAC enforcement (self vs managed vs admin), least-privilege policies.
- Transport security: TLS 1.2+ across all endpoints; HSTS for public domains.
- Secrets management: managed vault; rotation policies; no secrets in code or logs.
- Rate limiting & abuse prevention: per-IP and per-user limits; captcha or challenge mechanisms for critical flows.
- Data protection: PII minimization; at-rest encryption for sensitive fields; audit trails for admin actions.

### 4.2.5 Privacy & Compliance
- GDPR-aligned practices: consent for analytics, clear privacy notices, data subject rights (access, rectification, deletion).
- Data retention: academic verification docs retained per institutional policy; configurable retention windows; secure purge.
- Anonymization: lecturer analytics use anonymized or aggregated datasets unless explicit consent exists.
- Records of processing: documented data flows for academic, social, and events domains.

### 4.2.6 Availability & Resilience
- SLOs: availability ≥ 99.5%; error budget tracked monthly.
- Resilience patterns: retries with backoff, circuit breakers for external APIs (FPTU, roadmap.sh), graceful degradation.
- Redundancy: Multi-AZ deployments for core services; automated failover.
- Backups: daily database backups with integrity checks; restore drills quarterly.

### 4.2.7 Observability
- Logging: structured logs with correlation IDs; PII redaction.
- Metrics: latency, throughput, error rates, resource utilization; WebGL-specific FPS, memory, bundle load time.
- Tracing: distributed tracing across services; trace sampling and propagation.
- Alerting: SLO violations, elevated error rates, queue backlogs; on-call runbooks.

### 4.2.8 Accessibility (WCAG 2.1 AA)
- Keyboard navigation and focus management across all interactive components.
- ARIA roles and landmarks for complex UIs (Skill Tree, Code Arena, Event Wizard).
- Color contrast ratios ≥ 4.5:1; user preferences for reduced motion and high-contrast modes.
- Alternative text for images, captions for multimedia; forms with clear labels and errors.

### 4.2.9 Browser & Device Compatibility
- Supported browsers: latest 2 versions of Chrome, Edge, Firefox; Safari latest.
- WebGL: fallback messaging for unsupported devices; capability detection and dynamic quality settings.

### 4.2.10 Maintainability & Deployment
- CI/CD: automated tests (unit, integration), linting, and schema validation gates.
- Configuration: environment-based config; feature flags for gradual rollout.
- Documentation: OpenAPI and developer guides kept current; change logs.