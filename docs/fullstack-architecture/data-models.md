# **Data Models**

This section defines the core data models and entities for the platform.

## **Enum Types**

```typescript
// Core enum types matching PostgreSQL schema
export type UserRole = 'Student' | 'Lecturer' | 'Admin';
export type QuestStatus = 'NotStarted' | 'InProgress' | 'Completed' | 'Failed';
export type QuestType = 'Theory' | 'Practice' | 'Project' | 'Assessment';
export type SkillLevel = 'Beginner' | 'Intermediate' | 'Advanced' | 'Expert';
export type AchievementType = 'Quest' | 'Skill' | 'Social' | 'Special';
export type NotificationType = 'Quest' | 'Achievement' | 'Social' | 'System';
export type PartyRole = 'Leader' | 'Member';
export type MeetingStatus = 'Scheduled' | 'InProgress' | 'Completed' | 'Cancelled';
export type EventType = 'Competition' | 'Workshop' | 'Seminar' | 'Social';
export type EventStatus = 'Upcoming' | 'Ongoing' | 'Completed' | 'Cancelled';
export type SubmissionStatus = 'Pending' | 'Running' | 'Accepted' | 'WrongAnswer' | 'TimeLimitExceeded' | 'RuntimeError' | 'CompileError';
export type StudentTermSubjectStatus = 'Enrolled' | 'Completed' | 'Failed' | 'Withdrawn';
export type VerificationStatus = 'Pending' | 'Approved' | 'Rejected' | 'MoreInfoRequired';
export type ParticipantStatus = 'Invited' | 'Accepted' | 'Declined' | 'Attended';
export type GameSessionStatus = 'InProgress' | 'Completed' | 'Abandoned';
export type PartyJoinType = 'Invite Only' | 'Open';
```


## **User & Profile Core**

### **UserProfile**

**Purpose:** Represents an authenticated user and their extended, game-specific profile information. The core identity is managed by **Supabase Auth**, with the `auth_user_id` directly referencing `auth.users.id` as the primary key.

**Key Attributes:**
- `auth_user_id`: `string` - The unique identifier, a **UUID** from Supabase's `auth.users` table (Primary Key).
- `username`: `string` - The user's unique public name.
- `email`: `string` - The user's unique email address.
- `first_name`: `string` - The user's first name.
- `last_name`: `string` - The user's last name.
- `class_id`: `string` - Foreign key to the selected class.
- `created_at`: `string` - Account creation timestamp.
- `updated_at`: `string` - Last profile update timestamp.

#### **TypeScript Interface**
```typescript
export interface UserProfile {
  auth_user_id: string; // Primary key - Direct reference to auth.users.id
  username: string;
  email: string;
  first_name: string;
  last_name: string;
  class_id?: string;
  created_at: string; // ISO 8601 timestamp
  updated_at: string; // ISO 8601 timestamp
}
```

### **Role & UserRole**

**Purpose:** Implements Role-Based Access Control (RBAC) for the platform.

#### **TypeScript Interface**
```typescript
export interface Role {
  id: number;
  role_name: string;
}

export interface UserRole {
  auth_user_id: string; // References user_profiles.auth_user_id
  role_id: number; // References roles.id
}
```

### **LecturerVerificationRequest**

**Purpose:** Tracks requests for lecturer verification status.

#### **TypeScript Interface**
```typescript
export interface LecturerVerificationRequest {
  id: string;
  auth_user_id: string; // References user_profiles.auth_user_id
  status: VerificationStatus;
  submitted_at: string;
  reviewed_at?: string;
  reviewer_notes?: string;
}
```

## **Academic & Content Management**

### **Class & Curriculum**

**Purpose:** Represents academic organizational structures with enhanced curriculum versioning.

#### **TypeScript Interface**
```typescript
export interface Class {
  id: string;
  name: string;
  description?: string;
}

export interface CurriculumProgram {
  id: string;
  program_name: string;
  program_code: string; // Unique identifier like "BSE" for Bachelor of Software Engineering
}

export interface CurriculumVersion {
  id: string;
  program_id: string; // References curriculum_programs.id
  version_tag: string; // "K18A", "K18B", "2024-2025 Catalog"
  effective_year: number;
  is_published: boolean; // Is it visible to new students?
}

export interface Subject {
  id: string;
  subject_code: string; // "CS464"
  title: string; // "Introduction to Machine Learning"
  credits: number;
}

export interface CurriculumStructure {
  id: string;
  curriculum_version_id: string; // References curriculum_versions.id
  subject_id: string; // References subjects.id
  term_number: number; // Which academic term (1, 2, 3, etc.)
  is_required: boolean; // Required vs elective
}

export interface SyllabusVersion {
  id: string;
  subject_id: string; // References subjects.id
  version_tag: string; // "2024-Spring", "v1.2"
  content?: Record<string, any>; // Structured syllabus content as JSONB
  effective_date: string;
  is_active: boolean;
}
```

### **Student Enrollment & Progress Tracking**

**Purpose:** Comprehensive tracking of student academic journey with enhanced curriculum support.

#### **TypeScript Interface**
```typescript
export interface StudentEnrollment {
  id: string;
  auth_user_id: string; // References user_profiles.auth_user_id
  curriculum_version_id: string; // References curriculum_versions.id
  enrollment_date: string;
  expected_graduation_date?: string;
  is_active: boolean;
}

export interface StudentTermSubject {
  id: string;
  enrollment_id: string; // References student_enrollments.id
  subject_id: string; // References subjects.id
  academic_term: string; // "2024-Spring", "2024-Fall"
  status: StudentTermSubjectStatus; // 'Enrolled', 'Completed', 'Failed', 'Withdrawn'
  grade?: string; // Final grade if completed
  enrollment_date: string;
  completion_date?: string;
}
```

### **User Skills & Progress**

**Purpose:** Tracks user skill development and quest progress for gamification.

#### **TypeScript Interface**
```typescript
export interface UserSkill {
  id: string;
  auth_user_id: string; // References user_profiles.auth_user_id
  skill_id: string; // References skills.id (from Quests Service)
  experience_points: number;
  level: number;
  last_updated: string;
}

export interface UserQuestProgress {
  id: string;
  auth_user_id: string; // References user_profiles.auth_user_id
  quest_id: string; // References quests.id (from Quests Service)
  status: QuestStatus;
  progress_percentage: number;
  started_at?: string;
  completed_at?: string;
  last_updated: string;
}
```

### **Achievement System**

**Purpose:** Comprehensive achievement tracking with type categorization.

#### **TypeScript Interface**
```typescript
export interface Achievement {
  id: string;
  title: string;
  description: string;
  type: AchievementType; // 'Quest', 'Skill', 'Social', 'Special'
  criteria?: Record<string, any>; // Achievement unlock criteria as JSONB
  reward_experience_points: number;
  icon_url?: string;
  is_active: boolean;
  created_at: string;
}

export interface UserAchievement {
  id: string;
  auth_user_id: string; // References user_profiles.auth_user_id
  achievement_id: string; // References achievements.id
  unlocked_at: string;
  context?: Record<string, any>; // Additional context about how it was earned
}
```

### **Notification System**

**Purpose:** User notification system with type-based categorization.

#### **TypeScript Interface**
```typescript
export interface Notification {
  id: string;
  auth_user_id: string; // References user_profiles.auth_user_id
  type: NotificationType; // 'Quest', 'Achievement', 'Social', 'System'
  title: string;
  message: string;
  data?: Record<string, any>; // Additional notification data as JSONB
  is_read: boolean;
  created_at: string;
}
```

## **Quest & Skill Management**

### **QuestLine & Quest System**

**Purpose:** Enhanced quest system with chapters and comprehensive dependency management.

#### **TypeScript Interface**
```typescript
export interface QuestLine {
  id: string;
  curriculum_version_id?: string; // Optional link to curriculum for academic quests
  title: string;
  description?: string;
  created_at: string;
}

export interface QuestChapter {
  id: string;
  quest_line_id: string; // References quest_lines.id
  title: string; // "Week 5", "Midterm Prep"
  description?: string;
  sequence: number; // Order within the quest line
  created_at: string;
}

export interface Quest {
  id: string;
  quest_chapter_id: string; // References quest_chapters.id
  title: string;
  description?: string;
  type: QuestType; // 'Theory', 'Practice', 'Project', 'Assessment'
  experience_points: number;
  metadata?: Record<string, any>; // Flexible quest data as JSONB
  created_at: string;
  updated_at: string;
}

export interface QuestDependency {
  quest_id: string; // References quests.id
  prerequisite_quest_id: string; // References quests.id
}
```

### **SkillTree & Skill System**

**Purpose:** Comprehensive skill development system with hierarchical dependencies.

#### **TypeScript Interface**
```typescript
export interface SkillTree {
  id: string;
  name: string;
  description?: string;
  curriculum_version_id?: string; // Optional link to curriculum
  created_at: string;
}

export interface Skill {
  id: string;
  skill_tree_id: string; // References skill_trees.id
  name: string;
  description?: string;
  experience_required: number; // XP needed to unlock this skill
  level: SkillLevel; // 'Beginner', 'Intermediate', 'Advanced', 'Expert'
  created_at: string;
}

export interface SkillDependency {
  skill_id: string; // References skills.id
  prerequisite_skill_id: string; // References skills.id
}

export interface QuestSkill {
  quest_id: string; // References quests.id
  skill_id: string; // References skills.id
  experience_points: number; // XP awarded for this skill when quest is completed
}
```

### **Game Sessions & Analytics**

**Purpose:** Comprehensive user engagement tracking and learning analytics.

#### **TypeScript Interface**
```typescript
export interface GameSession {
  id: string;
  auth_user_id: string; // References user_profiles.auth_user_id (cross-service)
  quest_id?: string; // References quests.id
  start_time: string;
  end_time?: string;
  status: GameSessionStatus; // 'InProgress', 'Completed', 'Abandoned'
  metadata?: Record<string, any>; // Session-specific data as JSONB
}

export interface SessionEvent {
  id: string;
  session_id: string; // References game_sessions.id
  event_type: string; // 'quest_start', 'skill_unlock', 'achievement_earned', etc.
  event_data?: Record<string, any>; // Event-specific data as JSONB
  timestamp: string;
}
```

### **Notes & Knowledge Management**

**Purpose:** User-generated content linked to learning objectives.

#### **TypeScript Interface**
```typescript
export interface Note {
  id: string;
  auth_user_id: string; // References user_profiles.auth_user_id (cross-service)
  title: string;
  content?: Record<string, any>; // Rich text content as JSONB
  quest_id?: string; // References quests.id
  skill_id?: string; // References skills.id
  tags?: string[];
  is_public: boolean; // Can be shared with party/guild
  created_at: string;
  updated_at: string;
}
```
  id: string;
  curriculumId: string;
  name: string;
  createdAt: string;
}

export interface Skill {
  id: string;
  skillTreeId: string;
  name: string;
  description?: string;
  maxLevel: number;
  positionX?: number;
  positionY?: number;
  createdAt: string;
}

export interface UserSkill {
  userProfileId: string;
  skillId: string;
  level: number;
}

export interface SkillDependency {
  skillId: string;
  prerequisiteSkillId: string;
}
```

## **Game Session & Notifications**

### **GameSession**

**Purpose:** Tracks individual user attempts at interactive game events.

#### **TypeScript Interface**
```typescript
export interface GameSession {
  id: string;
  userProfileId: string;
  questId: string;
  status: GameSessionStatus;
  score: number;
  progressData?: Record<string, any>;
  startedAt: string;
  completedAt?: string;
}
```

### **Notification**

**Purpose:** User notification system.

#### **TypeScript Interface**
```typescript
export interface Notification {
  id: string;
  userProfileId: string;
  message: string;
  isRead: boolean;
  link?: string;
  createdAt: string;
}
```

## **Social & Community**

### **Party & PartyMembership**

**Purpose:** Small, private study groups for focused collaboration with role-based management.

#### **TypeScript Interface**
```typescript
export interface Party {
  id: string;
  name: string;
  description?: string;
  leader_id: string; // References user_profiles.auth_user_id (cross-service)
  max_members?: number;
  is_private: boolean;
  created_at: string;
}

export interface PartyMembership {
  party_id: string; // References parties.id
  auth_user_id: string; // References user_profiles.auth_user_id (cross-service)
  role: PartyRole; // 'Leader', 'Member'
  joined_at: string;
}
```

### **Guild & GuildMembership**

**Purpose:** Larger community-focused groups for knowledge sharing and competitions.

#### **TypeScript Interface**
```typescript
export interface Guild {
  id: string;
  name: string;
  description?: string;
  master_id: string; // References user_profiles.auth_user_id (cross-service)
  is_verified: boolean;
  created_at: string;
}

export interface GuildMembership {
  guild_id: string; // References guilds.id
  auth_user_id: string; // References user_profiles.auth_user_id (cross-service)
  joined_at: string;
}
```

## **Meetings & Collaboration**

### **Meeting System**

**Purpose:** Comprehensive meeting management with analytics, engagement tracking, and multi-context support.

#### **TypeScript Interface**
```typescript
export interface Meeting {
  id: string;
  party_id?: string; // References parties.id (optional for non-party meetings)
  creator_id: string; // References user_profiles.auth_user_id (cross-service)
  title: string;
  description?: string;
  scheduled_start_time: string;
  scheduled_end_time?: string;
  actual_start_time?: string;
  actual_end_time?: string;
  status: EventStatus; // 'Scheduled', 'InProgress', 'Completed', 'Cancelled'
  meeting_url?: string;
  created_at: string;
}

export interface MeetingParticipant {
  meeting_id: string; // References meetings.id
  auth_user_id: string; // References user_profiles.auth_user_id (cross-service)
  status: ParticipantStatus; // 'Invited', 'Accepted', 'Declined', 'Attended'
  joined_at?: string;
  left_at?: string;
}

export interface MeetingAgenda {
  id: string;
  meeting_id: string; // References meetings.id
  title: string;
  description?: string;
  estimated_duration?: number; // Minutes
  actual_duration?: number; // Minutes
  sequence: number; // Order in the agenda
  created_at: string;
}

export interface MeetingNote {
  id: string;
  meeting_id: string; // References meetings.id
  auth_user_id: string; // References user_profiles.auth_user_id (cross-service)
  content: Record<string, any>; // Rich text content as JSONB
  timestamp: string; // When the note was taken during the meeting
  created_at: string;
}

export interface MeetingParticipantActivity {
  id: string;
  meeting_id: string; // References meetings.id
  auth_user_id: string; // References user_profiles.auth_user_id (cross-service)
  activity_type: string; // 'join', 'leave', 'mute', 'unmute', 'camera_on', 'camera_off'
  connection_quality?: number; // 1-5 scale
  timestamp: string;
}

export interface MeetingParticipantEngagement {
  id: string;
  meeting_id: string; // References meetings.id
  auth_user_id: string; // References user_profiles.auth_user_id (cross-service)
  speaking_time: number; // Total speaking time in seconds
  chat_messages: number; // Number of chat messages sent
  reactions_given: number; // Number of reactions/emojis used
  polls_participated: number; // Number of polls participated in
}

export interface MeetingParticipantStats {
  id: string;
  meeting_id: string; // References meetings.id
  auth_user_id: string; // References user_profiles.auth_user_id (cross-service)
  attention_score?: number; // AI-calculated attention score (1-100)
  participation_score?: number; // Overall participation score (1-100)
  contribution_rating?: number; // Peer or self-rating (1-5)
  created_at: string;
}
```

## **Events & Competition**

### **Event Management**

**Purpose:** Comprehensive event system supporting various competition types and formats.

#### **TypeScript Interface**
```typescript
export interface Event {
  id: string;
  title: string;
  description?: string;
  type: EventType; // 'Competition', 'Workshop', 'Assessment', 'Social'
  status: EventStatus; // 'Scheduled', 'InProgress', 'Completed', 'Cancelled'
  start_time: string;
  end_time?: string;
  max_participants?: number;
  registration_deadline?: string;
  created_at: string;
}

export interface EventParticipant {
  event_id: string; // References events.id
  auth_user_id: string; // References user_profiles.auth_user_id (cross-service)
  registered_at: string;
  participated_at?: string;
  final_score?: number;
  final_rank?: number;
}
```

### **Code Battle System**

**Purpose:** Programming competition infrastructure with comprehensive judging and analytics.

#### **TypeScript Interface**
```typescript
export interface CodeProblem {
  id: string;
  title: string;
  problem_statement: string;
  difficulty: number;
  created_at: string;
}

export interface EventCodeProblem {
  event_id: string; // References events.id
  code_problem_id: string; // References code_problems.id
  sequence?: number; // Order in the event
}

export interface Language {
  id: string;
  name: string; // 'Python', 'Java', 'C++', etc.
  compile_cmd: string;
  run_cmd: string;
}

export interface Tag {
  id: string;
  name: string;
  created_at: string;
}

export interface CodeProblemTag {
  code_problem_id: string; // References code_problems.id
  tag_id: string; // References tags.id
}

export interface CodeProblemLanguageDetails {
  id: string;
  code_problem_id: string; // References code_problems.id
  language_id: string; // References languages.id
  starter_code?: string;
  solution_template?: string;
}

export interface Room {
  id: string;
  event_id: string; // References events.id
  name: string;
  description?: string;
  created_at: string;
}

export interface RoomPlayer {
  room_id: string; // References rooms.id
  auth_user_id: string; // References user_profiles.auth_user_id (cross-service)
  score: number;
  place?: number;
  state?: string; // 'Active', 'Disconnected', 'Finished'
  disconnected_at?: string;
}

export interface TestCase {
  id: string;
  code_problem_id: string; // References code_problems.id
  input: object; // JSON input data
  expected_output: object; // JSON expected output data
  is_hidden: boolean; // Whether test case is hidden from users
}

export interface Submission {
  id: string;
  user_id: string; // References user_profiles.auth_user_id (cross-service)
  code_problem_id: string; // References code_problems.id
  language_id: string; // References languages.id
  room_id: string; // References rooms.id
  code_submitted: string;
  status: string; // 'pending', 'running', 'accepted', 'wrong_answer', etc.
  execution_time_ms?: number;
  submitted_at: string;
}

export interface EventGuildParticipant {
  event_id: string; // References events.id
  guild_id: string; // References guilds.id (cross-service)
  registered_at: string;
}
```

### **Leaderboard System**

**Purpose:** Ranking and achievement tracking for individuals and guilds.

#### **TypeScript Interface**
```typescript
export interface LeaderboardEntry {
  id: string;
  user_id: string; // References user_profiles.auth_user_id (cross-service)
  event_id: string; // References events.id (required for event-based rankings)
  rank: number;
  score: number;
  snapshot_date: string; // UTC timestamp for ranking snapshot
}

export interface GuildLeaderboardEntry {
  id: string;
  guild_id: string; // References guilds.id
  event_id: string; // References events.id (required for event-based rankings)
  rank: number;
  total_score: number;
  member_count: number;
  snapshot_date: string; // UTC timestamp for ranking snapshot
}
```

## **Composite Types for API Responses**

```typescript
// Extended types for API responses with joined data
export interface UserProfileWithDetails extends UserProfile {
  class?: Class;
  curriculum?: Curriculum;
  roles?: Role[];
}

export interface QuestWithPrerequisites extends Quest {
  prerequisites?: Quest[];
}

export interface SkillWithDependencies extends Skill {
  dependencies?: Skill[];
  user_level?: number;
}

export interface PartyWithMembers extends Party {
  members?: UserProfile[];
  member_count: number;
}

export interface GuildWithMembers extends Guild {
  members?: UserProfile[];
  member_count: number;
}

export interface EventWithDetails extends Event {
  guild?: Guild;
  problems?: CodeProblem[];
  submissions?: Submission[];
}

export interface MeetingWithDetails extends Meeting {
  party?: Party;
  creator?: UserProfile;
  participants?: (MeetingParticipant & { user?: UserProfile })[];
  agenda?: MeetingAgenda[];
  notes?: MeetingNote[];
}

export interface RoomWithDetails extends Room {
  event?: Event;
  players?: (RoomPlayer & { user?: UserProfile })[];
}

export interface SubmissionWithDetails extends Submission {
  user?: UserProfile;
  event?: Event;
  problem?: CodeProblem;
  language?: Language;
  test_cases?: TestCase[];
}
```


## **CurriculumPack (AI-Generated Content Packs)**

**Purpose:** Deliver vetted, versioned content generated from a subject/curriculum to the Unity client via backend APIs. Supports curated sets and procedural (seeded) generation with full auditability.

#### **TypeScript Interfaces**
```typescript
export type CurriculumQuestionType = 'MCQ' | 'FreeText';

export interface CurriculumOption {
  id: string; // Option identifier (unique within question)
  text: string;
}

export interface CurriculumQuestion {
  id: string; // Unique within the pack
  type: CurriculumQuestionType;
  text: string;
  options?: CurriculumOption[]; // Required for MCQ
  correctAnswers?: string[]; // For MCQ: option ids; for Code/FreeText: rubric keys or expected tokens
  explanation?: string;
  difficulty?: 'Beginner' | 'Intermediate' | 'Advanced';
  tags?: string[]; // e.g., ['loops', 'conditionals']
  sequence?: number; // Rendering order
  weight?: number; // Default 1
  timeLimitSec?: number; // Optional per-item time limit
  metadata?: Record<string, any>; // e.g., language for code, unit tests ref, asset refs
}

export type CurriculumSource = 'Set' | 'Seed';

export interface CurriculumPackMeta {
  packId: string; // UUID
  subjectId?: string;
  curriculumId?: string;
  version: string; // SemVer, e.g., 1.0.0
  aiModel?: string; // e.g., 'gpt-4o-mini'
  generatorVersion?: string; // Content pipeline version
  promptHash?: string; // Hash of prompt/template for reproducibility
  seed?: string; // If procedural
  approvedBy?: string; // Human approver (for assessments)
  createdAt: string; // ISO timestamp
  source: CurriculumSource; // 'Set' (curated) or 'Seed' (procedural)
  ephemeral?: boolean; // If true, generated for a single run and not persisted
}

export interface CurriculumPack {
  meta: CurriculumPackMeta;
  items: CurriculumQuestion[]; // Snapshot of served questions/items
}
#### BossFightQuestionPack (Specialization)

This is a specialization of CurriculumPack for the Boss Fight loop.

```typescript
export interface BossFightQuestionPack extends CurriculumPack {
  meta: CurriculumPackMeta & { type: 'BossFightQuestionPack' };
  items: Array<{
    id: string;
    type: 'MCQ';
    text: string;
    choices: string[]; // Unity-friendly
    correctIndex: number; // Single-correct for fast combat resolution
    timeLimitSec?: number; // Default 25s
    tags?: string[];
  }>;
}
```

Minimal JSON example used by Unity:

```json
{
  "meta": { "packId": "uuid", "version": "1.0.0", "source": "Seed", "ephemeral": true, "type": "BossFightQuestionPack" },
  "items": [
    { "id": "q1", "type": "MCQ", "text": "What is 2+2?", "choices": ["3", "4", "5"], "correctIndex": 1, "timeLimitSec": 20 }
  ]
}
```

#### JSON Schema (Draft 2020-12)
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://bmad.local/schemas/curriculum-pack.json",
  "title": "CurriculumPack",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "meta": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "packId": { "type": "string", "format": "uuid" },
        "subjectId": { "type": ["string", "null"], "format": "uuid" },
        "curriculumId": { "type": ["string", "null"], "format": "uuid" },
        "version": {
          "type": "string",
          "pattern": "^[0-9]+\\.[0-9]+\\.[0-9]+(?:-[A-Za-z0-9.-]+)?$"
        },
        "aiModel": { "type": ["string", "null"], "maxLength": 100 },
        "generatorVersion": { "type": ["string", "null"], "maxLength": 50 },
        "promptHash": { "type": ["string", "null"], "maxLength": 128 },
        "seed": { "type": ["string", "null"], "maxLength": 64 },
        "approvedBy": { "type": ["string", "null"], "maxLength": 100 },
        "createdAt": { "type": "string", "format": "date-time" },
        "source": { "type": "string", "enum": ["Set", "Seed"] },
        "ephemeral": { "type": ["boolean", "null"] },
        "type": { "type": ["string", "null"], "enum": ["BossFightQuestionPack"] }
      },
      "required": ["packId", "version", "createdAt", "source"]
    },
    "items": {
      "type": "array",
      "minItems": 1,
      "maxItems": 200,
      "items": {
        "type": "object",
        "additionalProperties": true,
        "properties": {
          "id": { "type": "string", "minLength": 1, "maxLength": 64 },
          "type": { "type": "string", "enum": ["MCQ", "Code", "FreeText"] },
          "text": { "type": "string", "minLength": 1, "maxLength": 4000 },
          "options": {
            "type": "array",
            "items": {
              "type": "object",
              "additionalProperties": false,
              "properties": {
                "id": { "type": "string", "minLength": 1, "maxLength": 64 },
                "text": { "type": "string", "minLength": 1, "maxLength": 2000 }
              },
              "required": ["id", "text"]
            },
            "minItems": 2,
            "maxItems": 10
          },
          "correctAnswers": {
            "type": "array",
            "items": { "type": "string" },
            "minItems": 1,
            "maxItems": 10
          },
          "explanation": { "type": ["string", "null"], "maxLength": 4000 },
          "difficulty": { "type": ["string", "null"], "enum": ["Beginner", "Intermediate", "Advanced"] },
          "tags": {
            "type": "array",
            "items": { "type": "string", "minLength": 1, "maxLength": 50 },
            "maxItems": 20
          },
          "sequence": { "type": ["integer", "null"], "minimum": 0, "maximum": 10000 },
          "weight": { "type": ["number", "null"], "minimum": 0, "maximum": 100 },
          "timeLimitSec": { "type": ["integer", "null"], "minimum": 10, "maximum": 3600 },
          "metadata": { "type": ["object", "null"] }
        },
        "required": ["id", "type", "text"],
        "allOf": [
          {
            "if": { "properties": { "type": { "const": "MCQ" } }, "required": ["type"] },
            "then": { "required": ["options", "correctAnswers"] }
          }
        ]
      }
    }
  },
  "required": ["meta", "items"]
}

```

> Implementation note
- Keep total payloads small (e.g., < 300 KB) for WebGL delivery; consider chunking or presigned URLs for large attachments.
- Always snapshot items into the session (e.g., `SessionQuestions`) to ensure reproducibility and fair grading.

