# **API Specification**

This section defines the RESTful API for the RogueLearn platform using the OpenAPI 3.0 standard.

### **REST API Specification**

```yaml
openapi: 3.0.0
info:
  title: RogueLearn API
  version: v1.0.0
  description: The official API for the RogueLearn platform, providing services for gamified learning.
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
    UserProfile:
      type: object
      properties:
        id: { type: string, format: uuid }
        auth_user_id: { type: string }
        username: { type: string }
        email: { type: string }
        class_id: { type: string, format: uuid }
        route_id: { type: string, format: uuid, nullable: true }
        level: { type: integer }
        experience_points: { type: integer }
        profile_image_url: { type: string, nullable: true }
        onboarding_completed: { type: boolean }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
    Syllabus:
      type: object
      properties:
        id: { type: string, format: uuid }
        curriculum_id: { type: string, format: uuid }
        course_code: { type: string }
        course_title: { type: string }
        description: { type: string, nullable: true }
        credits: { type: integer, nullable: true }
    UserSyllabusEnrollment:
      type: object
      properties:
        id: { type: string, format: uuid }
        user_profile_id: { type: string, format: uuid }
        syllabus_id: { type: string, format: uuid }
        enrollment_date: { type: string, format: date-time }
    UserUploadedSyllabus:
      type: object
      properties:
        id: { type: string, format: uuid }
        enrollment_id: { type: string, format: uuid }
        file_url: { type: string }
        processing_status: { type: string, enum: [Pending, Processing, Completed, Failed] }
        structured_content: { type: object, nullable: true }
        uploaded_at: { type: string, format: date-time }
    GameSession:
      type: object
      properties:
        id: { type: string, format: uuid }
        quest_id: { type: string, format: uuid }
        status: { type: string, enum: [InProgress, Completed, Abandoned] }
        score: { type: integer }
        started_at: { type: string, format: date-time }
        completed_at: { type: string, format: date-time, nullable: true }
    Meeting:
      type: object
      properties:
        id: { type: string, format: uuid }
        party_id: { type: string, format: uuid }
        creator_id: { type: string, format: uuid }
        title: { type: string }
        description: { type: string, nullable: true }
     scheduled_start_time: { type: string, format: date-time }
                scheduled_end_time: { type: string, format: date-time }
        status: { type: string, enum: [Scheduled, InProgress, Completed, Cancelled] }
        created_at: { type: string, format: date-time }
    MeetingParticipant:
      type: object
      properties:
        meeting_id: { type: string, format: uuid }
        user_profile_id: { type: string, format: uuid }
        status: { type: string, enum: [Invited, Accepted, Declined, Attended] }
    MeetingAgenda:
      type: object
      properties:
        id: { type: string, format: uuid }
        meeting_id: { type: string, format: uuid }
        topic: { type: string }
        description: { type: string, nullable: true }
        presenter_id: { type: string, format: uuid, nullable: true }
        sequence: { type: integer }
        duration_minutes: { type: integer, nullable: true }
    MeetingNote:
      type: object
      properties:
        id: { type: string, format: uuid }
        meeting_id: { type: string, format: uuid }
        agenda_id: { type: string, format: uuid, nullable: true }
        creator_id: { type: string, format: uuid }
        content: { type: object }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
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
        id: { type: string, format: uuid }
        code_problem_id: { type: string, format: uuid }
        language_id: { type: string, format: uuid }
        starter_code: { type: string, nullable: true }
        solution_template: { type: string, nullable: true }
    Room:
      type: object
      properties:
        id: { type: string, format: uuid }
        event_id: { type: string, format: uuid }
        name: { type: string }
        description: { type: string, nullable: true }
        created_at: { type: string, format: date-time }
    RoomPlayer:
      type: object
      properties:
        room_id: { type: string, format: uuid }
        user_profile_id: { type: string, format: uuid }
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
        member_count: { type: integer }
        snapshot_date: { type: string, format: date-time }
    EventGuildParticipant:
      type: object
      properties:
        event_id: { type: string, format: uuid }
        guild_id: { type: string, format: uuid }
        registered_at: { type: string, format: date-time }
    Error:
      type: object
      properties:
        code: { type: string }
        message: { type: string }
    
    # Added: Curriculum Pack for AI-generated, vetted content delivery
    CurriculumPack:
      type: object
      properties:
        meta:
          type: object
          properties:
            pack_id: { type: string, format: uuid }
            version: { type: string }
            source: { type: string, enum: [Set, Seed] }
            created_at: { type: string, format: date-time }
            subject_id: { type: string, format: uuid, nullable: true }
            curriculum_id: { type: string, format: uuid, nullable: true }
            ai_model: { type: string, nullable: true }
            generator_version: { type: string, nullable: true }
            prompt_hash: { type: string, nullable: true }
            seed: { type: string, nullable: true }
            approved_by: { type: string, nullable: true }
        items:
          type: array
          items:
            type: object
            properties:
              id: { type: string }
              type: { type: string, enum: [MCQ, FreeText] }
              text: { type: string }
              options:
                type: array
                items:
                  type: object
                  properties:
                    id: { type: string }
                    text: { type: string }
              correct_answers:
                type: array
                items: { type: string }
              explanation: { type: string, nullable: true }
              difficulty: { type: string, enum: [Beginner, Intermediate, Advanced], nullable: true }
              tags:
                type: array
                items: { type: string }
              sequence: { type: integer, nullable: true }
              weight: { type: number, nullable: true }
              time_limit_sec: { type: integer, nullable: true }
              metadata: { type: object, nullable: true }
            required: [id, type, text]
      required: [meta, items]

paths:
  # User Profile Endpoints
  /profiles/me:
    get:
      summary: Get Current User's Profile
      tags: [Profiles]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Successful retrieval of user profile.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserProfile'
          
  # Academic Management Endpoints
  /enrollments:
    get:
      summary: Get All Enrollments for Current User
      tags: [Academic]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: A list of the user's syllabus enrollments.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/UserSyllabusEnrollment'

  /enrollments/{enrollmentId}/upload-syllabus:
    post:
      summary: Upload a Syllabus Document for an Enrollment
      tags: [Academic]
      security:
        - BearerAuth: []
      parameters:
        - name: enrollmentId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          multipart/form-data:
            schema:
              type: object
              properties:
                file:
                  type: string
                  format: binary
      responses:
        '202':
          description: Syllabus accepted for processing.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserUploadedSyllabus'
  
  # Duel Endpoints
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

  # Meeting Endpoints
  /meetings:
    get:
      summary: Get meetings for current user's parties
      tags: [Meetings]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: List of meetings
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Meeting'
    post:
      summary: Create a new meeting
      tags: [Meetings]
      security:
        - BearerAuth: []
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [partyId, title, scheduledStartTime]
              properties:
                partyId: { type: string, format: uuid }
                title: { type: string }
                description: { type: string }
                scheduled_start_time: { type: string, format: date-time }
                scheduled_end_time: { type: string, format: date-time }
      responses:
        '201':
          description: Meeting created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Meeting'

  /meetings/{meetingId}:
    get:
      summary: Get meeting details
      tags: [Meetings]
      security:
        - BearerAuth: []
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Meeting details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Meeting'
    put:
      summary: Update meeting
      tags: [Meetings]
      security:
        - BearerAuth: []
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                title: { type: string }
                description: { type: string }
                scheduledStartTime: { type: string, format: date-time }
                scheduledEndTime: { type: string, format: date-time }
                status: { type: string, enum: [Scheduled, InProgress, Completed, Cancelled] }
      responses:
        '200':
          description: Meeting updated
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Meeting'

  /meetings/{meetingId}/participants:
    get:
      summary: Get meeting participants
      tags: [Meetings]
      security:
        - BearerAuth: []
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: List of participants
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/MeetingParticipant'
    post:
      summary: Add participant to meeting
      tags: [Meetings]
      security:
        - BearerAuth: []
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [user_profile_id]
              properties:
                user_profile_id: { type: string, format: uuid }
      responses:
        '201':
          description: Participant added

  /meetings/{meetingId}/participants/{userId}/status:
    put:
      summary: Update participant status
      tags: [Meetings]
      security:
        - BearerAuth: []
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
        - name: userId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [status]
              properties:
                status: { type: string, enum: [Invited, Accepted, Declined, Attended] }
      responses:
        '200':
          description: Status updated

  /meetings/{meetingId}/agenda:
    get:
      summary: Get meeting agenda
      tags: [Meetings]
      security:
        - BearerAuth: []
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Meeting agenda
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/MeetingAgenda'
    post:
      summary: Add agenda item
      tags: [Meetings]
      security:
        - BearerAuth: []
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [topic, sequence]
              properties:
                topic: { type: string }
                description: { type: string }
                presenterId: { type: string, format: uuid }
                sequence: { type: integer }
                durationMinutes: { type: integer }
      responses:
        '201':
          description: Agenda item added
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MeetingAgenda'

  /meetings/{meetingId}/notes:
    get:
      summary: Get meeting notes
      tags: [Meetings]
      security:
        - BearerAuth: []
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Meeting notes
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/MeetingNote'
    post:
      summary: Add meeting note
      tags: [Meetings]
      security:
        - BearerAuth: []
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [content]
              properties:
                agendaId: { type: string, format: uuid }
                content: { type: object }
      responses:
        '201':
          description: Note added
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MeetingNote'

  # Code Battle Endpoints
  /languages:
    get:
      summary: Get supported programming languages
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: List of supported languages
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Language'

  /events/{eventId}/rooms:
    get:
      summary: Get rooms for an event
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      parameters:
        - name: eventId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: List of rooms
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Room'
    post:
      summary: Create a new room for an event
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      parameters:
        - name: eventId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [name]
              properties:
                name: { type: string }
                description: { type: string }
      responses:
        '201':
          description: Room created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Room'

  /rooms/{roomId}/join:
    post:
      summary: Join a code battle room
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      parameters:
        - name: roomId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Successfully joined room
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RoomPlayer'

  /rooms/{roomId}/players:
    get:
      summary: Get players in a room
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      parameters:
        - name: roomId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: List of players
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/RoomPlayer'

  /problems/{problemId}:
    get:
      summary: Get code problem details
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      parameters:
        - name: problemId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Problem details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CodeProblem'

  /problems/{problemId}/testcases:
    get:
      summary: Get test cases for a problem (sample only)
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      parameters:
        - name: problemId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Sample test cases
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/TestCase'

  /submissions:
    post:
      summary: Submit code for evaluation
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [codeProblemId, languageId, roomId, codeSubmitted]
              properties:
                codeProblemId: { type: string, format: uuid }
                languageId: { type: string, format: uuid }
                roomId: { type: string, format: uuid }
                codeSubmitted: { type: string }
      responses:
        '201':
          description: Submission created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Submission'

  /submissions/{submissionId}:
    get:
      summary: Get submission status and results
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      parameters:
        - name: submissionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Submission details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Submission'

  /events/{eventId}/submissions:
    get:
      summary: Get user's submissions for an event
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      parameters:
        - name: eventId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: List of submissions
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Submission'

  # Browser Extension Endpoints
  /extension/analyze:
    post:
      summary: Analyze content from browser extension
      tags: [Extension]
      security:
        - BearerAuth: []
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                content:
                  type: string
                url:
                  type: string
      responses:
        '200':
          description: Analysis results.

  # Game Session Endpoints (for Unity Client)
  /game/sessions:
    post:
      summary: Start a new game session (e.g., a Boss Fight)
      tags: [Game]
      security:
        - BearerAuth: []
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                questId:
                  type: string
                  format: uuid
                # Optional: bind or generate a curriculum pack for this session
                packId:
                  type: string
                  format: uuid
                  nullable: true
                subjectId:
                  type: string
                  format: uuid
                  nullable: true
                seed:
                  type: string
                  nullable: true
                source:
                  type: string
                  enum: [Set, Seed]
                  nullable: true
      responses:
        '201':
          description: Game session created.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/GameSession'

  /game/sessions/{sessionId}/complete:
    post:
      summary: Complete a game session and submit results
      tags: [Game]
      security:
        - BearerAuth: []
      parameters:
        - name: sessionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                score:
                  type: integer
                progressData:
                  type: object
      responses:
        '200':
          description: Session results submitted successfully.

  # Added: Fetch the Curriculum Pack snapshot for a session
  /game/sessions/{sessionId}/pack:
    get:
      summary: Get the Curriculum Pack snapshot for a game session
      tags: [Game]
      security:
        - BearerAuth: []
      parameters:
        - name: sessionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: The curriculum pack bound to this session
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CurriculumPack'

  # Leaderboard Endpoints
  /events/{eventId}/leaderboard:
    get:
      summary: Get event leaderboard
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      parameters:
        - name: eventId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Event leaderboard entries
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/LeaderboardEntry'

  /events/{eventId}/guild-leaderboard:
    get:
      summary: Get guild leaderboard for an event
      tags: [CodeBattle]
      security:
        - BearerAuth: []
      parameters:
        - name: eventId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Guild leaderboard entries
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/GuildLeaderboardEntry'


# Ephemeral Question Pack (No Persist)

## Endpoint

/game/sessions/ephemeral:
  post:
    summary: Start a new game session and return an ephemeral CurriculumPack inline (no persistence)
    tags: [Game]
    security:
      - BearerAuth: []
    requestBody:
      content:
        application/json:
          schema:
            type: object
            properties:
              questId:
                type: string
                format: uuid
              subjectId:
                type: string
                format: uuid
                nullable: true
              seed:
                type: string
                nullable: true
              packType:
                type: string
                enum: [BossFightQuestionPack]
                default: BossFightQuestionPack
              providerPreference:
                type: string
                enum: [Gemini, OpenAI]
                description: Preferred LLM provider to use first
              fallbackEnabled:
                type: boolean
                default: true
                description: If true, automatically fall back to the alternate provider on failure or schema validation errors
              syllabusContextId:
                type: string
                format: uuid
                nullable: true
              promptHints:
                type: object
                additionalProperties: true
                description: Optional hints to condition generation (topics, difficulty, count)
    responses:
      '201':
        description: Ephemeral session created and pack returned inline
        content:
          application/json:
            schema:
              type: object
              properties:
                session:
                  $ref: '#/components/schemas/GameSession'
                pack:
                  $ref: '#/components/schemas/CurriculumPack'
                providerInfo:
                  type: object
                  properties:
                    chosenProvider:
                      type: string
                      enum: [Gemini, OpenAI]
                    fallbackUsed:
                      type: boolean
                      description: True if fallback provider was used

## Provider Selection & Fallback Semantics
- providerPreference determines the first provider attempted.
- If fallbackEnabled is true and generation fails or JSON schema validation fails, the system retries once with the alternate provider.
- providerInfo in the response indicates which provider produced the pack and whether fallback occurred.

## Example Request

POST /game/sessions/ephemeral
{
  "questId": "f1b1a4d8-7f8a-4a10-a0b0-8d0e2a8f9e21",
  "subjectId": "3a2f0b77-0e72-4a0a-bf8a-1a9e9a4a9a77",
  "seed": "student-123-run-001",
  "packType": "BossFightQuestionPack",
  "providerPreference": "Gemini",
  "fallbackEnabled": true,
  "promptHints": { "topic": "Binary Search", "difficulty": "Beginner", "count": 6 }
}

## Example Response (201)
{
  "session": {
    "id": "b5f33d1e-5a2f-4a7b-8ed4-2b4f7d2b6a91",
    "status": "InProgress",
    "quest_id": "f1b1a4d8-7f8a-4a10-a0b0-8d0e2a8f9e21",
    "started_at": "2025-09-12T10:02:31Z"
  },
  "pack": {
    "meta": { "type": "BossFightQuestionPack", "version": "1.0.0", "ephemeral": true },
    "items": [
      { "id": "q1", "prompt": "What is the time complexity of binary search?", "choices": ["O(n)", "O(log n)", "O(n log n)", "O(1)"], "correctIndex": 1, "timeLimitSec": 25 }
    ]
  },
  "providerInfo": { "chosenProvider": "Gemini", "fallbackUsed": false }
}

