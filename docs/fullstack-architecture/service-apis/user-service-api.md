# User Service API Specification

This service manages user identity and academic context: profiles, academic structures (programs, subjects), enrollments, lecturer verification, achievements, skills, notes (Arsenal), and notifications.

```yaml
openapi: 3.0.0
info:
  title: RogueLearn User Service API
  version: v1.0.0
  description: Central authority for user identity, academic context, and personal notes (Arsenal).
servers:
  - url: https://api.roguelearn.com/v1
    description: Production

security:
  - BearerAuth: []

components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: JWT token obtained from Supabase after login.
  schemas:
    Error:
      type: object
      properties:
        code: { type: string }
        message: { type: string }
    UserProfile:
      type: object
      properties:
        id: { type: string, format: uuid }
        auth_user_id: { type: string, format: uuid }
        username: { type: string }
        email: { type: string }
        first_name: { type: string, nullable: true }
        last_name: { type: string, nullable: true }
        class_id: { type: string, format: uuid }
        route_id: { type: string, format: uuid, nullable: true }
        level: { type: integer }
        experience_points: { type: integer }
        onboarding_completed: { type: boolean }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
    CurriculumProgram:
      type: object
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        description: { type: string, nullable: true }
    CurriculumVersion:
      type: object
      properties:
        id: { type: string, format: uuid }
        program_id: { type: string, format: uuid }
        name: { type: string }
        effective_date: { type: string, format: date }
    Subject:
      type: object
      properties:
        id: { type: string, format: uuid }
        code: { type: string }
        name: { type: string }
        description: { type: string, nullable: true }
    CurriculumSubjectLink:
      type: object
      properties:
        id: { type: string, format: uuid }
        curriculum_version_id: { type: string, format: uuid }
        subject_id: { type: string, format: uuid }
        term_number: { type: integer }
        prerequisites: { type: array, items: { type: string, format: uuid }, nullable: true }
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
    LecturerVerificationRequest:
      type: object
      properties:
        id: { type: string, format: uuid }
        auth_user_id: { type: string, format: uuid }
        institution_name: { type: string }
        documents_url: { type: string }
        status: { type: string, enum: [Pending, Approved, Rejected] }
        submitted_at: { type: string, format: date-time }
        reviewed_at: { type: string, format: date-time, nullable: true }
    SkillTree:
      type: object
      properties:
        nodes:
          type: array
          items:
            type: object
            properties:
              id: { type: string, format: uuid }
              name: { type: string }
              unlocked: { type: boolean }
              progress_percent: { type: number }
    Achievement:
      type: object
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        earned_at: { type: string, format: date-time }
        description: { type: string, nullable: true }
    Notification:
      type: object
      properties:
        id: { type: string, format: uuid }
        type: { type: string }
        message: { type: string }
        read: { type: boolean }
        created_at: { type: string, format: date-time }
    Note:
      type: object
      properties:
        id: { type: string, format: uuid }
        auth_user_id: { type: string, format: uuid }
        title: { type: string }
        content: { type: object }
        is_public: { type: boolean }
        tags: { type: array, items: { type: string } }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
    NoteLinkRequest:
      type: object
      properties:
        quest_id: { type: string, format: uuid, nullable: true }
        skill_id: { type: string, format: uuid, nullable: true }

tags:
  - name: Profiles
    description: User profile management
  - name: Academic
    description: Academic structure and enrollment management
  - name: Admin
    description: Administrative endpoints
  - name: Verification
    description: Lecturer verification workflows
  - name: Skills
    description: Skill tree and progression
  - name: Achievements
    description: User achievements
  - name: Notifications
    description: User notifications
  - name: Notes
    description: Personal user notes (Arsenal) management

paths:
  # Profiles & Onboarding
  /profiles/me:
    get:
      summary: Get Current User's Profile
      description: Retrieves the complete profile for the currently authenticated user, including level, XP, and academic enrollment details.
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
    put:
      summary: Update Current User's Profile
      description: Updates the current user's profile information (e.g., username, first_name, last_name).
      tags: [Profiles]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                username: { type: string }
                first_name: { type: string }
                last_name: { type: string }
      responses:
        '200':
          description: Updated profile.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserProfile'

  /profiles/me/onboarding:
    post:
      summary: Complete Character Creation Onboarding
      description: Completes onboarding by submitting chosen Route (Curriculum) and Class (Career Specialization). Triggers initial questline generation.
      tags: [Profiles]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [curriculumVersionId, careerRoadmapId]
              properties:
                curriculumVersionId: { type: string, format: uuid }
                careerRoadmapId: { type: string, format: uuid }
      responses:
        '202':
          description: Onboarding completed; questline generation initiated.

  # Academic Management (Admin)
  /admin/curriculum-programs:
    get:
      summary: List curriculum programs
      description: Lists all curriculum programs.
      tags: [Academic, Admin]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Programs list.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/CurriculumProgram'
    post:
      summary: Create curriculum program
      description: Creates a new curriculum program.
      tags: [Academic, Admin]
      security:
        - BearerAuth: []
      requestBody:
        required: true
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
          description: Program created.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CurriculumProgram'

  /admin/subjects:
    get:
      summary: List subjects
      description: Lists all academic subjects.
      tags: [Academic, Admin]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Subjects list.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Subject'
    post:
      summary: Create subject
      description: Creates a new subject.
      tags: [Academic, Admin]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [code, name]
              properties:
                code: { type: string }
                name: { type: string }
                description: { type: string }
      responses:
        '201':
          description: Subject created.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Subject'

  /admin/curriculum-programs/{programId}/versions:
    get:
      summary: List versions for a curriculum program
      description: Lists all versions for a specific curriculum program.
      tags: [Academic, Admin]
      security:
        - BearerAuth: []
      parameters:
        - name: programId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Versions list.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/CurriculumVersion'
    post:
      summary: Create curriculum version
      description: Creates a new curriculum version.
      tags: [Academic, Admin]
      security:
        - BearerAuth: []
      parameters:
        - name: programId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [name, effective_date]
              properties:
                name: { type: string }
                effective_date: { type: string, format: date }
      responses:
        '201':
          description: Version created.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CurriculumVersion'

  /admin/curriculum-versions/{versionId}/subjects:
    post:
      summary: Associate subject to curriculum version
      description: Associates a subject with a curriculum version, defining its term and prerequisites.
      tags: [Academic, Admin]
      security:
        - BearerAuth: []
      parameters:
        - name: versionId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [subjectId, termNumber]
              properties:
                subjectId: { type: string, format: uuid }
                termNumber: { type: integer }
                prerequisites: { type: array, items: { type: string, format: uuid } }
      responses:
        '201':
          description: Link created.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CurriculumSubjectLink'

  # Enrollments & Syllabus
  /enrollments:
    get:
      summary: Get all enrollments for current user
      description: Retrieves all academic enrollments for the current user.
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
      description: Creates a new academic enrollment for the user, linking them to a curriculum_version_id.
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
      summary: Upload syllabus document for an enrollment
      description: Uploads a syllabus file (PDF, DOCX) for an enrollment, triggering AI processing to generate quests.
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

  # Verification
  /verification/requests:
    post:
      summary: Submit lecturer verification request
      description: Submits a request for lecturer verification, including necessary documents.
      tags: [Verification]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LecturerVerificationRequest'
      responses:
        '201':
          description: Verification request submitted.

  /admin/verification/requests:
    get:
      summary: List pending verification requests (Admin)
      description: Lists all pending verification requests.
      tags: [Verification, Admin]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Pending requests.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/LecturerVerificationRequest'

  /admin/verification/requests/{requestId}/approve:
    post:
      summary: Approve verification request (Admin)
      description: Approves a verification request.
      tags: [Verification, Admin]
      security:
        - BearerAuth: []
      parameters:
        - name: requestId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Request approved.

  /admin/verification/requests/{requestId}/reject:
    post:
      summary: Reject verification request (Admin)
      description: Rejects a verification request.
      tags: [Verification, Admin]
      security:
        - BearerAuth: []
      parameters:
        - name: requestId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Request rejected.

  # Skills, Achievements & Notifications
  /skills/tree:
    get:
      summary: Get personalized skill tree
      description: Retrieves the user's personalized skill tree, showing progress and unlocked nodes.
      tags: [Skills]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Skill tree.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/SkillTree'

  /achievements:
    get:
      summary: List user achievements
      description: Retrieves all achievements earned by the current user.
      tags: [Achievements]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Achievements list.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Achievement'

  /notifications:
    get:
      summary: List user notifications
      description: Retrieves all notifications for the current user.
      tags: [Notifications]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Notifications list.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Notification'

  /notifications/{notificationId}/read:
    post:
      summary: Mark notification as read
      description: Marks a specific notification as read.
      tags: [Notifications]
      security:
        - BearerAuth: []
      parameters:
        - name: notificationId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Notification marked as read.

  # Notes (Arsenal) Management
  /notes:
    get:
      summary: List all notes for current user
      description: Retrieves a list of all notes in the current user's Arsenal.
      tags: [Notes]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: A list of notes.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Note'
    post:
      summary: Create a new note
      description: Adds a new note to the user's Arsenal.
      tags: [Notes]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [title]
              properties:
                title: { type: string }
                content: { type: object }
                is_public: { type: boolean, default: false }
                tags: { type: array, items: { type: string } }
      responses:
        '201':
          description: Note created successfully.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Note'

  /notes/{noteId}:
    get:
      summary: Get a specific note
      description: Retrieves a single note by its ID.
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
          description: The requested note.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Note'
        '404':
          description: Note not found.
    put:
      summary: Update a note
      description: Updates the content, title, or tags of an existing note.
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
                content: { type: object }
                is_public: { type: boolean }
                tags: { type: array, items: { type: string } }
      responses:
        '200':
          description: Note updated successfully.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Note'
    delete:
      summary: Delete a note
      description: Permanently deletes a note from the user's Arsenal.
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
          description: Note deleted successfully.

  /notes/{noteId}/links:
    post:
      summary: Link a note to a quest or skill
      description: Creates an association between a note and a quest (from Quests Service) or a skill.
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
              $ref: '#/components/schemas/NoteLinkRequest'
      responses:
        '201':
          description: Link created successfully.
    delete:
      summary: Unlink a note from a quest or skill
      description: Removes an association between a note and a quest or skill.
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
              $ref: '#/components/schemas/NoteLinkRequest'
      responses:
        '204':
          description: Link removed successfully.