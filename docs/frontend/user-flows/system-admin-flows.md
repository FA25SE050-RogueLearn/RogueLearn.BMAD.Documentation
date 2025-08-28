# System Admin (Game Master) Flows

## Platform Management

**User Goal:** Monitor and maintain the platform, access analytics, and manage system settings

**Entry Points:** Admin dashboard, accessible only to authorized personnel

**Success Criteria:** Admin can view system health, user analytics, and manage global settings

### Flow Diagram

```mermaid
graph TD
    A[Start] --> B[Access Game Master Dashboard]
    B --> C{View Analytics}
    C --> D[Feature Usage Dashboard]
    D --> D1[Most Used Quests]
    D --> D2[Marketplace Trends]
    C --> E[User Cohort Analysis]
    E --> E1[Class & Route Popularity]
    E --> E2[Completion Rates by Cohort]
    C --> F[Platform Health Monitoring]
    B --> G{Manage Global Settings}
    G --> H[Set Bonus XP Days]
    G --> I[Launch Platform-Wide Challenges]
    B --> J{Review Moderation Queue}
    J --> K[Review Reported Content/Users]
    K --> L{Take Action}
    L --> M[Mute/Ban User]
    L --> N[Delete Content]
```

### Edge Cases & Error Handling:
- System performance degradation
- Data privacy concerns or breaches
- Conflicting global settings
- Complex moderation cases requiring escalation

**Notes:** The Admin interface should prioritize clarity and efficiency, with critical alerts prominently displayed and powerful tools accessible but protected from accidental misuse.