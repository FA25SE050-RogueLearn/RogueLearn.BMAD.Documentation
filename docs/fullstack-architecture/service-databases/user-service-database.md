# User Service Database Schema

## Overview
The User Service manages user profiles, authentication, academic structure, achievements, and user progress tracking. This service maintains the core user data and academic framework for the RogueLearn platform.

## Enums and Types

-- Verification status for lecturer requests
CREATE TYPE verification_status AS ENUM ('Pending', 'Approved', 'Rejected');

-- Classes difficulty level
CREATE TYPE difficulty_level AS ENUM ('Beginner', 'Intermediate', 'Advanced');

-- Skill tier level
CREATE TYPE skill_tier_level AS ENUM ('Foundation', 'Intermediate', 'Advanced', 'Expert');

-- Skill dependencies relationship type
CREATE TYPE skill_dependency_type AS ENUM ('Prerequisite', 'Complements', 'Alternative');

-- User skill reward source type
CREATE TYPE skill_reward_source_type AS ENUM ('Class', 'Quest', 'Event', 'Other');

-- Degree levels
CREATE TYPE degree_level AS ENUM ('Associate', 'Bachelor', 'Master', 'Doctorate');

-- Enrollment status
CREATE TYPE enrollment_status AS ENUM ('Active', 'Graduated', 'Withdrawn', 'Suspended');

-- Subject enrollment status
CREATE TYPE subject_enrollment_status AS ENUM ('Enrolled', 'Completed', 'Failed', 'Withdrawn');

-- Quest status
CREATE TYPE quest_status AS ENUM ('NotStarted', 'InProgress', 'Completed', 'Abandoned');

-- Notification types
CREATE TYPE notification_type AS ENUM ('Achievement', 'QuestComplete', 'PartyInvite', 'GuildInvite', 'MeetingReminder', 'System');

## Database Tables

### Core User Management

#### roles
System roles for RBAC
```sql
CREATE TABLE roles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL UNIQUE,
    description TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### classes
Roadmap.sh specializations (Step 2 of Character Creation flow)
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

#### class_nodes
Hierarchical structure nodes for a class/roadmap (modules, topics, checkpoints)
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

#### class_specialization_subjects
Maps a roadmap class specialization to concrete curriculum subjects that fulfill placeholders
```sql
CREATE TABLE class_specialization_subjects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    class_id UUID NOT NULL REFERENCES classes(id) ON DELETE CASCADE,
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    placeholder_subject_code TEXT NOT NULL,
    semester INTEGER NOT NULL
);
```

#### curriculum_programs
Abstract program definitions (e.g., "B.S. in Software Engineering")
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

#### user_profiles
Core user profile data linked to Supabase auth.users
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

#### user_roles
User role assignments
```sql
CREATE TABLE user_roles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    role_id UUID NOT NULL REFERENCES roles(id) ON DELETE CASCADE,
    assigned_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (auth_user_id, role_id)
);
```

#### lecturer_verification_requests
Lecturer verification workflow
```sql
CREATE TABLE lecturer_verification_requests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    institution_name VARCHAR(255) NOT NULL,
    department VARCHAR(255),
    verification_document_url TEXT,
    status verification_status NOT NULL DEFAULT 'Pending',
    submitted_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    reviewed_at TIMESTAMPTZ,
    reviewer_id UUID REFERENCES user_profiles(auth_user_id),
    notes TEXT
);
```

### Academic Structure

#### curriculum_versions
Versioned curriculum snapshots (e.g., "K18A", "K18B") with effective year tracking
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

#### subjects
Master list of all subjects with codes and credits
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

#### curriculum_structure
Junction table defining curriculum-subject relationships with semester sequencing
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

#### syllabus_versions
Versioned syllabus for each subject
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

### Admin-Owned Educational Governance

#### curriculum_version_activations
Activation plan and audit for curriculum versions
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

#### student_enrollments
Links students to specific curriculum versions with enrollment tracking
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

#### student_semester_subjects
Comprehensive academic semester tracking with status
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

### Skill Catalog

#### skills
Core skill catalog used to build the system-wide Skill Tree
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

#### skill_dependencies
Relationships among skills to form the Skill Tree graph
```sql
CREATE TABLE skill_dependencies (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    prerequisite_skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    relationship_type skill_dependency_type NOT NULL DEFAULT 'Prerequisite', -- Prerequisite, Complements, Alternative
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),  
    UNIQUE (skill_id, prerequisite_skill_id, relationship_type)
);
```

### Notes Management (Arsenal)

#### notes
User-owned notes that form the personal Arsenal. Content can be structured (JSONB) and optionally public.
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

#### note_quests
Links notes to quests (many-to-many). quest_id is a soft reference to Quests Service.
```sql
CREATE TABLE note_quests (
    note_id UUID NOT NULL REFERENCES notes(id) ON DELETE CASCADE,
    quest_id UUID NOT NULL, -- Soft reference to quests.id in Quests Service
    PRIMARY KEY (note_id, quest_id)
);
```

#### note_skills
Links notes to skills (many-to-many).
```sql
CREATE TABLE note_skills (
    note_id UUID NOT NULL REFERENCES notes(id) ON DELETE CASCADE,
    skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    PRIMARY KEY (note_id, skill_id)
);
```

#### tags
User-defined tags for flexible note organization.
```sql
CREATE TABLE tags (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    UNIQUE (auth_user_id, name)
);
```

#### note_tags
Join table for notes and tags (many-to-many).
```sql
CREATE TABLE note_tags (
    note_id UUID NOT NULL REFERENCES notes(id) ON DELETE CASCADE,
    tag_id UUID NOT NULL REFERENCES tags(id) ON DELETE CASCADE,
    PRIMARY KEY (note_id, tag_id)
);
```

#### Notes Model Usage
- When creating a note, the client can attach one or more quests and skills.
- Browsing the Arsenal can be filtered by quest or skill via the join tables.
- From a quest view, query related notes via `note_quests.quest_id = :questId`.
- Tags provide personal categorization that complements structured links to quests and skills.

### Skills and Progress Tracking

#### user_skills
User skill progression tracking with experience points
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

#### user_quest_progress
Summary of user progress on quests for quick reference and cross-service sync
```sql
CREATE TABLE user_quest_progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    quest_id UUID NOT NULL, -- This references quests.id in the Quests Service. No FK constraint.
    status quest_status NOT NULL,
    completed_at TIMESTAMPTZ,
    last_updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (auth_user_id, quest_id)
);
```

#### user_skill_rewards
Event-sourced ledger of XP rewards applied to user skills
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

### Achievements System

#### achievements
Central catalog of all possible achievements with type categorization
```sql
CREATE TABLE achievements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL UNIQUE,
    description TEXT NOT NULL,
    icon_url TEXT,
    source_service VARCHAR(255) NOT NULL -- e.g., 'QuestsService', 'CodeBattleService'
    key TEXT,
    rule_type TEXT,
    rule_config JSONB,
    category TEXT,
    version INTEGER,
    is_active BOOLEAN NOT NULL DEFAULT true,
);
```

#### user_achievements
Links users to earned achievements with context and timestamps
```sql
CREATE TABLE user_achievements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    achievement_id UUID NOT NULL REFERENCES achievements(id) ON DELETE CASCADE,
    earned_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    context JSONB -- To store additional details, like the event or quest that triggered the achievement
);
```

### Notifications

#### notifications
User notification system with type-based categorization
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

## Indexes for Performance

```sql
-- Core user management indexes
CREATE INDEX idx_roles_name ON roles(name);

-- User profile indexes
CREATE INDEX idx_user_profiles_username ON user_profiles(username);
CREATE INDEX idx_user_profiles_email ON user_profiles(email);
CREATE INDEX idx_user_profiles_class_id ON user_profiles(class_id);
CREATE INDEX idx_user_profiles_route_id ON user_profiles(route_id);
CREATE INDEX idx_user_profiles_level ON user_profiles(level);
CREATE INDEX idx_user_profiles_onboarding_completed ON user_profiles(onboarding_completed);

-- User roles indexes
CREATE INDEX idx_user_roles_auth_user_id ON user_roles(auth_user_id);
CREATE INDEX idx_user_roles_role_id ON user_roles(role_id);

-- Lecturer verification indexes
CREATE INDEX idx_lecturer_verification_requests_auth_user_id ON lecturer_verification_requests(auth_user_id);
CREATE INDEX idx_lecturer_verification_requests_status ON lecturer_verification_requests(status);
CREATE INDEX idx_lecturer_verification_requests_reviewer_id ON lecturer_verification_requests(reviewer_id);
CREATE INDEX idx_lecturer_verification_requests_submitted_at ON lecturer_verification_requests(submitted_at);

-- Roadmap.sh specialization indexes
CREATE INDEX idx_classes_name ON classes(name);
CREATE INDEX idx_classes_is_active ON classes(is_active);
CREATE INDEX idx_classes_difficulty_level ON classes(difficulty_level);

-- Class hierarchy and mapping indexes
CREATE INDEX idx_class_nodes_class_id ON class_nodes(class_id);
CREATE INDEX idx_class_nodes_parent_id ON class_nodes(parent_id);
CREATE INDEX idx_class_nodes_sequence ON class_nodes(sequence);
CREATE INDEX idx_class_nodes_is_active ON class_nodes(is_active);
CREATE INDEX idx_class_specialization_subjects_class_id ON class_specialization_subjects(class_id);
CREATE INDEX idx_class_specialization_subjects_subject_id ON class_specialization_subjects(subject_id);
CREATE INDEX idx_class_specialization_subjects_semester ON class_specialization_subjects(semester);

-- Curriculum program indexes
CREATE INDEX idx_curriculum_programs_program_code ON curriculum_programs(program_code);
CREATE INDEX idx_curriculum_programs_degree_level ON curriculum_programs(degree_level);

-- Academic structure indexes
CREATE INDEX idx_curriculum_versions_program_id ON curriculum_versions(program_id);
CREATE INDEX idx_curriculum_versions_effective_year ON curriculum_versions(effective_year);
CREATE INDEX idx_curriculum_versions_is_active ON curriculum_versions(is_active);

CREATE INDEX idx_subjects_subject_code ON subjects(subject_code);
CREATE INDEX idx_subjects_subject_name ON subjects(subject_name);

CREATE INDEX idx_curriculum_structure_curriculum_version_id ON curriculum_structure(curriculum_version_id);
CREATE INDEX idx_curriculum_structure_subject_id ON curriculum_structure(subject_id);
CREATE INDEX idx_curriculum_structure_semester ON curriculum_structure(semester);
CREATE INDEX idx_curriculum_structure_is_mandatory ON curriculum_structure(is_mandatory);

CREATE INDEX idx_syllabus_versions_subject_id ON syllabus_versions(subject_id);
CREATE INDEX idx_syllabus_versions_is_active ON syllabus_versions(is_active);
CREATE INDEX idx_syllabus_versions_effective_date ON syllabus_versions(effective_date);
CREATE INDEX idx_syllabus_versions_created_by ON syllabus_versions(created_by);

-- Admin governance indexes
CREATE INDEX idx_curriculum_version_activations_curriculum_version_id ON curriculum_version_activations(curriculum_version_id);
CREATE INDEX idx_curriculum_version_activations_effective_year ON curriculum_version_activations(effective_year);
CREATE INDEX idx_curriculum_version_activations_activated_by ON curriculum_version_activations(activated_by);

-- Student enrollment indexes
CREATE INDEX idx_student_enrollments_auth_user_id ON student_enrollments(auth_user_id);
CREATE INDEX idx_student_enrollments_curriculum_version_id ON student_enrollments(curriculum_version_id);
CREATE INDEX idx_student_enrollments_status ON student_enrollments(status);
CREATE INDEX idx_student_enrollments_enrollment_date ON student_enrollments(enrollment_date);

CREATE INDEX idx_student_semester_subjects_enrollment_id ON student_semester_subjects(enrollment_id);
CREATE INDEX idx_student_semester_subjects_subject_id ON student_semester_subjects(subject_id);
CREATE INDEX idx_student_semester_subjects_academic_year ON student_semester_subjects(academic_year);
CREATE INDEX idx_student_semester_subjects_semester ON student_semester_subjects(semester);
CREATE INDEX idx_student_semester_subjects_status ON student_semester_subjects(status);

-- Skills catalog indexes
CREATE INDEX idx_skills_name ON skills(name);
CREATE INDEX idx_skills_domain ON skills(domain);
CREATE INDEX idx_skills_tier ON skills(tier);

CREATE INDEX idx_skill_dependencies_skill_id ON skill_dependencies(skill_id);
CREATE INDEX idx_skill_dependencies_prerequisite_skill_id ON skill_dependencies(prerequisite_skill_id);
CREATE INDEX idx_skill_dependencies_relationship_type ON skill_dependencies(relationship_type);

-- Notes/Arsenal indexes
CREATE INDEX idx_notes_auth_user_id ON notes(auth_user_id);
CREATE INDEX idx_notes_is_public ON notes(is_public);
CREATE INDEX idx_notes_created_at ON notes(created_at);
CREATE INDEX idx_notes_updated_at ON notes(updated_at);

CREATE INDEX idx_note_quests_note_id ON note_quests(note_id);
CREATE INDEX idx_note_quests_quest_id ON note_quests(quest_id);

CREATE INDEX idx_note_skills_note_id ON note_skills(note_id);
CREATE INDEX idx_note_skills_skill_id ON note_skills(skill_id);

CREATE INDEX idx_tags_auth_user_id ON tags(auth_user_id);
CREATE INDEX idx_tags_name ON tags(name);

CREATE INDEX idx_note_tags_note_id ON note_tags(note_id);
CREATE INDEX idx_note_tags_tag_id ON note_tags(tag_id);

-- Skills and progress tracking indexes
CREATE INDEX idx_user_skills_auth_user_id ON user_skills(auth_user_id);
CREATE INDEX idx_user_skills_skill_id ON user_skills(skill_id);
CREATE INDEX idx_user_skills_skill_name ON user_skills(skill_name);
CREATE INDEX idx_user_skills_level ON user_skills(level);
CREATE INDEX idx_user_skills_last_updated_at ON user_skills(last_updated_at);

CREATE INDEX idx_user_quest_progress_auth_user_id ON user_quest_progress(auth_user_id);
CREATE INDEX idx_user_quest_progress_quest_id ON user_quest_progress(quest_id);
CREATE INDEX idx_user_quest_progress_status ON user_quest_progress(status);
CREATE INDEX idx_user_quest_progress_last_updated_at ON user_quest_progress(last_updated_at);

CREATE INDEX idx_user_skill_rewards_auth_user_id ON user_skill_rewards(auth_user_id);
CREATE INDEX idx_user_skill_rewards_skill_id ON user_skill_rewards(skill_id);
CREATE INDEX idx_user_skill_rewards_skill_name ON user_skill_rewards(skill_name);
CREATE INDEX idx_user_skill_rewards_source_service ON user_skill_rewards(source_service);
CREATE INDEX idx_user_skill_rewards_source_type ON user_skill_rewards(source_type);
CREATE INDEX idx_user_skill_rewards_source_id ON user_skill_rewards(source_id);
CREATE INDEX idx_user_skill_rewards_created_at ON user_skill_rewards(created_at);

-- Achievement system indexes
CREATE INDEX idx_achievements_name ON achievements(name);
CREATE INDEX idx_achievements_source_service ON achievements(source_service);
CREATE INDEX idx_achievements_category ON achievements(category);
CREATE INDEX idx_achievements_is_active ON achievements(is_active);
CREATE INDEX idx_achievements_key ON achievements(key);

CREATE INDEX idx_user_achievements_auth_user_id ON user_achievements(auth_user_id);
CREATE INDEX idx_user_achievements_achievement_id ON user_achievements(achievement_id);
CREATE INDEX idx_user_achievements_earned_at ON user_achievements(earned_at);

-- Notification indexes
CREATE INDEX idx_notifications_auth_user_id ON notifications(auth_user_id);
CREATE INDEX idx_notifications_type ON notifications(type);
CREATE INDEX idx_notifications_is_read ON notifications(is_read);
CREATE INDEX idx_notifications_created_at ON notifications(created_at);
CREATE INDEX idx_notifications_read_at ON notifications(read_at);

-- Composite indexes for common query patterns
CREATE INDEX idx_user_profiles_class_route ON user_profiles(class_id, route_id);
CREATE INDEX idx_student_enrollments_user_status ON student_enrollments(auth_user_id, status);
CREATE INDEX idx_student_semester_subjects_enrollment_semester ON student_semester_subjects(enrollment_id, semester);
CREATE INDEX idx_user_skills_user_level ON user_skills(auth_user_id, level);
CREATE INDEX idx_notifications_user_unread ON notifications(auth_user_id, is_read) WHERE is_read = FALSE;
CREATE INDEX idx_curriculum_structure_version_semester ON curriculum_structure(curriculum_version_id, semester);
```

## Service Responsibilities

### Primary Responsibilities
- User profile management and authentication integration
- Academic structure definition and management
- Student enrollment and academic progress tracking
- Skills progression and experience point management
- Achievement system coordination
- Cross-service user context provision
- Notification system management
- Skill Tree catalog ownership and graph relationships
- Reward event ingestion and XP ledger management

### Cross-Service Integration
- Provides user context to all other services
- Receives quest completion updates from Quests Service
- Receives achievement triggers from Social Service
- Receives meeting analytics from Meeting Service
- Receives competition results from Event Service

### Data Access Patterns
- **Read/Write Access**: All tables within User Service domain
- **Read-Only Access**: Provided to other services for user context
- **API Integration**: Exposes user profile and academic data via REST APIs