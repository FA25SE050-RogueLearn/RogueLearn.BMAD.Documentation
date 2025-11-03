### Story 5.A: Backend - Academic Integration & Skill XP (Subjects→Skills, Awards, GPA Bonus)
- Status: To Do
- Ownership: Minh Anh (Backend), support from Phuc
- Target Deadline: Next Weekend

As a system, I need to translate academic outcomes into game progression by mapping subjects to skills, awarding XP when processing transcripts, and granting a GPA-based bonus, so that students’ skills reflect their academic performance.

Scope
- Consolidates: Story 5.1 (Subject→Skill mapping), Story 5.2 (Award XP on transcript), Story 5.3 (GPA bonus XP).

Acceptance Criteria
1) Subject→Skill Mapping
- Entity SubjectSkillMapping: SubjectId, SkillId, Weight [0–1], CreatedAt.
- Repository registered with CRUD; Supabase RLS: only Admin/GameMaster can write.
- Admin seeding/updates available via service/admin API.

2) XP Awards on Transcript Processing
- ProcessAcademicRecordCommandHandler:
  - For each passed subject, compute XP: base_per_credit × subject.Credits × grade_factor × weight.
  - grade_factor defaults: A=1.0, B+=0.9, B=0.8, C=0.6, Passed(no-grade)=0.5.
  - Ingest each award via IngestXpEventCommand with SourceType=AcademicRecord, SourceId=SubjectId, UserId for idempotency.
- Persist in user_skill_rewards and update UserSkill XP/Level via existing handler.
- Structured logs show per-skill and total awarded XP.

3) GPA Bonus XP
- Compute xp_bonus = gpa × bonus_multiplier × total_credits_completed (multiplier configurable).
- Ingest as a single event with SourceId=EnrollmentId, SourceType=AcademicRecord for idempotency.
- Target: a general skill (Academic Mastery) or distribute proportionally to mapped skills (configurable).

Data/API Changes
- New table: subject_skill_mappings.
- Add enum SkillRewardSourceType.AcademicRecord.
- Optional: Add "Academic Mastery" to the skill catalog if not present.
- Optional admin endpoints: POST/PUT /api/admin/catalog/subject-skill-mapping.

Dependencies
- Skill and Subject catalogs; UserSkill/UserSkillReward repositories; IngestXpEventCommandHandler.

Security & Idempotency
- RLS prevents end-user writes to mappings.
- Uniqueness on (SourceType, SourceId, UserId) ensures awards are processed once.

Dev Notes
- Validate weight range; recommend sum of weights per subject ≤ 1.
- Indexes on (SubjectId) and (SkillId) for joins.
- Support partial transcripts: process only new passed subjects.

Definition of Done
- Mapping table, RLS, repository, and admin seed/update implemented.
- Transcript processing awards XP and GPA bonus via ingest command, idempotent.
- Tests cover mapping validation, grade factors, credit-weight computations, and idempotency.
- Logs demonstrate per-skill and total awards.