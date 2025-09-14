# **Database Schema**

This section provides the SQL DDL for the PostgreSQL database.

```sql
-- RogueLearn Database Schema for PostgreSQL 
-- FINAL VERSION: Integrated with Supabase Auth and combines all refined concepts. 

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
CREATE TYPE "CodeSubmissionStatus" AS ENUM ('Pending', 'Accepted', 'WrongAnswer', 'TimeLimitExceeded', 'CompilationError'); 
CREATE TYPE "VerificationStatus" AS ENUM ('Pending', 'Approved', 'Rejected', 'MoreInfoRequired'); 


-- ------------------------------------------ 
-- SECTION 2: USER & PROFILE CORE 
-- ------------------------------------------ 

-- This table holds public user data and is linked 1-to-1 with Supabase's auth.users table. 
-- The Id column MUST be the same UUID as the user's ID in auth.users. 
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

-- Defines system roles for RBAC 
CREATE TABLE "Roles" ( 
    "Id" SERIAL PRIMARY KEY, 
    "RoleName" TEXT UNIQUE NOT NULL 
); 

-- Assigns roles to users 
CREATE TABLE "UserRoles" ( 
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE, 
    "RoleId" INT NOT NULL REFERENCES "Roles"("Id") ON DELETE RESTRICT, 
    PRIMARY KEY ("UserProfileId", "RoleId") 
); 

-- Tracks requests for lecturer verification 
CREATE TABLE "LecturerVerificationRequests" ( 
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(), 
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE, 
    "Status" "VerificationStatus" NOT NULL DEFAULT 'Pending', 
    "SubmittedAt" TIMESTAMPTZ NOT NULL DEFAULT now(), 
    "ReviewedAt" TIMESTAMPTZ, 
    "ReviewerNotes" TEXT 
); 


-- ------------------------------------------ 
-- SECTION 3: ACADEMIC & CONTENT MANAGEMENT 
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

-- Junction table to track which users are enrolled in which syllabuses 
CREATE TABLE "UserSyllabusEnrollments" ( 
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(), 
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE, 
    "SyllabusId" UUID NOT NULL REFERENCES "Syllabuses"("Id") ON DELETE CASCADE, 
    "EnrollmentDate" TIMESTAMPTZ NOT NULL DEFAULT now(), 
    UNIQUE ("UserProfileId", "SyllabusId") 
); 

-- Tracks the processing of a user's uploaded syllabus file for a specific enrollment 
CREATE TABLE "UserUploadedSyllabuses" ( 
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(), 
    "EnrollmentId" UUID NOT NULL REFERENCES "UserSyllabusEnrollments"("Id") ON DELETE CASCADE, 
    "FileUrl" TEXT NOT NULL, -- Link to file in Supabase Storage 
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
    "QuestId" UUID, -- Forward reference 
    "SkillId" UUID, -- Forward reference 
    "Tags" TEXT[], 
    "CreatedAt" TIMESTAMPTZ NOT NULL DEFAULT now(), 
    "UpdatedAt" TIMESTAMPTZ NOT NULL DEFAULT now() 
); 


-- ------------------------------------------ 
-- SECTION 4: QUEST & SKILL MANAGEMENT 
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

-- Add forward-referenced foreign keys for Notes 
ALTER TABLE "Notes" ADD CONSTRAINT fk_notes_quest FOREIGN KEY ("QuestId") REFERENCES "Quests"("Id") ON DELETE SET NULL; 
ALTER TABLE "Notes" ADD CONSTRAINT fk_notes_skill FOREIGN KEY ("SkillId") REFERENCES "Skills"("Id") ON DELETE SET NULL; 


-- ------------------------------------------ 
-- SECTION 5: GAME SESSION & NOTIFICATIONS 
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
-- SECTION 6: SOCIAL & COMMUNITY 
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


-- ------------------------------------------ 
-- SECTION 7: EVENTS & COMPETITION 
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

CREATE TABLE "CodeProblems" ( 
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(), 
    "Title" TEXT NOT NULL, 
    "ProblemStatement" TEXT NOT NULL, 
    "TestCases" JSONB NOT NULL 
); 

CREATE TABLE "EventCodeProblems" ( 
    "EventId" UUID NOT NULL REFERENCES "Events"("Id") ON DELETE CASCADE, 
    "ProblemId" UUID NOT NULL REFERENCES "CodeProblems"("Id") ON DELETE CASCADE, 
    PRIMARY KEY ("EventId", "ProblemId") 
); 

CREATE TABLE "CodeSubmissions" ( 
    "Id" UUID PRIMARY KEY DEFAULT gen_random_uuid(), 
    "EventId" UUID NOT NULL REFERENCES "Events"("Id") ON DELETE CASCADE, 
    "ProblemId" UUID NOT NULL REFERENCES "CodeProblems"("Id") ON DELETE CASCADE, 
    "UserProfileId" UUID NOT NULL REFERENCES "UserProfiles"("Id") ON DELETE CASCADE, 
    "GuildId" UUID NOT NULL REFERENCES "Guilds"("Id") ON DELETE CASCADE, 
    "Code" TEXT NOT NULL, 
    "Language" TEXT NOT NULL, 
    "Status" "CodeSubmissionStatus" NOT NULL, 
    "SubmittedAt" TIMESTAMPTZ NOT NULL DEFAULT now() 
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
-- SECTION 8: INDEXES FOR PERFORMANCE 
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
CREATE INDEX "idx_codesubmissions_user_event" ON "CodeSubmissions"("UserProfileId", "EventId");
```

