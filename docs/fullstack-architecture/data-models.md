# **Data Models**

This section defines the core data models and entities for the platform.

## **Enum Types**

```typescript
// Core enum types matching PostgreSQL schema
export type QuestStatus = 'Not Started' | 'In Progress' | 'Completed';
export type QuestType = 'Main' | 'Side' | 'Daily' | 'Assignment' | 'Exam' | 'BossFight';
export type SyllabusProcessingStatus = 'Pending' | 'Processing' | 'Completed' | 'Failed';
export type GameSessionStatus = 'InProgress' | 'Completed' | 'Abandoned';
export type PartyJoinType = 'Invite Only' | 'Open';
export type EventType = 'Quiz' | 'CodeBattle' | 'Tournament' | 'Duel';
export type SubmissionStatus = 'In Queue' | 'Processing' | 'Accepted' | 'Wrong Answer' | 'Time Limit Exceeded' | 'Compilation Error' | 'Runtime Error (Generic)' | 'Internal Error' | 'Execution Server Error';
export type VerificationStatus = 'Pending' | 'Approved' | 'Rejected' | 'MoreInfoRequired';
export type MeetingStatus = 'Scheduled' | 'InProgress' | 'Completed' | 'Cancelled';
export type ParticipantStatus = 'Invited' | 'Accepted' | 'Declined' | 'Attended';
```

## **User & Profile Core**

### **UserProfile**

**Purpose:** Represents an authenticated user and their extended, game-specific profile information. The core identity is managed by **Supabase Auth**, with the `Id` directly referencing `auth.users.id`.

**Key Attributes:**
- `id`: `string` - The unique identifier, a **UUID** from Supabase's `auth.users` table.
- `username`: `string` - The user's public name.
- `email`: `string` - The user's email address.
- `classId`: `string` - Foreign key to the selected class.
- `curriculumId`: `string` - Foreign key to the selected curriculum.
- `level`: `number` - The character's current level.
- `experiencePoints`: `number` - The character's current XP.
- `stats`: `object` - Game-specific statistics stored as JSONB.

#### **TypeScript Interface**
```typescript
export interface UserProfile {
  id: string; // Direct reference to auth.users.id
  username: string;
  email: string;
  classId?: string;
  curriculumId?: string;
  level: number;
  experiencePoints: number;
  stats?: Record<string, any>;
  onboardingCompleted: boolean;
  createdAt: string; // ISO 8601 timestamp
  updatedAt: string; // ISO 8601 timestamp
}
```

### **Role & UserRole**

**Purpose:** Implements Role-Based Access Control (RBAC) for the platform.

#### **TypeScript Interface**
```typescript
export interface Role {
  id: number;
  roleName: string;
}

export interface UserRole {
  userProfileId: string;
  roleId: number;
}
```

### **LecturerVerificationRequest**

**Purpose:** Tracks requests for lecturer verification status.

#### **TypeScript Interface**
```typescript
export interface LecturerVerificationRequest {
  id: string;
  userProfileId: string;
  status: VerificationStatus;
  submittedAt: string;
  reviewedAt?: string;
  reviewerNotes?: string;
}
```

## **Academic & Content Management**

### **Class & Curriculum**

**Purpose:** Represents academic organizational structures.

#### **TypeScript Interface**
```typescript
export interface Class {
  id: string;
  name: string;
  description?: string;
}

export interface Curriculum {
  id: string;
  name: string;
  description?: string;
}
```

### **Syllabus & Enrollment**

**Purpose:** Represents course syllabuses and user enrollments.

#### **TypeScript Interface**
```typescript
export interface Syllabus {
  id: string;
  curriculumId: string;
  courseCode: string;
  courseTitle: string;
  description?: string;
  credits?: number;
}

export interface UserSyllabusEnrollment {
  id: string;
  userProfileId: string;
  syllabusId: string;
  enrollmentDate: string;
}

export interface UserUploadedSyllabus {
  id: string;
  enrollmentId: string;
  fileUrl: string;
  processingStatus: SyllabusProcessingStatus;
  structuredContent?: Record<string, any>;
  uploadedAt: string;
}
```

### **Note (Arsenal Item)**

**Purpose:** Represents user-generated knowledge stored in their "Arsenal."

#### **TypeScript Interface**
```typescript
export interface Note {
  id: string;
  userProfileId: string;
  title: string;
  content?: Record<string, any>; // Rich text JSON
  syllabusId?: string;
  questId?: string;
  skillId?: string;
  tags?: string[];
  createdAt: string;
  updatedAt: string;
}
```

## **Quest & Skill Management**

### **QuestLine & Quest**

**Purpose:** The `QuestLine` represents the entire learning path for a specific syllabus. Each `QuestLine` contains individual `Quests`.

#### **TypeScript Interface**
```typescript
export interface QuestLine {
  id: string;
  syllabusId: string;
  title: string;
  createdAt: string;
}

export interface Quest {
  id: string;
  questLineId: string;
  title: string;
  description?: string;
  type: QuestType;
  status: QuestStatus;
  dueDate?: string;
  experiencePoints: number;
  createdAt: string;
  updatedAt: string;
}

export interface QuestPrerequisite {
  questId: string;
  prerequisiteQuestId: string;
}
```

### **SkillTree & Skill**

**Purpose:** Visual representation of knowledge mastery for a curriculum.

#### **TypeScript Interface**
```typescript
export interface SkillTree {
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

**Purpose:** Small, private study groups for focused collaboration.

#### **TypeScript Interface**
```typescript
export interface Party {
  id: string;
  name: string;
  description?: string;
  joinType: PartyJoinType;
  leaderId: string;
  createdAt: string;
}

export interface PartyMembership {
  partyId: string;
  userProfileId: string;
  joinedAt: string;
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
  masterId: string;
  isVerified: boolean;
  createdAt: string;
}

export interface GuildMembership {
  guildId: string;
  userProfileId: string;
  joinedAt: string;
}
```

## **Meetings & Collaboration**

### **Meeting System**

**Purpose:** Manages party meetings, scheduling, agendas, and collaborative features.

#### **TypeScript Interface**
```typescript
export interface Meeting {
  id: string;
  partyId: string;
  creatorId: string;
  title: string;
  description?: string;
  scheduledStartTime: string;
  scheduledEndTime?: string;
  status: MeetingStatus;
  createdAt: string;
}

export interface MeetingParticipant {
  meetingId: string;
  userProfileId: string;
  status: ParticipantStatus;
}

export interface MeetingAgenda {
  id: string;
  meetingId: string;
  topic: string;
  description?: string;
  presenterId?: string;
  sequence: number;
  durationMinutes?: number;
}

export interface MeetingNote {
  id: string;
  meetingId: string;
  agendaId?: string;
  creatorId: string;
  content: Record<string, any>;
  createdAt: string;
  updatedAt: string;
}
```

## **Events & Competition**

### **Event & Competition System**

**Purpose:** Competitive events hosted by guilds.

#### **TypeScript Interface**
```typescript
export interface Event {
  id: string;
  guildId?: string;
  title: string;
  description?: string;
  type: EventType;
  startDate?: string;
  endDate?: string;
}

export interface CodeProblem {
  id: string;
  title: string;
  problemStatement: string;
}

export interface EventCodeProblem {
  eventId: string;
  problemId: string;
}

export interface Language {
  id: string;
  name: string;
  compileCmd?: string;
  runCmd: string;
  timeoutSeconds?: number;
}

export interface Room {
  id: string;
  eventId: string;
  name: string;
  description?: string;
  createdAt: string;
}

export interface RoomPlayer {
  roomId: string;
  userProfileId: string;
  score: number;
  place?: number;
  state?: string;
  disconnectedAt?: string;
}

export interface TestCase {
  id: string;
  codeProblemId: string;
  input: string;
  expectedOutput: string;
  timeConstraint?: number;
  spaceConstraint?: number;
}

export interface Submission {
  id: string;
  userProfileId: string;
  eventId: string;
  codeProblemId: string;
  languageId: string;
  sourceCode: string;
  status: SubmissionStatus;
  
  // Execution Results
  stdOut?: string;
  stdErr?: string;
  compileOutput?: string;
  exitCode?: number;
  exitSignal?: number;
  message?: string;
  
  // Performance Metrics
  time?: number;
  memory?: number;
  wallTime?: number;
  
  // Judge Tracking & Configuration
  token?: string;
  callbackUrl?: string;
  numberOfRuns?: number;
  compilerOptions?: string;
  commandLineArguments?: string;
  redirectStdErrToStdOut?: boolean;
  additionalFiles?: ArrayBuffer;
  enableNetwork?: boolean;
  
  // Judge Resource Limits
  cpuTimeLimit?: number;
  cpuExtraTime?: number;
  wallTimeLimit?: number;
  memoryLimit?: number;
  stackLimit?: number;
  maxProcessesAndOrThreads?: number;
  enablePerProcessAndThreadTimeLimit?: boolean;
  enablePerProcessAndThreadMemoryLimit?: boolean;
  maxFileSize?: number;
  
  // Timestamps & Host Info
  queuedAt: string;
  startedAt?: string;
  finishedAt?: string;
  updatedAt: string;
  queueHost?: string;
  executionHost?: string;
}

export interface LeaderboardEntry {
  id: string;
  userProfileId: string;
  eventId?: string;
  rank: number;
  score: number;
  snapshotDate: string;
}

export interface GuildLeaderboardEntry {
  id: string;
  guildId: string;
  eventId?: string;
  rank: number;
  totalScore: number;
  snapshotDate: string;
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
  userLevel?: number;
}

export interface PartyWithMembers extends Party {
  members?: UserProfile[];
  memberCount: number;
}

export interface GuildWithMembers extends Guild {
  members?: UserProfile[];
  memberCount: number;
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
  testCases?: TestCase[];
}
```

