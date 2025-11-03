### Story 5.B: Backend - Skill Tree API & Caching
- Status: To Do
- Ownership: Minh Anh (Backend)
- Target Deadline: Next Weekend

As a student, I want to view my learning path’s skill tree quickly and accurately, so I can track unlocked skills, current level/XP, and dependencies.

Scope
- Consolidates: Story 5.4 (Skill Tree computation/API), Story 5.5 (Caching and refresh).

Acceptance Criteria
1) Skill Tree Query & API
- GetLearningPathSkillTreeQuery builds nodes from skills mapped via SubjectSkillMapping to the learning path’s subjects (LearningPathQuest -> Quest -> Subject).
- Node metadata: Tier (SkillTierLevel), Stage (CurriculumStructure.Semester), IsUnlocked (any mapped quest InProgress/Completed), UserLevel/XP from UserSkill.
- Edges: from SkillDependency with RelationshipType (Prerequisite, Corequisite, Recommended).
- New controller endpoint to fetch current user’s primary learning path skill tree.

2) Caching & Refresh Strategy
- Optional cache keyed by (LearningPathId, AuthUserId) with TTL.
- Invalidation hooks on completion of ProcessAcademicRecord and on curriculum sync (Story 5.C).
- Response time target: <300ms under typical load.

Dependencies
- Learning path/quest/subject repositories, CurriculumStructure, UserSkill, SkillDependency.

Dev Notes
- Compact graph payload for client rendering (nodes, edges, minimal metadata).
- Consider depth-based collapsing or pagination for very large trees.

Definition of Done
- Query and API implemented with tests.
- Cache with invalidation hooks in place; performance meets target.