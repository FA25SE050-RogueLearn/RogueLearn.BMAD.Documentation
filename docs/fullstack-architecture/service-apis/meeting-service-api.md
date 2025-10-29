# Meeting Service API Specification

This document defines the API endpoints for the **Meeting Domain** within the consolidated `RogueLearn.UserService`. This domain manages meeting scheduling, persistence, and integration with external providers like Google Meet.

```yaml
openapi: 3.0.0
info:
  title: RogueLearn Core Service API - Meeting Domain
  version: v1.0.0
  description: |
    Manages meeting scheduling, participants, and summaries. 
    This is a logical domain within the consolidated RogueLearn.UserService.
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
  /meetings/{meetingId}/start:
    post:
      summary: Start a scheduled meeting
      description: Updates the meeting status to 'InProgress'.
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
      summary: End an ongoing meeting
      description: Finalizes the meeting and may trigger external summary processing (e.g., via Google Meet API).
      tags: [Meetings]
      parameters:
        - name: meetingId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Meeting ended
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