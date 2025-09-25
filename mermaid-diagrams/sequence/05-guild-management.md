# Guild Management & Educator Dashboard Flow

```mermaid
sequenceDiagram
    participant GM as Guild Master (Lecturer)
    participant S as Student
    participant UI as Web Interface
    participant APIGateway as API Gateway
    participant SocialService as Social Service
    participant QuestsService as Quests Service
    participant RealtimeHub as Real-time Hub
    participant Database as Database

    %% Step 1: Guild Master creates a Guild %%
    GM->>UI: Creates a new Guild
    UI->>APIGateway: POST /guilds
    APIGateway->>SocialService: Forwards request
    SocialService->>Database: CREATE Guild record in its schema
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
    SocialService->>Database: Get all member IDs for the specified guild
    Database-->>SocialService: Returns list of user IDs
    
    Note right of SocialService: Secure, internal service-to-service call to fetch progress.
    SocialService->>QuestsService: GetBulkProgressForUsers(userIds)
    
    activate QuestsService
    QuestsService->>Database: Fetch quest progress for all specified members
    Database-->>QuestsService: Returns detailed progress data
    QuestsService-->>SocialService: Returns anonymized, aggregated progress statistics
    deactivate QuestsService
    
    Note right of SocialService: Social Service's analytics engine processes<br/>the aggregated data to find insights.
    SocialService->>SocialService: Process data to identify struggling topics, etc.
    
    SocialService-->>APIGateway: Returns final analytics payload
    deactivate SocialService
    APIGateway-->>UI: Returns analytics data to the client
    UI->>GM: Displays dashboard with actionable insights

    %% Step 5: Guild Master takes action %%
    GM->>UI: Creates a targeted announcement for the guild
    UI->>APIGateway: POST /guilds/{guildId}/announcements
    APIGateway->>SocialService: Forwards request to create announcement
    SocialService->>Database: Store announcement
    
    %% Step 6: Students are notified in real-time %%
    SocialService->>RealtimeHub: NotifyGuildMembers('NewAnnouncement', guildId)
    RealtimeHub-->>UI: Pushes real-time notification to all online guild members
    UI-->>S: Displays the new announcement
```