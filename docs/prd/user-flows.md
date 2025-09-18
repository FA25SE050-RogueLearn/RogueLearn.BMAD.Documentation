# User Interaction Flows

This document visualizes the primary interaction flows for each user role within the RogueLearn platform using Mermaid diagrams.

## Student (Player) Flow

```mermaid
graph TD
    subgraph "Phase 1: Onboarding & Core Loop"
        A[Start] --> B{New User?};
        B -- Yes --> C[Create Account via In-App Form];
        B -- No --> F[View Dashboard / Character Sheet];
        C --> C1[Supabase Auth SDK handles registration];
        C1 --> C2[DB Trigger syncs new user to create profile];
        C2 --> D[Select Class & Route];
        D --> E[Upload Academic Documents];
        E --> E1[Upload GPA & Transcripts];
        E --> E2[Upload Timetable & Exam Schedule];
        E --> E3[Upload Course Syllabus];
        E1 --> AI1[AI Processing];
        E2 --> AI1;
        E3 --> AI1;
        AI1 --> AI2[Parse Curriculum & Detect Smaller Majors];
        AI2 --> AI3[Suggest Smaller Major Choices];
        AI3 --> AI4[User Selects Smaller Major];
        AI4 --> AI5[AI Suggests Related Classes];
        AI1 --> AI6[Generate Initial Character Stats];
        AI1 --> AI7[Populate Skill Tree with Academic Knowledge];
        AI1 --> AI8[Auto-create Quest Tasks from Schedule];
        AI5 --> F;
        AI6 --> F;
        AI7 --> F;
        AI8 --> F;
        F --> G{Engage with Features};
    end

    subgraph "Core Gameplay Loop"
        G --> H[View Dynamic Quest Line];
        H --> I[Complete Tasks & Events];
        I --> J[Face Boss Fights - Unity WebGL];
        J --> J1[Answer Questions by Difficulty];
        J1 --> J2[Real-time Score & Visual Feedback];
        J2 --> K[Update Character Stats];
        K --> L[View Leaderboards by Class/Overall];
        G --> M{Manage Arsenal};
        M --> M1[Rich Text Editor - Notion-like];
        M1 --> M2[Create/Organize Study Notes];
        M2 --> M3[Link Notes to Skill Tree Nodes];
        G --> N{View Skill Tree};
        N --> N1[Interconnected Mind Map Visualization];
        N1 --> N2[View Skill Levels & Arsenal Links];
        N2 --> N3[See Missing Skills for Goals];
        N3 --> N4[View Quest Contributions to Skills];
    end

    subgraph "Phase 2: Social & Browser Extension"
        G --> O{Social Features};
        O --> P[Create/Join Party with Rules];
        P --> P1[Invite Friends Directly];
        P --> P2[Browse & Invite Strangers];
        P --> P3[Set Party as Open to Join];
        P1 --> Q[Access Party Stash with Permissions];
        P2 --> Q;
        P3 --> Q;
        Q --> R[Schedule Study Meetings];
        R --> R1[Set Meeting Type & Details];
        R1 --> R2[Conduct Meeting with Recording];
        R2 --> R3[Generate AI Meeting Summary];
        R3 --> R4[Share Action Items & Results];
        O --> S[Join/Create Guilds];
        S --> S1[Use Guild Materials in Quest Line];
        G --> T{Browser Extension};
        T --> T1[Auto-extract Academic Documents];
        T1 --> T2[Organize Content into Arsenal];
        T --> T3[Highlight Text for Arsenal Popup];
        T3 --> T4[View Relevant Notes by Keywords];
    end

    subgraph "Notifications & AI Interaction"
        AI8 --> U[Dynamic Quest Line Updates];
        U --> V[Monitor SignalR Hubs and Realtime Channels];
        V --> V1[Quest Changes];
        V --> V2[New Learning Path Suggestions];
        V --> V3[Email & Push Notifications];
        M2 --> W[AI Scans Arsenal Content];
        W --> X[Proactive Study Suggestions];
    end
```

## Guild Master Flow (Student or Verified Lecturer)

```mermaid
graph TD
    subgraph "Guild Creation & Verification"
        A[Start] --> B{User Type?};
        B -- Student --> C[Create Basic Guild];
        B -- Lecturer --> D[Upload Academic Credentials];
        D --> E[Academic Credential Validation];
        E --> F[Become Verified Lecturer];
        F --> G[Create Guild with Enhanced Privileges];
        C --> H[Guild Management Dashboard];
        G --> H;
    end

    subgraph "Phase 3: Guild Management & Analytics"
        H --> I[View Aggregated Student Progress];
        I --> J[Identify Struggling Topics/Boss Fights];
        H --> K[Upload Reference Materials];
        K --> L[Share Materials with Guild Members];
        H --> M[Create Guild Announcements];
        M --> N[Send Communications to Members];
        H --> O[Basic Member Management];
        O --> P[Streamlined Educational Content Delivery];
    end

    subgraph "Student Integration"
        L --> Q[Students Access Materials];
        Q --> R[Materials Supplement Personal Quest Lines];
        N --> S[Students Receive Notifications];
    end
```

## Party Leader Flow

```mermaid
graph TD
    subgraph "Party Creation & Setup"
        A[Student decides to create a Party] --> B[Becomes Party Leader];
        B --> C[Set Party Rules & Permissions];
        B --> D{Invite Members};
        D --> E[Invite Friends Directly];
        D --> F[Browse & Invite Strangers by Stats];
        D --> G[Set Party as Open to Join];
        E --> H[Manage Party];
        F --> H;
        G --> H;
    end

    subgraph "Party Stash Management"
        H --> I[Configure Party Stash Permissions];
        I --> J[Set Individual Member Permissions];
        J --> K{Permission Types};
        K --> L[View-Only Access];
        K --> M[Edit Permissions];
        K --> N[Comment-Only Permissions];
        L --> O[Members Access Shared Arsenal];
        M --> O;
        N --> O;
    end

    subgraph "Enhanced Meeting Management"
        H --> P{Schedule Study Meeting};
        P --> Q[Set Meeting Details];
        Q --> Q1[Meeting Title & Description];
        Q --> Q2[Meeting Type Selection];
        Q2 --> Q3[Study Session];
        Q2 --> Q4[Project Discussion];
        Q2 --> Q5[Exam Prep];
        Q2 --> Q6[General Meeting];
        Q1 --> R[Send Meeting Invitations];
        Q3 --> R;
        Q4 --> R;
        Q5 --> R;
        Q6 --> R;
        R --> S[Conduct Meeting];
    end

    subgraph "Meeting Recording & AI Processing"
        S --> T{Content Capture Methods};
        T --> U[Manual Entry];
        T --> V[Audio Transcription];
        T --> W[Automated Capture via Browser Extension];
        U --> X[Generate Comprehensive Summary];
        V --> X;
        W --> X;
        X --> Y[AI-Generated Summary Options];
        Y --> Z[Executive Summary];
        Y --> AA[Key Discussion Points];
        Y --> BB[Action Items with Assignments];
        Y --> CC[Next Steps & Resources];
        Y --> DD[Study Materials Covered];
        Y --> EE[Unresolved Questions];
        Z --> FF[Share Results with Party];
        AA --> FF;
        BB --> FF;
        CC --> FF;
        DD --> FF;
        EE --> FF;
    end
```

## System Admin Flow

```mermaid
graph TD
    A[Start] --> B[Access Admin Dashboard];

    subgraph "System Analytics & Monitoring"
        B --> C{View Analytics};
        C --> D[User Engagement Metrics];
        D --> D1[Weekly Active Users];
        D --> D2[Quest Completion Rates];
        D --> D3[Party Creation Rates];
        D --> D4[User Retention Rates];
        C --> E[Platform Performance];
        E --> E1[API Response Times];
        E --> E2[System Uptime Monitoring];
        E --> E3[Error Rate Analysis];
        C --> F[Educational Analytics];
        F --> F1[Class & Route Popularity];
        F --> F2[Boss Fight Performance];
        F --> F3[Skill Tree Progression];
    end

    subgraph "Platform Management"
        B --> G{System Configuration};
        G --> H[Manage Notification Settings];
        G --> I[Configure Quest Generation Parameters];
        G --> J[Monitor AI Processing Performance];
        G --> K[Manage User Authentication - Supabase];
        G --> L[Database & Storage Management - Supabase];
    end

    subgraph "Content & User Management"
        B --> M{Content Moderation};
        M --> N[Review User-Generated Content];
        M --> O[Manage Guild Verification Requests];
        M --> P[Monitor Academic Credential Validation];
        M --> Q{User Management};
        Q --> R[Role-Based Access Control];
        Q --> S[Data Privacy & GDPR Compliance];
        Q --> T[User Data Retention Policies];
    end

    subgraph "Integration & Technical Operations"
        B --> U{System Integrations};
        U --> V[Monitor SignalR Hubs and Realtime Channels];
        U --> W[Unity WebGL Performance Monitoring];
        U --> X[Browser Extension Analytics];
        U --> Y[Third-party API Health Checks];
    end
```
