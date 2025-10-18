# Meeting Service Database Schema

## Overview
The Meeting Service manages virtual meetings with core functionality for meeting lifecycle, participant tracking, transcript management, and summary generation. This schema focuses on essential meeting operations and AI-powered content processing.

## Database Tables

### Core Meeting Management

#### meeting
Core meeting definitions and scheduling.
```sql
CREATE TABLE meeting (
    meeting_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    scheduled_start_time TIMESTAMPTZ NOT NULL,
    scheduled_end_time TIMESTAMPTZ NOT NULL,
    actual_start_time TIMESTAMPTZ,
    actual_end_time TIMESTAMPTZ,
    organizer_id UUID NOT NULL, -- Soft FK to user_profiles.auth_user_id in User Service
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### meeting_participant
Meeting participation tracking with join/leave times and roles.
```sql
CREATE TABLE meeting_participant (
    participant_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meeting(meeting_id) ON DELETE CASCADE,
    user_id UUID NOT NULL, -- Soft FK to user_profiles.auth_user_id in User Service
    join_time TIMESTAMPTZ,
    leave_time TIMESTAMPTZ,
    role_in_meeting VARCHAR(50) NOT NULL DEFAULT 'participant'
);
```

### AI-Powered Content Processing (Future Implementation)

#### transcript_segment
Meeting transcript segments with speaker identification and timing.
```sql
CREATE TABLE transcript_segment (
    segment_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meeting(meeting_id) ON DELETE CASCADE,
    speaker_id UUID NOT NULL REFERENCES meeting_participant(participant_id) ON DELETE CASCADE,
    start_time TIMESTAMPTZ NOT NULL,
    end_time TIMESTAMPTZ NOT NULL,
    transcript_text TEXT NOT NULL,
    chunk_number INTEGER NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### summary_chunk
AI-generated summary chunks for meeting segments.
```sql
CREATE TABLE summary_chunk (
    summary_chunk_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meeting(meeting_id) ON DELETE CASCADE,
    chunk_number INTEGER NOT NULL,
    summary_text TEXT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

#### meeting_summary
Complete AI-generated meeting summaries.
```sql
CREATE TABLE meeting_summary (
    meeting_summary_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    meeting_id UUID NOT NULL REFERENCES meeting(meeting_id) ON DELETE CASCADE,
    summary_text TEXT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

## Indexes for Performance

```sql
-- Meeting indexes
CREATE INDEX idx_meeting_organizer_id ON meeting(organizer_id);
CREATE INDEX idx_meeting_scheduled_start_time ON meeting(scheduled_start_time);

-- Participant indexes
CREATE INDEX idx_meeting_participant_meeting_id ON meeting_participant(meeting_id);
CREATE INDEX idx_meeting_participant_user_id ON meeting_participant(user_id);

-- Transcript indexes
CREATE INDEX idx_transcript_segment_meeting_id ON transcript_segment(meeting_id);
CREATE INDEX idx_transcript_segment_speaker_id ON transcript_segment(speaker_id);
CREATE INDEX idx_transcript_segment_chunk_number ON transcript_segment(chunk_number);

-- Summary indexes
CREATE INDEX idx_summary_chunk_meeting_id ON summary_chunk(meeting_id);
CREATE INDEX idx_summary_chunk_chunk_number ON summary_chunk(chunk_number);
CREATE INDEX idx_meeting_summary_meeting_id ON meeting_summary(meeting_id);
```

## Service Responsibilities

### Primary Responsibilities
- Meeting lifecycle management (scheduling, starting, ending)
- Participant tracking and role management
- Real-time transcript capture and processing (Future)
- AI-powered content summarization (Future)
- Meeting content storage and retrieval

### Cross-Service Integration
- **User Service**: Retrieves user profiles for organizers and participants.
- **AI Service**: Will process transcripts for summarization and insights.
- **Notification Service**: Will send meeting reminders and updates.
- **Social Service**: Consumes context from Parties and Guilds to associate meetings with them.