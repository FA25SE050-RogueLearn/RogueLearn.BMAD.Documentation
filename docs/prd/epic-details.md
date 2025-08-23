# **Epic Details**

This document provides detailed user stories for each epic, aligned with the phased rollout plan.

---

## **Phase 1: Core Student MVP**
*Focus: Establish the fundamental single-player experience.*

### **Epic: User Onboarding & Profile Management**
*Goal: Create a seamless and engaging entry point for new users, allowing them to set up their accounts and profiles.*

#### **Story: Foundational Project Setup**
**As a** Developer, **I want** to set up the initial project structure for both the Next.js frontend and the .NET backend, including basic CI/CD pipelines, **so that** we have a stable and automated foundation for development.

*   **Acceptance Criteria:**
    *   A new Git repository is created for both frontend and backend.
    *   Basic "Hello World" applications are running for both projects.
    *   A CI pipeline runs on every commit to the main branch, building and running basic tests.
    *   A health check endpoint is available on the backend (e.g., `/health`).

#### **Story: User Sign-Up and Login**
**As a** new user, **I want** to be able to sign up for an account and log in using my email and password via Clerk, **so that** my access is secure.

*   **Acceptance Criteria:**
    *   A user can navigate to a sign-up page.
    *   A user can create an account with a unique email and a password.
    *   A user can log in with their credentials.
    *   Upon successful login, the user is redirected to the main dashboard.
    *   Clerk is successfully integrated for handling authentication.

#### **Story: Protected Routes and Initial Dashboard**
**As a** logged-in user, **I want** to be directed to a personal dashboard that is only accessible after logging in, **so that** my private information is protected.

*   **Acceptance Criteria:**
    *   The main dashboard route (e.g., `/dashboard`) is protected and requires authentication.
    *   Unauthenticated users attempting to access the dashboard are redirected to the login page.
    *   The dashboard displays a welcome message and a prompt to start the onboarding process if the user's profile is incomplete.

#### **Story: Multi-Step Onboarding - Syllabus Upload**
**As a** new user on my first login, **I want** to be guided through a multi-step onboarding process, starting with uploading my course syllabus, **so that** the system can begin creating my personalized learning path.

*   **Acceptance Criteria:**
    *   A modal or dedicated page guides the user through onboarding steps.
    *   The first step prompts the user to upload a syllabus file (PDF, DOCX).
    *   The system provides feedback on whether the upload was successful or if there was an error.
    - The uploaded file is securely stored in Supabase storage.

#### **Story: Onboarding - Profile and Goal Selection**
**As a** new user, **I want** to complete my profile by selecting my 'Class' (career goal) and 'Route' (academic path) during onboarding, **so that** the system can tailor my quest line.

*   **Acceptance Criteria:**
    *   The onboarding flow includes a step for selecting a 'Class' from a predefined list.
    *   The user can also select their 'Route'.
    *   These selections are saved to the user's profile in the database.

#### **Story: Initial Character Sheet Generation**
**As a** new user, **I want** to see my personalized "Character Sheet" populated with my name, class, and route after completing the initial onboarding, **so that** I can see the immediate result of my setup.

*   **Acceptance Criteria:**
    *   Upon completing the final onboarding step, the user is navigated to their dashboard.
    *   The Character Sheet component on the dashboard displays the user's name, selected Class, and Route.
    *   A placeholder or loading state is shown for the skill tree and quest line, which will be populated by the AI service.

---

### **Epic: Core AI & Quest Generation**
*Goal: Leverage AI to transform academic documents into a personalized and dynamic learning path.*

#### **Story: AI Syllabus Processing & Quest Generation**
**As a** student, **I want** the system to process my syllabus with an AI service, **so that** a meaningful learning path is created.

---

### **Epic: Skill Tree & Knowledge Management**
*Goal: Provide tools for students to organize their knowledge and visualize their academic progress.*

#### **Story: The 'Arsenal' - Note Management**
**As a** student, **I want** to create, import, and organize my study notes, **so that** I have a centralized 'arsenal' of knowledge.

---

### **Epic: Gamification & Assessment**
*Goal: Make learning engaging and measurable through gamified challenges and progress tracking.*

#### **Story: 'Boss Fight' Challenge Generation**
**As a** student, **I want** the system to generate a 'Boss Fight' (quiz) for a specific topic, **so that** I can test my knowledge.

#### **Story: 'Boss Fight' Gameplay & Results**
**As a** student, **I want** to complete the challenge and see my results, **so that** I can see my character progress.

---

## **Phase 2: Social & Extension MVP**
*Focus: Introduce multiplayer and external integration features.*

### **Epic: Social & Collaboration (Parties)**
*Goal: Transform learning from an isolated activity into a collaborative, team-based effort.*

#### **Story: Party Creation & Management**
**As a** student, **I want** to create a 'Party' and become its 'Party Leader', **so that** I can organize a study group.

#### **Story: Party Invitations & Membership**
**As a** Party Leader, **I want** to invite other students to my party, **so that** we can study together.

#### **Story: Shared 'Party Stash'**
**As a** party member, **I want** access to a shared space for notes and resources, **so that** we can easily collaborate.

---

### **Epic: Browser Extension**
*Goal: Integrate the learning experience directly into the student's existing web-based academic workflow.*

#### **Story: Extension for Document Extraction**
**As a** student, **I want** a browser extension that can extract academic info from my university portal, **so that** I can easily import it into RogueLearn.

#### **Story: Contextual Note Access**
**As a** student, **I want** the extension to show me my relevant notes when I highlight text on a webpage, **so that** I can quickly access my knowledge.

---

## **Phase 3: Educator & Admin Toolkit**
*Focus: Empower educators and administrators with tools to manage and enhance the learning experience.*

### **Epic: Guild Master Toolkit**
*Goal: Provide educators with tools to create, manage, and enhance courses as interactive "Guilds".*

#### **Story: Guild Creation & Course Material Upload**
**As a** Guild Master, **I want** to create a Guild and upload course materials, **so that** I can establish an interactive learning environment for my students.

#### **Story: Guild Dashboard & Analytics**
**As a** Guild Master, **I want** to view aggregated progress data for all students in my Guild, **so that** I can identify areas where students are struggling.

---

### **Epic: Guide (Tutor) Toolkit**
*Goal: Enable tutors to provide personalized guidance and support to individual students.*

#### **Story: Guide Assignment & Student Progress Access**
**As a** Guide, **I want** to be assigned to specific students and access their progress, **so that** I can provide targeted assistance.

#### **Story: Custom Training Creation**
**As a** Guide, **I want** to create custom training drills for my assigned students, **so that** I can help them improve in specific areas.

---

### **Epic: Game Master (Admin) Toolkit**
*Goal: Provide system administrators with tools to manage and monitor the platform.*

#### **Story: System Monitoring & Analytics**
**As a** Game Master, **I want** to access a comprehensive analytics dashboard, **so that** I can monitor system health and user engagement.

#### **Story: Global Event Management**
**As a** Game Master, **I want** to create and trigger global events, **so that** I can enhance user engagement across the platform.

---

## **Phase 4: Living Ecosystem & Social**
*Focus: Enhance the platform with advanced AI, social, and competitive features.*

### **Epic: Advanced AI & Proactive Assistance**
*Goal: Leverage AI to provide proactive assistance and enhance the learning experience.*

#### **Story: AI-Powered Study Aid Suggestions**
**As a** student, **I want** the AI to scan my notes and suggest study aids, **so that** I can improve my learning efficiency.

#### **Story: Class Recording Processing**
**As a** student, **I want** to upload recordings of my classes and have the AI generate summaries, **so that** I can review key points efficiently.

---

### **Epic: Advanced Social & Competition**
*Goal: Enhance social interaction and competition to increase engagement.*

#### **Story: Guild vs Guild Competitions**
**As a** Guild Master, **I want** to initiate competitions between Guilds, **so that** I can foster healthy competition and engagement.

#### **Story: Achievement System**
**As a** student, **I want** to earn badges and achievements for my accomplishments, **so that** I can showcase my progress and skills.

---

## **Phase 5: Marketplace & Economy**
*Focus: Introduce a user-driven economy for sharing and monetizing knowledge.*

### **Epic: Marketplace**
*Goal: Create a platform for students to share and monetize their knowledge.*

#### **Story: Marketplace Creation & Content Upload**
**As a** student, **I want** to upload and share my study materials in the Marketplace, **so that** I can help others and potentially earn rewards.

#### **Story: Content Rating & Review**
**As a** student, **I want** to rate and review content in the Marketplace, **so that** I can help others find quality materials.

---

### **Epic: AI-Powered Curation**
*Goal: Use AI to curate and enhance user-generated content.*

#### **Story: AI Content Review & Elevation**
**As a** platform administrator, **I want** the AI to review and elevate high-quality user content, **so that** the best materials are easily accessible to all users.

#### **Story: Knowledge Pack Creation**
**As a** student, **I want** the AI to curate themed bundles of study materials, **so that** I can access comprehensive resources for specific topics or exams.