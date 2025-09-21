# Meeting Service Database Schema

## Overview
The Meeting Service manages virtual meetings, study sessions, and collaborative learning environments. It provides comprehensive meeting lifecycle management, participant tracking, and integration with other learning services.

## Database Tables

### Meeting Management

#### meetings
Core meeting definitions and metadata
```sql
CREATE TABLE meetings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    meeting_type meeting_type NOT NULL,
    creator_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    party_id UUID, -- Reference to parties in Social Service
    guild_id UUID, -- Reference to guilds in Social Service
    class_id UUID, -- Reference to classes in User Service
    subject_id UUID, -- Reference to subjects in User Service
    scheduled_start_time TIMESTAMPTZ NOT NULL,
    scheduled_end_time TIMESTAMPTZ NOT NULL,
    actual_start_time TIMESTAMPTZ,
    actual_end_time TIMESTAMPTZ,
    timezone VARCHAR(50) NOT NULL DEFAULT 'UTC',
    max_participants INTEGER,
    current_participants_count INTEGER NOT NULL DEFAULT 0,
    meeting_url TEXT, -- Virtual meeting room URL
    meeting_password VARCHAR(255),
    status meeting_status NOT NULL DEFAULT 'Scheduled',
    is_recurring BOOLEAN NOT NULL DEFAULT FALSE,
    recurrence_pattern JSONB, -- Recurrence configuration
    recording_enabled BOOLEAN NOT NULL DEFAULT FALSE,
    recording_url TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### meeting_participants
Meeting participation tracking with roles and status
```sql
CREATE TABLE meeting_participants (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    role participant_role NOT NULL DEFAULT 'Participant',
    status participation_status NOT NULL DEFAULT 'Invited',
    invited_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    joined_at TIMESTAMPTZ,
    left_at TIMESTAMPTZ,
    total_duration_minutes INTEGER NOT NULL DEFAULT 0,
    is_presenter BOOLEAN NOT NULL DEFAULT FALSE,
    can_share_screen BOOLEAN NOT NULL DEFAULT TRUE,
    can_use_chat BOOLEAN NOT NULL DEFAULT TRUE,
    UNIQUE (meeting_id, auth_user_id)
);
```

#### meeting_invitations
Meeting invitation system with RSVP tracking
```sql
CREATE TABLE meeting_invitations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    inviter_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    invitee_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    status invitation_status NOT NULL DEFAULT 'Pending',
    message TEXT,
    invited_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    responded_at TIMESTAMPTZ,
    response_message TEXT,
    reminder_sent_at TIMESTAMPTZ,
    UNIQUE (meeting_id, invitee_id)
);
```

### Meeting Content and Structure

#### meeting_agendas
Structured meeting agendas with time allocation
```sql
CREATE TABLE meeting_agendas (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    item_order INTEGER NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    allocated_minutes INTEGER,
    presenter_id UUID, -- Reference to user_profiles.auth_user_id in User Service
    agenda_type agenda_type NOT NULL,
    quest_id UUID, -- Reference to quests in Quests Service if agenda item relates to a quest
    resources JSONB, -- Links to documents, presentations, etc.
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (meeting_id, item_order)
);
```

#### meeting_notes
Collaborative meeting notes and documentation
```sql
CREATE TABLE meeting_notes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    agenda_item_id UUID REFERENCES meeting_agendas(id) ON DELETE SET NULL,
    author_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    note_type note_type NOT NULL,
    title VARCHAR(255),
    content TEXT NOT NULL,
    is_action_item BOOLEAN NOT NULL DEFAULT FALSE,
    assigned_to UUID, -- Reference to user_profiles.auth_user_id in User Service
    due_date DATE,
    priority priority_level,
    status note_status NOT NULL DEFAULT 'Active',
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### meeting_attachments
Files and resources shared during meetings
```sql
CREATE TABLE meeting_attachments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    uploader_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    file_name VARCHAR(255) NOT NULL,
    file_path TEXT NOT NULL,
    file_size_bytes BIGINT NOT NULL,
    mime_type VARCHAR(100) NOT NULL,
    description TEXT,
    is_presentation BOOLEAN NOT NULL DEFAULT FALSE,
    uploaded_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

### Real-time Collaboration

#### meeting_chat_messages
Real-time chat messages during meetings
```sql
CREATE TABLE meeting_chat_messages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    sender_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    message_type chat_message_type NOT NULL DEFAULT 'Text',
    content TEXT NOT NULL,
    reply_to_message_id UUID REFERENCES meeting_chat_messages(id),
    is_private BOOLEAN NOT NULL DEFAULT FALSE,
    recipient_id UUID, -- For private messages
    sent_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    edited_at TIMESTAMPTZ,
    deleted_at TIMESTAMPTZ
);
```

#### meeting_polls
Interactive polls and voting during meetings
```sql
CREATE TABLE meeting_polls (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    creator_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    question TEXT NOT NULL,
    poll_type poll_type NOT NULL,
    options JSONB NOT NULL, -- Array of poll options
    allow_multiple_choices BOOLEAN NOT NULL DEFAULT FALSE,
    is_anonymous BOOLEAN NOT NULL DEFAULT FALSE,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    closed_at TIMESTAMPTZ
);
```

#### meeting_poll_responses
User responses to meeting polls
```sql
CREATE TABLE meeting_poll_responses (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    poll_id UUID NOT NULL REFERENCES meeting_polls(id) ON DELETE CASCADE,
    respondent_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    selected_options JSONB NOT NULL, -- Array of selected option indices
    response_text TEXT, -- For open-ended polls
    responded_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (poll_id, respondent_id)
);
```

### Meeting Analytics and Insights

#### meeting_analytics
Comprehensive meeting analytics and metrics
```sql
CREATE TABLE meeting_analytics (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    total_participants INTEGER NOT NULL DEFAULT 0,
    average_attendance_duration_minutes DECIMAL(10,2),
    peak_concurrent_participants INTEGER NOT NULL DEFAULT 0,
    total_chat_messages INTEGER NOT NULL DEFAULT 0,
    total_screen_shares INTEGER NOT NULL DEFAULT 0,
    total_polls_created INTEGER NOT NULL DEFAULT 0,
    engagement_score DECIMAL(5,2), -- Calculated engagement metric (0-100)
    technical_issues_count INTEGER NOT NULL DEFAULT 0,
    recording_duration_minutes INTEGER,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### participant_engagement_metrics
Individual participant engagement tracking
```sql
CREATE TABLE participant_engagement_metrics (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meetings(id) ON DELETE CASCADE,
    auth_user_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    attendance_duration_minutes INTEGER NOT NULL DEFAULT 0,
    chat_messages_sent INTEGER NOT NULL DEFAULT 0,
    polls_participated INTEGER NOT NULL DEFAULT 0,
    screen_share_duration_minutes INTEGER NOT NULL DEFAULT 0,
    microphone_active_minutes INTEGER NOT NULL DEFAULT 0,
    camera_active_minutes INTEGER NOT NULL DEFAULT 0,
    engagement_score DECIMAL(5,2), -- Individual engagement score (0-100)
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    UNIQUE (meeting_id, auth_user_id)
);
```

### Meeting Templates and Recurring Meetings

#### meeting_templates
Reusable meeting templates for common scenarios
```sql
CREATE TABLE meeting_templates (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    template_type template_type NOT NULL,
    creator_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    default_duration_minutes INTEGER NOT NULL DEFAULT 60,
    default_agenda JSONB, -- Template agenda structure
    default_settings JSONB, -- Default meeting settings
    is_public BOOLEAN NOT NULL DEFAULT FALSE,
    usage_count INTEGER NOT NULL DEFAULT 0,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### recurring_meeting_series
Manages recurring meeting series and their instances
```sql
CREATE TABLE recurring_meeting_series (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    series_name VARCHAR(255) NOT NULL,
    creator_id UUID NOT NULL, -- Reference to user_profiles.auth_user_id in User Service
    recurrence_pattern JSONB NOT NULL, -- Detailed recurrence configuration
    series_start_date DATE NOT NULL,
    series_end_date DATE,
    max_occurrences INTEGER,
    template_meeting_id UUID REFERENCES meetings(id),
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

## Enums and Types

```sql
-- Meeting types
CREATE TYPE meeting_type AS ENUM ('StudySession', 'Lecture', 'Discussion', 'Presentation', 'Workshop', 'Review', 'Social', 'Office Hours');

-- Meeting status
CREATE TYPE meeting_status AS ENUM ('Scheduled', 'InProgress', 'Completed', 'Cancelled', 'Postponed');

-- Participant roles
CREATE TYPE participant_role AS ENUM ('Host', 'CoHost', 'Presenter', 'Participant', 'Observer');

-- Participation status
CREATE TYPE participation_status AS ENUM ('Invited', 'Accepted', 'Declined', 'Tentative', 'Attended', 'NoShow');

-- Invitation status
CREATE TYPE invitation_status AS ENUM ('Pending', 'Accepted', 'Declined', 'Expired');

-- Agenda types
CREATE TYPE agenda_type AS ENUM ('Introduction', 'Presentation', 'Discussion', 'Activity', 'QuestReview', 'Assessment', 'Wrap-up');

-- Note types
CREATE TYPE note_type AS ENUM ('General', 'ActionItem', 'Decision', 'Question', 'Resource', 'Summary');

-- Note status
CREATE TYPE note_status AS ENUM ('Active', 'Completed', 'Cancelled', 'Archived');

-- Priority levels
CREATE TYPE priority_level AS ENUM ('Low', 'Medium', 'High', 'Critical');

-- Chat message types
CREATE TYPE chat_message_type AS ENUM ('Text', 'Emoji', 'File', 'Link', 'System');

-- Poll types
CREATE TYPE poll_type AS ENUM ('MultipleChoice', 'SingleChoice', 'YesNo', 'Rating', 'OpenEnded');

-- Template types
CREATE TYPE template_type AS ENUM ('StudyGroup', 'Lecture', 'Workshop', 'Review', 'Social', 'Custom');
```

## Indexes for Performance

```sql
-- Meeting indexes
CREATE INDEX idx_meetings_creator_id ON meetings(creator_id);
CREATE INDEX idx_meetings_meeting_type ON meetings(meeting_type);
CREATE INDEX idx_meetings_status ON meetings(status);
CREATE INDEX idx_meetings_scheduled_start_time ON meetings(scheduled_start_time);
CREATE INDEX idx_meetings_party_id ON meetings(party_id);
CREATE INDEX idx_meetings_guild_id ON meetings(guild_id);
CREATE INDEX idx_meetings_class_id ON meetings(class_id);
CREATE INDEX idx_meetings_subject_id ON meetings(subject_id);

-- Participant indexes
CREATE INDEX idx_meeting_participants_meeting_id ON meeting_participants(meeting_id);
CREATE INDEX idx_meeting_participants_auth_user_id ON meeting_participants(auth_user_id);
CREATE INDEX idx_meeting_participants_status ON meeting_participants(status);

-- Invitation indexes
CREATE INDEX idx_meeting_invitations_meeting_id ON meeting_invitations(meeting_id);
CREATE INDEX idx_meeting_invitations_invitee_id ON meeting_invitations(invitee_id);
CREATE INDEX idx_meeting_invitations_status ON meeting_invitations(status);

-- Content indexes
CREATE INDEX idx_meeting_agendas_meeting_id ON meeting_agendas(meeting_id);
CREATE INDEX idx_meeting_notes_meeting_id ON meeting_notes(meeting_id);
CREATE INDEX idx_meeting_notes_author_id ON meeting_notes(author_id);
CREATE INDEX idx_meeting_notes_is_action_item ON meeting_notes(is_action_item);

-- Chat and collaboration indexes
CREATE INDEX idx_meeting_chat_messages_meeting_id ON meeting_chat_messages(meeting_id);
CREATE INDEX idx_meeting_chat_messages_sender_id ON meeting_chat_messages(sender_id);
CREATE INDEX idx_meeting_chat_messages_sent_at ON meeting_chat_messages(sent_at);
CREATE INDEX idx_meeting_polls_meeting_id ON meeting_polls(meeting_id);

-- Analytics indexes
CREATE INDEX idx_meeting_analytics_meeting_id ON meeting_analytics(meeting_id);
CREATE INDEX idx_participant_engagement_metrics_meeting_id ON participant_engagement_metrics(meeting_id);
CREATE INDEX idx_participant_engagement_metrics_auth_user_id ON participant_engagement_metrics(auth_user_id);

-- Template indexes
CREATE INDEX idx_meeting_templates_creator_id ON meeting_templates(creator_id);
CREATE INDEX idx_meeting_templates_template_type ON meeting_templates(template_type);
CREATE INDEX idx_meeting_templates_is_public ON meeting_templates(is_public);
```

## Service Responsibilities

### Primary Responsibilities
- Virtual meeting lifecycle management
- Participant invitation and RSVP tracking
- Real-time collaboration features (chat, polls, screen sharing)
- Meeting content management (agendas, notes, attachments)
- Meeting analytics and engagement metrics
- Recurring meeting series management
- Meeting templates and standardization

### Cross-Service Integration
- **User Service**: Retrieves user profiles and academic context
- **Social Service**: Integrates with parties and guilds for group meetings
- **Quests Service**: Links meetings to quest-related activities and reviews
- **Code Battle Service**: Supports code review sessions and competition debriefs

### Data Access Patterns
- **Read/Write Access**: All tables within Meeting Service domain
- **External References**: User profiles, parties, guilds, classes, and subjects
- **API Integration**: Exposes meeting management via REST APIs
- **Real-time Integration**: WebSocket connections for live collaboration features

### Real-time Features
- **WebSocket Integration**: Live chat, participant status updates, and real-time polls
- **Video Conferencing**: Integration with external video platforms (Zoom, Teams, etc.)
- **Screen Sharing**: Collaborative screen sharing and presentation features
- **Live Notifications**: Meeting reminders, participant join/leave notifications

### Analytics and Reporting
- **Engagement Tracking**: Individual and group engagement metrics
- **Attendance Analytics**: Participation patterns and attendance rates
- **Content Analytics**: Note-taking patterns and resource usage
- **Performance Insights**: Meeting effectiveness and learning outcomes