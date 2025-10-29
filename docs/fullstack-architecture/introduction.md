# **Introduction**

This document outlines the complete fullstack architecture for RogueLearn, including backend systems, frontend implementation, and their integration. It serves as the single source of truth for AI-driven development, ensuring consistency across the entire technology stack.

This unified approach combines what would traditionally be separate backend and frontend architecture documents, streamlining the development process for modern fullstack applications where these concerns are increasingly intertwined.

### **Starter Template or Existing Project**

The project will be built **from scratch** following a **multi-repo strategy** with a consolidated core backend and specialized, isolated microservices. No overarching starter template will be used, allowing for a custom structure tailored to the project's specific needs.

*   **Frontend**: A standalone Next.js 14+ application using Tailwind CSS and Shadcn/UI.
*   **Game Client**: A standalone Unity 2022.3 LTS project for interactive "Boss Fights".
*   **Backend**: The backend is composed of:
    *   A consolidated **`RogueLearn.UserService`** (.NET 9) handling all core business logic (User, Quests, Social, Meetings).
    *   Isolated microservices for specialized tasks: an **Event Service** (Go), a **Meeting Service** (Go), and a **Scraping Service** (Python).

This hybrid approach provides the simplicity of a consolidated core for tightly-coupled domains, while retaining the flexibility and performance benefits of microservices for specialized tasks.

### **Change Log**

| Date          | Version | Description                                                                    | Author             |
| :------------ | :------ | :----------------------------------------------------------------------------- | :----------------- |
| Sep 24, 2025  | 1.8     | Added ephemeral CurriculumPack flow: new /game/sessions/ephemeral API, CurriculumPackMeta ephemeral flag and BossFightQuestionPack type, Unity IPackProvider abstraction with remote/fallback providers; added Host/Join Boss Fight workflow diagrams in core-workflows and services-ecosystem. | Winston, Architect |
| Sep 13, 2025  | 1.7     | Replaced Clerk with Supabase Authentication across the entire architecture.      | Winston, Architect |
| Sep 12, 2025  | 1.6     | Integrated Unity WebGL feature for "Boss Fights" across the architecture.        | Winston, Architect |
| Sep 12, 2025  | 1.5     | Removed Marketplace feature per user request to focus on core learning experience. | Winston, Architect |
| Sep 12, 2025  | 1.4     | Aligned architecture with expanded PRD. Added Marketplace, Duels, and Real-Time features. | Winston, Architect |
| Sep 11, 2025  | 1.3     | Corrected Introduction to include Go as a backend technology.                  | Winston, Architect |
| Sep 11, 2025  | 1.2     | Replaced TanStack Router with native Next.js App Router per user feedback.     | Winston, Architect |
| Sep 11, 2025  | 1.1     | Noted TanStack Router as the selected routing library.                         | Winston, Architect |
| Sep 11, 2025  | 1.0     | Initial document creation and multi-repo decision.                             | Winston, Architect |