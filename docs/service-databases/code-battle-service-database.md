# Code Battle Service Database Schema

## Overview
The Code Battle Service manages competitive programming challenges, coding tournaments, and skill-based coding assessments. It provides a comprehensive platform for code competitions, automated testing, and performance analytics.

## Database Tables

### Programming Languages and Environment

#### languages
Supported programming languages with execution configuration
```sql
CREATE TABLE languages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL UNIQUE,
    version VARCHAR(50) NOT NULL,
    file_extension VARCHAR(10) NOT NULL,
    compile_cmd TEXT,
    execute_cmd TEXT NOT NULL,
    timeout_seconds INTEGER NOT NULL DEFAULT 30,
    memory_limit_mb INTEGER NOT NULL DEFAULT 256,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

### Competition Management

#### rooms
Competition rooms and coding battle environments
```sql
CREATE TABLE rooms (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    room_type room_type NOT NULL,
    max_players INTEGER NOT NULL DEFAULT 2,
    current_players_count INTEGER NOT NULL DEFAULT 0,
    difficulty_level difficulty_level NOT NULL,
    time_limit_minutes INTEGER NOT NULL DEFAULT 60,
    creator_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    party_id UUID, -- Reference to parties in Social Service
    guild_id UUID, -- Reference to guilds in Social Service
    class_id UUID, -- Reference to classes in User Service
    subject_id UUID, -- Reference to subjects in User Service
    status room_status NOT NULL DEFAULT 'Waiting',
    is_public BOOLEAN NOT NULL DEFAULT TRUE,
    requires_approval BOOLEAN NOT NULL DEFAULT FALSE,
    entry_fee_points INTEGER NOT NULL DEFAULT 0,
    winner_reward_points INTEGER NOT NULL DEFAULT 0,
    scheduled_start_time TIMESTAMPTZ,
    actual_start_time TIMESTAMPTZ,
    actual_end_time TIMESTAMPTZ,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### room_players
Player participation in competition rooms
```sql
CREATE TABLE room_players (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    room_id UUID NOT NULL REFERENCES rooms(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    player_role player_role NOT NULL DEFAULT 'Participant',
    status player_status NOT NULL DEFAULT 'Joined',
    joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    left_at TIMESTAMPTZ,
    final_score INTEGER NOT NULL DEFAULT 0,
    final_rank INTEGER,
    total_submissions INTEGER NOT NULL DEFAULT 0,
    problems_solved INTEGER NOT NULL DEFAULT 0,
    total_penalty_time INTEGER NOT NULL DEFAULT 0, -- In minutes
    UNIQUE (room_id, auth_user_id)
);
```

### Problem Management

#### code_problems
Coding problems and challenges
```sql
CREATE TABLE code_problems (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    problem_statement TEXT NOT NULL,
    input_format TEXT NOT NULL,
    output_format TEXT NOT NULL,
    constraints TEXT NOT NULL,
    difficulty_level difficulty_level NOT NULL,
    time_limit_seconds INTEGER NOT NULL DEFAULT 2,
    memory_limit_mb INTEGER NOT NULL DEFAULT 256,
    problem_type problem_type NOT NULL,
    tags TEXT[] NOT NULL DEFAULT '{}',
    skill_categories TEXT[] NOT NULL DEFAULT '{}',
    author_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    is_public BOOLEAN NOT NULL DEFAULT TRUE,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    solution_template JSONB, -- Language-specific solution templates
    editorial TEXT, -- Problem explanation and solution approach
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### test_cases
Test cases for problem validation
```sql
CREATE TABLE test_cases (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    problem_id UUID NOT NULL REFERENCES code_problems(id) ON DELETE CASCADE,
    input_data TEXT NOT NULL,
    expected_output TEXT NOT NULL,
    is_sample BOOLEAN NOT NULL DEFAULT FALSE,
    is_hidden BOOLEAN NOT NULL DEFAULT TRUE,
    test_case_order INTEGER NOT NULL,
    points INTEGER NOT NULL DEFAULT 1,
    explanation TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (problem_id, test_case_order)
);
```

#### room_problems
Problems assigned to specific competition rooms
```sql
CREATE TABLE room_problems (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    room_id UUID NOT NULL REFERENCES rooms(id) ON DELETE CASCADE,
    problem_id UUID NOT NULL REFERENCES code_problems(id) ON DELETE CASCADE,
    problem_order INTEGER NOT NULL,
    points INTEGER NOT NULL DEFAULT 100,
    penalty_per_wrong_submission INTEGER NOT NULL DEFAULT 20, -- In minutes
    is_bonus_problem BOOLEAN NOT NULL DEFAULT FALSE,
    unlock_time_minutes INTEGER NOT NULL DEFAULT 0, -- Minutes after room start
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (room_id, problem_id),
    UNIQUE (room_id, problem_order)
);
```

### Submission and Evaluation

#### submissions
Code submissions from participants
```sql
CREATE TABLE submissions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    room_id UUID NOT NULL REFERENCES rooms(id) ON DELETE CASCADE,
    problem_id UUID NOT NULL REFERENCES code_problems(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    language_id UUID NOT NULL REFERENCES languages(id),
    source_code TEXT NOT NULL,
    submission_time TIMESTAMPTZ NOT NULL DEFAULT now(),
    status submission_status NOT NULL DEFAULT 'Pending',
    verdict verdict_type,
    score INTEGER NOT NULL DEFAULT 0,
    execution_time_ms INTEGER,
    memory_used_kb INTEGER,
    test_cases_passed INTEGER NOT NULL DEFAULT 0,
    total_test_cases INTEGER NOT NULL DEFAULT 0,
    compilation_error TEXT,
    runtime_error TEXT,
    judge_feedback TEXT,
    is_final_submission BOOLEAN NOT NULL DEFAULT FALSE
);
```

#### submission_test_results
Detailed test case execution results
```sql
CREATE TABLE submission_test_results (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    submission_id UUID NOT NULL REFERENCES submissions(id) ON DELETE CASCADE,
    test_case_id UUID NOT NULL REFERENCES test_cases(id) ON DELETE CASCADE,
    status test_result_status NOT NULL,
    execution_time_ms INTEGER,
    memory_used_kb INTEGER,
    actual_output TEXT,
    error_message TEXT,
    points_earned INTEGER NOT NULL DEFAULT 0,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (submission_id, test_case_id)
);
```

### Tournament and Event Management

#### tournaments
Large-scale coding tournaments and competitions
```sql
CREATE TABLE tournaments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    tournament_type tournament_type NOT NULL,
    format tournament_format NOT NULL,
    max_participants INTEGER,
    current_participants_count INTEGER NOT NULL DEFAULT 0,
    registration_start_time TIMESTAMPTZ NOT NULL,
    registration_end_time TIMESTAMPTZ NOT NULL,
    tournament_start_time TIMESTAMPTZ NOT NULL,
    tournament_end_time TIMESTAMPTZ NOT NULL,
    organizer_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    entry_requirements JSONB, -- Skill level, prerequisites, etc.
    prize_pool JSONB, -- Prize distribution configuration
    rules TEXT NOT NULL,
    status tournament_status NOT NULL DEFAULT 'Upcoming',
    is_public BOOLEAN NOT NULL DEFAULT TRUE,
    banner_image_url TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### tournament_participants
Tournament registration and participation tracking
```sql
CREATE TABLE tournament_participants (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tournament_id UUID NOT NULL REFERENCES tournaments(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    team_name VARCHAR(255), -- For team tournaments
    registration_time TIMESTAMPTZ NOT NULL DEFAULT now(),
    status participant_status NOT NULL DEFAULT 'Registered',
    final_rank INTEGER,
    final_score INTEGER NOT NULL DEFAULT 0,
    prize_awarded JSONB, -- Prize details if won
    UNIQUE (tournament_id, auth_user_id)
);
```

#### tournament_rounds
Tournament round structure and progression
```sql
CREATE TABLE tournament_rounds (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tournament_id UUID NOT NULL REFERENCES tournaments(id) ON DELETE CASCADE,
    round_number INTEGER NOT NULL,
    round_name VARCHAR(255) NOT NULL,
    round_type round_type NOT NULL,
    start_time TIMESTAMPTZ NOT NULL,
    end_time TIMESTAMPTZ NOT NULL,
    problems UUID[] NOT NULL, -- Array of problem IDs
    advancement_criteria JSONB NOT NULL, -- Criteria for advancing to next round
    status round_status NOT NULL DEFAULT 'Upcoming',
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (tournament_id, round_number)
);
```

### Analytics and Performance Tracking

#### player_statistics
Comprehensive player performance statistics
```sql
CREATE TABLE player_statistics (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL UNIQUE, -- Reference to user_profiles.auth_user_id in User Service
    total_battles INTEGER NOT NULL DEFAULT 0,
    battles_won INTEGER NOT NULL DEFAULT 0,
    battles_lost INTEGER NOT NULL DEFAULT 0,
    total_problems_solved INTEGER NOT NULL DEFAULT 0,
    total_submissions INTEGER NOT NULL DEFAULT 0,
    accepted_submissions INTEGER NOT NULL DEFAULT 0,
    current_rating INTEGER NOT NULL DEFAULT 1200,
    peak_rating INTEGER NOT NULL DEFAULT 1200,
    rating_history JSONB NOT NULL DEFAULT '[]', -- Array of rating changes with timestamps
    favorite_languages UUID[] NOT NULL DEFAULT '{}', -- Array of language IDs
    skill_ratings JSONB NOT NULL DEFAULT '{}', -- Skill-specific ratings
    last_active_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### problem_statistics
Problem difficulty and performance analytics
```sql
CREATE TABLE problem_statistics (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    problem_id UUID NOT NULL UNIQUE REFERENCES code_problems(id) ON DELETE CASCADE,
    total_submissions INTEGER NOT NULL DEFAULT 0,
    accepted_submissions INTEGER NOT NULL DEFAULT 0,
    acceptance_rate DECIMAL(5,2) NOT NULL DEFAULT 0.00,
    average_attempts DECIMAL(5,2) NOT NULL DEFAULT 0.00,
    average_solve_time_minutes DECIMAL(10,2),
    difficulty_rating DECIMAL(5,2), -- Community-rated difficulty
    language_statistics JSONB NOT NULL DEFAULT '{}', -- Per-language statistics
    last_updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

### Leaderboards and Rankings

#### leaderboards
Various leaderboard configurations
```sql
CREATE TABLE leaderboards (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    leaderboard_type leaderboard_type NOT NULL,
    scope leaderboard_scope NOT NULL,
    time_period leaderboard_period,
    criteria JSONB NOT NULL, -- Ranking criteria configuration
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### leaderboard_entries
Current leaderboard standings
```sql
CREATE TABLE leaderboard_entries (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    leaderboard_id UUID NOT NULL REFERENCES leaderboards(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    rank INTEGER NOT NULL,
    score DECIMAL(10,2) NOT NULL,
    additional_metrics JSONB, -- Extra metrics for display
    last_updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (leaderboard_id, auth_user_id)
);
```

## Enums and Types

```sql
-- Room types
CREATE TYPE room_type AS ENUM ('OneVsOne', 'MultiPlayer', 'Practice', 'Tournament', 'Team', 'Classroom');

-- Difficulty levels
CREATE TYPE difficulty_level AS ENUM ('Easy', 'Medium', 'Hard', 'Expert');

-- Room status
CREATE TYPE room_status AS ENUM ('Waiting', 'InProgress', 'Completed', 'Cancelled', 'Paused');

-- Player roles
CREATE TYPE player_role AS ENUM ('Participant', 'Spectator', 'Moderator');

-- Player status
CREATE TYPE player_status AS ENUM ('Joined', 'Ready', 'Playing', 'Finished', 'Disconnected', 'Left');

-- Problem types
CREATE TYPE problem_type AS ENUM ('Algorithm', 'DataStructure', 'Math', 'String', 'Graph', 'Dynamic Programming', 'Greedy', 'Implementation');

-- Submission status
CREATE TYPE submission_status AS ENUM ('Pending', 'Judging', 'Completed', 'Error');

-- Verdict types
CREATE TYPE verdict_type AS ENUM ('Accepted', 'Wrong Answer', 'Time Limit Exceeded', 'Memory Limit Exceeded', 'Runtime Error', 'Compilation Error', 'Presentation Error');

-- Test result status
CREATE TYPE test_result_status AS ENUM ('Passed', 'Failed', 'Error', 'Timeout', 'Memory Exceeded');

-- Tournament types
CREATE TYPE tournament_type AS ENUM ('Individual', 'Team', 'School', 'Open', 'Invitational');

-- Tournament formats
CREATE TYPE tournament_format AS ENUM ('Single Elimination', 'Double Elimination', 'Round Robin', 'Swiss', 'ICPC Style');

-- Tournament status
CREATE TYPE tournament_status AS ENUM ('Upcoming', 'Registration Open', 'Registration Closed', 'InProgress', 'Completed', 'Cancelled');

-- Participant status
CREATE TYPE participant_status AS ENUM ('Registered', 'Confirmed', 'Competing', 'Eliminated', 'Finished', 'Disqualified');

-- Round types
CREATE TYPE round_type AS ENUM ('Qualification', 'Elimination', 'Semifinal', 'Final', 'Practice');

-- Round status
CREATE TYPE round_status AS ENUM ('Upcoming', 'InProgress', 'Completed', 'Cancelled');

-- Leaderboard types
CREATE TYPE leaderboard_type AS ENUM ('Rating', 'Problems Solved', 'Contest Performance', 'Skill Based', 'Custom');

-- Leaderboard scope
CREATE TYPE leaderboard_scope AS ENUM ('Global', 'Class', 'Guild', 'Subject', 'Custom');

-- Leaderboard period
CREATE TYPE leaderboard_period AS ENUM ('All Time', 'Monthly', 'Weekly', 'Daily', 'Custom');
```

## Indexes for Performance

```sql
-- Language indexes
CREATE INDEX idx_languages_name ON languages(name);
CREATE INDEX idx_languages_is_active ON languages(is_active);

-- Room indexes
CREATE INDEX idx_rooms_creator_id ON rooms(creator_id);
CREATE INDEX idx_rooms_room_type ON rooms(room_type);
CREATE INDEX idx_rooms_status ON rooms(status);
CREATE INDEX idx_rooms_difficulty_level ON rooms(difficulty_level);
CREATE INDEX idx_rooms_is_public ON rooms(is_public);
CREATE INDEX idx_rooms_scheduled_start_time ON rooms(scheduled_start_time);

-- Player indexes
CREATE INDEX idx_room_players_room_id ON room_players(room_id);
CREATE INDEX idx_room_players_auth_user_id ON room_players(auth_user_id);
CREATE INDEX idx_room_players_status ON room_players(status);

-- Problem indexes
CREATE INDEX idx_code_problems_difficulty_level ON code_problems(difficulty_level);
CREATE INDEX idx_code_problems_problem_type ON code_problems(problem_type);
CREATE INDEX idx_code_problems_author_id ON code_problems(author_id);
CREATE INDEX idx_code_problems_is_public ON code_problems(is_public);
CREATE INDEX idx_code_problems_tags ON code_problems USING GIN(tags);
CREATE INDEX idx_code_problems_skill_categories ON code_problems USING GIN(skill_categories);

-- Test case indexes
CREATE INDEX idx_test_cases_problem_id ON test_cases(problem_id);
CREATE INDEX idx_test_cases_is_sample ON test_cases(is_sample);

-- Submission indexes
CREATE INDEX idx_submissions_room_id ON submissions(room_id);
CREATE INDEX idx_submissions_problem_id ON submissions(problem_id);
CREATE INDEX idx_submissions_auth_user_id ON submissions(auth_user_id);
CREATE INDEX idx_submissions_language_id ON submissions(language_id);
CREATE INDEX idx_submissions_status ON submissions(status);
CREATE INDEX idx_submissions_verdict ON submissions(verdict);
CREATE INDEX idx_submissions_submission_time ON submissions(submission_time);

-- Tournament indexes
CREATE INDEX idx_tournaments_tournament_type ON tournaments(tournament_type);
CREATE INDEX idx_tournaments_status ON tournaments(status);
CREATE INDEX idx_tournaments_organizer_id ON tournaments(organizer_id);
CREATE INDEX idx_tournaments_tournament_start_time ON tournaments(tournament_start_time);

-- Statistics indexes
CREATE INDEX idx_player_statistics_auth_user_id ON player_statistics(auth_user_id);
CREATE INDEX idx_player_statistics_current_rating ON player_statistics(current_rating);
CREATE INDEX idx_problem_statistics_problem_id ON problem_statistics(problem_id);
CREATE INDEX idx_problem_statistics_acceptance_rate ON problem_statistics(acceptance_rate);

-- Leaderboard indexes
CREATE INDEX idx_leaderboards_leaderboard_type ON leaderboards(leaderboard_type);
CREATE INDEX idx_leaderboards_scope ON leaderboards(scope);
CREATE INDEX idx_leaderboard_entries_leaderboard_id ON leaderboard_entries(leaderboard_id);
CREATE INDEX idx_leaderboard_entries_rank ON leaderboard_entries(rank);
```

## Service Responsibilities

### Primary Responsibilities
- Competitive programming platform management
- Code submission and automated judging
- Tournament and competition organization
- Player rating and ranking systems
- Problem creation and management
- Performance analytics and statistics

### Cross-Service Integration
- **User Service**: Retrieves user profiles and skill levels for matchmaking
- **Social Service**: Integrates with parties and guilds for team competitions
- **Meeting Service**: Links code battles with review sessions and debriefs
- **Quests Service**: Provides coding challenges as quest activities

### Data Access Patterns
- **Read/Write Access**: All tables within Code Battle Service domain
- **External References**: User profiles, parties, guilds, classes, and subjects
- **API Integration**: Exposes competition data via REST APIs
- **Real-time Integration**: WebSocket connections for live competition updates

### Vector Database Integration (Qdrant)
- **Qdrant Collections**: Stores code and problem embeddings for semantic search and similarity matching
- **Content Indexing**: Code submissions, problem statements, and solution patterns
- **Similarity Engine**: Code plagiarism detection and problem recommendation system

### Real-time Features
- **Live Competitions**: Real-time leaderboard updates and submission results
- **Spectator Mode**: Live viewing of ongoing competitions
- **Chat Integration**: Real-time communication during team competitions
- **Judge System**: Automated code execution and result reporting

### Analytics and Insights
- **Performance Tracking**: Individual and aggregate performance metrics
- **Difficulty Analysis**: Problem difficulty calibration based on submission data
- **Skill Assessment**: Automated skill level evaluation based on performance
- **Competition Analytics**: Tournament and competition effectiveness metrics