erDiagram
    direction TB

    user_profiles {
        uuid auth_user_id PK "Primary Key, FK to Supabase auth.users table"
        text username UK "User's unique public name"
        text email UK "User's unique email address"
        text first_name "User's first name"
        text last_name "User's last name"
        uuid class_id FK "FK to the selected class"
        uuid route_id FK "FK to the selected curriculum program"
        int level "User's current level"
        int experience_points "User's total experience points"
        text profile_image_url "URL for the user's avatar"
        bool onboarding_completed "Flag for onboarding status"
        timestamp created_at "Timestamp of creation"
        timestamp updated_at "Last update timestamp"
    }

    roles {
        uuid id PK "Primary key for the role"
        text name UK "Name of the role (e.g., Student, Lecturer)"
        text description "Description of the role"
        timestamp created_at "Timestamp of creation"
    }

    user_roles {
        uuid auth_user_id PK,FK "Composite PK and FK to user_profiles"
        uuid role_id PK,FK "Composite PK and FK to roles"
        timestamp assigned_at "Timestamp when role was assigned"
    }

    lecturer_verification_requests {
        uuid id PK "Primary key for the verification request"
        uuid auth_user_id FK "FK to the user requesting verification"
        text institution_name "Name of the institution"
        text department "Department of the lecturer"
        text verification_document_url "URL to the verification document"
        text status "Enum: Pending, Approved, Rejected"
        timestamp submitted_at "Timestamp of submission"
        timestamp reviewed_at "Timestamp of review"
        uuid reviewer_id FK "FK to the user who reviewed the request"
        text notes "Notes from the reviewer"
    }

    classes {
        uuid id PK "Primary key for the class"
        text name "Name of the class"
        text description "Description of the class"
        text academic_year "Academic year (e.g., 2024-2025)"
        text semester "Semester (e.g., Fall, Spring)"
        uuid lecturer_id FK "FK to the lecturer (user_profiles)"
        timestamp created_at "Timestamp of creation"
        timestamp updated_at "Last update timestamp"
    }

    curriculum_programs {
        uuid id PK "Primary key for the curriculum program"
        text program_name "Name of the program (e.g., B.S. in Software Engineering)"
        text program_code UK "Unique code for the program (e.g., BSE)"
        text description "Description of the program"
        text degree_level "Enum: Bachelor, Master, etc."
        int total_credits "Total credits for the program"
        int duration_years "Duration of the program in years"
        timestamp created_at "Timestamp of creation"
        timestamp updated_at "Last update timestamp"
    }

    curriculum_versions {
        uuid id PK "Primary key for a version of a curriculum"
        uuid program_id FK "FK to the curriculum program"
        text version_code "Version code (e.g., K18A, 2024-Catalog)"
        int effective_year "The year this version becomes effective"
        bool is_active "Indicates if this is the active version"
        text description "Description of the version"
        timestamp created_at "Timestamp of creation"
    }

    subjects {
        uuid id PK "Primary key for a subject"
        text subject_code UK "Unique code for the subject (e.g., CS464)"
        text subject_name "Name of the subject"
        int credits "Number of credits for the subject"
        text description "Description of the subject"
        timestamp created_at "Timestamp of creation"
        timestamp updated_at "Last update timestamp"
    }

    curriculum_structure {
        uuid curriculum_version_id PK,FK "Composite PK and FK to curriculum_versions"
        uuid subject_id PK,FK "Composite PK and FK to subjects"
        int term_number "The prescribed term for this subject"
        bool is_mandatory "Indicates if the subject is mandatory"
        text prerequisite_subject_ids "Array of subject UUIDs"
        timestamp created_at "Timestamp of creation"
    }

    syllabus_versions {
        uuid id PK "Primary key for a version of a syllabus"
        uuid subject_id FK "FK to the subject"
        int version_number "Version number of the syllabus"
        json content "The structured content of the syllabus"
        date effective_date "Date this syllabus version is effective"
        bool is_active "Indicates if this is the active version"
        uuid created_by FK "FK to the user who created the syllabus"
        timestamp created_at "Timestamp of creation"
    }

    student_enrollments {
        uuid id PK "Primary key for a student's enrollment"
        uuid auth_user_id FK "FK to the student's profile"
        uuid curriculum_version_id FK "FK to the curriculum version"
        date enrollment_date "Date of enrollment"
        date expected_graduation_date "Expected graduation date"
        text status "Enum: Active, Graduated, Withdrawn"
        timestamp created_at "Timestamp of creation"
        timestamp updated_at "Last update timestamp"
    }

    student_term_subjects {
        uuid id PK "Primary key for a subject in a term"
        uuid enrollment_id FK "FK to the student's enrollment"
        uuid subject_id FK "FK to the subject"
        int term_number "The term number"
        text academic_year "The academic year"
        text semester "The semester"
        text status "Enum: Enrolled, Completed, Failed, Withdrawn"
        text grade "Final grade for the subject"
        int credits_earned "Credits earned"
        timestamp enrolled_at "Timestamp of enrollment"
        timestamp completed_at "Timestamp of completion"
    }

    user_skills {
        uuid id PK "Primary key for a user's skill"
        uuid auth_user_id FK "FK to the user's profile"
        text skill_name "Name of the skill"
        int experience_points "Experience points for this skill"
        int level "Level of this skill"
        timestamp last_updated_at "Last update timestamp"
    }

    user_quest_progress {
        uuid id PK "Primary key for quest progress"
        uuid auth_user_id FK "FK to the user's profile"
        uuid quest_id FK "FK to the quest in Quests Service"
        text status "Enum: NotStarted, InProgress, Completed"
        timestamp completed_at "Timestamp of completion"
        timestamp last_updated_at "Last update timestamp"
    }

    achievements {
        uuid id PK "Primary key for an achievement"
        text name UK "Name of the achievement"
        text description "Description of the achievement"
        text icon_url "URL for the achievement's icon"
        text source_service "The service that triggers this achievement"
    }

    user_achievements {
        uuid auth_user_id PK,FK "Composite PK and FK to user_profiles"
        uuid achievement_id PK,FK "Composite PK and FK to achievements"
        timestamp earned_at "Timestamp when the achievement was earned"
        json context "Additional details about how it was earned"
    }

    notifications {
        uuid id PK "Primary key for a notification"
        uuid auth_user_id FK "FK to the user's profile"
        text type "Enum: Achievement, QuestComplete, etc."
        text title "Title of the notification"
        text message "The notification message"
        bool is_read "Read status"
        json metadata "Additional notification data"
        timestamp created_at "Timestamp of creation"
        timestamp read_at "Timestamp when read"
    }
    
    user_profiles ||--o{ user_roles : " "
    roles ||--o{ user_roles : " "
    user_profiles ||--o{ lecturer_verification_requests : " "
    user_profiles ||--o{ classes : " "
    classes ||--o{ user_profiles : " "
    curriculum_programs ||--o{ user_profiles : " "
    curriculum_programs ||--o{ curriculum_versions : " "
    curriculum_versions ||--o{ student_enrollments : " "
    user_profiles ||--o{ student_enrollments : " "
    student_enrollments ||--o{ student_term_subjects : " "
    subjects ||--o{ student_term_subjects : " "
    subjects ||--o{ syllabus_versions : " "
    user_profiles ||--o{ syllabus_versions : " "
    curriculum_versions ||--o{ curriculum_structure : " "
    subjects ||--o{ curriculum_structure : " "
    user_profiles ||--o{ user_skills : " "
    user_profiles ||--o{ user_quest_progress : " "
    user_profiles ||--o{ user_achievements : " "
    achievements ||--o{ user_achievements : " "
    user_profiles ||--o{ notifications : " "