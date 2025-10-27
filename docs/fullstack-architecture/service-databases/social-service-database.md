# Social Service Database Schema

## Overview
The Social Service manages social interactions, party formation, and guild systems. It is responsible for community features but **no longer manages events or competitions**, which have been migrated to the new Event Service.

## Database Tables

### Party System

#### parties
Study groups and collaborative learning parties.
```sql
CREATE TABLE parties (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    party_type party_type NOT NULL,
    max_members INTEGER NOT NULL DEFAULT 6,
    current_member_count INTEGER NOT NULL DEFAULT 1,
    is_public BOOLEAN NOT NULL DEFAULT TRUE,
    created_by UUID NOT NULL, -- Soft FK to User Service: user_profiles table
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    disbanded_at TIMESTAMPTZ
);
```

#### party_members
Party membership tracking with roles and status.
```sql
CREATE TABLE party_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Soft FK to User Service: user_profiles table
    role party_role NOT NULL DEFAULT 'Member',
    status member_status NOT NULL DEFAULT 'Active',
    joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    left_at TIMESTAMPTZ,
    contribution_score INTEGER NOT NULL DEFAULT 0,
    UNIQUE (party_id, auth_user_id)
);
```

#### party_invitations
Party invitation system.
```sql
CREATE TABLE party_invitations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    inviter_id UUID NOT NULL, -- Soft FK to User Service: user_profiles table (inviter)
    invitee_id UUID NOT NULL, -- Soft FK to User Service: user_profiles table (invitee)
    status invitation_status NOT NULL DEFAULT 'Pending',
    message TEXT,
    invited_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    responded_at TIMESTAMPTZ,
    expires_at TIMESTAMPTZ NOT NULL DEFAULT (now() + INTERVAL '7 days'),
    UNIQUE (party_id, invitee_id)
);
```

#### party_activities
Tracks party collaborative activities and achievements.
```sql
CREATE TABLE party_activities (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    activity_type activity_type NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    quest_id UUID, -- Soft FK to Quests Service: quests table
    meeting_id UUID, -- Soft FK to Meeting Service: meetings table
    started_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    completed_at TIMESTAMPTZ,
    experience_points_earned INTEGER NOT NULL DEFAULT 0,
    participants UUID[] NOT NULL, -- Array of auth_user_ids
    metadata JSONB
);
```

#### party_stash_items
Snapshot of shared notes within a party (Arsenal sharing). Captures content at share-time while linking back to the original note.
```sql
CREATE TABLE party_stash_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    party_id UUID NOT NULL REFERENCES parties(id) ON DELETE CASCADE,
    original_note_id UUID NOT NULL, -- Soft FK to User Service: notes(id)
    shared_by_user_id UUID NOT NULL, -- Soft FK to User Service: user_profiles(auth_user_id)
    title TEXT NOT NULL,
    content JSONB NOT NULL,
    tags TEXT[],
    shared_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

### Guild System

#### guilds
Large-scale learning communities and organizations.
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
    created_by UUID NOT NULL, -- Soft FK to User Service: user_profiles table
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### guild_members
Guild membership with hierarchical roles.
```sql
CREATE TABLE guild_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Soft FK to User Service: user_profiles table
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
Guild invitation and application system.
```sql
CREATE TABLE guild_invitations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    guild_id UUID NOT NULL REFERENCES guilds(id) ON DELETE CASCADE,
    inviter_id UUID, -- Soft FK to User Service: user_profiles (null for applications)
    invitee_id UUID NOT NULL, -- Soft FK to User Service: user_profiles (the applicant or invitee)
    invitation_type invitation_type NOT NULL,
    status invitation_status NOT NULL DEFAULT 'Pending',
    message TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    responded_at TIMESTAMPTZ,
    expires_at TIMESTAMPTZ NOT NULL DEFAULT (now() + INTERVAL '14 days'),
    UNIQUE (guild_id, invitee_id)
);
```

### Social Interactions

#### friendships
User friendship connections.
```sql
CREATE TABLE friendships (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    requester_id UUID NOT NULL, -- Soft FK to User Service: user_profiles
    addressee_id UUID NOT NULL, -- Soft FK to User Service: user_profiles
    status friendship_status NOT NULL DEFAULT 'Pending',
    requested_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    accepted_at TIMESTAMPTZ,
    blocked_at TIMESTAMPTZ,
    UNIQUE (requester_id, addressee_id),
    CHECK (requester_id != addressee_id)
);
```

### Communication and Messaging

#### social_messages
Direct messages and party/guild communications.
```sql
CREATE TABLE social_messages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    sender_id UUID NOT NULL, -- Soft FK to User Service: user_profiles
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
Emoji reactions to messages.
```sql
CREATE TABLE message_reactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    message_id UUID NOT NULL REFERENCES social_messages(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Soft FK to User Service: user_profiles
    reaction_emoji VARCHAR(10) NOT NULL,
    reacted_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (message_id, auth_user_id, reaction_emoji)
);
```

-- REMOVED: `social_events` and `event_participants` tables have been migrated to the new Event Service.

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

-- Message types
CREATE TYPE message_type AS ENUM ('Direct', 'Party', 'Guild');
```

## Indexes for Performance

```sql
-- Party indexes
CREATE INDEX idx_parties_created_by ON parties(created_by);
CREATE INDEX idx_parties_party_type ON parties(party_type);
CREATE INDEX idx_parties_is_public ON parties(is_public);
CREATE INDEX idx_parties_created_at ON parties(created_at);
CREATE INDEX idx_parties_disbanded_at ON parties(disbanded_at);
CREATE INDEX idx_parties_public_type ON parties(is_public, party_type);
CREATE INDEX idx_parties_member_count ON parties(current_member_count, max_members);

-- Party member indexes
CREATE INDEX idx_party_members_party_id ON party_members(party_id);
CREATE INDEX idx_party_members_auth_user_id ON party_members(auth_user_id);
CREATE INDEX idx_party_members_status ON party_members(status);
CREATE INDEX idx_party_members_role ON party_members(role);
CREATE INDEX idx_party_members_joined_at ON party_members(joined_at);
CREATE INDEX idx_party_members_active ON party_members(party_id, status) WHERE status = 'Active';
CREATE INDEX idx_party_members_user_active ON party_members(auth_user_id, status) WHERE status = 'Active';

-- Party invitation indexes
CREATE INDEX idx_party_invitations_party_id ON party_invitations(party_id);
CREATE INDEX idx_party_invitations_inviter_id ON party_invitations(inviter_id);
CREATE INDEX idx_party_invitations_invitee_id ON party_invitations(invitee_id);
CREATE INDEX idx_party_invitations_status ON party_invitations(status);
CREATE INDEX idx_party_invitations_expires_at ON party_invitations(expires_at);
CREATE INDEX idx_party_invitations_pending ON party_invitations(invitee_id, status) WHERE status = 'Pending';

-- Party activity indexes
CREATE INDEX idx_party_activities_party_id ON party_activities(party_id);
CREATE INDEX idx_party_activities_activity_type ON party_activities(activity_type);
CREATE INDEX idx_party_activities_started_at ON party_activities(started_at);
CREATE INDEX idx_party_activities_completed_at ON party_activities(completed_at);
CREATE INDEX idx_party_activities_quest_id ON party_activities(quest_id);
CREATE INDEX idx_party_activities_meeting_id ON party_activities(meeting_id);
CREATE INDEX idx_party_activities_participants ON party_activities USING GIN(participants);

-- Party stash indexes
CREATE INDEX idx_party_stash_items_party_id ON party_stash_items(party_id);
CREATE INDEX idx_party_stash_items_shared_by_user_id ON party_stash_items(shared_by_user_id);
CREATE INDEX idx_party_stash_items_original_note_id ON party_stash_items(original_note_id);
CREATE INDEX idx_party_stash_items_shared_at ON party_stash_items(shared_at);
CREATE INDEX idx_party_stash_items_updated_at ON party_stash_items(updated_at);
CREATE INDEX idx_party_stash_items_tags ON party_stash_items USING GIN(tags);

-- Guild indexes
CREATE INDEX idx_guilds_guild_type ON guilds(guild_type);
CREATE INDEX idx_guilds_is_public ON guilds(is_public);
CREATE INDEX idx_guilds_created_by ON guilds(created_by);
CREATE INDEX idx_guilds_level ON guilds(level);
CREATE INDEX idx_guilds_experience_points ON guilds(experience_points);
CREATE INDEX idx_guilds_created_at ON guilds(created_at);
CREATE INDEX idx_guilds_public_type ON guilds(is_public, guild_type);
CREATE INDEX idx_guilds_member_count ON guilds(current_member_count, max_members);
CREATE INDEX idx_guilds_requires_approval ON guilds(requires_approval);

-- Guild member indexes
CREATE INDEX idx_guild_members_guild_id ON guild_members(guild_id);
CREATE INDEX idx_guild_members_auth_user_id ON guild_members(auth_user_id);
CREATE INDEX idx_guild_members_role ON guild_members(role);
CREATE INDEX idx_guild_members_status ON guild_members(status);
CREATE INDEX idx_guild_members_joined_at ON guild_members(joined_at);
CREATE INDEX idx_guild_members_contribution_points ON guild_members(contribution_points);
CREATE INDEX idx_guild_members_rank_within_guild ON guild_members(rank_within_guild);
CREATE INDEX idx_guild_members_active ON guild_members(guild_id, status) WHERE status = 'Active';
CREATE INDEX idx_guild_members_user_active ON guild_members(auth_user_id, status) WHERE status = 'Active';

-- Guild invitation indexes
CREATE INDEX idx_guild_invitations_guild_id ON guild_invitations(guild_id);
CREATE INDEX idx_guild_invitations_inviter_id ON guild_invitations(inviter_id);
CREATE INDEX idx_guild_invitations_invitee_id ON guild_invitations(invitee_id);
CREATE INDEX idx_guild_invitations_invitation_type ON guild_invitations(invitation_type);
CREATE INDEX idx_guild_invitations_status ON guild_invitations(status);
CREATE INDEX idx_guild_invitations_created_at ON guild_invitations(created_at);
CREATE INDEX idx_guild_invitations_expires_at ON guild_invitations(expires_at);
CREATE INDEX idx_guild_invitations_pending ON guild_invitations(invitee_id, status) WHERE status = 'Pending';

-- Friendship indexes
CREATE INDEX idx_friendships_requester_id ON friendships(requester_id);
CREATE INDEX idx_friendships_addressee_id ON friendships(addressee_id);
CREATE INDEX idx_friendships_status ON friendships(status);
CREATE INDEX idx_friendships_requested_at ON friendships(requested_at);
CREATE INDEX idx_friendships_accepted_at ON friendships(accepted_at);
CREATE INDEX idx_friendships_blocked_at ON friendships(blocked_at);
CREATE INDEX idx_friendships_pending ON friendships(addressee_id, status) WHERE status = 'Pending';
CREATE INDEX idx_friendships_accepted ON friendships(requester_id, addressee_id, status) WHERE status = 'Accepted';

-- Social message indexes
CREATE INDEX idx_social_messages_sender_id ON social_messages(sender_id);
CREATE INDEX idx_social_messages_recipient_id ON social_messages(recipient_id);
CREATE INDEX idx_social_messages_party_id ON social_messages(party_id);
CREATE INDEX idx_social_messages_guild_id ON social_messages(guild_id);
CREATE INDEX idx_social_messages_message_type ON social_messages(message_type);
CREATE INDEX idx_social_messages_sent_at ON social_messages(sent_at);
CREATE INDEX idx_social_messages_edited_at ON social_messages(edited_at);
CREATE INDEX idx_social_messages_deleted_at ON social_messages(deleted_at);
CREATE INDEX idx_social_messages_is_system_message ON social_messages(is_system_message);
CREATE INDEX idx_social_messages_party_recent ON social_messages(party_id, sent_at DESC) WHERE deleted_at IS NULL;
CREATE INDEX idx_social_messages_guild_recent ON social_messages(guild_id, sent_at DESC) WHERE deleted_at IS NULL;
CREATE INDEX idx_social_messages_direct_recent ON social_messages(recipient_id, sent_at DESC) WHERE message_type = 'Direct' AND deleted_at IS NULL;

-- Message reaction indexes
CREATE INDEX idx_message_reactions_message_id ON message_reactions(message_id);
CREATE INDEX idx_message_reactions_auth_user_id ON message_reactions(auth_user_id);
CREATE INDEX idx_message_reactions_reaction_emoji ON message_reactions(reaction_emoji);
CREATE INDEX idx_message_reactions_reacted_at ON message_reactions(reacted_at);
CREATE INDEX idx_message_reactions_message_emoji ON message_reactions(message_id, reaction_emoji);
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
- **Event Service**: Organizes team competitions and tournaments

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