erDiagram
    meeting {
        uuid meeting_id PK
        string title
        timestamp scheduled_start_time
        timestamp scheduled_end_time
        timestamp actual_start_time
        timestamp actual_end_time
        uuid organizer_id
        timestamp created_at
        timestamp updated_at
    }

    meeting_participant  {
        uuid participant_id PK
        uuid meeting_id FK
        uuid user_id
        timestamp join_time
        timestamp leave_time
        string role_in_meeting
    } 

    transcript_segment {
        uuid segment_id PK
        uuid meeting_id FK
        uuid speaker_id FK 
        timestamp start_time
        timestamp end_time
        text transcript_text
        int chunk_number
        timestamp created_at
        timestamp updated_at
    }

    summary_chunk {
        uuid summary_chunk_id PK
        uuid meeting_id FK
        int chunk_number
        text summary_text
        timestamp created_at
        timestamp updated_at
    } 

    meeting_summary {
        uuid meeting_summary_id PK
        uuid meeting_id FK
        text summary_text
        timestamp created_at
        timestamp updated_at
    }

    meeting ||--o{ meeting_participant : has
    meeting_participant || -- o{ transcript_segment: produces 
    meeting ||--o{ transcript_segment : contains
    meeting ||--o{ summary_chunk : summarized_by
    meeting ||--o{ meeting_summary : summarized_by
