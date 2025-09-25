# Guild Management & Educator Dashboard Flow

```mermaid
sequenceDiagram
    participant GM as Guild Master (Lecturer)
    participant S as Student
    participant UI as Web Interface (Next.js)
    participant APIGateway as API Gateway (Azure)
    participant SocialService as Social Service
    participant QuestsService as Quests Service
    participant RealtimeHub as Real-time Hub (SignalR)
    participant Database as Database (Supabase)

    %% Step 1: Guild Master creates a Guild %%
    GM->>UI: Creates a new Guild
    UI->>APIGateway: POST /guilds
    APIGateway->>SocialService: Forwards request
    SocialService->>Database: CREATE Guild record
    SocialService-->>APIGateway: 201 Created
    APIGateway-->>UI: Guild created

    %% Step 2: Student joins the Guild %%
    S->>UI: Finds and joins the Guild
    UI->>APIGateway: POST /guilds/{guildId}/join
    APIGateway->>SocialService: Forwards join request
    SocialService->>Database: CREATE GuildMembership record
    SocialService-->>APIGateway: 200 OK

    %% Step 3: Guild Master views the Analytics Dashboard %%
    GM->>UI: Navigates to the Guild Dashboard
    UI->>APIGateway: GET /guilds/{guildId}/analytics
    APIGateway->>SocialService: Forwards analytics request
    
    %% Step 4: Backend services collaborate to aggregate anonymized data %%
    activate SocialService
    SocialService->>Database: Get all member IDs for the guild
    Database-->>SocialService: Returns list of user IDs
    
    Note right of SocialService: Internal service-to-service call<br/>to fetch progress data.
    SocialService->>QuestsService: GetBulkProgress(userIds)
    activate QuestsService
    QuestsService->>Database: Fetch quest progress for all members
    Database-->>QuestsService: Returns progress data
    QuestsService-->>SocialService: Returns anonymized, aggregated data
    deactivate QuestsService
    
    Note right of SocialService: Analytics engine processes data<br/>to identify struggling topics.
    SocialService->>SocialService: Process data to find insights
    
    SocialService-->>APIGateway: Returns analytics payload (e.g., struggling topics)
    deactivate SocialService
    APIGateway-->>UI: Returns analytics data
    UI->>GM: Displays dashboard with insights

    %% Step 5: Guild Master takes action based on insights %%
    GM->>UI: Creates a targeted announcement
    UI->>APIGateway: POST /guilds/{guildId}/announcements
    APIGateway->>SocialService: Forwards request
    SocialService->>Database: Store announcement
    
    %% Step 6: Students are notified %%
    SocialService->>RealtimeHub: NotifyGuildMembers('NewAnnouncement', guildId)
    RealtimeHub-->>UI: Pushes real-time notification to all guild members' UIs
    UI-->>S: Displays new announcement
```