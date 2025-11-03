### Story 5.D: Backend - Specialization & Minor Integration
- Status: To Do
- Ownership: Minh Anh (Backend)
- Target Deadline: Next Weekend

As a student, I want my learning path to include specialization placeholders, allow minor selection, refresh gaps with specialization context, and generate AI quest steps that reflect my minor, so that my plan is personalized and actionable.

Scope
- Consolidates: Story 5.9 (Placeholders), Story 5.10 (Minor selection), Story 5.11 (Gap analysis refresh), Story 5.12 (AI quest steps).

Acceptance Criteria
1) Forge-Time Specialization Placeholders
- ForgeLearningPath includes placeholder chapters/labels where specialization subjects will appear.
- GetMyLearningPathQuery surfaces placeholders flagged as Placeholder.

2) Minor Selection Command
- SelectMinorCommandHandler updates UserProfile with MinorId (or SpecializationChoice).
- Resolve specialization subjects via ClassSpecializationSubjectRepository.
- Create Quests and LearningPathQuest links; set IsMandatory per class rules.
- Place quests into correct semesters via CurriculumStructure or Specialization chapter.
- Idempotent behavior on repeated selections; logs actions/outcomes.

3) Gap Analysis Refresh
- AnalyzeLearningGapCommandHandler reads MinorId and specialized required subjects.
- ForgingPayload includes specialization SubjectGaps and rationale.

4) AI Quest Step Generation
- GenerateQuestStepsCommandHandler passes class/minor context into IQuestGenerationPlugin.
- Steps refreshed for specialization subjects after minor selection.

Data Changes
- Add MinorId (or SpecializationChoice) to UserProfile.
- Ensure specialization mapping tables exist.

Non-functional Considerations
- Idempotency: Use SourceId keyed to MinorId or subject Ids when creating specialization quests and steps.
- UX: Mark specialization vs core content in learning path responses.

Definition of Done
- Placeholders visible; minor selection updates path; gap analysis considers specialization; AI steps reflect minor.
- Tests validate idempotency and context-aware generation.