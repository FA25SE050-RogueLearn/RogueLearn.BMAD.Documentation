# Frontend Implementation Stories — Guild Management (Learning Community — Flow 4)

This document focuses on Guild-related UI for Flow 4. It provides actionable user stories with acceptance criteria, UI scope, data/state expectations, and error handling.

## Scope and Assumptions
- Platform: Web SPA using the design tokens and patterns from docs/front-end-spec.
- Routes and components align with the visual and screen specifications.
- API paths are placeholders and must be replaced with actual endpoints under docs/fullstack-architecture/service-apis/.
- Accessibility (WCAG AA) and responsiveness are required.

---

## G-FE-001 — Guild Directory and Search
As a Student, I want to browse and search guilds so I can find communities that match my goals and language focus.

**Acceptance Criteria**
- Given I open “Guilds” via top navigation, when the page loads, then I see a paginated list/grid of guilds with name, description, member count, tags, and join status.
- Given I type in the search box, when I submit or pause, then the list filters by name/description/tags.
- Given I apply filters (language, capacity, eligibility), when I change filter chips, then results update with server-backed queries.
- Given a guild is at capacity, then I see a “Full” badge and a “Join Waitlist” action (if enabled).
- Given my profile does not meet eligibility rules, then the card shows “Not eligible” with a tooltip explaining why.

**UI Scope**
- **Route**: `/community/guilds`
- **Components**:
    - `GuildDirectoryPage`: The main page component.
    - `GuildList`: Displays a list of `GuildCard` components.
    - `GuildCard`: A card representing a single guild, showing its name, banner image, description, member count, and tags. It will also have a "View" button.
    - `FilterBar`: A bar with filter options like language, tags, and availability.
    - `SearchInput`: A search input field for text-based search.
    - `Pagination`: For navigating through pages of guilds.
    - `LoadingSkeleton`: A skeleton loader to show while guilds are being fetched.
    - `EmptyState`: A message to show when no guilds are found.

**State & Data**
- **Client state**: `query { text, tags, language, capacity }`, `pageable { page, size }`, `sort { name|members|created }`
- **Data shape**: `Guild { id, name, description, tags[], language, capacity, memberCount, eligibilityStatus, canApply }`

**APIs**
- `GET /api/guilds`

**Error Handling**
- Network error: inline retry + toast.
- Empty results: “No guilds found” guidance.

**Analytics**
- Track search term, filters used, card clicks.

---

## G-FE-002 — Create a Guild
As a Student, I want to create my own guild so I can start a new community.

**Acceptance Criteria**
- Given I am on the guild directory page, then I see a "Create Guild" button.
- Given I click the "Create Guild" button, then a modal or a new page appears with a form to create a new guild.
- Given I fill in the required information (name, description, etc.) and submit the form, then a new guild is created and I am redirected to the new guild's home page.

**UI Scope**
- **Route**: `/community/guilds/create`
- **Components**:
    - `CreateGuildPage`: A page with a form to create a new guild.
    - `CreateGuildForm`: A form with fields for guild name, description, language, and other settings.
    - `ImageUploader`: A component to upload a banner image for the guild.

**State & Data**
- **Form state**: `{ name: string, description: string, language: string, isPublic: boolean }`

**APIs**
- `POST /api/guilds`

**Error Handling**
- Validation errors on the form fields.
- Server errors on guild creation.

**Analytics**
- Track guild creation attempts and success rate.

---

## G-FE-003 — Guild Detail and Join Request
As a Student, I want to open a guild page and submit a join request so officers can review me.

**Acceptance Criteria**
- Given I click a guild card, then I see the detail page with description, rules, tags, officers, and member stats.
- Given I meet eligibility, then I see a “Request to Join” button that opens a modal with motivation textarea and optional screening questions.
- Given I submit a valid request, then I see a success state and my request becomes “Pending Review”.
- Given I do not meet eligibility, then the join button is disabled with a clear reason.

**UI Scope**
- **Route**: `/community/guilds/:guildId`
- **Components**:
    - `GuildDetailPage`: The main page for viewing a guild's details.
    - `GuildHeader`: Displays the guild's banner, name, and description.
    - `GuildTabs`: Tabs to navigate between different sections of the guild page (e.g., Home, Members, Posts).
    - `RulesPanel`: A panel displaying the guild's rules.
    - `OfficersList`: A list of the guild's officers.
    - `JoinRequestModal`: A modal for submitting a join request.
    - `SuccessState`: A message shown after successfully submitting a join request.

**State & Data**
- **Data shape**: `GuildDetail { id, name, description, rules[], officers[], tags[], eligibilityStatus }`
- **Form state**: `{ motivation: string, answers: Record<string,string> }`

**APIs**
- `GET /api/guilds/:guildId`
- `POST /api/guilds/:guildId/join-requests/apply`

**Error Handling**
- Validation: motivation required, screening answers required when presented.
- Server errors: show reasons (already applied, capacity reached) and guidance.

**Analytics**
- Track request opens, submissions, validation failures.

---

## G-FE-004 — Guild Membership Home
As a Guild Member, I want a guild home view so I can see announcements, events, and parties in my guild.

**Acceptance Criteria**
- Given I am a member, when I open my guild, then I see announcements, upcoming events, and parties with quick actions.
- Given I have role Leader/Officer, then I see management shortcuts (create event, moderate members, create party).

**UI Scope**
- **Route**: `/community/guilds/:guildId/home`
- **Components**:
    - `GuildHomePage`: The main component for the guild home page.
    - `AnnouncementFeed`: A feed of guild announcements (posts).
    - `EventsList`: A list of upcoming guild events.
    - `PartiesList`: A list of active parties in the guild.
    - `RoleBadges`: Badges to indicate the user's role in the guild.
    - `QuickActions`: A set of buttons for quick actions based on the user's role.

**APIs**
- `GET /api/guilds/:guildId/dashboard`
- `GET /api/guilds/:guildId/posts`

**Error Handling**
- Permission gating: hide/disable features not allowed for my role.

**Analytics**
- Track engagement (clicks on announcements, events, parties).

---

## G-FE-005 — Guild Posts
As a Guild Member, I want to be able to view, create, and interact with posts in my guild.

**Acceptance Criteria**
- Given I am a member, when I navigate to the posts tab, then I see a list of posts.
- Given I can create a new post with a title, content, and optional tags.
- Given I can edit my own posts.
- Given I can delete my own posts.
- As a Guild Master or Officer, I can pin, lock, and approve posts.

**UI Scope**
- **Route**: `/community/guilds/:guildId/posts`
- **Components**:
    - `GuildPostsPage`: The main component for the guild posts page.
    - `PostList`: A list of posts.
    - `PostCard`: A card for a single post, showing the author, content, and actions.
    - `CreatePostForm`: A form for creating a new post.
    - `EditPostForm`: A form for editing an existing post.

**State & Data**
- **Data shape**: `GuildPost { id, title, content, author, tags[], createdAt, isPinned, isLocked }`

**APIs**
- `GET /api/guilds/:guildId/posts`
- `GET /api/guilds/:guildId/posts/:postId`
- `POST /api/guilds/:guildId/posts`
- `PUT /api/guilds/:guildId/posts/:postId`
- `DELETE /api/guilds/:guildId/posts/:postId`
- `POST /api/admin/guilds/:guildId/posts/:postId/pin`
- `POST /api/admin/guilds/:guildId/posts/:postId/unpin`
- `POST /api/admin/guilds/:guildId/posts/:postId/lock`
- `POST /api/admin/guilds/:guildId/posts/:postId/unlock`
- `POST /api/admin/guilds/:guildId/posts/:postId/approve`
- `POST /api/admin/guilds/:guildId/posts/:postId/reject`

**Error Handling**
- Form validation errors.
- Server errors on post creation/editing/deletion.
- Permission errors for admin actions.

**Analytics**
- Track post creation, views, and interactions.

---

## G-FE-006 — Notifications (Guild)
As a Student, I want timely guild notifications so I don’t miss invites and decisions.

**Acceptance Criteria**
- Given I receive a guild invite or decision, then I see a bell icon count and an inbox listing notifications by type and unread status.
- Given I click a notification, then I’m taken to the relevant screen; unread becomes read.

**UI Scope**
- **Global Components**:
    - `NotificationBell`: A bell icon in the main navigation that shows the number of unread notifications.
    - `NotificationInbox`: A dropdown or a separate page that lists all guild-related notifications.

**APIs**
- `GET /api/notifications?context=guild`
- `POST /api/notifications/:id/read`

**Error Handling**
- Inbox fetch errors: retry and show partial offline cache if available.

**Analytics**
- Track notification open rate and click-through.

---

## G-FE-007 — Responsive Design and Accessibility (Guild)
As a Student, I want guild screens to work on any device and be accessible.

**Acceptance Criteria**
- Screens adapt to mobile/tablet/desktop breakpoints; critical actions remain reachable.
- Keyboard navigation covers interactive controls; focus states visible; ARIA roles set.
- Color contrast and font sizes meet AA standards.

**References**
- `docs/front-end-spec/screen-specifications.md`
- `docs/front-end-spec/visual-design-system.md`

---

## G-FE-008 — Guild Management
As a Guild Master or Officer, I want to manage my guild so I can maintain a healthy community.

**Acceptance Criteria**
- Given I am a Guild Master or Officer, then I see a "Manage" tab in the guild navigation.
- Given I click the "Manage" tab, then I see a page with options to manage members, roles, and guild settings.
- Given I can view a list of all members, their roles, and their join date.
- Given I can approve or decline join requests.
- Given I can kick members from the guild.
- Given I can assign and revoke roles to members.
- Given I can edit the guild's name, description, and other settings.

**UI Scope**
- **Route**: `/community/guilds/:guildId/manage`
- **Components**:
    - `GuildManagementPage`: The main page for managing a guild.
    - `MemberManagementTab`: A tab for managing guild members.
    - `RoleManagementTab`: A tab for managing guild roles.
    - `SettingsManagementTab`: A tab for managing guild settings.
    - `MemberList`: A list of guild members with their roles and actions.
    - `JoinRequestList`: A list of pending join requests.
    - `RoleEditor`: A form for creating and editing roles.
    - `GuildSettingsForm`: A form for editing the guild's settings.

**APIs**
- `GET /api/guilds/:guildId/members`
- `GET /api/guilds/:guildId/join-requests`
- `POST /api/guilds/:guildId/join-requests/:requestId/approve`
- `POST /api/guilds/:guildId/join-requests/:requestId/decline`
- `DELETE /api/guilds/:guildId/members/:memberId`
- `POST /api/guilds/:guildId/members/:memberId/roles`
- `PUT /api/guilds/:guildId`

**Error Handling**
- Permission errors for actions that are not allowed.
- Validation errors on forms.
- Server errors on management actions.

**Analytics**
- Track management actions taken by guild masters and officers.

---

## G-FE-009 — Guild Invitations
As a Guild Master or Officer, I want to invite users to join my guild so I can grow my community. As a Student, I want to receive and respond to guild invitations.

**Acceptance Criteria (Guild Leader)**
- Given I am on the guild management page, then I see an "Invite Members" section.
- Given I can search for users by their username or email.
- Given I select a user and click "Invite", then an invitation is sent to that user.
- Given the user has already been invited or is already a member, then I see a message indicating that.

**Acceptance Criteria (Student)**
- Given I receive a guild invitation, then I get a notification.
- Given I open the invitation (from notifications or a dedicated "invites" page), then I see details about the guild and options to "Accept" or "Decline".
- Given I accept the invitation, then I become a member of the guild.
- Given I decline the invitation, then the invitation is removed.

**UI Scope**
- **Components (Guild Leader)**:
    - `InviteMemberForm`: A form within the management page to search for and invite users.
    - `UserSearchInput`: An autocomplete input for finding users.
- **Components (Student)**:
    - `GuildInvitationNotification`: A notification item for a new guild invite.
    - `GuildInvitationsPage`: A page to view and manage pending guild invitations.

**APIs**
- `POST /api/guilds/:guildId/invitations`
- `POST /api/guilds/invitations/:invitationId/accept`
- `POST /api/guilds/invitations/:invitationId/decline`
- `GET /api/guilds/invitations/me`

**Error Handling**
- User not found when searching for invites.
- Invitation failed to send.
- Errors on accepting/declining invites.

---

## G-FE-010 — Leave Guild
As a Guild Member, I want to leave a guild when it's no longer a good fit for me.

**Acceptance Criteria**
- Given I am a member of a guild, when I view the guild's main page, then I see a "Leave Guild" button.
- Given I click the "Leave Guild" button, then I see a confirmation modal to prevent accidental leaving.
- Given I confirm that I want to leave, then I am no longer a member of the guild and am redirected to the guild directory.
- Given I am the Guild Master, then the "Leave Guild" button is disabled, and I am instructed to transfer leadership first.

**UI Scope**_
- **Components**:
    - `LeaveGuildButton`: A button on the guild home page.
    - `LeaveGuildConfirmationModal`: A modal to confirm the action.

**APIs**
- `POST /api/guilds/leave`

**Error Handling**
- Show an error if the user is the Guild Master and tries to leave.
- Handle server errors on leaving the guild.

---

## G-FE-011 — Advanced Guild Leadership
As a Guild Master, I want access to critical administrative functions so I can manage the guild's lifecycle.

**Acceptance Criteria (Transfer Leadership)**
- Given I am the Guild Master, in the guild management area, then I see an option to "Transfer Leadership".
- Given I click "Transfer Leadership", then I can select a guild member to become the new Guild Master.
- Given I confirm the transfer, then the selected member is promoted to Guild Master and I am demoted to a regular member (or a default officer role).

**Acceptance Criteria (Delete Guild)**
- Given I am the Guild Master, in the guild management area, then I see an option to "Delete Guild".
- Given I click "Delete Guild", then I see a confirmation modal that requires me to type the guild's name to confirm.
- Given I confirm the deletion, then the guild is permanently deleted, and all members are notified.

**UI Scope**
- **Components**:
    - `TransferLeadershipSection`: A part of the management page for transferring leadership.
    - `DeleteGuildSection`: A part of the management page for deleting the guild.
    - `ConfirmationModal`: A generic but configurable confirmation modal for these dangerous actions.

**APIs**
- `POST /api/guilds/:guildId/transfer-leadership`
- `DELETE /api/guilds/:guildId`

**Error Handling**
- The user must be the Guild Master to perform these actions.
- Validation for selecting a valid member for leadership transfer.
- Strict confirmation steps to prevent accidental deletion.

---

## G-FE-012 — Error and Edge Case Handling (Guild)
As a Student, I want consistent handling of guild-specific issues.

**Acceptance Criteria**
- Capacity reached: request disabled or waitlist shown.
- Eligibility failed: disabled join button with clear explanation.
- Permission denied: gated UI with explanation and next steps.
- Offline mode (read-only): cached directory and detail (if available), with sync banner when back online.

**UI Scope**
- Global error boundary components, toasts, banners.

**APIs**
- N/A

**Analytics**
- Track error occurrences and retry success.