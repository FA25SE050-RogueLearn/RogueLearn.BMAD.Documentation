# 5. Other Requirements

> Provide other system requirements (messages list), glossary of abbreviations, references, and open questions.

## 5.1 Messages List
- Success messages: Upload completed, Verification successful, Quest created.
- Error messages: Upload failed (retry), Invalid file type, Assessment submission error.
- Warning messages: Large file size, Unverified status limits features.

## 5.2 Glossary of Abbreviations
- FR: Functional Requirement
- NFR: Non-Functional Requirement
- UI: User Interface
- API: Application Programming Interface
- PRD: Product Requirements Document
- ERD: Entity Relationship Diagram

## 5.3 References
- FPTU Academic Portal/API
- roadmap.sh APIs & content
- GitHub OAuth/API
- Unity WebGL documentation
- Internal Architecture docs (service databases, interfaces)

## 5.4 Open Questions
- Should we integrate additional LMS providers beyond GitHub and roadmap.sh?
- What restrictions do we impose for non-verified students when joining guilds/parties?
- What is the data retention policy for academic documents and quest submissions?

---

## 5.5 Glossary (authoritative from docs/prd/glossary.md)

- Arsenal: The note-taking workspace where students create, organize, and link notes to skills and quests.
- Character Creation: The onboarding flow where a student selects curriculum, specialization, and initial roadmap.
- XP (Experience Points): Numeric progression awarded for completing quests, boss fights, and learning milestones.
- Skill Tree: Graph of interrelated skills with prerequisites and levels that represent the student's growth.
- Quest: A structured learning activity with objectives, steps, and resources derived from syllabus or roadmap gaps.
- Boss Fight: A mock exam or capstone assessment delivered via WebGL, used to benchmark mastery and grant XP.
- Party: A small study group that collaborates on quests and shares resources; managed by a Party Leader.
- Guild: A larger community that aggregates several parties; managed by a Guild Master.
- Party Leader: Role that manages party membership, events, and collaborative activities.
- Guild Master: Role that manages guild membership, invitations, and guild achievements.
- Game Master (Admin): Platform administrator with ownership of Elective Library and University Curriculum administration.
- Verified Lecturer: Educator status granted after verification, enabling enhanced privileges for events/content moderation.
- Learning Path: Sequence of quests curated to guide learners through a topic with clear progression checkpoints.
- Assessment: Formal evaluation component tied to quests (e.g., quizzes, assignments, boss fights) with submissions.
- Roadmap.sh: External reference for specialization tracks used to inform curriculum alignment and skill taxonomy.
- FPTU: FPT University; source of academic data, verification, and curriculum structures.
- Vector Index: Embedding-powered index that supports semantic search and content recommendation across notes/resources.
- Telemetry: Analytics and event tracking for performance, engagement, and reliability metrics.
- Unity WebGL: Technology used to render interactive boss fights and game-like experiences in the browser.