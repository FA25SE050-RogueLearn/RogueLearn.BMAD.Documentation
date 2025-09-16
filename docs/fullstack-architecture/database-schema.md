# **Database Schema**

This section provides the SQL DDL for the PostgreSQL database.

```sql
-- RogueLearn Database Schema for PostgreSQL
-- FINAL VERSION: Integrated with Supabase Auth and a robust, real-time Code Battle engine.

-- Enable UUID generation if not already enabled
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- ------------------------------------------
-- SECTION 1: ENUM TYPE DEFINITIONS
-- ------------------------------------------
CREATE TYPE "QuestStatus" AS ENUM ('Not Started', 'In Progress', 'Completed');
CREATE TYPE "QuestType" AS ENUM ('Main', 'Side', 'Daily', 'Assignment', 'Exam', 'BossFight');
CREATE TYPE "SyllabusProcessingStatus" AS ENUM ('Pending', 'Processing', 'Completed', 'Failed');
CREATE TYPE "GameSessionStatus" AS ENUM ('InProgress', 'Completed', 'Abandoned');
CREATE TYPE "PartyJoinType" AS ENUM ('Invite Only', 'Open');
CREATE TYPE "EventType" AS ENUM ('Quiz', 'CodeBattle', 'Tournament', 'Duel');
CREATE TYPE "SubmissionStatus" AS ENUM ('In Queue', 'Processing', 'Accepted', 'Wrong Answer', 'Time Limit Exceeded', 'Compilation Error', 'Runtime Error (Generic)', 'Internal Error', 'Execution Server Error');
CREATE TYPE "VerificationStatus" AS ENUM ('Pending', 'Approved', 'Rejected', 'MoreInfoRequired');
CREATE TYPE "MeetingStatus" AS ENUM ('Scheduled', 'InProgress', 'Completed', 'Cancelled');
CREATE TYPE "ParticipantStatus" AS ENUM ('Invited', 'Accepted', 'Declined', 'Attended');


-- ------------------------------------------
-- SECTION 2: USER & PROFILE CORE (No Changes)
-- ------------------------------------------

CREATE TABLE "UserProfiles" (
    "Id" UUID PRIMARY KEY NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
    "Username" TEXT UNIQUE NOT NULL,
    "Email" TEXT UNIQUE NOT NULL,
    "ClassId" UUID, -- FK to Classes table
    "CurriculumId" UUID, -- FK to Curriculums table
    "Level" INTEGER NOT NULL DEFAULT 1,
    "ExperiencePoints" INTEGER NOT NULL DEFAULT 0,
    "Stats" JSONB,
    "OnboardingCompleted" BOOLEAN NOT NULL DEFAULT false,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "UpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "Roles" (
    "Id" SERIAL PRIMARY KEY,
    "RoleName" TEXT UNIQUE NOT NULL
);

CREATE TABLE "UserRoles" (
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "RoleId" INT NOT NULL REFERENCES "Roles"("Id") ON DELETE RESTRICT,
    PRIMARY KEY ("UserProfileId", "RoleId")
);

CREATE TABLE "LecturerVerificationRequests" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "Status" "VerificationStatus" NOT NULL DEFAULT 'Pending',
    "SubmittedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "ReviewedAt" TIMESTAMPTZ,
    "ReviewerNotes" TEXT
);


-- ------------------------------------------
-- SECTION 3: ACADEMIC & CONTENT MANAGEMENT (No Changes)
-- ------------------------------------------

CREATE TABLE "Classes" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "Name" TEXT UNIQUE NOT NULL,
    "Description" TEXT
);

CREATE TABLE "Curriculums" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "Name" TEXT UNIQUE NOT NULL,
    "Description" TEXT
);

CREATE TABLE "Syllabuses" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "CurriculumId" UUID NOT NULL REFERENCES "Curriculums"("Id") ON DELETE CASCADE,
    "CourseCode" TEXT NOT NULL,
    "CourseTitle" TEXT NOT NULL,
    "Description" TEXT,
    "Credits" INT
);

CREATE TABLE "UserSyllabusEnrollments" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "SyllabusId" UUID NOT NULL REFERENCES "Syllabuses"("Id") ON DELETE CASCADE,
    "EnrollmentDate" TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE ("UserProfileId", "SyllabusId")
);

CREATE TABLE "UserUploadedSyllabuses" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "EnrollmentId" UUID NOT NULL REFERENCES "UserSyllabusEnrollments"("Id") ON DELETE CASCADE,
    "FileUrl" TEXT NOT NULL,
    "ProcessingStatus" "SyllabusProcessingStatus" NOT NULL DEFAULT 'Pending',
    "StructuredContent" JSONB,
    "UploadedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "Notes" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "Title" TEXT NOT NULL,
    "Content" JSONB,
    "SyllabusId" UUID REFERENCES "Syllabuses"("Id") ON DELETE SET NULL,
    "QuestId" UUID,
    "SkillId" UUID,
    "Tags" TEXT[],
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "UpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);


-- ------------------------------------------
-- SECTION 4: QUEST & SKILL MANAGEMENT (No Changes)
-- ------------------------------------------

CREATE TABLE "QuestLines" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "SyllabusId" UUID NOT NULL REFERENCES "Syllabuses"("Id") ON DELETE CASCADE,
    "Title" TEXT NOT NULL,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "Quests" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "QuestLineId" UUID NOT NULL REFERENCES "QuestLines"("Id") ON DELETE CASCADE,
    "Title" TEXT NOT NULL,
    "Description" TEXT,
    "Type" "QuestType" NOT NULL,
    "Status" "QuestStatus" NOT NULL DEFAULT 'Not Started',
    "DueDate" TIMESTAMPTZ,
    "ExperiencePoints" INTEGER NOT NULL DEFAULT 10,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "UpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "QuestPrerequisites" (
    "QuestId" UUID NOT NULL REFERENCES "Quests"("Id") ON DELETE CASCADE,
    "PrerequisiteQuestId" UUID NOT NULL REFERENCES "Quests"("Id") ON DELETE CASCADE,
    PRIMARY KEY ("QuestId", "PrerequisiteQuestId")
);

CREATE TABLE "SkillTrees" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "CurriculumId" UUID NOT NULL REFERENCES "Curriculums"("Id") ON DELETE CASCADE,
    "Name" TEXT NOT NULL,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "Skills" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "SkillTreeId" UUID NOT NULL REFERENCES "SkillTrees"("Id") ON DELETE CASCADE,
    "Name" TEXT NOT NULL,
    "Description" TEXT,
    "MaxLevel" INTEGER NOT NULL DEFAULT 10,
    "PositionX" INTEGER,
    "PositionY" INTEGER,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "UserSkills" (
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "SkillId" UUID NOT NULL REFERENCES "Skills"("Id") ON DELETE CASCADE,
    "Level" INTEGER NOT NULL DEFAULT 0,
    PRIMARY KEY ("UserProfileId", "SkillId")
);

CREATE TABLE "SkillDependencies" (
    "SkillId" UUID NOT NULL REFERENCES "Skills"("Id") ON DELETE CASCADE,
    "PrerequisiteSkillId" UUID NOT NULL REFERENCES "Skills"("Id") ON DELETE CASCADE,
    PRIMARY KEY ("SkillId", "PrerequisiteSkillId")
);

ALTER TABLE "Notes" ADD CONSTRAINT fk_notes_quest FOREIGN KEY ("QuestId") REFERENCES "Quests"("Id") ON DELETE SET NULL;
ALTER TABLE "Notes" ADD CONSTRAINT fk_notes_skill FOREIGN KEY ("SkillId") REFERENCES "Skills"("Id") ON DELETE SET NULL;


-- ------------------------------------------
-- SECTION 5: GAME SESSION & NOTIFICATIONS (No Changes)
-- ------------------------------------------

CREATE TABLE "GameSessions" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "QuestId" UUID NOT NULL REFERENCES "Quests"("Id") ON DELETE CASCADE,
    "Status" "GameSessionStatus" NOT NULL DEFAULT 'InProgress',
    "Score" INTEGER NOT NULL DEFAULT 0,
    "ProgressData" JSONB,
    "StartedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "CompletedAt" TIMESTAMPTZ
);

CREATE TABLE "Notifications" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "Message" TEXT NOT NULL,
    "IsRead" BOOLEAN NOT NULL DEFAULT false,
    "Link" TEXT,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);


-- ------------------------------------------
-- SECTION 6: SOCIAL & COMMUNITY (No Changes)
-- ------------------------------------------

CREATE TABLE "Parties" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "Name" TEXT NOT NULL,
    "Description" TEXT,
    "JoinType" "PartyJoinType" NOT NULL DEFAULT 'Invite Only',
    "LeaderId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "PartyMemberships" (
    "PartyId" UUID NOT NULL REFERENCES "Parties"("Id") ON DELETE CASCADE,
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "JoinedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    PRIMARY KEY ("PartyId", "UserProfileId")
);

CREATE TABLE "Guilds" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "Name" TEXT NOT NULL,
    "Description" TEXT,
    "MasterId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "IsVerified" BOOLEAN NOT NULL DEFAULT false,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "GuildMemberships" (
    "GuildId" UUID NOT NULL REFERENCES "Guilds"("Id") ON DELETE CASCADE,
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "JoinedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    PRIMARY KEY ("GuildId", "UserProfileId")
);

CREATE TABLE "Meetings" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "PartyId" UUID NOT NULL REFERENCES "Parties"("Id") ON DELETE CASCADE,
    "CreatorId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE SET NULL,
    "Title" TEXT NOT NULL,
    "Description" TEXT,
    "ScheduledStartTime" TIMESTAMPTZ NOT NULL,
    "ScheduledEndTime" TIMESTAMPTZ,
    "Status" "MeetingStatus" NOT NULL DEFAULT 'Scheduled',
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "MeetingParticipants" (
    "MeetingId" UUID NOT NULL REFERENCES "Meetings"("Id") ON DELETE CASCADE,
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "Status" "ParticipantStatus" NOT NULL DEFAULT 'Invited',
    PRIMARY KEY ("MeetingId", "UserProfileId")
);

CREATE TABLE "MeetingAgenda" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "MeetingId" UUID NOT NULL REFERENCES "Meetings"("Id") ON DELETE CASCADE,
    "Topic" TEXT NOT NULL,
    "Description" TEXT,
    "PresenterId" UUID REFERENCES "UserProfiles"("Id") ON DELETE SET NULL,
    "Sequence" INTEGER NOT NULL,
    "DurationMinutes" INTEGER
);

CREATE TABLE "MeetingNotes" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "MeetingId" UUID NOT NULL REFERENCES "Meetings"("Id") ON DELETE CASCADE,
    "AgendaId" UUID REFERENCES "MeetingAgenda"("Id") ON DELETE SET NULL,
    "CreatorId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "Content" JSONB NOT NULL,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "UpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);


-- ------------------------------------------
-- SECTION 7: EVENTS & COMPETITION (Modified)
-- ------------------------------------------

CREATE TABLE "Events" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "GuildId" UUID REFERENCES "Guilds"("Id") ON DELETE CASCADE,
    "Title" TEXT NOT NULL,
    "Description" TEXT,
    "Type" "EventType" NOT NULL,
    "StartDate" TIMESTAMPTZ,
    "EndDate" TIMESTAMPTZ
);

-- This table now serves as a canonical reference for a problem's text.
-- The actual question content (template, etc.) is in ChromaDB.
CREATE TABLE "CodeProblems" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "Title" TEXT NOT NULL,
    "ProblemStatement" TEXT NOT NULL
);

CREATE TABLE "EventCodeProblems" (
    "EventId" UUID NOT NULL REFERENCES "Events"("Id") ON DELETE CASCADE,
    "ProblemId" UUID NOT NULL REFERENCES "CodeProblems"("Id") ON DELETE CASCADE,
    PRIMARY KEY ("EventId", "ProblemId")
);

CREATE TABLE "LeaderboardEntries" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "EventId" UUID REFERENCES "Events"("Id") ON DELETE CASCADE,
    "Rank" INT NOT NULL,
    "Score" BIGINT NOT NULL,
    "SnapshotDate" DATE NOT NULL
);

CREATE TABLE "GuildLeaderboardEntries" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "GuildId" UUID NOT NULL REFERENCES "Guilds"("Id") ON DELETE CASCADE,
    "EventId" UUID REFERENCES "Events"("Id") ON DELETE CASCADE,
    "Rank" INT NOT NULL,
    "TotalScore" BIGINT NOT NULL,
    "SnapshotDate" DATE NOT NULL
);


-- ------------------------------------------
-- SECTION 8: REAL-TIME CODE BATTLES & JUDGING (FULLY MERGED & DETAILED)
-- ------------------------------------------

CREATE TABLE "Languages" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "Name" TEXT NOT NULL UNIQUE,
    "CompileCmd" TEXT,
    "RunCmd" TEXT NOT NULL,
    "TimeoutSeconds" DOUBLE PRECISION
);

CREATE TABLE "Rooms" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "EventId" UUID NOT NULL REFERENCES "Events"("Id") ON DELETE CASCADE,
    "Name" TEXT NOT NULL,
    "Description" TEXT,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "RoomPlayers" (
    "RoomId" UUID NOT NULL REFERENCES "Rooms"("Id") ON DELETE CASCADE,
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "Score" INTEGER NOT NULL DEFAULT 0,
    "Place" INTEGER,
    "State" TEXT,
    "DisconnectedAt" TIMESTAMPTZ,
    PRIMARY KEY ("RoomId", "UserProfileId")
);

CREATE TABLE "TestCases" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "CodeProblemId" UUID NOT NULL REFERENCES "CodeProblems"("Id") ON DELETE CASCADE,
    "Input" TEXT NOT NULL,
    "ExpectedOutput" TEXT NOT NULL,
    "TimeConstraint" DOUBLE PRECISION,
    "SpaceConstraint" INTEGER
);

-- This fully-featured table replaces the original "CodeSubmissions"
-- It contains all fields necessary for a detailed, external judging service.
CREATE TABLE "Submissions" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE,
    "EventId" UUID NOT NULL REFERENCES "Events"("Id") ON DELETE CASCADE,
    "CodeProblemId" UUID NOT NULL REFERENCES "CodeProblems"("Id") ON DELETE CASCADE,
    "LanguageId" UUID NOT NULL REFERENCES "Languages"("Id") ON DELETE RESTRICT,
    "SourceCode" TEXT NOT NULL,
    "Status" "SubmissionStatus" NOT NULL DEFAULT 'In Queue',
    
    -- Execution Results
    "StdOut" TEXT,
    "StdErr" TEXT,
    "CompileOutput" TEXT,
    "ExitCode" INTEGER,
    "ExitSignal" INTEGER,
    "Message" TEXT, -- Human-readable message from the judge
    
    -- Performance Metrics
    "Time" NUMERIC, -- Execution time in seconds
    "Memory" INTEGER, -- Memory used in kilobytes
    "WallTime" NUMERIC, -- Wall clock time in seconds

    -- Judge Tracking & Configuration
    "Token" TEXT UNIQUE, -- Unique token to track with the judge (e.g., Judge0)
    "CallbackUrl" TEXT,
    "NumberOfRuns" INTEGER,
    "CompilerOptions" TEXT,
    "CommandLineArguments" TEXT,
    "RedirectStdErrToStdOut" BOOLEAN,
    "AdditionalFiles" BYTEA,
    "EnableNetwork" BOOLEAN,

    -- Judge Resource Limits
    "CpuTimeLimit" NUMERIC,
    "CpuExtraTime" NUMERIC,
    "WallTimeLimit" NUMERIC,
    "MemoryLimit" INTEGER,
    "StackLimit" INTEGER,
    "MaxProcessesAndOrThreads" INTEGER,
    "EnablePerProcessAndThreadTimeLimit" BOOLEAN,
    "EnablePerProcessAndThreadMemoryLimit" BOOLEAN,
    "MaxFileSize" INTEGER,
    
    -- Timestamps & Host Info
    "QueuedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "StartedAt" TIMESTAMPTZ,
    "FinishedAt" TIMESTAMPTZ,
    "UpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "QueueHost" TEXT,
    "ExecutionHost" TEXT
);


-- ------------------------------------------
-- SECTION 9: INDEXES FOR PERFORMANCE (Updated)
-- ------------------------------------------
CREATE INDEX "idx_userprofiles_email" ON "UserProfiles"("Email");
CREATE INDEX "idx_enrollments_user_id" ON "UserSyllabusEnrollments"("UserProfileId");
CREATE INDEX "idx_enrollments_syllabus_id" ON "UserSyllabusEnrollments"("SyllabusId");
CREATE INDEX "idx_notes_user_id" ON "Notes"("UserProfileId");
CREATE INDEX "idx_quests_questline_id" ON "Quests"("QuestLineId");
CREATE INDEX "idx_skills_skilltree_id" ON "Skills"("SkillTreeId");
CREATE INDEX "idx_userskills_user_id" ON "UserSkills"("UserProfileId");
CREATE INDEX "idx_gamesessions_user_id" ON "GameSessions"("UserProfileId");
CREATE INDEX "idx_guildmemberships_user_id" ON "GuildMemberships"("UserProfileId");
CREATE INDEX "idx_meetings_party_id" ON "Meetings"("PartyId");
CREATE INDEX "idx_meetingparticipants_user_id" ON "MeetingParticipants"("UserProfileId");
CREATE INDEX "idx_meetingagenda_meeting_id" ON "MeetingAgenda"("MeetingId");
CREATE INDEX "idx_meetingnotes_meeting_id" ON "MeetingNotes"("MeetingId");
CREATE INDEX "idx_rooms_event_id" ON "Rooms"("EventId");
CREATE INDEX "idx_roomplayers_user_id" ON "RoomPlayers"("UserProfileId");
CREATE INDEX "idx_testcases_codeproblem_id" ON "TestCases"("CodeProblemId");
CREATE INDEX "idx_submissions_user_event" ON "Submissions"("UserProfileId", "EventId");
CREATE INDEX "idx_submissions_problem_id" ON "Submissions"("CodeProblemId");
CREATE INDEX "idx_submissions_token" ON "Submissions"("Token");
```

