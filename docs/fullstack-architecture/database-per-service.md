# **Database Per Service Documentation**

This document outlines which database tables and collections are used by each microservice in the RogueLearn architecture.

## **PostgreSQL Tables by Service**

### **User Service** (`roguelearn-user-service`)
**Purpose**: Manages user profiles, preferences, and user-related operations (authentication handled by Supabase Auth)

**PostgreSQL Tables**:
- `UserProfiles` - Core user profile data linked to Supabase auth.users
- `Roles` - System roles for RBAC
- `UserRoles` - User role assignments
- `LecturerVerificationRequests` - Lecturer verification workflow
- `Classes` - Academic class definitions
- `Curriculums` - Curriculum definitions
- `UserSyllabusEnrollments` - User enrollment in syllabuses
- `UserSkills` - User skill progression tracking
- `Notifications` - User notifications

### **Quests Service** (`roguelearn-quests-service`)
**Purpose**: Manages syllabi, quests, skill trees, and game session logic

**PostgreSQL Tables**:
- `Syllabuses` - Course syllabi information
- `UserUploadedSyllabuses` - User-uploaded syllabus files and processing status
- `QuestLines` - Quest line containers
- `Quests` - Individual quest definitions
- `QuestPrerequisites` - Quest dependency relationships
- `SkillTrees` - Skill tree structures
- `Skills` - Individual skill definitions
- `SkillDependencies` - Skill prerequisite relationships
- `GameSessions` - Game session tracking and progress
- `Notes` - User notes linked to quests and skills

### **Social Service** (`roguelearn-social-service`)
**Purpose**: Manages parties, guilds, events, and real-time features like duels

**PostgreSQL Tables**:
- `Parties` - Party/group definitions
- `PartyMemberships` - Party membership tracking
- `Guilds` - Guild definitions and verification
- `GuildMemberships` - Guild membership tracking
- `Events` - Event definitions (excluding meetings)
- `LeaderboardEntries` - Individual user leaderboards
- `GuildLeaderboardEntries` - Guild-based leaderboards

### **Meeting Service** (`roguelearn-meeting-service`)
**Purpose**: Manages party meetings, scheduling, and meeting-related features

**PostgreSQL Tables**:
- `Meetings` - Meeting definitions and scheduling (to be added)
- `MeetingParticipants` - Meeting attendance tracking (to be added)
- `MeetingAgendas` - Meeting agenda items (to be added)
- `MeetingNotes` - Meeting notes and minutes (to be added)

*Note: Meeting-specific tables need to be added to the database schema*

### **Code Battle Service** (`roguelearn-code-battle-service`)
**Purpose**: Compiles, runs, and scores user-submitted code

**PostgreSQL Tables**:
- `CodeProblems` - Code challenge problem definitions
- `EventCodeProblems` - Association between events and code problems
- `CodeSubmissions` - User code submissions and results

**ChromaDB Collections**:
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

### **ChromaDB Access**
Only the Code Battle Service has direct access to ChromaDB collections for vector operations.

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