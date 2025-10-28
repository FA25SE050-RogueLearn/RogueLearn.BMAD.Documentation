RogueLearn Review 2 — Solution Outline (FA25_CP_grp4)

System Objectives
- Help Software Engineering students stay engaged with learning through game mechanics.
- Personalize learning based on academic performance (e.g., GPA).
- Improve outcomes for failed or at-risk courses.
- Build learning communities.
- Host competitive learning events (e.g., code battles) to increase motivation.

Flow 1: Personalized Learning and Game Suggestions
Summary
- Ingest academic data from FAP and course materials from FLM. Provide a dashboard with insights and tailored game suggestions to improve weak subjects.

Actors
- Student (e.g., Student A)
- Admin
- System (RogueLearn)

Preconditions
- Student has a RogueLearn account and valid FAP credentials.
- Admin has uploaded FLM syllabus/course details or configured imports.

Step-by-Step Flow
1. Student logs into RogueLearn and connects to their FAP account.
2. Student clicks Crawl; the system extracts academic data from FAP and optionally from FLM.
3. System renders a dashboard with academic insights and personalized suggestions.
4. Suggestion example: “Play game ABC to improve course XYZ (PRJ), previously failed.”
5. Student enters the Game section and plays scenes related to course XYZ.
6. Upon completion, game results are posted back to RogueLearn.
7. Student invites Friend B to a multiplayer Boss Fight to reinforce knowledge for the FE exam; results for both are updated.

Steps Table
| Step | Actor   | Action                                             | System Response                                        | Data Inputs/Outputs                             | Outcome/Notes                                           |
|------|---------|-----------------------------------------------------|--------------------------------------------------------|-------------------------------------------------|---------------------------------------------------------|
| 1    | Student | Login to RogueLearn; connect FAP                   | Authenticates and establishes FAP session              | Input: FAP credentials                           | Ready to crawl academic data                            |
| 2    | Student | Click “Crawl”                                      | Extracts FAP data; imports FLM syllabus if configured | Output: Academic records, course list           | Data captured for analytics and suggestions             |
| 3    | System  | Generate dashboard                                 | Displays GPA trends, failed/at-risk courses, insights | Output: Dashboard widgets                        | Personalized learning overview                          |
| 4    | System  | Produce suggestions                                | Maps courses to game content and recommends actions   | Output: Game/course recommendation               | Example: “Play ABC to improve PRJ”                      |
| 5    | Student | Play course-related game scenes                    | Tracks progress and learning interactions             | Input: Gameplay events; Output: progress logs    | Practice aligned to weak topics                          |
| 6    | System  | Post gameplay results to RogueLearn                | Updates student profile and progress dashboard        | Output: Updated progress and badges              | Feedback loop for personalized learning                 |
| 7    | Student | Initiate multiplayer Boss Fight with Friend B      | Spins up multiplayer session; tracks both players     | Input: Party members; Output: match stats        | Reinforces FE exam topics; updates both students         |

Key Data Fields
| Source | Field                         | Description                                               |
|--------|-------------------------------|-----------------------------------------------------------|
| FAP    | Student ID, GPA, course grades| Academic records for personalization                      |
| FLM    | Syllabus, course objectives   | Course content used to generate game mappings             |
| Game   | Scenes, skills, outcomes      | Game JSON mapped to syllabus topics (AI-generated optional)|

Errors and Edge Cases
- Invalid FAP credentials: prompt re-login; do not store raw passwords.
- Crawl throttling or FAP rate limits: queue jobs and notify student.
- Missing FLM syllabus: show limited suggestions; prompt Admin to upload.
- Game result sync failure: retry with backoff; persist locally then reconcile.

Metrics and Dashboard Widgets
- GPA trendlines; failed/at-risk course counts.
- Suggested actions taken vs. outcomes (delta in grades/skills).
- Gameplay time on-topic; scene completion rates; boss fight outcomes.

Flow 2: Join a Guild (e.g., FCode — Java Team)
Summary
- Students discover and join guilds that align with their learning goals and programming language focus.

Actors
- Student (Student A)
- Guild Master / Officers
- System (RogueLearn)

Preconditions
- Student profile is complete.
- Guild exists (e.g., FCode — Java Team) with defined entry rules.

Step-by-Step Flow
1. Student searches for guilds and opens the FCode — Java Team page.
2. Student requests to join; provides short motivation or answers screening questions.
3. Guild Master reviews and approves/declines the request.
4. On approval, System enrolls the student, assigns roles/badges, and shares onboarding materials.

Steps Table
| Step | Actor        | Action                                        | System Response                               | Data Inputs/Outputs                         | Outcome/Notes                                |
|------|--------------|-----------------------------------------------|-----------------------------------------------|---------------------------------------------|----------------------------------------------|
| 1    | Student      | Search and view guild                         | Shows guild page and entry criteria           | Output: Guild info                           | Student evaluates fit                         |
| 2    | Student      | Submit join request                           | Records application for review                | Input: Motivation, questionnaire             | Pending approval                              |
| 3    | Guild Master | Review request                                | Approve/decline; optional comments            | Output: Decision + notifications             | Controls guild membership                      |
| 4    | System       | Enroll approved student                        | Grants roles/badges; provides onboarding      | Output: Membership, resources links          | Student becomes guild member                  |

Guild Entry Rules (Examples)
- Prerequisites: minimum GPA, prior course completion (e.g., OOP, Java basics).
- Capacity: max members; waiting list logic.
- Code of conduct and activity expectations.

Flow 3: Code Battle Event
Summary
- Guild Master proposes an online competitive event (with participation fee). Admin approves. The system schedules and runs the event, automates judging, and publishes leaderboards.

Actors
- Guild Master (User X)
- Admin
- Participants (Students)
- System (RogueLearn)

Event Configuration Fields
| Field                         | Description                                                    | Example                    |
|-------------------------------|----------------------------------------------------------------|----------------------------|
| Title & Description           | Name and overview of the event                                 | FCode Java Code Battle     |
| Date & Time                   | Start/end; timezone                                            | 2025-11-10 19:00–21:00 ICT |
| Rules                         | Competition rules, allowed languages, disqualification criteria| Single-file Java solutions |
| Rooms & Capacity              | Number of rooms; participants per room                         | 5 rooms × 10 per room      |
| Eligible Guilds               | Guilds allowed to participate                                  | FCode, DevClub             |
| Min/Max Participants          | Enrollment bounds                                              | Min 20, Max 50             |
| Scoring & Difficulty          | Points per problem, difficulty distribution                    | Easy/Medium/Hard           |
| Participation Fee             | Fee per participant                                            | 200,000 VND                |
| Prize                         | Award structure                                                | Top 3 prize ranking        |

Approval and Scheduling Flow
| Step | Actor        | Action                                        | System Response                                 | Outcome/Notes                          |
|------|--------------|-----------------------------------------------|-------------------------------------------------|----------------------------------------|
| 1    | Guild Master | Submit event proposal                          | Records proposal; notifies Admin                | Pending review                          |
| 2    | Admin        | Review and approve                             | Approves; creates event schedule and info pages | Event published                         |
| 3    | System       | Open registration                              | Accepts sign-ups; tracks fees and eligibility   | Participants enrolled                   |

Competition Process Steps
| Step | Actor        | Action                                         | System Response                                          | Data Inputs/Outputs                      | Outcome/Notes                                  |
|------|--------------|------------------------------------------------|----------------------------------------------------------|------------------------------------------|-----------------------------------------------|
| 1    | System       | Issue room codes; assign participants          | Generates codes; assigns by capacity and guild rules     | Output: Room codes, rosters               | Balances rooms                                 |
| 2    | Participant  | Check-in to room                               | Marks attendance; unlocks problem set                   | Input: Check-in token                     | Ready to compete                               |
| 3    | Participant  | Solve problems; submit code snippet            | Accepts submissions; timestamps; queues for judging      | Input: Code files/snippets                | Attempts recorded                              |
| 4    | System       | Auto-judge with test cases                     | Runs test suite; computes scores and feedback            | Output: Pass/fail, score, feedback        | Objective evaluation                           |
| 5    | System       | Notify results to participant                   | Sends per-submission results; aggregates final score     | Output: Notifications                     | Transparent results                            |
| 6    | System/Admin | Publish leaderboard; award prizes              | Finalizes rankings; processes prize distribution         | Output: Leaderboard, awards               | Event closure                                  |

Event Dashboards and Metrics
- Players count per room; submission attempts; pass rate.
- Score distribution by problem difficulty; average time per problem.
- Code quality metrics (e.g., complexity proxies, style checks if enabled).
- Financials: participation fees collected; prize payouts; credits.

Flow 4: Learning Community (Study Party)
Summary
- After playing games, students receive suggestions to join or form study groups to reinforce learning with peers.

Actors
- Student A (Party Creator)
- Student B (Invitee)
- Party Leader (could be Student A or assigned leader)
- System (RogueLearn)

Step-by-Step Flow
1. Student A creates a study party (e.g., “PRJ — HJF — SBA Party”).
2. Student A invites Student B to join.
3. Members share learning materials in the party space.
4. Party Leader schedules weekly meetings and generates meeting links.
5. System sends notifications with meeting details to all members.
6. Members join the meeting.
7. System generates meeting summaries.
8. Party dashboard shows engagement and outcomes.

Steps Table
| Step | Actor        | Action                                       | System Response                                    | Data Inputs/Outputs                       | Outcome/Notes                                   |
|------|--------------|----------------------------------------------|----------------------------------------------------|-------------------------------------------|-------------------------------------------------|
| 1    | Student A    | Create study party                           | Creates party space; sets visibility and rules     | Input: Party name, description            | Party created                                    |
| 2    | Student A    | Invite Student B                             | Sends invite; tracks acceptance                    | Output: Invitation notification           | Member onboarding                                |
| 3    | Members      | Share learning materials                      | Stores files/links; versions and tags              | Input: Materials; Output: library         | Collaborative resources                           |
| 4    | Party Leader | Schedule weekly meetings                      | Generates meeting link; adds to calendar           | Output: Calendar event, meeting link      | Regular cadence                                  |
| 5    | System       | Notify members                                | Sends notifications (email/app)                    | Output: Notifications                     | Attendance awareness                             |
| 6    | Members      | Join meeting                                  | Tracks attendance; enables live notes              | Input: Join link; Output: attendance logs | Interactive session                               |
| 7    | System       | Produce meeting summary                       | Summarizes notes/action items                      | Output: Summary document                  | Knowledge capture                                |
| 8    | System       | Render party dashboard                        | Shows engagement metrics and outcomes              | Output: Dashboard                         | Visibility into group effectiveness               |

Party Dashboard Widgets
- Attendance rate; meeting frequency; shared materials count.
- Topic coverage; action items completed; study time per member.

Flow 5: Admin / Game Master Operations
Summary
- Admins manage data integrations (FAP, FLM), gameplay settings, and mappings between games and courses. Game JSON may be AI-generated from syllabus content.

Actors
- Admin / Game Master
- System (RogueLearn)

Operations Table (CRUD)
| Operation            | Action                                                     | System Response                                     | Data Inputs/Outputs                                 | Notes                                                 |
|----------------------|------------------------------------------------------------|-----------------------------------------------------|------------------------------------------------------|-------------------------------------------------------|
| FAP Integration      | Configure credentials and crawl schedules                  | Secure storage; scheduled jobs                      | Input: API creds; Output: academic records           | Respect rate limits; privacy compliance              |
| FLM Content Import   | Upload syllabus/course details                             | Validates; stores; versions content                 | Input: Syllabus docs; Output: course content         | Supports CSV/JSON/Markdown                           |
| Game Settings        | Configure gameplay parameters and difficulty               | Applies config; logs changes                        | Input: Settings JSON; Output: active config          | Per-course or global settings                        |
| Game–Course Mapping  | Map game scenes to syllabus topics                         | Updates mapping; drives suggestions                  | Input: Mapping JSON; Output: recommendation rules    | AI-assisted generation from syllabus                 |
| Content CRUD         | Create/Read/Update/Delete any of the above resources       | Reflects changes in dashboards and suggestions      | Output: Audit logs                                   | Role-based access control                            |

Security and Compliance
- Do not store raw FAP passwords; use tokens/secure secret management.
- Role-based access control for Admin/Guild Master operations.
- Event fee handling complies with institutional policies.

Glossary
- FAP: Academic portal providing student records (e.g., GPA, course grades).
- FLM: Course syllabi and learning materials repository.
- FE exam: Foundational/Final Examination depending on context; here used as a study focus.

Backend Entity Mapping per Flow
This section maps each flow to the concrete database tables defined in:
- docs/fullstack-architecture/service-databases/merged-backend-database.md
- docs/fullstack-architecture/service-databases/event-service-database.md

Flow 1: Personalized Learning and Game Suggestions — Associated Backend Entities
| Table | File | Purpose | Key Fields Referenced | Operations in Flow |
|-------|------|---------|-----------------------|--------------------|
| user_profiles | merged-backend-database.md | Core user identity and preferences | auth_user_id, preferences, class_id, route_id | Read/update profile and preferences |
| curriculum_programs | merged-backend-database.md | Degree programs | program_code, degree_level | Read for context |
| curriculum_versions | merged-backend-database.md | Versioned curricula | program_id, version_code, is_active | Read/validate current version |
| curriculum_structure | merged-backend-database.md | Subjects per curriculum/semester | curriculum_version_id, subject_id, semester | Read for course mapping |
| subjects | merged-backend-database.md | University courses | subject_code, subject_name | Read/search courses |
| syllabus_versions | merged-backend-database.md | Versioned syllabus content | subject_id, version_number, content | Read syllabus to map game/quests |
| quests | merged-backend-database.md | Quest catalog | quest_type, difficulty_level, subject_id | Read/assign to student |
| quest_steps | merged-backend-database.md | Step-level quest content | quest_id, step_type, sequence | Read during gameplay |
| user_quest_progress | merged-backend-database.md | Track quest progress | auth_user_id, quest_id, status | Create/Update on completion |
| user_skills | merged-backend-database.md | Track skill levels | auth_user_id, skill_id, level | Update on learning |
| user_skill_rewards | merged-backend-database.md | XP rewards ledger | source_type, skill_id, points_awarded | Create reward entries |
| skills | merged-backend-database.md | Skills catalog | name, tier, domain | Read to align suggestions |
| notifications | merged-backend-database.md | System notifications | type, title, message | Create suggestion notifications |
| quest_analytics | merged-backend-database.md | Quest KPIs | quest_id, metrics JSONB | Read/update analytics (optional) |

Flow 2: Join a Guild — Associated Backend Entities
| Table | File | Purpose | Key Fields Referenced | Operations in Flow |
|-------|------|---------|-----------------------|--------------------|
| guilds | merged-backend-database.md | Guild registry | name, guild_type, capacity | Read/search guilds |
| guild_members | merged-backend-database.md | Memberships | guild_id, auth_user_id, role | Create membership on approval |
| guild_invitations | merged-backend-database.md | Invites/applications | guild_id, invitee_user_id, status | Create/update invite/application |
| notifications | merged-backend-database.md | Notifications | type='GuildInvite' | Create invite status notifications |
| social_messages | merged-backend-database.md | Guild chat | context=guild, sender_id, content | Optional onboarding messages |
| message_reactions | merged-backend-database.md | Reactions to messages | message_id, emoji, reactor_id | Optional engagement |

Flow 3: Code Battle Event — Associated Backend Entities
Event Service (primary)
| Table | File | Purpose | Key Fields Referenced | Operations in Flow |
|-------|------|---------|-----------------------|--------------------|
| event_requests | event-service-database.md | Guild Master requests | requester_guild_id, status | Create/Update request; Admin review |
| events | event-service-database.md | Approved events | type, started_date, number_of_rooms | Create on approval; Read during event |
| rooms | event-service-database.md | Event rooms | event_id, name | Create rooms per event |
| room_players | event-service-database.md | Players in rooms | room_id, user_id, score | Create/update attendance and scores |
| event_guild_participants | event-service-database.md | Guild participation | event_id, guild_id, room_id | Create on registration |
| languages | event-service-database.md | Supported languages | name, compile_cmd, run_cmd | Read for execution environment |
| code_problems | event-service-database.md | Problem catalog | title, difficulty | Read/select problems |
| code_problem_tags | event-service-database.md | Problem tagging | code_problem_id, tag_id | Read filter by topic |
| code_problem_language_details | event-service-database.md | Per-language stubs/constraints | language_id, solution_stub | Read to present starter code |
| test_cases | event-service-database.md | Problem test cases | code_problem_id, input, expected_output | Read/run auto-judge |
| event_code_problems | event-service-database.md | Problems assigned to event | event_id, code_problem_id, score | Create mapping for scoring |
| submissions | event-service-database.md | Code submissions | user_id, code_problem_id, status | Create/update per attempt |
| leaderboard_entries | event-service-database.md | Player rankings | event_id, user_id, rank, score | Create at closure |
| guild_leaderboard_entries | event-service-database.md | Guild rankings | event_id, guild_id, rank | Create at closure |

Cross-Service Links (soft FKs)
| Table | File | Purpose |
|-------|------|---------|
| user_profiles | merged-backend-database.md | Player identity referenced by room_players, submissions, leaderboard_entries |
| guilds | merged-backend-database.md | Guild identity referenced by event_guild_participants and guild_leaderboard_entries |

Flow 4: Learning Community (Study Party) — Associated Backend Entities
| Table | File | Purpose | Key Fields Referenced | Operations in Flow |
|-------|------|---------|-----------------------|--------------------|
| party_members | merged-backend-database.md | Party membership | party_id, auth_user_id, role | Create membership |
| party_invitations | merged-backend-database.md | Invites to party | party_id, invitee_user_id, status | Create/update invitations |
| party_activities | merged-backend-database.md | Logged activities | party_id, activity_type, metadata | Create session/activity logs |
| party_stash_items | merged-backend-database.md | Shared materials | party_id, item_type, url/content | Create/update shared resources |
| meetings | merged-backend-database.md | Scheduled meetings | party_id, start_time, meeting_link | Create weekly meetings |
| meeting_participants | merged-backend-database.md | Attendance tracking | meeting_id, auth_user_id, state | Create/update attendance |
| meeting_summaries | merged-backend-database.md | Meeting summaries | meeting_id, content | Create auto-generated summary |
| social_messages | merged-backend-database.md | Party chat/messages | context=party, content | Create/update messages |
| message_reactions | merged-backend-database.md | Reactions to messages | message_id, emoji, reactor_id | Create reactions |
| notifications | merged-backend-database.md | Meeting and invite notifications | type='PartyInvite'/'Reminder' | Create notifications |

Flow 5: Admin / Game Master Operations — Associated Backend Entities
Learning & Curriculum
| Table | File | Purpose | Operations |
|-------|------|---------|-----------|
| subjects | merged-backend-database.md | Course catalog | CRUD |
| curriculum_programs | merged-backend-database.md | Degree programs | CRUD |
| curriculum_versions | merged-backend-database.md | Versioned curricula | CRUD |
| curriculum_structure | merged-backend-database.md | Subject-to-curriculum mapping | CRUD |
| syllabus_versions | merged-backend-database.md | Syllabus content versions | CRUD |
| curriculum_version_activations | merged-backend-database.md | Activation governance | Create/Read |

Skills & Quests
| Table | File | Purpose | Operations |
|-------|------|---------|-----------|
| skills | merged-backend-database.md | Skill catalog | CRUD |
| skill_dependencies | merged-backend-database.md | Prerequisite/relationships | CRUD |
| quests | merged-backend-database.md | Quest definitions | CRUD |
| quest_steps | merged-backend-database.md | Step content | CRUD |
| quest_resources | merged-backend-database.md | Linked resources | CRUD |
| learning_path_quests | merged-backend-database.md | Path-to-quest mapping | CRUD |

Events & Code Problems
| Table | File | Purpose | Operations |
|-------|------|---------|-----------|
| languages | event-service-database.md | Language catalog/execution config | CRUD |
| code_problems | event-service-database.md | Problem catalog | CRUD |
| code_problem_tags | event-service-database.md | Tagging | CRUD |
| code_problem_language_details | event-service-database.md | Starter code/constraints | CRUD |
| test_cases | event-service-database.md | Validation cases | CRUD |

Notes
- Game-to-course mapping leverages subjects + syllabus_versions combined with quests.subject_id and learning_path_quests. A dedicated mapping table can be introduced later if needed for more granular alignment.
- FAP integration configuration is handled outside the schema here; imported academic records are utilized via curriculum_structure, subjects, and student_semester_subjects.



