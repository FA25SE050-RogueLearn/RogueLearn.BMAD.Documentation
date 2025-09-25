# AI-Powered Quest Generation & Learning Path Creation Flow

```mermaid
sequenceDiagram
    participant U as Student User
    participant UI as Web Interface (Next.js)
    participant APIGateway as API Gateway (Azure)
    participant QuestsService as Quests Service
    participant AIProxyService as AI Proxy Service
    participant Gemini as Gemini API (External)
    participant RealtimeHub as Real-time Hub (SignalR)
    participant Database as Database (Supabase)

    %% Step 1: User initiates the process by uploading a syllabus %%
    U->>UI: Uploads a new course syllabus
    UI->>APIGateway: POST /syllabi/upload (Authenticated request with file)
    APIGateway->>QuestsService: Forwards request

    %% Step 2: Backend acknowledges the request and starts the job asynchronously %%
    QuestsService->>Database: CREATE UserUploadedSyllabus record with status 'Processing'
    QuestsService-->>APIGateway: 202 Accepted (Response is immediate)
    APIGateway-->>UI: 202 Accepted
    UI-->>U: Show notification: "Syllabus processing has started..."

    %% Step 3: The Quests Service uses the AI Proxy to analyze the document %%
    activate QuestsService
    Note right of QuestsService: The following steps happen in the background.
    QuestsService->>AIProxyService: RequestSyllabusAnalysis(syllabus_content)
    
    activate AIProxyService
    AIProxyService->>Gemini: POST /generateContent (Sends engineered prompt with syllabus text and requests structured JSON output)
    Gemini-->>AIProxyService: Returns structured JSON (learning objectives, topics, week-by-week breakdown)
    deactivate AIProxyService
    AIProxyService-->>QuestsService: Returns parsed and validated data

    %% Step 4: The Quests Service creates the learning content in the database %%
    QuestsService->>Database: CREATE SkillTree, Skills, QuestLine, QuestChapters, and Quests from AI data
    Database-->>QuestsService: Confirms creation of all entities
    QuestsService->>Database: UPDATE UserUploadedSyllabus record, set status to 'Completed'
    deactivate QuestsService

    %% Step 5: The user is notified in real-time upon completion %%
    QuestsService->>RealtimeHub: NotifyUser(userId, 'SyllabusProcessed')
    RealtimeHub-->>UI: Pushes real-time event to the user's client
    UI->>U: Display notification: "Your new Quest Line is ready!"
    U->>UI: Clicks to view the new learning path

    Note over U,Database: Dynamic learning system continuously adapts to user progre
```