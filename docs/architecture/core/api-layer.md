# API Layer

## API Design Principles

1. **RESTful Architecture**
   - Resource-based URLs
   - Proper use of HTTP methods
   - Consistent response formats
   - Hypermedia links for navigation

2. **GraphQL for Complex Queries**
   - Used specifically for skill tree visualization
   - Allows frontend to request exactly what it needs
   - Reduces over-fetching and under-fetching

3. **Real-time Updates with SignalR**
   - Notifications and alerts
   - Party chat and collaboration
   - Live updates to skill tree and progress

## Core API Endpoints

1. **Authentication API**
   - `/api/auth/register` - User registration
   - `/api/auth/login` - User login
   - `/api/auth/refresh` - Refresh token
   - `/api/auth/profile` - Get/update user profile

2. **Arsenal API**
   - `/api/arsenal` - Get user's arsenal items
   - `/api/arsenal/item` - Create/update/delete arsenal items
   - `/api/arsenal/search` - Search arsenal items
   - `/api/arsenal/tags` - Manage tags for arsenal items

3. **Learning API**
   - `/api/skills` - Get skill tree
   - `/api/skills/{id}` - Get/update specific skill
   - `/api/quests` - Get user's quests
   - `/api/quests/{id}` - Get/update specific quest
   - `/api/bossfights` - Get upcoming Boss Fights

4. **Social API**
   - `/api/parties` - Create/join/leave parties
   - `/api/parties/{id}` - Get/update party details
   - `/api/parties/{id}/members` - Manage party members
   - `/api/messages` - Send/receive messages

5. **Extension API**
   - `/api/extension/capture` - Capture content from web pages
   - `/api/extension/analyze` - Analyze web page for relevant skills
   - `/api/extension/sync` - Sync extension data with main application

## API Documentation

API documentation will be generated using Swagger/OpenAPI and will be available through Azure API Management's developer portal. This will include:

- Endpoint descriptions and examples
- Request and response schemas
- Authentication requirements
- Rate limiting information
- SDK generation options