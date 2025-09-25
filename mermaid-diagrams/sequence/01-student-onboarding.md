# Student Onboarding & Character Creation Flow

```mermaid
sequenceDiagram
    participant U as Student User
    participant UI as Web Interface (Next.js)
    participant SupabaseAuth as Supabase Auth (External)
    participant APIGateway as API Gateway (Azure)
    participant UserService as User Service
    participant QuestsService as Quests Service
    participant AIProxyService as AI Proxy Service
    participant Gemini as Gemini API (External)
    participant Database as Database (Supabase)

    %% Step 1: User Registration via Supabase SDK %%
    U->>UI: Clicks 'Sign Up' and submits credentials
    UI->>SupabaseAuth: Calls Supabase signUp() with credentials
    SupabaseAuth-->>UI: Confirms sign-up, returns session/JWT
    
    %% Step 2: Backend Profile Creation via DB Trigger %%
    Note over SupabaseAuth, Database: Supabase creates a new record in auth.users table
    Database->>UserService: PostgreSQL Trigger on new user INSERT fires, calling User Service endpoint
    UserService->>Database: CREATE UserProfile record with Supabase User UUID
    Database-->>UserService: Confirms creation
    UserService-->>Database: 200 OK
    
    %% Step 3: User completes onboarding and uploads syllabus %%
    U->>UI: Selects Class, Route, and uploads syllabus.pdf
    UI->>APIGateway: POST /enrollments/{enrollmentId}/upload-syllabus (with JWT & file)
    APIGateway->>QuestsService: Forwards request
    
    %% Step 4: QuestsService orchestrates asynchronous AI processing %%
    QuestsService->>Database: Marks UserUploadedSyllabus as 'Processing'
    QuestsService-->>APIGateway: 202 Accepted (responds immediately)
    APIGateway-->>UI: 202 Accepted
    
    QuestsService->>AIProxyService: ProcessSyllabus(file_content)
    Note right of QuestsService: This is an internal, secure service-to-service call.

    %% Step 5: AI Proxy handles the LLM interaction %%
    AIProxyService->>Gemini: POST /generateContent (sends engineered prompt with syllabus text)
    Gemini-->>AIProxyService: Returns structured JSON data (topics, concepts, etc.)
    AIProxyService-->>QuestsService: Returns parsed syllabus data
    
    %% Step 6: QuestsService creates the gamified content in the database %%
    QuestsService->>Database: CREATE QuestLine, Quests, SkillTree from parsed data
    Database-->>QuestsService: Confirms creation
    QuestsService->>Database: Updates UserUploadedSyllabus to 'Completed'
    
    %% Step 7: Frontend is notified of completion and user sees the result %%
    Note left of UI: Frontend can poll for status or receive a<br/>real-time update (e.g., via SignalR) to know processing is done.
    UI->>U: Displays notification: "Your learning path is ready!"
    U->>UI: Navigates to Dashboard and sees personalized QuestLine and SkillTree

    Note over U,Quest: User now has personalized<br/>learning profile ready
```