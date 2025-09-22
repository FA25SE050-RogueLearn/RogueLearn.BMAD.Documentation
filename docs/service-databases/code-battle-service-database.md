# Code Battle Service Database Schema

## Overview
The Code Battle Service manages competitive programming challenges, coding tournaments, and skill-based coding assessments. It provides a comprehensive platform for code competitions, automated testing, and performance analytics.

## Database Tables

### Programming Languages and Environment

#### languages
Supported programming languages with execution configuration
```sql
CREATE TABLE public.languages (
  id UUID NOT NULL DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  compile_cmd TEXT NOT NULL,
  run_cmd TEXT NOT NULL,
  CONSTRAINT languages_pkey PRIMARY KEY (id)
);
```

#### tags
Available tags for categorizing problems
```sql
CREATE TABLE public.tags (
  id UUID NOT NULL DEFAULT gen_random_uuid(),
  name TEXT NOT NULL DEFAULT ''::TEXT UNIQUE,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  CONSTRAINT tags_pkey PRIMARY KEY (id)
);
```

#### code_problems
Coding problems and challenges
```sql
CREATE TABLE public.code_problems (
  id UUID NOT NULL DEFAULT gen_random_uuid(),
  title TEXT NOT NULL DEFAULT ''::TEXT,
  problem_statement TEXT NOT NULL DEFAULT ''::TEXT,
  difficulty INTEGER NOT NULL DEFAULT 1,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  CONSTRAINT code_problems_pkey PRIMARY KEY (id)
);
```

#### code_problem_tags
Many-to-many relationship between problems and tags
```sql
CREATE TABLE public.code_problem_tags (
  code_problem_id UUID NOT NULL DEFAULT gen_random_uuid(),
  tag_id UUID NOT NULL DEFAULT gen_random_uuid(),
  CONSTRAINT code_problem_tags_pkey PRIMARY KEY (code_problem_id, tag_id),
  CONSTRAINT code_problem_tags_code_problem_id_fkey FOREIGN KEY (code_problem_id) REFERENCES public.code_problems(id),
  CONSTRAINT code_problem_tags_tag_id_fkey FOREIGN KEY (tag_id) REFERENCES public.tags(id)
);
```

#### code_problem_language_details
Language-specific starter code and templates for problems
```sql
CREATE TABLE public.code_problem_language_details (
  code_problem_id UUID NOT NULL,
  language_id UUID NOT NULL,
  solution_stub TEXT NOT NULL DEFAULT ''::TEXT,
  driver_code TEXT NOT NULL DEFAULT ''::TEXT,
  time_constraint_ms INTEGER NOT NULL DEFAULT 1000,
  space_constraint_mb INTEGER NOT NULL DEFAULT 16,
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
  id UUID NOT NULL DEFAULT gen_random_uuid(),
  title TEXT NOT NULL DEFAULT ''::TEXT,
  description TEXT NOT NULL DEFAULT ''::TEXT,
  type TEXT NOT NULL,
  started_date TIMESTAMPTZ NOT NULL DEFAULT now(),
  end_date TIMESTAMPTZ NOT NULL DEFAULT now(),
  CONSTRAINT events_pkey PRIMARY KEY (id)
);
```

#### event_code_problems
Problems assigned to specific events
```sql
CREATE TABLE public.event_code_problems (
  event_id UUID NOT NULL,
  code_problem_id UUID NOT NULL,
  CONSTRAINT event_code_problems_pkey PRIMARY KEY (event_id, code_problem_id),
  CONSTRAINT event_code_problems_event_id_fkey FOREIGN KEY (event_id) REFERENCES public.events(id),
  CONSTRAINT event_code_problems_code_problem_id_fkey FOREIGN KEY (code_problem_id) REFERENCES public.code_problems(id)
);
```

#### event_guild_participants
Guild participation in events
```sql
CREATE TABLE public.event_guild_participants (
  event_id UUID NOT NULL,
  guild_id UUID NOT NULL,
  joined_at TIMESTAMPTZ DEFAULT now(),
  CONSTRAINT event_guild_participants_pkey PRIMARY KEY (event_id, guild_id),
  CONSTRAINT event_guild_participants_event_id_fkey FOREIGN KEY (event_id) REFERENCES public.events(id)
);
```

#### guild_leaderboard_entries
Guild-based leaderboard entries
```sql
CREATE TABLE public.guild_leaderboard_entries (
  id UUID NOT NULL DEFAULT gen_random_uuid(),
  guild_id UUID NOT NULL,
  event_id UUID NOT NULL,
  rank INTEGER NOT NULL,
  total_score INTEGER NOT NULL,
  snapshot_date TIMESTAMPTZ NOT NULL DEFAULT now(),
  CONSTRAINT guild_leaderboard_entries_pkey PRIMARY KEY (id),
  CONSTRAINT guild_leaderboard_entries_event_id_fkey FOREIGN KEY (event_id) REFERENCES public.events(id)
);
```

### Competition Management

#### rooms
Competition rooms and coding battle environments
```sql
CREATE TABLE public.rooms (
  id UUID NOT NULL DEFAULT gen_random_uuid(),
  event_id UUID NOT NULL,
  name TEXT NOT NULL DEFAULT ''::TEXT,
  description TEXT NOT NULL DEFAULT ''::TEXT,
  created_date TIMESTAMPTZ NOT NULL DEFAULT now(),
  CONSTRAINT rooms_pkey PRIMARY KEY (id),
  CONSTRAINT Rooms_event_id_fkey FOREIGN KEY (event_id) REFERENCES public.events(id)
);
```

#### room_players
Player participation in competition rooms
```sql
CREATE TABLE public.room_players (
  room_id UUID NOT NULL,
  user_id UUID NOT NULL,
  score INTEGER NOT NULL DEFAULT 0,
  place INTEGER,
  state TEXT DEFAULT ''::TEXT,
  disconnected_at TIMESTAMPTZ,
  CONSTRAINT room_players_pkey PRIMARY KEY (room_id, user_id),
  CONSTRAINT room_players_room_id_fkey FOREIGN KEY (room_id) REFERENCES public.rooms(id)
);
```

### Problem Management

#### test_cases
Test cases for problem validation
```sql
CREATE TABLE public.test_cases (
  id UUID NOT NULL DEFAULT gen_random_uuid(),
  code_problem_id UUID NOT NULL,
  input JSON NOT NULL,
  expected_output JSON NOT NULL,
  is_hidden BOOLEAN NOT NULL DEFAULT true,
  CONSTRAINT test_cases_pkey PRIMARY KEY (id),
  CONSTRAINT test_cases_code_problem_id_fkey FOREIGN KEY (code_problem_id) REFERENCES public.code_problems(id)
);
```

#### submissions
Code submissions from participants
```sql
CREATE TABLE public.submissions (
  id UUID NOT NULL DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL,
  code_problem_id UUID NOT NULL,
  language_id UUID NOT NULL,
  room_id UUID NOT NULL,
  code_submitted TEXT NOT NULL DEFAULT ''::TEXT,
  status TEXT NOT NULL,
  execution_time_ms INTEGER,
  submitted_at TIMESTAMPTZ NOT NULL DEFAULT now(),
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
  id UUID NOT NULL DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL,
  event_id UUID NOT NULL,
  rank INTEGER NOT NULL,
  score INTEGER NOT NULL,
  snapshot_date TIMESTAMPTZ NOT NULL DEFAULT now(),
  CONSTRAINT leaderboard_entries_pkey PRIMARY KEY (id),
  CONSTRAINT leaderboard_entries_event_id_fkey FOREIGN KEY (event_id) REFERENCES public.events(id)
);
```

### Event and Tournament Management

#### events
Events and competitions
```sql
CREATE TABLE events (
  id UUID NOT NULL DEFAULT gen_random_uuid(),
  title TEXT NOT NULL DEFAULT ''::TEXT,
  description TEXT NOT NULL DEFAULT ''::TEXT,
  type TEXT NOT NULL,
  started_date TIMESTAMPTZ NOT NULL DEFAULT now(),
  end_date TIMESTAMPTZ NOT NULL DEFAULT now(),
  CONSTRAINT events_pkey PRIMARY KEY (id)
);
```

#### event_code_problems
Problems assigned to events
```sql
CREATE TABLE event_code_problems (
    event_id UUID NOT NULL,
    code_problem_id UUID NOT NULL,
    CONSTRAINT event_code_problems_pkey PRIMARY KEY (event_id, code_problem_id),
    CONSTRAINT event_code_problems_event_id_fkey FOREIGN KEY (event_id) REFERENCES events(id) ON DELETE CASCADE,
    CONSTRAINT event_code_problems_code_problem_id_fkey FOREIGN KEY (code_problem_id) REFERENCES code_problems(id) ON DELETE CASCADE
);
```

#### event_guild_participants
Guild participation in events
```sql
CREATE TABLE event_guild_participants (
    event_id UUID NOT NULL,
    guild_id UUID NOT NULL,
    joined_at TIMESTAMPTZ DEFAULT now(),
    CONSTRAINT event_guild_participants_pkey PRIMARY KEY (event_id, guild_id),
    CONSTRAINT event_guild_participants_event_id_fkey FOREIGN KEY (event_id) REFERENCES events(id) ON DELETE CASCADE
);
```

#### guild_leaderboard_entries
Guild leaderboard standings
```sql
CREATE TABLE guild_leaderboard_entries (
    id UUID NOT NULL DEFAULT gen_random_uuid(),
    guild_id UUID NOT NULL,
    event_id UUID NOT NULL,
    rank INTEGER NOT NULL,
    total_score INTEGER NOT NULL,
    snapshot_date TIMESTAMPTZ NOT NULL DEFAULT now(),
    CONSTRAINT guild_leaderboard_entries_pkey PRIMARY KEY (id),
    CONSTRAINT guild_leaderboard_entries_event_id_fkey FOREIGN KEY (event_id) REFERENCES events(id) ON DELETE CASCADE
);
```

## Enums and Types

Based on the simplified schema, most enum types are no longer needed since the script uses simple TEXT fields instead of custom types. The events table uses TEXT type for event types to maintain simplicity.

```sql
-- Event types are stored as TEXT values for simplicity
-- Common values might include: 'tournament', 'practice', 'guild_battle', etc.
```

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
CREATE INDEX idx_leaderboard_entries_created_at ON leaderboard_entries(created_at);

-- Guild leaderboard entries indexes
CREATE INDEX idx_guild_leaderboard_entries_guild_id ON guild_leaderboard_entries(guild_id);
CREATE INDEX idx_guild_leaderboard_entries_event_id ON guild_leaderboard_entries(event_id);
CREATE INDEX idx_guild_leaderboard_entries_rank ON guild_leaderboard_entries(rank);
CREATE INDEX idx_guild_leaderboard_entries_created_at ON guild_leaderboard_entries(created_at);

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