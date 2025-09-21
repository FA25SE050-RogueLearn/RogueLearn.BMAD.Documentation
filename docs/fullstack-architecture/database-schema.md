# **Database Schema**

This section provides the SQL DDL for the PostgreSQL database.

```sql
-- RogueLearn Database Schema for PostgreSQL
-- FINAL VERSION: Integrated with Supabase Auth and a robust, real-time Code Battle engine.
-- CORRECTED: Fixed table creation order and constraint issues

-- Enable UUID generation if not already enabled
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Enum Types
CREATE TYPE user_role AS ENUM ('Student', 'Lecturer', 'Admin');
CREATE TYPE quest_status AS ENUM ('NotStarted', 'InProgress', 'Completed', 'Failed');
CREATE TYPE quest_type AS ENUM ('Theory', 'Practice', 'Project', 'Assessment');
CREATE TYPE skill_level AS ENUM ('Beginner', 'Intermediate', 'Advanced', 'Expert');
CREATE TYPE achievement_type AS ENUM ('Quest', 'Skill', 'Social', 'Special');
CREATE TYPE notification_type AS ENUM ('Quest', 'Achievement', 'Social', 'System');
CREATE TYPE party_role AS ENUM ('Leader', 'Member');
CREATE TYPE meeting_status AS ENUM ('Scheduled', 'InProgress', 'Completed', 'Cancelled');
CREATE TYPE event_type AS ENUM ('Competition', 'Workshop', 'Seminar', 'Social');
CREATE TYPE event_status AS ENUM ('Upcoming', 'Ongoing', 'Completed', 'Cancelled');
CREATE TYPE submission_status AS ENUM ('Pending', 'Running', 'Accepted', 'WrongAnswer', 'TimeLimitExceeded', 'RuntimeError', 'CompileError');
CREATE TYPE student_term_subject_status AS ENUM ('Enrolled', 'Completed', 'Failed', 'Withdrawn');
CREATE TYPE game_session_status AS ENUM ('InProgress', 'Completed', 'Abandoned');
CREATE TYPE verification_status AS ENUM ('Pending', 'Approved', 'Rejected');
CREATE TYPE party_join_type AS ENUM ('Open', 'Invite Only', 'Request');
CREATE TYPE participant_status AS ENUM ('Invited', 'Accepted', 'Declined', 'Attended');


-- ------------------------------------------
-- SECTION 1: FOUNDATION TABLES (No Dependencies)
-- ------------------------------------------

-- Create classes table first as it's referenced by user_profiles
CREATE TABLE classes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT UNIQUE NOT NULL,
    description TEXT
);

-- Create roles table early as it's referenced by user_roles
CREATE TABLE roles (
    id SERIAL PRIMARY KEY,
    role_name TEXT UNIQUE NOT NULL
);

-- ------------------------------------------
-- SECTION 2: USER & PROFILE CORE
-- ------------------------------------------

CREATE TABLE user_profiles (
    auth_user_id UUID PRIMARY KEY, -- FK to Supabase Auth Users (now primary key)
    username TEXT UNIQUE NOT NULL,
    email TEXT UNIQUE NOT NULL,
    first_name TEXT NOT NULL,
    last_name TEXT NOT NULL,
    class_id UUID REFERENCES classes(id) ON DELETE SET NULL, -- Fixed FK reference
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE user_roles (
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    role_id INT NOT NULL REFERENCES roles(id) ON DELETE RESTRICT,
    PRIMARY KEY (auth_user_id, role_id)
);

CREATE TABLE lecturer_verification_requests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    status verification_status NOT NULL DEFAULT 'Pending',
    submitted_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    reviewed_at TIMESTAMPTZ,
    reviewer_notes TEXT
);


-- ------------------------------------------
-- SECTION 3: ACADEMIC & CONTENT MANAGEMENT (Updated with Enhanced Curriculum Structure)
-- ------------------------------------------

-- ------------------------------------------
-- CURRICULUM MANAGEMENT SYSTEM
-- ------------------------------------------

-- The abstract program, e.g., "B.S. in Software Engineering"
CREATE TABLE curriculum_programs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    program_name TEXT NOT NULL,
    program_code TEXT UNIQUE NOT NULL
);

-- The specific, versioned "snapshot" of a program, e.g., "K18A"
CREATE TABLE curriculum_versions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    program_id UUID NOT NULL REFERENCES curriculum_programs(id) ON DELETE CASCADE,
    version_tag TEXT NOT NULL, -- "K18A", "K18B", "2024-2025 Catalog"
    effective_year INTEGER NOT NULL,
    is_published BOOLEAN NOT NULL DEFAULT true, -- Is it visible to new students?
    UNIQUE (program_id, version_tag)
);

-- The master list of all subjects offered
CREATE TABLE subjects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    subject_code TEXT UNIQUE NOT NULL, -- "CS464"
    title TEXT NOT NULL, -- "Introduction to Machine Learning"
    credits INTEGER NOT NULL
);

-- The CRUCIAL junction table defining the ideal path for a curriculum version
CREATE TABLE curriculum_structure (
    curriculum_version_id UUID NOT NULL REFERENCES curriculum_versions(id) ON DELETE CASCADE,
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    prescribed_semester INTEGER NOT NULL, -- The semester this subject is *supposed* to be taken in (1, 2, 3...)
    PRIMARY KEY (curriculum_version_id, subject_id)
);

-- A table to handle versioned syllabi for each subject
CREATE TABLE syllabus_versions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    version_description TEXT NOT NULL, -- "Fall 2025 Syllabus"
    effective_date DATE NOT NULL,
    syllabus_content JSONB, -- The structured content of the syllabus
    file_url TEXT
);

-- Locks a student into a specific curriculum version
CREATE TABLE student_enrollments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    curriculum_version_id UUID NOT NULL REFERENCES curriculum_versions(id) ON DELETE CASCADE,
    start_date DATE NOT NULL,
    expected_graduation_date DATE
);

-- **KEY TABLE** - Tracks what a student is ACTUALLY taking each semester
CREATE TABLE student_term_subjects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    enrollment_id UUID NOT NULL REFERENCES student_enrollments(id) ON DELETE CASCADE,
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    academic_term TEXT NOT NULL, -- e.g., "Fall 2025", "Semester 3"
    status student_term_subject_status NOT NULL DEFAULT 'Enrolled', -- 'Enrolled', 'Completed', 'Failed', 'Withdrawn'
    final_grade TEXT, -- Optional
    is_retake BOOLEAN NOT NULL DEFAULT false
);

CREATE TABLE notes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    title TEXT NOT NULL,
    content JSONB,
    quest_id UUID,
    skill_id UUID,
    tags TEXT[],
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);


-- ------------------------------------------
-- SECTION 4: QUEST & SKILL MANAGEMENT (No Changes)
-- ------------------------------------------

CREATE TABLE quest_lines (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    class_id UUID NOT NULL REFERENCES classes(id) ON DELETE CASCADE,
    curriculum_version_id UUID NOT NULL REFERENCES curriculum_versions(id) ON DELETE CASCADE,
    title TEXT NOT NULL,
    description TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE quest_chapters (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    quest_line_id UUID NOT NULL REFERENCES quest_lines(id) ON DELETE CASCADE,
    title TEXT NOT NULL, -- e.g., "Semester 3: Week 5", "Finals Week"
    sequence INTEGER NOT NULL, -- 1, 2, 3... to order the chapters linearly
    status quest_status NOT NULL DEFAULT 'NotStarted', -- 'NotStarted', 'InProgress', 'Completed'
    start_date DATE,
    end_date DATE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (quest_line_id, sequence)
);

CREATE TABLE quests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    quest_chapter_id UUID NOT NULL REFERENCES quest_chapters(id) ON DELETE CASCADE,
    syllabus_version_id UUID NOT NULL REFERENCES syllabus_versions(id) ON DELETE CASCADE,
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    title TEXT NOT NULL,
    description TEXT,
    type quest_type NOT NULL,
    status quest_status NOT NULL DEFAULT 'NotStarted',
    due_date TIMESTAMPTZ,
    experience_points INTEGER NOT NULL DEFAULT 10,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE quest_prerequisites (
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    prerequisite_quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    PRIMARY KEY (quest_id, prerequisite_quest_id)
);

CREATE TABLE skill_trees (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    curriculum_version_id UUID NOT NULL REFERENCES curriculum_versions(id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE skills (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    skill_tree_id UUID NOT NULL REFERENCES skill_trees(id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    description TEXT,
    max_level INTEGER NOT NULL DEFAULT 10,
    position_x INTEGER,
    position_y INTEGER,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE user_skills (
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    level INTEGER NOT NULL DEFAULT 0,
    PRIMARY KEY (auth_user_id, skill_id)
);

CREATE TABLE user_skill_trees (
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    skill_tree_id UUID NOT NULL REFERENCES skill_trees(id) ON DELETE CASCADE,
    PRIMARY KEY (auth_user_id, skill_tree_id)
);

CREATE TABLE quest_skills (
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    PRIMARY KEY (quest_id, skill_id)
);

CREATE TABLE skill_dependencies (
    skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    prerequisite_skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,
    PRIMARY KEY (skill_id, prerequisite_skill_id)
);

-- Add foreign key constraints for notes table after dependencies are created
ALTER TABLE notes ADD CONSTRAINT fk_notes_quest_id FOREIGN KEY (quest_id) REFERENCES quests(id) ON DELETE SET NULL;
ALTER TABLE notes ADD CONSTRAINT fk_notes_skill_id FOREIGN KEY (skill_id) REFERENCES skills(id) ON DELETE SET NULL;


-- ------------------------------------------
-- SECTION 5: USER QUEST PROGRESS & ACHIEVEMENTS
-- ------------------------------------------

-- This table stores a summary of a user's progress on quests, providing a quick reference 
-- for the User Service without needing to constantly query the Quest Service.
CREATE TABLE user_quest_progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    quest_id UUID NOT NULL,
    status quest_status NOT NULL,
    completed_at TIMESTAMPTZ,
    last_updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (auth_user_id, quest_id)
);

-- This table serves as a central catalog of all possible achievements a user can earn.
CREATE TABLE achievements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL UNIQUE,
    description TEXT NOT NULL,
    icon_url TEXT,
    source_service VARCHAR(255) NOT NULL -- e.g., 'QuestsService', 'CodeBattleService'
);

-- This table links users to the achievements they have earned.
CREATE TABLE user_achievements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    achievement_id UUID NOT NULL REFERENCES achievements(id) ON DELETE CASCADE,
    earned_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    context JSONB -- To store additional details, like the event or quest that triggered the achievement
);


-- ------------------------------------------
-- SECTION 6: GAME SESSION & NOTIFICATIONS (No Changes)
-- ------------------------------------------

CREATE TABLE game_sessions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    quest_id UUID NOT NULL REFERENCES quests(id) ON DELETE CASCADE,
    status game_session_status NOT NULL DEFAULT 'InProgress',
    score INTEGER NOT NULL DEFAULT 0,
    progress_data JSONB,
    started_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    completed_at TIMESTAMPTZ
);

CREATE TABLE notifications (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    message TEXT NOT NULL,
    is_read BOOLEAN NOT NULL DEFAULT false,
    link TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);


-- ------------------------------------------
-- SECTION 7: SOCIAL & COMMUNITY (No Changes)
-- ------------------------------------------

CREATE TABLE parties (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    description TEXT,
    join_type party_join_type NOT NULL DEFAULT 'Invite Only',
    leader_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE party_memberships (
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    PRIMARY KEY (party_id, auth_user_id)
);

CREATE TABLE guilds (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    description TEXT,
    master_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    is_verified BOOLEAN NOT NULL DEFAULT false,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE guild_memberships (
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL UNIQUE REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    PRIMARY KEY (guild_id, auth_user_id)
);

-- This table stores shared notes in party stash, duplicating user notes for party collaboration
CREATE TABLE party_stash_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    original_note_id UUID NOT NULL REFERENCES notes(id) ON DELETE CASCADE,
    shared_by_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    title TEXT NOT NULL,
    content JSONB NOT NULL,
    syllabus_version_id UUID REFERENCES syllabus_versions(id) ON DELETE SET NULL,
    quest_id UUID REFERENCES quests(id) ON DELETE SET NULL,
    skill_id UUID REFERENCES skills(id) ON DELETE SET NULL,
    tags TEXT[],
    shared_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE meetings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID REFERENCES parties(id) ON DELETE CASCADE, -- Nullable - for party meetings
    guild_id UUID REFERENCES guilds(id) ON DELETE CASCADE, -- Nullable - for guild meetings
    creator_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE SET NULL,
    title TEXT NOT NULL,
    description TEXT,
    scheduled_start_time TIMESTAMPTZ NOT NULL,
    scheduled_end_time TIMESTAMPTZ,
    status meeting_status NOT NULL DEFAULT 'Scheduled',
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    -- Ensure exactly one of party_id or guild_id is set
    CONSTRAINT meetings_context_check CHECK (
        (party_id IS NOT NULL AND guild_id IS NULL) OR 
        (party_id IS NULL AND guild_id IS NOT NULL)
    )
);

CREATE TABLE meeting_participants (
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    status participant_status NOT NULL DEFAULT 'Invited',
    PRIMARY KEY (meeting_id, auth_user_id)
);

CREATE TABLE meeting_agenda (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL UNIQUE REFERENCES meetings(id) ON DELETE CASCADE,
    topic TEXT NOT NULL,
    description TEXT,
    presenter_id UUID REFERENCES user_profiles(auth_user_id) ON DELETE SET NULL,
    sequence INTEGER NOT NULL,
    duration_minutes INTEGER
);

CREATE TABLE meeting_notes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL UNIQUE REFERENCES meetings(id) ON DELETE CASCADE,
    agenda_id UUID REFERENCES meeting_agenda(id) ON DELETE SET NULL,
    content JSONB NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE meeting_participant_agenda_access (
    participant_id UUID NOT NULL,
    meeting_id UUID NOT NULL,
    agenda_id UUID NOT NULL REFERENCES meeting_agenda(id) ON DELETE CASCADE,
    PRIMARY KEY (participant_id, meeting_id, agenda_id),
    FOREIGN KEY (participant_id, meeting_id) REFERENCES meeting_participants(auth_user_id, meeting_id) ON DELETE CASCADE
);

-- This table tracks detailed participant activity during meetings including check-in/check-out times
CREATE TABLE meeting_participant_activity (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    check_in_time TIMESTAMPTZ NOT NULL DEFAULT now(),
    check_out_time TIMESTAMPTZ,
    activity_type VARCHAR(50) NOT NULL, -- 'join', 'leave', 'reconnect', 'disconnect'
    device_info JSONB, -- Browser, OS, device details
    ip_address INET,
    location TEXT, -- Geographic location if available
    connection_quality VARCHAR(20), -- 'excellent', 'good', 'fair', 'poor'
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    FOREIGN KEY (meeting_id, auth_user_id) REFERENCES meeting_participants(meeting_id, auth_user_id) ON DELETE CASCADE
);

-- This table tracks participant engagement and interactions during meetings
CREATE TABLE meeting_participant_engagement (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    agenda_id UUID REFERENCES meeting_agenda(id) ON DELETE SET NULL,
    engagement_type VARCHAR(50) NOT NULL, -- 'spoke', 'shared_screen', 'chat_message', 'reaction', 'raised_hand'
    duration INTEGER, -- Duration in seconds for activities like speaking or screen sharing
    content JSONB, -- Additional context like chat message content, reaction type, etc.
    timestamp TIMESTAMPTZ NOT NULL DEFAULT now(),
    FOREIGN KEY (meeting_id, auth_user_id) REFERENCES meeting_participants(meeting_id, auth_user_id) ON DELETE CASCADE
);

-- This table provides aggregated statistics for participant performance in meetings
CREATE TABLE meeting_participant_stats (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    total_attendance_minutes INTEGER NOT NULL DEFAULT 0,
    speaking_time_minutes INTEGER NOT NULL DEFAULT 0,
    screen_share_time_minutes INTEGER NOT NULL DEFAULT 0,
    chat_messages_count INTEGER NOT NULL DEFAULT 0,
    reactions_count INTEGER NOT NULL DEFAULT 0,
    hand_raises_count INTEGER NOT NULL DEFAULT 0,
    connection_issues_count INTEGER NOT NULL DEFAULT 0,
    engagement_score DECIMAL(5,2), -- Calculated engagement score (0-100)
    last_updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (meeting_id, auth_user_id),
    FOREIGN KEY (meeting_id, auth_user_id) REFERENCES meeting_participants(meeting_id, auth_user_id) ON DELETE CASCADE
);


-- ------------------------------------------
-- SECTION 8: EVENTS & COMPETITION (Modified)
-- ------------------------------------------

CREATE TABLE events (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title TEXT NOT NULL,
    description TEXT,
    type event_type NOT NULL,
    start_date TIMESTAMPTZ,
    end_date TIMESTAMPTZ,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Junction table for many-to-many relationship between Events and Guilds
-- This allows multiple guilds to participate in the same event
CREATE TABLE event_guild_participants (
    event_id UUID NOT NULL REFERENCES events(id) ON DELETE CASCADE,
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    PRIMARY KEY (event_id, guild_id)
);

-- This table now serves as a canonical reference for a problem's text.
-- The actual question content (template, etc.) is in Qdrant.
CREATE TABLE code_problems (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title TEXT NOT NULL,
    problem_statement TEXT NOT NULL,
    language_id UUID NOT NULL REFERENCES languages(id) ON DELETE RESTRICT
);

CREATE TABLE event_code_problems (
    event_id UUID NOT NULL REFERENCES events(id) ON DELETE CASCADE,
    problem_id UUID NOT NULL REFERENCES code_problems(id) ON DELETE CASCADE,
    PRIMARY KEY (event_id, problem_id)
);

CREATE TABLE leaderboard_entries (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    event_id UUID REFERENCES events(id) ON DELETE CASCADE,
    rank INT NOT NULL,
    score BIGINT NOT NULL,
    snapshot_date DATE NOT NULL
);

CREATE TABLE guild_leaderboard_entries (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    event_id UUID REFERENCES events(id) ON DELETE CASCADE,
    rank INT NOT NULL,
    total_score BIGINT NOT NULL,
    snapshot_date DATE NOT NULL
);


-- ------------------------------------------
-- SECTION 9: REAL-TIME CODE BATTLES & JUDGING (FULLY MERGED & DETAILED)
-- ------------------------------------------

CREATE TABLE languages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL UNIQUE,
    compile_cmd TEXT,
    run_cmd TEXT NOT NULL,
    timeout_seconds DOUBLE PRECISION
);

CREATE TABLE rooms (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    event_id UUID NOT NULL REFERENCES events(id) ON DELETE CASCADE,
    name TEXT NOT NULL,
    description TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE room_players (
    room_id UUID NOT NULL REFERENCES rooms(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    score INTEGER NOT NULL DEFAULT 0,
    place INTEGER,
    state TEXT,
    disconnected_at TIMESTAMPTZ,
    PRIMARY KEY (room_id, auth_user_id)
);

CREATE TABLE test_cases (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    code_problem_id UUID NOT NULL REFERENCES code_problems(id) ON DELETE CASCADE,
    language_id UUID NOT NULL REFERENCES languages(id) ON DELETE RESTRICT,
    input TEXT NOT NULL,
    expected_output TEXT NOT NULL,
    time_constraint DOUBLE PRECISION,
    space_constraint INTEGER
);

-- This fully-featured table replaces the original "code_submissions"
-- It contains all fields necessary for a detailed, external judging service.
CREATE TABLE submissions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    event_id UUID NOT NULL REFERENCES events(id) ON DELETE CASCADE,
    code_problem_id UUID NOT NULL REFERENCES code_problems(id) ON DELETE CASCADE,
    language_id UUID NOT NULL REFERENCES languages(id) ON DELETE RESTRICT,
    source_code TEXT NOT NULL,
    status submission_status NOT NULL DEFAULT 'Pending',
    
    -- Execution Results
    std_out TEXT,
    std_err TEXT,
    compile_output TEXT,
    exit_code INTEGER,
    exit_signal INTEGER,
    message TEXT, -- Human-readable message from the judge
    
    -- Performance Metrics
    time NUMERIC, -- Execution time in seconds
    memory INTEGER, -- Memory used in kilobytes
    wall_time NUMERIC, -- Wall clock time in seconds

    -- Judge Tracking & Configuration
    token TEXT UNIQUE, -- Unique token to track with the judge (e.g., Judge0)
    callback_url TEXT,
    number_of_runs INTEGER,
    compiler_options TEXT,
    command_line_arguments TEXT,
    redirect_std_err_to_std_out BOOLEAN,
    additional_files BYTEA,
    enable_network BOOLEAN,

    -- Judge Resource Limits
    cpu_time_limit NUMERIC,
    cpu_extra_time NUMERIC,
    wall_time_limit NUMERIC,
    memory_limit INTEGER,
    stack_limit INTEGER,
    max_processes_and_or_threads INTEGER,
    enable_per_process_and_thread_time_limit BOOLEAN,
    enable_per_process_and_thread_memory_limit BOOLEAN,
    max_file_size INTEGER,
    
    -- Timestamps & Host Info
    queued_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    started_at TIMESTAMPTZ,
    finished_at TIMESTAMPTZ,
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    queue_host TEXT,
    execution_host TEXT
);


-- ------------------------------------------
-- SECTION 10: INDEXES FOR PERFORMANCE (Updated)
-- ------------------------------------------
CREATE INDEX idx_user_profiles_email ON user_profiles(email);
CREATE INDEX idx_notes_user_id ON notes(auth_user_id);
CREATE INDEX idx_party_stash_items_party_id ON party_stash_items(party_id);
CREATE INDEX idx_party_stash_items_shared_by_user_id ON party_stash_items(shared_by_user_id);
CREATE INDEX idx_party_stash_items_original_note_id ON party_stash_items(original_note_id);
CREATE INDEX idx_party_stash_items_shared_at ON party_stash_items(shared_at);
CREATE INDEX idx_quest_lines_user_class ON quest_lines(auth_user_id, class_id);
CREATE INDEX idx_quest_lines_curriculum_version ON quest_lines(curriculum_version_id);
CREATE INDEX idx_quest_chapters_quest_line_id ON quest_chapters(quest_line_id);
CREATE INDEX idx_quest_chapters_sequence ON quest_chapters(quest_line_id, sequence);
CREATE INDEX idx_quest_chapters_status ON quest_chapters(status);
CREATE INDEX idx_quests_chapter_id ON quests(quest_chapter_id);
CREATE INDEX idx_quests_subject_id ON quests(subject_id);
CREATE INDEX idx_quests_syllabus_version_id ON quests(syllabus_version_id);
CREATE INDEX idx_skills_skill_tree_id ON skills(skill_tree_id);
CREATE INDEX idx_user_skills_user_id ON user_skills(auth_user_id);
CREATE INDEX idx_game_sessions_user_id ON game_sessions(auth_user_id);
CREATE INDEX idx_guild_memberships_user_id ON guild_memberships(auth_user_id);
CREATE INDEX idx_meetings_party_id ON meetings(party_id);
CREATE INDEX idx_meetings_guild_id ON meetings(guild_id);
CREATE INDEX idx_meeting_participants_user_id ON meeting_participants(auth_user_id);
CREATE INDEX idx_meeting_agenda_meeting_id ON meeting_agenda(meeting_id);
CREATE INDEX idx_meeting_notes_meeting_id ON meeting_notes(meeting_id);
CREATE INDEX idx_rooms_event_id ON rooms(event_id);
CREATE INDEX idx_room_players_user_id ON room_players(auth_user_id);
CREATE INDEX idx_test_cases_code_problem_id ON test_cases(code_problem_id);
CREATE INDEX idx_submissions_user_event ON submissions(auth_user_id, event_id);
CREATE INDEX idx_submissions_problem_id ON submissions(code_problem_id);
CREATE INDEX idx_submissions_token ON submissions(token);
CREATE INDEX idx_user_quest_progress_user_id ON user_quest_progress(auth_user_id);
CREATE INDEX idx_user_achievements_user_id ON user_achievements(auth_user_id);
-- Event and Guild participation indexes
CREATE INDEX idx_event_guild_participants_event_id ON event_guild_participants(event_id);
CREATE INDEX idx_event_guild_participants_guild_id ON event_guild_participants(guild_id);
-- Meeting participant activity tracking indexes
CREATE INDEX idx_meeting_participant_activity_meeting_id ON meeting_participant_activity(meeting_id);
CREATE INDEX idx_meeting_participant_activity_user_id ON meeting_participant_activity(auth_user_id);
CREATE INDEX idx_meeting_participant_activity_check_in_time ON meeting_participant_activity(check_in_time);
CREATE INDEX idx_meeting_participant_engagement_meeting_id ON meeting_participant_engagement(meeting_id);
CREATE INDEX idx_meeting_participant_engagement_user_id ON meeting_participant_engagement(auth_user_id);
CREATE INDEX idx_meeting_participant_engagement_timestamp ON meeting_participant_engagement(timestamp);
CREATE INDEX idx_meeting_participant_stats_meeting_id ON meeting_participant_stats(meeting_id);
CREATE INDEX idx_meeting_participant_stats_user_id ON meeting_participant_stats(auth_user_id);
-- Enhanced curriculum system indexes
CREATE INDEX idx_curriculum_versions_program_id ON curriculum_versions(program_id);
CREATE INDEX idx_curriculum_versions_effective_year ON curriculum_versions(effective_year);
CREATE INDEX idx_curriculum_structure_version_id ON curriculum_structure(curriculum_version_id);
CREATE INDEX idx_curriculum_structure_subject_id ON curriculum_structure(subject_id);
CREATE INDEX idx_curriculum_structure_semester ON curriculum_structure(prescribed_semester);
CREATE INDEX idx_syllabus_versions_subject_id ON syllabus_versions(subject_id);
CREATE INDEX idx_syllabus_versions_effective_date ON syllabus_versions(effective_date);
CREATE INDEX idx_student_enrollments_user_id ON student_enrollments(auth_user_id);
CREATE INDEX idx_student_enrollments_curriculum_version_id ON student_enrollments(curriculum_version_id);
CREATE INDEX idx_student_term_subjects_enrollment_id ON student_term_subjects(enrollment_id);
CREATE INDEX idx_student_term_subjects_subject_id ON student_term_subjects(subject_id);
CREATE INDEX idx_student_term_subjects_term ON student_term_subjects(academic_term);
CREATE INDEX idx_student_term_subjects_status ON student_term_subjects(status);
```

