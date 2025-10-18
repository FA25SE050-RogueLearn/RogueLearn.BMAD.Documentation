# 4. Non-Functional Requirements

## 4.1 External Interfaces
> Information to ensure the system communicates properly with users and external elements.

- User Interface: Responsive web (desktop/mobile), accessible per WCAG 2.1 AA, consistent with platform design system.
- FPTU Portal/API: OAuth2 or token-based access for academic calendar, course, and verification endpoints.
- roadmap.sh API: Read-only integration for career path content used in gap analysis.
- GitHub OAuth/API: OAuth login (optional), repository linking to Arsenal notes.
- Analytics/Telemetry: Client-side event tracking and server-side metrics for usage and performance monitoring.
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

- Unity WebGL (Boss Fight) optimization:
  - Lazy load game bundle; initial payload ≤ 50 MB, deferred assets afterwards.
  - Target FPS: desktop ≥ 60; low-end devices ≥ 30; frame pacing within ±10 ms.
  - Memory budget ≤ 512 MB; texture compression (ASTC/ETC2); canvas scaling for DPI.
  - Input latency ≤ 100 ms; network join/reconnect ≤ 1500 ms.
  - Graceful suspend/resume; watchdogs for fatal WebGL context loss with recovery.

- Mobile performance:
  - Touch response ≤ 100 ms; scroll jank budget: ≤ 0.1% frames > 50 ms.
  - Battery usage: ≤ 5% drain per 10-minute session on mid-tier devices.
  - Offline capability: read-only Arsenal and cached Quest Detail; deferred sync when online.