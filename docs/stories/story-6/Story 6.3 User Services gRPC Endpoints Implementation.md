# User Service gRPC Endpoints (v1)

- Goal: Expose gRPC endpoints for user-facing capabilities to support low-latency, strongly-typed inter-service communication.
- Scope: User Profiles, User Context, Achievements, Guilds.
- Consumers: Internal services and clients requiring binary RPC with contract-first APIs.
- Out of scope: Public HTTP exposure, API Gateway concerns, data migrations.

## Background

- The application uses MediatR with Commands/Queries across feature folders, AutoMapper, FluentValidation, and centralized auth. These handlers are the source of truth for business logic and should be invoked from gRPC service implementations.
- Representative handlers to wire:
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\UserProfiles\Queries\GetUserProfileByAuthId\GetUserProfileByAuthIdQuery.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\UserProfiles\Commands\UpdateMyProfile\UpdateMyProfileCommand.cs:6`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\UserProfiles\Commands\LogNewUser\LogNewUserCommand.cs:6`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\UserProfiles\Queries\GetAllUserProfiles\GetAllUserProfilesQuery.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\UserContext\Queries\GetUserContextByAuthId\GetUserContextByAuthIdQuery.cs:6`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Commands\CreateAchievement\CreateAchievementCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Commands\UpdateAchievement\UpdateAchievementCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Commands\DeleteAchievement\DeleteAchievementCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Commands\AwardAchievementToUser\AwardAchievementToUserCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Commands\RevokeAchievementFromUser\RevokeAchievementFromUserCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Queries\GetAllAchievements\GetAllAchievementsQuery.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Queries\GetUserAchievementsByAuthId\GetUserAchievementsByAuthIdQuery.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\CreateGuild\CreateGuildCommand.cs:6`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\InviteMember\InviteGuildMembersCommand.cs:7`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\ApplyJoinRequest\ApplyGuildJoinRequestCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\ApproveJoinRequest\ApproveGuildJoinRequestCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\DeclineJoinRequest\DeclineGuildJoinRequestCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\AcceptInvitation\AcceptGuildInvitationCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\AssignGuildRole\AssignGuildRoleCommand.cs:6`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\RevokeGuildRole\RevokeGuildRoleCommand.cs:6`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\TransferLeadership\TransferGuildLeadershipCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\LeaveGuild\LeaveGuildCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\RemoveMember\RemoveGuildMemberCommand.cs:5`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Queries\GetAllGuilds\GetAllGuildsQuery.cs:6`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Queries\GetGuildById\GetGuildByIdQuery.cs:6`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Queries\GetGuildDashboard\GetGuildDashboardQuery.cs:6`
  - `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Queries\GetGuildMembers\GetGuildMembersQuery.cs:6`

## Architecture

- Add gRPC to the API host and implement gRPC service classes that translate RPCs to MediatR commands/queries.
- Authentication via JWT/Bearer; forward identity from metadata to handlers as `AuthUserId`.
- Error handling via `RpcException` with `StatusCode` aligned to domain errors.
- Reflection service enabled in non-production for tooling (e.g., `grpcurl`).
- Versioned `.proto` definitions under `protos/user/v1`.

## Setup Tasks

- Add package `Grpc.AspNetCore` and `Google.Protobuf` as needed.
- Host configuration:
  - Register gRPC: `builder.Services.AddGrpc()`
  - Map services: `app.MapGrpcService<UserProfilesGrpcService>()`, `UserContextGrpcService`, `AchievementsGrpcService`, `GuildsGrpcService`
- Enable gRPC Reflection in Dev only.
- Ensure auth middleware supports reading `Authorization` from gRPC metadata and populates claims for handlers.

## Services and RPCs

### UserProfilesService
- GetByAuthId(auth_user_id) → UserProfile
  - Maps to `GetUserProfileByAuthIdQuery` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\UserProfiles\Queries\GetUserProfileByAuthId\GetUserProfileByAuthIdQuery.cs:5`
- UpdateMyProfile(profile_update) → UserProfile
  - Maps to `UpdateMyProfileCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\UserProfiles\Commands\UpdateMyProfile\UpdateMyProfileCommand.cs:6`
- LogNewUser(auth_user_id) → Empty
  - Maps to `LogNewUserCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\UserProfiles\Commands\LogNewUser\LogNewUserCommand.cs:6`
- GetAll(pagination) → UserProfileList
  - Maps to `GetAllUserProfilesQuery` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\UserProfiles\Queries\GetAllUserProfiles\GetAllUserProfilesQuery.cs:5`

### UserContextService
- GetByAuthId(auth_user_id) → UserContext
  - Maps to `GetUserContextByAuthIdQuery` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\UserContext\Queries\GetUserContextByAuthId\GetUserContextByAuthIdQuery.cs:6`

### AchievementsService
- CreateAchievement(payload) → Achievement
  - Maps to `CreateAchievementCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Commands\CreateAchievement\CreateAchievementCommand.cs:5`
- UpdateAchievement(payload) → Achievement
  - Maps to `UpdateAchievementCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Commands\UpdateAchievement\UpdateAchievementCommand.cs:5`
- DeleteAchievement(achievement_id) → Empty
  - Maps to `DeleteAchievementCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Commands\DeleteAchievement\DeleteAchievementCommand.cs:5`
- AwardToUser(achievement_id, auth_user_id) → Empty
  - Maps to `AwardAchievementToUserCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Commands\AwardAchievementToUser\AwardAchievementToUserCommand.cs:5`
- RevokeFromUser(achievement_id, auth_user_id) → Empty
  - Maps to `RevokeAchievementFromUserCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Commands\RevokeAchievementFromUser\RevokeAchievementFromUserCommand.cs:5`
- GetAll(pagination/filter) → AchievementList
  - Maps to `GetAllAchievementsQuery` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Queries\GetAllAchievements\GetAllAchievementsQuery.cs:5`
- GetByAuthUserId(auth_user_id) → UserAchievementList
  - Maps to `GetUserAchievementsByAuthIdQuery` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Achievements\Queries\GetUserAchievementsByAuthId\GetUserAchievementsByAuthIdQuery.cs:5`

### GuildsService
- CreateGuild(name, description, privacy, max_members, creator_auth_user_id) → CreateGuildResponse
  - Maps to `CreateGuildCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\CreateGuild\CreateGuildCommand.cs:6`
- GetAllGuilds(include_private) → GuildList
  - Maps to `GetAllGuildsQuery` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Queries\GetAllGuilds\GetAllGuildsQuery.cs:6`
- GetGuildById(guild_id) → Guild
  - Maps to `GetGuildByIdQuery` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Queries\GetGuildById\GetGuildByIdQuery.cs:6`
- GetDashboard(guild_id) → GuildDashboard
  - Maps to `GetGuildDashboardQuery` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Queries\GetGuildDashboard\GetGuildDashboardQuery.cs:6`
- GetMembers(guild_id) → GuildMemberList
  - Maps to `GetGuildMembersQuery` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Queries\GetGuildMembers\GetGuildMembersQuery.cs:6`
- InviteMember(guild_id, inviter_auth_user_id, targets, message) → Empty
  - Maps to `InviteGuildMembersCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\InviteMember\InviteGuildMembersCommand.cs:7`
- ApplyJoinRequest(guild_id, auth_user_id, message) → Empty
  - Maps to `ApplyGuildJoinRequestCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\ApplyJoinRequest\ApplyGuildJoinRequestCommand.cs:5`
- ApproveJoinRequest(guild_id, request_id, actor_auth_user_id) → Empty
  - Maps to `ApproveGuildJoinRequestCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\ApproveJoinRequest\ApproveGuildJoinRequestCommand.cs:5`
- DeclineJoinRequest(guild_id, request_id, actor_auth_user_id) → Empty
  - Maps to `DeclineGuildJoinRequestCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\DeclineJoinRequest\DeclineGuildJoinRequestCommand.cs:5`
- AcceptInvitation(guild_id, invitation_id, auth_user_id) → Empty
  - Maps to `AcceptGuildInvitationCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\AcceptInvitation\AcceptGuildInvitationCommand.cs:5`
- AssignRole(guild_id, member_auth_user_id, role, actor_auth_user_id, is_admin_override) → Empty
  - Maps to `AssignGuildRoleCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\AssignGuildRole\AssignGuildRoleCommand.cs:6`
- RevokeRole(guild_id, member_auth_user_id, role, actor_auth_user_id, is_admin_override) → Empty
  - Maps to `RevokeGuildRoleCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\RevokeGuildRole\RevokeGuildRoleCommand.cs:6`
- TransferLeadership(guild_id, to_user_id) → Empty
  - Maps to `TransferGuildLeadershipCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\TransferLeadership\TransferGuildLeadershipCommand.cs:5`
- LeaveGuild(guild_id, auth_user_id) → Empty
  - Maps to `LeaveGuildCommand` `d:\Code Stiffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\LeaveGuild\LeaveGuildCommand.cs:5`
- RemoveMember(guild_id, member_id, reason) → Empty
  - Maps to `RemoveGuildMemberCommand` `d:\Code Stuffs\MinhAnh2610\RogueLearn.User\src\RogueLearn.User.Application\Features\Guilds\Commands\RemoveMember\RemoveGuildMemberCommand.cs:5`
- Additional queries: invitations, join requests, member roles, my guild, my join requests (map to existing query handlers in `src/RogueLearn.User.Application/Features/Guilds/Queries/...`).

## Proto Contracts

- Organize under `protos/user/v1/` with separate services or one consolidated file.
- Outline (names only; actual messages should mirror DTOs):

```proto
service UserProfilesService {
  rpc GetByAuthId(GetByAuthIdRequest) returns (UserProfile);
  rpc UpdateMyProfile(UpdateMyProfileRequest) returns (UserProfile);
  rpc LogNewUser(LogNewUserRequest) returns (google.protobuf.Empty);
  rpc GetAll(GetAllRequest) returns (UserProfileList);
}

service UserContextService {
  rpc GetByAuthId(GetByAuthIdRequest) returns (UserContext);
}

service AchievementsService {
  rpc CreateAchievement(CreateAchievementRequest) returns (Achievement);
  rpc UpdateAchievement(UpdateAchievementRequest) returns (Achievement);
  rpc DeleteAchievement(DeleteAchievementRequest) returns (google.protobuf.Empty);
  rpc AwardToUser(AwardToUserRequest) returns (google.protobuf.Empty);
  rpc RevokeFromUser(RevokeFromUserRequest) returns (google.protobuf.Empty);
  rpc GetAll(GetAllAchievementsRequest) returns (AchievementList);
  rpc GetByAuthUserId(GetUserAchievementsRequest) returns (UserAchievementList);
}

service GuildsService {
  rpc CreateGuild(CreateGuildRequest) returns (CreateGuildResponse);
  rpc GetAllGuilds(GetAllGuildsRequest) returns (GuildList);
  rpc GetGuildById(GetGuildByIdRequest) returns (Guild);
  rpc GetDashboard(GetGuildDashboardRequest) returns (GuildDashboard);
  rpc GetMembers(GetGuildMembersRequest) returns (GuildMemberList);
  rpc InviteMember(InviteMemberRequest) returns (google.protobuf.Empty);
  rpc ApplyJoinRequest(ApplyJoinRequest) returns (google.protobuf.Empty);
  rpc ApproveJoinRequest(ApproveJoinRequest) returns (google.protobuf.Empty);
  rpc DeclineJoinRequest(DeclineJoinRequest) returns (google.protobuf.Empty);
  rpc AcceptInvitation(AcceptInvitationRequest) returns (google.protobuf.Empty);
  rpc AssignRole(AssignRoleRequest) returns (google.protobuf.Empty);
  rpc RevokeRole(RevokeRoleRequest) returns (google.protobuf.Empty);
  rpc TransferLeadership(TransferLeadershipRequest) returns (google.protobuf.Empty);
  rpc LeaveGuild(LeaveGuildRequest) returns (google.protobuf.Empty);
  rpc RemoveMember(RemoveMemberRequest) returns (google.protobuf.Empty);
}
```

## Security

- Require `Authorization: Bearer <token>` in gRPC metadata; validate and populate `AuthUserId` claim.
- Enforce authorization in handlers; ensure role-based checks for Guild actions already implemented remain effective through gRPC.

## Error Handling

- Translate domain exceptions to `RpcException`:
  - `InvalidArgument` for validation failures.
  - `NotFound` for missing resources.
  - `PermissionDenied` for authz failures.
  - `Internal` for unexpected errors.
- Surface validation details using trailers where appropriate.

## Non-Functional

- Performance: keep payloads aligned to existing DTOs; page large lists.
- Observability: log request IDs and user IDs; structured logs via Serilog.
- Backwards compatibility: no change to HTTP controllers; gRPC complements existing API.

## Acceptance Criteria

- gRPC services registered in the API host; `builder.Services.AddGrpc()` and `app.MapGrpcService<...>()` added in `src/RogueLearn.User.Api/Program.cs`.
- `.proto` files authored for UserProfiles, UserContext, Achievements, Guilds under `protos/user/v1`, compiled into C# stubs.
- Each RPC implemented by delegating to the corresponding MediatR handler, maintaining validators and mapping profiles.
- JWT authentication enforced on all RPCs; identity forwarded to handlers.
- Error handling yields correct `StatusCode`s and messages.
- Dev-only gRPC Reflection enabled; basic `grpcurl` calls succeed.
- Unit tests cover service-to-handler wiring and common error paths; integration test covers at least one RPC per service.

## Definition of Done

- All services compile; tests pass; local smoke test with `grpcurl` returns expected results for at least:
  - UserProfilesService.GetByAuthId
  - UserContextService.GetByAuthId
  - AchievementsService.GetAll
  - GuildsService.GetGuildById
- Documentation of RPCs and expected metadata provided alongside `.proto` comments.
- No changes to public HTTP routes; Swagger remains untouched.