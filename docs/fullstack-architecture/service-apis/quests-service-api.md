```yaml
openapi: 3.0.0
info:
  title: RogueLearn Quests Service API
  version: 1.0.0
  description: |
    Manages the core learning loop: quests, learning paths, ephemeral boss fights,
    game sessions, and the personal Arsenal (notes). All IDs are UUIDs. All endpoints
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
  - name: Notes
    description: Personal Arsenal notes
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
    GameSessionResult:
      type: object
      required: [score]
      properties:
        score: { type: number }
        timeTakenSeconds: { type: integer }
        correctCount: { type: integer }
        incorrectCount: { type: integer }
    Note:
      type: object
      properties:
        id: { type: string, format: uuid }
        title: { type: string }
        content: { type: string }
        createdAt: { type: string, format: date-time }
        updatedAt: { type: string, format: date-time }
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

  /notes:
    get:
      summary: List notes
      description: Retrieves all notes from the user's Arsenal.
      tags: [Notes]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Notes list
          content:
            application/json:
              schema:
                type: array
                items: { $ref: '#/components/schemas/Note' }
    post:
      summary: Create note
      description: Creates a new note.
      tags: [Notes]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [title, content]
              properties:
                title: { type: string }
                content: { type: string }
      responses:
        '201':
          description: Note created

  /notes/{noteId}:
    get:
      summary: Get note
      description: Retrieves a specific note.
      tags: [Notes]
      security:
        - BearerAuth: []
      parameters:
        - name: noteId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Note detail
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Note'
    put:
      summary: Update note
      description: Updates an existing note.
      tags: [Notes]
      security:
        - BearerAuth: []
      parameters:
        - name: noteId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                title: { type: string }
                content: { type: string }
      responses:
        '200':
          description: Note updated
    delete:
      summary: Delete note
      description: Deletes a note.
      tags: [Notes]
      security:
        - BearerAuth: []
      parameters:
        - name: noteId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '204':
          description: Note deleted
```