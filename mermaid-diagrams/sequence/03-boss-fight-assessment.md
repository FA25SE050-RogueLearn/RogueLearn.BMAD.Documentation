# Gamified Assessment & Boss Fight Experience Flow

```mermaid
sequenceDiagram
    participant U as Student User
    participant UI as Web Interface (Next.js)
    participant Unity as Unity WebGL Client
    participant APIGateway as API Gateway (Azure)
    participant QuestsService as Quests Service
    participant UserService as User Service
    participant SocialService as Social Service
    participant Database as Database (Supabase)

    %% Step 1: User starts the assessment from the web UI %%
    U->>UI: Clicks "Start Boss Fight" on a quest
    UI->>APIGateway: POST /game/sessions (questId)
    APIGateway->>QuestsService: Forwards request to start session

    %% Step 2: Backend creates a game session and fetches the assessment content %%
    activate QuestsService
    QuestsService->>Database: CREATE GameSession record (status: 'InProgress')
    Database-->>QuestsService: Returns new sessionId
    QuestsService->>Database: Fetch or Generate CurriculumPack for the quest
    Database-->>QuestsService: Returns assessment questions (CurriculumPack)
    
    QuestsService-->>APIGateway: 201 Created { sessionId, pack, gameBuildUrl }
    deactivate QuestsService
    APIGateway-->>UI: Returns session data and assessment content

    %% Step 3: Web UI launches the Unity client with the session data %%
    UI->>Unity: Initialize game embed with session data via JS bridge
    Unity->>U: Displays Boss Fight arena and gameplay UI

    %% Step 4: Gameplay loop is handled entirely within the Unity client %%
    loop Gameplay
        U->>Unity: Answers questions presented in the game
        Unity->>Unity: Evaluates answers, plays animations, updates local score
    end

    %% Step 5: Unity client reports final score back to the web UI upon completion %%
    Unity->>UI: Calls onGameComplete(finalScore, progressData) via JS bridge
    
    %% Step 6: Web UI sends the final results to the backend %%
    UI->>APIGateway: POST /game/sessions/{sessionId}/complete (score, data)
    APIGateway->>QuestsService: Forwards completion request

    %% Step 7: Backend finalizes the session and updates all relevant user data %%
    activate QuestsService
    QuestsService->>Database: UPDATE GameSession status to 'Completed' and save score
    QuestsService->>UserService: UpdateUserProgress(userId, xpGained, skillsUpdated)
    QuestsService->>SocialService: UpdateLeaderboard(userId, eventId, score)
    deactivate QuestsService
    
    QuestsService-->>APIGateway: 200 OK
    APIGateway-->>UI: 200 OK

    %% Step 8: User sees their final results on the web UI %%
    UI->>U: Display results screen (score, XP, skill points, new rank)
```
