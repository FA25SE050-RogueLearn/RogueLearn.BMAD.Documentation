# **High Level Architecture**

### **Technical Summary**

RogueLearn will be implemented as a cloud-native application featuring a decoupled frontend and a targeted microservices backend. The architecture is defined by:
- A **Next.js frontend application** for all user-facing experiences.
- A **consolidated .NET 9 `RogueLearn.UserService`** that manages the majority of business logic, including the User, Quests, Social, and Meeting domains.
- A specialized, isolated **Go Event Service** for secure code execution and competitive programming events.
- An internal **Python Scraping Service** for data ingestion.
- **Supabase** providing the PostgreSQL database, storage, and authentication.

This approach centralizes core business logic for simplicity and transactional integrity while isolating performance-intensive or technologically distinct components.

### **Platform and Infrastructure Choice**

*   **Platform:** A hybrid-cloud approach leveraging best-in-class services.
    *   **Frontend Hosting:** **Vercel**. It is purpose-built for Next.js, providing seamless deployments, global CDN, and serverless functions out-of-the-box.
    *   **Backend Hosting:** **Azure Container Apps**. This is a serverless container platform that is ideal for running our consolidated .NET service and the specialized Go/Python services.
    *   **Database, Storage & Auth:** **Supabase**. Provides a managed PostgreSQL instance, user authentication, real-time capabilities, and file storage.

### **Repository Structure**

*A Multi-Repo Strategy will be maintained for clear separation of concerns:*
*   **`RogueLearn.Frontend`**: The Next.js frontend application.
*   **`RogueLearn.BMAD.Documentation`**: The source of truth documentation.
*   **`roguelearn-unity-games`**: The Unity project for "Boss Fights".
*   **`RogueLearn.UserService`**: The consolidated .NET service containing User, Quest, Social, and Meeting domains.
*   **`RogueLearn.EventService`**: The isolated Go microservice for code battles.
*   **`RogueLearn.Scraper`**: The isolated Python microservice for scraping.
*   **`RogueLearn.Protos`**: Centralized repository for any gRPC/Protobuf definitions.
*   **`RogueLearn.Kubernetes`**: GitOps repository for deployment configurations.
*   **`RogueLearn.CleanArchitecture`**: The .NET template for the consolidated service.

### **High Level Architecture Diagram**

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
        GoogleMeet[Google Meet API]
    end

    subgraph "Backend Layer (Azure)"
        APIGateway[API Gateway REST]
        RealtimeHub[Real-time Hub WebSockets]
        
        subgraph "Services"
            UserService[.NET UserService<br/>(User, Quests, Social, Meeting, AI Proxy)]
            EventService[Go Event Service<br/>(Code Battles)]
            ScrapingService[Python Scraping Service<br/>(Internal)]
        end
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
    APIGateway --> EventService
    
    RealtimeHub --> UserService

    UserService -- Sync Trigger & All Core Logic --> Database
    UserService --> FileStorage
    UserService --> ScrapingService
    UserService --> Gemini
    UserService --> GoogleMeet
    
    EventService --> Database
    ScraperService --> ExternalWeb

    %% Event Bus interactions for isolated services
    UserService -. "publish code.submission" .-> EventService
    EventService -. "publish code.evaluation.completed" .-> UserService
    UserService -. "publish reward.triggered" .-> WebApp
    UserService -. "publish skilltree.updated" .-> WebApp
```

### **Architectural and Design Patterns**

*   **Consolidated Core Service:** The main backend follows a monolithic approach for the User, Quest, Social, and Meeting domains. *Rationale:* This simplifies development, deployment, and data consistency for tightly coupled business logic.
*   **Specialized Microservices:** Performance-critical or technologically distinct functions (Event, Scraping) are isolated into separate microservices. *Rationale:* Allows using the best tool for the job (Go, Python) and independent scaling.
*   **API Gateway:** A single entry point for all client requests. *Rationale:* Simplifies the client and centralizes cross-cutting concerns.
*   **Clean Architecture (.NET):** The consolidated `RogueLearn.UserService` will be structured internally using Clean Architecture to maintain a clear separation of concerns between its different domains.
*   **Repository Pattern (.NET):** Data access within the Core Service will be abstracted.
*   **Database Triggers:** A PostgreSQL trigger will be used to sync new users from Supabase's `auth.users` table to our application's `UserProfiles` table. *Rationale:* Provides a reliable, event-driven way to create user profiles without webhooks.