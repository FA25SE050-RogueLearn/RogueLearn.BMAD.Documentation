# Tutor (The Guide) Flows

## Student Support

**User Goal:** Provide targeted assistance to assigned students

**Entry Points:** From Dashboard, Guide Assignment section

**Success Criteria:** Tutor can view student progress, provide feedback, and assign practice materials

### Flow Diagram

```mermaid
graph TD
    A[Start] --> B{Assigned to Student by Guild Master}
    B -- Yes --> C[View Scoped Student Dashboard]
    C --> D[Review Quest Progress & Skill Tree]
    C --> E[Read Student's 'Arsenal' Notes]
    C --> F[View Burnout Forecast]
    E --> G{Provide Support}
    G --> H[Create & Assign Training Drills]
    G --> I[Send Mentor Reflections]
    G --> J[Leave Inline Feedback on Notes]
```

### Edge Cases & Error Handling:
- Student revokes access to notes
- Conflicting advice between tutor and lecturer
- Tutor assignment changes mid-course
- Communication barriers with student

**Notes:** The Tutor interface should emphasize the mentorship relationship, with tools that facilitate personalized guidance rather than generic feedback.