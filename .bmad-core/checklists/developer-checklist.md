# Developer Endpoint & Application Layer Checklist

Use this checklist when adding or modifying API endpoints and their corresponding feature handlers. It encodes the project rules for endpoints, mapping, and the Application layer boundaries.

## Per Endpoint

- [ ] Comment the endpoint with a clear summary of what it does (purpose, inputs, outputs, side-effects). Prefer XML doc comments or Swagger annotations so it appears in API docs.
- [ ] If admin-authenticated only:
  - [ ] Apply the `[AdminOnly]` attribute
  - [ ] Prefix the route with `/admin` (e.g., `api/admin/...`)
- [ ] If the endpoint needs the authenticated user id, use `ClaimsPrincipalExtensions.GetAuthUserId(User)` (do not parse claims manually).
- [ ] Define explicit response types for all outcomes (success and expected errors):
  - [ ] Use `[ProducesResponseType(typeof(MyResponseDto), StatusCodes.Status200OK)]` (and other status codes as needed)
  - [ ] Return strongly-typed DTOs and keep response shape consistent
- [ ] Keep endpoint thin: no business logic in the endpoint. Delegate to the corresponding feature handler.
- [ ] Use mapping profiles for input/output conversions:
  - [ ] Map request DTOs to commands/queries
  - [ ] Map domain/entities to response DTOs
- [ ] Apply consistent error handling (e.g., ProblemDetails) and logging per standards.
- [ ] Ensure naming and routing conventions are consistent with the rest of the API.

## Feature Handlers (Business Logic Lives Here)

- [ ] Implement all business and orchestration logic in the feature handler corresponding to the endpoint.
- [ ] Accept well-defined inputs (command/query DTO) and return a well-defined result/DTO.
- [ ] Use `CancellationToken` and propagate it to downstream calls.
- [ ] Validate inputs (and any invariants) within the handler.
- [ ] Use mapping profiles for conversions to/from domain models as needed.
- [ ] Keep handlers cohesive and unit-testable.

## Mapping Profiles (Mapper)

- [ ] Add/update AutoMapper profiles (or equivalent) for all new DTOs and responses.
- [ ] Centralize mapping rules in profiles; avoid ad-hoc mapping in endpoints/handlers.
- [ ] Handle nulls, collections, and nested objects explicitly in mappings.
- [ ] Validate mappings (unit tests or runtime assertions where appropriate).
- [ ] Keep DTOs and domain models decoupled.

## Application Layer: Services & Interfaces

- [ ] Restrict `Services` and `Interfaces` (in the Application layer) to 3rd-party integrations only (e.g., Supabase Storage, AI services, external providers).
- [ ] Define interfaces in `Application` for external services; implement them in `Infrastructure`.
- [ ] Do NOT place domain/business services here; domain logic stays in feature handlers/domain layer.
- [ ] Encapsulate provider-specific concerns behind clean abstractions.
- [ ] Apply resilience (retry, timeout, circuit breaker) where appropriate for external calls.

## Application Layer: Models

- [ ] Only place models in `Application/Models` that represent AI-related data or any 3rd-party service payloads.
- [ ] Do NOT store domain entities or core domain models here.
- [ ] Document model purpose and originating service.

## Final Checks Before Review

- [ ] Endpoint comments present and clear.
- [ ] Admin-only rules applied and `/admin` route prefix used where required.
- [ ] `GetAuthUserId` used wherever user id is needed.
- [ ] Explicit response types declared for all outcomes.
- [ ] Endpoint is thin; business logic is in the handler.
- [ ] Mapping profiles are used for all DTO/domain conversions.
- [ ] Application layer boundaries respected for services, interfaces, and models.