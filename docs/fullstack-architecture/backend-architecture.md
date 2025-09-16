# **Backend Architecture**

### **Service Architecture**

*   **Pattern:** **Clean Architecture** will be strictly followed in all .NET services.
*   **Function Organization:** Services will be deployed as standard ASP.NET Core Web API projects to Azure Container Apps.

### **Microservices Overview**

The backend consists of the following microservices:

#### **Core .NET Services**
*   **User Service** (`roguelearn-user-service`) - Manages user profiles, preferences, and user-related operations
*   **Quests Service** (`roguelearn-quests-service`) - Handles syllabi, quests, skill trees, and game session logic
*   **Social Service** (`roguelearn-social-service`) - Manages parties, guilds, events, and real-time duels
*   **AI Proxy Service** (`roguelearn-ai-proxy-service`) - Secure gateway for Gemini API communications

#### **Specialized Go Services**
*   **Code Battle Service** (`roguelearn-code-battle-service`) - Compiles, executes, and scores user-submitted code in secure sandboxes
*   **Meeting Service** (`roguelearn-meeting-service`) - Handles party meetings, scheduling, and collaboration features

### **Service Communication Patterns**

#### **Synchronous Communication**
*   **API Gateway Pattern:** All external requests route through Azure API Management
*   **Service-to-Service:** Direct HTTP calls for immediate data requirements
*   **Authentication:** JWT tokens validated at gateway level, propagated to services

#### **Asynchronous Communication**
*   **Real-time Features:** SignalR hubs for live updates (duels, meetings, code battles)
*   **Event-Driven:** Database triggers for user synchronization
*   **Background Processing:** Async operations for AI processing and code evaluation

### **Meeting Service Architecture**

The Meeting Service handles all collaboration features using Go:

```go
// Meeting Service Domain Models
type Meeting struct {
    ID                 uuid.UUID `json:"id"`
    PartyID           uuid.UUID `json:"partyId"`
    Title             string    `json:"title"`
    Status            string    `json:"status"`
    ScheduledStartTime time.Time `json:"scheduledStartTime"`
    Participants      []MeetingParticipant `json:"participants"`
    AgendaItems       []MeetingAgenda `json:"agendaItems"`
}

// Meeting Service Repository Pattern
type MeetingRepository interface {
    GetByID(ctx context.Context, id uuid.UUID) (*Meeting, error)
    GetByPartyID(ctx context.Context, partyId uuid.UUID) ([]*Meeting, error)
    Create(ctx context.Context, meeting *Meeting) error
    Update(ctx context.Context, meeting *Meeting) error
}
```

### **Code Battle Service Architecture**

The Code Battle Service uses Go for performance-critical code execution:

```go
// Code Battle Service Core Types
type Submission struct {
    ID           uuid.UUID `json:"id"`
    EventID      uuid.UUID `json:"eventId"`
    ProblemID    uuid.UUID `json:"problemId"`
    UserID       uuid.UUID `json:"userId"`
    LanguageID   uuid.UUID `json:"languageId"`
    SourceCode   string    `json:"sourceCode"`
    Status       string    `json:"status"`
    Score        *int      `json:"score,omitempty"`
    ExecutionTime *int     `json:"executionTime,omitempty"`
}

// Judge Service Interface
type JudgeService interface {
    EvaluateSubmission(ctx context.Context, submission *Submission) (*EvaluationResult, error)
    GetSupportedLanguages() []Language
}
```

#### **Code Execution Security**
*   **Sandboxing:** Docker containers with resource limits
*   **Time Limits:** Configurable execution timeouts
*   **Memory Limits:** Restricted memory allocation per submission
*   **Network Isolation:** No external network access during execution

### **Shared Libraries and Common Code**

*   **Repository:** **`roguelearn-buildingblocks`** - A shared repository containing common libraries, utilities, and code used across all backend microservices.
*   **Purpose:** Promotes code reuse, consistency, and maintainability across services.
*   **Contents:**
    *   Common domain models and DTOs
    *   Shared authentication and authorization utilities
    *   Database connection and configuration helpers
    *   Logging and monitoring abstractions
    *   Common validation attributes and extensions
    *   Shared exception handling middleware
    *   API response standardization utilities
    *   Real-time communication abstractions (SignalR)
    *   Meeting and code battle shared models

### **Database Architecture**

*   **Data Access:** **Entity Framework Core (EF Core)** will be used as the ORM for .NET services.
*   **Data Access Layer:** The **Repository Pattern** will be used to abstract all data access logic.
*   **Go Services:** Use `database/sql` with PostgreSQL drivers for direct database access.

```csharp
// Example: Enhanced Repository Pattern
public interface IQuestRepository
{
    Task<Quest?> GetByIdAsync(Guid id);
    Task<IEnumerable<Quest>> GetByQuestLineIdAsync(Guid questLineId);
    Task AddAsync(Quest quest);
    Task UpdateAsync(Quest quest);
    Task DeleteAsync(Guid id);
}
```

### **Real-time Communication Architecture**

#### **SignalR Hubs**
*   **Social Hub:** Handles duels, party notifications, guild events
*   **Code Battle Hub:** Provides live updates during coding competitions

#### **WebSocket Connections (Go Services)**
*   **Meeting Hub:** Manages real-time meeting collaboration via WebSocket
*   **Code Battle Hub:** Provides live updates during coding competitions via WebSocket

```csharp
// Social Hub Example (SignalR for .NET services)
public class SocialHub : Hub
{
    public async Task JoinPartyRoom(string partyId)
    {
        await Groups.AddToGroupAsync(Context.ConnectionId, $"party-{partyId}");
    }
}
```

```go
// Meeting Hub Example (WebSocket for Go services)
type MeetingHub struct {
    connections map[string]*websocket.Conn
    rooms       map[string][]string
}

func (h *MeetingHub) JoinMeetingRoom(conn *websocket.Conn, meetingId string) {
    h.rooms[meetingId] = append(h.rooms[meetingId], conn.RemoteAddr().String())
}
```

### **Authentication and Authorization**

*   **Authentication:** The API Gateway will validate JWTs issued by **Supabase**.
*   **Authorization:** Each microservice will implement its own authorization logic using .NET's `[Authorize]` attributes and custom policies.
*   **Service-to-Service:** Internal services use service accounts and API keys for secure communication.

### **Deployment and Scaling**

#### **Container Strategy**
*   **Platform:** Azure Container Apps for auto-scaling and serverless deployment
*   **Images:** Multi-stage Docker builds for optimized container sizes
*   **Configuration:** Environment-based configuration with Azure Key Vault integration

#### **Scaling Patterns**
*   **Horizontal Scaling:** Auto-scaling based on CPU/memory metrics
*   **Database Scaling:** Read replicas for query-heavy operations
*   **Caching:** Redis for session state and frequently accessed data

