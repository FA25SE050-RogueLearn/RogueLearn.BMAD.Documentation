# User Interaction Flows

This document visualizes primary interaction flows using high-level business logic. Technical implementation details (SDKs, database triggers, specific vendors) are intentionally omitted.

## Player Flow

```mermaid
graph TD
    subgraph "Phase 1: Onboarding & Optional Student Verification"
        A[Start] --> B{New User?}
        B -- Yes --> C[Create Account]
        B -- No --> F[View Dashboard]
        C --> D[Optional: Verify Student Status]
        D --> D1{Verified?}
        D1 -- Yes --> E[Unlock University-Aligned Features]
        D1 -- No --> E
        E --> R[Select Route (Curriculum)]
        R --> S[Select Class (Career Focus)]
        S --> T[AI Gap Analysis]
        T --> U[Generate Quest Line]
        U --> V[Create Integrated Skill Tree]
        V --> W{Optional: Upload Supporting Documents}
        W --> X[Enhance Visualization & Notes]
        X --> Y[Proceed]
        Y --> Z{Engage with Platform Features}
    end

    subgraph "Core Learning Loop"
        Z --> H[View Quest Line]
        H --> I[Complete Academic & Career Quests]
        I --> J[Boss Challenges & Assessments]
        J --> K[Update Progress]
        K --> L[View Leaderboards]
        Z --> M[Manage Notes & Resources]
        M --> N[Link Notes to Skill Tree]
        Z --> O[View Integrated Skill Tree]
        O --> P[Track Gap Analysis]
    end

    subgraph "Phase 2: Collaboration"
        Z --> Q[Create/Join Party]
        Q --> Q1[Invite Compatible Members]
        Q --> Q2[Access Shared Resources]
        Q2 --> Q3[Schedule Study Sessions]
        Q3 --> Q4[Conduct Collaborative Learning]
        Q4 --> Q5[Share Summary & Action Items]
        Z --> S[Join/Create Guilds]
        S --> S1[Access Guild Materials]
    end

    subgraph "Notifications & AI Assistance"
        U --> N1[Quest Updates]
        T --> N2[Performance-Based Adjustments]
        Q3 --> N3[Session Reminders]
        N1 --> N4[Email & Push Notifications]
        N2 --> N4
        N3 --> N4
    end
```

## Guild Master Flow

```mermaid
graph TD
    subgraph "Guild Creation & Lecturer Verification"
        A[Start] --> B{User Type?}
        B -- Player --> C[Create Guild]
        B -- Lecturer --> D[Submit Academic Credentials]
        D --> E{Verified?}
        E -- Yes --> F[Create Guild with Educational Privileges]
        E -- No --> C
        C --> H[Guild Management Dashboard]
        F --> H
    end

    subgraph "Guild Management"
        H --> I[Monitor Member Progress]
        I --> J[Identify Knowledge Gaps]
        H --> K[Upload Curriculum Reference Materials]
        K --> L[Share Resources with Guild]
        H --> M[Create Educational Announcements]
        M --> N[Send Updates to Members]
        H --> O[Manage Route-Compatible Members]
    end

    subgraph "Student Integration"
        L --> Q[Students Access Materials]
        Q --> R[Materials Enhance Personal Quest Lines]
        N --> S[Students Receive Notifications]
    end
```

## Party Leader Flow

```mermaid
graph TD
    subgraph "Party Creation"
        A[Player decides to create a Party] --> B[Becomes Party Leader]
        B --> C[Set Compatibility Rules]
        B --> D{Invite Members}
        D --> E[Invite Friends]
        D --> F[Browse by Route/Class Compatibility]
        D --> G[Set Party as Open]
        E --> H[Manage Party]
        F --> H
        G --> H
    end

    subgraph "Shared Resources Management"
        H --> I[Configure Resource Permissions]
        I --> J[Set Content Access]
        J --> K{Permission Types}
        K --> L[View-Only]
        K --> M[Edit]
        K --> N[Comment-Only]
        L --> O[Members Access Shared Resources]
        M --> O
        N --> O
    end

    subgraph "Study Session Management"
        H --> P{Schedule Study Session}
        P --> Q[Set Focus]
        Q --> Q1[Subject & Goals]
        Q --> Q2[Session Type]
        Q2 --> Q3[Route-Specific Study]
        Q2 --> Q4[Career Discussion]
        Q2 --> Q5[Exam Preparation]
        Q2 --> Q6[Project Collaboration]
        Q1 --> R[Send Invitations]
        Q3 --> R
        Q4 --> R
        Q5 --> R
        Q6 --> R
        R --> S[Conduct Collaborative Learning]
    end

    subgraph "Progress Tracking & AI Assistance"
        S --> T{Learning Content Capture}
        T --> U[Manual Notes]
        T --> V[Session Recording]
        U --> X[Generate Summary]
        V --> X
        X --> Y[AI-Generated Insights]
        Y --> Z[Progress Summary]
        Y --> AA[Key Learning Points]
        Y --> BB[Action Items]
        Z --> FF[Share Results with Party]
        AA --> FF
        BB --> FF
    end
```

## Game Master (Admin) Flow

```mermaid
graph TD
    A[Start] --> B[Access Admin Dashboard]

    subgraph "Analytics & Monitoring"
        B --> C{View Educational Analytics}
        C --> D[Player Engagement]
        D --> E[Route Completion]
        D --> F[Career Progress]
        C --> G[Platform Performance]
        C --> H[Educational Quality]
    end

    subgraph "Platform Management"
        B --> I{System Configuration}
        I --> J[Manage Notification Settings]
        I --> K[Configure Quest Parameters]
    end

    subgraph "Content & User Management"
        B --> O{Content Moderation}
        O --> P[Review Generated Content]
        O --> Q[Guild Verification]
        O --> R[Credential Validation]
        B --> S{User Management}
        S --> T[Access Control]
        S --> U[Data Privacy]
        S --> V[Data Retention]
        S --> W[Student Status Monitoring]
    end

    subgraph "Admin-Owned Educational Governance"
        B --> AO[Elective Library Curation & Approval]
        B --> AQ[University Curriculum Import & Administration]
    end

    subgraph "Integrations & Operations"
        B --> X[Monitor External Educational Integrations]
    end
```
