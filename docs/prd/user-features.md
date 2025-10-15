# User Role Features

This document lists the features available to each user role in the RogueLearn platform. It complements the role definitions and flows by focusing specifically on capabilities and permissions per role.

References:
- See role definitions in [User Roles and Actors](./user-roles.md)
- See end-to-end journeys in [User Flows](./user-flows.md) and [Main Flows](./main-flows.md)

---

## Player (Student)
- Account creation and onboarding
  - Route (Curriculum) selection
  - Class (Career specialization) selection
  - Optional document upload for skill tree and Arsenal enhancement
  - Optional FPTU student verification path
- AI-powered learning setup
  - Gap analysis between curriculum and career specialization
  - Quest line generation aligned to curriculum (FPTU calendar integration when verified)
  - Integrated skill tree visualization and tracking
- Core learning loop
  - View and complete academic and career quests
  - Engage in Boss Challenges (Unity WebGL assessments)
  - Progress updates and leaderboard rankings
  - Track gap analysis recommendations
- Personal knowledge management
  - Manage notes and resources (Arsenal)
  - Link notes to skill tree nodes and quests
- Social learning
  - Create or join Parties (study groups)
  - Access shared party resources and participate in sessions
  - Receive AI-generated meeting summaries, action items, and reminders
- Guild participation
  - Join guilds and access guild materials and announcements
- Notifications & assistance
  - Quest updates, performance-based adjustments, and session reminders

## Party Leader (Enhanced Player)
- Party creation and configuration
  - Set compatibility rules (Route/Class)
  - Invite members, browse by compatibility, or open party for joining
- Shared resources management
  - Configure resource permissions (view-only, edit, comment-only)
  - Manage content access for party members
- Study session management
  - Schedule sessions by subject and goals
  - Select session types (route-specific study, career discussion, exam prep, project collaboration)
- Progress tracking and AI assistance
  - Capture learning content (manual notes, session recordings)
  - Generate AI summaries, insights, key points, and action items
  - Share results with party and track collective progress
- Member communication
  - Send invitations and updates; coordinate schedules and reminders

## Guild Master
- Guild creation and management
  - Create guilds (streamlined for Verified Lecturers)
  - Access guild management dashboard
  - Manage route-compatible membership
- Community support and curation
  - Upload curriculum reference materials; share resources with members
  - Create educational announcements and updates
  - Monitor anonymized member progress and identify knowledge gaps
- Events and competitions
  - Propose/create guild-based competitive events (subject to admin approval)
  - Manage guild participation, rankings, and post-event insights

## Verified Lecturer (Special Status)
- Platform recognition and visibility
  - Verified Educator badge and credibility indicators
  - Enhanced guild visibility for route-specific communities
- Streamlined creation & analytics
  - Simplified guild creation with educational templates
  - Access to curriculum analytics and route-based student progress tracking
- Content capabilities
  - Ability to create curriculum-verified quest content and assessments
- Support
  - Priority support for curriculum-career integration use cases

## Game Master (Admin)
- Event creation privileges
  - Create platform-wide events with standard templates (auto-approved)
  - Create custom events (subject to peer review)
  - Approve/reject Guild Master event requests
  - Override event parameters for educational quality assurance
  - Access event analytics and performance monitoring tools
- Analytics & monitoring
  - View educational analytics (player engagement, route completion, career progress)
  - Monitor platform performance and educational quality
- Platform management
  - Configure system settings (notifications, quest parameters)
  - Content moderation, guild verification, credential validation
  - User management (access control, data privacy, retention)
  - Student status monitoring (for verified flows)
- Admin-owned educational governance
  - Elective Library curation & approval (vetting, tagging, versioning, publishing)
  - University curriculum import & administration (imports, versioning, activation windows, reporting)
  - Audit & compliance (reviewer attribution, decision logs)
- Integrations & operations
  - Monitor external educational integrations and data sync

## Guild Member (Player in a Guild)
- Access guild-specific resources and content
- Participate in guild activities and discussions
- Receive mentorship and educational support
- Collaborate with guild members on shared goals

## Spectator (Competition Viewing)
- Watch live code battle events
- View real-time rankings and performance metrics

---

Notes
- Verified Lecturer is a status that enhances capabilities for Players and Guild Masters; it is not a separate standalone role.
- Some advanced features (e.g., event lifecycle management, browser extension integrations) are introduced in later phases and may be limited in MVP.