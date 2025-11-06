# Frontend Implementation Stories — Party Management (Learning Community — Flow 4)

This document outlines the frontend implementation for the Party Management feature, based on the existing `RogueLearn.Frontend` codebase. It details the page composition, component responsibilities, API wiring, and authentication flow necessary to build a cohesive user experience.

---

## 1. Top-Level Routes & Page Composition

The party management feature is primarily accessed through two top-level routes:

-   **`/party`**: The main party management page.
-   **`/party/[partyId]`**: The detail page for a specific party.

### 1.1. `/party` — Party Management Overview

This page serves as the central hub for users to view their parties and create new ones, designed to be engaging and intuitive.

-   **File Location**: `src/app/party/page.tsx`
-   **Layout**: `DashboardLayout` (`src/components/layout/DashboardLayout.tsx`) provides the overall page structure.
-   **Core Component**: `PartyManagementClient` (`src/components/party/PartyManagementClient.tsx`)
    -   **UI Vision**: A two-column layout.
        -   **Left Column (Sticky Sidebar)**:
            -   A prominent "Create New Party" button with a unique icon that opens the `PartyCreationWizard`.
            -   A filterable list of the user's parties, rendered as `PartyCard` components. Each card displays the party's emblem, name, and member count.
        -   **Right Column (Main Content)**:
            -   **Default View**: A welcoming component that perhaps shows notifications or a summary of recent activities across all parties.
            -   **On Party Selection**: Displays a preview of the selected party's dashboard, showing recent messages, upcoming meetings, or newly added stash items.

### 1.2. `/party/[partyId]` — Party Detail Page

This page provides an immersive, detailed view of a single party.

-   **File Location**: `src/app/party/[partyId]/page.tsx`
-   **Layout**: `DashboardLayout` is used, with the main content area dedicated to the party details.
-   **Core Component**: `PartyDetailPageClient` (`src/pages/party/PartyDetailPageClient.tsx`)
    -   **Functionality**: Fetches party data via `partiesApi.getById(partyId)`.
    -   **UI Vision**: A persistent header displays the party's name, emblem, and primary actions (e.g., "Invite Member," "Leave Party"). Below the header, `PartyDetailClient` renders a rich, tabbed interface.

---

## 2. Core Components & Responsibilities

This section breaks down the key components and their roles in the party management feature.

### 2.1. `PartyManagementClient.tsx`

-   **Responsibility**: Manages the two-column layout for the `/party` page, orchestrating the `PartyListClient` and the main content area.
-   **State**: Manages the currently selected party for the preview display.

### 2.2. `PartyListClient.tsx`

-   **Responsibility**: Fetches and displays a list of parties using the `PartyCard` component.
-   **UI**: A vertical list of `PartyCard`s with a search/filter bar at the top.
-   **API Interaction**: `partiesApi.getMine()` or `partiesApi.admin.getAll()`.

### 2.3. `PartyCard.tsx` (New Component)

-   **Responsibility**: A reusable component to display a summary of a party.
-   **UI**: A visually appealing card with the party's emblem, name, number of members, and perhaps a progress bar for a shared goal.

### 2.4. `PartyCreationWizard.tsx` (Replaces `PartyCreationForm`)

-   **Responsibility**: A multi-step modal for a guided party creation experience.
-   **UI**:
    -   **Step 1: Core Identity**: Form fields for `name`, `description`, and an emblem/icon uploader.
    -   **Step 2: Configuration**: Sliders and toggles for `partyType`, `visibility`, and `maxMembers`.
    -   **Step 3: Invite Members**: An interface to search for and invite initial members.
-   **API Interaction**: `partiesApi.create(partyData)` on the final step.

### 2.5. `PartyDetailClient.tsx`

-   **Responsibility**: Renders the main content of the party detail page with a visually engaging tabbed layout.
-   **UI**: Tabs with icons for each section:
    -   **Dashboard**: `PartyDashboard`
    -   **Stash**: `PartyStash`
    -   **Scheduler**: `MeetingScheduler`
    -   **Live Meeting**: `LiveMeeting`

### 2.6. `PartyDashboard.tsx`

-   **Responsibility**: Provides a comprehensive overview of the party's status and activities.
-   **UI**: A grid-based dashboard with components like:
    -   `PartyStats`: Cards displaying key metrics (members, resources, quests).
    -   `PartyMembersList`: A list of member avatars with names and roles.
    -   `InvitationManagement`: A section to view pending invites and send new ones via a modal form.

### 2.7. `PartyStash.tsx`

-   **Responsibility**: Manages the party's shared resources.
-   **UI**: A rich interface with:
    -   Grid or list view for resources, each as a `ResourceCard`.
    -   Filtering by tags, file type, or member.
    -   An "Add Resource" button that opens a dedicated `AddResourceForm` modal.

### 2.8. `MeetingScheduler.tsx`

-   **Responsibility**: A mock component to be fleshed out.
-   **Future Vision**:
    -   Integrate a calendar view (e.g., `react-big-calendar`).
    -   Allow members to propose, vote on, and schedule meetings.
    -   Display a list of upcoming and past meetings.

### 2.9. `LiveMeeting.tsx`

-   **Responsibility**: A mock component for real-time collaboration.
-   **Future Vision**:
    -   A layout with a main content area (for video/screen-sharing), a participant list, and a chat panel.
    -   Integration with a WebRTC service like Jitsi.

---

## 3. API Contract & Data Models

The frontend interacts with a set of party-related endpoints defined in `src/api/partiesApi.ts`. The data structures are defined in `src/types/parties.ts`.

### 3.1. User-Facing Endpoints

-   **Get My Parties**: `GET /api/parties/mine`
-   **Get Party by ID**: `GET /api/parties/{partyId}`
-   **Create Party**: `POST /api/parties`
-   **Get Members**: `GET /api/parties/{partyId}/members`
-   **Get Resources**: `GET /api/parties/{partyId}/resources`
-   **Add Resource**: `POST /api/parties/{partyId}/resources`
-   **Invite Member**: `POST /api/parties/{partyId}/invitations`
-   **Get Pending Invites**: `GET /api/parties/{partyId}/invitations/pending`

### 3.2. Admin-Only Endpoints

-   **Get All Parties**: `GET /api/admin/parties`
-   **Admin Get Resources**: `GET /api/admin/parties/{partyId}/resources`
-   **Admin Add Resource**: `POST /api/admin/parties/{partyId}/resources`

### 3.3. Core Data Transfer Objects (DTOs)

-   `PartyDto`: Represents a party.
-   `PartyMemberDto`: Represents a member of a party.
-   `PartyInvitationDto`: Represents an invitation to a party.
-   `PartyStashItemDto`: Represents an item in the party's stash.

---

## 4. Authentication & API Client

-   **Axios Client**: A centralized Axios instance is configured in `src/api/axiosClient.ts`.
-   **Base URL**: The `NEXT_PUBLIC_API_URL` environment variable sets the base URL for all API requests.
-   **Auth Interceptor**: An interceptor automatically attaches the JWT bearer token from the logged-in Supabase user to every outgoing request. This handles authentication seamlessly for all API calls made through the client.

---

## 5. Open Items & Assumptions

-   **`MeetingScheduler.tsx` & `LiveMeeting.tsx`**: These components are currently mock implementations. The story for their full functionality, including backend integration for scheduling and real-time updates, needs to be defined.
-   **Roles & Permissions**: The current implementation does not fully flesh out role-based access control within a party (e.g., Leader vs. Member permissions). The UI should be updated to conditionally render actions based on the user's role.
-   **State Management**: For a more robust implementation, consider a centralized state management solution (like Zustand or Redux Toolkit) to manage party data, especially if real-time updates (e.g., via WebSockets) are introduced.
-   **UX & Error Handling**: Comprehensive loading states, error boundaries, and user-friendly error messages should be implemented for all API interactions.
-   **Testing**: A full suite of unit and integration tests should be written for these components to ensure they are reliable and maintainable.
   
---

## 6. UI/UX Enhancements & Open Items

-   **Animations & Transitions**: Implement smooth transitions between views and subtle animations on interactive elements to create a more dynamic feel.
-   **Gamification Elements**: Incorporate visual cues like progress bars for party goals or badges for member achievements.
-   **Roles & Permissions**: The UI should dynamically adapt based on user roles (e.g., only a "Leader" sees the "Invite Member" button).
-   **State Management**: Use a centralized state management solution (e.g., Zustand) for real-time updates.
-   **Error Handling**: Implement comprehensive loading states and user-friendly error messages.
-   **Testing**: Write unit and integration tests for all new and modified components.