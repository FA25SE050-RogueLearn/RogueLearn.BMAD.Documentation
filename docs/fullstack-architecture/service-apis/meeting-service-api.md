# Meeting Service API Specification

```yaml
openapi: 3.0.0
info:
  title: RogueLearn Meeting Service API
  version: v1.0.0
  description: Meeting lifecycle, participants, transcripts, and summaries.
servers:
  - url: https://api.roguelearn.com/v1
    description: Production Server

security:
  - BearerAuth: []

components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: "JWT token obtained from Supabase after login."

  schemas:
    Meeting:
      type: object
      properties:
        meeting_id: { type: string, format: uuid }
        title: { type: string }
        scheduled_start_time: { type: string, format: date-time }
        scheduled_end_time: { type: string, format: date-time }
        actual_start_time: { type: string, format: date-time, nullable: true }
        actual_end_time: { type: string, format: date-time, nullable: true }
        organizer_id: { type: string, format: uuid }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
    MeetingParticipant:
      type: object
      properties:
        participant_id: { type: string, format: uuid }
        meeting_id: { type: string, format: uuid }
        user_id: { type: string, format: uuid }
        join_time: { type: string, format: date-time, nullable: true }
        leave_time: { type: string, format: date-time, nullable: true }
        role_in_meeting: { type: string }
    TranscriptSegment:
      type: object
      properties:
        segment_id: { type: string, format: uuid }
        meeting_id: { type: string, format: uuid }
        speaker_id: { type: string, format: uuid }
        start_time: { type: string, format: date-time }
        end_time: { type: string, format: date-time }
        transcript_text: { type: string }
        chunk_number: { type: integer }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
    SummaryChunk:
      type: object
      properties:
        summary_chunk_id: { type: string, format: uuid }
        meeting_id: { type: string, format: uuid }
        chunk_number: { type: integer }
        summary_text: { type: string }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
    MeetingSummary:
      type: object
      properties:
        meeting_summary_id: { type: string, format: uuid }
        meeting_id: { type: string, format: uuid }
        summary_text: { type: string }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
    Error:
      type: object
      properties:
        code: { type: string }
        message: { type: string }

paths:
  /meetings:
    get:
      summary: List meetings for current user or party
      tags: [Meetings]
      security:
        - BearerAuth: []
      parameters:
        - in: query
          name: partyId
          schema: { type: string, format: uuid }
          description: Filter meetings for a party.
      responses:
        '200':
          description: Meetings list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Meeting'
    post:
      summary: Schedule a meeting for a party
      tags: [Meetings]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                partyId: { type: string, format: uuid }
                title: { type: string }
                scheduledStartTime: { type: string, format: date-time }
      responses:
        '201':
          description: Meeting created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Meeting'
  /meetings/{meetingId}:
    get:
      summary: Get meeting by ID
      tags: [Meetings]
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Meeting details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Meeting'
    post:
      summary: Meeting lifecycle actions
      description: Use subroutes for start and end.
      tags: [Meetings]
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '405':
          description: Use /start or /end subroutes.
  /meetings/{meetingId}/start:
    post:
      summary: Start a scheduled meeting
      tags: [Meetings]
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Meeting started
  /meetings/{meetingId}/end:
    post:
      summary: End an ongoing meeting and trigger AI summary processing
      tags: [Meetings]
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Meeting ended; summary processing triggered
  /meetings/{meetingId}/participants:
    get:
      summary: List participants
      tags: [Participants]
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Participants list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/MeetingParticipant'
  /meetings/{meetingId}/transcripts:
    get:
      summary: List transcript segments
      tags: [Transcripts]
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Transcript segments
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/TranscriptSegment'
  /meetings/{meetingId}/summary:
    get:
      summary: Get meeting summary
      tags: [Summaries]
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Meeting summary
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MeetingSummary'
```