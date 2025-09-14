# **Database Schema**

This section provides the SQL DDL for the PostgreSQL database.

```sql
-- ========= User Management (Managed by Auth Service) =========

CREATE TABLE "UserProfiles" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "UserId" text NOT NULL UNIQUE, -- Clerk User ID
    "Username" text NOT NULL,
    "Email" text NOT NULL,
    "ClassId" uuid, -- FK to a future "Classes" table
    "RouteId" uuid, -- FK to a future "Routes" table
    "Level" integer NOT NULL DEFAULT 1,
    "ExperiencePoints" integer NOT NULL DEFAULT 0,
    "ProfileImageUrl" text,
    "OnboardingCompleted" boolean NOT NULL DEFAULT false,
    "CreatedAt" timestamp with time zone NOT NULL DEFAULT now(),
    "UpdatedAt" timestamp with time zone NOT NULL DEFAULT now()
);

-- ========= Course & Syllabus Management (Managed by Quests Service) =========

CREATE TABLE "Courses" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "UserProfileId" uuid NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "Name" text NOT NULL,
    "CourseCode" text,
    "CreatedAt" timestamp with time zone NOT NULL DEFAULT now(),
    "UpdatedAt" timestamp with time zone NOT NULL DEFAULT now()
);

CREATE TYPE "SyllabusStatus" AS ENUM ('Pending', 'Processing', 'Completed', 'Failed');

CREATE TABLE "Syllabi" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "CourseId" uuid NOT NULL REFERENCES "Courses"("Id") ON DELETE CASCADE,
    "FileUrl" text NOT NULL, -- Link to file in Supabase Storage
    "ProcessingStatus" "SyllabusStatus" NOT NULL DEFAULT 'Pending',
    "StructuredContent" jsonb, -- The AI-parsed JSON output
    "SchemaVersion" text NOT NULL DEFAULT '1.0',
    "UploadedAt" timestamp with time zone NOT NULL DEFAULT now()
);

-- ========= Quest Management (Managed by Quests Service) =========

CREATE TABLE "QuestLines" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "CourseId" uuid NOT NULL REFERENCES "Courses"("Id") ON DELETE CASCADE,
    "Title" text NOT NULL,
    "CreatedAt" timestamp with time zone NOT NULL DEFAULT now()
);

CREATE TYPE "QuestType" AS ENUM ('Learning', 'Assignment', 'Exam', 'BossFight');
CREATE TYPE "ProgressStatus" AS ENUM ('Not Started', 'In Progress', 'Completed');

CREATE TABLE "Quests" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "QuestLineId" uuid NOT NULL REFERENCES "QuestLines"("Id") ON DELETE CASCADE,
    "Title" text NOT NULL,
    "Description" text,
    "Type" "QuestType" NOT NULL,
    "Status" "ProgressStatus" NOT NULL DEFAULT 'Not Started',
    "DueDate" timestamp with time zone,
    "ExperiencePoints" integer NOT NULL DEFAULT 10,
    "Prerequisites" uuid[], -- Array of Quest IDs
    "CreatedAt" timestamp with time zone NOT NULL DEFAULT now(),
    "UpdatedAt" timestamp with time zone NOT NULL DEFAULT now()
);

-- ========= Skill Tree Management (Managed by Quests Service) =========

CREATE TABLE "SkillTrees" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "CourseId" uuid NOT NULL REFERENCES "Courses"("Id") ON DELETE CASCADE,
    "Name" text NOT NULL,
    "CreatedAt" timestamp with time zone NOT NULL DEFAULT now()
);

CREATE TABLE "Skills" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "SkillTreeId" uuid NOT NULL REFERENCES "SkillTrees"("Id") ON DELETE CASCADE,
    "Name" text NOT NULL,
    "Description" text,
    "Level" integer NOT NULL DEFAULT 0,
    "MaxLevel" integer NOT NULL DEFAULT 10,
    "Prerequisites" uuid[], -- Array of Skill IDs
    "PositionX" integer,
    "PositionY" integer,
    "CreatedAt" timestamp with time zone NOT NULL DEFAULT now(),
    "UpdatedAt" timestamp with time zone NOT NULL DEFAULT now()
);

-- ========= Game Session Management (Managed by Quests Service) =========

CREATE TYPE "GameSessionStatus" AS ENUM ('InProgress', 'Completed', 'Abandoned');

CREATE TABLE "GameSessions" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "UserProfileId" uuid NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "QuestId" uuid NOT NULL REFERENCES "Quests"("Id") ON DELETE CASCADE,
    "Status" "GameSessionStatus" NOT NULL DEFAULT 'InProgress',
    "Score" integer NOT NULL DEFAULT 0,
    "ProgressData" jsonb, -- To store game-specific state
    "StartedAt" timestamp with time zone NOT NULL DEFAULT now(),
    "CompletedAt" timestamp with time zone,
    "UpdatedAt" timestamp with time zone NOT NULL DEFAULT now()
);

-- ========= Arsenal (Notes) Management (Managed by Quests Service) =========

CREATE TABLE "Notes" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "UserProfileId" uuid NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "Title" text NOT NULL,
    "Content" jsonb,
    "CourseId" uuid REFERENCES "Courses"("Id") ON DELETE SET NULL,
    "QuestId" uuid REFERENCES "Quests"("Id") ON DELETE SET NULL,
    "SkillId" uuid REFERENCES "Skills"("Id") ON DELETE SET NULL,
    "Tags" text[],
    "CreatedAt" timestamp with time zone NOT NULL DEFAULT now(),
    "UpdatedAt" timestamp with time zone NOT NULL DEFAULT now()
);

-- ========= Social Features (Managed by Social Service) =========

CREATE TYPE "PartyJoinType" AS ENUM ('Invite Only', 'Open');

CREATE TABLE "Parties" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "Name" text NOT NULL,
    "Description" text,
    "JoinType" "PartyJoinType" NOT NULL DEFAULT 'Invite Only',
    "LeaderId" uuid NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "CreatedAt" timestamp with time zone NOT NULL DEFAULT now(),
    "UpdatedAt" timestamp with time zone NOT NULL DEFAULT now()
);

CREATE TABLE "PartyMemberships" (
    "PartyId" uuid NOT NULL REFERENCES "Parties"("Id") ON DELETE CASCADE,
    "UserProfileId" uuid NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "JoinedAt" timestamp with time zone NOT NULL DEFAULT now(),
    PRIMARY KEY ("PartyId", "UserProfileId")
);

CREATE TABLE "Guilds" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "Name" text NOT NULL,
    "Description" text,
    "MasterId" uuid NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "IsVerified" boolean NOT NULL DEFAULT false,
    "CreatedAt" timestamp with time zone NOT NULL DEFAULT now(),
    "UpdatedAt" timestamp with time zone NOT NULL DEFAULT now()
);

CREATE TABLE "GuildMemberships" (
    "GuildId" uuid NOT NULL REFERENCES "Guilds"("Id") ON DELETE CASCADE,
    "UserProfileId" uuid NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "JoinedAt" timestamp with time zone NOT NULL DEFAULT now(),
    PRIMARY KEY ("GuildId", "UserProfileId")
);

CREATE TYPE "EventType" AS ENUM ('Quiz', 'CodeBattle', 'Tournament', 'Duel');

CREATE TABLE "Events" (
    "Id" uuid PRIMARY KEY DEFAULT gen_random_uuid(),
    "GuildId" uuid NOT NULL REFERENCES "Guilds"("Id") ON DELETE CASCADE,
    "Title" text NOT NULL,
    "Description" text,
    "Type" "EventType" NOT NULL,
    "StartDate" timestamp with time zone,
    "EndDate" timestamp with time zone,
    "CreatedAt" timestamp with time zone NOT NULL DEFAULT now(),
    "UpdatedAt" timestamp with time zone NOT NULL DEFAULT now()
);

CREATE TABLE "CodeBattles" (
    "EventId" uuid PRIMARY KEY REFERENCES "Events"("Id") ON DELETE CASCADE,
    "ProblemStatement" text NOT NULL,
    "TestCases" jsonb
);

-- ========= Indexes for Performance =========
CREATE INDEX idx_courses_user_profile_id ON "Courses"("UserProfileId");
CREATE INDEX idx_syllabi_course_id ON "Syllabi"("CourseId");
CREATE INDEX idx_questlines_course_id ON "QuestLines"("CourseId");
CREATE INDEX idx_quests_questline_id ON "Quests"("QuestLineId");
CREATE INDEX idx_skilltrees_course_id ON "SkillTrees"("CourseId");
CREATE INDEX idx_skills_skilltree_id ON "Skills"("SkillTreeId");
CREATE INDEX idx_gamesessions_user_profile_id ON "GameSessions"("UserProfileId");
CREATE INDEX idx_gamesessions_quest_id ON "GameSessions"("QuestId");
CREATE INDEX idx_notes_user_profile_id ON "Notes"("UserProfileId");
CREATE INDEX idx_parties_leader_id ON "Parties"("LeaderId");
CREATE INDEX idx_guilds_master_id ON "Guilds"("MasterId");
CREATE INDEX idx_events_guild_id ON "Events"("GuildId");
```
