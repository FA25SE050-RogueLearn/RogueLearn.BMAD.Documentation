# RogueLearn System Overview ERD

## Entity Relationship Diagram

```mermaid
---
config:
  layout: elk
---
erDiagram
    %% Authentication & User Management
    AuthUsers ||--|| UserProfile : authenticates
    
    %% Academic Structure
    "Class" ||--o{ UserProfile : contains
    UserProfile }o--|| CurriculumProgram : follows
    CurriculumProgram ||--o{ CurriculumVersion : includes
    CurriculumProgram ||--o{ SkillTree : develops
    
    %% Learning Journey
    UserProfile ||--o{ StudentEnrollment : enrolls_in
    CurriculumVersion ||--o{ StudentEnrollment : enrolled_by
    CurriculumProgram ||--o{ QuestLine : generates
    QuestLine ||--o{ Quest : contains
    Subject ||--o{ Quest : generates
    SkillTree ||--o{ Skill : branches_into
    
    %% Game Mechanics
    UserProfile ||--o{ GameSession : plays
    Quest ||--o{ GameSession : engaged_in
    UserProfile ||--o{ UserQuestProgress : tracks
    
    %% Achievement System
    UserProfile ||--o{ UserAchievement : earns
    Achievement ||--o{ UserAchievement : awarded_as
    
    %% Social Dynamics
    UserProfile ||--o{ Party : leads
    UserProfile ||--o| Guild : masters
    Guild ||--o{ Event : joins
    
    %% Competitive Elements
    UserProfile ||--o{ LeaderboardEntry : ranks_in
    Event ||--o{ LeaderboardEntry : scored_in
    Event ||--o{ Room : creates
    Event ||--o{ EventCodeProblem : includes
    CodeProblem ||--o{ EventCodeProblem : featured_in
    
    %% Meeting & Collaboration
    Party ||--o{ Meeting : conducts
    UserProfile ||--o{ Meeting : creates
    
    %% Code Battle Integration
    CodeProblem ||--o{ Submission : solved_by
    Language ||--o{ Submission : written_in
```

## System Architecture Overview

### Service Boundaries & Responsibilities

#### **User Service** (`roguelearn-user-service`)
- **Primary Responsibility**: User identity, profiles, roles, academic structure, and achievement tracking
- **Key Entities**: UserProfiles, Roles, Classes, CurriculumPrograms, StudentEnrollments, UserQuestProgress, Achievements, UserAchievements
- **External Integration**: Supabase Auth for authentication
- **Cross-Service Impact**: Provides user context to all other services and maintains quest progress summaries

#### **Quests Service** (`roguelearn-quests-service`)
- **Primary Responsibility**: Learning content, gamification, and progress tracking
- **Key Entities**: Subjects, Quests, Skills, GameSessions
- **Dependencies**: User Service (UserProfiles), Academic structure (CurriculumPrograms)
- **Cross-Service Impact**: Drives user engagement and learning progression

#### **Social Service** (`roguelearn-social-service`)
- **Primary Responsibility**: Community features, events, competition, and collaborative knowledge sharing
- **Key Entities**: Parties, Guilds, Events, LeaderboardEntries, PartyStashItems
- **Dependencies**: User Service (UserProfiles), Quests Service (Notes for party stash)
- **Cross-Service Impact**: Enables collaborative learning, competition, and shared knowledge repositories

#### **Meeting Service** (`roguelearn-meeting-service`)
- **Primary Responsibility**: Multi-context collaboration (parties and guilds), meeting management, and participant activity analytics
- **Key Entities**: Meetings, MeetingParticipants, MeetingAgenda, MeetingNotes, MeetingParticipantActivity, MeetingParticipantEngagement, MeetingParticipantStats
- **Dependencies**: Social Service (Parties, Guilds), User Service (UserProfiles)
- **Cross-Service Impact**: Facilitates team collaboration across parties and guilds, study groups, and provides detailed meeting analytics

#### **Code Battle Service** (`roguelearn-code-battle-service`)
- **Primary Responsibility**: Code compilation, execution, and competitive programming
- **Key Entities**: CodeProblems, Submissions, Languages, Rooms
- **Dependencies**: Social Service (Events), User Service (UserProfiles)
- **External Integration**: Qdrant for vector embeddings, Judge0 for code execution

### Data Flow Patterns

#### **User Journey Flow**
1. **Authentication**: Supabase Auth → User Service (UserProfiles)
2. **Onboarding**: User Service → Academic assignment (Classes, Curriculums)
3. **Learning**: Quests Service → Content delivery and progress tracking → User Service (UserQuestProgress sync)
4. **Achievement**: Cross-service events → User Service (Achievement awards)
5. **Social**: Social Service → Community engagement and events
6. **Collaboration**: Meeting Service → Team study sessions with detailed activity tracking and engagement analytics
7. **Competition**: Code Battle Service → Competitive programming challenges

#### **Cross-Service Data Dependencies**
- **UserProfiles**: Referenced by all services for user context
- **UserQuestProgress**: Maintained by User Service, updated by Quests Service
- **Achievements**: Managed by User Service, triggered by all services
- **Events**: Bridge between Social Service and Code Battle Service
- **Curriculums**: Shared between User Service and Quests Service
- **Parties**: Bridge between Social Service and Meeting Service for party-based meetings
- **Guilds**: Bridge between Social Service and Meeting Service for guild-based meetings
- **Notes**: Referenced by Social Service (PartyStashItems) for collaborative knowledge sharing
- **Meeting Analytics**: Meeting Service maintains detailed participant activity data for performance insights

### Scalability Considerations

#### **Service Independence**
- Each service manages its own primary data
- Cross-service references use UUIDs for loose coupling
- Services can scale independently based on usage patterns

#### **Data Consistency**
- Foreign key relationships maintain referential integrity
- Cascade deletes ensure data cleanup
- Event-driven synchronization for cross-service updates

#### **Performance Optimization**
- Indexed foreign keys for cross-service queries
- Read replicas for analytics and reporting
- Caching layers for frequently accessed user data

### Integration Points

#### **External Systems**
- **Supabase Auth**: User authentication and session management
- **Qdrant**: Vector embeddings for code similarity and problem matching
- **Judge0**: Code compilation and execution service

#### **Internal APIs**
- RESTful APIs for synchronous operations
- Event streaming for asynchronous updates
- GraphQL federation for unified data access

This system design enables a comprehensive gamified learning platform with strong separation of concerns, scalable architecture, and rich feature integration across all learning aspects.