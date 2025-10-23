# Event Service API Specification

```yaml
openapi: 3.0.0
info:
  title: RogueLearn Event Service API
  version: v1.0.0
  description: Event management, competitive coding events, problems, submissions, rooms, and leaderboards.
servers:
  - url: https://api.roguelearn.com/v1
    description: Production Server

security:
  - BearerAuth: []

components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: "JWT token obtained from Supabase after login."

  schemas:
    Language:
      type: object
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        compile_cmd: { type: string }
        run_cmd: { type: string }
    Tag:
      type: object
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        created_at: { type: string, format: date-time }
    CodeProblemTag:
      type: object
      properties:
        code_problem_id: { type: string, format: uuid }
        tag_id: { type: string, format: uuid }
    CodeProblemLanguageDetails:
      type: object
      properties:
        code_problem_id: { type: string, format: uuid }
        language_id: { type: string, format: uuid }
        solution_stub: { type: string }
        driver_code: { type: string }
        time_constraint_ms: { type: integer }
        space_constraint_mb: { type: integer }
    Room:
      type: object
      properties:
        id: { type: string, format: uuid }
        event_id: { type: string, format: uuid }
        name: { type: string }
        description: { type: string, nullable: true }
        created_date: { type: string, format: date-time }
    RoomPlayer:
      type: object
      properties:
        room_id: { type: string, format: uuid }
        user_id: { type: string, format: uuid }
        score: { type: integer }
        place: { type: integer, nullable: true }
        state: { type: string, nullable: true }
        disconnected_at: { type: string, format: date-time, nullable: true }
    CodeProblem:
      type: object
      properties:
        id: { type: string, format: uuid }
        title: { type: string }
        problem_statement: { type: string }
        difficulty: { type: integer }
        created_at: { type: string, format: date-time }
    TestCase:
      type: object
      properties:
        id: { type: string, format: uuid }
        code_problem_id: { type: string, format: uuid }
        input: { type: object }
        expected_output: { type: object }
        is_hidden: { type: boolean }
    Submission:
      type: object
      properties:
        id: { type: string, format: uuid }
        user_id: { type: string, format: uuid }
        code_problem_id: { type: string, format: uuid }
        language_id: { type: string, format: uuid }
        room_id: { type: string, format: uuid }
        code_submitted: { type: string }
        status: { type: string, enum: [pending, running, accepted, wrong_answer, time_limit_exceeded, compilation_error, runtime_error] }
        execution_time_ms: { type: integer, nullable: true }
        submitted_at: { type: string, format: date-time }
        submitted_guild_id: { type: string, format: uuid }
    LeaderboardEntry:
      type: object
      properties:
        id: { type: string, format: uuid }
        user_id: { type: string, format: uuid }
        event_id: { type: string, format: uuid }
        rank: { type: integer }
        score: { type: integer }
        snapshot_date: { type: string, format: date-time }
    GuildLeaderboardEntry:
      type: object
      properties:
        id: { type: string, format: uuid }
        guild_id: { type: string, format: uuid }
        event_id: { type: string, format: uuid }
        rank: { type: integer }
        total_score: { type: integer }
        snapshot_date: { type: string, format: date-time }
    EventGuildParticipant:
      type: object
      properties:
        event_id: { type: string, format: uuid }
        guild_id: { type: string, format: uuid }
        joined_at: { type: string, format: date-time }
        room_id: { type: string, format: uuid, nullable: true }
    Error:
      type: object
      properties:
        code: { type: string }
        message: { type: string }

paths:
  /languages:
    get:
      summary: List languages
      tags: [Languages]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Languages list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Language'
  /problems:
    get:
      summary: List code problems
      tags: [Problems]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Problems list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/CodeProblem'
  /admin/problems:
    post:
      summary: Import a new code problem
      description: Admin only. Includes test cases and language details.
      tags: [Problems]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                title: { type: string }
                problem_statement: { type: string }
                difficulty: { type: integer }
                tags: { type: array, items: { type: string } }
                language_details: { type: array, items: { $ref: '#/components/schemas/CodeProblemLanguageDetails' } }
                test_cases: { type: array, items: { $ref: '#/components/schemas/TestCase' } }
      responses:
        '201':
          description: Problem imported
  /test-cases:
    get:
      summary: List test cases
      tags: [TestCases]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Test cases list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/TestCase'
  /submissions:
    get:
      summary: List submissions
      tags: [Submissions]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Submissions list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Submission'
    post:
      summary: Submit a code solution
      description: Queue for secure execution and judging.
      tags: [Submissions]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                roomId: { type: string, format: uuid }
                problemId: { type: string, format: uuid }
                languageId: { type: string, format: uuid }
                sourceCode: { type: string }
      responses:
        '201':
          description: Submission created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Submission'
  /submissions/{submissionId}:
    get:
      summary: Poll submission result
      tags: [Submissions]
      security:
        - BearerAuth: []
      parameters:
        - name: submissionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Submission status
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Submission'
  /rooms:
    get:
      summary: List rooms
      tags: [Rooms]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Rooms list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Room'
  /events/{eventId}/rooms:
    get:
      summary: List rooms for event
      tags: [Rooms]
      security:
        - BearerAuth: []
      parameters:
        - name: eventId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Event rooms list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Room'
  /events/{eventId}/rooms/join:
    post:
      summary: Join a room for event
      tags: [Rooms]
      security:
        - BearerAuth: []
      parameters:
        - name: eventId
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
                preferredRoomId: { type: string, format: uuid, nullable: true }
      responses:
        '200':
          description: Joined room
  /leaderboards:
    get:
      summary: List leaderboards
      tags: [Leaderboards]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Leaderboards list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/LeaderboardEntry'
  /leaderboards/events/{eventId}:
    get:
      summary: Get event leaderboards
      tags: [Leaderboards]
      security:
        - BearerAuth: []
      parameters:
        - name: eventId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Individual and guild leaderboards
          content:
            application/json:
              schema:
                type: object
                properties:
                  individual:
                    type: array
                    items:
                      $ref: '#/components/schemas/LeaderboardEntry'
                  guild:
                    type: array
                    items:
                      $ref: '#/components/schemas/GuildLeaderboardEntry'
  /duels/challenge:
    post:
      summary: Challenge a User to a Duel
      tags: [Duels]
      security:
        - BearerAuth: []
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                opponentId:
                  type: string
                  format: uuid
      responses:
        '202':
          description: Challenge sent.
```