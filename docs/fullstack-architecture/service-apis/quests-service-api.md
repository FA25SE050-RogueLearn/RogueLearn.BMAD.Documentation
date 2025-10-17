openapi: 3.0.0
info:
  title: RogueLearn Quests Service API
  version: 1.0.0
  description: |
    Manages the core learning loop: quests, learning paths, ephemeral boss fights,
    and game sessions. All IDs are UUIDs. All endpoints
    require BearerAuth unless otherwise noted.
servers:
  - url: https://api.roguelearn.local/quests
security:
  - BearerAuth: []
tags:
  - name: LearningPaths
    description: User learning path and primary questline management
  - name: Quests
    description: Quest retrieval and objective progression
  - name: GameSessions
    description: Ephemeral boss fights and session completion
  - name: BossFightSessions
    description: Boss Fight (2D, Unity WebGL) session lifecycle (single-player and co-op) and in-session actions
components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
  schemas:
    Error:
      type: object
      properties:
        code: { type: string }
        message: { type: string }
    LearningPath:
      type: object
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        description: { type: string }
        primary: { type: boolean }
        questIds: { type: array, items: { type: string, format: uuid } }
    Quest:
      type: object
      properties:
        id: { type: string, format: uuid }
        title: { type: string }
        type: { type: string, enum: [LocalProject, CodingChallenge, StudyTask, BossFight] }
        status: { type: string, enum: [Locked, Available, InProgress, Completed] }
        objectives:
          type: array
          items:
            type: object
            properties:
              id: { type: string, format: uuid }
              title: { type: string }
              status: { type: string, enum: [Pending, Completed] }
              verificationType: { type: string, enum: [None, ProjectVerification, CodeSubmission] }
    BossFightQuestion:
      type: object
      properties:
        id: { type: string, format: uuid }
        prompt: { type: string }
        options: { type: array, items: { type: string } }
        correctIndex: { type: integer }
    BossFightQuestionPack:
      type: object
      properties:
        id: { type: string, format: uuid }
        subjectId: { type: string, format: uuid }
        generatedBy: { type: string, enum: [Gemini, OpenAI, Claude] }
        questions: { type: array, items: { $ref: '#/components/schemas/BossFightQuestion' } }
    EphemeralSessionInitRequest:
      type: object
      required: [questId]
      properties:
        questId: { type: string, format: uuid }
        providerPreference: { type: string, enum: [Gemini, OpenAI, Claude] }
        fallbackEnabled: { type: boolean, default: true }
    EphemeralSessionInitResponse:
      type: object
      properties:
        sessionId: { type: string, format: uuid }
        pack: { $ref: '#/components/schemas/BossFightQuestionPack' }
    BossFightSessionRuleSet:
      type: object
      properties:
        timePerQuestionSec: { type: integer, minimum: 5, maximum: 180, default: 45 }
        teamCharges: { type: integer, minimum: 0, maximum: 10, default: 3 }
        powerPlayWindowSec: { type: integer, minimum: 5, maximum: 60, default: 15 }
        difficultyPreset: { type: string, enum: [Easy, Medium, Hard, Expert, Adaptive], default: Adaptive }
        maxPlayers: { type: integer, minimum: 1, maximum: 12, default: 8 }
        antiGriefingEnabled: { type: boolean, default: true }
    BossFightCoopSessionInitRequest:
      type: object
      required: [questId]
      properties:
        questId: { type: string, format: uuid }
        mode: { type: string, enum: [SinglePlayer, Coop], default: SinglePlayer }
        partyId: { type: string, format: uuid }
        rules: { $ref: '#/components/schemas/BossFightSessionRuleSet' }
        providerPreference: { type: string, enum: [Gemini, OpenAI, Claude] }
        fallbackEnabled: { type: boolean, default: true }
    BossFightCoopSessionInitResponse:
      type: object
      properties:
        sessionId: { type: string, format: uuid }
        relayJoinCode: { type: string, description: 'Unity Relay join code for NGO clients (WSS). Optional for SinglePlayer.' }
        rules: { $ref: '#/components/schemas/BossFightSessionRuleSet' }
        pack: { $ref: '#/components/schemas/BossFightQuestionPack' }
    PlayerReadyRequest:
      type: object
      required: [playerId, ready]
      properties:
        playerId: { type: string, format: uuid }
        ready: { type: boolean }
    BossFightSessionParticipant:
      type: object
      properties:
        playerId: { type: string, format: uuid }
        displayName: { type: string }
        ready: { type: boolean }
        connected: { type: boolean }
        score: { type: number }
        correctCount: { type: integer }
        incorrectCount: { type: integer }
    BossFightSessionStatus:
      type: object
      properties:
        sessionId: { type: string, format: uuid }
        questId: { type: string, format: uuid }
        mode: { type: string, enum: [SinglePlayer, Coop] }
        rules: { $ref: '#/components/schemas/BossFightSessionRuleSet' }
        relayJoinCode: { type: string }
        phase: { type: string, enum: [Initializing, Lobby, InProgress, Completed, Cancelled] }
        participants: { type: array, items: { $ref: '#/components/schemas/BossFightSessionParticipant' } }
        startedAt: { type: string, format: date-time }
        updatedAt: { type: string, format: date-time }
    AnswerSubmission:
      type: object
      required: [playerId, questionId, selectedIndex, timeSpentMs]
      properties:
        playerId: { type: string, format: uuid }
        questionId: { type: string, format: uuid }
        selectedIndex: { type: integer }
        timeSpentMs: { type: integer, minimum: 0, maximum: 180000 }
        clientSeqNo: { type: integer, description: 'Monotonic sequence number to assist anti-cheat ordering.' }
        clientTimestamp: { type: string, format: date-time }
    AnswerSubmissionResponse:
      type: object
      properties:
        correct: { type: boolean }
        scoreDelta: { type: number }
        totalScore: { type: number }
        correctCount: { type: integer }
        incorrectCount: { type: integer }
    ChargeSpendRequest:
      type: object
      required: [playerId, chargeKey]
      properties:
        playerId: { type: string, format: uuid }
        chargeKey: { type: string, enum: [DamageBoost, Heal, Shield, TimeFreeze] }
    ChargeSpendResponse:
      type: object
      properties:
        accepted: { type: boolean }
        remainingCharges: { type: integer }
        effectApplied: { type: string }
    PowerPlayRequest:
      type: object
      required: [playerId]
      properties:
        playerId: { type: string, format: uuid }
    PowerPlayResponse:
      type: object
      properties:
        accepted: { type: boolean }
        windowSec: { type: integer }
        windowEndsAt: { type: string, format: date-time }
    GameSessionResult:
      type: object
      required: [score]
      properties:
        score: { type: number }
        timeTakenSeconds: { type: integer }
        correctCount: { type: integer }
        incorrectCount: { type: integer }
paths:
  /learning-paths/me:
    get:
      summary: Get my learning paths
      description: Retrieves the primary questline and supplementary learning paths for the current user.
      tags: [LearningPaths]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: My learning paths
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/LearningPath'
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

  /quests/{questId}:
    get:
      summary: Get quest details
      description: Retrieves detailed information for a specific quest, including objectives and status.
      tags: [Quests]
      security:
        - BearerAuth: []
      parameters:
        - name: questId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Quest detail
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Quest'
        '404':
          description: Not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

  /quests/{questId}/objectives/{objectiveId}/complete:
    post:
      summary: Complete quest objective
      description: Marks a quest objective as complete. For LocalProject or CodingChallenge types, this initiates verification or code submission flows.
      tags: [Quests]
      security:
        - BearerAuth: []
      parameters:
        - name: questId
          in: path
          required: true
          schema: { type: string, format: uuid }
        - name: objectiveId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '202':
          description: Objective completion accepted.
        '400':
          description: Invalid state
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

  /game/sessions/ephemeral:
    post:
      summary: Start ephemeral Boss Fight session
      description: Starts a new ephemeral Boss Fight session; returns in-memory question pack and session ID without persisting to DB.
      tags: [GameSessions]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/EphemeralSessionInitRequest'
      responses:
        '201':
          description: Session created with question pack
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/EphemeralSessionInitResponse'

  /game/bossfight/sessions:
    post:
      summary: Start Boss Fight session (SinglePlayer or Co-op)
      description: |
        Starts a Boss Fight session with configurable rules. For Co-op mode, returns a Unity Relay join code for NGO clients to connect.
        Returns an in-memory question pack and session ID. Use status endpoint to track lobby readiness.
      tags: [BossFightSessions]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/BossFightCoopSessionInitRequest'
      responses:
        '201':
          description: Boss Fight session created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/BossFightCoopSessionInitResponse'

  /game/bossfight/sessions/{sessionId}:
    get:
      summary: Get Boss Fight session status
      description: Retrieves the current status, participants, rules, and phase for a Boss Fight session.
      tags: [BossFightSessions]
      security:
        - BearerAuth: []
      parameters:
        - name: sessionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Session status
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/BossFightSessionStatus'

  /game/bossfight/sessions/{sessionId}/ready:
    post:
      summary: Ready-check for Boss Fight session
      description: Marks a player ready or not-ready in the lobby. Returns updated session status.
      tags: [BossFightSessions]
      security:
        - BearerAuth: []
      parameters:
        - name: sessionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/PlayerReadyRequest'
      responses:
        '200':
          description: Updated session status
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/BossFightSessionStatus'

  /game/bossfight/sessions/{sessionId}/answer:
    post:
      summary: Submit answer
      description: Server-authoritative validation of a question answer; updates score and counters.
      tags: [BossFightSessions]
      security:
        - BearerAuth: []
      parameters:
        - name: sessionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/AnswerSubmission'
      responses:
        '200':
          description: Answer processed
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AnswerSubmissionResponse'

  /game/bossfight/sessions/{sessionId}/charge:
    post:
      summary: Spend a team charge
      description: Applies a team charge effect with server-side checks and returns remaining charges.
      tags: [BossFightSessions]
      security:
        - BearerAuth: []
      parameters:
        - name: sessionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ChargeSpendRequest'
      responses:
        '200':
          description: Charge applied
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ChargeSpendResponse'

  /game/bossfight/sessions/{sessionId}/power-play:
    post:
      summary: Trigger power play window
      description: Starts a brief window of increased boss vulnerability; enforces cooldowns and rules.
      tags: [BossFightSessions]
      security:
        - BearerAuth: []
      parameters:
        - name: sessionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/PowerPlayRequest'
      responses:
        '200':
          description: Power play started
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PowerPlayResponse'

  /game/bossfight/sessions/{sessionId}/complete:
    post:
      summary: Complete Boss Fight session
      description: Submits the final results and leaderboard-impacting metrics for a Boss Fight session.
      tags: [BossFightSessions]
      security:
        - BearerAuth: []
      parameters:
        - name: sessionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/GameSessionResult'
      responses:
        '202':
          description: Results accepted for processing.

  /game/bossfight/sessions/{sessionId}/cancel:
    post:
      summary: Cancel Boss Fight session
      description: Cancels an in-progress or lobby session. May impact party cooldowns.
      tags: [BossFightSessions]
      security:
        - BearerAuth: []
      parameters:
        - name: sessionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '202':
          description: Cancel request accepted.

  /game/sessions/{sessionId}/complete:
    post:
      summary: Complete game session
      description: Submits the results of a completed game session (score, timing, counts).
      tags: [GameSessions]
      security:
        - BearerAuth: []
      parameters:
        - name: sessionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/GameSessionResult'
      responses:
        '202':
          description: Results accepted for processing.