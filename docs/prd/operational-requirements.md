# Operational Requirements

This document specifies the operational requirements for the RogueLearn platform, addressing a key blocker identified in the PRD checklist report.

## Deployment

*   **Frequency**: We aim for a continuous deployment model, with automated deployments to production upon successful merging to the main branch. Initially, during the MVP phase, deployments might be batched and released weekly.
*   **Environments**:
    *   **Development (Dev)**: Individual developer environments, likely running locally.
    *   **Staging**: A production-like environment for final testing and QA before release. This environment should be as close to production as possible.
    *   **Production (Prod)**: The live environment for end-users.

## Monitoring

A basic monitoring plan is required to ensure system health and gather insights.

*   **Key Metrics to Watch**:
    *   **Application Performance**: Response times (p95, p99), error rates (5xx, 4xx).
    *   **System Health**: CPU utilization, memory usage, disk space.
    *   **User Activity**: Number of active users, quests created, party formations.
*   **Tooling**: To be determined by the architecture, but could include tools like Prometheus, Grafana, Sentry, or a cloud provider's native monitoring solution.