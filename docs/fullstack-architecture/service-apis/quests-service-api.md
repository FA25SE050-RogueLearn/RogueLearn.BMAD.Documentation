# Quests Service API Specification

```yaml
openapi: 3.0.0
info:
  title: RogueLearn Quests Service API
  version: v1.0.0
  description: Quests, learning paths, user progress, and curriculum packs.
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
    CurriculumPack:
      type: object
      properties:
        meta:
          type: object
          properties:
            pack_id: { type: string, format: uuid }
            version: { type: string }
            source: { type: string, enum: [Set, Seed] }
            created_at: { type: string, format: date-time }
            subject_id: { type: string, format: uuid, nullable: true }
            curriculum_id: { type: string, format: uuid, nullable: true }
            ai_model: { type: string, nullable: true }
            generator_version: { type: string, nullable: true }
            prompt_hash: { type: string, nullable: true }
            seed: { type: string, nullable: true }
            approved_by: { type: string, nullable: true }
        items:
          type: array
          items:
            type: object
            properties:
              id: { type: string }
              type: { type: string, enum: [MCQ, FreeText] }
              text: { type: string }
              options:
                type: array
                items:
                  type: object
                  properties:
                    id: { type: string }
                    text: { type: string }
              correct_answers:
                type: array
                items: { type: string }
              explanation: { type: string, nullable: true }
              difficulty: { type: string, enum: [Beginner, Intermediate, Advanced], nullable: true }
              tags:
                type: array
                items: { type: string }
              sequence: { type: integer, nullable: true }
              weight: { type: number, nullable: true }
              time_limit_sec: { type: integer, nullable: true }
              metadata: { type: object, nullable: true }
            required: [id, type, text]
      required: [meta, items]

    GameSession:
      type: object
      properties:
        id: { type: string, format: uuid }
        quest_id: { type: string, format: uuid }
        status: { type: string, enum: [InProgress, Completed, Abandoned] }
        score: { type: integer }
        started_at: { type: string, format: date-time }
        completed_at: { type: string, format: date-time, nullable: true }
    Error:
      type: object
      properties:
        code: { type: string }
        message: { type: string }

paths:
  /quests:
    get:
      summary: List quests
      tags: [Quests]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Quests list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/GameSession'
    post:
      summary: Create a quest
      tags: [Quests]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
      responses:
        '201':
          description: Quest created
  /learning-paths:
    get:
      summary: List learning paths
      tags: [LearningPaths]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Learning paths list
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
    post:
      summary: Create a learning path
      tags: [LearningPaths]
      security:
        - BearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
      responses:
        '201':
          description: Learning path created
  /curriculum-packs:
    get:
      summary: List curriculum packs
      tags: [CurriculumPacks]
      security:
        - BearerAuth: []
      responses:
        '200':
          description: Curriculum packs list
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/CurriculumPack'
```