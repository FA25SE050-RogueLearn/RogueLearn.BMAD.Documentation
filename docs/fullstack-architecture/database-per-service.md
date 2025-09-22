# **Database Per Service Documentation**

This document outlines which database tables and collections are used by each microservice in the RogueLearn architecture.

## **PostgreSQL Tables by Service**

### **User Service** (`roguelearn-user-service`)
**Purpose**: Manages user profiles, authentication, academic structure, achievements, and user progress tracking

**PostgreSQL Tables**:
- `UserProfiles` - Core user profile data linked to Supabase auth.users (AuthUserId as primary key)
- `Roles` - System roles for RBAC
- `UserRoles` - User role assignments
- `LecturerVerificationRequests` - Lecturer verification workflow
- `Classes` - Academic class definitions
- `CurriculumPrograms` - Abstract program definitions (e.g., "B.S. in Software Engineering")
- `CurriculumVersions` - Versioned curriculum snapshots (e.g., "K18A", "K18B") with effective year tracking
- `Subjects` - Master list of all subjects with codes and credits
- `CurriculumStructure` - Junction table defining curriculum-subject relationships with term sequencing
- `SyllabusVersions` - Versioned syllabi for each subject
- `StudentEnrollments` - Links students to specific curriculum versions with enrollment tracking
- `StudentTermSubjects` - Comprehensive academic term tracking with status (Enrolled, Completed, Failed, Withdrawn)
- `UserSkills` - User skill progression tracking with experience points
- `UserQuestProgress` - Summary of user progress on quests for quick reference and cross-service sync
- `Achievements` - Central catalog of all possible achievements with type categorization
- `UserAchievements` - Links users to earned achievements with context and timestamps
- `Notifications` - User notification system with type-based categorization

### **Quests Service** (`roguelearn-quests-service`)
**Purpose**: Manages quest lines, quests, skill trees, game sessions, and learning content with comprehensive progress tracking

**PostgreSQL Tables**:
- `QuestLines` - Organized sequences of learning objectives for entire academic journeys with curriculum integration
- `QuestChapters` - Time-boxed containers representing weeks or major milestones (e.g., "Week 5", "Midterm Prep")
- `Quests` - Individual learning tasks within chapters with rewards, metadata, and flexible quest types
- `QuestDependencies` - Quest prerequisite relationships enabling complex learning paths
- `SkillTrees` - Skill tree structures and hierarchies for different domains
- `Skills` - Individual skill definitions with experience requirements and level progression
- `SkillDependencies` - Skill prerequisite relationships for progressive mastery
- `QuestSkills` - Links quests to skill development with experience point rewards
- `GameSessions` - Comprehensive game session tracking with user engagement analytics
- `SessionEvents` - Detailed interaction logging for learning analytics and adaptive content
- `Notes` - User-generated content linked to quests and skills for collaborative knowledge sharing

### **Social Service** (`roguelearn-social-service`)
**Purpose**: Manages parties, guilds, events, leaderboards, and collaborative knowledge sharing

**PostgreSQL Tables**:
- `Parties` - Party/group definitions with configurable join types
- `PartyMemberships` - Party membership tracking with join timestamps
- `PartyStashItems` - Shared note repository for party collaboration
- `Guilds` - Guild definitions with verification status
- `GuildMemberships` - Guild membership tracking (unique per user)
- `Events` - Event definitions for competitions and activities
- `LeaderboardEntries` - Individual user rankings per event
- `GuildLeaderboardEntries` - Guild-based competitive rankings

### **Meeting Service** (`roguelearn-meeting-service`)
**Purpose**: Manages multi-context meetings, scheduling, participant tracking, and comprehensive meeting analytics

**PostgreSQL Tables**:
- `Meetings` - Meeting definitions supporting both party and guild contexts with flexible scheduling
- `MeetingParticipants` - Meeting invitation and attendance tracking with status management
- `MeetingAgenda` - Structured meeting agenda items with sequencing and time allocation
- `MeetingNotes` - Collaborative note-taking during meetings with participant attribution
- `MeetingParticipantActivity` - Detailed participant activity tracking (check-in/out, device info, connection quality)
- `MeetingParticipantEngagement` - Real-time engagement tracking (speaking time, screen sharing, chat participation)
- `MeetingParticipantStats` - Aggregated participant performance and engagement metrics for analytics

### **Code Battle Service** (`roguelearn-code-battle-service`)
**Purpose**: Manages code problems, real-time battles, compilation, execution, and competitive programming

**PostgreSQL Tables**:
- `languages` - Supported programming languages with execution configurations
- `code_problems` - Code challenge problem definitions with statements and difficulty
- `tags` - Categorization tags for code problems
- `code_problem_tags` - Many-to-many relationship between problems and tags
- `code_problem_language_details` - Language-specific starter code and templates
- `event_code_problems` - Association between events and available problems
- `test_cases` - Input/output test cases with JSON data and hidden flag
- `rooms` - Battle rooms within events for competitive coding
- `room_players` - Player participation with scoring and state tracking
- `submissions` - Room-based submission tracking with execution time
- `leaderboard_entries` - User rankings and scores for events with snapshot dates
- `guild_leaderboard_entries` - Guild-based rankings and total scores for events
- `event_guild_participants` - Guild participation tracking in events

**Qdrant Collections**:
- `code_embeddings` - Vector embeddings of code submissions for similarity analysis
- `problem_embeddings` - Vector embeddings of problem statements for matching
- `solution_patterns` - Common solution pattern embeddings for automated scoring

## **Shared Data Access Patterns**

### **Cross-Service Data Access**
Some services may need to read data from other services' primary tables:

- **User Service** provides user profile data to all other services
- **Quests Service** quest and skill data is referenced by Social Service for achievements
- **Social Service** party and guild data is used by Meeting Service for scheduling
- **Code Battle Service** may reference user profiles for submission tracking

### **Data Synchronization**
- User profile changes in User Service trigger updates to related services
- Quest completion in Quests Service updates user experience points in User Service
- Social achievements in Social Service may unlock new quests in Quests Service

## **Database Connection Strategy**

### **Primary Database Access**
Each service maintains its own database connection pool to the shared PostgreSQL instance with appropriate permissions:

- **Read/Write Access**: Services have full access to their primary tables
- **Read-Only Access**: Services have read access to commonly referenced tables (UserProfiles, etc.)
- **No Direct Access**: Services use API calls for complex cross-service operations

### **Qdrant Access**
Only the Code Battle Service has direct access to Qdrant collections for vector operations.

## **Future Considerations**

### **Database Sharding**
As the system scales, consider:
- Sharding user data by geographic region or user ID ranges
- Separating read replicas for analytics and reporting
- Moving high-frequency data (game sessions, notifications) to dedicated instances

### **Microservice Database Independence**
Future evolution may include:
- Splitting shared tables into service-specific databases
- Implementing event-driven data synchronization between services
- Using CQRS patterns for complex read/write scenarios