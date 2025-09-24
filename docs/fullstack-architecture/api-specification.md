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
        meeting_id: { type: string, format: uuid }
        title: { type: string }
        scheduled_start_time: { type: string, format: date-time }
        scheduled_end_time: { type: string, format: date-time }
        actual_start_time: { type: string, format: date-time, nullable: true }
        actual_end_time: { type: string, format: date-time, nullable: true }
        organizer_id: { type: string, format: uuid }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
    MeetingParticipant:
      type: object
      properties:
        participant_id: { type: string, format: uuid }
        meeting_id: { type: string, format: uuid }
        user_id: { type: string, format: uuid }
        join_time: { type: string, format: date-time, nullable: true }
        leave_time: { type: string, format: date-time, nullable: true }
        role_in_meeting: { type: string }
    TranscriptSegment:
      type: object
      properties:
        segment_id: { type: string, format: uuid }
        meeting_id: { type: string, format: uuid }
        speaker_id: { type: string, format: uuid }
        start_time: { type: string, format: date-time }
        end_time: { type: string, format: date-time }
        transcript_text: { type: string }
        chunk_number: { type: integer }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
    SummaryChunk:
      type: object
      properties:
        summary_chunk_id: { type: string, format: uuid }
        meeting_id: { type: string, format: uuid }
        chunk_number: { type: integer }
        summary_text: { type: string }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
    MeetingSummary:
      type: object
      properties:
        meeting_summary_id: { type: string, format: uuid }
        meeting_id: { type: string, format: uuid }
        summary_text: { type: string }
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
      summary: Get meetings for current user
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
              required: [title, scheduled_start_time, scheduled_end_time]
              properties:
                title: { type: string }
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
                scheduled_start_time: { type: string, format: date-time }
                scheduled_end_time: { type: string, format: date-time }
                actual_start_time: { type: string, format: date-time }
                actual_end_time: { type: string, format: date-time }
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
              required: [user_id]
              properties:
                user_id: { type: string, format: uuid }
                role_in_meeting: { type: string }
      responses:
        '201':
          description: Participant added

  /meetings/{meetingId}/participants/{participantId}:
    put:
      summary: Update participant join/leave times
      tags: [Meetings]
      security:
        - BearerAuth: []
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
        - name: participantId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                join_time: { type: string, format: date-time }
                leave_time: { type: string, format: date-time }
      responses:
        '200':
          description: Participant updated

  /meetings/{meetingId}/transcript:
    get:
      summary: Get meeting transcript segments
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
          description: Meeting transcript segments
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/TranscriptSegment'
    post:
      summary: Add transcript segment
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
              required: [speaker_id, start_time, end_time, transcript_text, chunk_number]
              properties:
                speaker_id: { type: string, format: uuid }
                start_time: { type: string, format: date-time }
                end_time: { type: string, format: date-time }
                transcript_text: { type: string }
                chunk_number: { type: integer }
      responses:
        '201':
          description: Transcript segment added
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TranscriptSegment'

  /meetings/{meetingId}/summary-chunks:
    get:
      summary: Get meeting summary chunks
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
          description: Meeting summary chunks
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/SummaryChunk'
    post:
      summary: Add summary chunk
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
              required: [chunk_number, summary_text]
              properties:
                chunk_number: { type: integer }
                summary_text: { type: string }
      responses:
        '201':
          description: Summary chunk added
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/SummaryChunk'

  /meetings/{meetingId}/summary:
    get:
      summary: Get meeting summary
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
          description: Meeting summary
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MeetingSummary'
    post:
      summary: Create meeting summary
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
              required: [summary_text]
              properties:
                summary_text: { type: string }
      responses:
        '201':
          description: Meeting summary created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MeetingSummary'

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

