# Quests Service Database Schema

## Overview
The Quests Service manages the gamified learning experience through quests, challenges, and learning paths. It provides structured learning activities that align with academic curricula and skill development goals.

## Database Tables

### Quest Management

#### quests
Core quest definitions with metadata and configuration
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
    prerequisites UUID[], -- Array of quest IDs that must be completed first
    subject_id UUID, -- Reference to subjects table in User Service
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_by UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### quest_steps
Individual steps within a quest for structured progression
```sql
CREATE TABLE quest_steps (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    step_number INTEGER NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    step_type step_type NOT NULL,
    content JSONB, -- Flexible content structure based on step_type
    validation_criteria JSONB, -- Criteria for step completion validation
    experience_points INTEGER NOT NULL DEFAULT 0,
    is_optional BOOLEAN NOT NULL DEFAULT FALSE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (quest_id, step_number)
);
```

#### quest_resources
Learning resources attached to quests (documents, videos, links, etc.)
```sql
CREATE TABLE quest_resources (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    resource_type resource_type NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    url TEXT,
    file_path TEXT,
    metadata JSONB, -- Additional resource-specific metadata
    display_order INTEGER NOT NULL DEFAULT 0,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

### User Progress Tracking

#### user_quest_attempts
Tracks user attempts and progress on quests
```sql
CREATE TABLE user_quest_attempts (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    status quest_attempt_status NOT NULL DEFAULT 'InProgress',
    started_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    completed_at TIMESTAMPTZ,
    abandoned_at TIMESTAMPTZ,
    total_experience_earned INTEGER NOT NULL DEFAULT 0,
    completion_percentage DECIMAL(5,2) NOT NULL DEFAULT 0.00,
    current_step_id UUID REFERENCES quest_steps(id),
    notes TEXT,
    UNIQUE (auth_user_id, quest_id)
);
```

#### user_quest_step_progress
Detailed tracking of individual step completion
```sql
CREATE TABLE user_quest_step_progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    attempt_id UUID NOT NULL REFERENCES user_quest_attempts(id) ON DELETE CASCADE,
    step_id UUID NOT NULL REFERENCES quest_steps(id) ON DELETE CASCADE,
    status step_completion_status NOT NULL DEFAULT 'NotStarted',
    started_at TIMESTAMPTZ,
    completed_at TIMESTAMPTZ,
    submission_data JSONB, -- User's submission or work for this step
    feedback TEXT,
    experience_earned INTEGER NOT NULL DEFAULT 0,
    attempts_count INTEGER NOT NULL DEFAULT 0,
    UNIQUE (attempt_id, step_id)
);
```

### Learning Paths and Curriculum Integration

#### learning_paths
Structured sequences of quests for comprehensive learning
```sql
CREATE TABLE learning_paths (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    path_type path_type NOT NULL,
    subject_id UUID, -- Reference to subjects table in User Service
    curriculum_version_id UUID, -- Reference to curriculum_versions in User Service
    difficulty_progression difficulty_level[] NOT NULL, -- Array showing difficulty progression
    estimated_total_duration_hours INTEGER,
    total_experience_points INTEGER NOT NULL DEFAULT 0,
    is_published BOOLEAN NOT NULL DEFAULT FALSE,
    created_by UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### learning_path_quests
Junction table linking quests to learning paths with sequencing
```sql
CREATE TABLE learning_path_quests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    learning_path_id UUID NOT NULL REFERENCES learning_paths(id) ON DELETE CASCADE,
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    sequence_order INTEGER NOT NULL,
    is_mandatory BOOLEAN NOT NULL DEFAULT TRUE,
    unlock_criteria JSONB, -- Conditions that must be met to unlock this quest
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (learning_path_id, quest_id),
    UNIQUE (learning_path_id, sequence_order)
);
```

#### user_learning_path_progress
Tracks user progress through learning paths
```sql
CREATE TABLE user_learning_path_progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    learning_path_id UUID NOT NULL REFERENCES learning_paths(id) ON DELETE CASCADE,
    status path_progress_status NOT NULL DEFAULT 'NotStarted',
    started_at TIMESTAMPTZ,
    completed_at TIMESTAMPTZ,
    current_quest_id UUID REFERENCES quests(id),
    completed_quests_count INTEGER NOT NULL DEFAULT 0,
    total_quests_count INTEGER NOT NULL DEFAULT 0,
    completion_percentage DECIMAL(5,2) NOT NULL DEFAULT 0.00,
    total_experience_earned INTEGER NOT NULL DEFAULT 0,
    UNIQUE (auth_user_id, learning_path_id)
);
```

### Assessment and Validation

#### quest_assessments
Assessment configurations for quests requiring evaluation
```sql
CREATE TABLE quest_assessments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    assessment_type assessment_type NOT NULL,
    configuration JSONB NOT NULL, -- Assessment-specific configuration
    passing_criteria JSONB NOT NULL, -- Criteria for successful completion
    max_attempts INTEGER,
    time_limit_minutes INTEGER,
    auto_grade BOOLEAN NOT NULL DEFAULT FALSE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### quest_submissions
User submissions for assessed quests
```sql
CREATE TABLE quest_submissions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    attempt_id UUID NOT NULL REFERENCES user_quest_attempts(id) ON DELETE CASCADE,
    assessment_id UUID NOT NULL REFERENCES quest_assessments(id) ON DELETE CASCADE,
    submission_data JSONB NOT NULL,
    submitted_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    graded_at TIMESTAMPTZ,
    grade DECIMAL(5,2),
    max_grade DECIMAL(5,2) NOT NULL,
    feedback TEXT,
    graded_by UUID, -- Reference to user_profiles.auth_user_id in User Service
    is_passed BOOLEAN,
    attempt_number INTEGER NOT NULL DEFAULT 1
);
```

### Analytics and Insights

#### quest_analytics
Aggregated analytics data for quest performance and engagement
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
    difficulty_rating DECIMAL(3,2), -- User-reported difficulty (1.0-5.0)
    engagement_score DECIMAL(5,2), -- Calculated engagement metric
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (quest_id, date_recorded)
);
```

## Enums and Types

```sql
-- Quest types
CREATE TYPE quest_type AS ENUM ('Tutorial', 'Practice', 'Challenge', 'Project', 'Assessment', 'Exploration');

-- Difficulty levels
CREATE TYPE difficulty_level AS ENUM ('Beginner', 'Intermediate', 'Advanced', 'Expert');

-- Quest step types
CREATE TYPE step_type AS ENUM ('Reading', 'Video', 'Interactive', 'Coding', 'Quiz', 'Discussion', 'Submission', 'Reflection');

-- Resource types
CREATE TYPE resource_type AS ENUM ('Document', 'Video', 'Audio', 'Interactive', 'Link', 'Code', 'Dataset');

-- Quest attempt status
CREATE TYPE quest_attempt_status AS ENUM ('InProgress', 'Completed', 'Abandoned', 'Paused');

-- Step completion status
CREATE TYPE step_completion_status AS ENUM ('NotStarted', 'InProgress', 'Completed', 'Skipped');

-- Learning path types
CREATE TYPE path_type AS ENUM ('Course', 'Specialization', 'Bootcamp', 'Custom');

-- Path progress status
CREATE TYPE path_progress_status AS ENUM ('NotStarted', 'InProgress', 'Completed', 'Paused');

-- Assessment types
CREATE TYPE assessment_type AS ENUM ('Quiz', 'Assignment', 'Project', 'PeerReview', 'AutoGraded', 'ManualReview');
```

## Indexes for Performance

```sql
-- Quest indexes
CREATE INDEX idx_quests_quest_type ON quests(quest_type);
CREATE INDEX idx_quests_difficulty_level ON quests(difficulty_level);
CREATE INDEX idx_quests_subject_id ON quests(subject_id);
CREATE INDEX idx_quests_is_active ON quests(is_active);
CREATE INDEX idx_quests_skill_tags ON quests USING GIN(skill_tags);

-- Quest steps indexes
CREATE INDEX idx_quest_steps_quest_id ON quest_steps(quest_id);
CREATE INDEX idx_quest_steps_step_type ON quest_steps(step_type);

-- User progress indexes
CREATE INDEX idx_user_quest_attempts_auth_user_id ON user_quest_attempts(auth_user_id);
CREATE INDEX idx_user_quest_attempts_quest_id ON user_quest_attempts(quest_id);
CREATE INDEX idx_user_quest_attempts_status ON user_quest_attempts(status);
CREATE INDEX idx_user_quest_step_progress_attempt_id ON user_quest_step_progress(attempt_id);

-- Learning path indexes
CREATE INDEX idx_learning_paths_path_type ON learning_paths(path_type);
CREATE INDEX idx_learning_paths_subject_id ON learning_paths(subject_id);
CREATE INDEX idx_learning_paths_is_published ON learning_paths(is_published);
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
- Curriculum-aligned learning activities
- Publish reward events (XP, Skill Points, Unlocks) per PRD FR58

### Cross-Service Integration
- **User Service**: Retrieves user profiles, academic context, and skill levels; publishes Reward Cascade events (`XPGranted`, `SkillPointsGranted`, `ChallengeUnlocked`) with correlation IDs to User Service for authoritative ledgering (`user_skill_rewards`) and skill progression updates (`user_skills`)
- **Social Service**: Provides quest completion data for party/guild activities
- **Meeting Service**: Integrates collaborative quests with meeting sessions
- **Code Battle Service**: Links coding challenges to quest progression

### Data Access Patterns
- **Read/Write Access**: All tables within Quests Service domain
- **External References**: User profiles, subjects, curriculum data, and Skill Catalog (skills, dependencies) from User Service
- **API Integration**: Exposes quest data and progress tracking via REST APIs
- **Event Publishing**: Publishes quest completion and objective events for achievement system and Reward Cascade; sends `XPGranted` and related reward events to User Service for ledgering per FR58

### Vector Database Integration
- **Qdrant Collections**: Stores quest content embeddings for semantic search and recommendations
- **Content Indexing**: Quest descriptions, step content, and resource materials
- **Recommendation Engine**: Skill-based and progress-based quest suggestions

## Ownership and Integration Notes

- **Rewards Ledger Non-Ownership**: Although quests define `experience_points_reward` and track earned XP within attempts/progress for analytics, the Quests Service does not own the authoritative XP ledger. All reward events are published to the User Service, which persists them in `user_skill_rewards` and applies progression to `user_skills`.
- **Reward Cascade Compliance (FR58)**: Objective completion triggers a pipeline of events: `XPGranted`, `SkillPointsGranted`, `ChallengeUnlocked`. Events must include correlation IDs to ensure auditability and ordering. Quests Service emits these events; User Service ingests and persists them.
- **Skill Tree Catalog Non-Ownership**: Quests Service consumes the Skill Catalog (skills and dependencies) maintained by the User Service to tag quests (`skill_tags`), generate objectives, and perform recommendations. It does not define or own `skills` or `skill_dependencies` tables.