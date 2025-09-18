# **Database Schema**

This section provides the SQL DDL for the PostgreSQL database.

```sql
-- RogueLearn Database Schema for PostgreSQL
-- FINAL VERSION: Integrated with Supabase Auth and a robust, real-time Code Battle engine.

-- Enable UUID generation if not already enabled
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Enum Types
CREATE TYPE "UserRole" AS ENUM ('Student', 'Lecturer', 'Admin');
CREATE TYPE "QuestStatus" AS ENUM ('NotStarted', 'InProgress', 'Completed', 'Failed');
CREATE TYPE "QuestType" AS ENUM ('Theory', 'Practice', 'Project', 'Assessment');
CREATE TYPE "SkillLevel" AS ENUM ('Beginner', 'Intermediate', 'Advanced', 'Expert');
CREATE TYPE "AchievementType" AS ENUM ('Quest', 'Skill', 'Social', 'Special');
CREATE TYPE "NotificationType" AS ENUM ('Quest', 'Achievement', 'Social', 'System');
CREATE TYPE "PartyRole" AS ENUM ('Leader', 'Member');
CREATE TYPE "MeetingStatus" AS ENUM ('Scheduled', 'InProgress', 'Completed', 'Cancelled');
CREATE TYPE "EventType" AS ENUM ('Competition', 'Workshop', 'Seminar', 'Social');
CREATE TYPE "EventStatus" AS ENUM ('Upcoming', 'Ongoing', 'Completed', 'Cancelled');
CREATE TYPE "SubmissionStatus" AS ENUM ('Pending', 'Running', 'Accepted', 'WrongAnswer', 'TimeLimitExceeded', 'RuntimeError', 'CompileError');
CREATE TYPE "StudentTermSubjectStatus" AS ENUM ('Enrolled', 'Completed', 'Failed', 'Withdrawn');


-- ------------------------------------------
-- SECTION 2: USER & PROFILE CORE (No Changes)
-- ------------------------------------------

CREATE TABLE "UserProfiles" (
    "AuthUserId" UUID PRIMARY KEY, -- FK to Supabase Auth Users (now primary key)
    "Username" TEXT UNIQUE NOT NULL,
    "Email" TEXT UNIQUE NOT NULL,
    "FirstName" TEXT NOT NULL,
    "LastName" TEXT NOT NULL,
    "ClassId" UUID, -- FK to Classes table
    "CreatedAt" TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    "UpdatedAt" TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE "Roles" (
    "Id" SERIAL PRIMARY KEY,
    "RoleName" TEXT UNIQUE NOT NULL
);

CREATE TABLE "UserRoles" (
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "RoleId" INT NOT NULL REFERENCES "Roles"("Id") ON DELETE RESTRICT,
    PRIMARY KEY ("AuthUserId", "RoleId")
);

CREATE TABLE "LecturerVerificationRequests" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "Status" "VerificationStatus" NOT NULL DEFAULT 'Pending',
    "SubmittedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "ReviewedAt" TIMESTAMPTZ,
    "ReviewerNotes" TEXT
);


-- ------------------------------------------
-- SECTION 3: ACADEMIC & CONTENT MANAGEMENT (Updated with Enhanced Curriculum Structure)
-- ------------------------------------------

CREATE TABLE "Classes" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "Name" TEXT UNIQUE NOT NULL,
    "Description" TEXT
);

-- ------------------------------------------
-- CURRICULUM MANAGEMENT SYSTEM
-- ------------------------------------------

-- The abstract program, e.g., "B.S. in Software Engineering"
CREATE TABLE "CurriculumPrograms" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "ProgramName" TEXT NOT NULL,
    "ProgramCode" TEXT UNIQUE NOT NULL
);

-- The specific, versioned "snapshot" of a program, e.g., "K18A"
CREATE TABLE "CurriculumVersions" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "ProgramId" UUID NOT NULL REFERENCES "CurriculumPrograms"("Id") ON DELETE CASCADE,
    "VersionTag" TEXT NOT NULL, -- "K18A", "K18B", "2024-2025 Catalog"
    "EffectiveYear" INTEGER NOT NULL,
    "IsPublished" BOOLEAN NOT NULL DEFAULT true, -- Is it visible to new students?
    UNIQUE ("ProgramId", "VersionTag")
);

-- The master list of all subjects offered
CREATE TABLE "Subjects" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "SubjectCode" TEXT UNIQUE NOT NULL, -- "CS464"
    "Title" TEXT NOT NULL, -- "Introduction to Machine Learning"
    "Credits" INTEGER NOT NULL
);

-- The CRUCIAL junction table defining the ideal path for a curriculum version
CREATE TABLE "CurriculumStructure" (
    "CurriculumVersionId" UUID NOT NULL REFERENCES "CurriculumVersions"("Id") ON DELETE CASCADE,
    "SubjectId" UUID NOT NULL REFERENCES "Subjects"("Id") ON DELETE CASCADE,
    "PrescribedSemester" INTEGER NOT NULL, -- The semester this subject is *supposed* to be taken in (1, 2, 3...)
    PRIMARY KEY ("CurriculumVersionId", "SubjectId")
);

-- A table to handle versioned syllabi for each subject
CREATE TABLE "SyllabusVersions" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "SubjectId" UUID NOT NULL REFERENCES "Subjects"("Id") ON DELETE CASCADE,
    "VersionDescription" TEXT NOT NULL, -- "Fall 2025 Syllabus"
    "EffectiveDate" DATE NOT NULL,
    "SyllabusContent" JSONB, -- The structured content of the syllabus
    "FileUrl" TEXT
);

-- Locks a student into a specific curriculum version
CREATE TABLE "StudentEnrollments" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "CurriculumVersionId" UUID NOT NULL REFERENCES "CurriculumVersions"("Id") ON DELETE CASCADE,
    "StartDate" DATE NOT NULL,
    "ExpectedGraduationDate" DATE
);

-- **KEY TABLE** - Tracks what a student is ACTUALLY taking each semester
CREATE TABLE "StudentTermSubjects" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "EnrollmentId" UUID NOT NULL REFERENCES "StudentEnrollments"("Id") ON DELETE CASCADE,
    "SubjectId" UUID NOT NULL REFERENCES "Subjects"("Id") ON DELETE CASCADE,
    "AcademicTerm" TEXT NOT NULL, -- e.g., "Fall 2025", "Semester 3"
    "Status" TEXT NOT NULL DEFAULT 'Enrolled', -- 'Enrolled', 'Completed', 'Failed', 'Withdrawn'
    "FinalGrade" TEXT, -- Optional
    "IsRetake" BOOLEAN NOT NULL DEFAULT false
);

CREATE TABLE "Notes" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "Title" TEXT NOT NULL,
    "Content" JSONB,
    "QuestId" UUID,
    "SkillId" UUID,
    "Tags" TEXT[],
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "UpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- This table stores shared notes in party stash, duplicating user notes for party collaboration
CREATE TABLE "PartyStashItems" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "PartyId" UUID NOT NULL REFERENCES "Parties"("Id") ON DELETE CASCADE,
    "OriginalNoteId" UUID NOT NULL REFERENCES "Notes"("Id") ON DELETE CASCADE,
    "SharedByUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "Title" TEXT NOT NULL,
    "Content" JSONB NOT NULL,
    "SyllabusId" UUID REFERENCES "Syllabuses"("Id") ON DELETE SET NULL,
    "QuestId" UUID REFERENCES "Quests"("Id") ON DELETE SET NULL,
    "SkillId" UUID REFERENCES "Skills"("Id") ON DELETE SET NULL,
    "Tags" TEXT[],
    "SharedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "UpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);


-- ------------------------------------------
-- SECTION 4: QUEST & SKILL MANAGEMENT (No Changes)
-- ------------------------------------------

CREATE TABLE "QuestLines" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "ClassId" UUID NOT NULL REFERENCES "Classes"("Id") ON DELETE CASCADE,
    "CurriculumVersionId" UUID NOT NULL REFERENCES "CurriculumVersions"("Id") ON DELETE CASCADE,
    "Title" TEXT NOT NULL,
    "Description" TEXT,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "UpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "QuestChapters" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "QuestLineId" UUID NOT NULL REFERENCES "QuestLines"("Id") ON DELETE CASCADE,
    "Title" TEXT NOT NULL, -- e.g., "Semester 3: Week 5", "Finals Week"
    "Sequence" INTEGER NOT NULL, -- 1, 2, 3... to order the chapters linearly
    "Status" "QuestStatus" NOT NULL DEFAULT 'Not Started', -- 'Not Started', 'In Progress', 'Completed'
    "StartDate" DATE,
    "EndDate" DATE,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "UpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE ("QuestLineId", "Sequence")
);

CREATE TABLE "Quests" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "QuestChapterId" UUID NOT NULL REFERENCES "QuestChapters"("Id") ON DELETE CASCADE,
    "SyllabusId" UUID NOT NULL REFERENCES "Syllabuses"("Id") ON DELETE CASCADE,
    "SubjectId" UUID NOT NULL REFERENCES "Subjects"("Id") ON DELETE CASCADE,
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
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "SkillId" UUID NOT NULL REFERENCES "Skills"("Id") ON DELETE CASCADE,
    "Level" INTEGER NOT NULL DEFAULT 0,
    PRIMARY KEY ("AuthUserId", "SkillId")
);

CREATE TABLE "UserSkillTrees" (
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "SkillTreeId" UUID NOT NULL REFERENCES "SkillTrees"("Id") ON DELETE CASCADE,
    PRIMARY KEY ("AuthUserId", "SkillTreeId")
);

CREATE TABLE "QuestSkills" (
    "QuestId" UUID NOT NULL REFERENCES "Quests"("Id") ON DELETE CASCADE,
    "SkillId" UUID NOT NULL REFERENCES "Skills"("Id") ON DELETE CASCADE,
    PRIMARY KEY ("QuestId", "SkillId")
);

CREATE TABLE "SkillDependencies" (
    "SkillId" UUID NOT NULL REFERENCES "Skills"("Id") ON DELETE CASCADE,
    "PrerequisiteSkillId" UUID NOT NULL REFERENCES "Skills"("Id") ON DELETE CASCADE,
    PRIMARY KEY ("SkillId", "PrerequisiteSkillId")
);

ALTER TABLE "Notes" ADD CONSTRAINT fk_notes_quest FOREIGN KEY ("QuestId") REFERENCES "Quests"("Id") ON DELETE SET NULL;
ALTER TABLE "Notes" ADD CONSTRAINT fk_notes_skill FOREIGN KEY ("SkillId") REFERENCES "Skills"("Id") ON DELETE SET NULL;


-- ------------------------------------------
-- SECTION 5: USER QUEST PROGRESS & ACHIEVEMENTS
-- ------------------------------------------

-- This table stores a summary of a user's progress on quests, providing a quick reference 
-- for the User Service without needing to constantly query the Quest Service.
CREATE TABLE "UserQuestProgress" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "QuestId" UUID NOT NULL,
    "Status" "QuestStatus" NOT NULL,
    "CompletedAt" TIMESTAMPTZ,
    "LastUpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE ("AuthUserId", "QuestId")
);

-- This table serves as a central catalog of all possible achievements a user can earn.
CREATE TABLE "Achievements" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "Name" TEXT NOT NULL UNIQUE,
    "Description" TEXT NOT NULL,
    "IconUrl" TEXT,
    "SourceService" VARCHAR(255) NOT NULL -- e.g., 'QuestsService', 'CodeBattleService'
);

-- This table links users to the achievements they have earned.
CREATE TABLE "UserAchievements" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "AchievementId" UUID NOT NULL REFERENCES "Achievements"("Id") ON DELETE CASCADE,
    "EarnedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "Context" JSONB -- To store additional details, like the event or quest that triggered the achievement
);


-- ------------------------------------------
-- SECTION 6: GAME SESSION & NOTIFICATIONS (No Changes)
-- ------------------------------------------

CREATE TABLE "GameSessions" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "QuestId" UUID NOT NULL REFERENCES "Quests"("Id") ON DELETE CASCADE,
    "Status" "GameSessionStatus" NOT NULL DEFAULT 'InProgress',
    "Score" INTEGER NOT NULL DEFAULT 0,
    "ProgressData" JSONB,
    "StartedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "CompletedAt" TIMESTAMPTZ
);

CREATE TABLE "Notifications" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "Message" TEXT NOT NULL,
    "IsRead" BOOLEAN NOT NULL DEFAULT false,
    "Link" TEXT,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);


-- ------------------------------------------
-- SECTION 7: SOCIAL & COMMUNITY (No Changes)
-- ------------------------------------------

CREATE TABLE "Parties" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "Name" TEXT NOT NULL,
    "Description" TEXT,
    "JoinType" "PartyJoinType" NOT NULL DEFAULT 'Invite Only',
    "LeaderId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "PartyMemberships" (
    "PartyId" UUID NOT NULL REFERENCES "Parties"("Id") ON DELETE CASCADE,
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "JoinedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    PRIMARY KEY ("PartyId", "AuthUserId")
);

CREATE TABLE "Guilds" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "Name" TEXT NOT NULL,
    "Description" TEXT,
    "MasterId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "IsVerified" BOOLEAN NOT NULL DEFAULT false,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "GuildMemberships" (
    "GuildId" UUID NOT NULL REFERENCES "Guilds"("Id") ON DELETE CASCADE,
    "AuthUserId" UUID NOT NULL UNIQUE REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "JoinedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    PRIMARY KEY ("GuildId", "AuthUserId")
);

CREATE TABLE "Meetings" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "ContextId" UUID NOT NULL, -- References either Parties.Id or Guilds.Id
    "ContextType" VARCHAR(20) NOT NULL CHECK ("ContextType" IN ('party', 'guild')),
    "CreatorId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE SET NULL,
    "Title" TEXT NOT NULL,
    "Description" TEXT,
    "ScheduledStartTime" TIMESTAMPTZ NOT NULL,
    "ScheduledEndTime" TIMESTAMPTZ,
    "Status" "MeetingStatus" NOT NULL DEFAULT 'Scheduled',
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    -- Ensure ContextId references valid Party or Guild based on ContextType
    CONSTRAINT "meetings_context_party_fk" 
        CHECK (("ContextType" = 'party' AND "ContextId" IN (SELECT "Id" FROM "Parties")) OR "ContextType" != 'party'),
    CONSTRAINT "meetings_context_guild_fk" 
        CHECK (("ContextType" = 'guild' AND "ContextId" IN (SELECT "Id" FROM "Guilds")) OR "ContextType" != 'guild')
);

CREATE TABLE "MeetingParticipants" (
    "MeetingId" UUID NOT NULL REFERENCES "Meetings"("Id") ON DELETE CASCADE,
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "Status" "ParticipantStatus" NOT NULL DEFAULT 'Invited',
    PRIMARY KEY ("MeetingId", "AuthUserId")
);

CREATE TABLE "MeetingAgenda" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "MeetingId" UUID NOT NULL UNIQUE REFERENCES "Meetings"("Id") ON DELETE CASCADE,
    "Topic" TEXT NOT NULL,
    "Description" TEXT,
    "PresenterId" UUID REFERENCES "UserProfiles"("AuthUserId") ON DELETE SET NULL,
    "Sequence" INTEGER NOT NULL,
    "DurationMinutes" INTEGER
);

CREATE TABLE "MeetingNotes" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "MeetingId" UUID NOT NULL UNIQUE REFERENCES "Meetings"("Id") ON DELETE CASCADE,
    "AgendaId" UUID REFERENCES "MeetingAgenda"("Id") ON DELETE SET NULL,
    "Content" JSONB NOT NULL,
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "UpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE "MeetingParticipantAgendaAccess" (
    "ParticipantId" UUID NOT NULL,
    "MeetingId" UUID NOT NULL,
    "AgendaId" UUID NOT NULL REFERENCES "MeetingAgenda"("Id") ON DELETE CASCADE,
    PRIMARY KEY ("ParticipantId", "MeetingId", "AgendaId"),
    FOREIGN KEY ("ParticipantId", "MeetingId") REFERENCES "MeetingParticipants"("AuthUserId", "MeetingId") ON DELETE CASCADE
);

-- This table tracks detailed participant activity during meetings including check-in/check-out times
CREATE TABLE "MeetingParticipantActivity" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "MeetingId" UUID NOT NULL REFERENCES "Meetings"("Id") ON DELETE CASCADE,
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "CheckInTime" TIMESTAMPTZ NOT NULL DEFAULT now(),
    "CheckOutTime" TIMESTAMPTZ,
    "ActivityType" VARCHAR(50) NOT NULL, -- 'join', 'leave', 'reconnect', 'disconnect'
    "DeviceInfo" JSONB, -- Browser, OS, device details
    "IpAddress" INET,
    "Location" TEXT, -- Geographic location if available
    "ConnectionQuality" VARCHAR(20), -- 'excellent', 'good', 'fair', 'poor'
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    FOREIGN KEY ("MeetingId", "AuthUserId") REFERENCES "MeetingParticipants"("MeetingId", "AuthUserId") ON DELETE CASCADE
);

-- This table tracks participant engagement and interactions during meetings
CREATE TABLE "MeetingParticipantEngagement" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "MeetingId" UUID NOT NULL REFERENCES "Meetings"("Id") ON DELETE CASCADE,
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "AgendaId" UUID REFERENCES "MeetingAgenda"("Id") ON DELETE SET NULL,
    "EngagementType" VARCHAR(50) NOT NULL, -- 'spoke', 'shared_screen', 'chat_message', 'reaction', 'raised_hand'
    "Duration" INTEGER, -- Duration in seconds for activities like speaking or screen sharing
    "Content" JSONB, -- Additional context like chat message content, reaction type, etc.
    "Timestamp" TIMESTAMPTZ NOT NULL DEFAULT now(),
    FOREIGN KEY ("MeetingId", "AuthUserId") REFERENCES "MeetingParticipants"("MeetingId", "AuthUserId") ON DELETE CASCADE
);

-- This table provides aggregated statistics for participant performance in meetings
CREATE TABLE "MeetingParticipantStats" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "MeetingId" UUID NOT NULL REFERENCES "Meetings"("Id") ON DELETE CASCADE,
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "TotalAttendanceMinutes" INTEGER NOT NULL DEFAULT 0,
    "SpeakingTimeMinutes" INTEGER NOT NULL DEFAULT 0,
    "ScreenShareTimeMinutes" INTEGER NOT NULL DEFAULT 0,
    "ChatMessagesCount" INTEGER NOT NULL DEFAULT 0,
    "ReactionsCount" INTEGER NOT NULL DEFAULT 0,
    "HandRaisesCount" INTEGER NOT NULL DEFAULT 0,
    "ConnectionIssuesCount" INTEGER NOT NULL DEFAULT 0,
    "EngagementScore" DECIMAL(5,2), -- Calculated engagement score (0-100)
    "LastUpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE ("MeetingId", "AuthUserId"),
    FOREIGN KEY ("MeetingId", "AuthUserId") REFERENCES "MeetingParticipants"("MeetingId", "AuthUserId") ON DELETE CASCADE
);


-- ------------------------------------------
-- SECTION 8: EVENTS & COMPETITION (Modified)
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
    "ProblemStatement" TEXT NOT NULL,
    "LanguageId" UUID NOT NULL REFERENCES "Languages"("Id") ON DELETE RESTRICT
);

CREATE TABLE "EventCodeProblems" (
    "EventId" UUID NOT NULL REFERENCES "Events"("Id") ON DELETE CASCADE,
    "ProblemId" UUID NOT NULL REFERENCES "CodeProblems"("Id") ON DELETE CASCADE,
    PRIMARY KEY ("EventId", "ProblemId")
);

CREATE TABLE "LeaderboardEntries" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
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
-- SECTION 9: REAL-TIME CODE BATTLES & JUDGING (FULLY MERGED & DETAILED)
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
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "Score" INTEGER NOT NULL DEFAULT 0,
    "Place" INTEGER,
    "State" TEXT,
    "DisconnectedAt" TIMESTAMPTZ,
    PRIMARY KEY ("RoomId", "AuthUserId")
);

CREATE TABLE "TestCases" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "CodeProblemId" UUID NOT NULL REFERENCES "CodeProblems"("Id") ON DELETE CASCADE,
    "LanguageId" UUID NOT NULL REFERENCES "Languages"("Id") ON DELETE RESTRICT,
    "Input" TEXT NOT NULL,
    "ExpectedOutput" TEXT NOT NULL,
    "TimeConstraint" DOUBLE PRECISION,
    "SpaceConstraint" INTEGER
);

-- This fully-featured table replaces the original "CodeSubmissions"
-- It contains all fields necessary for a detailed, external judging service.
CREATE TABLE "Submissions" (
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "AuthUserId" UUID NOT NULL REFERENCES "UserProfiles"("AuthUserId") ON DELETE CASCADE,
    "EventId" UUID NOT NULL REFERENCES "Events"("Id") ON DELETE CASCADE,
    "CodeProblemId" UUID NOT NULL REFERENCES "CodeProblems"("Id") ON DELETE CASCADE,
    "LanguageId" UUID NOT NULL REFERENCES "Languages"("Id") ON DELETE RESTRICT,
    "SourceCode" TEXT NOT NULL,
    "Status" "SubmissionStatus" NOT NULL DEFAULT 'Pending',
    
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
-- SECTION 10: INDEXES FOR PERFORMANCE (Updated)
-- ------------------------------------------
CREATE INDEX "idx_userprofiles_email" ON "UserProfiles"("Email");
CREATE INDEX "idx_enrollments_user_id" ON "UserSyllabusEnrollments"("UserProfileId");
CREATE INDEX "idx_enrollments_syllabus_id" ON "UserSyllabusEnrollments"("SyllabusId");
CREATE INDEX "idx_notes_user_id" ON "Notes"("UserProfileId");
CREATE INDEX "idx_partystashitems_party_id" ON "PartyStashItems"("PartyId");
CREATE INDEX "idx_partystashitems_shared_by_user_id" ON "PartyStashItems"("SharedByUserId");
CREATE INDEX "idx_partystashitems_original_note_id" ON "PartyStashItems"("OriginalNoteId");
CREATE INDEX "idx_partystashitems_shared_at" ON "PartyStashItems"("SharedAt");
CREATE INDEX "idx_questlines_user_class" ON "QuestLines"("AuthUserId", "ClassId");
CREATE INDEX "idx_questlines_curriculum_version" ON "QuestLines"("CurriculumVersionId");
CREATE INDEX "idx_questlines_subject" ON "QuestLines"("SubjectId");
CREATE INDEX "idx_questchapters_questline_id" ON "QuestChapters"("QuestLineId");
CREATE INDEX "idx_questchapters_sequence" ON "QuestChapters"("QuestLineId", "Sequence");
CREATE INDEX "idx_questchapters_status" ON "QuestChapters"("Status");
CREATE INDEX "idx_quests_chapter_id" ON "Quests"("QuestChapterId");
CREATE INDEX "idx_quests_subject_id" ON "Quests"("SubjectId");
CREATE INDEX "idx_quests_syllabus_id" ON "Quests"("SyllabusId");
CREATE INDEX "idx_skills_skilltree_id" ON "Skills"("SkillTreeId");
CREATE INDEX "idx_userskills_user_id" ON "UserSkills"("UserProfileId");
CREATE INDEX "idx_gamesessions_user_id" ON "GameSessions"("UserProfileId");
CREATE INDEX "idx_guildmemberships_user_id" ON "GuildMemberships"("UserProfileId");
CREATE INDEX "idx_meetings_context" ON "Meetings"("ContextId", "ContextType");
CREATE INDEX "idx_meetings_context_type" ON "Meetings"("ContextType");
CREATE INDEX "idx_meetingparticipants_user_id" ON "MeetingParticipants"("UserProfileId");
CREATE INDEX "idx_meetingagenda_meeting_id" ON "MeetingAgenda"("MeetingId");
CREATE INDEX "idx_meetingnotes_meeting_id" ON "MeetingNotes"("MeetingId");
CREATE INDEX "idx_rooms_event_id" ON "Rooms"("EventId");
CREATE INDEX "idx_roomplayers_user_id" ON "RoomPlayers"("AuthUserId");
CREATE INDEX "idx_testcases_codeproblem_id" ON "TestCases"("CodeProblemId");
CREATE INDEX "idx_submissions_user_event" ON "Submissions"("AuthUserId", "EventId");
CREATE INDEX "idx_submissions_problem_id" ON "Submissions"("CodeProblemId");
CREATE INDEX "idx_submissions_token" ON "Submissions"("Token");
CREATE INDEX "idx_user_quest_progress_user_id" ON "UserQuestProgress"("UserProfileId");
CREATE INDEX "idx_user_achievements_user_id" ON "UserAchievements"("UserProfileId");
-- Meeting participant activity tracking indexes
CREATE INDEX "idx_meeting_participant_activity_meeting_id" ON "MeetingParticipantActivity"("MeetingId");
CREATE INDEX "idx_meeting_participant_activity_user_id" ON "MeetingParticipantActivity"("UserProfileId");
CREATE INDEX "idx_meeting_participant_activity_checkin_time" ON "MeetingParticipantActivity"("CheckInTime");
CREATE INDEX "idx_meeting_participant_engagement_meeting_id" ON "MeetingParticipantEngagement"("MeetingId");
CREATE INDEX "idx_meeting_participant_engagement_user_id" ON "MeetingParticipantEngagement"("UserProfileId");
CREATE INDEX "idx_meeting_participant_engagement_timestamp" ON "MeetingParticipantEngagement"("Timestamp");
CREATE INDEX "idx_meeting_participant_stats_meeting_id" ON "MeetingParticipantStats"("MeetingId");
CREATE INDEX "idx_meeting_participant_stats_user_id" ON "MeetingParticipantStats"("UserProfileId");
-- Enhanced curriculum system indexes
CREATE INDEX "idx_curriculum_versions_program_id" ON "CurriculumVersions"("ProgramId");
CREATE INDEX "idx_curriculum_versions_effective_year" ON "CurriculumVersions"("EffectiveYear");
CREATE INDEX "idx_curriculum_structure_version_id" ON "CurriculumStructure"("CurriculumVersionId");
CREATE INDEX "idx_curriculum_structure_subject_id" ON "CurriculumStructure"("SubjectId");
CREATE INDEX "idx_curriculum_structure_semester" ON "CurriculumStructure"("PrescribedSemester");
CREATE INDEX "idx_syllabus_versions_subject_id" ON "SyllabusVersions"("SubjectId");
CREATE INDEX "idx_syllabus_versions_effective_date" ON "SyllabusVersions"("EffectiveDate");
CREATE INDEX "idx_student_enrollments_user_id" ON "StudentEnrollments"("UserProfileId");
CREATE INDEX "idx_student_enrollments_curriculum_version_id" ON "StudentEnrollments"("CurriculumVersionId");
CREATE INDEX "idx_student_term_subjects_enrollment_id" ON "StudentTermSubjects"("EnrollmentId");
CREATE INDEX "idx_student_term_subjects_subject_id" ON "StudentTermSubjects"("SubjectId");
CREATE INDEX "idx_student_term_subjects_term" ON "StudentTermSubjects"("AcademicTerm");
CREATE INDEX "idx_student_term_subjects_status" ON "StudentTermSubjects"("Status");
```

