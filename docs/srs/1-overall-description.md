# 1. Overall Description

## 1.1 Product Overview
> Gives the overall description about the product with some introduction and the context diagram. The context diagram presents the boundary and connections between the system you’re developing and everything else. This identifies external entities (terminators) outside the system that interface to it in some way, as well as data, control, and material flows between the terminators and the system.

RogueLearn is a gamified learning platform that aligns university curricula with career roadmaps. It converts academic progress into quests, skill trees, and boss fights, supporting students with AI-driven personalization and integrations (e.g., FPTU portal, roadmap.sh).

Context Diagram (Mermaid)
```mermaid
flowchart LR
    subgraph RogueLearn System
      UI[Web/App UI]
      Auth[Authentication]
      Quest[Quest Generator]
      Skill[Skill Tree Service]
      Notes[Arsenal/Notes]
      Exam[Boss Fight (WebGL)]
      DB[(Database)]
      UI --> Auth
      Auth --> Quest
      Auth --> Skill
      Auth --> Notes
      Quest --> DB
      Skill --> DB
      Notes --> DB
      Exam --> DB
    end

    Student[Student User] -->|Login/Use| UI
    Instructor[Instructor/Advisor] -->|Review/Feedback| UI
    Admin[Administrator] -->|Manage/Configure| UI

    FPTU[FPTU Portal/API] -->|Academic Data| Quest
    Roadmap[roadmap.sh API] -->|Career Data| Quest
    GitHub[GitHub OAuth/API] -->|Project Links| Notes
    Analytics[Telemetry/Monitoring] -->|Usage Metrics| DB
```

## 1.2 Business Rules
> Define key business rules governing data, processes, and constraints.

- Users must complete a three-step onboarding (Curriculum Route → Class/Specialization → Skill-based Roadmap). See PRD FR1.
- Academic documents (transcripts, schedules, IDs) can be uploaded to influence stats and unlock content; FPTU students may be verified. See PRD FR2 and FR48.
- AI generates quest lines per semester, aligned with syllabus and academic calendar; supplementary gap quests from roadmap.sh when applicable. See PRD FR4, FR4A, and FR9.
- Skill nodes map to courses and objectives; prerequisites must be met before unlocking dependent skills; Arsenal notes link to skill nodes. See PRD FR8, FR11, FR13, and FR15.
- Boss Fight scores convert to skill experience following predefined conversion rules. See PRD FR16–FR18.
- Leaderboards update in near real-time for events; periodic batch updates for quests and skills. See PRD FR19.
- Dynamic quest adjustments based on performance and schedule changes. See PRD FR20.
- Quest memory ensures cross-semester continuity; failed courses generate recovery quests. See PRD FR49 and FR51.
- Privacy: student academic data is processed under explicit consent and can be deleted upon request.