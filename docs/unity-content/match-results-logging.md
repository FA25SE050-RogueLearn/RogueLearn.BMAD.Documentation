# Unity Match Results Logging Guidance

## Purpose
Provide a clear, consistent way for Unity clients and headless servers to log match summaries and question events to RogueLearn services for auditing, analytics, and academic records.

## Where to Post
- Use the Quests Service endpoints for game sessions:
  - POST /quests/game/sessions/{sessionId}/events — stream or batch QuestionEvent during the match
  - POST /quests/game/sessions/{sessionId}/complete — submit final GameSessionResult summary
- See the cross-service summary and payload details in 4.12 Match Results API (docs/4.12.match-results-api.md).

## Identity
- Include both identifiers when available:
  - playerId: Unity/NGO client ID
  - authUserId: RogueLearn platform user UUID (from JWT)

## Idempotency
- For events: Idempotency-Key: event::<sessionId>-<sequence>
- For results: Idempotency-Key: result::<sessionId>

## Content Storage Guidance
- Dynamic questions (LLM-generated): include promptText and full option text for auditability
- Static question packs: include promptHash and option textHash with questionPackId

## Example (Unity C# pseudo-code)
```csharp
public class ResultsLogger : MonoBehaviour
{
    private readonly HttpClient _http = new HttpClient();
    private string _baseUrl = Environment.GetEnvironmentVariable("QUESTS_API_BASE") ?? "https://api.roguelearn.local/quests";
    private string _apiKey = Environment.GetEnvironmentVariable("RESULTS_API_KEY");

    public async Task PostEventBatch(Guid sessionId, List<QuestionEventDto> events)
    {
        var url = $"{_baseUrl}/game/sessions/{sessionId}/events";
        var body = JsonSerializer.Serialize(new { events });

        var req = new HttpRequestMessage(HttpMethod.Post, url)
        {
            Content = new StringContent(body, Encoding.UTF8, "application/json")
        };
        if (!string.IsNullOrEmpty(_apiKey)) req.Headers.Add("x-api-key", _apiKey);
        // Use the last event sequence as idempotency key for the batch
        var lastSeq = events.Count > 0 ? events[^1].sequence : 0;
        req.Headers.Add("Idempotency-Key", $"event::{sessionId}-{lastSeq}");
        var resp = await _http.SendAsync(req);
        resp.EnsureSuccessStatusCode();
    }

    public async Task CompleteSession(Guid sessionId, GameSessionResultDto summary)
    {
        var url = $"{_baseUrl}/game/sessions/{sessionId}/complete";
        var body = JsonSerializer.Serialize(summary);
        var req = new HttpRequestMessage(HttpMethod.Post, url)
        {
            Content = new StringContent(body, Encoding.UTF8, "application/json")
        };
        if (!string.IsNullOrEmpty(_apiKey)) req.Headers.Add("x-api-key", _apiKey);
        req.Headers.Add("Idempotency-Key", $"result::{sessionId}");
        var resp = await _http.SendAsync(req);
        resp.EnsureSuccessStatusCode();
    }
}
```

## Telemetry Checklist
- Log QuestionEvent for:
  - Question served (optional, sequence assigned)
  - Answer submitted (chosenOptionId, isCorrect, latencyMs)
  - Timeout (isCorrect=false, chosenOptionId=null)
  - Power-Play window start/end (optional, for analytics)
- Summary on completion:
  - score, correctCount, incorrectCount, timeTakenSeconds
  - players[] with playerId, authUserId, displayName, joinedAt/leftAt
  - generator metadata (provider, model, promptTemplateId, packId)

## Security
- HTTPS only; prefer API key from server environment for headless
- Client-side: include Bearer JWT issued by RogueLearn during login

## Retention
- Detailed events kept 30–90 days; summaries kept long-term