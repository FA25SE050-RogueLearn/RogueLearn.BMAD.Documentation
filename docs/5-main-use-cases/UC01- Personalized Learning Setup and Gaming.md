# UC-01: Personalized Learning Setup and Gaming

**Actor:** Player (Student)

---

## Transactions

1. **Student provides FAP academic transcript**
    * Student navigates to the `/onboarding/connect-fap` page.
    * Using a browser or an extension, the student copies the full HTML source of their FAP academic record page.
    * Student pastes the copied HTML content into the provided text area on the web application.

2. **Student initiates data extraction**
    * Student selects the correct curriculum version from the dropdown.
    * Student clicks the "Sync & Forge Skill Tree" (for new users) or "Sync & Update Progress" (for returning users) button.
    * The system begins processing the provided HTML and displays a progress indicator to the user.

3. **System analyzes data**
    * System calculates GPA, identifies failed subjects (PRJ301, SWE201)
    * System runs AI analysis for learning gaps
    * System generates recommendations

4. **Student views dashboard**
    * System displays academic overview with charts
    * Shows AI recommendation: "Play Agile Quest to improve PRJ301"
    * Student clicks "Start My Learning Journey"

5. **Student selects game**
    * Student navigates to "My Games"
    * Student clicks "Agile Quest" (mapped to PRJ301)
    * System shows game overview (0/10 chapters)

6. **Student starts chapter**
    * Student clicks "Start Chapter 1: Scrum Fundamentals"
    * System loads game scene with story
    * Student begins interactive challenges

7. **Student completes challenges**
    * Student answers questions about Scrum roles
    * System validates answers in real-time
    * Student solves scenario-based problems

8. **Student finishes chapter**
    * Student completes all challenges
    * System calculates score (87/100)
    * System awards XP (+150), skill points (+10), achievement

9. **System updates progress**
    * System saves game progress
    * Updates PRJ301 mastery: 15% → 28%
    * Unlocks Chapter 2

10. **System shows recommendations**
    * Displays next steps: Continue solo or invite friend for boss fight
    * Student views updated dashboard