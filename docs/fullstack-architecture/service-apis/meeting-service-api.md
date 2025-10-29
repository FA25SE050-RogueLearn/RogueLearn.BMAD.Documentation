# Meeting Service API Specification

This document defines the API for the specialized, real-time **Go Meeting Service**. This service is responsible for managing the live aspects of a meeting. Note that all data persistence (creating and storing meetings, participants, transcripts, etc.) is handled by the consolidated `RogueLearn.UserService`.

```yaml
openapi: 3.0.0
info:
  title: RogueLearn Meeting Service API
  version: v1.0.0
  description: |
    Manages the real-time aspects of meetings (e.g., WebSocket communication for live sessions). 
    This service is stateless and calls the main RogueLearn.UserService for all data persistence.
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
      description: This endpoint delegates to the RogueLearn.UserService to retrieve meeting data.
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
      description: This endpoint delegates to the RogueLearn.UserService to create and persist the meeting.
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
      description: This endpoint delegates to the RogueLearn.UserService to retrieve meeting data.
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
      description: This endpoint triggers the real-time logic and delegates to the RogueLearn.UserService to update the meeting status.
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
      description: This endpoint stops the real-time logic and delegates to the RogueLearn.UserService to finalize the meeting and trigger summarization.
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
      description: This endpoint delegates to the RogueLearn.UserService to retrieve participant data.
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
      description: This endpoint delegates to the RogueLearn.UserService to retrieve transcript data.
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
      description: This endpoint delegates to the RogueLearn.UserService to retrieve summary data.
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
