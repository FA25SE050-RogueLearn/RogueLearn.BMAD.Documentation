# Social Service API Specification

```yaml
openapi: 3.0.0
info:
  title: RogueLearn Social Service API
  version: v1.0.0
  description: Parties, guilds, messaging, and social stats.
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
    MessageReaction:
      type: object
      properties:
        id: { type: string, format: uuid }
        message_id: { type: string, format: uuid }
        reactor_id: { type: string, format: uuid }
        reaction_type: { type: string }
        reacted_at: { type: string, format: date-time }
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
  /messages:
    get:
      summary: List messages
      tags: [Messages]
      security:
        - BearerAuth: []
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
      tags: [Messages]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/SocialMessage'
      responses:
        '201':
          description: Message sent
```