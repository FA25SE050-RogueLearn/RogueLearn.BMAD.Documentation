# Business Requirements Document (BRD): RogueLearn

## 1. Project Vision and Mission

### **Vision Statement**
To be the definitive platform that transforms higher education from a series of academic requirements into a clear, engaging, and direct journey toward career readiness for every student.

### **Mission Statement**
Our mission is to increase student employability by providing a gamified, AI-powered learning experience that intelligently maps university curricula to specific career goals. We empower students by making every academic task a meaningful step in their main quest to become a job-ready professional, equipped with the skills, experience, and portfolio to succeed.

---

## 2. Business Objectives

1.  **Enhance Student Employability:** The primary objective is to provide students with a clear, demonstrable path from their academic studies to career readiness. The platform must successfully bridge the gap between theoretical knowledge and the practical skills, portfolio projects, and interview preparation required by the job market.

2.  **Increase Student Engagement and Motivation:** To combat academic disengagement, the platform must transform the learning process into a compelling, gamified experience. Success is measured by sustained user activity, consistent progress through the "Main Quest," and positive qualitative feedback on motivation.

3.  **Create a Personalized and Adaptive Learning Pathway:** The system must intelligently process a student's official curriculum (e.g., FPT University's) and integrate it with industry-standard roadmaps (e.g., roadmap.sh) to generate a personalized and relevant "Main Quest Line." This pathway must adapt to a student's specialization choices and elective learning.

4.  **Validate the Gamified Career-Readiness Model:** As a capstone project, a key objective is to validate the core hypothesis: that a gamified, career-focused approach leads to better learning outcomes and higher engagement than traditional study methods. This will be measured through user feedback and engagement analytics.

---

## 3. Target Audience

### **Primary Target Audience (MVP Focus)**

The primary target audience for the initial release of RogueLearn is **FPT University students currently enrolled in the Software Engineering major**.

*   **Rationale:** This group is the ideal "beachhead market" for validation. Their curriculum is well-structured, providing a predictable and high-quality data source for our core AI quest generation feature. Their clear, career-oriented academic path makes them the perfect user base to test, validate, and refine our primary value proposition of enhancing employability.

### **Secondary Target Audience (Post-MVP Expansion)**

Following a successful MVP launch and validation with the primary group, the platform will be expanded to support:

1.  Students in other IT-related majors at FPT University (e.g., Information Security, Artificial Intelligence).
2.  Software engineering and computer science students at other universities.

---

## 4. Key Business Requirements

To achieve its objectives, the RogueLearn platform must fulfill the following core business requirements:

1.  **Provide a Career-Focused Onboarding Experience:** The platform must guide new users through a "Character Creation" process where they select a specific, curated career path (e.g., ".NET Developer") which defines their "Main Quest."

2.  **Generate a Unified, Goal-Oriented Learning Path:** The system must be able to ingest a student's academic documents (e.g., FPT curriculum) and intelligently merge them with industry-standard roadmaps (e.g., roadmap.sh) to produce a single, unified "Main Quest Line" that is explicitly tied to the user's selected career goal.

3.  **Support Academic Specializations as Core Quest Chapters:** The system must seamlessly integrate senior-year specialization choices (e.g., .NET, React) as advanced, mandatory chapters of the user's Main Quest.

4.  **Offer a Library of Career-Enhancing Electives:** To provide flexibility, the platform must offer a curated library of "Elective Skill Arcs" that allow students to learn valuable skills outside their core track.

5.  **Deliver Tangible Career Preparation Outcomes:** The system must include a dedicated "Career Preparations" epic. This module must contain:
    *   **Project Quests** that guide students in building portfolio-worthy projects on their public GitHub.
    *   **Algorithmic Challenges (Code Battles)** that build a quantifiable, LeetCode-style profile for technical interviews.
    *   Quests focused on resume refinement and professional networking.

6.  **Facilitate Collaborative Learning:** The platform must support social and collaborative learning through small-group "Parties" that enable students to study together and share resources.

7.  **Implement Core Gamification Mechanics:** The system must include foundational gamification elements—including Experience Points (XP), Levels, Skill Trees, and Achievements—to drive student motivation.

---

## 5. Success Metrics

The success of the RogueLearn platform will be measured against the following Key Performance Indicators (KPIs):

#### **1. Career Readiness Impact**
*   **Portfolio Completion Rate:** > 60% of active, final-semester users will complete at least one portfolio-worthy project through a "Project Quest."
*   **Career Readiness Confidence Score:** Achieve an average user-reported confidence score of > 4.0 out of 5 on the question, "How prepared do you feel for a technical interview in your chosen career path?"
*   **Code Battle Participation:** > 30% of active users participate in at least one Code Battle event per semester, establishing a quantifiable, LeetCode-style profile.

#### **2. Learning Engagement & Platform Adoption**
*   **Main Quest Progression Rate:** The average active user completes > 75% of the required quests in their "Main Quest Line" for a given semester.
*   **Weekly Active Users (WAU):** Achieve a WAU rate of > 50% of registered users within the target FPT University cohort.
*   **User Retention:** Achieve a month-over-month retention rate of > 30% for active users.

#### **3. Core Feature Performance**
*   **AI Quest Generation Quality:** Achieve an average user satisfaction score of > 4.0 out of 5 for the relevance and quality of AI-generated quests.
*   **Elective Arc Adoption:** > 25% of active users will voluntarily start at least one "Elective Skill Arc" per semester.
*   **Social Feature Engagement:** > 40% of active users will be a member of at least one "Party."