# Social Service API Specification

This document defines the API endpoints for the **Social Domain** within the consolidated `RogueLearn.UserService`. This domain manages parties, guilds, messaging, social events, and shared resources.

```yaml
openapi: 3.0.0
info:
  title: RogueLearn Core Service API - Social Domain
  version: v1.0.0
  description: Manages parties, guilds, messaging, social events, and shared resources. Part of the consolidated RogueLearn.UserService.
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
    Party:
      type: object
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        description: { type: string, nullable: true }
        party_type: { type: string, enum: [Study, Project, Competition] }
        max_members: { type: integer }
        current_member_count: { type: integer }
        is_public: { type: boolean }
        subject_id: { type: string, format: uuid, nullable: true }
        class_id: { type: string, format: uuid, nullable: true }
        created_by: { type: string, format: uuid }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
        disbanded_at: { type: string, format: date-time, nullable: true }
    PartyMember:
      type: object
      properties:
        id: { type: string, format: uuid }
        party_id: { type: string, format: uuid }
        auth_user_id: { type: string, format: uuid }
        role: { type: string, enum: [Leader, Officer, Member] }
        status: { type: string, enum: [Active, Banned, Left] }
        joined_at: { type: string, format: date-time }
        left_at: { type: string, format: date-time, nullable: true }
        contribution_score: { type: integer }
    PartyInvitation:
      type: object
      properties:
        id: { type: string, format: uuid }
        party_id: { type: string, format: uuid }
        inviter_id: { type: string, format: uuid }
        invitee_id: { type: string, format: uuid }
        status: { type: string, enum: [Pending, Accepted, Declined, Revoked, Expired] }
        message: { type: string, nullable: true }
        invited_at: { type: string, format: date-time }
        responded_at: { type: string, format: date-time, nullable: true }
        expires_at: { type: string, format: date-time }
    Guild:
      type: object
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        description: { type: string, nullable: true }
        guild_type: { type: string, enum: [Academic, Competitive, Social] }
        max_members: { type: integer }
        current_member_count: { type: integer }
        is_public: { type: boolean }
        created_by: { type: string, format: uuid }
        created_at: { type: string, format: date-time }
        updated_at: { type: string, format: date-time }
        disbanded_at: { type: string, format: date-time, nullable: true }
    GuildMember:
      type: object
      properties:
        id: { type: string, format: uuid }
        guild_id: { type: string, format: uuid }
        auth_user_id: { type: string, format: uuid }
        role: { type: string, enum: [Leader, Officer, Member] }
        status: { type: string, enum: [Active, Banned, Left] }
        joined_at: { type: string, format: date-time }
        left_at: { type: string, format: date-time, nullable: true }
        contribution_points: { type: integer }
        rank_within_guild: { type: integer, nullable: true }
    GuildInvitation:
      type: object
      properties:
        id: { type: string, format: uuid }
        guild_id: { type: string, format: uuid }
        inviter_id: { type: string, format: uuid, nullable: true }
        invitee_id: { type: string, format: uuid }
        invitation_type: { type: string, enum: [Invite, Application] }
        status: { type: string, enum: [Pending, Accepted, Declined, Revoked, Expired] }
        message: { type: string, nullable: true }
        created_at: { type: string, format: date-time }
        responded_at: { type: string, format: date-time, nullable: true }
        expires_at: { type: string, format: date-time }
    GuildAchievement:
      type: object
      properties:
        id: { type: string, format: uuid }
        guild_id: { type: string, format: uuid }
        achievement_name: { type: string }
        description: { type: string, nullable: true }
        achieved_at: { type: string, format: date-time }
    SocialEvent:
      type: object
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        description: { type: string, nullable: true }
        event_type: { type: string, enum: [Guild, Party, Global] }
        organizer_guild_id: { type: string, format: uuid, nullable: true }
        starts_at: { type: string, format: date-time }
        ends_at: { type: string, format: date-time }
        is_registration_open: { type: boolean }
    SocialMessage:
      type: object
      properties:
        id: { type: string, format: uuid }
        sender_id: { type: string, format: uuid }
        recipient_id: { type: string, format: uuid, nullable: true }
        party_id: { type: string, format: uuid, nullable: true }
        guild_id: { type: string, format: uuid, nullable: true }
        content: { type: string }
        created_at: { type: string, format: date-time }
        is_edited: { type: boolean }
        edited_at: { type: string, format: date-time, nullable: true }
        is_deleted: { type: boolean }
        deleted_at: { type: string, format: date-time, nullable: true }
    SocialMessageCreate:
      type: object
      required: [content]
      properties:
        recipient_id: { type: string, format: uuid, nullable: true }
        party_id: { type: string, format: uuid, nullable: true }
        guild_id: { type: string, format: uuid, nullable: true }
        content: { type: string }
    MessageReaction:
      type: object
      properties:
        id: { type: string, format: uuid }
        message_id: { type: string, format: uuid }
        reactor_id: { type: string, format: uuid }
        reaction_type: { type: string }
        reacted_at: { type: string, format: date-time }
    PartyStashItem:
      type: object
      properties:
        id: { type: string, format: uuid }
        party_id: { type: string, format: uuid }
        item_type: { type: string, enum: [Link, File, Note, Resource] }
        title: { type: string }
        description: { type: string, nullable: true }
        url: { type: string, nullable: true }
        created_by: { type: string, format: uuid }
        created_at: { type: string, format: date-time }
    GuildAnnouncement:
      type: object
      properties:
        id: { type: string, format: uuid }
        guild_id: { type: string, format: uuid }
        title: { type: string }
        content: { type: string }
        created_by: { type: string, format: uuid }
        created_at: { type: string, format: date-time }
    UserSocialStats:
      type: object
      properties:
        id: { type: string, format: uuid }
        auth_user_id: { type: string, format: uuid }
        friends_count: { type: integer }
        parties_joined: { type: integer }
        parties_created: { type: integer }
        guilds_joined: { type: integer }
        guilds_created: { type: integer }
        total_collaboration_hours: { type: number }
        social_reputation_score: { type: integer }
        last_updated_at: { type: string, format: date-time }
    Error:
      type: object
      properties:
        code: { type: string }
        message: { type: string }

paths:
  /parties:
    get:
      summary: List parties
      tags: [Parties]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Parties list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Party'
    post:
      summary: Create a party
      tags: [Parties]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Party'
      responses:
        '201':
          description: Party created
  /parties/{partyId}:
    get:
      summary: Get party details
      tags: [Parties]
      security:
        - BearerAuth: []
      parameters:
        - name: partyId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Party details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Party'
  /parties/{partyId}/invitations:
    post:
      summary: Invite a user to party
      tags: [Parties]
      security:
        - BearerAuth: []
      parameters:
        - name: partyId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                inviteeId: { type: string, format: uuid }
                message: { type: string }
      responses:
        '201':
          description: Invitation created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PartyInvitation'
  /parties/{partyId}/leave:
    post:
      summary: Leave party
      tags: [Parties]
      security:
        - BearerAuth: []
      parameters:
        - name: partyId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '204':
          description: Left party
  /parties/{partyId}/members/{userId}:
    delete:
      summary: Remove member from party
      description: Leader only or Officer where permitted. RBAC enforced via party roles.
      tags: [Parties]
      security:
        - BearerAuth: []
      parameters:
        - name: partyId
          in: path
          required: true
          schema: { type: string, format: uuid }
        - name: userId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '204':
          description: Member removed
  /parties/{partyId}/stash:
    get:
      summary: Get party stash
      tags: [Parties]
      security:
        - BearerAuth: []
      parameters:
        - name: partyId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Shared resources in party
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/PartyStashItem'
  /guilds:
    get:
      summary: List guilds
      tags: [Guilds]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Guilds list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Guild'
    post:
      summary: Create a guild
      tags: [Guilds]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Guild'
      responses:
        '201':
          description: Guild created
  /guilds/{guildId}:
    get:
      summary: Get guild details
      tags: [Guilds]
      security:
        - BearerAuth: []
      parameters:
        - name: guildId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Guild details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Guild'
  /guilds/{guildId}/join:
    post:
      summary: Request to join guild
      tags: [Guilds]
      security:
        - BearerAuth: []
      parameters:
        - name: guildId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '201':
          description: Join request created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/GuildInvitation'
  /guilds/{guildId}/members:
    get:
      summary: List guild members
      tags: [Guilds]
      security:
        - BearerAuth: []
      parameters:
        - name: guildId
          in: path
          required: true
          schema: { type: string, format: uuid }
      responses:
        '200':
          description: Guild members
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/GuildMember'
      description: Guild Master or Officers only for full roster views. RBAC enforced.
  /guilds/{guildId}/announcements:
    post:
      summary: Create guild announcement
      tags: [Guilds]
      security:
        - BearerAuth: []
      parameters:
        - name: guildId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/GuildAnnouncement'
      responses:
        '201':
          description: Announcement created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/GuildAnnouncement'
      description: Guild Master only. RBAC enforced via guild roles.
  /messages:
    get:
      summary: List messages
      description: Retrieve messages for DM, party chat, or guild chat.
      tags: [Messages]
      security:
        - BearerAuth: []
      parameters:
        - in: query
          name: userId
          schema: { type: string, format: uuid }
          description: Direct messages with a user.
        - in: query
          name: partyId
          schema: { type: string, format: uuid }
          description: Messages within a party.
        - in: query
          name: guildId
          schema: { type: string, format: uuid }
          description: Messages within a guild.
      responses:
        '200':
          description: Messages list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/SocialMessage'
    post:
      summary: Send a message
      description: Sends a message to a user, party, or guild.
      tags: [Messages]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/SocialMessageCreate'
      responses:
        '201':
          description: Message sent
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/SocialMessage'
  /events:
    get:
      summary: List social or competitive events
      description: Events created by Guild Masters or Admins.
      tags: [Events]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Events list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/SocialEvent'
  /events/{eventId}/register:
    post:
      summary: Register for an event
      description: Registers the current user or their guild for an event.
      tags: [Events]
      security:
        - BearerAuth: []
      parameters:
        - name: eventId
          in: path
          required: true
          schema: { type: string, format: uuid }
      requestBody:
        required: false
      responses:
        '201':
          description: Registration successful
```
