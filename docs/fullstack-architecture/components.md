# **Components**

This section details the major logical components of the platform, reflecting the consolidated backend architecture.

### **Frontend Application (`roguelearn-web`)**

*   **Responsibility:** Provides the entire user-facing experience, including embedding the Unity Game Client.
*   **Technology Stack:** Next.js, TypeScript, React, Tailwind CSS.

### **Unity Game Client (`roguelearn-unity-games`)**

*   **Responsibility:** Renders and manages the interactive "Boss Fight" experiences.
*   **Technology Stack:** Unity 2022.3 LTS, C#, WebGL.

### **Core Service (`RogueLearn.UserService`)**

*   **Responsibility:** A consolidated .NET service that manages the core business logic for the platform. This service is organized internally into the following logical domains:
    *   **User Domain:** Manages user profiles, preferences, roles, verification, achievements, and the personal "Arsenal" of notes. It is the authority for user identity.
    *   **Quests Domain:** Owns the core learning loop, including academic management (Syllabuses, Enrollments), Quests, Skill Trees, and Game Sessions.
    *   **Social Domain:** Manages all multi-user features like Parties and Guilds.
    *   **Meeting Domain:** Manages meeting scheduling, persistence, and integration with external providers like Google Meet.
    *   **AI Proxy:** Acts as a secure, internal gateway for all communications with the Gemini API.
*   **Technology Stack:** .NET 9, C#, SignalR.

### **Event Service (`RogueLearn.EventService`)**

*   **Responsibility:** An isolated microservice that manages competitive programming features including code compilation, execution, and scoring in secure sandboxed environments.
*   **Technology Stack:** Go, Docker (for sandboxing).

### **Scraping Service (`RogueLearn.Scraper`)**

*   **Responsibility:** A specialized, internal-only service for extracting raw HTML from external URLs. It contains no business logic.
*   **Technology Stack:** Python, FastAPI, Botasaurus.

### **Component Interaction Diagram**

This diagram shows how the consolidated components interact.

```mermaid
graph TD
    subgraph "Clients"
        WebApp["Next.js Web App"] -- embeds --> Unity["Unity Game Client"]
    end

    subgraph "Entry Layer (Azure)"
        Gateway["API Gateway (REST)"]
        Realtime["Real-time Hub (SignalR)"]
    end

    subgraph "Backend Services (Azure Container Apps)"
        UserService[.NET UserService<br/>(User, Quests, Social, Meeting, AI Proxy)]
        EventService["Event Service (Go)"]
        ScrapingService["Scraping Service (Python, Internal Only)"]
    end

    subgraph "External Dependencies"
        Gemini["Gemini API"]
        GoogleMeet["Google Meet API"]
        ExternalWeb["External University Websites"]
    end

    subgraph "Data & Asset Persistence (Supabase)"
        DB["PostgreSQL DB"]
        Store["File Storage"]
        GameAssets["Game Asset Hosting (CDN)"]
        SupabaseAuth["Supabase GoTrue Auth"]
    end

    WebApp -- "Auth via SDK" --> SupabaseAuth
    WebApp -- HTTP --> Gateway
    WebApp -- WebSocket --> Realtime
    Unity -- HTTP --> Gateway
    Unity -- "loads assets from" --> GameAssets
    
    Gateway --> UserService
    Gateway --> EventService
    
    Realtime --> UserService

    UserService -- Sync Trigger & All Core Logic --> DB
    UserService --> Store
    UserService --> ScrapingService
    UserService --> Gemini
    UserService --> GoogleMeet
    
    EventService --> DB
    ScraperService --> ExternalWeb

    %% Event-driven interactions for isolated services
    UserService -. "publish code.submission" .-> EventService
    EventService -. "publish code.evaluation.completed" .-> UserService
    UserService -. "publish reward.triggered" .-> WebApp
    UserService -. "publish skilltree.updated" .-> WebApp
    ```
