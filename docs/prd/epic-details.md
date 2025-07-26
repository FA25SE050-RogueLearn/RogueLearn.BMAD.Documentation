# **Epic Details**

## **Epic 1: Foundation & Character Creation**

*Goal:* Establish the core technical foundation for both the frontend and backend applications and implement the complete new user onboarding and character creation flow.

---

### **Story 1.1: Project Initialization & Health Checks**

**As a** Developer,

**I want** to initialize the Next.js frontend and .NET backend projects in their separate repositories and have a basic health check endpoint,

**so that** we have a stable foundation and can verify that the two services can potentially communicate.

### **Acceptance Criteria**

1. The Next.js frontend project is created and runs successfully in a local development environment.
2. The .NET backend Web API project is created and runs successfully in a local development environment.
3. The backend exposes a public /api/health endpoint that returns a 200 OK status with a simple JSON response (e.g., {"status": "ok"}).
4. The frontend is configured to communicate with the backend API, successfully handling CORS policies.
5. A basic UI component on a test page in the frontend successfully calls the backend /api/health endpoint and displays the "ok" status.
6. Each repository contains a README.md file with basic setup and run instructions.

---

### **Story 1.2: User Authentication & Account Creation**

**As a** new user,

**I want** to be able to sign up for a new account and log in to the application using a secure authentication provider,

**so that** I can gain access to the platform and my personal data is protected.

### **Acceptance Criteria**

1. The Next.js frontend is integrated with the Clerk React provider.
2. The main landing page displays "Sign In" and "Sign Up" buttons for unauthenticated users.
3. The Clerk-hosted sign-in and sign-up flows are correctly configured and functional.
4. Upon successful authentication, Clerk redirects the user back to the application.
5. On a user's first successful login, a new user record is created in the Supabase users table, containing their Clerk User ID and email.
6. The .NET backend is configured with a middleware to validate JWTs issued by Clerk for all protected API endpoints.

---

### **Story 1.3: Protected Dashboard & Onboarding Trigger**

**As a** newly signed-in user,

**I want** to be directed to my personal space and, if it's my first time, be guided through the setup process,

**so that** I can begin my personalized learning journey.

### **Acceptance Criteria**

1. A /dashboard page exists and is a protected route, accessible only to authenticated users.
2. An /onboarding page exists for the character creation flow.
3. After signing in, a user who has NOT completed onboarding is automatically redirected to the /onboarding page.
4. After signing in, a user who HAS completed onboarding is automatically redirected to the /dashboard page.
5. The main dashboard displays a personalized welcome message.

---

### **Story 1.4: Onboarding Flow - Syllabus Upload**

**As a** new user in the onboarding flow,

**I want** to upload my course syllabus document,

**so that** the system can analyze it to create my personalized learning path.

### **Acceptance Criteria**

1. The /onboarding page presents the first step of a setup wizard: "Upload Your Syllabus".
2. A file input control allows the user to select and upload a document (PDF, DOCX).
3. The selected file is successfully uploaded to a dedicated Supabase Storage bucket under a user-specific folder.
4. A new record is created in the syllabi table in the database, linking the uploaded file's path to the user's ID.
5. The UI displays a confirmation message upon successful upload.

---

### **Story 1.5: Character Sheet Creation & First View**

**As a** new user who has completed the initial setup,

**I want** to see my personalized "Character Sheet" for the first time,

**so that** I can see the result of my onboarding and feel ready to begin my adventure.

### **Acceptance Criteria**

1. After the final step of the onboarding wizard, the onboarding_complete flag for the user is set to true in the users table.
2. A call is made to a (mocked for now) backend endpoint, /api/users/me/process-syllabus, which simulates the AI processing and returns placeholder stats.
3. The user is redirected from /onboarding to their /dashboard.
4. The dashboard now displays the "Character Sheet" layout, populated with placeholder data (e.g., Level 1, 0 XP, subject strengths at 10%).

## **Epic 2: The Solo Adventure - Learning Path & Challenges**

*Goal:* Implement the core value proposition for an individual student. It focuses on the AI-driven analysis of the syllabus to generate a tangible learning path, the ability for students to manage their study materials, and the mechanism to engage in gamified challenges.

---

### **Story 2.1: AI Syllabus Processing & Quest Generation**

**As a** student,

**I want** the system to process my uploaded syllabus with a real AI service,

**so that** a meaningful and accurate learning path is created for my course.

### **Acceptance Criteria**

1. The backend /api/users/me/process-syllabus endpoint is implemented to call a real AI service (e.g., OpenAI, Gemini).
2. The AI service is prompted to extract key learning topics, chapters, and a likely chronological sequence from the syllabus text.
3. The backend successfully receives the structured data from the AI service.
4. For each extracted topic, a new record is created in a quests table, linked to the user and the syllabus.
5. The quests are ordered sequentially based on the AI's analysis.
6. The Character Sheet on the dashboard now displays the real, AI-generated quest line instead of placeholder data.

---

### **Story 2.2: The 'Arsenal' - Note Management**

**As a** student,

**I want** to create, import, and organize my study notes for each course,

**so that** I have a centralized 'arsenal' of knowledge to use for my challenges.

### **Acceptance Criteria**

1. On the Course Dashboard, a new "Arsenal" section is available.
2. Users can create a new note using a rich-text editor.
3. Users can upload existing note documents (PDF, DOCX, TXT), which are stored in Supabase Storage.
4. Each note (created or uploaded) is stored in a notes table and associated with the user and a specific course.
5. The Arsenal UI displays all notes for the selected course.
6. Users can view, edit, and delete notes they have created.

---

### **Story 2.3: 'Boss Fight' Challenge Generation**

**As a** student,

**I want** the system to generate a 'Boss Fight' (quiz) for a specific chapter or 'quest',

**so that** I can test my knowledge in an engaging way.

### **Acceptance Criteria**

1. Each quest in the Quest Log has a "Challenge" button.
2. When clicked, the backend is called to generate a quiz for that specific topic.
3. The backend prompts an AI service, providing it with the relevant chapter/topic name and the user's notes from their 'Arsenal' for that course.
4. The AI returns a set of multiple-choice questions based *only* on the provided notes.
5. The backend stores the generated quiz questions.
6. The frontend receives the questions and displays the "Challenge Interface" to the student.

---

### **Story 2.4: 'Boss Fight' Gameplay & Results**

**As a** student,

**I want** to complete the challenge and see my results,

**so that** I know how well I've mastered the material and can see my character progress.

### **Acceptance Criteria**

1. The student can select answers for each question in the Challenge Interface.
2. Upon submitting the quiz, the answers are sent to the backend for evaluation.
3. The backend calculates the score and determines the 'XP' gained.
4. The student's stats (XP, level, topic mastery) are updated in the users table.
5. The frontend displays a results screen showing the score, correct/incorrect answers, and XP gained.
6. The Character Sheet on the main dashboard updates to reflect the new stats.

## **Epic 3: The Party System - Collaborative Study**

*Goal:* This epic builds on the solo experience by introducing the social layer. It allows students to form study groups ("Parties"), share resources, and create a sense of community, transforming learning from an isolated activity into a team effort.

---

### **Story 3.1: Party Creation & Management**

**As a** student,

**I want** to create a 'Party' and become its 'Party Leader',

**so that** I can organize a study group for a specific course.

### **Acceptance Criteria**

1. A "Parties" or "Guilds" section is available in the main dashboard.
2. A user can initiate a "Create Party" flow from this section.
3. The user must give the Party a name and can optionally associate it with one of their active courses.
4. Upon creation, a new record is saved in a parties table.
5. The creator is automatically assigned the 'Party Leader' role for that party.
6. The Party Leader has access to a simple Party management dashboard where they can see members and edit the party name.

## **Epic 4: The Lecturer's 'Guild Master' Toolkit**

*Goal:* To empower educators (Guild Masters) with tools to manage their courses, monitor student progress, and create custom learning content within the QuestLearn ecosystem, fostering a more guided and interactive educational experience.

---

### **Story 4.1: Guild Creation and Course Material Management**

**As a** Guild Master (Lecturer),

**I want** to create a "Guild" for my course and upload my own course materials,

**so that** my students can access them to supplement their learning paths.

### **Acceptance Criteria**

1.  A new user role, "Guild Master," is available in the system.
2.  Authenticated Guild Masters have access to a "Guild Management" dashboard.
3.  From the dashboard, a Guild Master can create a new Guild, providing a name and description.
4.  The Guild Master can upload course materials (e.g., PDFs, DOCX, PowerPoints) to a dedicated storage area for the Guild.
5.  Students (Players) can join a Guild, and once joined, they can view and access the materials uploaded by the Guild Master.

---

### **Story 4.2: Aggregated Student Progress Dashboard**

**As a** Guild Master,

**I want** to view a dashboard with aggregated, anonymized progress data for all students in my Guild,

**so that** I can quickly identify topics or challenges where the class is struggling.

### **Acceptance Criteria**

1.  The Guild Management dashboard features a "Progress Overview" tab.
2.  This tab displays anonymized data, such as the average completion rate for each quest (topic) in the course.
3.  A visual indicator (e.g., color-coding) highlights quests or 'boss fights' where the average success rate is below a configurable threshold (e.g., 60%).
4.  The data is presented in a clear, graphical format (e.g., bar charts).
5.  No personally identifiable student data is visible on this aggregated view.

---

### **Story 4.3: AI-Assisted Side Quest Generation**

**As a** Guild Master,

**I want** to use an AI assistant to generate draft 'side quests' based on my uploaded reference materials,

**so that** I can efficiently create supplementary content for my students.

### **Acceptance Criteria**

1.  In the Guild Management dashboard, there is a "Content Creation" section.
2.  The Guild Master can select one or more uploaded documents and a specific topic.
3.  An "AI Generate Side Quest" button triggers a backend process.
4.  The backend prompts an AI service with the content of the selected documents and the topic, requesting it to create a draft quest (e.g., a set of questions, a small project description).
5.  The generated draft quest is displayed to the Guild Master in an editable interface.
6.  The Guild Master can review, edit, and then save and assign the quest to the entire Guild.

---

### **Story 4.4: Custom Skill Tree Overlays and Boss Fights**

**As a** Guild Master,

**I want** to design and publish custom skill tree overlays and create custom "Boss Fights",

**so that** I can tailor the learning experience to my specific curriculum and assessment needs.

### **Acceptance Criteria**

1.  A "Skill Tree Designer" tool is available for Guild Masters.
2.  The tool allows the Guild Master to define a custom overlay, highlighting or re-arranging skills relevant to their course.
3.  Players in the Guild can toggle this overlay on their personal skill tree.
4.  A "Boss Fight Creator" allows the Guild Master to define a custom challenge, specifying required knowledge nodes, a description, rewards, and a deadline.
5.  The created Boss Fight can be assigned to the entire Guild and appears in the students' quest logs.

## **Epic 5: The Tutor & Admin Support Systems**

*Goal:* To provide specialized tools for support roles, enabling Tutors (Guides) to offer personalized student assistance and System Admins (Game Masters) to manage the platform's health, events, and community standards.

---

### **Story 5.1: Guide Assignment and Scoped Student Access**

**As a** Guide (Tutor),

**I want** to be assigned to a specific student for a specific course and have read-only access to their progress and notes for that course,

**so that** I can provide targeted, context-aware support without compromising student privacy.

### **Acceptance Criteria**

1.  A "Guide" role can be assigned to users by a Guild Master or Admin.
2.  A Guild Master can assign a Guide to a specific student within their Guild.
3.  When assigned, the Guide gains read-only access to the student's dashboard, but only for the specific course they are assigned to.
4.  This access includes viewing the student's quest line progress, skill tree, and their 'Arsenal' of notes for that course.
5.  Access is strictly limited; the Guide cannot see data from the student's other courses or edit any of the student's content.

---

### **Story 5.2: Guide-Led Training Drills and Mentorship**

**As a** Guide,

**I want** to create and assign custom, non-graded 'training drills' and send motivational "Mentor Reflections",

**so that** I can offer personalized practice and encouragement to my assigned student.

### **Acceptance Criteria**

1.  A Guide has a simple interface to create a 'training drill' (e.g., a short set of questions or a small task description).
2.  The Guide can assign this drill to their student. The drill appears as a special, non-graded side quest for the student.
3.  A "Mentor Reflection" feature allows the Guide to send a message (text, or link to a video/audio clip) to the student.
4.  The reflection can be optionally tied to a specific quest or skill node, appearing as a note or comment for the student in their view.

---

### **Story 5.3: Game Master Backend Interface and Global Events**

**As a** Game Master (System Admin),

**I want** access to a non-academic back-end interface to monitor application health and trigger global events,

**so that** I can manage the platform's stability and create engaging, platform-wide experiences.

### **Acceptance Criteria**

1.  A "Game Master" role provides access to a separate admin dashboard.
2.  The dashboard displays key application health metrics (e.g., API response times, error rates).
3.  The Game Master can trigger a "Global Event," such as a "Bonus XP Day."
4.  When a global event is active, all users see a notification, and the relevant game mechanics (e.g., XP calculation) are adjusted accordingly.
5.  The Game Master can schedule events to run for a specific duration.

---

### **Story 5.4: Game Master Moderation and Community Management**

**As a** Game Master,

**I want** tools to manage community-reported abuse and moderate disputes,

**so that** I can maintain a safe and fair environment for all users.

### **Acceptance Criteria**

1.  The admin dashboard includes a moderation queue for user-reported content or behavior.
2.  The Game Master can review reports, view the context (e.g., flagged note, chat message), and take action.
3.  Available actions include muting a user, banning a user, and deleting inappropriate content.
4.  A log of all moderation actions is maintained for accountability.

## **Epic 6: The Proactive AI Companion**

*Goal:* To evolve the AI from a reactive processor into a proactive assistant that actively helps students study, manages their knowledge, and identifies potential learning risks before they become major issues.

---

### **Story 6.1: Proactive Study Aid Suggestions**

**As a** Player,

**I want** the system's AI to proactively scan my 'Arsenal' of notes and suggest the creation of 'spells' (study aids like flashcards, summaries, or mind maps),

**so that** I can discover new ways to engage with my own study materials.

### **Acceptance Criteria**

1.  An asynchronous backend process periodically scans a player's notes in their Arsenal.
2.  The AI is prompted to identify concepts within the notes that would be suitable for creating study aids (e.g., a list of key terms for flashcards, a dense paragraph for a summary).
3.  If a suitable opportunity is found, the user receives a notification (e.g., "I've noticed you have detailed notes on 'Binary Trees'. Shall I craft a set of flashcards for you?").
4.  The notification provides a one-click action for the user to accept the suggestion.
5.  If accepted, the AI generates the suggested study aid and adds it to the user's Arsenal.

---

### **Story 6.2: Automated Lecture Summarization**

**As a** Player,

**I want** to be able to upload or link to audio/video recordings of my online classes,

**so that** the system can automatically generate text summaries and transcripts for me.

### **Acceptance Criteria**

1.  The 'Arsenal' interface allows users to upload audio/video files or provide a URL to a recording.
2.  The backend uses a speech-to-text service to transcribe the content of the recording.
3.  The full transcript is saved and linked to the original media file.
4.  The transcript is then passed to a generative AI service with a prompt to create a concise summary of the key topics discussed.
5.  Both the summary and the full transcript are added to the user's Arsenal as new notes, linked for easy reference.

---

### **Story 6.3: At-Risk Player Identification**

**As an** AI System,

**I want** to run a predictive analytics module to identify players who are at risk of falling behind,

**so that** the system can alert Guild Masters or Guides to intervene.

### **Acceptance Criteria**

1.  A scheduled backend job analyzes player data for risk indicators (e.g., low login frequency, high 'Boss Fight' failure rate, low note-taking activity).
2.  A risk score is calculated for each student in a Guild.
3.  If a student's risk score crosses a predefined threshold, a notification is sent to their assigned Guide or the Guild Master.
4.  The notification provides a high-level summary of the risk factors (e.g., "Player X has not logged in for 7 days and has failed their last 3 challenges.") without revealing sensitive personal data.
5.  The system provides a link to the Guide/Guild Master's view of the student's dashboard for more context.

## **Epic 7: The Community & Competition Platform**

*Goal:* To build a vibrant community by enabling competitive and collaborative events, including Guild-level competitions and skill-based PvP challenges, while also recognizing individual player achievements.

---

### **Story 7.1: Guild-vs-Guild Competition Mode**

**As a** Guild Master,

**I want** to be able to initiate a "Guild-vs-Guild" competition,

**so that** my students can engage in a friendly, large-scale academic rivalry.

### **Acceptance Criteria**

1.  The Guild Management dashboard has an option to challenge another Guild to a competition.
2.  The challenge can be based on various metrics, such as "Highest average score on a specific Boss Fight" or "Fastest collective completion of a new quest line."
3.  The challenged Guild Master must accept the challenge for the event to begin.
4.  A shared leaderboard, visible to members of both Guilds, tracks the competition's progress in real-time.
5.  At the end of the event, the winning Guild is announced, and a record of the victory is displayed on the Guild's profile.

---

### **Story 7.2: Player vs. Player (PvP) Skill Challenges**

**As a** Player,

**I want** to participate in PvP events based on coding and design challenges related to my quest line,

**so that** I can test my practical skills against my peers in a competitive format.

### **Acceptance Criteria**

1.  The system can host scheduled PvP events (e.g., "Saturday Algorithm Battle").
2.  Events are themed around specific skills (e.g., CSS, algorithms, system design).
3.  Players who have the relevant skills on their skill tree are invited to participate.
4.  The event consists of a timed challenge hosted in an integrated coding environment.
5.  A real-time leaderboard displays rankings during the event.
6.  Winners are awarded badges and their achievements are noted on their public profile.

---

### **Story 7.3: Personal Achievements and Badges**

**As a** Player,

**I want** to earn personal achievements and badges for reaching milestones and participating in events,

**so that** I have a visible record of my accomplishments and can customize my profile.

### **Acceptance Criteria**

1.  A new "Achievements" tab is available on the player's profile.
2.  The system defines a set of achievements, such as "First Boss Fight Victory," "Party Leader," "10-Quest Streak," and "PvP Participant."
3.  When a player meets the criteria for an achievement, they receive a notification and the corresponding badge is unlocked.
4.  Unlocked badges are displayed in the Achievements tab.
5.  Players can select a few favorite badges to display on their main profile page.

## **Epic 8: The Knowledge Marketplace**

*Goal:* To create a self-sustaining ecosystem where players can share, rate, and trade high-quality study materials, fostering a collaborative economy and enabling the AI to curate and elevate the best user-generated content for the entire community.

---

### **Story 8.1: Marketplace for Study Materials**

**As a** Player,

**I want** a "Marketplace" where I can upload my high-quality study materials and other players can view and acquire them,

**so that** I can share my knowledge and benefit from the work of others.

### **Acceptance Criteria**

1.  A "Marketplace" section is accessible from the main dashboard.
2.  Players can upload their notes or other study aids to the Marketplace.
3.  When uploading, the player must provide a title, description, and tags for the material.
4.  Other players can browse, search, and filter the listings in the Marketplace.
5.  Each item in the Marketplace has a details page with a rating and review system.

---

### **Story 8.2: In-Game Currency and Transactions**

**As a** Player,

**I want** to use an in-game currency, earned through my activities, to acquire materials from the Marketplace,

**so that** my effort and engagement in the game have tangible value.

### **Acceptance Criteria**

1.  The system includes an in-game currency (e.g., "Knowledge Gems").
2.  Players earn this currency by completing quests, winning boss fights, and participating in events.
3.  Sellers in the Marketplace can set a price in this currency for their materials.
4.  Players can purchase materials from other players, which transfers the currency from the buyer to the seller.
5.  A player's current currency balance is visible on their profile.

---

### **Story 8.3: AI Curation for the 'Eternal Codex'**

**As an** AI System,

**I want** to review, rate, and elevate the highest-quality, user-generated materials to a shared "Eternal Codex",

**so that** the best knowledge becomes a permanent, globally accessible resource.

### **Acceptance Criteria**

1.  The AI periodically analyzes top-rated and frequently downloaded materials from the Marketplace.
2.  The AI assesses the content for quality, accuracy, and clarity.
3.  If a submission meets a high standard, the AI flags it for elevation.
4.  The original author is notified and credited.
5.  The material is copied to the "Eternal Codex," a special, publicly searchable library of premium content, making it available to all users, potentially for free or as part of a premium offering.

---

### **Story 3.2: Inviting and Joining a Party**

**As a** Party Leader,

**I want** to invite my classmates to join my Party,

**so that** we can study together.

### **Acceptance Criteria**

1. The Party management dashboard provides a unique, shareable invitation link.
2. When another registered user clicks this link, they are shown a confirmation page with the Party name and an option to "Join".
3. Upon joining, the user is added to the party_members table, linking their user ID to the party ID.
4. The Party dashboard now displays all current members.
5. Users can see a list of all Parties they are a member of in their main "Parties" section.
6. Users have the ability to leave a Party.

---

### **Story 3.3: The 'Party Stash' - Shared Resources**

**As a** Party member,

**I want** a shared space where we can pool our resources and notes,

**so that** we can benefit from each other's work.

### **Acceptance Criteria**

1. Each Party has a dedicated **"Party Stash"** view.
2. Any member of the Party can post a new resource to the stash (e.g., a text note, a link to an external article, or upload a document).
3. Uploaded documents are stored in a shared folder for that party in Supabase Storage.
4. All posted resources are visible to all members of the Party.
5. Party members can comment on resources posted in the stash.
6. The Party Leader has moderation rights to remove inappropriate posts from the stash.

## **Epic 4: The Lecturer's 'Guild Master' Toolkit**

*Goal:* To empower Lecturers with tools to monitor class progress and efficiently create and assign supplementary learning materials.

---

### **Story 4.1: Role-Based Access for Lecturers**

**As a** System Administrator,

**I want** to be able to assign the "Lecturer" role to a specific user,

**so that** they can access the educator-specific features of the platform.

### **Acceptance Criteria**

1. A secure mechanism (e.g., an admin script or interface) exists to elevate a user's role to "Lecturer".
2. The application's RBAC system recognizes the "Lecturer" role.
3. When a Lecturer logs in, they are presented with a different main navigation bar that includes a link to their "Guild Master Dashboard".
4. A Lecturer can still view the application as a student would, but has access to the additional tools.

---

### **Story 4.2: The Guild Master Dashboard - Analytics**

**As a** Lecturer,

**I want** to view a dashboard with aggregated, anonymized analytics for my course,

**so that** I can understand my students' collective progress and challenges.

### **Acceptance Criteria**

1. A /dashboard/lecturer page exists and is accessible only to users with the "Lecturer" role.
2. The dashboard displays a list of the Lecturer's active courses ("Guilds").
3. Upon selecting a course, the dashboard shows high-level statistics (e.g., average 'quest' completion rate, number of active students).
4. The dashboard features a module that lists the top 3 'Boss Fights' or topics where students are scoring the lowest on average.
5. All data presented is aggregated and anonymized; no individual student performance is visible on this dashboard.

---

### **Story 4.3: AI-Assisted Quest Curation**

**As a** Lecturer,

**I want** to upload my own materials and have the AI generate a draft 'side quest' for me,

**so that** I can create supplementary content for my students quickly and efficiently.

### **Acceptance Criteria**

1. The Lecturer dashboard has a "Create Side Quest" feature.
2. The feature allows a Lecturer to upload a document (PDF, DOCX) or provide a URL.
3. The backend uses an AI service to process the provided material and generates a draft quiz (e.g., a set of multiple-choice and short-answer questions).
4. The Lecturer is presented with an interface showing the AI-generated questions and answers.
5. The Lecturer can edit, add, or delete any of the AI-generated questions.

---

### **Story 4.4: Quest Assignment and Student View**

**As a** Lecturer,

**I want** to assign the curated quest to my class,

**so that** my students can benefit from the extra practice.

### **Acceptance Criteria**

1. After finalizing the quest content, the Lecturer can "Assign" it to a specific course.
2. The assigned quest appears in the 'Side Quests' section for all students enrolled in that course.
3. Students can complete the quest, but it is not tied to their main progression (i.e., no XP is awarded).
4. The Lecturer can see completion statistics for the assigned side quest on their dashboard.