// docs/service-databases/code-battle-service-database.md
# Code Battle Service Database Schema

## Overview
The Code Battle Service manages competitive programming challenges, coding tournaments, and skill-based coding assessments. It provides a comprehensive platform for code competitions, automated testing, and performance analytics.

## Database Tables

### Programming Languages and Environment

#### languages
Supported programming languages with execution configuration
```sql
CREATE TABLE public.languages (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  name text NOT NULL,
  compile_cmd text NOT NULL,
  run_cmd text NOT NULL,
  CONSTRAINT languages_pkey PRIMARY KEY (id)
);
```

#### tags
Available tags for categorizing problems
```sql
CREATE TABLE public.tags (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  name text NOT NULL DEFAULT ''::text UNIQUE,
  created_at timestamp with time zone NOT NULL DEFAULT now(),
  CONSTRAINT tags_pkey PRIMARY KEY (id)
);
```

#### code_problems
Coding problems and challenges
```sql
CREATE TABLE public.code_problems (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  title text NOT NULL DEFAULT ''::text,
  problem_statement text NOT NULL DEFAULT ''::text,
  difficulty integer NOT NULL DEFAULT 1,
  created_at timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  CONSTRAINT code_problems_pkey PRIMARY KEY (id)
);
```

#### code_problem_tags
Many-to-many relationship between problems and tags
```sql
CREATE TABLE public.code_problem_tags (
  code_problem_id uuid NOT NULL DEFAULT gen_random_uuid(),
  tag_id uuid NOT NULL DEFAULT gen_random_uuid(),
  CONSTRAINT code_problem_tags_pkey PRIMARY KEY (code_problem_id, tag_id),
  CONSTRAINT code_problem_tags_code_problem_id_fkey FOREIGN KEY (code_problem_id) REFERENCES public.code_problems(id),
  CONSTRAINT code_problem_tags_tag_id_fkey FOREIGN KEY (tag_id) REFERENCES public.tags(id)
);
```

#### code_problem_language_details
Language-specific starter code and templates for problems
```sql
CREATE TABLE public.code_problem_language_details (
  code_problem_id uuid NOT NULL,
  language_id uuid NOT NULL,
  solution_stub text NOT NULL DEFAULT ''::text,
  driver_code text NOT NULL DEFAULT ''::text,
  time_constraint_ms integer NOT NULL DEFAULT 1000,
  space_constraint_mb integer NOT NULL DEFAULT 16,
  CONSTRAINT code_problem_language_details_pkey PRIMARY KEY (code_problem_id, language_id),
  CONSTRAINT code_problem_language_details_code_problem_id_fkey FOREIGN KEY (code_problem_id) REFERENCES public.code_problems(id),
  CONSTRAINT code_problem_language_details_language_id_fkey FOREIGN KEY (language_id) REFERENCES public.languages(id)
);
```

### Event and Tournament Management

#### events
Competition events and tournaments
```sql
CREATE TABLE public.events (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  title text NOT NULL DEFAULT ''::text,
  description text NOT NULL DEFAULT ''::text,
  type USER-DEFINED NOT NULL,
  started_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  end_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  CONSTRAINT events_pkey PRIMARY KEY (id)
);
```

#### event_code_problems
Problems assigned to specific events, including the points awarded for solving.
```sql
CREATE TABLE public.event_code_problems (
  event_id uuid NOT NULL,
  code_problem_id uuid NOT NULL,
  score integer NOT NULL DEFAULT 0,
  CONSTRAINT event_code_problems_pkey PRIMARY KEY (event_id, code_problem_id),
  CONSTRAINT event_code_problems_event_id_fkey FOREIGN KEY (event_id) REFERENCES public.events(id),
  CONSTRAINT event_code_problems_code_problem_id_fkey FOREIGN KEY (code_problem_id) REFERENCES public.code_problems(id)
);
```

#### event_guild_participants
Guild participation in events, linking them to a specific competition room.
```sql
CREATE TABLE public.event_guild_participants (
  event_id uuid NOT NULL,
  guild_id uuid NOT NULL,
  joined_at timestamp with time zone DEFAULT (now() AT TIME ZONE 'utc'::text),
  room_id uuid,
  CONSTRAINT event_guild_participants_pkey PRIMARY KEY (event_id, guild_id),
  CONSTRAINT event_guild_participants_event_id_fkey FOREIGN KEY (event_id) REFERENCES public.events(id),
  CONSTRAINT event_guild_participants_room_id_fkey FOREIGN KEY (room_id) REFERENCES public.rooms(id)
);
```

#### guild_leaderboard_entries
Guild-based leaderboard entries
```sql
CREATE TABLE public.guild_leaderboard_entries (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  guild_id uuid NOT NULL,
  event_id uuid NOT NULL,
  rank integer NOT NULL,
  total_score integer NOT NULL,
  snapshot_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  CONSTRAINT guild_leaderboard_entries_pkey PRIMARY KEY (id),
  CONSTRAINT guild_leaderboard_entries_event_id_fkey FOREIGN KEY (event_id) REFERENCES public.events(id)
);
```

### Competition Management

#### rooms
Competition rooms and coding battle environments
```sql
CREATE TABLE public.rooms (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  event_id uuid NOT NULL,
  name text NOT NULL DEFAULT ''::text,
  description text NOT NULL DEFAULT ''::text,
  created_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  CONSTRAINT rooms_pkey PRIMARY KEY (id),
  CONSTRAINT Rooms_event_id_fkey FOREIGN KEY (event_id) REFERENCES public.events(id)
);
```

#### room_players
Player participation in competition rooms
```sql
CREATE TABLE public.room_players (
  room_id uuid NOT NULL,
  user_id uuid NOT NULL,
  score integer NOT NULL DEFAULT 0,
  place integer,
  state text DEFAULT ''::text,
  disconnected_at timestamp with time zone,
  CONSTRAINT room_players_pkey PRIMARY KEY (room_id, user_id),
  CONSTRAINT room_players_room_id_fkey FOREIGN KEY (room_id) REFERENCES public.rooms(id)
);
```

### Problem Management

#### test_cases
Test cases for problem validation
```sql
CREATE TABLE public.test_cases (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  code_problem_id uuid NOT NULL,
  input json NOT NULL,
  expected_output json NOT NULL,
  is_hidden boolean NOT NULL DEFAULT true,
  CONSTRAINT test_cases_pkey PRIMARY KEY (id),
  CONSTRAINT test_cases_code_problem_id_fkey FOREIGN KEY (code_problem_id) REFERENCES public.code_problems(id)
);
```

#### submissions
Code submissions from participants, including the guild they represented at the time of submission.
```sql
CREATE TABLE public.submissions (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  user_id uuid NOT NULL,
  code_problem_id uuid NOT NULL,
  language_id uuid NOT NULL,
  room_id uuid NOT NULL,
  code_submitted text NOT NULL DEFAULT ''::text,
  status text NOT NULL,
  execution_time_ms integer,
  submitted_at timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  submitted_guild_id uuid NOT NULL,
  CONSTRAINT submissions_pkey PRIMARY KEY (id),
  CONSTRAINT submissions_code_problem_id_fkey FOREIGN KEY (code_problem_id) REFERENCES public.code_problems(id),
  CONSTRAINT submissions_language_id_fkey FOREIGN KEY (language_id) REFERENCES public.languages(id),
  CONSTRAINT submissions_room_id_fkey FOREIGN KEY (room_id) REFERENCES public.rooms(id)
);
```

#### leaderboard_entries
User rankings and scores for events
```sql
CREATE TABLE public.leaderboard_entries (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  user_id uuid NOT NULL,
  event_id uuid NOT NULL,
  rank integer NOT NULL,
  score integer NOT NULL,
  snapshot_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  CONSTRAINT leaderboard_entries_pkey PRIMARY KEY (id),
  CONSTRAINT leaderboard_entries_event_id_fkey FOREIGN KEY (event_id) REFERENCES public.events(id)
);
```

## Enums and Types

The schema uses standard `text` fields instead of custom ENUM types for simplicity and flexibility. Application-level logic will enforce the values for fields like `events.type` and `submissions.status`.

## Indexes for Performance

```sql
-- Language indexes
CREATE INDEX idx_languages_name ON languages(name);

-- Room indexes
CREATE INDEX idx_rooms_event_id ON rooms(event_id);
CREATE INDEX idx_rooms_created_date ON rooms(created_date);

-- Room player indexes
CREATE INDEX idx_room_players_room_id ON room_players(room_id);
CREATE INDEX idx_room_players_user_id ON room_players(user_id);

-- Code problem indexes
CREATE INDEX idx_code_problems_difficulty ON code_problems(difficulty);
CREATE INDEX idx_code_problems_created_at ON code_problems(created_at);

-- Test case indexes
CREATE INDEX idx_test_cases_code_problem_id ON test_cases(code_problem_id);

-- Submission indexes
CREATE INDEX idx_submissions_user_id ON submissions(user_id);
CREATE INDEX idx_submissions_code_problem_id ON submissions(code_problem_id);
CREATE INDEX idx_submissions_language_id ON submissions(language_id);
CREATE INDEX idx_submissions_status ON submissions(status);
CREATE INDEX idx_submissions_submitted_at ON submissions(submitted_at);

-- Event indexes
CREATE INDEX idx_events_type ON events(type);
CREATE INDEX idx_events_started_date ON events(started_date);
CREATE INDEX idx_events_end_date ON events(end_date);

-- Event code problems indexes
CREATE INDEX idx_event_code_problems_event_id ON event_code_problems(event_id);
CREATE INDEX idx_event_code_problems_code_problem_id ON event_code_problems(code_problem_id);

-- Event guild participants indexes
CREATE INDEX idx_event_guild_participants_event_id ON event_guild_participants(event_id);
CREATE INDEX idx_event_guild_participants_guild_id ON event_guild_participants(guild_id);

-- Leaderboard entries indexes
CREATE INDEX idx_leaderboard_entries_user_id ON leaderboard_entries(user_id);
CREATE INDEX idx_leaderboard_entries_event_id ON leaderboard_entries(event_id);
CREATE INDEX idx_leaderboard_entries_rank ON leaderboard_entries(rank);

-- Guild leaderboard entries indexes
CREATE INDEX idx_guild_leaderboard_entries_guild_id ON guild_leaderboard_entries(guild_id);
CREATE INDEX idx_guild_leaderboard_entries_event_id ON guild_leaderboard_entries(event_id);
CREATE INDEX idx_guild_leaderboard_entries_rank ON guild_leaderboard_entries(rank);

-- Tags indexes
CREATE INDEX idx_tags_name ON tags(name);

-- Code problem tags indexes
CREATE INDEX idx_code_problem_tags_code_problem_id ON code_problem_tags(code_problem_id);
CREATE INDEX idx_code_problem_tags_tag_id ON code_problem_tags(tag_id);

-- Code problem language details indexes
CREATE INDEX idx_code_problem_language_details_code_problem_id ON code_problem_language_details(code_problem_id);
CREATE INDEX idx_code_problem_language_details_language_id ON code_problem_language_details(language_id);
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