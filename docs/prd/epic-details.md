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