# 4.12 Match Results API

## Overview
Server posts match results and question events for auditing generated content and player performance.

## Endpoints
- POST /api/matches/results
  - Body: Match summary with optional `question_events` array (see schema).
  - Headers:
    - `x-api-key: <RESULTS_API_KEY>` (server-to-server) or `Authorization: Bearer <JWT>` (client auth)
    - `Idempotency-Key: result::<match_id>` (recommended)
  - Responses:
    - 201 Created: `{ "match_id": "..." }`
    - 409 Conflict: duplicate idempotency key.

- (Optional) POST /api/matches/events
  - Body: A batch of question events (streaming during the match).
  - Headers:
    - `x-api-key: <RESULTS_API_KEY>` or `Authorization: Bearer <JWT>`
    - `Idempotency-Key: event::<match_id>-<sequence>` (recommended per event or per batch)
  - Responses:
    - 202 Accepted: batch queued
    - 409 Conflict: duplicate event idempotency key

## Schemas
- MatchResult:
  - `match_id` (string, uuid)
  - `relay_join_code` (string)
  - `region` (string)
  - `started_at`, `ended_at` (ISO8601)
  - `generator` (GeneratorMetadata)
  - `players` (array of PlayerSummary)
  - `question_events` (array of QuestionEvent)
  - `questions_total`, `correct_total` (int)

- PlayerSummary:
  - `player_id` (string) — Unity/NGO player identifier
  - `auth_user_id` (string, uuid) — platform user ID, if known
  - `display_name` (string?)
  - `joined_at`, `left_at` (ISO8601)
  - `score` (int)
  - `accuracy` (float)

- QuestionEvent:
  - `sequence` (int)
  - `player_id` (string)
  - `auth_user_id` (string, uuid)
  - `question_id` (string)  e.g., `pack:item` or generator ID
  - `question_pack_id` (string?)
  - `generator_seed` (string?)
  - `prompt_text` (string?) or `prompt_hash` (string)
  - `options` (array of `{id, text? | text_hash}`)
  - `correct_option_id` (string)
  - `chosen_option_id` (string)
  - `is_correct` (bool)
  - `answered_at` (ISO8601)
  - `latency_ms` (int)

- GeneratorMetadata:
  - `provider` (string, enum: [Gemini, OpenAI, Claude])
  - `model` (string)
  - `prompt_template_id` (string?)
  - `pack_id` (string?)
  - `version` (string?)

## Security
- HTTPS only.
- Server-to-server auth via API key; clients via JWT.
- Validate `match_id` uniqueness; enforce idempotency.
- Limit payload size for `question_events` (batch up to N=100 by default).
- PII minimization: prefer `auth_user_id` and `display_name`; avoid storing emails.

## Notes
- If question content is dynamic, prefer storing `prompt_text` and all `options.text` to audit correctness; otherwise store hashes + pack refs.
- Consider data retention policy and PII minimization.
 - Recommended retention: keep full `question_events` 30–90 days; retain summary `MatchResult` indefinitely for academic records.
 - Idempotency: if a retry occurs, reuse the same `Idempotency-Key` per result or event `sequence`.

## Example Payloads

### MatchResult summary with embedded question events
{
  "match_id": "b7b59d2e-3f2c-4f6a-9a3f-6e5f5c4d2a10",
  "relay_join_code": "ABCD1234",
  "region": "us-central",
  "started_at": "2025-02-10T15:02:33Z",
  "ended_at": "2025-02-10T15:08:41Z",
  "generator": {
    "provider": "Gemini",
    "model": "gemini-1.5-pro",
    "prompt_template_id": "math_mcq_v2",
    "pack_id": "pack-88f1",
    "version": "2025-01"
  },
  "players": [
    {
      "player_id": "p-101",
      "auth_user_id": "8aa40a58-ef31-4b43-9a3a-bf84f4a4f3c6",
      "display_name": "Alex",
      "joined_at": "2025-02-10T15:02:40Z",
      "left_at": "2025-02-10T15:08:41Z",
      "score": 1200,
      "accuracy": 0.85
    }
  ],
  "questions_total": 12,
  "correct_total": 10,
  "question_events": [
    {
      "sequence": 1,
      "player_id": "p-101",
      "auth_user_id": "8aa40a58-ef31-4b43-9a3a-bf84f4a4f3c6",
      "question_id": "pack-88f1:q-1",
      "question_pack_id": "pack-88f1",
      "generator_seed": "seed-9012",
      "prompt_hash": "sha256:4a21...",
      "options": [
        { "id": "A", "text_hash": "sha256:a1" },
        { "id": "B", "text_hash": "sha256:b2" },
        { "id": "C", "text_hash": "sha256:c3" },
        { "id": "D", "text_hash": "sha256:d4" }
      ],
      "correct_option_id": "C",
      "chosen_option_id": "C",
      "is_correct": true,
      "answered_at": "2025-02-10T15:03:01Z",
      "latency_ms": 2100
    }
  ]
}

### Streaming event batch
{
  "events": [
    {
      "sequence": 4,
      "player_id": "p-101",
      "auth_user_id": "8aa40a58-ef31-4b43-9a3a-bf84f4a4f3c6",
      "question_id": "pack-88f1:q-4",
      "chosen_option_id": "B",
      "is_correct": false,
      "answered_at": "2025-02-10T15:05:44Z",
      "latency_ms": 3400
    }
  ]
}
