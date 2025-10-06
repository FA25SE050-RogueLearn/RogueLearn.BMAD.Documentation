# User Service ERD

## Entity Relationship Diagram

```mermaid
erDiagram
    %% Authentication & Core User Management
    AuthUsers ||--|| UserProfile : authenticates
    UserProfile ||--o{ UserRole : has
    Role ||--o{ UserRole : assigned_to
    UserProfile ||--o{ LecturerVerificationRequest : submits
    "Class" ||--o{ UserProfile : contains
    
    %% Enhanced Curriculum & Student Tracking
    CurriculumProgram ||--o{ CurriculumVersion : has_versions
    CurriculumVersion ||--o{ StudentEnrollment : enrolls_students
    UserProfile ||--o{ StudentEnrollment : enrolled_in
    StudentEnrollment ||--o{ StudentTermSubject : tracks_subjects
    Subject ||--o{ StudentTermSubject : taken_by_students
    Subject ||--o{ SyllabusVersion : has_versions
    
    %% User Progression & Achievements
    UserProfile ||--o{ UserSkill : develops
    UserProfile ||--o{ Notification : receives
    UserProfile ||--o{ UserQuestProgress : tracks
    UserProfile ||--o{ UserAchievement : earns
    Achievement ||--o{ UserAchievement : awarded_as

    %% Admin-Owned Educational Governance
    UserProfile ||--o{ ElectiveSource : submits
    ElectiveSource ||--o{ ElectivePack : curated_into
    Subject ||--o{ ElectivePack : relates_to
    CurriculumVersion ||--o{ ElectivePack : aligned_with
    UserProfile ||--o{ ElectivePack : approved_by
    UserProfile ||--o{ CurriculumImportJob : creates
    CurriculumProgram ||--o{ CurriculumImportJob : imports_for
    CurriculumVersion ||--o{ CurriculumVersionActivation : has_activations
    UserProfile ||--o{ CurriculumVersionActivation : activates
```

## Key Features

### Core User Management
- **UserProfiles**: Central user data linked to Supabase Auth
- **Roles & UserRoles**: Role-based access control (RBAC)
- **LecturerVerificationRequests**: Lecturer verification workflow

### Academic Structure

#### Enhanced Curriculum Management (Current)
- **CurriculumPrograms**: Abstract program definitions (e.g., "B.S. in Software Engineering")
- **CurriculumVersions**: Versioned curriculum snapshots (e.g., "K18A", "K18B")
- **Subjects**: Master list of all subjects with codes and credits
- **StudentEnrollments**: Links students to specific curriculum versions
- **StudentTermSubjects**: Tracks actual subject enrollment per academic term
- **SyllabusVersions**: Versioned syllabus for each subject

### Student Progress Tracking
- **StudentTermSubjects**: Comprehensive tracking of student academic journey
  - Handles retakes, failures, and deviations from prescribed curriculum
  - Tracks status: 'Enrolled', 'Completed', 'Failed', 'Withdrawn'
  - Supports flexible academic term definitions

### User Progression
- **UserSkills**: User skill progression tracking
- **UserQuestProgress**: Summary of user progress on quests for quick reference
- **Achievements**: Central catalog of all possible achievements
- **UserAchievements**: Links users to earned achievements with context
- **Notifications**: User notification system

### Data Integrity
- All user-related data cascades on user deletion
- Role deletions are restricted to prevent orphaned assignments
- Unique constraints on usernames and emails
- Foreign key relationships maintain referential integrity
- Enhanced curriculum structure supports versioning and academic flexibility

### Admin-Owned Educational Governance
- **ElectiveSource**: Faculty/community submissions proposed for elective content library
- **ElectivePack**: Versioned elective collections aligned to subjects or curriculum versions
- **CurriculumImportJob**: Administrative job to import curriculum catalogs (file/API) per program
- **CurriculumVersionActivation**: Scheduled activation records with audit for curriculum version changes