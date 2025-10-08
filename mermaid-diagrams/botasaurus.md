sequenceDiagram
    participant Admin as Game Master (Admin)
    participant WebUI as RogueLearn Frontend
    participant Gateway as API Gateway
    participant QuestService as Roguelearn.Quest Service (.NET)
    participant ScraperService as Scraping Service (Python/Botasaurus)
    participant Database as Quest DB (Postgres)

    Admin->>WebUI: Provides Curriculum URL
    WebUI->>Gateway: POST /api/admin/curriculum-ingestion-jobs
    Gateway->>QuestService: Forwards request

    activate QuestService
    QuestService->>Database: CREATE IngestionJob (Status: Pending)
    Database-->>QuestService: Returns JobID
    QuestService-->>Gateway: 202 Accepted (with JobID)
    Gateway-->>WebUI: 202 Accepted (with JobID)
    WebUI-->>Admin: "Job started. You will be notified."
    
    Note right of QuestService: Asynchronously triggers scraping
    QuestService->>ScraperService: POST /scrape (URL, JobID)
    deactivate QuestService

    activate ScraperService
    ScraperService->>ScraperService: Use Botasaurus to scrape data
    ScraperService-->>QuestService: POST /api/internal/ingestion-callback (JobID, RawData)
    deactivate ScraperService
    
    activate QuestService
    QuestService->>Database: UPDATE IngestionJob (Status: Processing)
    QuestService->>QuestService: Parse RawData into Domain Models
    QuestService->>Database: CREATE QuestLine, Quests, etc.
    Database-->>QuestService: Confirm creation
    QuestService->>Database: UPDATE IngestionJob (Status: Completed)
    deactivate QuestService

    Note over QuestService, Admin: Notify user of completion (e.g., via SignalR, email)