# **Database Per Service Documentation**

This document outlines which database tables and collections are used by each microservice in the RogueLearn architecture.

## **PostgreSQL Tables by Service**

### **User Service** (`roguelearn-user-service`)
**Purpose**: Manages user profiles, authentication, academic structure, achievements, and user progress tracking

**PostgreSQL Tables**:
- `user_profiles` - Core user profile data linked to Supabase auth.users (auth_user_id as primary key)
- `roles` - System roles for RBAC
- `user_roles` - User role assignments
- `lecturer_verification_requests` - Lecturer verification workflow
- `classes` - Academic class definitions
- `curriculum_programs` - Abstract program definitions (e.g., "B.S. in Software Engineering")
- `curriculum_versions` - Versioned curriculum snapshots (e.g., "K18A", "K18B") with effective year tracking
- `subjects` - Master list of all subjects with codes and credits
- `curriculum_structure` - Junction table defining curriculum-subject relationships with term sequencing
- `syllabus_versions` - Versioned syllabus for each subject
- `student_enrollments` - Links students to specific curriculum versions with enrollment tracking
- `student_term_subjects` - Comprehensive academic term tracking with status (Enrolled, Completed, Failed, Withdrawn)
- `skills` - Skill Catalog of named competencies with domain/tier metadata
- `skill_dependencies` - Skill Tree graph relationships among skills (prerequisites, complements)
- `user_skills` - User skill progression tracking with experience points
- `user_quest_progress` - Summary of user progress on quests for quick reference and cross-service sync
- `user_skill_rewards` - Event-sourced ledger of XP rewards applied to user skills
- `achievements` - Central catalog of all possible achievements with type categorization
- `user_achievements` - Links users to earned achievements with context and timestamps
- `notifications` - User notification system with type-based categorization

### **Quests Service** (`roguelearn-quests-service`)
**Purpose**: Manages quest lines, quests, game sessions, and learning content with comprehensive progress tracking. Consumes the Skill Catalog from User Service for tagging, recommendations, and progression logic.

**PostgreSQL Tables**:
- `learning_paths` - Structured sequences of quests for comprehensive learning
- `quests` - Core quest definitions with metadata and configuration
- `quest_steps` - Individual steps within a quest for structured progression
- `learning_path_quests` - Junction table linking quests to learning paths with sequencing
- `quest_assessments` - Assessment configurations for quests requiring evaluation
- `user_quest_attempts` - Tracks user attempts and progress on quests
- `user_quest_step_progress` - Detailed tracking of individual step completion
- `user_learning_path_progress` - Tracks user progress through learning paths
- `quest_submissions` - User submissions for assessed quests
- `quest_analytics` - Aggregated analytics data for quest performance and engagement
- `notes` - User-generated content linked to quests and skills for collaborative knowledge sharing

### **Social Service** (`roguelearn-social-service`)
**Purpose**: Manages parties, guilds, events, leaderboards, and collaborative knowledge sharing

**PostgreSQL Tables**:
- `parties` - Party/group definitions with configurable join types
- `party_members` - Party membership tracking with roles and status
- `party_invitations` - Manages invitations for users to join parties
- `party_activities` - Tracks collaborative activities and achievements within a party
- `party_stash_items` - Shared note repository for party collaboration
- `guilds` - Guild definitions with verification status
- `guild_members` - Guild membership with hierarchical roles
- `guild_invitations` - Manages invitations and applications to join guilds
- `guild_achievements` - Tracks guild-level achievements and milestones
- `friendships` - Manages user-to-user friend connections
- `user_social_stats` - Aggregated social statistics for users
- `social_messages` - Direct, party, and guild communication
- `message_reactions` - Emoji reactions to social messages
- `social_events` - Social learning events and activities
- `event_participants` - Tracks user participation in social events

### **Meeting Service** (`roguelearn-meeting-service`)
**Purpose**: Manages meetings with AI-powered transcription, summarization, and content processing capabilities

**PostgreSQL Tables**:
- `meeting` - Core meeting definitions with scheduling and organizer information
- `meeting_participant` - Participant tracking with join/leave times and role assignments
- `transcript_segment` - AI-generated transcript segments with speaker identification and timing
- `summary_chunk` - Chunked meeting summaries for efficient processing and retrieval
- `meeting_summary` - Complete meeting summaries generated from transcript analysis

### **Code Battle Service** (`roguelearn-code-battle-service`)
**Purpose**: Manages code problems, real-time battles, compilation, execution, and competitive programming

**PostgreSQL Tables**:
- `events` - Event definitions for competitions and activities
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

- **User Service** provides user profile data to all other services, and owns the Skill Catalog (skills and dependencies) and rewards ledger (`user_skill_rewards`)
- **Quests Service** quest data is referenced by Social Service for achievements; publishes reward events (XP, Skill Points, Unlocks) to User Service for authoritative ledgering
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
- **Read-Only Access**: Services have read access to commonly referenced tables (e.g., `user_profiles`)
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