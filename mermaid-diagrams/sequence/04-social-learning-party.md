# Social Learning & Party Collaboration Flow

```mermaid
sequenceDiagram
    participant U as Student User
    participant UI as Web Interface (Next.js)
    participant APIGateway as API Gateway (Azure)
    participant SocialService as Social Service
    participant MeetingService as Meeting Service
    participant AIProxyService as AI Proxy Service
    participant Gemini as Gemini API (External)
    participant RealtimeHub as Real-time Hub (SignalR)
    participant Database as Database (Supabase)

    %% Step 1: User creates a study party %%
    U->>UI: Creates a new Party
    UI->>APIGateway: POST /parties
    APIGateway->>SocialService: Forwards request
    SocialService->>Database: CREATE Party and PartyMembership records
    Database-->>SocialService: Confirms creation
    SocialService-->>APIGateway: 201 Created
    APIGateway-->>UI: Party created successfully

    %% Step 2: User schedules a meeting for the party %%
    U->>UI: Schedules a new meeting for the Party
    UI->>APIGateway: POST /meetings (partyId, schedule, etc.)
    APIGateway->>MeetingService: Forwards request
    MeetingService->>Database: CREATE Meeting record
    MeetingService->>RealtimeHub: NotifyPartyMembers('NewMeeting')
    RealtimeHub-->>UI: Pushes real-time notification to party members

    %% Step 3: Meeting is conducted and content is captured %%
    U->>UI: Starts the scheduled meeting
    UI->>MeetingService: PUT /meetings/{id}/start
    Note over UI, MeetingService: The UI's built-in recording feature captures discussions and shared content during the session.
    UI->>MeetingService: POST /meetings/{id}/transcript-segment (streams content)
    MeetingService->>Database: INSERT TranscriptSegment record

    %% Step 4: Meeting ends and AI processing begins asynchronously %%
    U->>UI: Ends the meeting
    UI->>MeetingService: PUT /meetings/{id}/end
    MeetingService-->>UI: 200 OK (immediate response)
    
    activate MeetingService
    Note right of MeetingService: The following summary generation happens in the background.
    MeetingService->>AIProxyService: RequestMeetingSummary(meetingId, full_transcript)
    
    activate AIProxyService
    AIProxyService->>Gemini: POST /generateContent (prompt for structured summary and action items)
    Gemini-->>AIProxyService: Returns structured JSON summary
    deactivate AIProxyService
    AIProxyService-->>MeetingService: Returns validated summary data

    %% Step 5: Summary is stored and users are notified %%
    MeetingService->>Database: CREATE MeetingSummary record
    deactivate MeetingService
    
    MeetingService->>RealtimeHub: NotifyPartyMembers('SummaryReady', meetingId)
    RealtimeHub-->>UI: Pushes real-time notification to party members
    U->>UI: Clicks notification to view the AI-generated summary
```