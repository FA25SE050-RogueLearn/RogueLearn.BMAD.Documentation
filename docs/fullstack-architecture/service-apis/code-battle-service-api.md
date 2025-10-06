# Code Battle Service API Specification

```yaml
openapi: 3.0.0
info:
  title: RogueLearn Code Battle Service API
  version: v1.0.0
  description: Competitive coding events, problems, submissions, rooms, and leaderboards.
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
      responses:
        '202':
          description: Challenge sent.
```