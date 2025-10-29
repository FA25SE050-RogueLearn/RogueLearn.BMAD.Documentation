### **Story 4.3: Backend - Implement Guild Event Registration & Leaderboard API**
-   **Status:** To Do
-   **Ownership:** Minh Anh (Backend), with support from Phuc
-   **Target Deadline:** End of Weekend

**As a** Guild Master,
**I want** to register my guild for an approved event via an API in the **Event Service**, and **as a developer**, I need a way to fetch basic leaderboard results from that same service.

#### **Acceptance Criteria**
1.  An endpoint `POST /api/events/{eventId}/register-guild` is created in the **Event Service**. It accepts a `guildId` in the request body.
2.  The registration endpoint performs a **secure, internal service-to-service call** to the `SocialService` (within `UserService`) to validate that the authenticated user is the `GuildMaster` of the provided `guildId`.
3.  A `GET /api/events/{eventId}/leaderboard` endpoint is created in the **Event Service** that returns a simple, non-real-time list of top scores for a completed event.
4.  The registration endpoint is protected, requiring a standard authenticated user JWT. Authorization is handled within the service by checking the user's role in the specified guild.
5.  The leaderboard endpoint is public or requires standard authentication.

#### **Dev Notes**
*   **Architectural Guidance:** This story requires cross-service communication. The `EventService` will need a client to call the `SocialService`. The `SocialService` will need to expose a new, **internal-only** endpoint to validate a user's role within a guild.
*   **Collaboration:** Coordinate with Phuc to define the contract for the internal guild validation endpoint that will be added to the `UserService`'s social domain.
