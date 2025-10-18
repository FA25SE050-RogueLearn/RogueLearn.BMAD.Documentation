# Event Service Database Schema (Consolidated)

## Overview
The Event Service is the central authority for creating, managing, and executing all competitive and social events on the RogueLearn platform. It orchestrates interactions between guilds (from Social Service) and owns the catalog of code problems, the submission engine, and all event-related entities like rooms and leaderboards. This schema is the new source of truth for all event-related data, consolidating the previous Code Battle Service.

## Enums and Types

```sql
CREATE TYPE event_type AS ENUM (
    'code_battle',
    'workshop',
    'seminar',
    'social'
);

CREATE TYPE event_request_status AS ENUM (
  'pending',
  'approved',
  'rejected'
);

CREATE TYPE room_player_state AS ENUM (
    'present',
    'disconnected',
    'left',
    'completed'
);

CREATE TYPE submission_status AS ENUM (
  'event_unspecified',
  'pending',
  'accepted',
  'wrong_answer',
  'limit_exceed',
  'runtime_error',
  'compilation_error'
);
```

## Database Tables

### Event & Request Management

#### event_requests
Stores requests from Guild Masters to create an event. These are reviewed by Game Masters.
```sql
CREATE TABLE public.event_requests (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  status event_request_status NOT NULL DEFAULT 'pending'::event_request_status,
  requester_guild_id uuid NOT NULL, -- Soft FK to Social Service guilds
  processed_by_admin_id uuid, -- Soft FK to User Service user_profiles
  created_at timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  processed_at timestamp with time zone,
  event_type event_type NOT NULL,
  title text NOT NULL,
  description text NOT NULL,
  proposed_start_date timestamp with time zone NOT NULL,
  proposed_end_date timestamp with time zone NOT NULL,
  notes text DEFAULT ''::text,
  participation_details jsonb NOT NULL,
  room_configuration jsonb NOT NULL,
  event_specifics jsonb,
  rejection_reason text,
  approved_event_id uuid,
  CONSTRAINT event_requests_pkey PRIMARY KEY (id),
  CONSTRAINT event_requests_approved_event_id_fkey FOREIGN KEY (approved_event_id) REFERENCES public.events(id)
);
```

#### events
Defines the core details of an event after it has been approved or created directly by an admin.
```sql
CREATE TABLE public.events (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  title text NOT NULL DEFAULT ''::text,
  description text NOT NULL DEFAULT ''::text,
  type event_type NOT NULL,
  started_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  end_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  max_guilds integer,
  max_players_per_guild integer,
  number_of_rooms integer,
  guilds_per_room integer,
  room_naming_prefix text,
  original_request_id uuid,
  CONSTRAINT events_pkey PRIMARY KEY (id),
  CONSTRAINT events_original_request_id_fkey FOREIGN KEY (original_request_id) REFERENCES public.event_requests(id)
);
```

### Problem & Language Catalog (Migrated from Code Battle Service)

#### languages
Supported programming languages and their execution configurations.
```sql
CREATE TABLE public.languages (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  name text NOT NULL,
  compile_cmd text NOT NULL,
  run_cmd text NOT NULL,
  temp_file_dir text,
  temp_file_name text,
  CONSTRAINT languages_pkey PRIMARY KEY (id)
);
```

#### tags
Reusable tags for categorizing code problems.
```sql
CREATE TABLE public.tags (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  name text NOT NULL DEFAULT ''::text UNIQUE,
  created_at timestamp with time zone NOT NULL DEFAULT now(),
  CONSTRAINT tags_pkey PRIMARY KEY (id)
);
```

#### code_problems
The canonical store of all coding problems.
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
Links problems to tags.
```sql
CREATE TABLE public.code_problem_tags (
  code_problem_id uuid NOT NULL REFERENCES public.code_problems(id) ON DELETE CASCADE,
  tag_id uuid NOT NULL REFERENCES public.tags(id) ON DELETE CASCADE,
  CONSTRAINT code_problem_tags_pkey PRIMARY KEY (tag_id, code_problem_id)
);
```

#### code_problem_language_details
Language-specific starter code and constraints for each problem.
```sql
CREATE TABLE public.code_problem_language_details (
  code_problem_id uuid NOT NULL REFERENCES public.code_problems(id),
  language_id uuid NOT NULL REFERENCES public.languages(id),
  solution_stub text NOT NULL DEFAULT ''::text,
  driver_code text NOT NULL DEFAULT ''::text,
  time_constraint_ms integer NOT NULL DEFAULT 1000,
  space_constraint_mb integer NOT NULL DEFAULT 16,
  CONSTRAINT code_problem_language_details_pkey PRIMARY KEY (language_id, code_problem_id)
);
```

#### test_cases
Test cases used to validate a code problem.
```sql
CREATE TABLE public.test_cases (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  code_problem_id uuid NOT NULL REFERENCES public.code_problems(id) ON DELETE CASCADE,
  input text NOT NULL,
  expected_output text NOT NULL,
  is_hidden boolean NOT NULL DEFAULT true,
  CONSTRAINT test_cases_pkey PRIMARY KEY (id)
);
```

### Participant, Room & Submission Management

#### event_guild_participants
Tracks which guilds are participating in an event.
```sql
CREATE TABLE public.event_guild_participants (
  event_id uuid NOT NULL REFERENCES public.events(id) ON DELETE CASCADE,
  guild_id uuid NOT NULL, -- Soft FK to Social Service guilds
  joined_at timestamp with time zone DEFAULT (now() AT TIME ZONE 'utc'::text),
  room_id uuid,
  CONSTRAINT event_guild_participants_pkey PRIMARY KEY (guild_id, event_id)
);
```

#### rooms
Defines the virtual "rooms" where code battles take place during an event.
```sql
CREATE TABLE public.rooms (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  event_id uuid NOT NULL REFERENCES public.events(id) ON DELETE CASCADE,
  name text NOT NULL DEFAULT ''::text,
  description text NOT NULL DEFAULT ''::text,
  created_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  CONSTRAINT rooms_pkey PRIMARY KEY (id)
);
```

#### room_players
Tracks which players are in which room for a given event.
```sql
CREATE TABLE public.room_players (
  room_id uuid NOT NULL REFERENCES public.rooms(id) ON DELETE CASCADE,
  user_id uuid NOT NULL, -- Soft FK to User Service user_profiles
  username text NOT NULL DEFAULT ''::text,
  score integer NOT NULL DEFAULT 0,
  place integer,
  state room_player_state NOT NULL DEFAULT 'present'::room_player_state,
  disconnected_at timestamp with time zone,
  joined_at timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  CONSTRAINT room_players_pkey PRIMARY KEY (room_id, user_id)
);
```

#### event_code_problems
Links specific code problems to an event.
```sql
CREATE TABLE public.event_code_problems (
  event_id uuid NOT NULL REFERENCES public.events(id) ON DELETE CASCADE,
  code_problem_id uuid NOT NULL REFERENCES public.code_problems(id) ON DELETE CASCADE,
  score integer NOT NULL DEFAULT 0,
  CONSTRAINT event_code_problems_pkey PRIMARY KEY (code_problem_id, event_id)
);
```

#### submissions
A log of all code submissions made during an event.
```sql
CREATE TABLE public.submissions (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  user_id uuid NOT NULL, -- Soft FK to user_profiles
  code_problem_id uuid NOT NULL REFERENCES public.code_problems(id) ON DELETE CASCADE,
  language_id uuid NOT NULL REFERENCES public.languages(id) ON DELETE CASCADE,
  room_id uuid REFERENCES public.rooms(id) ON DELETE SET NULL,
  code_submitted text NOT NULL DEFAULT ''::text,
  status submission_status NOT NULL,
  execution_time_ms integer,
  submitted_at timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  submitted_guild_id uuid, -- Soft FK to Social Service guilds
  CONSTRAINT submissions_pkey PRIMARY KEY (id)
);
```

### Leaderboards

#### leaderboard_entries
Stores the final ranking of individual players for an event.
```sql
CREATE TABLE public.leaderboard_entries (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  user_id uuid NOT NULL, -- Soft FK to User Service user_profiles
  username text NOT NULL DEFAULT ''::text,
  event_id uuid NOT NULL REFERENCES public.events(id) ON DELETE CASCADE,
  rank integer NOT NULL,
  score integer NOT NULL,
  snapshot_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  CONSTRAINT leaderboard_entries_pkey PRIMARY KEY (id)
);
```

#### guild_leaderboard_entries
Stores the final ranking of guilds for an event.
```sql
CREATE TABLE public.guild_leaderboard_entries (
  id uuid NOT NULL DEFAULT gen_random_uuid(),
  guild_id uuid NOT NULL, -- Soft FK to Social Service guilds
  guild_name text NOT NULL DEFAULT ''::text,
  event_id uuid NOT NULL REFERENCES public.events(id) ON DELETE CASCADE,
  rank integer NOT NULL,
  total_score integer NOT NULL,
  snapshot_date timestamp with time zone NOT NULL DEFAULT (now() AT TIME ZONE 'utc'::text),
  CONSTRAINT guild_leaderboard_entries_pkey PRIMARY KEY (id)
);