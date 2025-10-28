# Quests Service Database Schema

## Overview
The Quests Service manages the gamified learning experience through quests, challenges, and learning paths. It provides structured learning activities aligned with academic curricula and skill development goals.

-- Enable UUID generation if not already enabled
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

## Database Tables

### Learning Paths and Curriculum Integration

#### learning_paths
Top-level container for an entire learning journey (e.g., a full course or curriculum), also referred to as a QuestLine.
```sql
CREATE TABLE learning_paths (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    path_type path_type NOT NULL,
    curriculum_version_id UUID, -- Soft FK to User Service: curriculum_versions
    estimated_total_duration_hours INTEGER,
    total_experience_points INTEGER NOT NULL DEFAULT 0,
    is_published BOOLEAN NOT NULL DEFAULT FALSE,
    created_by UUID NOT NULL, -- Soft FK to User Service: user_profiles(auth_user_id)
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### quest_chapters
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

#### learning_path_quests
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

### Quest Management

#### quests
Core quest definitions with metadata and configuration. Prerequisites are managed in the `quest_prerequisites` table.
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
    subject_id UUID, -- Soft FK to User Service: subjects
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_by UUID NOT NULL, -- Soft FK to User Service: user_profiles(auth_user_id)
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### quest_prerequisites
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

#### quest_steps
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

#### quest_resources
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

### User Progress Tracking

#### user_quest_attempts
Tracks user attempts and progress on quests.
```sql
CREATE TABLE user_quest_attempts (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL, -- Soft FK to User Service: user_profiles(auth_user_id)
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

#### user_quest_step_progress
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

#### user_learning_path_progress
Tracks user progress through learning paths.
```sql
CREATE TABLE user_learning_path_progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL, -- Soft FK to User Service: user_profiles(auth_user_id)
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

### Assessment and Validation

#### quest_assessments
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

#### quest_submissions
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

### Analytics and Insights

#### quest_analytics
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

## Enums and Types
```sql
CREATE TYPE quest_type AS ENUM ('Tutorial', 'Practice', 'Challenge', 'Project', 'Assessment', 'Exploration');
CREATE TYPE difficulty_level AS ENUM ('Beginner', 'Intermediate', 'Advanced', 'Expert');
CREATE TYPE step_type AS ENUM ('Reading', 'Video', 'Interactive', 'Coding', 'Quiz', 'Discussion', 'Submission', 'Reflection');
CREATE TYPE resource_type AS ENUM ('Document', 'Video', 'Audio', 'Interactive', 'Link', 'Code', 'Dataset');
CREATE TYPE quest_attempt_status AS ENUM ('InProgress', 'Completed', 'Abandoned', 'Paused');
CREATE TYPE step_completion_status AS ENUM ('NotStarted', 'InProgress', 'Completed', 'Skipped');
CREATE TYPE path_type AS ENUM ('Course', 'Specialization', 'Bootcamp', 'Custom');
CREATE TYPE path_progress_status AS ENUM ('NotStarted', 'InProgress', 'Completed', 'Paused');
CREATE TYPE assessment_type AS ENUM ('Quiz', 'Assignment', 'Project', 'PeerReview', 'AutoGraded', 'ManualReview');
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

-- Example of applying the trigger to a table.
CREATE TRIGGER update_learning_path_quests_updated_at 
    BEFORE UPDATE ON learning_path_quests 
    FOR EACH ROW 
    EXECUTE FUNCTION update_updated_at_column();
```

## Indexes for Performance
```sql
-- Quest indexes
CREATE INDEX idx_quests_quest_type ON quests(quest_type);
CREATE INDEX idx_quests_difficulty_level ON quests(difficulty_level);
CREATE INDEX idx_quests_subject_id ON quests(subject_id);
CREATE INDEX idx_quests_is_active ON quests(is_active);
CREATE INDEX idx_quests_skill_tags ON quests USING GIN(skill_tags);

-- Quest prerequisites index
CREATE INDEX idx_quest_prerequisites_quest_id ON quest_prerequisites(quest_id);
CREATE INDEX idx_quest_prerequisites_prereq_id ON quest_prerequisites(prerequisite_quest_id);

-- Quest steps indexes
CREATE INDEX idx_quest_steps_quest_id ON quest_steps(quest_id);
CREATE INDEX idx_quest_steps_step_type ON quest_steps(step_type);

-- User progress indexes
CREATE INDEX idx_user_quest_attempts_auth_user_id ON user_quest_attempts(auth_user_id);
CREATE INDEX idx_user_quest_attempts_quest_id ON user_quest_attempts(quest_id);
CREATE INDEX idx_user_quest_attempts_status ON user_quest_attempts(status);
CREATE INDEX idx_user_quest_step_progress_attempt_id ON user_quest_step_progress(attempt_id);

-- Learning path and chapter indexes
CREATE INDEX idx_learning_paths_path_type ON learning_paths(path_type);
CREATE INDEX idx_learning_paths_subject_id ON learning_paths(subject_id);
CREATE INDEX idx_learning_paths_is_published ON learning_paths(is_published);
CREATE INDEX idx_quest_chapters_quest_line_id ON quest_chapters(quest_line_id);
CREATE INDEX idx_learning_path_quests_learning_path_id ON learning_path_quests(learning_path_id);
CREATE INDEX idx_user_learning_path_progress_auth_user_id ON user_learning_path_progress(auth_user_id);

-- Assessment indexes
CREATE INDEX idx_quest_assessments_quest_id ON quest_assessments(quest_id);
CREATE INDEX idx_quest_submissions_attempt_id ON quest_submissions(attempt_id);
CREATE INDEX idx_quest_submissions_assessment_id ON quest_submissions(assessment_id);

-- Analytics indexes
CREATE INDEX idx_quest_analytics_quest_id ON quest_analytics(quest_id);
CREATE INDEX idx_quest_analytics_date_recorded ON quest_analytics(date_recorded);
```

## Service Responsibilities

### Primary Responsibilities
- Quest creation, management, and lifecycle
- Learning path design and progression tracking
- User progress monitoring and analytics
- Assessment and validation systems
- Skill-based quest recommendations
- Publish Reward Cascade events (XP, Skill Points, Unlocks) per PRD FR58

### Cross-Service Integration
- User Service: Retrieves user profiles, academic context, and skill levels; ingests reward events for authoritative ledgering in `user_skill_rewards` and skill progression updates in `user_skills`
- Social Service: Consumes quest completion data for party/guild activities
- Meeting Service: Integrates collaborative quests with meeting sessions
- Event Service: Links coding challenges to quest progression

### Data Access Patterns
- Read/Write Access: All tables within Quests Service domain
- External References: User profiles, subjects, curriculum data, and Skill Catalog from User Service
- API Integration: Exposes quest data and progress tracking via REST APIs
- Event Publishing: Emits `XPGranted`, `SkillPointsGranted`, `ChallengeUnlocked` with correlation IDs for Reward Cascade compliance
- Vector Database Integration: Qdrant collections store quest content embeddings for semantic search and recommendations

### Real-time Features
- Optional WebSocket or server-sent events for live progress updates, attempt status changes, and assessment notifications
- Live recommendations and unlock notifications based on Reward Cascade events

## Ownership and Integration Notes
- Notes (Arsenal) Non-Ownership: Quests Service does not own the `notes` table; links are managed via User Service APIs
- Rewards Ledger Non-Ownership: Quests Service defines rewards but does not own the authoritative XP ledger; User Service persists in `user_skill_rewards`
- Reward Cascade Compliance (FR58): Objective completion triggers `XPGranted`, `SkillPointsGranted`, `ChallengeUnlocked` events with correlation IDs for auditability
- Skill Tree Catalog Non-Ownership: Quests Service consumes the Skill Catalog (skills, dependencies) from User Service and does not define those tables