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
    Course:
      type: object
      properties:
        id: { type: string, format: uuid }
        userId: { type: string }
        name: { type: string }
        courseCode: { type: string, nullable: true }
        processingStatus: { type: string, enum: [Pending, Processing, Completed, Failed] }
        createdAt: { type: string, format: date-time }
        updatedAt: { type: string, format: date-time }
    GameSession:
      type: object
      properties:
        id: { type: string, format: uuid }
        questId: { type: string, format: uuid }
        status: { type: string, enum: [InProgress, Completed, Abandoned] }
        score: { type: integer }
        startedAt: { type: string, format: date-time }
        completedAt: { type: string, format: date-time, nullable: true }
    Error:
      type: object
      properties:
        code: { type: string }
        message: { type: string }

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
          
  # Course Management Endpoints
  /courses:
    get:
      summary: Get All Courses for Current User
      tags: [Courses]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: A list of the user's courses.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Course'

  /courses/{courseId}/syllabus:
    post:
      summary: Upload a Syllabus for a Course
      tags: [Courses]
      security:
        - BearerAuth: []
      parameters:
        - name: courseId
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
```
