# RogueLearn Services Ecosystem

## Service Architecture Overview

```mermaid
graph TB
    %% External Systems
    subgraph "External Systems"
        SA[Supabase Auth]
        PC[Pinecone]
        RL[Relay Netcode]
    end

    %% Core Services
    subgraph "RogueLearn Microservices"
        US[User Service<br/>roguelearn-user-service]
        QS[Quests Service<br/>roguelearn-quests-service]
        SS[Social Service<br/>roguelearn-social-service]
        MS[Meeting Service<br/>roguelearn-meeting-service]
        CBS[Event Service<br/>roguelearn-event-service]
    end

    %% Client Applications
    subgraph "Client Applications"
        WEB[Web Application]
        MOB[Mobile Application]
        API[API Gateway]
        UNITY[Unity WebGL (embedded)]
    end

    %% Unity is embedded in Web App %%
    WEB -.->|Embedded in Web App| UNITY

    %% Authentication Flow
    WEB --> SA
    MOB --> SA
    SA --> US

    %% API Gateway
    WEB --> API
    MOB --> API
    UNITY --> API
    API --> US
    API --> QS
    API --> SS
    API --> MS
    API --> CBS

    %% Inter-Service Dependencies
    QS --> US
    SS --> US
    MS --> US
    MS --> SS
    CBS --> US
    CBS --> SS

    %% External Service Dependencies
    CBS --> PC
    UNITY --> RL

    %% Data Flow Indicators
    US -.->|User Context| QS
    US -.->|User Context| SS
    US -.->|User Context| MS
    US -.->|User Context| CBS
    QS -.->|Progress Updates| US
    SS -.->|Achievement Triggers| US
    MS -.->|Meeting Analytics| US
    CBS -.->|Event Results| US
    SS -.->|Event Context| CBS
    SS -.->|Party/Guild Context| MS
    QS -.->|Notes Sharing| SS

    classDef service fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef external fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef client fill:#f3e5f5,stroke:#4a148c,stroke-width:2px

    class US,QS,SS,MS,CBS service
    class SA,PC,RL external
    class WEB,MOB,API,UNITY client
```

## Service Responsibilities & Boundaries

### **User Service** (`roguelearn-user-service`)
**Primary Domain**: Identity, Academic Structure, Achievement Management

**Core Responsibilities**:
- User authentication integration with Supabase Auth
- User profile and role management
- Academic structure (Classes, Curriculum Programs, Student Enrollments)
- Achievement system and user achievement tracking
- Quest progress summary maintenance
- Lecturer verification and administrative functions

**Key Entities**: `UserProfiles`, `Roles`, `Classes`, `CurriculumPrograms`, `StudentEnrollments`, `Achievements`, `UserAchievements`, `UserQuestProgress`

**External Dependencies**: 
- Supabase Auth for authentication
- Receives progress updates from Quests Service
- Receives achievement triggers from all services

---

### **Quests Service** (`roguelearn-quests-service`)
**Primary Domain**: Learning Content, Gamification, Progress Tracking

**Core Responsibilities**:
- Quest and quest line management
- Skill trees and skill development tracking
- User progress monitoring and analytics
- Game session management and experience tracking
- Notes system for learning context
- AI-assisted quest generation

**Key Entities**: `QuestLines`, `Quests`, `SkillTrees`, `Skills`, `UserSkill`, `GameSessions`, `Notes`

**Service Dependencies**:
- User Service: User profiles, curriculum data
- AI Service: Quest generation assistance

**Data Sharing**:
- Provides notes to Social Service for party stash functionality
- Sends progress updates to User Service

---

### **Social Service** (`roguelearn-social-service`)
**Primary Domain**: Community, Events, Competition Management

**Core Responsibilities**:
- Party system for small group collaboration
- Guild system for larger community organization
- Event management and leaderboards
- Collaborative knowledge sharing through party stash
- Social features and community engagement

**Key Entities**: `Parties`, `PartyMemberships`, `Guilds`, `GuildMemberships`, `Events`, `LeaderboardEntries`, `PartyStashItems`

**Service Dependencies**:
- User Service: User profiles and context
- Quests Service: Notes for party stash sharing

**Integration Points**:
- Provides event context to Code Battle Service
- Provides party/guild context to Meeting Service
- Triggers achievements in User Service

---

### **Meeting Service** (`roguelearn-meeting-service`)
**Primary Domain**: Collaboration, Meeting Management, Activity Analytics

**Core Responsibilities**:
- Multi-context meeting management (parties and guilds)
- Meeting scheduling and participant management
- Detailed participant activity tracking and analytics
- Meeting engagement metrics and performance insights
- Agenda management and meeting notes

**Key Entities**: `Meetings`, `MeetingParticipants`, `MeetingAgenda`, `MeetingNotes`, `MeetingParticipantActivity`, `MeetingParticipantEngagement`, `MeetingParticipantStats`

**Service Dependencies**:
- User Service: User profiles
- Social Service: Party and guild context for meetings

**Analytics Capabilities**:
- Real-time participant engagement tracking
- Meeting performance analytics
- Attendance and participation metrics

---

### **Event Service** (`roguelearn-event-service`)
**Primary Domain**: Event Management, Code Execution

**Core Responsibilities**:
- Event management and coordination
- Code problem management and storage
- Real-time competitive coding battles
- Code compilation and execution
- Submission tracking and evaluation
- Programming language support management

**Key Entities**: `CodeProblems`, `Submissions`, `Languages`, `Rooms`, `RoomPlayers`, `TestCases`

**Service Dependencies**:
- User Service: User profiles
- Social Service: Event context for competitions

**External Dependencies**:
- Pinecone: Vector embeddings for problem similarity

---

## Data Flow Patterns

### **Authentication & User Context Flow**
```
Supabase Auth → User Service → All Other Services
```
- Centralized authentication through Supabase Auth
- User Service maintains user profiles and provides context to all services
- All services reference users through `AuthUserId` from Supabase Auth

### **Learning Journey Flow**
```
User Service (Curriculum) → Quests Service (Content) → User Service (Progress)
```
- Academic structure defined in User Service
- Learning content and gamification managed by Quests Service
- Progress summaries maintained in User Service for academic tracking

### **Social & Collaboration Flow**
```
Social Service (Community) ↔ Meeting Service (Collaboration)
Social Service (Events) ↔ Event Service (Competition)
```
- Social Service provides community context for meetings and competitions
- Meeting Service enables collaboration within parties and guilds
- Event Service provides competitive programming within events

### **Achievement & Analytics Flow**
```
All Services → User Service (Achievements)
Meeting Service → User Service (Analytics)
```
- All services can trigger achievements in User Service
- Meeting Service provides detailed analytics for learning insights

## Integration Architecture

### **API Gateway Pattern**
- Centralized API Gateway routes requests to appropriate services
- Handles authentication, rate limiting, and request routing
- Provides unified API interface for client applications

### **Event-Driven Communication**
- Services communicate through events for loose coupling
- Achievement triggers, progress updates, and analytics data flow through events
- Ensures eventual consistency across service boundaries

### **Shared Data References**
- Services use UUIDs for cross-service references
- Foreign key relationships maintain data integrity
- Cascade deletes ensure proper cleanup across services

### **External System Integration**
- **Supabase Auth**: Centralized authentication and user management
- **Pinecone**: Vector database for AI-powered features

## Scalability & Performance Considerations

### **Service Independence**
- Each service can scale independently based on usage patterns
- Database per service pattern ensures data ownership
- Microservice architecture enables technology diversity

### **Caching Strategy**
- User context cached across services for performance
- Frequently accessed data cached at API Gateway level
- Read replicas for analytics and reporting workloads

### **Load Distribution**
- Event Service handles compute-intensive operations
- Meeting Service manages real-time collaboration features
- Social Service handles community and event management
- Quests Service processes learning content and gamification

This ecosystem design provides a comprehensive, scalable, and maintainable architecture for the RogueLearn gamified learning platform, with clear service boundaries, well-defined data flows, and robust integration patterns.