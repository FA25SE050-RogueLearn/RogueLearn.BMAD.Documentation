# **High Level Architecture**

### **Technical Summary**

RogueLearn will be implemented as a cloud-native, multi-repository application. It features a decoupled frontend built with Next.js, interactive "Boss Fights" built with **Unity WebGL**, and a microservices-based backend using **.NET 9** and **Go**. The system is deployed on Vercel and Azure Container Apps. Communication is handled by a RESTful API Gateway and a real-time SignalR hub for interactive features. The architecture supports AI-powered quest generation, social collaboration, competitive Duels, and code-grading battles. Data, storage, and authentication are consolidated and managed within a **Supabase** project.

### **Platform and Infrastructure Choice**

To best support our technology stack and scalability goals, I recommend the following platform configuration:

*   **Platform:** A hybrid-cloud approach leveraging best-in-class services.
    *   **Frontend Hosting:** **Vercel**. It is purpose-built for Next.js, providing seamless deployments, global CDN, and serverless functions out-of-the-box.
    *   **Backend Hosting:** **Azure Container Apps**. This is a serverless container platform that is ideal for running our .NET and Go microservices.
    *   **Database, Storage & Auth:** **Supabase**. Provides a managed PostgreSQL instance, user authentication, real-time capabilities, and file storage which will be used for both user documents and hosting Unity game assets.
*   **Key Services:**
    *   **Vercel:** Next.js Hosting, Edge Network (CDN)
    *   **Azure:** Container Apps, API Management (for the API Gateway)
    *   **Supabase:** PostgreSQL Database, Storage (for documents and game assets), **Authentication**
    *   **Internal AI Proxy Service:** A dedicated backend service to securely manage communication with the Gemini API.

### **Repository Structure**

As established, we will use a **Multi-Repo Strategy**. This provides the best separation of concerns and allows for independent development lifecycles. The initial repository structure will be:

*   **`roguelearn-web`**: The Next.js frontend application.
*   **`roguelearn-unity-games`**: The Unity project containing the "Boss Fight" game client.
*   **`roguelearn-user-service`**: .NET microservice for user profiles, preferences, and user-related operations (authentication handled by Supabase Auth).
*   **`roguelearn-quests-service`**: .NET microservice for syllabus, quests, skill trees, and game session logic.
*   **`roguelearn-social-service`**: .NET microservice for Parties, Guilds, Events, and real-time features like Duels.
*   **`roguelearn-meeting-service`**: Go microservice for party meetings, scheduling, and meeting-related features.
*   **`roguelearn-code-battle-service`**: **Go** microservice for compiling, running, and scoring user-submitted code with Qdrant for vector storage.
*   **`roguelearn-buildingblocks`**: Shared common libraries and utilities used across backend microservices.
*   **`roguelearn-shared-types`**: A private NPM package for shared TypeScript interfaces.

### **High Level Architecture Diagram**

This diagram illustrates the primary components and data flow of the RogueLearn platform, updated to integrate Verification into the User Service and move Rewards and Skill Tree (formerly Knowledge Graph) into the Quests Service, with an Event Bus for asynchronous orchestration.

```mermaid
graph TD
    subgraph "User Layer"
        User[👩‍🎓 Student]
        BrowserExtension[Chrome Extension]
    end

    subgraph "Presentation Layer (Vercel)"
        Frontend[Next.js Web App]
        UnityClient[Unity Game Client]
        Frontend --> UnityClient
    end

    subgraph "External Services"
        Gemini[Gemini API]
    end

    subgraph "Backend Layer (Azure)"
        APIGateway[API Gateway REST]
        RealtimeHub[Real-time Hub WebSockets]
        
        subgraph "Microservices"
            subgraph UserService[.NET User Service]
                UserCore[Profiles, Academic, Achievements]
                UserVerification[Verification Module]
                UserRewards[Rewards Module]
                UserSkillTree[Skill Tree Module]
            end
            subgraph QuestService[.NET Quests Service]
                QuestCore[Syllabus, Quests, Sessions]
            end
            SocialService[.NET Social Service]
            MeetingService[Go Meeting Service]
            CodeBattleService[Go Code Battle Service]
            AIProxyService[.NET AI Proxy Service]
        end
    end

    subgraph "Event Bus (Azure Service Bus)"
        TopicQuestCompleted[topic: quest.completed]
        TopicVerification[topic: verification.updated]
        TopicSkillTree[topic: skilltree.updated]
        TopicRewards[topic: reward.triggered]
    end

    subgraph "Data & Asset Layer (Supabase)"
        Database[PostgreSQL Database]
        FileStorage[File Storage for Docs]
        GameAssetHosting[Game Asset Hosting]
        SupabaseAuth[Supabase GoTrue Auth]
    end

    User --> Frontend

    BrowserExtension --> APIGateway

    Frontend -- Auth via SDK --> SupabaseAuth
    Frontend --> APIGateway
    Frontend -.-> RealtimeHub

    UnityClient --> GameAssetHosting
    UnityClient --> APIGateway
    
    APIGateway --> UserService
    APIGateway --> QuestService
    APIGateway --> SocialService
    APIGateway --> MeetingService
    APIGateway --> CodeBattleService
    APIGateway --> AIProxyService
    
    %% Internal modules are accessed via the parent services
    APIGateway --> UserVerification
    APIGateway --> UserRewards
    APIGateway --> UserSkillTree
    
    RealtimeHub --> SocialService
    RealtimeHub --> MeetingService
    RealtimeHub --> UserVerification
    RealtimeHub --> UserRewards

    UserService -- Sync Trigger --> Database

    QuestService --> Database
    QuestService --> FileStorage
    
    SocialService --> Database
    
    CodeBattleService --> Database
    
    UserVerification --> Database
    UserRewards --> Database
    UserSkillTree --> Database
    
    AIProxyService --> Gemini
    AIProxyService --> Database

    Database <--> FileStorage
    SupabaseAuth <--> Database

    %% Event Bus interactions
    QuestService -- publish --> TopicQuestCompleted
    UserVerification -- publish --> TopicVerification
    UserSkillTree -- subscribe --> TopicQuestCompleted
    UserSkillTree -- publish --> TopicSkillTree
    UserRewards -- subscribe --> TopicQuestCompleted
    UserRewards -- subscribe --> TopicVerification
    UserRewards -- publish --> TopicRewards
```

### **Architectural and Design Patterns**

*   **Microservices Architecture:** The backend will be composed of small, independent services. *Rationale:* This allows for independent development, deployment, and scaling.
*   **API Gateway:** A single entry point for synchronous requests. *Rationale:* Simplifies the client, centralizes cross-cutting concerns like auth and rate limiting.
*   **Event-Driven Orchestration:** Cross-service workflows coordinated via an Event Bus (Azure Service Bus). *Rationale:* Decouples services and enables scalable, resilient processing.
*   **Clean Architecture (.NET):** Each microservice will separate domain logic, application logic, and infrastructure. *Rationale:* Produces highly testable and maintainable services.
*   **Component-Based UI (Next.js):** The frontend will be built as a collection of reusable components. *Rationale:* Promotes reusability and faster development.
*   **Repository Pattern (.NET):** Data access within each microservice will be abstracted. *Rationale:* Decouples business logic from data access implementation.
*   **Database Triggers:** A PostgreSQL trigger will be used to sync new users from Supabase's `auth.users` table to our application's `UserProfiles` table. *Rationale:* Provides a reliable, event-driven way to create user profiles without webhooks.

