# User Service Database Schema

## Overview
The User Service manages user profiles, authentication, academic structure, achievements, and user progress tracking. This service maintains the core user data and academic framework for the RogueLearn platform.

## Enums and Types

-- Verification status for lecturer requests
CREATE TYPE verification_status AS ENUM ('Pending', 'Approved', 'Rejected');

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
    difficulty_level INTEGER DEFAULT 1, -- 1=Beginner, 2=Intermediate, 3=Advanced
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
    email VARCHAR(255) NOT NULL UNIQUE,
    first_name VARCHAR(255),
    last_name VARCHAR(255),
    class_id UUID REFERENCES classes(id), -- Selected roadmap.sh specialization (Step 2)
    route_id UUID REFERENCES curriculum_programs(id), -- Selected curriculum (Step 1)
    level INTEGER NOT NULL DEFAULT 1,
    experience_points INTEGER NOT NULL DEFAULT 0,
    profile_image_url TEXT,
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
Junction table defining curriculum-subject relationships with term sequencing
```sql
CREATE TABLE curriculum_structure (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    curriculum_version_id UUID NOT NULL REFERENCES curriculum_versions(id) ON DELETE CASCADE,
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    term_number INTEGER NOT NULL,
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

#### student_term_subjects
Comprehensive academic term tracking with status
```sql
CREATE TABLE student_term_subjects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    enrollment_id UUID NOT NULL REFERENCES student_enrollments(id) ON DELETE CASCADE,
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    term_number INTEGER NOT NULL,
    academic_year VARCHAR(10) NOT NULL,
    semester VARCHAR(20) NOT NULL,
    status subject_enrollment_status NOT NULL DEFAULT 'Enrolled',
    grade VARCHAR(5),
    credits_earned INTEGER DEFAULT 0,
    enrolled_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    completed_at TIMESTAMPTZ,
    UNIQUE (enrollment_id, subject_id, term_number, academic_year, semester)
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
    tier INTEGER NOT NULL DEFAULT 1, -- 1=Foundation, 2=Intermediate, 3=Advanced, 4=Expert
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
    relationship_type VARCHAR(30) NOT NULL DEFAULT 'Prerequisite', -- Prerequisite, Complements, Alternative
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
    is_public BOOLEAN NOT NULL DEFAULT FALSE,
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
    source_type VARCHAR(50) NOT NULL, -- e.g., QuestComplete, BossFight, PartyActivity
    source_id UUID, -- Optional reference to the originating entity
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
-- User profile indexes
CREATE INDEX idx_user_profiles_username ON user_profiles(username);
CREATE INDEX idx_user_profiles_email ON user_profiles(email);
CREATE INDEX idx_user_profiles_class_id ON user_profiles(class_id);

-- Roadmap.sh specialization indexes
CREATE INDEX idx_classes_name ON classes(name);
CREATE INDEX idx_classes_is_active ON classes(is_active);
CREATE INDEX idx_classes_difficulty_level ON classes(difficulty_level);
-- Class hierarchy and mapping indexes
CREATE INDEX idx_class_nodes_class_id ON class_nodes(class_id);
CREATE INDEX idx_class_nodes_parent_id ON class_nodes(parent_id);
CREATE INDEX idx_class_specialization_subjects_class_id ON class_specialization_subjects(class_id);
CREATE INDEX idx_class_specialization_subjects_subject_id ON class_specialization_subjects(subject_id);

-- Academic structure indexes
CREATE INDEX idx_curriculum_structure_curriculum_version_id ON curriculum_structure(curriculum_version_id);
CREATE INDEX idx_curriculum_structure_subject_id ON curriculum_structure(subject_id);
CREATE INDEX idx_student_enrollments_auth_user_id ON student_enrollments(auth_user_id);
CREATE INDEX idx_student_term_subjects_enrollment_id ON student_term_subjects(enrollment_id);

-- Notes/Arsenal indexes
CREATE INDEX idx_notes_auth_user_id ON notes(auth_user_id);
CREATE INDEX idx_note_quests_note_id ON note_quests(note_id);
CREATE INDEX idx_note_quests_quest_id ON note_quests(quest_id);
CREATE INDEX idx_note_skills_note_id ON note_skills(note_id);
CREATE INDEX idx_note_skills_skill_id ON note_skills(skill_id);
CREATE INDEX idx_tags_auth_user_id ON tags(auth_user_id);
CREATE INDEX idx_note_tags_note_id ON note_tags(note_id);
CREATE INDEX idx_note_tags_tag_id ON note_tags(tag_id);

-- Skills and progress indexes
CREATE INDEX idx_user_skills_auth_user_id ON user_skills(auth_user_id);
CREATE INDEX idx_user_quest_progress_auth_user_id ON user_quest_progress(auth_user_id);
CREATE INDEX idx_user_quest_progress_quest_id ON user_quest_progress(quest_id);
CREATE INDEX idx_skills_name ON skills(name);
CREATE INDEX idx_skills_domain ON skills(domain);
CREATE INDEX idx_skill_dependencies_skill_id ON skill_dependencies(skill_id);
CREATE INDEX idx_skill_dependencies_prereq_id ON skill_dependencies(prerequisite_skill_id);
CREATE INDEX idx_user_skill_rewards_auth_user_id ON user_skill_rewards(auth_user_id);
CREATE INDEX idx_user_skill_rewards_skill_name ON user_skill_rewards(skill_name);
CREATE INDEX idx_user_skill_rewards_source_service ON user_skill_rewards(source_service);
CREATE INDEX idx_user_skill_rewards_created_at ON user_skill_rewards(created_at);

-- Achievement indexes
CREATE INDEX idx_user_achievements_auth_user_id ON user_achievements(auth_user_id);
CREATE INDEX idx_user_achievements_achievement_id ON user_achievements(achievement_id);

-- Notification indexes
CREATE INDEX idx_notifications_auth_user_id ON notifications(auth_user_id);
CREATE INDEX idx_notifications_type ON notifications(type);
CREATE INDEX idx_notifications_is_read ON notifications(is_read);
CREATE INDEX idx_notifications_created_at ON notifications(created_at);

-- Admin governance indexes
CREATE INDEX idx_curriculum_version_activations_version_id ON curriculum_version_activations(curriculum_version_id);
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
- Receives competition results from Code Battle Service

### Data Access Patterns
- **Read/Write Access**: All tables within User Service domain
- **Read-Only Access**: Provided to other services for user context
- **API Integration**: Exposes user profile and academic data via REST APIs