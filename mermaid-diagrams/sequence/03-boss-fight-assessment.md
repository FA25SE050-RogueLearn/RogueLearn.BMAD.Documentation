# Gamified Assessment & Boss Fight Experience Flow

```mermaid
sequenceDiagram
    participant U as Student User
    participant UI as Web Interface
    participant GameClient as Game Client (Unity WebGL)
    participant GameServer as Netcode Host (Server Authoritative)
    participant APIGateway as API Gateway
    participant QuestsService as Quests Service
    participant UserService as User Service
    participant SocialService as Social Service
    participant Database as Database

    %% Step 0: Auth & session handoff %%
    UI->>UI: Retrieve auth token/session
    UI->>UI: Prepare session launch (pack is returned after Start Session)

    %% Step 1: Start Boss Fight from Web UI %%
    U->>UI: Clicks "Start Boss Fight" on a quest
    UI->>APIGateway: POST /game/sessions (questId)
    APIGateway->>QuestsService: Start session
    activate QuestsService
    QuestsService->>Database: CREATE GameSession (status: 'InProgress')
    Database-->>QuestsService: sessionId
    QuestsService->>QuestsService: Generate CurriculumPack (in-memory, ephemeral)
    QuestsService-->>APIGateway: 201 { sessionId, pack, gameBuildUrl }
    deactivate QuestsService
    APIGateway-->>UI: Session data + assessment content

    %% Step 2: Launch Game Client and connect to Netcode Host %%
    UI->>GameClient: Initialize WebGL embed with session data via JS bridge
    GameClient->>GameServer: Connect (Netcode) and join room/lobby
    GameServer->>QuestsService: RequestEphemeralPack(sessionId)
    QuestsService-->>GameServer: CurriculumPack (in-memory cache for session)
    GameServer-->>GameClient: Sync replicated states (ready counts, charges, timers)
    GameClient->>U: Display Boss Fight arena and HUD

    %% Step 3: Answer Station majority gating %%
    U->>GameClient: Interact 'Ready at Station'
    GameClient->>GameServer: RequestToggleReadyAtStation(stationId, isReady)
    alt Majority reached
        GameServer-->>GameClient: AnswerModeEntered (replicated)
        GameClient->>GameClient: Show Question UI from pack, start timer
    else Not enough players ready
        GameServer-->>GameClient: StationReadyChanged (update HUD)
    end

    %% Step 4: Question phase & server-side validation %%
    U->>GameClient: Submit answer
    GameClient->>GameServer: SubmitAnswer(answerData)
    alt Correct answer
        GameServer->>GameServer: teamCharges++ (replicated)
        GameServer-->>GameClient: TeamChargeAdded (HUD update)
        GameServer->>GameServer: Start Power Play window, select empowered player
        GameServer-->>GameClient: PowerPlayStarted(playerId, timer, multipliers)
    else Incorrect answer or timeout
        GameServer-->>GameClient: Feedback, optional lockout/cooldown
    end

    %% Step 5: Power Play window (first-hit ends window) %%
    U->>GameClient: Empowered player attacks boss
    GameClient->>GameServer: Damage event
    GameServer->>GameServer: Apply vulnerability/bonus to first hit, end window
    GameServer-->>GameClient: PowerPlayEnded, clear buffs

    %% Step 6: Exit Answer Mode, prep next question %%
    GameServer-->>GameClient: AnswerModeExited, hide panel, stop timer
    GameClient->>GameClient: Pending state until majority requests next question

    %% Step 7: Session completion and backend updates %%
    GameClient->>UI: onGameComplete(finalScore, progressData) via JS bridge
    UI->>APIGateway: POST /game/sessions/{sessionId}/complete (score, data)
    APIGateway->>QuestsService: Forward completion
    activate QuestsService
    QuestsService->>Database: UPDATE GameSession status 'Completed', save score
    QuestsService->>UserService: UpdateUserProgress(userId, xpGained, skillsUpdated)
    QuestsService->>SocialService: UpdateLeaderboard(userId, eventId, score)
    deactivate QuestsService
    QuestsService-->>APIGateway: 200 OK
    APIGateway-->>UI: 200 OK

    %% Step 8: Web UI displays final results %%
    UI->>U: Show score, XP, skill points, rank, leaderboard updates