# Social Service Database Schema

## Overview
The Social Service manages social interactions, party formation, guild systems, and collaborative learning experiences. It enables students to form study groups, join guilds, and participate in collaborative learning activities.

## Database Tables

### Party System

#### parties
Study groups and collaborative learning parties
```sql
CREATE TABLE parties (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    party_type party_type NOT NULL,
    max_members INTEGER NOT NULL DEFAULT 6,
    current_member_count INTEGER NOT NULL DEFAULT 1,
    is_public BOOLEAN NOT NULL DEFAULT TRUE,
    subject_id UUID, -- Reference to subjects table in User Service
    class_id UUID, -- Reference to classes table in User Service
    created_by UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    disbanded_at TIMESTAMPTZ
);
```

#### party_members
Party membership tracking with roles and status
```sql
CREATE TABLE party_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    role party_role NOT NULL DEFAULT 'Member',
    status member_status NOT NULL DEFAULT 'Active',
    joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    left_at TIMESTAMPTZ,
    contribution_score INTEGER NOT NULL DEFAULT 0,
    UNIQUE (party_id, auth_user_id)
);
```

#### party_invitations
Party invitation system
```sql
CREATE TABLE party_invitations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    inviter_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    invitee_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    status invitation_status NOT NULL DEFAULT 'Pending',
    message TEXT,
    invited_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    responded_at TIMESTAMPTZ,
    expires_at TIMESTAMPTZ NOT NULL DEFAULT (now() + INTERVAL '7 days'),
    UNIQUE (party_id, invitee_id)
);
```

#### party_activities
Tracks party collaborative activities and achievements
```sql
CREATE TABLE party_activities (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    activity_type activity_type NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    quest_id UUID, -- Reference to quests in Quests Service
    meeting_id UUID, -- Reference to meetings in Meeting Service
    started_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    completed_at TIMESTAMPTZ,
    experience_points_earned INTEGER NOT NULL DEFAULT 0,
    participants UUID[] NOT NULL, -- Array of auth_user_ids
    metadata JSONB
);
```

### Guild System

#### guilds
Large-scale learning communities and organizations
```sql
CREATE TABLE guilds (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL UNIQUE,
    description TEXT NOT NULL,
    guild_type guild_type NOT NULL,
    max_members INTEGER NOT NULL DEFAULT 100,
    current_member_count INTEGER NOT NULL DEFAULT 1,
    level INTEGER NOT NULL DEFAULT 1,
    experience_points INTEGER NOT NULL DEFAULT 0,
    is_public BOOLEAN NOT NULL DEFAULT TRUE,
    requires_approval BOOLEAN NOT NULL DEFAULT FALSE,
    banner_image_url TEXT,
    created_by UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### guild_members
Guild membership with hierarchical roles
```sql
CREATE TABLE guild_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    role guild_role NOT NULL DEFAULT 'Member',
    status member_status NOT NULL DEFAULT 'Active',
    joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    left_at TIMESTAMPTZ,
    contribution_points INTEGER NOT NULL DEFAULT 0,
    rank_within_guild INTEGER,
    UNIQUE (guild_id, auth_user_id)
);
```

#### guild_invitations
Guild invitation and application system
```sql
CREATE TABLE guild_invitations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    inviter_id UUID, -- Reference to user_profiles.auth_user_id in User Service (null for applications)
    invitee_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    invitation_type invitation_type NOT NULL,
    status invitation_status NOT NULL DEFAULT 'Pending',
    message TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    responded_at TIMESTAMPTZ,
    expires_at TIMESTAMPTZ NOT NULL DEFAULT (now() + INTERVAL '14 days'),
    UNIQUE (guild_id, invitee_id)
);
```

#### guild_achievements
Guild-level achievements and milestones
```sql
CREATE TABLE guild_achievements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    achievement_name VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    achieved_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    experience_points_awarded INTEGER NOT NULL DEFAULT 0,
    contributing_members UUID[] NOT NULL, -- Array of auth_user_ids who contributed
    metadata JSONB
);
```

### Social Interactions

#### friendships
User friendship connections
```sql
CREATE TABLE friendships (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    requester_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    addressee_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    status friendship_status NOT NULL DEFAULT 'Pending',
    requested_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    accepted_at TIMESTAMPTZ,
    blocked_at TIMESTAMPTZ,
    UNIQUE (requester_id, addressee_id),
    CHECK (requester_id != addressee_id)
);
```

#### user_social_stats
Aggregated social statistics for users
```sql
CREATE TABLE user_social_stats (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    auth_user_id UUID NOT NULL UNIQUE, -- Reference to user_profiles.auth_user_id in User Service
    friends_count INTEGER NOT NULL DEFAULT 0,
    parties_joined INTEGER NOT NULL DEFAULT 0,
    parties_created INTEGER NOT NULL DEFAULT 0,
    guilds_joined INTEGER NOT NULL DEFAULT 0,
    guilds_created INTEGER NOT NULL DEFAULT 0,
    total_collaboration_hours DECIMAL(10,2) NOT NULL DEFAULT 0.00,
    social_reputation_score INTEGER NOT NULL DEFAULT 0,
    last_updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

### Communication and Messaging

#### social_messages
Direct messages and party/guild communications
```sql
CREATE TABLE social_messages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    sender_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    message_type message_type NOT NULL,
    recipient_id UUID, -- For direct messages
    party_id UUID REFERENCES parties(id) ON DELETE CASCADE, -- For party messages
    guild_id UUID REFERENCES guilds(id) ON DELETE CASCADE, -- For guild messages
    content TEXT NOT NULL,
    attachments JSONB, -- File attachments metadata
    is_system_message BOOLEAN NOT NULL DEFAULT FALSE,
    sent_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    edited_at TIMESTAMPTZ,
    deleted_at TIMESTAMPTZ,
    CHECK (
        (message_type = 'Direct' AND recipient_id IS NOT NULL AND party_id IS NULL AND guild_id IS NULL) OR
        (message_type = 'Party' AND party_id IS NOT NULL AND recipient_id IS NULL AND guild_id IS NULL) OR
        (message_type = 'Guild' AND guild_id IS NOT NULL AND recipient_id IS NULL AND party_id IS NULL)
    )
);
```

#### message_reactions
Emoji reactions to messages
```sql
CREATE TABLE message_reactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    message_id UUID NOT NULL REFERENCES social_messages(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    reaction_emoji VARCHAR(10) NOT NULL,
    reacted_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (message_id, auth_user_id, reaction_emoji)
);
```

### Events and Activities

#### social_events
Social learning events and activities
```sql
CREATE TABLE social_events (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    event_type event_type NOT NULL,
    organizer_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    party_id UUID REFERENCES parties(id) ON DELETE SET NULL,
    guild_id UUID REFERENCES guilds(id) ON DELETE SET NULL,
    max_participants INTEGER,
    current_participants_count INTEGER NOT NULL DEFAULT 0,
    scheduled_start_time TIMESTAMPTZ NOT NULL,
    scheduled_end_time TIMESTAMPTZ NOT NULL,
    location_type location_type NOT NULL,
    location_details JSONB, -- Virtual meeting links or physical location
    status event_status NOT NULL DEFAULT 'Scheduled',
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### event_participants
Event participation tracking
```sql
CREATE TABLE event_participants (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    event_id UUID NOT NULL REFERENCES social_events(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    status participation_status NOT NULL DEFAULT 'Registered',
    registered_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    attended_at TIMESTAMPTZ,
    feedback_rating INTEGER CHECK (feedback_rating >= 1 AND feedback_rating <= 5),
    feedback_comment TEXT,
    UNIQUE (event_id, auth_user_id)
);
```

## Enums and Types

```sql
-- Party types
CREATE TYPE party_type AS ENUM ('StudyGroup', 'ProjectTeam', 'PeerReview', 'Casual', 'Competition');

-- Party and guild member roles
CREATE TYPE party_role AS ENUM ('Leader', 'CoLeader', 'Member');
CREATE TYPE guild_role AS ENUM ('GuildMaster', 'Officer', 'Veteran', 'Member', 'Recruit');

-- Member status
CREATE TYPE member_status AS ENUM ('Active', 'Inactive', 'Suspended', 'Left');

-- Invitation status
CREATE TYPE invitation_status AS ENUM ('Pending', 'Accepted', 'Declined', 'Expired', 'Cancelled');

-- Invitation types
CREATE TYPE invitation_type AS ENUM ('Invite', 'Application');

-- Guild types
CREATE TYPE guild_type AS ENUM ('Academic', 'Professional', 'Hobby', 'Competition', 'Study', 'Research');

-- Activity types
CREATE TYPE activity_type AS ENUM ('QuestCompletion', 'StudySession', 'ProjectWork', 'Discussion', 'Competition', 'Meeting');

-- Friendship status
CREATE TYPE friendship_status AS ENUM ('Pending', 'Accepted', 'Blocked');

-- Message types
CREATE TYPE message_type AS ENUM ('Direct', 'Party', 'Guild');

-- Event types
CREATE TYPE event_type AS ENUM ('StudySession', 'Workshop', 'Competition', 'Social', 'Presentation', 'Review');

-- Location types
CREATE TYPE location_type AS ENUM ('Virtual', 'Physical', 'Hybrid');

-- Event status
CREATE TYPE event_status AS ENUM ('Scheduled', 'InProgress', 'Completed', 'Cancelled', 'Postponed');

-- Participation status
CREATE TYPE participation_status AS ENUM ('Registered', 'Attended', 'NoShow', 'Cancelled');
```

## Indexes for Performance

```sql
-- Party indexes
CREATE INDEX idx_parties_created_by ON parties(created_by);
CREATE INDEX idx_parties_party_type ON parties(party_type);
CREATE INDEX idx_parties_subject_id ON parties(subject_id);
CREATE INDEX idx_parties_class_id ON parties(class_id);
CREATE INDEX idx_parties_is_public ON parties(is_public);

-- Party member indexes
CREATE INDEX idx_party_members_party_id ON party_members(party_id);
CREATE INDEX idx_party_members_auth_user_id ON party_members(auth_user_id);
CREATE INDEX idx_party_members_status ON party_members(status);

-- Guild indexes
CREATE INDEX idx_guilds_guild_type ON guilds(guild_type);
CREATE INDEX idx_guilds_is_public ON guilds(is_public);
CREATE INDEX idx_guilds_created_by ON guilds(created_by);

-- Guild member indexes
CREATE INDEX idx_guild_members_guild_id ON guild_members(guild_id);
CREATE INDEX idx_guild_members_auth_user_id ON guild_members(auth_user_id);
CREATE INDEX idx_guild_members_role ON guild_members(role);

-- Social interaction indexes
CREATE INDEX idx_friendships_requester_id ON friendships(requester_id);
CREATE INDEX idx_friendships_addressee_id ON friendships(addressee_id);
CREATE INDEX idx_friendships_status ON friendships(status);

-- Message indexes
CREATE INDEX idx_social_messages_sender_id ON social_messages(sender_id);
CREATE INDEX idx_social_messages_recipient_id ON social_messages(recipient_id);
CREATE INDEX idx_social_messages_party_id ON social_messages(party_id);
CREATE INDEX idx_social_messages_guild_id ON social_messages(guild_id);
CREATE INDEX idx_social_messages_sent_at ON social_messages(sent_at);

-- Event indexes
CREATE INDEX idx_social_events_organizer_id ON social_events(organizer_id);
CREATE INDEX idx_social_events_event_type ON social_events(event_type);
CREATE INDEX idx_social_events_scheduled_start_time ON social_events(scheduled_start_time);
CREATE INDEX idx_social_events_status ON social_events(status);
CREATE INDEX idx_event_participants_event_id ON event_participants(event_id);
CREATE INDEX idx_event_participants_auth_user_id ON event_participants(auth_user_id);
```

## Service Responsibilities

### Primary Responsibilities
- Party formation and management for collaborative learning
- Guild system for large-scale learning communities
- Social networking and friendship management
- Event organization and participation tracking
- Social messaging and communication
- Social statistics and reputation tracking

### Cross-Service Integration
- **User Service**: Retrieves user profiles and academic context
- **Quests Service**: Integrates collaborative quest completion
- **Meeting Service**: Links social events with meeting sessions
- **Code Battle Service**: Organizes team competitions and tournaments

### Data Access Patterns
- **Read/Write Access**: All tables within Social Service domain
- **External References**: User profiles, subjects, classes, and Skill Catalog from User Service
- **API Integration**: Exposes social features via REST APIs and WebSocket connections
- **Event Publishing**: Publishes social events for achievement and notification systems; emits Reward Cascade events (e.g., XP for party activities via `party_activities.experience_points_earned`) to User Service for authoritative ledgering in `user_skill_rewards` per PRD FR58

### Real-time Features
- **WebSocket Integration**: Real-time messaging, party updates, and event notifications
- **Live Updates**: Party member status, guild activities, and social interactions
- **Notification System**: Social invitations, friend requests, and event reminders

## Ownership and Integration Notes

- **Rewards Ledger Non-Ownership**: Social Service may calculate or accumulate experience for party/guild activities but does not own the authoritative XP ledger. All reward events are published to the User Service and persisted in `user_skill_rewards` with correlation IDs.
- **Skill Tree Catalog Non-Ownership**: Social Service does not own or define skill catalog tables; it consumes the Skill Catalog from User Service for tagging activities and recommendations.