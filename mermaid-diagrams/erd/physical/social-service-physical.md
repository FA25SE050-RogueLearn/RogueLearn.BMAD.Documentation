erDiagram
    direction TB

    parties {
        uuid id PK "Primary key for the party"
        text name "Name of the party"
        text description "Description of the party"
        text type "Enum: StudyGroup, ProjectTeam, etc."
        int max_members "Maximum number of members"
        int current_member_count "Current number of members"
        bool is_public "Indicates if the party is public"
        uuid subject_id FK "FK to User Service: subjects table"
        uuid class_id FK "FK to User Service: classes table"
        uuid created_by FK "FK to User Service: user_profiles table"
        timestamp created_at "Timestamp of creation"
        timestamp updated_at "Last update timestamp"
        timestamp disbanded_at "Timestamp when disbanded"
    }

    party_members {
        uuid id PK "Unique identifier for membership"
        uuid party_id FK "FK to the party"
        uuid auth_user_id FK "FK to User Service: user_profiles table"
        text role "Enum: Leader, CoLeader, Member"
        text status "Enum: Active, Inactive, etc."
        timestamp joined_at "Timestamp when member joined"
        timestamp left_at "Timestamp when member left"
        int contribution_score "Member's contribution score"
    }

    party_invitations {
        uuid id PK "Unique identifier for an invitation"
        uuid party_id FK "FK to the party"
        uuid inviter_id FK "FK to User Service: user_profiles table (inviter)"
        uuid invitee_id FK "FK to User Service: user_profiles table (invitee)"
        text status "Enum: Pending, Accepted, etc."
        text message "Invitation message"
        timestamp invited_at "Timestamp when invited"
        timestamp responded_at "Timestamp of response"
        timestamp expires_at "Timestamp of expiration"
    }

    party_activities {
        uuid id PK "Unique identifier for a party activity"
        uuid party_id FK "FK to the party"
        text activity_type "Enum: QuestCompletion, StudySession, etc."
        text title "Title of the activity"
        text description "Description of the activity"
        uuid quest_id FK "FK to Quests Service: quests table"
        uuid meeting_id FK "FK to Meeting Service: meetings table"
        timestamp started_at "Timestamp when activity started"
        timestamp completed_at "Timestamp when activity completed"
        int experience_points_earned "XP earned from this activity"
        text participants "Array of auth_user_ids"
        json metadata "Additional activity-specific data"
    }

    guilds {
        uuid id PK "Primary key for the guild"
        text name "Name of the guild"
        text description "Description of the guild"
        text type "Enum: Academic, Professional, etc."
        int max_members "Maximum number of members"
        int current_member_count "Current member count"
        int level "Guild level"
        int experience_points "Guild experience points"
        bool is_public "Indicates if the guild is public"
        bool requires_approval "Indicates if joining requires approval"
        text banner_image_url "URL for the guild's banner"
        uuid created_by FK "FK to User Service: user_profiles table"
        timestamp created_at "Timestamp of creation"
        timestamp updated_at "Last update timestamp"
    }

    guild_members {
        uuid id PK "Unique identifier for membership"
        uuid guild_id FK "FK to the guild"
        uuid auth_user_id FK "FK to User Service: user_profiles table"
        text role "Enum: GuildMaster, Officer, Member, etc."
        text status "Enum: Active, Inactive, etc."
        timestamp joined_at "Timestamp when member joined"
        timestamp left_at "Timestamp when member left"
        int contribution_points "Member's contribution points to the guild"
        int rank_within_guild "Member's rank within the guild"
    }

    guild_invitations {
        uuid id PK "Unique identifier for an invitation or application"
        uuid guild_id FK "FK to the guild"
        uuid inviter_id FK "FK to User Service: user_profiles (null for applications)"
        uuid invitee_id FK "FK to User Service: user_profiles (the applicant or invitee)"
        text invitation_type "Enum: Invite, Application"
        text status "Enum: Pending, Accepted, etc."
        text message "Invitation or application message"
        timestamp created_at "Timestamp of creation"
        timestamp responded_at "Timestamp of response"
        timestamp expires_at "Timestamp of expiration"
    }

    guild_achievements {
        uuid id PK "Unique identifier for a guild achievement"
        uuid guild_id FK "FK to the guild"
        text achievement_name "Name of the achievement"
        text description "Description of the achievement"
        timestamp achieved_at "Timestamp when achieved"
        int experience_points_awarded "XP awarded to the guild"
        text contributing_members "Array of auth_user_ids who contributed"
        json metadata "Additional achievement-specific data"
    }

    friendships {
        uuid id PK "Unique identifier for a friendship link"
        uuid requester_id FK "FK to User Service: user_profiles (requester)"
        uuid addressee_id FK "FK to User Service: user_profiles (addressee)"
        text status "Enum: Pending, Accepted, Blocked"
        timestamp requested_at "Timestamp of friend request"
        timestamp accepted_at "Timestamp when request was accepted"
        timestamp blocked_at "Timestamp when blocked"
    }

    user_social_stats {
        uuid id PK "Unique identifier for a user's social stats"
        uuid auth_user_id UK,FK "FK to User Service: user_profiles table"
        int friends_count "Number of friends"
        int parties_joined "Number of parties joined"
        int parties_created "Number of parties created"
        int guilds_joined "Number of guilds joined"
        int guilds_created "Number of guilds created"
        text total_collaboration_hours "Total collaboration hours"
        int social_reputation_score "Calculated social score"
        timestamp last_updated_at "Last update timestamp"
    }

    social_messages {
        uuid id PK "Unique identifier for a message"
        uuid sender_id FK "FK to User Service: user_profiles (sender)"
        text message_type "Enum: Direct, Party, Guild"
        uuid recipient_id FK "FK to User Service: user_profiles (for Direct messages)"
        uuid party_id FK "FK to parties table (for Party messages)"
        uuid guild_id FK "FK to guilds table (for Guild messages)"
        text content "The message content"
        json attachments "Metadata for any attachments"
        bool is_system_message "Flag for system-generated messages"
        timestamp sent_at "Timestamp when sent"
        timestamp edited_at "Timestamp when edited"
        timestamp deleted_at "Timestamp when deleted"
    }

    message_reactions {
        uuid id PK "Unique identifier for a reaction"
        uuid message_id FK "FK to the message being reacted to"
        uuid auth_user_id FK "FK to User Service: user_profiles (reacter)"
        text reaction_emoji "The emoji used for the reaction"
        timestamp reacted_at "Timestamp of the reaction"
    }

    social_events {
        uuid id PK "Unique identifier for a social event"
        text title "Title of the event"
        text description "Description of the event"
        text event_type "Enum: StudySession, Workshop, etc."
        uuid organizer_id FK "FK to User Service: user_profiles (organizer)"
        uuid party_id FK "FK to parties table (if a party event)"
        uuid guild_id FK "FK to guilds table (if a guild event)"
        int max_participants "Maximum number of participants"
        int current_participants_count "Current participant count"
        timestamp scheduled_start_time "Scheduled start time"
        timestamp scheduled_end_time "Scheduled end time"
        text location_type "Enum: Virtual, Physical, Hybrid"
        json location_details "Details for the location (e.g., URL)"
        text status "Enum: Scheduled, InProgress, etc."
        timestamp created_at "Timestamp of creation"
        timestamp updated_at "Last update timestamp"
    }

    event_participants {
        uuid id PK "Unique identifier for participation"
        uuid event_id FK "FK to the social event"
        uuid auth_user_id FK "FK to User Service: user_profiles (participant)"
        text status "Enum: Registered, Attended, etc."
        timestamp registered_at "Timestamp of registration"
        timestamp attended_at "Timestamp of attendance"
        int feedback_rating "Feedback rating (1-5)"
        text feedback_comment "Feedback comment"
    }

    parties ||--o{ party_members : " "
    parties ||--o{ party_invitations : " "
    parties ||--o{ party_activities : " "
    parties ||--o{ social_messages : " "
    parties ||--o{ social_events : " "
    guilds ||--o{ guild_members : " "
    guilds ||--o{ guild_invitations : " "
    guilds ||--o{ guild_achievements : " "
    guilds ||--o{ social_messages : " "
    guilds ||--o{ social_events : " "
    social_messages ||--o{ message_reactions : " "
    social_events ||--o{ event_participants : " "