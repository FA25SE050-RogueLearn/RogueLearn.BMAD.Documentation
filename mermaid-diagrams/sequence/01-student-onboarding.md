# Student Onboarding & Quest Generation Flow (Corrected)

## Overview
This corrected diagram shows the definitive flow. The User Service manages the user's profile and academic/career choices. The Quest Service is the engine responsible for orchestrating the AI-driven generation of the personalized questline by fetching the necessary data from the User Service.

## Sequence Diagram
```mermaid
sequenceDiagram
    participant User as 👤 Student User
    participant Frontend as 🖥️ Web Interface
    participant APIGateway as 🚪 API Gateway
    participant UserService as 🤵 User Service (Data Authority)
    participant QuestService as 🗺️ Quest Service (Generation Engine)
    participant UserDB as 💾 User DB
    participant QuestDB as 💾 Quest DB

    %% Phase 1: Onboarding - User Profile Configuration (User Service owned)
    Note over User, QuestDB: Phase 1: Onboarding - User makes selections via User Service

    User->>Frontend: Begins "Character Creation"
    Frontend->>APIGateway: GET /curriculum-programs
    APIGateway->>UserService: Forwards request
    
    activate UserService
    UserService->>UserDB: Fetch available curriculum_programs
    UserDB-->>UserService: Returns programs list
    UserService-->>APIGateway: Returns programs list
    deactivate UserService

    APIGateway-->>Frontend: Returns programs list
    Frontend->>User: Displays "Route" (Curriculum) options
    User->>Frontend: Selects a Route

    Frontend->>APIGateway: GET /classes
    APIGateway->>UserService: Forwards request

    activate UserService
    UserService->>UserDB: Fetch available classes
    UserDB-->>UserService: Returns classes list
    UserService-->>APIGateway: Returns classes list
    deactivate UserService
    
    APIGateway-->>Frontend: Returns classes list
    Frontend->>User: Displays "Class" (Career) options
    User->>Frontend: Selects a Class

    Frontend->>APIGateway: POST /onboarding/complete { curriculumVersionId, classId }
    APIGateway->>UserService: Forwards request
    
    activate UserService
    UserService->>UserDB: UPDATE user_profiles with route_id, class_id, and set onboarding_completed = true
    UserDB-->>UserService: Confirms update
    UserService-->>APIGateway: 200 OK (Onboarding Profile Complete)
    deactivate UserService
    APIGateway-->>Frontend: 200 OK

    %% Phase 2: QuestLine Generation Trigger (Quest Service owned)
    Note over User, QuestDB: Phase 2: Frontend triggers Quest Service to generate the QuestLine

    Frontend->>APIGateway: POST /api/me/quest-lines/generate
    APIGateway->>QuestService: Forwards request for current user
    
    activate QuestService
    Note right of QuestService: Quest Service is now the orchestrator.

    %% Step 2a: Quest Service fetches required data from User Service
    QuestService->>UserService: GET /internal/users/me/profile-for-generation
    activate UserService
    UserService->>UserDB: Get user's route_id and class_id
    UserDB-->>UserService: Returns user profile data
    UserService-->>QuestService: Returns User Profile DTO
    deactivate UserService

    QuestService->>UserService: GET /internal/curriculums/{route_id}/structure
    activate UserService
    UserService->>UserDB: Get curriculum_structure for the route
    UserDB-->>UserService: Returns curriculum data
    UserService-->>QuestService: Returns Curriculum DTO
    deactivate UserService

    QuestService->>UserService: GET /internal/classes/{class_id}/roadmap
    activate UserService
    UserService->>UserDB: Get class_nodes for the class
    UserDB-->>UserService: Returns roadmap data
    UserService-->>QuestService: Returns Roadmap DTO
    deactivate UserService

    %% Step 2b: Quest Service performs internal AI processing
    Note over QuestService: Uses its internal Semantic Kernel instance for AI processing.
    QuestService->>QuestService: Perform Gap Analysis (Curriculum vs. Roadmap)
    QuestService->>QuestService: Generate Unified Learning Plan

    %% Step 2c: Quest Service persists the gamified content
    Note over QuestService: Persists the generated content into its own database.
    QuestService->>QuestDB: CREATE LearningPath, QuestChapters, Quests, QuestSteps, etc.
    QuestService->>QuestDB: CREATE user_learning_path_progress record
    QuestDB-->>QuestService: Confirms all creations
    
    QuestService-->>APIGateway: 201 Created { questLineId }
    deactivate QuestService
    APIGateway-->>Frontend: 201 Created

    %% Phase 3: User Experience
    Note over User, QuestDB: Phase 3: User experiences their new QuestLine

    Frontend->>User: Redirects to personalized dashboard
    User->>Frontend: Sees newly generated, integrated QuestLine
```