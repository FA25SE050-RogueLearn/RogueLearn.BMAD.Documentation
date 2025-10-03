# **User Interface Design Goals**

The visual theme will evoke a "Magical Scribe" or "Adventurer's Journal" aesthetic while maintaining a clean, modern, and readable UI. The application will be a responsive web app, designed to meet WCAG 2.1 AA accessibility standards with mobile-first design principles.

## **Core Learning Experience Interfaces**

- **Main Dashboard ("Character Sheet"):** The central hub displaying user avatar, level/XP progress bar, active quest card with clear "Continue Quest" CTA, character stats (Class/Route), upcoming events list, and skill tree snapshot. Must be highly interactive with hover/tap feedback and optimized for single-column mobile layout expanding to multi-column on desktop.

- **Character Creation Screen:** This screen must allow users to select their "Class" (career goal) and "Route" (academic path). It should also provide a secure and intuitive interface for uploading academic documents (transcripts, schedules). The design should be clean, with clear instructions and feedback on the upload process, including FPTU student verification flow.

- **Quest Log/Quest Line Interface:** A comprehensive view of the user's learning journey showing current, completed, and upcoming quests. Must display quest dependencies, difficulty levels, estimated completion times, and progress indicators. Should integrate seamlessly with the AI-generated learning path and allow quest filtering by subject, difficulty, and type.

- **Skill Tree Visualization:** The interface must include a visually engaging and interactive skill tree. This will display acquired knowledge, show connections between skills, and clearly illustrate the path toward the user's career goals, including highlighting any knowledge gaps. Must support zoom/pan navigation and provide detailed tooltips for each skill node.

- **AI Gap Analysis Results Display:** A clear visualization showing the alignment between academic curriculum and career goals, highlighting knowledge gaps with actionable recommendations. Must present complex data in an easily digestible format with progress tracking over time.

## **Assessment & Gaming Interfaces**

- **Boss Fight/Event Screen:** This screen will present exams and tests as "Boss Fights." It must clearly display the event's description, the knowledge and skills required to "win," and the user's current readiness. Must integrate Unity WebGL components seamlessly with loading states, error handling, and mobile optimization.

- **Assessment/Quiz Interface:** Beyond boss fights, provide interfaces for regular knowledge checks, practice questions, and skill assessments. Must include immediate feedback, progress indicators, and adaptive difficulty adjustment based on performance.

- **Real-time Multiplayer Interface:** For collaborative boss fights and study sessions, providing live participant lists, shared progress indicators, communication tools, and synchronized game states across devices.

## **Social & Collaboration Interfaces**

- **Party Management Screen:** This interface must provide clear options for creating and managing a party. It should include an intuitive system for setting party rules, inviting friends, and a browse/search feature to find and invite other players, displaying key stats to help with selection.

- **Party Creation Wizard:** Step-by-step interface for setting up study groups with compatibility matching, schedule coordination, and goal alignment. Must include member invitation system with status tracking and notification management.

- **Guild Discovery & Management Interface:** A dedicated interface for lecturers to create and manage guilds, upload documents, and communicate with player members. Players will need an interface to join guilds and access these materials. Must include guild browsing, filtering by academic focus, and member management tools.

- **Communication Hub:** In-app messaging system for parties and guilds, announcement boards, and collaborative note sharing. Must support rich text, file attachments, and integration with academic calendars.

## **Content Management & Organization**

- **Arsenal/Notes Editor:** The notes editor must be a clean, intuitive, Notion-like interface that supports rich text formatting, embedding, and organization. Must include tagging system, search functionality, and integration with skill tree nodes for contextual note linking.

- **Document Upload & Processing Interface:** Secure interface for uploading academic transcripts, schedules, and supporting documents with progress indicators, validation feedback, and processing status updates.

- **Skill Tree Interaction:** When a user interacts with a skill node on the tree, the interface should seamlessly display links to related notes in their Arsenal, suggested learning resources, and progress tracking for that specific skill area.

## **Gamification & Progress Tracking**

- **Leaderboard Display:** The UI must present a clear and motivating leaderboard, with filters for viewing rankings by class and overall. Must include weekly/monthly views, category-specific rankings, and privacy controls for competitive display.

- **Achievement & Badge System:** Visual display of earned achievements, progress toward unlockable badges, and celebration animations for milestone completions. Must integrate with the overall progression system and provide social sharing capabilities.

- **Progress Analytics Dashboard:** Personal analytics showing learning patterns, time spent, completion rates, and performance trends with actionable insights for improvement.

## **Administrative & Management Interfaces**

- **Admin Dashboard (Game Master):** Comprehensive platform management interface including user analytics, content moderation tools, system health monitoring, and configuration management. Must provide data visualization and reporting capabilities.

- **Content Moderation Interface:** Tools for reviewing user-generated content, managing reports, and maintaining community standards with efficient workflow management and escalation procedures.

- **Settings & Preferences:** User account management including privacy controls, notification preferences, accessibility options, and data export capabilities.

## **Mobile & Cross-Platform Considerations**

- **Browser Extension UI:** The extension's pop-up (triggered on text highlight) must be unobtrusive and provide a quick, readable view of relevant notes from the user's "Arsenal."

- **Mobile-Optimized Interfaces:** All interfaces must follow mobile-first design with touch-friendly interactions, optimized Unity WebGL performance, and responsive layouts that adapt seamlessly across devices.

- **Cross-Device Synchronization:** Visual indicators for sync status, offline capability notifications, and conflict resolution interfaces for multi-device usage.

## **Accessibility & Technical Requirements**

- **Notification Center:** A clear and accessible notification system to alert users to dynamic changes in their questline and other important updates. Must support multiple notification types, priority levels, and user-configurable delivery preferences.

- **Loading & Error States:** Comprehensive loading skeletons, error recovery interfaces, and graceful degradation for network issues or Unity WebGL compatibility problems.

- **Accessibility Compliance:** All interfaces must meet WCAG 2.1 AA standards with specific attention to screen reader compatibility for gamified elements, keyboard navigation for complex interactions, and high contrast modes for visual accessibility.