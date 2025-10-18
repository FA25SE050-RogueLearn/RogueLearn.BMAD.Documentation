# RogueLearn System Overview ERD

## Entity Relationship Diagram

```mermaid
erDiagram
    %% Core Services & Entities

    subgraph User Service
        user_profiles {
            uuid auth_user_id PK
            string username
            uuid class_id FK
            uuid route_id FK
        }
        curriculum_programs {
            uuid id PK
            string program_name
        }
        classes {
            uuid id PK
            string name
        }
        notes {
            uuid id PK
            uuid auth_user_id FK
        }
        achievements {
            uuid id PK
            string name
        }
        user_achievements {
            uuid auth_user_id PK,FK
            uuid achievement_id PK,FK
        }
    end

    subgraph Quest Service
        learning_paths {
            uuid id PK
            string name
            uuid curriculum_version_id
        }
        quests {
            uuid id PK
            string title
        }
        user_quest_attempts {
            uuid id PK
            uuid auth_user_id
            uuid quest_id FK
        }
    end

    subgraph Social Service
        parties {
            uuid id PK
            string name
            uuid created_by
        }
        guilds {
            uuid id PK
            string name
            uuid created_by
        }
        party_members {
            uuid party_id PK,FK
            uuid auth_user_id PK
        }
        guild_members {
            uuid guild_id PK,FK
            uuid auth_user_id PK
        }
    end

    subgraph Event Service
        events {
            uuid id PK
            string title
            uuid original_request_id FK
        }
        event_requests {
            uuid id PK
            uuid requester_guild_id
        }
        rooms {
            uuid id PK
            uuid event_id FK
        }
        room_players {
            uuid room_id PK,FK
            uuid user_id PK
        }
        submissions {
            uuid id PK
            uuid user_id
            uuid code_problem_id
        }
        code_problems {
            uuid id PK
            string title
        }
    end

    subgraph Meeting Service
        meeting {
            uuid meeting_id PK
            string title
            uuid organizer_id
        }
        meeting_participant {
            uuid participant_id PK
            uuid meeting_id FK
            uuid user_id
        }
    end

    %% Relationships between Services
    user_profiles }o--|| curriculum_programs : "selects as Route"
    user_profiles }o--|| classes : "selects as Class"
    user_profiles ||--o{ learning_paths : "has"
    user_profiles ||--o{ user_quest_attempts : "attempts"
    user_profiles ||--o{ party_members : "is member of"
    user_profiles ||--o{ guild_members : "is member of"
    user_profiles ||--o{ room_players : "plays in"
    user_profiles ||--o{ submissions : "submits"
    user_profiles ||--o{ meeting_participant : "attends"
    user_profiles ||--o{ user_achievements : "earns"
    achievements ||--o{ user_achievements : " "
    
    curriculum_programs ||--o{ learning_paths : "generates"
    learning_paths ||--o{ quests : "contains"
    quests ||--o{ user_quest_attempts : " "

    parties ||--o{ party_members : " "
    guilds ||--o{ guild_members : " "
    guilds }o--|| event_requests : "requests"

    events ||--o{ rooms : "has"
    rooms ||--o{ room_players : " "
    events ||--o{ event_requests : "approves"
    events ||--o{ code_problems : "features"
    code_problems ||--o{ submissions : " "

    parties ||--o{ meeting : "holds"
    meeting ||--o{ meeting_participant : " "
```

## System Architecture Overview

### Service Boundaries & Responsibilities

#### **User Service** (`roguelearn-user-service`)
- **Primary Responsibility**: User identity, profiles, roles, academic structure, and achievement tracking. Owns the "Arsenal" of notes.
- **Key Entities**: `user_profiles`, `roles`, `classes`, `curriculum_programs`, `student_enrollments`, `achievements`, `notes`.

#### **Quests Service** (`roguelearn-quests-service`)
- **Primary Responsibility**: Learning content, gamification, and progress tracking. Transforms academic blueprints into playable content.
- **Key Entities**: `learning_paths`, `quests`, `quest_steps`, `user_quest_attempts`.

#### **Social Service** (`roguelearn-social-service`)
- **Primary Responsibility**: Community features (Parties, Guilds, Friends).
- **Key Entities**: `parties`, `guilds`, `party_members`, `guild_members`, `friendships`.

#### **Event Service** (`roguelearn-event-service`)
- **Primary Responsibility**: Creation, management, and execution of all competitive and social events. Owns the code problem catalog and submission engine.
- **Key Entities**: `events`, `event_requests`, `rooms`, `code_problems`, `submissions`, `leaderboard_entries`.

#### **Meeting Service** (`roguelearn-meeting-service`)
- **Primary Responsibility**: Real-time collaborative meetings for Parties and Guilds.
- **Key Entities**: `meeting`, `meeting_participant`.

### Data Flow & Relationships (Soft References)
-   All services reference `user_profiles` in the **User Service** via `auth_user_id` for user context.
-   The **Social Service** (`guilds`) is referenced by the **Event Service** (`event_requests`) when a Guild Master requests an event.
-   The **Event Service** (`code_problems`) provides the catalog of challenges for code battle events.
-   The **User Service** (`notes`) are referenced by the **Social Service** (`party_stash_items`) for collaborative sharing.
-   The **Quest Service** (`quests`) are referenced by the **User Service** (`note_quests`) to link notes to specific learning objectives.