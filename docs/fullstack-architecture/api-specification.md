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
        userId: { type: string }
        username: { type: string }
        email: { type: string }
        classId: { type: string, format: uuid }
        routeId: { type: string, format: uuid, nullable: true }
        level: { type: integer }
        experiencePoints: { type: integer }
        profileImageUrl: { type: string, nullable: true }
        onboardingCompleted: { type: boolean }
        createdAt: { type: string, format: date-time }
        updatedAt: { type: string, format: date-time }
    Syllabus:
      type: object
      properties:
        id: { type: string, format: uuid }
        curriculumId: { type: string, format: uuid }
        courseCode: { type: string }
        courseTitle: { type: string }
        description: { type: string, nullable: true }
        credits: { type: integer, nullable: true }
    UserSyllabusEnrollment:
      type: object
      properties:
        id: { type: string, format: uuid }
        userProfileId: { type: string, format: uuid }
        syllabusId: { type: string, format: uuid }
        enrollmentDate: { type: string, format: date-time }
    UserUploadedSyllabus:
      type: object
      properties:
        id: { type: string, format: uuid }
        enrollmentId: { type: string, format: uuid }
        fileUrl: { type: string }
        processingStatus: { type: string, enum: [Pending, Processing, Completed, Failed] }
        structuredContent: { type: object, nullable: true }
        uploadedAt: { type: string, format: date-time }
    GameSession:
      type: object
      properties:
        id: { type: string, format: uuid }
        questId: { type: string, format: uuid }
        status: { type: string, enum: [InProgress, Completed, Abandoned] }
        score: { type: integer }
        startedAt: { type: string, format: date-time }
        completedAt: { type: string, format: date-time, nullable: true }
    Meeting:
      type: object
      properties:
        id: { type: string, format: uuid }
        partyId: { type: string, format: uuid }
        creatorId: { type: string, format: uuid }
        title: { type: string }
        description: { type: string, nullable: true }
        scheduledStartTime: { type: string, format: date-time }
        scheduledEndTime: { type: string, format: date-time, nullable: true }
        status: { type: string, enum: [Scheduled, InProgress, Completed, Cancelled] }
        createdAt: { type: string, format: date-time }
    MeetingParticipant:
      type: object
      properties:
        meetingId: { type: string, format: uuid }
        userProfileId: { type: string, format: uuid }
        status: { type: string, enum: [Invited, Accepted, Declined, Attended] }
    MeetingAgenda:
      type: object
      properties:
        id: { type: string, format: uuid }
        meetingId: { type: string, format: uuid }
        topic: { type: string }
        description: { type: string, nullable: true }
        presenterId: { type: string, format: uuid, nullable: true }
        sequence: { type: integer }
        durationMinutes: { type: integer, nullable: true }
    MeetingNote:
      type: object
      properties:
        id: { type: string, format: uuid }
        meetingId: { type: string, format: uuid }
        agendaId: { type: string, format: uuid, nullable: true }
        creatorId: { type: string, format: uuid }
        content: { type: object }
        createdAt: { type: string, format: date-time }
        updatedAt: { type: string, format: date-time }
    Language:
      type: object
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        compileCmd: { type: string, nullable: true }
        runCmd: { type: string }
        timeoutSeconds: { type: number, nullable: true }
    Room:
      type: object
      properties:
        id: { type: string, format: uuid }
        eventId: { type: string, format: uuid }
        name: { type: string }
        description: { type: string, nullable: true }
        createdAt: { type: string, format: date-time }
    RoomPlayer:
      type: object
      properties:
        roomId: { type: string, format: uuid }
        userProfileId: { type: string, format: uuid }
        score: { type: integer }
        place: { type: integer, nullable: true }
        state: { type: string, nullable: true }
        disconnectedAt: { type: string, format: date-time, nullable: true }
    CodeProblem:
      type: object
      properties:
        id: { type: string, format: uuid }
        title: { type: string }
        problemStatement: { type: string }
    TestCase:
      type: object
      properties:
        id: { type: string, format: uuid }
        codeProblemId: { type: string, format: uuid }
        input: { type: string }
        expectedOutput: { type: string }
        timeConstraint: { type: number, nullable: true }
        spaceConstraint: { type: integer, nullable: true }
    Submission:
      type: object
      properties:
        id: { type: string, format: uuid }
        userProfileId: { type: string, format: uuid }
        eventId: { type: string, format: uuid }
        codeProblemId: { type: string, format: uuid }
        languageId: { type: string, format: uuid }
        sourceCode: { type: string }
        status: { type: string, enum: [In Queue, Processing, Accepted, Wrong Answer, Time Limit Exceeded, Compilation Error, Runtime Error (Generic), Internal Error, Execution Server Error] }
        stdOut: { type: string, nullable: true }
        stdErr: { type: string, nullable: true }
        compileOutput: { type: string, nullable: true }
        exitCode: { type: integer, nullable: true }
        exitSignal: { type: integer, nullable: true }
        message: { type: string, nullable: true }
        time: { type: number, nullable: true }
        memory: { type: integer, nullable: true }
        wallTime: { type: number, nullable: true }
        token: { type: string, nullable: true }
        queuedAt: { type: string, format: date-time }
        startedAt: { type: string, format: date-time, nullable: true }
        finishedAt: { type: string, format: date-time, nullable: true }
        updatedAt: { type: string, format: date-time }
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
            packId: { type: string, format: uuid }
            version: { type: string }
            source: { type: string, enum: [Set, Seed] }
            createdAt: { type: string, format: date-time }
            subjectId: { type: string, format: uuid, nullable: true }
            curriculumId: { type: string, format: uuid, nullable: true }
            aiModel: { type: string, nullable: true }
            generatorVersion: { type: string, nullable: true }
            promptHash: { type: string, nullable: true }
            seed: { type: string, nullable: true }
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
              correctAnswers:
                type: array
                items: { type: string }
              explanation: { type: string, nullable: true }
              difficulty: { type: string, enum: [Beginner, Intermediate, Advanced], nullable: true }
              tags:
                type: array
                items: { type: string }
              sequence: { type: integer, nullable: true }
              weight: { type: number, nullable: true }
              timeLimitSec: { type: integer, nullable: true }
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
                scheduledStartTime: { type: string, format: date-time }
                scheduledEndTime: { type: string, format: date-time }
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
              required: [userProfileId]
              properties:
                userProfileId: { type: string, format: uuid }
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
              required: [eventId, codeProblemId, languageId, sourceCode]
              properties:
                eventId: { type: string, format: uuid }
                codeProblemId: { type: string, format: uuid }
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

