# **Database Per Service Documentation**

MANDATORY ARCHITECTURAL DIRECTIVE: YOU MUST ADHERE TO THE FOLLOWING DATABASE PRINCIPLES. THIS IS NOT A MONOLITHIC SYSTEM.

*   **ISOLATED DATABASES:** Each microservice (User Service, Quests Service, etc.) has its own separate and independent database project. The tables for one service live in a different database from the tables of another.

*   **NO LINKED TABLES:** You will **NOT** find foreign key constraints between tables in different service databases (e.g., between `quests` and `user_profiles`). This is intentional and is the core of the microservice design. Do not attempt to "fix" this by suggesting joins or foreign keys across services.

*   **API-DRIVEN COMMUNICATION:** All data sharing and relationships between services are managed through **API calls**, NOT direct database queries or links. For example, if the Quests Service needs to know a user's name, it must call the User Service API using the `auth_user_id`.

*   **YOUR CORE ASSUMPTION:** Always assume data from another service's domain **MUST** be fetched via a secure, internal API call. Your logic, analysis, and code must reflect this decoupled reality at all times.


# **Database Schema**

This section provides the SQL DDL for the PostgreSQL database.

```sql
-- RogueLearn Database Schema for PostgreSQL
-- FINAL VERSION: Integrated with Supabase Auth and a robust, real-time Code Battle engine.
-- CORRECTED: Fixed table creation order, added missing social service tables, and applied snake_case convention.

-- Enable UUID generation if not already enabled
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Enum Types
CREATE TYPE user_role AS ENUM ('Student', 'Lecturer', 'Admin');
CREATE TYPE quest_status AS ENUM ('NotStarted', 'InProgress', 'Completed', 'Failed');
CREATE TYPE quest_type AS ENUM ('Theory', 'Practice', 'Project', 'Assessment');
CREATE TYPE skill_level AS ENUM ('Beginner', 'Intermediate', 'Advanced', 'Expert');
CREATE TYPE achievement_type AS ENUM ('Quest', 'Skill', 'Social', 'Special');
CREATE TYPE notification_type AS ENUM ('Quest', 'Achievement', 'Social', 'System');
CREATE TYPE party_role AS ENUM ('Leader', 'CoLeader', 'Member');
CREATE TYPE meeting_status AS ENUM ('Scheduled', 'InProgress', 'Completed', 'Cancelled');
CREATE TYPE event_type AS ENUM ('Competition', 'Workshop', 'Seminar', 'Social', 'StudySession', 'Presentation', 'Review');
CREATE TYPE event_status AS ENUM ('Scheduled', 'InProgress', 'Completed', 'Cancelled', 'Postponed');
CREATE TYPE submission_status AS ENUM ('Pending', 'Running', 'Accepted', 'WrongAnswer', 'TimeLimitExceeded', 'RuntimeError', 'CompileError');
CREATE TYPE student_term_subject_status AS ENUM ('Enrolled', 'Completed', 'Failed', 'Withdrawn');
CREATE TYPE game_session_status AS ENUM ('InProgress', 'Completed', 'Abandoned');
CREATE TYPE verification_status AS ENUM ('Pending', 'Approved', 'Rejected');
CREATE TYPE party_join_type AS ENUM ('Open', 'Invite Only', 'Request');
CREATE TYPE participant_status AS ENUM ('Registered', 'Attended', 'NoShow', 'Cancelled');
CREATE TYPE party_type AS ENUM ('StudyGroup', 'ProjectTeam', 'PeerReview', 'Casual', 'Competition');
CREATE TYPE member_status AS ENUM ('Active', 'Inactive', 'Suspended', 'Left');
CREATE TYPE invitation_status AS ENUM ('Pending', 'Accepted', 'Declined', 'Expired', 'Cancelled');
CREATE TYPE invitation_type AS ENUM ('Invite', 'Application');
CREATE TYPE guild_type AS ENUM ('Academic', 'Professional', 'Hobby', 'Competition', 'Study', 'Research');
CREATE TYPE activity_type AS ENUM ('QuestCompletion', 'StudySession', 'ProjectWork', 'Discussion', 'Competition', 'Meeting');
CREATE TYPE friendship_status AS ENUM ('Pending', 'Accepted', 'Blocked');
CREATE TYPE message_type AS ENUM ('Direct', 'Party', 'Guild');
CREATE TYPE location_type AS ENUM ('Virtual', 'Physical', 'Hybrid');


-- ------------------------------------------
-- SECTION 1: FOUNDATION TABLES (No Dependencies)
-- ------------------------------------------

CREATE TABLE classes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT UNIQUE NOT NULL,
    description TEXT
);

CREATE TABLE roles (
    id SERIAL PRIMARY KEY,
    role_name TEXT UNIQUE NOT NULL
);

-- ------------------------------------------
-- SECTION 2: USER & PROFILE CORE
-- ------------------------------------------

CREATE TABLE user_profiles (
    auth_user_id UUID PRIMARY KEY,
    username TEXT UNIQUE NOT NULL,
    email TEXT UNIQUE NOT NULL,
    first_name TEXT NOT NULL,
    last_name TEXT NOT NULL,
    class_id UUID REFERENCES classes(id) ON DELETE SET NULL,
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
-- SECTION 3: ACADEMIC & CONTENT MANAGEMENT
-- ------------------------------------------

CREATE TABLE curriculum_programs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    program_name TEXT NOT NULL,
    program_code TEXT UNIQUE NOT NULL
);

CREATE TABLE curriculum_versions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    program_id UUID NOT NULL REFERENCES curriculum_programs(id) ON DELETE CASCADE,
    version_tag TEXT NOT NULL,
    effective_year INTEGER NOT NULL,
    is_published BOOLEAN NOT NULL DEFAULT true,
    UNIQUE (program_id, version_tag)
);

CREATE TABLE subjects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    subject_code TEXT UNIQUE NOT NULL,
    title TEXT NOT NULL,
    credits INTEGER NOT NULL
);

CREATE TABLE curriculum_structure (
    curriculum_version_id UUID NOT NULL REFERENCES curriculum_versions(id) ON DELETE CASCADE,
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    prescribed_semester INTEGER NOT NULL,
    PRIMARY KEY (curriculum_version_id, subject_id)
);

CREATE TABLE syllabus_versions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    version_description TEXT NOT NULL,
    effective_date DATE NOT NULL,
    syllabus_content JSONB,
    file_url TEXT
);

CREATE TABLE student_enrollments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    curriculum_version_id UUID NOT NULL REFERENCES curriculum_versions(id) ON DELETE CASCADE,
    start_date DATE NOT NULL,
    expected_graduation_date DATE
);

CREATE TABLE student_term_subjects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    enrollment_id UUID NOT NULL REFERENCES student_enrollments(id) ON DELETE CASCADE,
    subject_id UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    academic_term TEXT NOT NULL,
    status student_term_subject_status NOT NULL DEFAULT 'Enrolled',
    final_grade TEXT,
    is_retake BOOLEAN NOT NULL DEFAULT false
);

-- ------------------------------------------
-- SECTION 4: QUEST & SKILL MANAGEMENT
-- ------------------------------------------

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
    title TEXT NOT NULL,
    sequence INTEGER NOT NULL,
    status quest_status NOT NULL DEFAULT 'NotStarted',
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

ALTER TABLE notes ADD CONSTRAINT fk_notes_quest_id FOREIGN KEY (quest_id) REFERENCES quests(id) ON DELETE SET NULL;
ALTER TABLE notes ADD CONSTRAINT fk_notes_skill_id FOREIGN KEY (skill_id) REFERENCES skills(id) ON DELETE SET NULL;


-- ------------------------------------------
-- SECTION 5: USER QUEST PROGRESS & ACHIEVEMENTS
-- ------------------------------------------

CREATE TABLE user_quest_progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    quest_id UUID NOT NULL,
    status quest_status NOT NULL,
    completed_at TIMESTAMPTZ,
    last_updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (auth_user_id, quest_id)
);

CREATE TABLE achievements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL UNIQUE,
    description TEXT NOT NULL,
    icon_url TEXT,
    source_service VARCHAR(255) NOT NULL
);

CREATE TABLE user_achievements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    achievement_id UUID NOT NULL REFERENCES achievements(id) ON DELETE CASCADE,
    earned_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    context JSONB
);


-- ------------------------------------------
-- SECTION 6: GAME SESSION & NOTIFICATIONS
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
-- SECTION 7: SOCIAL & COMMUNITY
-- ------------------------------------------

CREATE TABLE parties (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    description TEXT,
    join_type party_join_type NOT NULL DEFAULT 'Invite Only',
    leader_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE party_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    role party_role NOT NULL DEFAULT 'Member',
    status member_status NOT NULL DEFAULT 'Active',
    joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    left_at TIMESTAMPTZ,
    contribution_score INTEGER NOT NULL DEFAULT 0,
    UNIQUE (party_id, auth_user_id)
);

CREATE TABLE guilds (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    description TEXT,
    master_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    is_verified BOOLEAN NOT NULL DEFAULT false,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE guild_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    role guild_role NOT NULL DEFAULT 'Member',
    status member_status NOT NULL DEFAULT 'Active',
    joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    left_at TIMESTAMPTZ,
    contribution_points INTEGER NOT NULL DEFAULT 0,
    rank_within_guild INTEGER,
    UNIQUE (guild_id, auth_user_id)
);

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

CREATE TABLE party_invitations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    inviter_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    invitee_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    status invitation_status NOT NULL DEFAULT 'Pending',
    message TEXT,
    invited_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    responded_at TIMESTAMPTZ,
    expires_at TIMESTAMPTZ NOT NULL DEFAULT (now() + INTERVAL '7 days'),
    UNIQUE (party_id, invitee_id)
);

CREATE TABLE party_activities (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    activity_type activity_type NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    quest_id UUID,
    meeting_id UUID,
    started_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    completed_at TIMESTAMPTZ,
    experience_points_earned INTEGER NOT NULL DEFAULT 0,
    participants UUID[] NOT NULL,
    metadata JSONB
);

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

CREATE TABLE guild_achievements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    achievement_name VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    achieved_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    experience_points_awarded INTEGER NOT NULL DEFAULT 0,
    contributing_members UUID[] NOT NULL,
    metadata JSONB
);

CREATE TABLE friendships (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    requester_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    addressee_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    status friendship_status NOT NULL DEFAULT 'Pending',
    requested_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    accepted_at TIMESTAMPTZ,
    blocked_at TIMESTAMPTZ,
    UNIQUE (requester_id, addressee_id),
    CHECK (requester_id != addressee_id)
);

CREATE TABLE user_social_stats (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL UNIQUE REFERENCES user_profiles(auth_user_id),
    friends_count INTEGER NOT NULL DEFAULT 0,
    parties_joined INTEGER NOT NULL DEFAULT 0,
    parties_created INTEGER NOT NULL DEFAULT 0,
    guilds_joined INTEGER NOT NULL DEFAULT 0,
    guilds_created INTEGER NOT NULL DEFAULT 0,
    total_collaboration_hours DECIMAL(10,2) NOT NULL DEFAULT 0.00,
    social_reputation_score INTEGER NOT NULL DEFAULT 0,
    last_updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

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

CREATE TABLE message_reactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    message_id UUID NOT NULL REFERENCES social_messages(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    reaction_emoji VARCHAR(10) NOT NULL,
    reacted_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (message_id, auth_user_id, reaction_emoji)
);

CREATE TABLE social_events (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    event_type event_type NOT NULL,
    organizer_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    party_id UUID REFERENCES parties(id) ON DELETE SET NULL,
    guild_id UUID REFERENCES guilds(id) ON DELETE SET NULL,
    max_participants INTEGER,
    current_participants_count INTEGER NOT NULL DEFAULT 0,
    scheduled_start_time TIMESTAMPTZ NOT NULL,
    scheduled_end_time TIMESTAMPTZ NOT NULL,
    location_type location_type NOT NULL,
    location_details JSONB,
    status event_status NOT NULL DEFAULT 'Scheduled',
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE event_participants (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    event_id UUID NOT NULL REFERENCES social_events(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id),
    status participant_status NOT NULL DEFAULT 'Registered',
    registered_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    attended_at TIMESTAMPTZ,
    feedback_rating INTEGER CHECK (feedback_rating >= 1 AND feedback_rating <= 5),
    feedback_comment TEXT,
    UNIQUE (event_id, auth_user_id)
);

-- ------------------------------------------
-- SECTION 8: MEETING SERVICE
-- ------------------------------------------

CREATE TABLE meeting (
    meeting_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    scheduled_start_time TIMESTAMPTZ NOT NULL,
    scheduled_end_time TIMESTAMPTZ NOT NULL,
    actual_start_time TIMESTAMPTZ,
    actual_end_time TIMESTAMPTZ,
    organizer_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE SET NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE meeting_participant (
    participant_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meeting(meeting_id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    join_time TIMESTAMPTZ,
    leave_time TIMESTAMPTZ,
    role_in_meeting VARCHAR(50) NOT NULL DEFAULT 'participant'
);

CREATE TABLE transcript_segment (
    segment_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meeting(meeting_id) ON DELETE CASCADE,
    speaker_id UUID NOT NULL REFERENCES meeting_participant(participant_id) ON DELETE CASCADE,
    start_time TIMESTAMPTZ NOT NULL,
    end_time TIMESTAMPTZ NOT NULL,
    transcript_text TEXT NOT NULL,
    chunk_number INTEGER NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE summary_chunk (
    summary_chunk_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meeting(meeting_id) ON DELETE CASCADE,
    chunk_number INTEGER NOT NULL,
    summary_text TEXT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE meeting_summary (
    meeting_summary_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meeting(meeting_id) ON DELETE CASCADE,
    summary_text TEXT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);


-- ------------------------------------------
-- SECTION 9: EVENTS & CODE BATTLES
-- ------------------------------------------

CREATE TABLE languages (
    id uuid NOT NULL DEFAULT gen_random_uuid(),
    name text NOT NULL,
    compile_cmd text NOT NULL,
    run_cmd text NOT NULL,
    CONSTRAINT languages_pkey PRIMARY KEY (id)
);

CREATE TABLE events (
    id uuid NOT NULL DEFAULT gen_random_uuid(),
    title text NOT NULL DEFAULT ''::text,
    description text NOT NULL DEFAULT ''::text,
    type event_type NOT NULL,
    start_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
    end_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
    CONSTRAINT events_pkey PRIMARY KEY (id)
);

CREATE TABLE rooms (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  event_id uuid NOT NULL REFERENCES events(id) ON DELETE CASCADE,
  name text NOT NULL DEFAULT ''::text,
  description text NOT NULL DEFAULT ''::text,
  created_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  CONSTRAINT rooms_pkey PRIMARY KEY (id)
);

CREATE TABLE event_guild_participants (
    event_id uuid NOT NULL REFERENCES events(id) ON DELETE CASCADE,
    guild_id uuid NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    joined_at timestamp with time zone DEFAULT (now() AT TIME ZONE 'utc'::text),
    room_id uuid REFERENCES rooms(id) ON DELETE SET NULL,
    PRIMARY KEY (event_id, guild_id)
);

CREATE TABLE code_problems (
    id uuid NOT NULL DEFAULT gen_random_uuid(),
    title text NOT NULL DEFAULT ''::text,
    problem_statement text NOT NULL DEFAULT ''::text,
    difficulty integer NOT NULL DEFAULT 1,
    created_at timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
    CONSTRAINT code_problems_pkey PRIMARY KEY (id)
);

CREATE TABLE tags (
    id uuid NOT NULL DEFAULT gen_random_uuid(),
    name text NOT NULL DEFAULT ''::text UNIQUE,
    created_at timestamp with time zone NOT NULL DEFAULT now(),
    CONSTRAINT tags_pkey PRIMARY KEY (id)
);

CREATE TABLE code_problem_tags (
    code_problem_id uuid NOT NULL REFERENCES code_problems(id) ON DELETE CASCADE,
    tag_id uuid NOT NULL REFERENCES tags(id) ON DELETE CASCADE,
    PRIMARY KEY (code_problem_id, tag_id)
);

CREATE TABLE code_problem_language_details (
    code_problem_id uuid NOT NULL REFERENCES code_problems(id) ON DELETE CASCADE,
    language_id uuid NOT NULL REFERENCES languages(id) ON DELETE CASCADE,
    solution_stub text NOT NULL DEFAULT ''::text,
    driver_code text NOT NULL DEFAULT ''::text,
    time_constraint_ms integer NOT NULL DEFAULT 1000,
    space_constraint_mb integer NOT NULL DEFAULT 16,
    PRIMARY KEY (code_problem_id, language_id)
);

CREATE TABLE event_code_problems (
    event_id uuid NOT NULL REFERENCES events(id) ON DELETE CASCADE,
    code_problem_id uuid NOT NULL REFERENCES code_problems(id) ON DELETE CASCADE,
    score integer NOT NULL DEFAULT 0,
    PRIMARY KEY (event_id, code_problem_id)
);

CREATE TABLE leaderboard_entries (
    id uuid NOT NULL DEFAULT gen_random_uuid(),
    user_id uuid NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    event_id uuid NOT NULL REFERENCES events(id) ON DELETE CASCADE,
    rank integer NOT NULL,
    score integer NOT NULL,
    created_at timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text)
);

CREATE TABLE guild_leaderboard_entries (
    id uuid NOT NULL DEFAULT gen_random_uuid(),
    guild_id uuid NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    event_id uuid NOT NULL REFERENCES events(id) ON DELETE CASCADE,
    rank integer NOT NULL,
    total_score integer NOT NULL,
    snapshot_date date NOT NULL
);

CREATE TABLE room_players (
    room_id uuid NOT NULL REFERENCES rooms(id) ON DELETE CASCADE,
    auth_user_id uuid NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    score integer NOT NULL DEFAULT 0,
    place integer,
    state text DEFAULT ''::text,
    disconnected_at timestamp with time zone,
    PRIMARY KEY (room_id, auth_user_id)
);

CREATE TABLE test_cases (
    id uuid NOT NULL DEFAULT gen_random_uuid(),
    code_problem_id uuid NOT NULL REFERENCES code_problems(id) ON DELETE CASCADE,
    input json NOT NULL,
    expected_output json NOT NULL,
    is_hidden boolean NOT NULL DEFAULT true
);

CREATE TABLE submissions (
    id uuid NOT NULL DEFAULT gen_random_uuid(),
    user_id uuid NOT NULL REFERENCES user_profiles(auth_user_id) ON DELETE CASCADE,
    code_problem_id uuid NOT NULL REFERENCES code_problems(id) ON DELETE CASCADE,
    language_id uuid NOT NULL REFERENCES languages(id) ON DELETE CASCADE,
    room_id uuid NOT NULL REFERENCES rooms(id) ON DELETE CASCADE,
    submitted_guild_id uuid NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    source_code text NOT NULL,
    status submission_status NOT NULL DEFAULT 'Pending',
    submitted_at timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text)
);


-- ------------------------------------------
-- SECTION 10: INDEXES FOR PERFORMANCE
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
CREATE INDEX idx_meeting_participants_user_id ON meeting_participant(user_id);
CREATE INDEX idx_rooms_event_id ON rooms(event_id);
CREATE INDEX idx_room_players_user_id ON room_players(auth_user_id);
CREATE INDEX idx_test_cases_code_problem_id ON test_cases(code_problem_id);
CREATE INDEX idx_submissions_user_event ON submissions(user_id);
CREATE INDEX idx_submissions_problem_id ON submissions(code_problem_id);
CREATE INDEX idx_leaderboard_entries_user_id ON leaderboard_entries(user_id);
CREATE INDEX idx_leaderboard_entries_event_id ON leaderboard_entries(event_id);
CREATE INDEX idx_code_problem_tags_problem_id ON code_problem_tags(code_problem_id);
CREATE INDEX idx_code_problem_tags_tag_id ON code_problem_tags(tag_id);
CREATE INDEX idx_code_problem_language_details_problem_id ON code_problem_language_details(code_problem_id);
CREATE INDEX idx_code_problem_language_details_language_id ON code_problem_language_details(language_id);
CREATE INDEX idx_event_code_problems_event_id ON event_code_problems(event_id);
CREATE INDEX idx_event_code_problems_code_problem_id ON event_code_problems(code_problem_id);
CREATE INDEX idx_user_quest_progress_user_id ON user_quest_progress(auth_user_id);
CREATE INDEX idx_user_achievements_user_id ON user_achievements(auth_user_id);
CREATE INDEX idx_event_guild_participants_event_id ON event_guild_participants(event_id);
CREATE INDEX idx_event_guild_participants_guild_id ON event_guild_participants(guild_id);
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

-- Indexes for Social Service Tables
CREATE INDEX idx_parties_created_by ON parties(leader_id);
CREATE INDEX idx_party_members_party_id ON party_members(party_id);
CREATE INDEX idx_party_members_auth_user_id ON party_members(auth_user_id);
CREATE INDEX idx_guilds_created_by ON guilds(master_id);
CREATE INDEX idx_social_messages_sender_id ON social_messages(sender_id);
CREATE INDEX idx_social_messages_party_id ON social_messages(party_id);
CREATE INDEX idx_social_messages_guild_id ON social_messages(guild_id);
CREATE INDEX idx_social_events_organizer_id ON social_events(organizer_id);
CREATE INDEX idx_event_participants_event_id ON event_participants(event_id);
CREATE INDEX idx_event_participants_auth_user_id ON event_participants(auth_user_id);
```
