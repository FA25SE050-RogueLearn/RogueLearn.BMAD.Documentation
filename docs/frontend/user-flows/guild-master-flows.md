# Guild Master (Lecturer) Flows

## Guild Management

**User Goal:** Create and manage a guild for a course, upload materials, and monitor student progress

**Entry Points:** From Dashboard, Guild Management section

**Success Criteria:** Lecturer can successfully create a guild, add course materials, and view anonymized student progress data

### Flow Diagram

```mermaid
graph TD
    A[Start] --> B[Create/Manage Guild]
    B --> C[Upload Course Materials]
    B --> D{Monitor Guild Members}
    D --> E[View Anonymized Progress Dashboard]
    E --> F[Identify Struggling Topics]
    B --> G{Create Custom Content}
    G -- AI-Assisted --> H[Draft Side Quests]
    H --> I[Review, Edit & Assign Quests]
    G --> J[Design Custom Skill Tree Overlays]
    G --> K[Create Custom 'Boss Fights']
    B --> L{Engage Community}
    L --> M[Initiate Guild-vs-Guild Battle]
    L --> N[Assign Guides to Students]
```

### Edge Cases & Error Handling:
- Large course material uploads
- Privacy concerns with student data
- AI-generated content requires significant editing
- Schedule conflicts for guild events

**Notes:** The Guild Management interface should balance powerful tools with ease of use, allowing lecturers to focus on teaching rather than technical complexity.