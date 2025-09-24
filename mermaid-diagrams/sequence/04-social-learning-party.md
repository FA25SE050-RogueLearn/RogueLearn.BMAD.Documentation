# Social Learning & Party Collaboration Flow

```mermaid
sequenceDiagram
    participant S as Student User
    participant W as Web Interface
    participant PS as Party Service
    participant MS as Meeting Service
    participant RS as Recording Service
    participant BE as Browser Extension
    participant AI as AI Service
    participant NS as Notification Service
    participant DB as Database

    Note over S,DB: Social Learning & Party System Flow

    %% Party Creation
    S->>W: Create/Join Study Party
    W->>PS: Process party request
    PS->>DB: Store party data
    PS->>NS: Send party notifications
    NS->>S: Party confirmation

    %% Meeting Scheduling
    S->>W: Schedule study meeting
    W->>MS: Create meeting
    MS->>DB: Store meeting details
    MS->>NS: Send meeting invites
    NS->>S: Meeting notifications

    %% Meeting Start & Recording
    S->>W: Start meeting
    W->>MS: Initialize meeting session
    MS->>RS: Activate built-in recording system
    Note over RS: System handles all meeting recording
    RS->>DB: Initialize recording session

    %% Content Capture (System-based)
    loop During Meeting
        S->>W: Participate in discussion
        W->>RS: Capture discussion content
        RS->>DB: Store meeting content
        
        S->>BE: Browse external resources
        BE->>W: Capture external web content
        W->>DB: Store external resources
        Note over BE: Extension only captures external web resources
    end

    %% Meeting End & Processing
    S->>W: End meeting
    W->>MS: Close meeting session
    MS->>RS: Stop recording
    RS->>AI: Process recorded content (post-meeting)
    AI->>DB: Generate meeting summary
    AI->>NS: Send summary notifications
    NS->>S: Meeting summary delivered
```