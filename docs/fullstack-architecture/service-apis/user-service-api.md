# User Service API Specification

```yaml
openapi: 3.0.0
info:
  title: RogueLearn User Service API
  version: v1.0.0
  description: User profiles and account-related endpoints.
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
        subject_id: { type: string, format: uuid }
        version_number: { type: integer }
        content: { type: object }
        effective_date: { type: string, format: date }
        is_active: { type: boolean }
    UserSyllabusEnrollment:
      type: object
      properties:
        id: { type: string, format: uuid }
        auth_user_id: { type: string, format: uuid }
        curriculum_version_id: { type: string, format: uuid }
        enrollment_date: { type: string, format: date }
        expected_graduation_date: { type: string, format: date, nullable: true }
        status: { type: string, enum: [Active, Graduated, Withdrawn, Suspended] }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
    UserUploadedSyllabus:
      type: object
      properties:
        id: { type: string, format: uuid }
        enrollment_id: { type: string, format: uuid }
        file_url: { type: string }
        processing_status: { type: string, enum: [Pending, Processing, Completed, Failed] }
        structured_content: { type: object, nullable: true }
        uploaded_at: { type: string, format: date-time }
    Error:
      type: object
      properties:
        code: { type: string }
        message: { type: string }

paths:
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
    post:
      summary: Create a new enrollment for current user
      tags: [Academic]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [curriculum_version_id]
              properties:
                curriculum_version_id: { type: string, format: uuid }
                enrollment_date: { type: string, format: date, nullable: true }
                expected_graduation_date: { type: string, format: date, nullable: true }
                status: { type: string, enum: [Active, Graduated, Withdrawn, Suspended], nullable: true }
      responses:
        '201':
          description: Enrollment created
          content:
            application/json:
              schema:
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
```