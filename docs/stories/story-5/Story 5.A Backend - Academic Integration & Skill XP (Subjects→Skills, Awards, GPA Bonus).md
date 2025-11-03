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
  - grade_factor aligned with PRD FR5, FR7, and FR8:
    - If numerical grade (FPT scale 1–10) is available: grade_factor = grade / 10.0 (e.g., 8.5 → 0.85).
    - If only letter grade is available: convert to numeric, then grade_factor = numeric / 10.0. Recommended mapping: A=9.5, B+=8.5, B=8.0, C=6.5, D=5.0, S/P (pass/no-grade)=6.0. F (fail) is excluded from "passed subject" processing.
    - weight corresponds to the Subject→Skill relevance score from the mapping; keep within [0–1].
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
 - Tests cover mapping validation, grade factor conversion (numeric and letter per FR5/FR7), credit-weight computations aligned with FR8, and idempotency.
- Logs demonstrate per-skill and total awards.