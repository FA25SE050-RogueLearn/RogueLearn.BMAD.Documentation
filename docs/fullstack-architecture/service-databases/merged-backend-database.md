# Merged Backend Database Schema

## Overview
This document defines the unified database schema that merges the User Service, Quests Service, Social Service, and Meeting Service databases into a single comprehensive backend database. This consolidation simplifies data management, reduces cross-service complexity, and improves performance while maintaining clear domain boundaries through table organization.

**Note**: The Code Event Service database remains separate and is not included in this merge.

-- Enable UUID generation if not already enabled
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

## Enums and Types

### User Service Enums
```sql
-- User management enums
CREATE TYPE user_role_type AS ENUM ('Student', 'Lecturer', 'Admin', 'SuperAdmin');
CREATE TYPE verification_status AS ENUM ('Pending', 'Approved', 'Rejected');
CREATE TYPE enrollment_status AS ENUM ('Active', 'Inactive', 'Graduated', 'Dropped', 'Suspended');
CREATE TYPE semester_status AS ENUM ('Enrolled', 'Completed', 'Failed', 'Withdrawn');
CREATE TYPE skill_relationship_type AS ENUM ('Prerequisite', 'Corequisite', 'Recommended');
CREATE TYPE skill_reward_source_type AS ENUM ('QuestComplete', 'BossFight', 'PartyActivity', 'GuildActivity', 'MeetingParticipation', 'CodeChallenge');
CREATE TYPE skill_tier_level AS ENUM ('Foundation', 'Intermediate', 'Advanced', 'Expert');
CREATE TYPE notification_type AS ENUM ('Achievement', 'QuestComplete', 'PartyInvite', 'GuildInvite', 'FriendRequest', 'System', 'Reminder');
CREATE TYPE degree_level AS ENUM ('Associate', 'Bachelor', 'Master', 'Doctorate');
CREATE TYPE subject_enrollment_status AS ENUM ('Enrolled', 'Completed', 'Failed', 'Withdrawn');
```

### Quests Service Enums
```sql
-- Quest and learning path enums
CREATE TYPE quest_type AS ENUM ('Tutorial', 'Practice', 'Challenge', 'Project', 'Assessment', 'Exploration');
CREATE TYPE difficulty_level AS ENUM ('Beginner', 'Intermediate', 'Advanced', 'Expert');
CREATE TYPE step_type AS ENUM ('Reading', 'Video', 'Interactive', 'Coding', 'Quiz', 'Discussion', 'Submission', 'Reflection');
CREATE TYPE resource_type AS ENUM ('Document', 'Video', 'Audio', 'Interactive', 'Link', 'Code', 'Dataset');
CREATE TYPE quest_attempt_status AS ENUM ('InProgress', 'Completed', 'Abandoned', 'Paused');
CREATE TYPE step_completion_status AS ENUM ('NotStarted', 'InProgress', 'Completed', 'Skipped');
CREATE TYPE path_type AS ENUM ('Course', 'Specialization', 'Bootcamp', 'Custom');
CREATE TYPE path_progress_status AS ENUM ('NotStarted', 'InProgress', 'Completed', 'Paused');
CREATE TYPE assessment_type AS ENUM ('Quiz', 'Assignment', 'Project', 'PeerReview', 'AutoGraded', 'ManualReview');
CREATE TYPE quest_status AS ENUM ('NotStarted', 'InProgress', 'Completed', 'Abandoned');
```

### Social Service Enums
```sql
-- Social interaction enums
CREATE TYPE party_type AS ENUM ('StudyGroup', 'ProjectTeam', 'PeerReview', 'Casual', 'Competition');
CREATE TYPE party_role AS ENUM ('Leader', 'CoLeader', 'Member');
CREATE TYPE guild_role AS ENUM ('GuildMaster', 'Officer', 'Veteran', 'Member', 'Recruit');
CREATE TYPE member_status AS ENUM ('Active', 'Inactive', 'Suspended', 'Left');
CREATE TYPE invitation_status AS ENUM ('Pending', 'Accepted', 'Declined', 'Expired', 'Cancelled');
CREATE TYPE invitation_type AS ENUM ('Invite', 'Application');
CREATE TYPE guild_type AS ENUM ('Academic', 'Professional', 'Hobby', 'Competition', 'Study', 'Research');
CREATE TYPE activity_type AS ENUM ('QuestCompletion', 'StudySession', 'ProjectWork', 'Discussion', 'Competition', 'Meeting');
CREATE TYPE message_type AS ENUM ('Direct', 'Party', 'Guild');
CREATE TYPE friendship_status AS ENUM ('Pending', 'Accepted', 'Blocked');
```

### Meeting Service Enums
```sql
-- Meeting management enums
CREATE TYPE transcript_segment_status AS ENUM ('Processed', 'Failed');
```

## Database Tables

### User Service Tables

#### Core User Management

##### roles
System roles and permissions.
```sql
CREATE TABLE roles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    permissions JSONB,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### Academic Structure

##### classes
Roadmap.sh specialization classes (Frontend, Backend, DevOps, etc.).
```sql
CREATE TABLE classes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL, -- e.g., "Backend Developer", "Frontend Developer"
    description TEXT,
    roadmap_url TEXT, -- Link to roadmap.sh specialization
    skill_focus_areas TEXT[], -- Array of primary skill domains
    difficulty_level difficulty_level DEFAULT 'Beginner',
    estimated_duration_months INTEGER, -- Expected time to complete roadmap
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### class_nodes
Hierarchical structure within classes (nodes in the roadmap tree).
```sql
CREATE TABLE class_nodes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    class_id UUID NOT NULL REFERENCES classes(id) ON DELETE CASCADE,
    parent_id UUID REFERENCES class_nodes(id) ON DELETE CASCADE,
    title TEXT NOT NULL,
    node_type TEXT,
    description TEXT,
    sequence INTEGER NOT NULL DEFAULT 0,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    is_locked_by_import BOOLEAN NOT NULL DEFAULT FALSE,
    metadata JSONB,
    created_at TIMESTAMPTZ DEFAULT now()
);
```


##### curriculum_programs
University degree programs and curricula.
```sql
CREATE TABLE curriculum_programs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    program_name VARCHAR(255) NOT NULL,
    program_code VARCHAR(50) NOT NULL UNIQUE,
    description TEXT,
    degree_level degree_level NOT NULL,
    total_credits INTEGER,
    duration_years INTEGER,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### user_profiles
Central user profile management with gamification elements.
```sql
CREATE TABLE user_profiles (
    auth_user_id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
    username VARCHAR(255) NOT NULL UNIQUE,
    bio TEXT,
    email VARCHAR(255) NOT NULL UNIQUE,
    first_name VARCHAR(255),
    last_name VARCHAR(255),
    class_id UUID REFERENCES classes(id), -- Selected roadmap.sh specialization (Step 2)
    route_id UUID REFERENCES curriculum_programs(id), -- Selected curriculum (Step 1)
    level INTEGER NOT NULL DEFAULT 1,
    experience_points INTEGER NOT NULL DEFAULT 0,
    profile_image_url TEXT,
    preferences JSONB,
    onboarding_completed BOOLEAN NOT NULL DEFAULT FALSE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### user_roles
Many-to-many relationship between users and roles.
```sql
CREATE TABLE user_roles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    role_id UUID NOT NULL REFERENCES roles(id) ON DELETE CASCADE,
    assigned_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    assigned_by UUID REFERENCES user_profiles(auth_user_id),
    UNIQUE (auth_user_id, role_id)
);
```

##### lecturer_verification_requests
Lecturer role verification system.
```sql
CREATE TABLE lecturer_verification_requests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    status verification_status NOT NULL DEFAULT 'Pending',
    documents JSONB,
    submitted_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    reviewed_at TIMESTAMPTZ,
    reviewer_id UUID REFERENCES user_profiles(auth_user_id),
    review_notes TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### curriculum_versions
Versioned curriculum definitions.
```sql
CREATE TABLE curriculum_versions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    program_id UUID NOT NULL REFERENCES curriculum_programs(id) ON DELETE CASCADE,
    version_code VARCHAR(50) NOT NULL,
    effective_year INTEGER NOT NULL,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    description TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (program_id, version_code)
);
```

##### subjects
University subjects and courses.
```sql
CREATE TABLE subjects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    subject_code VARCHAR(50) NOT NULL UNIQUE,
    subject_name VARCHAR(255) NOT NULL,
    credits INTEGER NOT NULL,
    description TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### class_specialization_subjects
Maps class specializations to university subjects.
```sql
CREATE TABLE class_specialization_subjects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    class_id UUID NOT NULL REFERENCES classes(id) ON DELETE CASCADE,
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    placeholder_subject_code TEXT NOT NULL,
    semester INTEGER NOT NULL
);
```

##### curriculum_structure
Defines which subjects belong to which curriculum version and semester.
```sql
CREATE TABLE curriculum_structure (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    curriculum_version_id UUID NOT NULL REFERENCES curriculum_versions(id) ON DELETE CASCADE,
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    semester INTEGER NOT NULL,
    is_mandatory BOOLEAN NOT NULL DEFAULT TRUE,
    prerequisite_subject_ids UUID[],
    prerequisites_text TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (curriculum_version_id, subject_id)
);
```

##### syllabus_versions
Versioned syllabus content for subjects.
```sql
CREATE TABLE syllabus_versions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    content JSONB NOT NULL,
    effective_date DATE NOT NULL,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_by UUID REFERENCES user_profiles(auth_user_id),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (subject_id, version_number)
);
```

#### Admin-Owned Educational Governance

##### curriculum_version_activations
Tracks curriculum version activations by administrators.
```sql
CREATE TABLE curriculum_version_activations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    curriculum_version_id UUID NOT NULL REFERENCES curriculum_versions(id) ON DELETE CASCADE,
    effective_year INTEGER NOT NULL,
    activated_by UUID REFERENCES user_profiles(auth_user_id),
    activated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    notes TEXT,
    UNIQUE (curriculum_version_id, effective_year)
);
```

##### student_enrollments
Student enrollment in curriculum programs.
```sql
CREATE TABLE student_enrollments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    curriculum_version_id UUID NOT NULL REFERENCES curriculum_versions(id) ON DELETE CASCADE,
    enrollment_date DATE NOT NULL,
    expected_graduation_date DATE,
    status enrollment_status NOT NULL DEFAULT 'Active',
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (auth_user_id, curriculum_version_id)
);
```

##### student_semester_subjects
Tracks student enrollment in specific subjects per semester.
```sql
CREATE TABLE student_semester_subjects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    enrollment_id UUID NOT NULL REFERENCES student_enrollments(id) ON DELETE CASCADE,
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    academic_year VARCHAR(10) NOT NULL,
    semester INTEGER NOT NULL,
    status subject_enrollment_status NOT NULL DEFAULT 'Enrolled',
    grade VARCHAR(5),
    credits_earned INTEGER DEFAULT 0,
    enrolled_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    completed_at TIMESTAMPTZ,
    UNIQUE (enrollment_id, subject_id, academic_year, semester)
);
```

#### Skill Catalog

##### skills
Central skill catalog with hierarchical organization.
```sql
CREATE TABLE skills (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL UNIQUE,
    domain VARCHAR(100), -- e.g., Programming, Mathematics, Design
    tier skill_tier_level NOT NULL DEFAULT 'Foundation', -- Foundation, Intermediate, Advanced, Expert
    description TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### skill_dependencies
Skill prerequisite and relationship mapping.
```sql
CREATE TABLE skill_dependencies (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    prerequisite_skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    relationship_type skill_relationship_type NOT NULL DEFAULT 'Prerequisite', -- Prerequisite, Complements, Alternative
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),  
    UNIQUE (skill_id, prerequisite_skill_id, relationship_type)
);
```

#### Notes Management (Arsenal)

### Quests Service Tables (moved earlier for dependency order)

#### Quest Management

##### quests (moved earlier)
Core quest definitions with metadata and configuration.
```sql
CREATE TABLE quests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    quest_type quest_type NOT NULL,
    difficulty_level difficulty_level NOT NULL,
    estimated_duration_minutes INTEGER,
    experience_points_reward INTEGER NOT NULL DEFAULT 0,
    skill_tags TEXT[],
    subject_id UUID REFERENCES subjects(id),
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_by UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### notes
User-created learning notes and resources.
```sql
CREATE TABLE notes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    title TEXT NOT NULL,
    content JSONB,
    is_public BOOLEAN NOT NULL DEFAULT false,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### note_quests
Links notes to specific quests.
```sql
CREATE TABLE note_quests (
    note_id UUID NOT NULL REFERENCES notes(id) ON DELETE CASCADE,
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE, -- This references quests.id in the Quests Service. No FK constraint.
    PRIMARY KEY (note_id, quest_id)
);
```

##### note_skills
Links notes to specific skills.
```sql
CREATE TABLE note_skills (
    note_id UUID NOT NULL REFERENCES notes(id) ON DELETE CASCADE,
    skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    PRIMARY KEY (note_id, skill_id)
);
```

##### tags
User-defined tags for organizing notes.
```sql
CREATE TABLE tags (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    UNIQUE (auth_user_id, name)
);
```

##### note_tags
Many-to-many relationship between notes and tags.
```sql
CREATE TABLE note_tags (
    note_id UUID NOT NULL REFERENCES notes(id) ON DELETE CASCADE,
    tag_id UUID NOT NULL REFERENCES tags(id) ON DELETE CASCADE,
    PRIMARY KEY (note_id, tag_id)
);
```

#### Skills and Progress Tracking

##### user_skills
User skill progression and levels.
```sql
CREATE TABLE user_skills (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    skill_name VARCHAR(255) NOT NULL,
    experience_points INTEGER NOT NULL DEFAULT 0,
    level INTEGER NOT NULL DEFAULT 1,
    last_updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (auth_user_id, skill_name)
);
```

##### user_quest_progress
Tracks user progress on quests (cross-reference with Quests Service).
```sql
CREATE TABLE user_quest_progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE, -- This references quests.id in the Quests Service. No FK constraint.
    status quest_status NOT NULL,
    completed_at TIMESTAMPTZ,
    last_updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (auth_user_id, quest_id)
);
```

##### user_skill_rewards
Event-sourced ledger of XP rewards applied to user skills.
```sql
CREATE TABLE user_skill_rewards (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    source_service VARCHAR(100) NOT NULL, -- e.g., QuestsService, CodeBattleService, MeetingService
    source_type skill_reward_source_type NOT NULL, -- e.g., QuestComplete, BossFight, PartyActivity
    source_id UUID, -- Optional reference to the originating entity
    skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    skill_name VARCHAR(255) NOT NULL, -- Name from skills catalog
    points_awarded INTEGER NOT NULL,
    reason TEXT, -- Optional human-readable context
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### Achievements System

##### achievements
Central catalog of all possible achievements with type categorization.
```sql
CREATE TABLE achievements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL UNIQUE,
    description TEXT NOT NULL,
    icon_url TEXT,
    source_service VARCHAR(255) NOT NULL, -- e.g., 'QuestsService', 'CodeBattleService'
    key TEXT,
    rule_type TEXT,
    rule_config JSONB,
    category TEXT,
    version INTEGER,
    is_active BOOLEAN NOT NULL DEFAULT true
);
```

##### user_achievements
Links users to earned achievements with context and timestamps.
```sql
CREATE TABLE user_achievements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    achievement_id UUID NOT NULL REFERENCES achievements(id) ON DELETE CASCADE,
    earned_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    context JSONB -- To store additional details, like the event or quest that triggered the achievement
);
```

#### Notifications

##### notifications
User notification system with type-based categorization.
```sql
CREATE TABLE notifications (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    type notification_type NOT NULL,
    title VARCHAR(255) NOT NULL,
    message TEXT NOT NULL,
    is_read BOOLEAN NOT NULL DEFAULT FALSE,
    metadata JSONB,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    read_at TIMESTAMPTZ
);
```

### Quests Service Tables

#### Learning Paths and Curriculum Integration

##### learning_paths
Top-level container for an entire learning journey (QuestLine).
```sql
CREATE TABLE learning_paths (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    path_type path_type NOT NULL,
    curriculum_version_id UUID REFERENCES curriculum_versions(id),
    estimated_total_duration_hours INTEGER,
    total_experience_points INTEGER NOT NULL DEFAULT 0,
    is_published BOOLEAN NOT NULL DEFAULT FALSE,
    created_by UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### quest_chapters
High-level thematic groupings of quests within a learning path.
```sql
CREATE TABLE quest_chapters (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    learning_path_id UUID NOT NULL REFERENCES learning_paths(id) ON DELETE CASCADE,
    title TEXT NOT NULL,
    sequence INT NOT NULL,
    status path_progress_status NOT NULL DEFAULT 'NotStarted',
    start_date DATE,
    end_date DATE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### learning_path_quests
Defines which quests belong to a learning path and in what order.
```sql
CREATE TABLE learning_path_quests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    learning_path_id UUID NOT NULL REFERENCES learning_paths(id) ON DELETE CASCADE,
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    difficulty_level difficulty_level NOT NULL,
    sequence_order INTEGER NOT NULL,
    is_mandatory BOOLEAN NOT NULL DEFAULT TRUE,
    unlock_criteria JSONB,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (learning_path_id, quest_id),
    UNIQUE (learning_path_id, sequence_order)
);
```

#### Quest Management

##### quest_prerequisites
A join table to define the many-to-many relationship for quest dependencies.
```sql
CREATE TABLE quest_prerequisites (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    prerequisite_quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE(quest_id, prerequisite_quest_id)
);
```

##### quest_steps
Individual, actionable tasks a user must complete to finish a single Quest.
```sql
CREATE TABLE quest_steps (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    step_number INTEGER NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    step_type step_type NOT NULL,
    content JSONB,
    validation_criteria JSONB,
    experience_points INTEGER NOT NULL DEFAULT 0,
    is_optional BOOLEAN NOT NULL DEFAULT FALSE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (quest_id, step_number)
);
```

##### quest_resources
Learning resources attached to quests (documents, videos, links, etc.).
```sql
CREATE TABLE quest_resources (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    resource_type resource_type NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    url TEXT,
    file_path TEXT,
    metadata JSONB,
    display_order INTEGER NOT NULL DEFAULT 0,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### User Progress Tracking

##### user_quest_attempts
Tracks user attempts and progress on quests.
```sql
CREATE TABLE user_quest_attempts (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    status quest_attempt_status NOT NULL DEFAULT 'InProgress',
    started_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    completed_at TIMESTAMPTZ,
    abandoned_at TIMESTAMPTZ,
    total_experience_earned INTEGER NOT NULL DEFAULT 0,
    completion_percentage DECIMAL(5,2) NOT NULL DEFAULT 0.00,
    current_step_id UUID REFERENCES quest_steps(id),
    notes TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (auth_user_id, quest_id)
);
```

##### user_quest_step_progress
Detailed tracking of individual step completion.
```sql
CREATE TABLE user_quest_step_progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    attempt_id UUID NOT NULL REFERENCES user_quest_attempts(id) ON DELETE CASCADE,
    step_id UUID NOT NULL REFERENCES quest_steps(id) ON DELETE CASCADE,
    status step_completion_status NOT NULL DEFAULT 'NotStarted',
    started_at TIMESTAMPTZ,
    completed_at TIMESTAMPTZ,
    submission_data JSONB,
    feedback TEXT,
    experience_earned INTEGER NOT NULL DEFAULT 0,
    attempts_count INTEGER NOT NULL DEFAULT 0,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (attempt_id, step_id)
);
```

##### user_learning_path_progress
Tracks user progress through learning paths.
```sql
CREATE TABLE user_learning_path_progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    learning_path_id UUID NOT NULL REFERENCES learning_paths(id) ON DELETE CASCADE,
    status path_progress_status NOT NULL DEFAULT 'NotStarted',
    started_at TIMESTAMPTZ,
    completed_at TIMESTAMPTZ,
    current_quest_id UUID REFERENCES quests(id),
    completed_quests_count INTEGER NOT NULL DEFAULT 0,
    total_quests_count INTEGER NOT NULL DEFAULT 0,
    completion_percentage DECIMAL(5,2) NOT NULL DEFAULT 0.00,
    total_experience_earned INTEGER NOT NULL DEFAULT 0,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (auth_user_id, learning_path_id)
);
```

#### Assessment and Validation

##### quest_assessments
Assessment configurations for quests requiring evaluation.
```sql
CREATE TABLE quest_assessments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    assessment_type assessment_type NOT NULL,
    configuration JSONB NOT NULL,
    passing_criteria JSONB NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### quest_submissions
User submissions for assessed quests.
```sql
CREATE TABLE quest_submissions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    attempt_id UUID NOT NULL REFERENCES user_quest_attempts(id) ON DELETE CASCADE,
    submission_data JSONB NOT NULL,
    graded_at TIMESTAMPTZ,
    grade DECIMAL(5,2),
    max_grade DECIMAL(5,2) NOT NULL,
    feedback TEXT,
    is_passed BOOLEAN,
    attempt_number INTEGER NOT NULL DEFAULT 1,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### Analytics and Insights

##### quest_analytics
Aggregated analytics data for quest performance and engagement.
```sql
CREATE TABLE quest_analytics (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    date_recorded DATE NOT NULL,
    total_attempts INTEGER NOT NULL DEFAULT 0,
    successful_completions INTEGER NOT NULL DEFAULT 0,
    average_completion_time_minutes DECIMAL(10,2),
    average_attempts_to_complete DECIMAL(5,2),
    abandonment_rate DECIMAL(5,4),
    difficulty_rating DECIMAL(3,2),
    engagement_score DECIMAL(5,2),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (quest_id, date_recorded)
);
```

### Social Service Tables

#### Party System

##### parties
Study groups and collaborative learning parties.
```sql
CREATE TABLE parties (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    party_type party_type NOT NULL,
    max_members INTEGER NOT NULL DEFAULT 6,
    current_member_count INTEGER NOT NULL DEFAULT 1,
    is_public BOOLEAN NOT NULL DEFAULT TRUE,
    created_by UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    disbanded_at TIMESTAMPTZ
);
```

##### party_members
Party membership tracking with roles and status.
```sql
CREATE TABLE party_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    role party_role NOT NULL DEFAULT 'Member',
    status member_status NOT NULL DEFAULT 'Active',
    joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    left_at TIMESTAMPTZ,
    contribution_score INTEGER NOT NULL DEFAULT 0,
    UNIQUE (party_id, auth_user_id)
);
```

##### party_invitations
Party invitation system.
```sql
CREATE TABLE party_invitations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    inviter_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    invitee_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    status invitation_status NOT NULL DEFAULT 'Pending',
    message TEXT,
    invited_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    responded_at TIMESTAMPTZ,
    expires_at TIMESTAMPTZ NOT NULL DEFAULT (now() + INTERVAL '7 days'),
    UNIQUE (party_id, invitee_id)
);
```

##### meetings (moved earlier)
Core meeting definitions and scheduling.
```sql
CREATE TABLE meetings (
    meeting_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    scheduled_start_time TIMESTAMPTZ NOT NULL,
    scheduled_end_time TIMESTAMPTZ NOT NULL,
    actual_start_time TIMESTAMPTZ,
    actual_end_time TIMESTAMPTZ,
    organizer_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### party_activities
Tracks party collaborative activities and achievements.
```sql
CREATE TABLE party_activities (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    activity_type activity_type NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    quest_id UUID REFERENCES quests(id),
    meeting_id UUID REFERENCES meetings(meeting_id),
    started_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    completed_at TIMESTAMPTZ,
    experience_points_earned INTEGER NOT NULL DEFAULT 0,
    participants UUID[] NOT NULL,
    metadata JSONB
);
```

##### party_stash_items
Snapshot of shared notes within a party (Arsenal sharing).
```sql
CREATE TABLE party_stash_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    original_note_id UUID NOT NULL REFERENCES notes(id),
    shared_by_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    title TEXT NOT NULL,
    content JSONB NOT NULL,
    tags TEXT[],
    shared_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### Guild System

##### guilds
Large-scale learning communities and organizations.
```sql
CREATE TABLE guilds (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL UNIQUE,
    description TEXT NOT NULL,
    guild_type guild_type NOT NULL,
    max_members INTEGER NOT NULL DEFAULT 100,
    current_member_count INTEGER NOT NULL DEFAULT 1,
    level INTEGER NOT NULL DEFAULT 1,
    experience_points INTEGER NOT NULL DEFAULT 0,
    is_public BOOLEAN NOT NULL DEFAULT TRUE,
    requires_approval BOOLEAN NOT NULL DEFAULT FALSE,
    banner_image_url TEXT,
    created_by UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### guild_members
Guild membership with hierarchical roles.
```sql
CREATE TABLE guild_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    role guild_role NOT NULL DEFAULT 'Member',
    status member_status NOT NULL DEFAULT 'Active',
    joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    left_at TIMESTAMPTZ,
    contribution_points INTEGER NOT NULL DEFAULT 0,
    rank_within_guild INTEGER,
    UNIQUE (guild_id, auth_user_id)
);
```

##### guild_invitations
Guild invitation and application system.
```sql
CREATE TABLE guild_invitations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    inviter_id UUID REFERENCES user_profiles(auth_user_id),
    invitee_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    invitation_type invitation_type NOT NULL,
    status invitation_status NOT NULL DEFAULT 'Pending',
    message TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    responded_at TIMESTAMPTZ,
    expires_at TIMESTAMPTZ NOT NULL DEFAULT (now() + INTERVAL '14 days'),
    UNIQUE (guild_id, invitee_id)
);
```

#### Communication and Messaging

##### social_messages
Direct messages and party/guild communications.
```sql
CREATE TABLE social_messages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    sender_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    message_type message_type NOT NULL,
    recipient_id UUID REFERENCES user_profiles(auth_user_id),
    party_id UUID REFERENCES parties(id) ON DELETE CASCADE,
    guild_id UUID REFERENCES guilds(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    attachments JSONB,
    is_system_message BOOLEAN NOT NULL DEFAULT FALSE,
    sent_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    edited_at TIMESTAMPTZ,
    deleted_at TIMESTAMPTZ,
    CHECK (
        (message_type = 'Direct' AND recipient_id IS NOT NULL AND party_id IS NULL AND guild_id IS NULL) OR
        (message_type = 'Party' AND party_id IS NOT NULL AND recipient_id IS NULL AND guild_id IS NULL) OR
        (message_type = 'Guild' AND guild_id IS NOT NULL AND recipient_id IS NULL AND party_id IS NULL)
    )
);
```

##### message_reactions
Emoji reactions to messages.
```sql
CREATE TABLE message_reactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    message_id UUID NOT NULL REFERENCES social_messages(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    reaction_emoji VARCHAR(10) NOT NULL,
    reacted_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (message_id, auth_user_id, reaction_emoji)
);
```

### Meeting Service Tables

#### Core Meeting Management

##### meeting_participants
Meeting participation tracking with join/leave times and roles.
```sql
CREATE TABLE meeting_participants (
    participant_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(meeting_id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    join_time TIMESTAMPTZ,
    leave_time TIMESTAMPTZ,
    role_in_meeting VARCHAR(50) NOT NULL DEFAULT 'participant'
);
```

#### AI-Powered Content Processing (Future Implementation)

##### transcript_segments
Meeting transcript segments with speaker identification and timing.
```sql
CREATE TABLE transcript_segments (
    segment_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(meeting_id) ON DELETE CASCADE,
    speaker_id UUID NOT NULL REFERENCES meeting_participants(participant_id) ON DELETE CASCADE,
    start_time TIMESTAMPTZ NOT NULL,
    end_time TIMESTAMPTZ NOT NULL,
    transcript_text TEXT NOT NULL,
    chunk_number INTEGER NOT NULL,
    status transcript_segment_status NOT NULL DEFAULT 'Processed',
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### summary_chunks
AI-generated summary chunks for meeting segments.
```sql
CREATE TABLE summary_chunks (
    summary_chunk_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(meeting_id) ON DELETE CASCADE,
    chunk_number INTEGER NOT NULL,
    summary_text TEXT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

##### meeting_summaries
Complete AI-generated meeting summaries.
```sql
CREATE TABLE meeting_summaries (
    meeting_summary_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(meeting_id) ON DELETE CASCADE,
    summary_text TEXT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

## Triggers and Functions

```sql
-- Automatically updates the 'updated_at' timestamp on any row modification.
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ language 'plpgsql';

-- Apply the trigger to tables with updated_at columns
CREATE TRIGGER update_user_profiles_updated_at 
    BEFORE UPDATE ON user_profiles 
    FOR EACH ROW 
    EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_learning_paths_updated_at 
    BEFORE UPDATE ON learning_paths 
    FOR EACH ROW 
    EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_quests_updated_at 
    BEFORE UPDATE ON quests 
    FOR EACH ROW 
    EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_parties_updated_at 
    BEFORE UPDATE ON parties 
    FOR EACH ROW 
    EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_guilds_updated_at 
    BEFORE UPDATE ON guilds 
    FOR EACH ROW 
    EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_meetings_updated_at 
    BEFORE UPDATE ON meetings 
    FOR EACH ROW 
    EXECUTE FUNCTION update_updated_at_column();
```

## Indexes for Performance

### User Service Indexes

#### Core User Management
```sql
-- User profiles - primary lookups and authentication
CREATE INDEX idx_user_profiles_username ON user_profiles(username);
CREATE INDEX idx_user_profiles_email ON user_profiles(email);
CREATE INDEX idx_user_profiles_class_id ON user_profiles(class_id);
CREATE INDEX idx_user_profiles_route_id ON user_profiles(route_id);
CREATE INDEX idx_user_profiles_level ON user_profiles(level);
CREATE INDEX idx_user_profiles_created_at ON user_profiles(created_at);

-- Roles and permissions
CREATE INDEX idx_roles_name ON roles(name);
CREATE INDEX idx_user_roles_auth_user_id ON user_roles(auth_user_id);
CREATE INDEX idx_user_roles_role_id ON user_roles(role_id);
CREATE INDEX idx_user_roles_assigned_at ON user_roles(assigned_at);

-- Lecturer verification
CREATE INDEX idx_lecturer_verification_status ON lecturer_verification_requests(status);
CREATE INDEX idx_lecturer_verification_user_id ON lecturer_verification_requests(auth_user_id);
CREATE INDEX idx_lecturer_verification_submitted_at ON lecturer_verification_requests(submitted_at);
CREATE INDEX idx_lecturer_verification_reviewer_id ON lecturer_verification_requests(reviewer_id);
```

#### Academic Structure
```sql
-- Classes and specializations
CREATE INDEX idx_classes_name ON classes(name);
CREATE INDEX idx_classes_difficulty_level ON classes(difficulty_level);
CREATE INDEX idx_classes_is_active ON classes(is_active);

-- Class nodes hierarchy
CREATE INDEX idx_class_nodes_class_id ON class_nodes(class_id);
CREATE INDEX idx_class_nodes_parent_id ON class_nodes(parent_id);
CREATE INDEX idx_class_nodes_sequence ON class_nodes(class_id, sequence);
CREATE INDEX idx_class_nodes_type ON class_nodes(node_type);

-- Class specialization subjects
CREATE INDEX idx_class_spec_subjects_class_id ON class_specialization_subjects(class_id);
CREATE INDEX idx_class_spec_subjects_subject_id ON class_specialization_subjects(subject_id);
CREATE INDEX idx_class_spec_subjects_semester ON class_specialization_subjects(semester);

-- Curriculum programs
CREATE INDEX idx_curriculum_programs_code ON curriculum_programs(program_code);
CREATE INDEX idx_curriculum_programs_active ON curriculum_programs(is_active);
CREATE INDEX idx_curriculum_programs_degree_level ON curriculum_programs(degree_level);

-- Curriculum versions
CREATE INDEX idx_curriculum_versions_program_id ON curriculum_versions(program_id);
CREATE INDEX idx_curriculum_versions_effective_year ON curriculum_versions(effective_year);
CREATE INDEX idx_curriculum_versions_active ON curriculum_versions(is_active);
CREATE INDEX idx_curriculum_versions_created_by ON curriculum_versions(created_by);

-- Subjects
CREATE INDEX idx_subjects_code ON subjects(subject_code);
CREATE INDEX idx_subjects_name ON subjects(subject_name);
CREATE INDEX idx_subjects_active ON subjects(is_active);
CREATE INDEX idx_subjects_credits ON subjects(credits);

-- Curriculum structure
CREATE INDEX idx_curriculum_structure_version_id ON curriculum_structure(curriculum_version_id);
CREATE INDEX idx_curriculum_structure_subject_id ON curriculum_structure(subject_id);
CREATE INDEX idx_curriculum_structure_semester ON curriculum_structure(semester);
CREATE INDEX idx_curriculum_structure_mandatory ON curriculum_structure(is_mandatory);

-- Syllabus versions
CREATE INDEX idx_syllabus_versions_subject_id ON syllabus_versions(subject_id);
CREATE INDEX idx_syllabus_versions_active ON syllabus_versions(is_active);
CREATE INDEX idx_syllabus_versions_effective_date ON syllabus_versions(effective_date);
CREATE INDEX idx_syllabus_versions_created_by ON syllabus_versions(created_by);
```

#### Student Management
```sql
-- Curriculum activations
CREATE INDEX idx_curriculum_activations_version_id ON curriculum_version_activations(curriculum_version_id);
CREATE INDEX idx_curriculum_activations_effective_year ON curriculum_version_activations(effective_year);
CREATE INDEX idx_curriculum_activations_activated_by ON curriculum_version_activations(activated_by);

-- Student enrollments
CREATE INDEX idx_student_enrollments_user_id ON student_enrollments(auth_user_id);
CREATE INDEX idx_student_enrollments_curriculum_version_id ON student_enrollments(curriculum_version_id);
CREATE INDEX idx_student_enrollments_status ON student_enrollments(status);
CREATE INDEX idx_student_enrollments_enrollment_date ON student_enrollments(enrollment_date);
CREATE INDEX idx_student_enrollments_expected_graduation ON student_enrollments(expected_graduation_date);

-- Student semester subjects
CREATE INDEX idx_student_semester_subjects_enrollment_id ON student_semester_subjects(enrollment_id);
CREATE INDEX idx_student_semester_subjects_subject_id ON student_semester_subjects(subject_id);
CREATE INDEX idx_student_semester_subjects_academic_year ON student_semester_subjects(academic_year);
CREATE INDEX idx_student_semester_subjects_semester ON student_semester_subjects(semester);
CREATE INDEX idx_student_semester_subjects_status ON student_semester_subjects(status);
CREATE INDEX idx_student_semester_subjects_grade ON student_semester_subjects(grade);
```

#### Skills and Progress
```sql
-- Skills catalog
CREATE INDEX idx_skills_name ON skills(name);
CREATE INDEX idx_skills_domain ON skills(domain);
CREATE INDEX idx_skills_tier ON skills(tier);
CREATE INDEX idx_skills_active ON skills(is_active);

-- Skill dependencies
CREATE INDEX idx_skill_dependencies_skill_id ON skill_dependencies(skill_id);
CREATE INDEX idx_skill_dependencies_prerequisite_skill_id ON skill_dependencies(prerequisite_skill_id);
CREATE INDEX idx_skill_dependencies_relationship_type ON skill_dependencies(relationship_type);

-- User skills
CREATE INDEX idx_user_skills_user_id ON user_skills(auth_user_id);
CREATE INDEX idx_user_skills_skill_id ON user_skills(skill_id);
CREATE INDEX idx_user_skills_level ON user_skills(level);
CREATE INDEX idx_user_skills_last_updated ON user_skills(last_updated_at);

-- User quest progress
CREATE INDEX idx_user_quest_progress_user_id ON user_quest_progress(auth_user_id);
CREATE INDEX idx_user_quest_progress_quest_id ON user_quest_progress(quest_id);
CREATE INDEX idx_user_quest_progress_status ON user_quest_progress(status);
CREATE INDEX idx_user_quest_progress_last_updated ON user_quest_progress(last_updated_at);

-- User skill rewards
CREATE INDEX idx_user_skill_rewards_user_id ON user_skill_rewards(auth_user_id);
CREATE INDEX idx_user_skill_rewards_skill_id ON user_skill_rewards(skill_id);
CREATE INDEX idx_user_skill_rewards_source_service ON user_skill_rewards(source_service);
CREATE INDEX idx_user_skill_rewards_source_type ON user_skill_rewards(source_type);
CREATE INDEX idx_user_skill_rewards_created_at ON user_skill_rewards(created_at);
```

#### Notes Management (Arsenal)
```sql
-- Notes
CREATE INDEX idx_notes_user_id ON notes(auth_user_id);
CREATE INDEX idx_notes_is_public ON notes(is_public);
CREATE INDEX idx_notes_created_at ON notes(created_at);
CREATE INDEX idx_notes_updated_at ON notes(updated_at);

-- Note relationships
CREATE INDEX idx_note_quests_note_id ON note_quests(note_id);
CREATE INDEX idx_note_quests_quest_id ON note_quests(quest_id);
CREATE INDEX idx_note_skills_note_id ON note_skills(note_id);
CREATE INDEX idx_note_skills_skill_id ON note_skills(skill_id);

-- Tags
CREATE INDEX idx_tags_user_id ON tags(auth_user_id);
CREATE INDEX idx_tags_name ON tags(name);
CREATE INDEX idx_note_tags_note_id ON note_tags(note_id);
CREATE INDEX idx_note_tags_tag_id ON note_tags(tag_id);
```

#### Achievements and Notifications
```sql
-- Achievements
CREATE INDEX idx_achievements_name ON achievements(name);
CREATE INDEX idx_achievements_source_service ON achievements(source_service);
CREATE INDEX idx_achievements_category ON achievements(category);
CREATE INDEX idx_achievements_active ON achievements(is_active);

-- User achievements
CREATE INDEX idx_user_achievements_user_id ON user_achievements(auth_user_id);
CREATE INDEX idx_user_achievements_achievement_id ON user_achievements(achievement_id);
CREATE INDEX idx_user_achievements_earned_at ON user_achievements(earned_at);

-- Notifications
CREATE INDEX idx_notifications_user_id ON notifications(auth_user_id);
CREATE INDEX idx_notifications_type ON notifications(type);
CREATE INDEX idx_notifications_is_read ON notifications(is_read);
CREATE INDEX idx_notifications_created_at ON notifications(created_at);
```

### Quests Service Indexes

#### Learning Paths and Curriculum Integration
```sql
-- Learning paths
CREATE INDEX idx_learning_paths_name ON learning_paths(name);
CREATE INDEX idx_learning_paths_path_type ON learning_paths(path_type);
CREATE INDEX idx_learning_paths_curriculum_version_id ON learning_paths(curriculum_version_id);
CREATE INDEX idx_learning_paths_is_published ON learning_paths(is_published);
CREATE INDEX idx_learning_paths_created_by ON learning_paths(created_by);
CREATE INDEX idx_learning_paths_created_at ON learning_paths(created_at);

-- Quest chapters
CREATE INDEX idx_quest_chapters_learning_path_id ON quest_chapters(learning_path_id);
CREATE INDEX idx_quest_chapters_sequence ON quest_chapters(learning_path_id, sequence);
CREATE INDEX idx_quest_chapters_status ON quest_chapters(status);

-- Learning path quests
CREATE INDEX idx_learning_path_quests_path_id ON learning_path_quests(learning_path_id);
CREATE INDEX idx_learning_path_quests_quest_id ON learning_path_quests(quest_id);
CREATE INDEX idx_learning_path_quests_sequence ON learning_path_quests(learning_path_id, sequence_order);
CREATE INDEX idx_learning_path_quests_difficulty ON learning_path_quests(difficulty_level);
CREATE INDEX idx_learning_path_quests_mandatory ON learning_path_quests(is_mandatory);
```

#### Quest Management
```sql
-- Quests
CREATE INDEX idx_quests_title ON quests(title);
CREATE INDEX idx_quests_type ON quests(quest_type);
CREATE INDEX idx_quests_difficulty ON quests(difficulty_level);
CREATE INDEX idx_quests_subject_id ON quests(subject_id);
CREATE INDEX idx_quests_active ON quests(is_active);
CREATE INDEX idx_quests_created_by ON quests(created_by);
CREATE INDEX idx_quests_created_at ON quests(created_at);
CREATE INDEX idx_quests_skill_tags ON quests USING GIN(skill_tags);

-- Quest prerequisites
CREATE INDEX idx_quest_prerequisites_quest_id ON quest_prerequisites(quest_id);
CREATE INDEX idx_quest_prerequisites_prerequisite_id ON quest_prerequisites(prerequisite_quest_id);

-- Quest steps
CREATE INDEX idx_quest_steps_quest_id ON quest_steps(quest_id);
CREATE INDEX idx_quest_steps_step_number ON quest_steps(quest_id, step_number);
CREATE INDEX idx_quest_steps_type ON quest_steps(step_type);
CREATE INDEX idx_quest_steps_optional ON quest_steps(is_optional);

-- Quest resources
CREATE INDEX idx_quest_resources_quest_id ON quest_resources(quest_id);
CREATE INDEX idx_quest_resources_type ON quest_resources(resource_type);
CREATE INDEX idx_quest_resources_display_order ON quest_resources(quest_id, display_order);
```

#### User Progress Tracking
```sql
-- User quest attempts
CREATE INDEX idx_user_quest_attempts_user_id ON user_quest_attempts(auth_user_id);
CREATE INDEX idx_user_quest_attempts_quest_id ON user_quest_attempts(quest_id);
CREATE INDEX idx_user_quest_attempts_status ON user_quest_attempts(status);
CREATE INDEX idx_user_quest_attempts_started_at ON user_quest_attempts(started_at);
CREATE INDEX idx_user_quest_attempts_completed_at ON user_quest_attempts(completed_at);
CREATE INDEX idx_user_quest_attempts_current_step ON user_quest_attempts(current_step_id);

-- User quest step progress
CREATE INDEX idx_user_quest_step_progress_attempt_id ON user_quest_step_progress(attempt_id);
CREATE INDEX idx_user_quest_step_progress_step_id ON user_quest_step_progress(step_id);
CREATE INDEX idx_user_quest_step_progress_status ON user_quest_step_progress(status);
CREATE INDEX idx_user_quest_step_progress_completed_at ON user_quest_step_progress(completed_at);

-- User learning path progress
CREATE INDEX idx_user_learning_path_progress_user_id ON user_learning_path_progress(auth_user_id);
CREATE INDEX idx_user_learning_path_progress_path_id ON user_learning_path_progress(learning_path_id);
CREATE INDEX idx_user_learning_path_progress_status ON user_learning_path_progress(status);
CREATE INDEX idx_user_learning_path_progress_current_quest ON user_learning_path_progress(current_quest_id);
CREATE INDEX idx_user_learning_path_progress_started_at ON user_learning_path_progress(started_at);
CREATE INDEX idx_user_learning_path_progress_completed_at ON user_learning_path_progress(completed_at);
```

#### Assessment and Analytics
```sql
-- Quest assessments
CREATE INDEX idx_quest_assessments_quest_id ON quest_assessments(quest_id);
CREATE INDEX idx_quest_assessments_type ON quest_assessments(assessment_type);

-- Quest submissions
CREATE INDEX idx_quest_submissions_attempt_id ON quest_submissions(attempt_id);
CREATE INDEX idx_quest_submissions_graded_at ON quest_submissions(graded_at);
CREATE INDEX idx_quest_submissions_is_passed ON quest_submissions(is_passed);
CREATE INDEX idx_quest_submissions_attempt_number ON quest_submissions(attempt_number);

-- Quest analytics
CREATE INDEX idx_quest_analytics_quest_id ON quest_analytics(quest_id);
CREATE INDEX idx_quest_analytics_date_recorded ON quest_analytics(date_recorded);
CREATE INDEX idx_quest_analytics_completion_rate ON quest_analytics(quest_id, date_recorded);
```

### Social Service Indexes

#### Party System
```sql
-- Parties
CREATE INDEX idx_parties_name ON parties(name);
CREATE INDEX idx_parties_type ON parties(party_type);
CREATE INDEX idx_parties_is_public ON parties(is_public);
CREATE INDEX idx_parties_created_by ON parties(created_by);
CREATE INDEX idx_parties_created_at ON parties(created_at);
CREATE INDEX idx_parties_disbanded_at ON parties(disbanded_at);
CREATE INDEX idx_parties_active ON parties(disbanded_at) WHERE disbanded_at IS NULL;

-- Party members
CREATE INDEX idx_party_members_party_id ON party_members(party_id);
CREATE INDEX idx_party_members_user_id ON party_members(auth_user_id);
CREATE INDEX idx_party_members_role ON party_members(role);
CREATE INDEX idx_party_members_status ON party_members(status);
CREATE INDEX idx_party_members_joined_at ON party_members(joined_at);
CREATE INDEX idx_party_members_contribution ON party_members(contribution_score);
CREATE INDEX idx_party_members_active ON party_members(party_id, status) WHERE status = 'Active';

-- Party invitations
CREATE INDEX idx_party_invitations_party_id ON party_invitations(party_id);
CREATE INDEX idx_party_invitations_inviter_id ON party_invitations(inviter_id);
CREATE INDEX idx_party_invitations_invitee_id ON party_invitations(invitee_id);
CREATE INDEX idx_party_invitations_status ON party_invitations(status);
CREATE INDEX idx_party_invitations_invited_at ON party_invitations(invited_at);
CREATE INDEX idx_party_invitations_expires_at ON party_invitations(expires_at);
CREATE INDEX idx_party_invitations_pending ON party_invitations(invitee_id, status) WHERE status = 'Pending';

-- Party activities
CREATE INDEX idx_party_activities_party_id ON party_activities(party_id);
CREATE INDEX idx_party_activities_type ON party_activities(activity_type);
CREATE INDEX idx_party_activities_quest_id ON party_activities(quest_id);
CREATE INDEX idx_party_activities_meeting_id ON party_activities(meeting_id);
CREATE INDEX idx_party_activities_started_at ON party_activities(started_at);
CREATE INDEX idx_party_activities_completed_at ON party_activities(completed_at);
CREATE INDEX idx_party_activities_participants ON party_activities USING GIN(participants);

-- Party stash items
CREATE INDEX idx_party_stash_items_party_id ON party_stash_items(party_id);
CREATE INDEX idx_party_stash_items_original_note_id ON party_stash_items(original_note_id);
CREATE INDEX idx_party_stash_items_shared_by ON party_stash_items(shared_by_user_id);
CREATE INDEX idx_party_stash_items_shared_at ON party_stash_items(shared_at);
CREATE INDEX idx_party_stash_items_tags ON party_stash_items USING GIN(tags);
```

#### Guild System
```sql
-- Guilds
CREATE INDEX idx_guilds_name ON guilds(name);
CREATE INDEX idx_guilds_type ON guilds(guild_type);
CREATE INDEX idx_guilds_is_public ON guilds(is_public);
CREATE INDEX idx_guilds_requires_approval ON guilds(requires_approval);
CREATE INDEX idx_guilds_level ON guilds(level);
CREATE INDEX idx_guilds_experience_points ON guilds(experience_points);
CREATE INDEX idx_guilds_created_by ON guilds(created_by);
CREATE INDEX idx_guilds_created_at ON guilds(created_at);

-- Guild members
CREATE INDEX idx_guild_members_guild_id ON guild_members(guild_id);
CREATE INDEX idx_guild_members_user_id ON guild_members(auth_user_id);
CREATE INDEX idx_guild_members_role ON guild_members(role);
CREATE INDEX idx_guild_members_status ON guild_members(status);
CREATE INDEX idx_guild_members_joined_at ON guild_members(joined_at);
CREATE INDEX idx_guild_members_contribution ON guild_members(contribution_points);
CREATE INDEX idx_guild_members_rank ON guild_members(guild_id, rank_within_guild);
CREATE INDEX idx_guild_members_active ON guild_members(guild_id, status) WHERE status = 'Active';

-- Guild invitations
CREATE INDEX idx_guild_invitations_guild_id ON guild_invitations(guild_id);
CREATE INDEX idx_guild_invitations_inviter_id ON guild_invitations(inviter_id);
CREATE INDEX idx_guild_invitations_invitee_id ON guild_invitations(invitee_id);
CREATE INDEX idx_guild_invitations_type ON guild_invitations(invitation_type);
CREATE INDEX idx_guild_invitations_status ON guild_invitations(status);
CREATE INDEX idx_guild_invitations_created_at ON guild_invitations(created_at);
CREATE INDEX idx_guild_invitations_expires_at ON guild_invitations(expires_at);
CREATE INDEX idx_guild_invitations_pending ON guild_invitations(invitee_id, status) WHERE status = 'Pending';
```

#### Social Interactions and Communication
```sql
-- Friendships
CREATE INDEX idx_friendships_requester_id ON friendships(requester_id);
CREATE INDEX idx_friendships_addressee_id ON friendships(addressee_id);
CREATE INDEX idx_friendships_status ON friendships(status);
CREATE INDEX idx_friendships_requested_at ON friendships(requested_at);
CREATE INDEX idx_friendships_accepted_at ON friendships(accepted_at);
CREATE INDEX idx_friendships_user_friends ON friendships(requester_id, status) WHERE status = 'Accepted';
CREATE INDEX idx_friendships_pending_requests ON friendships(addressee_id, status) WHERE status = 'Pending';

-- Social messages
CREATE INDEX idx_social_messages_sender_id ON social_messages(sender_id);
CREATE INDEX idx_social_messages_recipient_id ON social_messages(recipient_id);
CREATE INDEX idx_social_messages_party_id ON social_messages(party_id);
CREATE INDEX idx_social_messages_guild_id ON social_messages(guild_id);
CREATE INDEX idx_social_messages_type ON social_messages(message_type);
CREATE INDEX idx_social_messages_sent_at ON social_messages(sent_at);
CREATE INDEX idx_social_messages_is_system ON social_messages(is_system_message);
CREATE INDEX idx_social_messages_recent_direct ON social_messages(recipient_id, sent_at DESC) WHERE message_type = 'Direct' AND deleted_at IS NULL;
CREATE INDEX idx_social_messages_recent_party ON social_messages(party_id, sent_at DESC) WHERE message_type = 'Party' AND deleted_at IS NULL;
CREATE INDEX idx_social_messages_recent_guild ON social_messages(guild_id, sent_at DESC) WHERE message_type = 'Guild' AND deleted_at IS NULL;

-- Message reactions
CREATE INDEX idx_message_reactions_message_id ON message_reactions(message_id);
CREATE INDEX idx_message_reactions_user_id ON message_reactions(auth_user_id);
CREATE INDEX idx_message_reactions_emoji ON message_reactions(reaction_emoji);
CREATE INDEX idx_message_reactions_reacted_at ON message_reactions(reacted_at);
```

### Meeting Service Indexes

#### Core Meeting Management
```sql
-- Meetings
CREATE INDEX idx_meetings_party_id ON meetings(party_id);
CREATE INDEX idx_meetings_organizer_id ON meetings(organizer_id);
CREATE INDEX idx_meetings_scheduled_start_time ON meetings(scheduled_start_time);
CREATE INDEX idx_meetings_scheduled_end_time ON meetings(scheduled_end_time);
CREATE INDEX idx_meetings_actual_start_time ON meetings(actual_start_time);
CREATE INDEX idx_meetings_actual_end_time ON meetings(actual_end_time);
CREATE INDEX idx_meetings_created_at ON meetings(created_at);

-- Meeting participants
CREATE INDEX idx_meeting_participants_meeting_id ON meeting_participants(meeting_id);
CREATE INDEX idx_meeting_participants_user_id ON meeting_participants(user_id);
CREATE INDEX idx_meeting_participants_join_time ON meeting_participants(join_time);
CREATE INDEX idx_meeting_participants_leave_time ON meeting_participants(leave_time);
CREATE INDEX idx_meeting_participants_role ON meeting_participants(role_in_meeting);
```

#### AI-Powered Content Processing
```sql
-- Transcript segments
CREATE INDEX idx_transcript_segments_meeting_id ON transcript_segments(meeting_id);
CREATE INDEX idx_transcript_segments_speaker_id ON transcript_segments(speaker_id);
CREATE INDEX idx_transcript_segments_start_time ON transcript_segments(start_time);
CREATE INDEX idx_transcript_segments_end_time ON transcript_segments(end_time);
CREATE INDEX idx_transcript_segments_chunk_number ON transcript_segments(meeting_id, chunk_number);
CREATE INDEX idx_transcript_segments_status ON transcript_segments(status);

-- Summary chunks
CREATE INDEX idx_summary_chunks_meeting_id ON summary_chunks(meeting_id);
CREATE INDEX idx_summary_chunks_chunk_number ON summary_chunks(meeting_id, chunk_number);
CREATE INDEX idx_summary_chunks_created_at ON summary_chunks(created_at);

-- Meeting summaries
CREATE INDEX idx_meeting_summaries_meeting_id ON meeting_summaries(meeting_id);
CREATE INDEX idx_meeting_summaries_created_at ON meeting_summaries(created_at);
```

### Composite Indexes for Cross-Service Queries

#### User Activity and Progress
```sql
-- User comprehensive activity tracking
CREATE INDEX idx_user_activity_comprehensive ON user_profiles(auth_user_id, level, total_experience_points, created_at);
CREATE INDEX idx_user_quest_skill_progress ON user_quest_progress(auth_user_id, status, last_updated_at);
CREATE INDEX idx_user_social_activity ON party_members(auth_user_id, status, joined_at, contribution_score);

-- Cross-service user engagement
CREATE INDEX idx_user_engagement_quests ON user_quest_attempts(auth_user_id, status, started_at, total_experience_earned);
CREATE INDEX idx_user_engagement_social ON guild_members(auth_user_id, status, joined_at, contribution_points);
CREATE INDEX idx_user_engagement_meetings ON meeting_participants(user_id, join_time, leave_time);
```

#### Performance Optimization
```sql
-- Recent activity queries
CREATE INDEX idx_recent_quest_completions ON user_quest_attempts(completed_at DESC, status) WHERE status = 'Completed';
CREATE INDEX idx_recent_party_activities ON party_activities(started_at DESC, activity_type);
CREATE INDEX idx_recent_social_messages ON social_messages(sent_at DESC, message_type) WHERE deleted_at IS NULL;
CREATE INDEX idx_recent_achievements ON user_achievements(earned_at DESC);

-- Leaderboard and ranking queries
CREATE INDEX idx_user_level_ranking ON user_profiles(level DESC, total_experience_points DESC);
CREATE INDEX idx_guild_ranking ON guilds(level DESC, experience_points DESC);
CREATE INDEX idx_party_contribution_ranking ON party_members(party_id, contribution_score DESC) WHERE status = 'Active';
CREATE INDEX idx_skill_level_ranking ON user_skills(skill_id, level DESC, experience_points DESC);
```

## Service Responsibilities and Cross-Service Integration

### Merged Database Benefits

This unified database schema consolidates four previously separate services (User, Quests, Social, and Meeting) into a single, cohesive data layer that provides:

1. **Simplified Data Management**: Single database instance reduces operational complexity
2. **Enhanced Performance**: Eliminates cross-service database queries and joins
3. **ACID Compliance**: Full transactional integrity across all business operations
4. **Reduced Latency**: Direct table joins instead of service-to-service API calls
5. **Simplified Backup and Recovery**: Single database backup strategy

### Primary Service Responsibilities

#### User Management Domain
- **Authentication & Authorization**: User profiles, roles, and permissions
- **Academic Structure**: Classes, curriculum programs, subjects, and academic hierarchy
- **Student Lifecycle**: Enrollments, semester subjects, and academic progress
- **Skills System**: Skill definitions, dependencies, and user skill tracking
- **Arsenal (Notes)**: Personal knowledge management and note-taking
- **Achievements**: Recognition system for user accomplishments
- **Notifications**: System-wide notification management

#### Quest Management Domain
- **Learning Path Design**: Structured learning journeys and curriculum integration
- **Quest Creation**: Interactive learning content with steps and resources
- **Progress Tracking**: User attempts, step completion, and learning path progress
- **Assessment System**: Quest evaluations and submission management
- **Analytics**: Quest performance metrics and completion analytics

#### Social Interaction Domain
- **Party System**: Small group collaboration and shared activities
- **Guild System**: Large community management and hierarchical structures
- **Friendship Management**: Direct user-to-user social connections
- **Communication**: Messaging system across parties, guilds, and direct messages
- **Social Engagement**: Reactions, interactions, and community features

#### Meeting Management Domain
- **Meeting Coordination**: Scheduling and participant management
- **AI-Powered Processing**: Transcript generation and content analysis
- **Summary Generation**: Automated meeting summaries and insights
- **Collaboration Tools**: Meeting-based learning and discussion features

### Cross-Service Data Integration Patterns

#### User-Centric Integration
```sql
-- Example: Get comprehensive user profile with all activities
SELECT 
    up.username, up.level, up.total_experience_points,
    COUNT(DISTINCT uqa.id) as completed_quests,
    COUNT(DISTINCT pm.id) as party_memberships,
    COUNT(DISTINCT gm.id) as guild_memberships,
    COUNT(DISTINCT mp.id) as meeting_participations
FROM user_profiles up
LEFT JOIN user_quest_attempts uqa ON up.auth_user_id = uqa.auth_user_id 
    AND uqa.status = 'Completed'
LEFT JOIN party_members pm ON up.auth_user_id = pm.auth_user_id 
    AND pm.status = 'Active'
LEFT JOIN guild_members gm ON up.auth_user_id = gm.auth_user_id 
    AND gm.status = 'Active'
LEFT JOIN meeting_participants mp ON up.auth_user_id = mp.user_id
WHERE up.auth_user_id = $1
GROUP BY up.auth_user_id, up.username, up.level, up.total_experience_points;
```

#### Activity-Based Integration
```sql
-- Example: Get party activities with quest and meeting details
SELECT 
    pa.activity_type,
    pa.started_at,
    pa.completed_at,
    q.title as quest_title,
    m.title as meeting_title,
    array_agg(up.username) as participants
FROM party_activities pa
LEFT JOIN quests q ON pa.quest_id = q.id
LEFT JOIN meetings m ON pa.meeting_id = m.id
JOIN user_profiles up ON up.auth_user_id = ANY(pa.participants)
WHERE pa.party_id = $1
GROUP BY pa.id, pa.activity_type, pa.started_at, pa.completed_at, q.title, m.title
ORDER BY pa.started_at DESC;
```

### Data Access Patterns

#### High-Frequency Queries
- User authentication and profile lookups
- Quest progress tracking and updates
- Social message retrieval and posting
- Real-time notification delivery
- Meeting participant management

#### Analytical Queries
- User engagement metrics across all services
- Quest completion rates and learning analytics
- Social interaction patterns and community health
- Meeting effectiveness and participation trends

#### Batch Operations
- Daily/weekly progress summaries
- Achievement calculation and awarding
- Skill level updates based on quest completions
- Community ranking and leaderboard updates

### Real-Time Features Support

The merged schema supports real-time features through:

1. **Optimized Indexes**: Comprehensive indexing strategy for fast lookups
2. **Efficient Joins**: Direct table relationships eliminate service boundaries
3. **Trigger-Based Updates**: Automatic timestamp and status management
4. **Composite Queries**: Single queries spanning multiple domains

### Migration and Deployment Considerations

#### From Microservices to Monolithic Database
1. **Data Migration**: Systematic migration of data from four separate databases
2. **Foreign Key Establishment**: Creation of cross-service relationships
3. **Index Optimization**: Comprehensive index strategy for merged queries
4. **Application Layer Updates**: Service layer modifications to use single database

#### Performance Monitoring
- Query performance across merged domains
- Index utilization and optimization
- Connection pooling and resource management
- Backup and recovery procedures for larger dataset

### Future Extensibility

The merged schema maintains extensibility through:
- **Modular Table Design**: Clear domain separation within single database
- **Flexible Relationships**: Support for future cross-domain features
- **Scalable Indexing**: Index strategy that supports growth
- **Service Boundary Preservation**: Logical separation maintained for potential future splitting

This unified database architecture provides a solid foundation for the RogueLearn platform while maintaining the flexibility to evolve with changing requirements and scale with user growth.