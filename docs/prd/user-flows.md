# User Interaction Flows

This document visualizes the primary interaction flows for each user role within the QuestLearn platform using Mermaid diagrams.

## Student (Player) Flow

```mermaid
graph TD
    subgraph "User Interaction"
        A[Start] --> B{New User?};
        B -- Yes --> C[Create Account & Onboard];
        C --> D[Select Class & Route];
        D --> E{Provide Academic Docs};
        E -- Manual Upload --> E_MANUAL[Upload Docs];
        E_MANUAL --> F[View Character Sheet];
        F --> G{Manage Notes}; 
        G --> H[Create/Organize Notes];
        F --> I{Engage with Quests};
        I --> J[View Main Quest Line];
        J --> K[Complete Tasks/Events];
        F --> M{Social Interaction};
        M --> N[Create/Join Party];
        N --> O[Use Party Stash];
        M --> P[View Leaderboards];
        B -- No --> F;
    end

    subgraph "System & AI Interaction"
        E_MANUAL -- Ingested by --> S1[System];
        E_EXTRACT -- Ingested by --> S1;
        S1 -- Parsed by --> AI1[AI Engine];
        AI1 --> J;
        AI1 -- Influences --> F;
        K -- Triggers --> L[Face Boss Fights];
        S1 -- Generates --> L;
        J -- Dynamically Updated by --> AI1;
        S1 -- Sends --> Notif[Notifications];
        Notif --> A;
    end

    subgraph "Browser Extension"
        E -- Use Extension --> E_EXTRACT[Extract Docs];
        E_EXTRACT --> F;
        Ext1[Browser Extension] -- Scans & Extracts Web Content --> G;
        Ext1 -- Displays Relevant Notes --> G;
    end
```

## Lecturer (Guild Master) Flow

```mermaid
graph TD
    subgraph "Lecturer Interaction"
        A[Start] --> B[Create/Manage Guild];
        B --> C[Upload Course Materials];
        C --> D{Monitor Students};
        D --> E[View Anonymized Progress];
        E --> F[Identify Struggling Topics];
        D --> G{Create Content};
        G --> H[Draft Side Quests];
        H --> I[Assign Quests to Guild];
    end

    subgraph "AI Interaction"
        C -- Assisted by --> AI1[AI Assistant];
        AI1 --> H;
    end
```

## Tutor (The Guide) Flow

```mermaid
graph TD
    A[Start] --> B{Assigned to Student};
    B -- Yes --> C[View Student Progress];
    C --> D[Access Course-Specific Notes];
    D --> E[Create/Assign Training Drills];
```

## Party Leader Flow

```mermaid
graph TD
    A[Student decides to create a Party] --> B[Becomes Party Leader];
    B --> C[Set Party Rules & Permissions];
    B --> D{Invite Members};
    D --> E[Invite Friends Directly];
    D --> F[Browse and Invite Strangers];
    C --> G[Manage Party];
    G --> H[Use Shared Party Stash];
```

## System Admin (The Game Master) Flow

```mermaid
graph TD
    A[Start] --> B[Access Back-End Interface];
    B --> C[Monitor Application Health];
    C --> D[Oversee Platform Performance];
```