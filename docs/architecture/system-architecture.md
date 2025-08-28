# System Architecture

## Component Diagram

```
+---------------------+      +----------------------+
|                     |      |                      |
|  Browser Extension  +----->+  Frontend (Next.js)  |
|                     |      |                      |
+---------------------+      +----------+-----------+
                                        |
                                        v
                              +---------+-----------+
                              |                     |
                              |  API Gateway        |
                              |                     |
                              +---------+-----------+
                                        |
                +---------------------+-+------------------+----------------+
                |                     |                    |                |
    +-----------v---------+  +--------v----------+  +-----v--------------+ |
    |                     |  |                   |  |                    | |
    |  User Service       |  |  Content Service  |  |  Learning Service  | |
    |  (.NET Core)        |  |  (.NET Core)      |  |  (.NET Core)       | |
    |                     |  |                   |  |                    | |
    +-----------+---------+  +--------+----------+  +-----+--------------+ |
                |                     |                    |                |
                |                     |                    |                |
    +-----------v---------+  +--------v----------+  +-----v--------------+ |
    |                     |  |                   |  |                    | |
    |  Social Service     |  |  Educator Service |  |  Marketplace      | |
    |  (.NET Core)        |  |  (.NET Core)      |  |  Service (.NET)    | |
    |                     |  |                   |  |                    | |
    +-----------+---------+  +--------+----------+  +-----+--------------+ |
                |                     |                    |                |
                |                     |                    |                |
    +-----------v---------------------v--------------------v--------------+
    |                                                                     |
    |                          Database Layer                             |
    |                                                                     |
    +---------------------------------------------------------------------+
```

## Service Descriptions

1. **Frontend (Next.js)**
   - Provides the user interface for the web application
   - Implements the gamified RPG experience
   - Renders the skill tree visualization
   - Manages client-side state and caching
   - Communicates with backend services via API

2. **Browser Extension**
   - Captures learning materials from web browsing
   - Provides quick access to the user's "Arsenal"
   - Integrates with the main application via API
   - Operates within the browser security sandbox

3. **API Gateway**
   - Routes requests to appropriate backend services
   - Handles authentication and authorization
   - Implements rate limiting and throttling
   - Provides API documentation and developer portal

4. **User Service**
   - Manages user accounts and profiles
   - Handles authentication and authorization
   - Manages user roles and permissions
   - Tracks user progress and statistics

5. **Content Service**
   - Manages learning materials and resources
   - Handles the "Arsenal" of captured content
   - Processes and categorizes learning materials
   - Provides search and recommendation capabilities

6. **Learning Service**
   - Generates personalized learning paths
   - Manages the skill tree and progression
   - Handles "Boss Fights" (exams) and quests
   - Integrates with AI for content analysis and path generation

7. **Social Service**
   - Manages party creation and membership
   - Handles party chat and collaboration features
   - Manages guild vs guild competitions
   - Tracks achievements and badges
   - Provides notification system for social interactions

8. **Educator Service**
   - Manages Guild Master toolkit functionality
   - Handles Guide (Tutor) toolkit features
   - Provides Game Master (Admin) toolkit capabilities
   - Manages course creation and student progress tracking
   - Handles custom training drills and assessments

9. **Marketplace Service**
   - Manages user-generated content marketplace
   - Handles content rating and review system
   - Processes content monetization
   - Provides AI-powered content curation
   - Manages themed knowledge packs

10. **Database Layer**
    - Stores user data, content, and learning paths
    - Provides data persistence and integrity
    - Supports complex queries for the skill tree
    - Manages social and marketplace data
    - Handles educator and administrative data