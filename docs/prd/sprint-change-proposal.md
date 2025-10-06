# Sprint Change Proposal — Architecture Readjustment

Objective Types, Workspace Routing, Verification Pipeline, Skill Graph, and Reward Orchestration

## Executive Summary
- Introduce `Objective.type` with workspace-specific routing (Mission Control, Code Arena)
- Implement conditional completion policies and automated verification for local projects
- Establish a Skill Knowledge Graph to power micro-objective generation and contextual unlocks
- Add Reward Cascade via domain events for XP, Skill Points, and unlocks
- Adjust AI-driven quest logic in FR9/FR20 to create follow-up micro-objectives

## Change Objectives
- Align backend domain model to support type-driven UX and completion logic
- Reduce manual grading through verification automation; integrate Code Battle Service for coding tests
- Increase engagement via real-time suggestions and unlocks with coherent event orchestration

## Architecture Overview
- Current: Quests Service generates quests; Code Battle Service handles competitions/tests; limited verification automation; rewards embedded per-service
- Proposed: Modular domain with clear separation of concerns and event-driven orchestration:
  - Quests Service: objective lifecycle, type routing, micro-objective triggers
  - Verification Service: automated checks for LocalProject (MVP rules → Post-MVP build/test)
  - Code Battle Service: authoritative test-case execution for CodingChallenge
  - Knowledge Graph Service: skill graph storage and query API for suggestions/unlocks
  - Reward Orchestrator: consume `ObjectiveCompleted` and emit `XPGranted`, `SkillPointsGranted`, `ChallengeUnlocked`
  - Event Bus: shared topics for `ObjectiveCompleted`, `VerificationResult`, `CodeSubmissionResult`, reward events

## Component Changes
- Quests Service
  - Add `Objective.type`, `verification_policy`, `completion_policy`
  - Implement micro-objective generation trigger on foundational completion (FR9/FR20)
  - Emit `ObjectiveCompleted` and listen for `VerificationResult` / `CodeSubmissionResult`
- Verification Service
  - MVP rules: existence check, structure scan, keyword/pattern checks with scoring
  - Post-MVP: clone → build → unit tests → smoke test
  - Provide `POST /verification/submit` and async result callback
- Code Battle Service
  - Accept code submissions; run predefined test cases; return pass/fail per case and aggregate
  - Webhook/callback to Quests Service for authoritative completion status
- Knowledge Graph Service
  - Graph storage of skills and relationships (`is_alternative_to`, `complements`, `prerequisite_of`, `applies_to`)
  - Query API for next micro-objective selection with latency target (<5s suggestion surfacing)
- Reward Orchestrator
  - Subscribe to `ObjectiveCompleted`; emit `XPGranted`, `SkillPointsGranted`, `ChallengeUnlocked` with ordering
  - Persist correlation IDs and audit trail
- Frontend
  - Workspace Router: launch Mission Control for `LocalProject`; Code Arena for `CodingChallenge`
  - Mission Control: submission & verification panel wired to Verification Service
  - Code Arena: problem panel, editor, tests; integrate Code Battle submission & results

## Data Model Changes
- `Objective`:
  - `type`: enum { Reading, Study, CodingChallenge, LocalProject }
  - `verification_policy`: policy ref or inline config
  - `completion_policy`: conditional logic per type
  - `tags`: include `Foundational`, `Practical` for unlock rules
- Events:
  - `ObjectiveCompleted { objectiveId, questId, userId, type, timestamp }`
  - `VerificationResult { objectiveId, pass, score, issues[] }`
  - `CodeSubmissionResult { objectiveId, passRate, failures[] }`
  - `XPGranted { userId, amount, objectiveId, correlationId }`
  - `SkillPointsGranted { userId, amount, skillId?, objectiveId, correlationId }`
  - `ChallengeUnlocked { userId, challengeId, objectiveId, correlationId }`

## API Contracts (Proposed)
- Quests Service
  - `POST /objectives/{id}/submit` → routes to Verification or Code Battle based on `type`
  - `GET /quests/{id}` → includes objectives with type, policies, and status
- Verification Service
  - `POST /verification/submit` { repoUrl | fileUploadId, ruleset }
  - `POST /verification/callback` (to Quests Service) { objectiveId, score, pass, feedback[] }
- Code Battle Service
  - `POST /code/submit` { objectiveId, problemId, language, source }
  - `POST /code/callback` (to Quests Service) { objectiveId, passRate, failures[] }
- Knowledge Graph Service
  - `GET /graph/suggest-next` { userId, objectiveId, context } → returns micro-objective candidates
- Reward Orchestrator
  - `POST /rewards/grant` (internal) → event emissions with ordering guarantees

## Sequence Flows
- CodingChallenge Completion
  - User submits in Code Arena → Code Battle executes tests → callback → Quests marks completion → Reward events → UI updates
- LocalProject Verification
  - Mission Control submission → Verification Service evaluates → callback with score → Quests auto-completes if pass → Reward events → feedback persists
- Micro-Objective Generation
  - Foundational completion → Quests queries Knowledge Graph → appends micro-objective under current quest → notify user within 5s
- Reward Cascade Ordering
  - ObjectiveCompleted → emit XP → emit Skill Points → emit Unlocks; UI shows staggered notifications

## Non-Functional Requirements Impacts
- Performance: suggestion latency <5s; Code Battle submit <5s authoritative target; local run <2s
- Reliability: retries on callbacks; idempotent event handling with correlation IDs
- Security: repo access scoped tokens; code execution sandboxing; input validation
- Observability: structured logs, traces across services; event audit trail

## Deployment & Rollout Plan
- Phase 1 (MVP): introduce `Objective.type`, workspace routing, Verification MVP rules, Code Battle integration for CodingChallenge
- Phase 2: Knowledge Graph Service online; micro-objective generation; Reward Orchestrator events
- Phase 3: Verification Post-MVP (clone/build/tests); expanded unlock logic
- Migration: backfill `type` for existing objectives; map legacy completion to policies

## Risks & Mitigations
- Verification false positives/negatives → tunable scoring, manual override with audit, progressive rulesets
- Callback flakiness → signed callbacks, retry with exponential backoff, dead-letter queues
- UX complexity → clear `Launch Workspace` actions, consistent progress context, help overlays

## Open Questions
- Knowledge Graph initial seed source and curation cadence (lecturer vs. automated import)
- Standardized language/toolchain support for Code Arena (which runtimes first)
- Verification rulesets per course—centralized or per-objective override?

## Decision Log
- Adopt type-driven architecture for clearer UX routing and backend policies
- Event-driven reward cascade for extensibility and auditability
- Separate Verification Service to decouple evaluation from quest lifecycle

## References
- PRD FR54–FR60; FR9/FR20 adjustments
- Front-end Spec: Quest Wireframes (Mission Control, Code Arena)
- Service Specs: Code Battle Service; Verification pipeline (to be authored)