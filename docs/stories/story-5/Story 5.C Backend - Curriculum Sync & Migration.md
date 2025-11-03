### Story 5.C: Backend - Curriculum Sync & Migration
- Status: To Do
- Ownership: Minh Anh (Backend)
- Target Deadline: Next Weekend

As a student, I want my learning path to stay aligned with curriculum changes and be able to migrate to new versions with a clear preview, so that my progress remains intact.

Scope
- Consolidates: Story 5.6 (Sync), Story 5.7 (Update Event & Worker), Story 5.8 (Migration).

Acceptance Criteria
1) Sync Existing Learning Path
- SyncLearningPathWithCurriculumCommandHandler diffs current CurriculumStructure vs baseline/snapshot.
- Added subjects: create Quests and LearningPathQuest links with SequenceOrder/IsMandatory.
- Removed subjects: mark Quests IsActive=false, archive LearningPathQuest; retain UserQuestProgress.
- Reordering: recompute SequenceOrder with stable sorting; renumber only changed segments.
- Update/create QuestChapters to reflect semester changes.
- Idempotent and logs changes applied; dry-run mode returns diff plan.

2) Curriculum Update Event & Worker
- Publish CurriculumUpdated event after import/activation with payload (CurriculumVersionId, affected classes/programs, change summary).
- Background worker subscribes and runs SyncLearningPathWithCurriculum for impacted users in batches.
- Include rate limiting and retry/backoff.

3) Migrate to New Curriculum Version
- Migration flow creates a new LearningPath tied to new CurriculumVersion.
- Map UserQuestProgress by SubjectCode equivalence or mapping table; provide preview endpoint.
- Confirm stage applies changes; preserve historical progress, no hard deletions.

Data/API Changes
- Optional: IsArchived on LearningPathQuest (if not present); otherwise use Quest.IsActive and retain links.

Dependencies
- Curriculum structure repositories; learning path and quest repositories.

Definition of Done
- Sync, event publishing, worker subscription, and migration flows implemented.
- Tests cover add/remove/reorder sync cases, event->worker pipeline, and migration mapping/edge cases.