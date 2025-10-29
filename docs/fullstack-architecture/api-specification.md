# **API Specification**

This document provides an overview of RogueLearn's API structure, which is divided between a main **Consolidated Core API** (`RogueLearn.UserService`) and a specialized API for the Event Service.

## **Common Conventions**
- API standard: OpenAPI `3.0.0` across all service specs.
- Authentication: All endpoints assume `BearerAuth` with a JWT from Supabase unless explicitly noted otherwise.
- Error model: `Error { code, message }` returned on failure.
- IDs: All resource identifiers are UUIDs.
- Versioning: External base URL versioning (e.g., `https://api.roguelearn.com/v1`).

## **Consolidated Core Service API (`RogueLearn.UserService`)**
Spec: `./service-apis/user-service-api.md` (This file will now contain all core domains)

The **`RogueLearn.UserService`** provides a unified API for all primary application functionality, organized by logical domain.

#### **User & Academic Domain (`/api/users`, `/api/curriculums`)**
- Manages user profiles, roles, academic programs, and enrollments.
- **Representative APIs:** `GET /profiles/me`, `POST /enrollments`, `POST /verification/requests`

#### **Quest & Gamification Domain (`/api/quests`, `/api/learning-paths`)**
- Manages quests, learning paths, game sessions, skills, and progress.
- **Representative APIs:** `GET /learning-paths/me`, `GET /quests/{questId}`, `POST /game/sessions/{sessionId}/complete`

#### **Social Domain (`/api/parties`, `/api/guilds`, `/api/messages`)**
- Manages parties, guilds, social events, messaging, and shared resources.
- **Representative APIs:** `POST /parties`, `GET /guilds/{guildId}`, `POST /messages`

#### **Meeting Domain (`/api/meetings`)**
- Manages meeting scheduling, persistence, and integration with external providers (e.g., Google Meet).
- **Representative APIs:** `POST /meetings`, `GET /meetings/{meetingId}/summary`

#### **Notes/Arsenal Domain (`/api/notes`)**
- Manages creation, update, and management of user-generated content.
- **Representative APIs:** `GET /notes`, `POST /notes/{noteId}/link-quests`

---

### **Event Service API (Specialized)**
Spec: `./service-apis/event-service-api.md`

The **Event Service** maintains its specialized API for competitive programming.
- Manages problems, test cases, rooms, submissions, leaderboards, and duels.
- **Representative APIs:** `GET /problems`, `POST /submissions`, `GET /leaderboards/events/{eventId}`

---

## **Next Steps**
- For detailed request/response formats, refer to each service's OpenAPI specification file.
- The individual specifications for the former `quests-service-api.md`, `social-service-api.md`, and `meeting-service-api.md` should be merged into the single, consolidated `user-service-api.md` to reflect the new architecture.