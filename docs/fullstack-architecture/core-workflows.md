# **Core Workflows**

### **Workflow 1: New User Onboarding & First QuestLine Generation**

```mermaid
sequenceDiagram
    participant User
    participant Frontend (Vercel)
    participant SupabaseAuth (External)
    participant APIGateway (Azure)
    participant AuthService
    participant QuestsService
    participant AIProxyService
    participant Gemini (External)
    participant Database (Supabase)

    %% Step 1: User Registration via Supabase SDK %%
    User->>Frontend: Clicks 'Sign Up'
    Frontend->>SupabaseAuth: Calls Supabase signUp() with credentials
    SupabaseAuth-->>User: Completes sign-up & sets auth token in browser
    
    %% Step 2: Supabase Trigger notifies our backend of the new user %%
    Note over SupabaseAuth, Database: Supabase creates a new record in auth.users table
    Database->>AuthService: PostgreSQL Trigger on new user INSERT fires, calling AuthService endpoint
    AuthService->>Database: CREATE UserProfile record with Supabase User UUID
    Database-->>AuthService: Confirms creation
    AuthService-->>Database: 200 OK
    
    %% Step 3: User is back on our app and uploads syllabus %%
    User->>Frontend: Uploads syllabus.pdf for an enrollment
    Frontend->>APIGateway: POST /enrollments/{enrollmentId}/upload-syllabus (with JWT & file)
    APIGateway->>QuestsService: Forwards request
    
    %% Step 4: QuestsService orchestrates AI processing %%
    QuestsService->>Database: Marks UserUploadedSyllabus as 'Processing'
    QuestsService->>AIProxyService: ProcessSyllabus(file_content)
    Note right of QuestsService: This is an internal, secure service-to-service call.

    %% Step 5: AI Proxy handles the LLM interaction %%
    AIProxyService->>Gemini: POST /generateContent (sends engineered prompt)
    Gemini-->>AIProxyService: Returns structured JSON data
    AIProxyService-->>QuestsService: Returns parsed syllabus data
    
    %% Step 6: QuestsService creates the gamified content %%
    QuestsService->>Database: CREATE QuestLine, Quests, SkillTree
    Database-->>QuestsService: Confirms creation
    QuestsService->>Database: Updates UserUploadedSyllabus to 'Completed'
    
    %% Step 7: Frontend is notified of completion %%
    QuestsService-->>APIGateway: 202 Accepted (or success)
    APIGateway-->>Frontend: 202 Accepted
    Note left of Frontend: Frontend can now poll for status<br/>or receive a real-time update<br/>to show the new QuestLine.
    User->>Frontend: Sees personalized QuestLine and SkillTree
```

### **Workflow 2: Meeting Scheduling & Management**

```mermaid
sequenceDiagram
    participant User
    participant Frontend (Vercel)
    participant APIGateway (Azure)
    participant MeetingService
    participant NotificationService
    participant Database (Supabase)
    participant PartyMembers

    %% Step 1: User creates a meeting %%
    User->>Frontend: Creates new meeting for party
    Frontend->>APIGateway: POST /meetings (with JWT, partyId, title, schedule)
    APIGateway->>MeetingService: Forwards request
    
    %% Step 2: Meeting service validates and creates meeting %%
    MeetingService->>Database: Validates user is party member
    MeetingService->>Database: CREATE Meeting record
    Database-->>MeetingService: Returns meeting ID
    
    %% Step 3: Auto-invite all party members %%
    MeetingService->>Database: CREATE MeetingParticipant records for all party members
    MeetingService->>NotificationService: Send meeting invitations
    NotificationService->>PartyMembers: Sends meeting notifications
    
    %% Step 4: Participants respond to invitation %%
    PartyMembers->>Frontend: Accept/Decline meeting
    Frontend->>APIGateway: PUT /meetings/{meetingId}/participants/{userId}/status
    APIGateway->>MeetingService: Updates participant status
    MeetingService->>Database: UPDATE MeetingParticipant status
    
    %% Step 5: Meeting starts %%
    User->>Frontend: Starts meeting
    Frontend->>APIGateway: PUT /meetings/{meetingId} (status: InProgress)
    APIGateway->>MeetingService: Updates meeting status
    MeetingService->>Database: UPDATE Meeting status to 'InProgress'
    
    %% Step 6: Real-time collaboration %%
    Note over User, PartyMembers: Real-time features via WebSocket/SignalR
    User->>Frontend: Adds agenda items, takes notes
    Frontend->>APIGateway: POST /meetings/{meetingId}/agenda, /notes
    APIGateway->>MeetingService: Creates agenda/notes
    MeetingService->>Database: Stores meeting content
    
    %% Step 7: Meeting completion %%
    User->>Frontend: Ends meeting
    Frontend->>APIGateway: PUT /meetings/{meetingId} (status: Completed)
    APIGateway->>MeetingService: Finalizes meeting
    MeetingService->>Database: UPDATE Meeting status, final notes
    MeetingService->>NotificationService: Send meeting summary
    NotificationService->>PartyMembers: Meeting summary notifications
```

### **Workflow 3: Code Battle Event Participation**

```mermaid
sequenceDiagram
    participant User
    participant Frontend (Vercel)
    participant APIGateway (Azure)
    participant CodeBattleService
    parameter JudgeService
    participant Database (Supabase)
    participant EventParticipants

    %% Step 1: User joins code battle event %%
    User->>Frontend: Joins code battle event
    Frontend->>APIGateway: POST /events/{eventId}/rooms (creates/joins room)
    APIGateway->>CodeBattleService: Forwards request
    
    %% Step 2: Room management %%
    CodeBattleService->>Database: CREATE/UPDATE Room, RoomPlayer records
    CodeBattleService->>Database: Fetch event problems and test cases
    Database-->>CodeBattleService: Returns room and problem data
    
    %% Step 3: Real-time battle begins %%
    Note over User, EventParticipants: Real-time features via WebSocket/SignalR
    CodeBattleService->>Frontend: Sends problem details to all participants
    Frontend-->>User: Displays coding problem and editor
    
    %% Step 4: User submits solution %%
    User->>Frontend: Writes and submits code
    Frontend->>APIGateway: POST /submissions (eventId, problemId, code)
    APIGateway->>CodeBattleService: Forwards submission
    
    %% Step 5: Code evaluation %%
    CodeBattleService->>Database: CREATE Submission record (status: Pending)
    CodeBattleService->>JudgeService: Evaluate code against test cases
    JudgeService->>Database: Fetch test cases for problem
    
    %% Step 6: Judge service processes submission %%
    Note over JudgeService: Compiles and runs code<br/>against all test cases
    JudgeService->>Database: UPDATE Submission with results
    JudgeService-->>CodeBattleService: Returns evaluation results
    
    %% Step 7: Real-time results and leaderboard %%
    CodeBattleService->>Database: UPDATE LeaderboardEntry scores
    CodeBattleService->>EventParticipants: Broadcasts results via WebSocket
    Frontend-->>User: Shows submission results and updated leaderboard
    
    %% Step 8: Event completion %%
    Note over CodeBattleService: When event time expires
    CodeBattleService->>Database: Finalize all submissions and rankings
    CodeBattleService->>EventParticipants: Sends final results and rewards
```

### **Workflow 4: Real-time Code Battle Room Management**

```mermaid
sequenceDiagram
    participant Player1
    participant Player2
    participant Frontend (Vercel)
    participant APIGateway (Azure)
    participant CodeBattleService
    participant WebSocketHub
    participant Database (Supabase)

    %% Step 1: Players join room %%
    Player1->>Frontend: Joins battle room
    Player2->>Frontend: Joins battle room
    Frontend->>APIGateway: POST /rooms/{roomId}/join
    APIGateway->>CodeBattleService: Adds players to room
    
    %% Step 2: Real-time connection establishment %%
    CodeBattleService->>Database: UPDATE RoomPlayer records
    CodeBattleService->>WebSocketHub: Establish real-time connections
    WebSocketHub-->>Player1: Connected to room
    WebSocketHub-->>Player2: Connected to room
    
    %% Step 3: Battle synchronization %%
    CodeBattleService->>WebSocketHub: Broadcast problem to all players
    WebSocketHub->>Player1: Sends problem details
    WebSocketHub->>Player2: Sends problem details
    
    %% Step 4: Real-time coding and updates %%
    Player1->>Frontend: Types code solution
    Frontend->>WebSocketHub: Sends typing indicators (optional)
    WebSocketHub->>Player2: Shows Player1 is coding
    
    %% Step 5: Submission race %%
    Player1->>Frontend: Submits solution first
    Frontend->>APIGateway: POST /submissions
    APIGateway->>CodeBattleService: Processes submission
    CodeBattleService->>WebSocketHub: Broadcast submission event
    WebSocketHub->>Player2: Shows Player1 submitted
    
    %% Step 6: Live results %%
    CodeBattleService->>Database: Gets evaluation results
    CodeBattleService->>WebSocketHub: Broadcast results
    WebSocketHub->>Player1: Shows own results
    WebSocketHub->>Player2: Shows competitor results
    
    %% Step 7: Room completion %%
    Note over CodeBattleService: When all problems solved or time expires
    CodeBattleService->>WebSocketHub: Broadcast final rankings
    WebSocketHub->>Player1: Final results and rewards
    WebSocketHub->>Player2: Final results and rewards
```

