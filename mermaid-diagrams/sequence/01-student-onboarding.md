# Student Onboarding & Character Creation Flow

```mermaid
sequenceDiagram
    participant U as Student User
    participant UI as Web Interface
    participant Auth as Authentication Service
    participant AI as AI System
    participant DB as Database
    participant STree as Skill Tree Service
    participant Quest as Quest Generator

    U->>UI: Access RogueLearn platform
    UI->>Auth: Initiate account creation
    Auth->>UI: Present registration form
    
    U->>UI: Fill registration details
    UI->>Auth: Submit user credentials
    Auth->>DB: Store user account
    Auth->>UI: Account created successfully
    
    UI->>U: Present "Character Creation" interface
    U->>UI: Select Class (career goal)
    U->>UI: Select Route (academic path)
    
    U->>UI: Upload academic documents
    UI->>AI: Process uploaded documents
    AI->>AI: Parse GPA, transcripts, schedules
    AI->>DB: Store processed academic data
    
    AI->>STree: Generate initial skill tree
    STree->>AI: Skill tree structure created
    AI->>Quest: Create welcome quest line
    Quest->>AI: Initial quests generated
    
    AI->>DB: Store character stats & progress
    DB->>UI: Character creation complete
    UI->>U: Display Character Sheet dashboard
    
    Note over U,Quest: User now has personalized<br/>learning profile ready
```