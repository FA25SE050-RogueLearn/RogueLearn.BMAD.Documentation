# Data Requirements

This document outlines the comprehensive data model for RogueLearn, covering all phases from MVP to post-MVP features. The data model is designed to support the functional requirements outlined in the requirements document and provides a scalable foundation for the gamified learning platform.

## Core Entities and Relationships

### Phase 1: Core Student MVP

**User Management & Authentication**

*   **User**: Represents an individual using the RogueLearn platform.
    *   `user_id` (Primary Key)
    *   `username`
    *   `email`
    *   `password_hash` (Managed by Clerk)
    *   `clerk_user_id` (External ID from Clerk authentication)
    *   `created_at`
    *   `last_login_at`
    *   `profile_image_url`
    *   `account_status` (e.g., 'Active', 'Inactive', 'Suspended')
    *   `onboarding_completed` (Boolean)
    *   `character_creation_completed` (Boolean)

*   **UserProfile**: Extends User with game-specific attributes from character creation.
    *   `user_profile_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `class_id` (Foreign Key to `Class`, representing career goal)
    *   `route_id` (Foreign Key to `Route`, representing academic path)
    *   `smaller_major_id` (Foreign Key to `SmallerMajor`, optional)
    *   `level`
    *   `experience_points`
    *   `initial_stats` (JSON object with stats influenced by academic data)
    *   `created_at`
    *   `updated_at`
  
*   **UserRole**: Represents a role that a user can have in different contexts.
    *   `user_role_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `context_type` (e.g., 'Party', 'Guild')
    *   `context_id` (ID of the context entity)
    *   `role` (e.g., 'Guild Master', 'Player', 'Party Leader', 'Game Master')
    *   `assigned_at`
    *   `assigned_by` (Foreign Key to `User`, optional)
    *   `status` (e.g., 'Active', 'Inactive')

**Character Creation & Academic Paths**

*   **Class**: Represents a career goal that a user can select during character creation.
    *   `class_id` (Primary Key)
    *   `name` (e.g., 'Full-Stack Developer', 'Data Scientist')
    *   `description`
    *   `icon_url`
    *   `suggested_routes` (JSON array of recommended route IDs)
    *   `created_at`
    *   `updated_at`

*   **Route**: Represents an academic path that a user can select during character creation.
    *   `route_id` (Primary Key)
    *   `name` (e.g., 'Software Engineering', 'Computer Science')
    *   `description`
    *   `icon_url`
    *   `created_at`
    *   `updated_at`

*   **SmallerMajor**: Represents specialized tracks within academic routes detected by AI.
    *   `smaller_major_id` (Primary Key)
    *   `route_id` (Foreign Key to `Route`)
    *   `name` (e.g., '.NET Specialization', 'Machine Learning Track')
    *   `description`
    *   `detection_keywords` (JSON array of keywords for AI detection)
    *   `suggested_classes` (JSON array of related class IDs)
    *   `created_at`
    *   `updated_at`

**Academic Document Management**

*   **Course**: Represents a course or subject that a user is studying.
    *   `course_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `name`
    *   `description`
    *   `course_code` (Optional, e.g., 'CS101')
    *   `semester` (Optional)
    *   `year` (Optional)
    *   `created_at`
    *   `updated_at`
    *   `activation_status` (e.g., 'Active', 'Completed', 'Archived')

*   **Syllabus**: Represents the document uploaded by the user for a course.
    *   `syllabus_id` (Primary Key)
    *   `course_id` (Foreign Key to `Course`)
    *   `content` (The raw text or structured data from the uploaded file)
    *   `structured_content` (JSON object with AI-parsed curriculum data)
    *   `file_type` (e.g., 'PDF', 'DOCX')
    *   `file_url` (URL to the stored file in Supabase)
    *   `uploaded_at`
    *   `processed_at` (When the AI finished processing)
    *   `processing_status` (e.g., 'Pending', 'Processing', 'Completed', 'Failed')
    *   `detected_route_id` (Foreign Key to `Route`, AI-detected)
    *   `detected_smaller_majors` (JSON array of detected smaller major IDs)

*   **AcademicDocument**: Represents additional academic documents uploaded by the user.
    *   `academic_document_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `document_type` (e.g., 'GPA', 'Transcript', 'Timetable', 'Exam Schedule')
    *   `content` (The raw text or structured data from the uploaded file)
    *   `structured_content` (JSON object with AI-parsed data)
    *   `file_type` (e.g., 'PDF', 'DOCX')
    *   `file_url` (URL to the stored file in Supabase)
    *   `uploaded_at`
    *   `processed_at`
    *   `processing_status`
    *   `influences_stats` (Boolean, whether this document affects character stats)

**Quest System & Learning Paths**

*   **QuestLine**: Represents the main learning path generated for a user.
    *   `quest_line_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `course_id` (Foreign Key to `Course`)
    *   `title`
    *   `description`
    *   `generation_source` (e.g., 'Syllabus', 'Schedule', 'Manual')
    *   `is_dynamic` (Boolean, whether AI can modify uncompleted tasks)
    *   `created_at`
    *   `updated_at`
    *   `activation_status` (e.g., 'Active', 'Completed', 'Archived')

*   **Quest**: Represents a learning task or objective generated from the syllabus or schedule.
    *   `quest_id` (Primary Key)
    *   `quest_line_id` (Foreign Key to `QuestLine`)
    *   `title`
    *   `description`
    *   `quest_type` (e.g., 'Learning', 'Assignment', 'Exam', 'Schedule Event')
    *   `progress_status` (e.g., 'Not Started', 'In Progress', 'Completed')
    *   `due_date` (Optional, from schedule or manual input)
    *   `created_at`
    *   `updated_at`
    *   `completion_date`
    *   `experience_points` (XP reward for completion)
    *   `prerequisites` (JSON array of prerequisite quest IDs)
    *   `is_ai_modifiable` (Boolean, whether AI can change this quest)
    *   `source_document_id` (Foreign Key to AcademicDocument, optional)

*   **QuestSkillRelationship**: A junction table to manage relationships between quests and skills.
    *   `quest_skill_relationship_id` (Primary Key)
    *   `quest_id` (Foreign Key to `Quest`)
    *   `skill_id` (Foreign Key to `Skill`)
    *   `relationship_type` (e.g., 'Requires', 'Rewards', 'Enhances')
    *   `created_at`

**Skill Tree & Knowledge Management**

*   **SkillTree**: Represents the user's knowledge structure.
    *   `skill_tree_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `course_id` (Foreign Key to `Course`, optional)
    *   `name`
    *   `description`
    *   `is_interconnected` (Boolean, whether skills show relationships)
    *   `visualization_type` (e.g., 'Mind Map', 'Tree', 'Network')
    *   `created_at`
    *   `updated_at`

*   **Skill**: Represents a specific knowledge or ability in the skill tree.
    *   `skill_id` (Primary Key)
    *   `skill_tree_id` (Foreign Key to `SkillTree`)
    *   `name`
    *   `description`
    *   `level` (Current proficiency level)
    *   `max_level` (Maximum achievable level)
    *   `icon_url`
    *   `position_x` (X coordinate in the skill tree visualization)
    *   `position_y` (Y coordinate in the skill tree visualization)
    *   `prerequisites` (JSON array of prerequisite skill IDs)
    *   `is_missing` (Boolean, whether this skill is needed to reach user's goal)
    *   `contributed_notes` (JSON array of note IDs that contribute to this skill)
    *   `related_quests` (JSON array of quest IDs that contribute to this skill)
    *   `created_at`
    *   `updated_at`

**Notes & Arsenal System**

*   **Note**: Represents user-created notes in their "Arsenal" with rich text capabilities.
    *   `note_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `course_id` (Foreign Key to `Course`, optional)
    *   `quest_id` (Foreign Key to `Quest`, optional)
    *   `skill_id` (Foreign Key to `Skill`, optional)
    *   `title`
    *   `content` (Rich text content with Notion-like functionality)
    *   `content_type` (e.g., 'Rich Text', 'Markdown', 'HTML')
    *   `organization_structure` (JSON object for flexible content organization)
    *   `created_at`
    *   `updated_at`
    *   `tags` (JSON array of tag strings)
    *   `is_arsenal_item` (Boolean, whether this is part of the user's Arsenal)
    *   `contributes_to_skills` (JSON array of skill IDs this note contributes to)

**Gamified Assessment System**

*   **BossFight**: Represents a gamified mock exam with difficulty-based scoring and Unity WebGL integration.
    *   `boss_fight_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `quest_id` (Foreign Key to `Quest`, optional)
    *   `title`
    *   `description`
    *   `difficulty` (e.g., 'Easy', 'Medium', 'Hard')
    *   `progress_status` (e.g., 'Not Started', 'In Progress', 'Completed', 'Failed')
    *   `generation_source` (e.g., 'User Input', 'AI Generated')
    *   `upcoming_test_date` (Optional, from user input)
    *   `unity_scene_id` (Unity scene identifier for boss fight)
    *   `unity_game_state` (JSON object storing Unity game progress)
    *   `unity_session_id` (Session ID for Unity WebGL instance)
    *   `interactive_elements` (JSON array of Unity interactive components)
    *   `created_at`
    *   `started_at`
    *   `completed_at`
    *   `score`
    *   `max_score`
    *   `weighted_score` (Score calculated with difficulty weighting)
    *   `experience_points` (XP reward for completion)
    *   `required_skills` (JSON array of required skill IDs)

*   **Question**: Represents a question in a boss fight with difficulty-based scoring.
    *   `question_id` (Primary Key)
    *   `boss_fight_id` (Foreign Key to `BossFight`)
    *   `content`
    *   `question_type` (e.g., 'Multiple Choice', 'True/False', 'Short Answer')
    *   `difficulty_level` (e.g., 'Easy', 'Medium', 'Hard')
    *   `base_points` (Base points for correct answer)
    *   `difficulty_multiplier` (Multiplier based on difficulty)
    *   `options` (JSON array of answer options for multiple choice)
    *   `correct_answer`
    *   `explanation` (Optional explanation for the answer)
    *   `created_at`

*   **UserAnswer**: Represents a user's answer to a question.
    *   `user_answer_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `question_id` (Foreign Key to `Question`)
    *   `boss_fight_id` (Foreign Key to `BossFight`)
    *   `answer`
    *   `is_correct`
    *   `points_earned` (Points earned including difficulty weighting)
    *   `submitted_at`

**Leaderboard & Ranking System**

*   **Leaderboard**: Represents a ranking system for users.
    *   `leaderboard_id` (Primary Key)
    *   `name`
    *   `description`
    *   `leaderboard_type` (e.g., 'Class-specific', 'Global', 'PvP Event')
    *   `class_id` (Foreign Key to `Class`, optional)
    *   `created_at`
    *   `updated_at`
    *   `is_active` (Boolean)

*   **LeaderboardEntry**: Represents a user's position on a leaderboard.
    *   `leaderboard_entry_id` (Primary Key)
    *   `leaderboard_id` (Foreign Key to `Leaderboard`)
    *   `user_id` (Foreign Key to `User`)
    *   `score`
    *   `rank`
    *   `updated_at`

**Notification System**

*   **Notification**: Represents system notifications for users about quest changes and learning paths.
    *   `notification_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `title`
    *   `content`
    *   `notification_type` (e.g., 'Quest Update', 'New Learning Path', 'Achievement', 'Quest Line Change')
    *   `related_entity_type` (e.g., 'Quest', 'QuestLine', 'Skill')
    *   `related_entity_id` (ID of the related entity)
    *   `is_read`
    *   `created_at`
    *   `expires_at` (Optional)
    *   `action_required` (Boolean, whether user action is needed)

### Phase 2: Social & Extension MVP

**Browser Extension Integration**

*   **BrowserExtensionData**: Represents data extracted by the browser extension from university portals and web pages.
    *   `browser_extension_data_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `source_url`
    *   `source_domain`
    *   `data_type` (e.g., 'Syllabus', 'GPA', 'Timetable', 'Exam Schedule', 'Web Content')
    *   `content`
    *   `structured_content` (JSON object with organized extracted data)
    *   `extraction_method` (e.g., 'Auto Scan', 'Manual Trigger')
    *   `extracted_at`
    *   `processed_at`
    *   `processing_status`
    *   `auto_organized` (Boolean, whether content was automatically organized)

*   **ExtractedNote**: Represents a note automatically created from extracted web content.
    *   `extracted_note_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `browser_extension_data_id` (Foreign Key to `BrowserExtensionData`)
    *   `title`
    *   `content`
    *   `category`
    *   `auto_categorized` (Boolean, whether category was automatically assigned)
    *   `tags` (JSON array of tag strings)
    *   `created_at`
    *   `updated_at`
    *   `is_arsenal_item` (Boolean)

*   **HighlightTrigger**: Represents highlighted text that triggers Arsenal popup.
    *   `highlight_trigger_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `highlighted_text`
    *   `source_url`
    *   `triggered_at`
    *   `relevant_notes` (JSON array of note IDs that were shown)
    *   `user_action` (e.g., 'Viewed', 'Clicked', 'Ignored')

**Party System (Study Groups)**

*   **Party**: Represents a study group created by users.
    *   `party_id` (Primary Key)
    *   `name`
    *   `description`
    *   `created_at`
    *   `updated_at`
    *   `activation_status` (e.g., 'Active', 'Archived')
    *   `join_type` (e.g., 'Invite Only', 'Open')
    *   `max_members` (Optional)
    *   `party_rules` (JSON object with configurable rules)
    *   `default_permissions` (JSON object with default member permissions)

*   **PartyMembership**: A junction table to manage the many-to-many relationship between `User` and `Party`.
    *   `party_membership_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `party_id` (Foreign Key to `Party`)
    *   `is_creator` (Boolean)
    *   `is_party_leader` (Boolean)
    *   `permission_level` (e.g., 'View', 'Comment', 'Edit')
    *   `granular_permissions` (JSON object with specific permissions)
    *   `joined_at`
    *   `account_status` (e.g., 'Active', 'Inactive')

*   **PartyInvitation**: Represents an invitation to join a party.
    *   `party_invitation_id` (Primary Key)
    *   `party_id` (Foreign Key to `Party`)
    *   `inviter_id` (Foreign Key to `User`)
    *   `invitee_id` (Foreign Key to `User`)
    *   `invitation_method` (e.g., 'Direct Invite', 'Browse and Invite')
    *   `invitation_status` (e.g., 'Pending', 'Accepted', 'Declined')
    *   `created_at`
    *   `responded_at`
    *   `message` (Optional invitation message)

*   **UserBrowsing**: Represents users browsing other users for party invitations.
    *   `user_browsing_id` (Primary Key)
    *   `browser_id` (Foreign Key to `User`, the one browsing)
    *   `viewed_user_id` (Foreign Key to `User`, the one being viewed)
    *   `party_id` (Foreign Key to `Party`, context of browsing)
    *   `viewed_stats` (JSON object with stats that were visible)
    *   `browsed_at`
    *   `action_taken` (e.g., 'Invited', 'Skipped', 'Viewed Only')

**Party Stash (Shared Resources)**

*   **PartyStash**: Represents the shared resource space for a party.
    *   `party_stash_id` (Primary Key)
    *   `party_id` (Foreign Key to `Party`)
    *   `name`
    *   `description`
    *   `created_at`
    *   `updated_at`

*   **PartyStashItem**: Represents an item in the party stash with role-based permissions.
    *   `party_stash_item_id` (Primary Key)
    *   `party_stash_id` (Foreign Key to `PartyStash`)
    *   `contributor_id` (Foreign Key to `User`)
    *   `item_type` (e.g., 'Note', 'Link', 'File')
    *   `title`
    *   `content` (For notes) or `url` (For links) or `file_url` (For files)
    *   `created_at`
    *   `updated_at`
    *   `access_permissions` (JSON object defining who can view/edit/comment)

*   **PartyStashPermission**: Manages individual member permissions for stash items.
    *   `party_stash_permission_id` (Primary Key)
    *   `party_stash_item_id` (Foreign Key to `PartyStashItem`)
    *   `user_id` (Foreign Key to `User`)
    *   `permission_type` (e.g., 'View', 'Edit', 'Comment')
    *   `granted_by` (Foreign Key to `User`, the Party Leader)
    *   `granted_at`
    *   `is_active` (Boolean)

**Party Meetings & Study Sessions**

*   **PartyMeeting**: Represents a study session or meeting conducted by a party.
    *   `party_meeting_id` (Primary Key)
    *   `party_id` (Foreign Key to `Party`)
    *   `organizer_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `meeting_type` (e.g., 'Study Session', 'Project Discussion', 'Exam Prep', 'General Meeting')
    *   `scheduled_start_time`
    *   `scheduled_end_time`
    *   `actual_start_time` (Optional)
    *   `actual_end_time` (Optional)
    *   `meeting_status` (e.g., 'Scheduled', 'In Progress', 'Completed', 'Cancelled')
    *   `meeting_url` (Optional, for virtual meetings)
    *   `location` (Optional, for physical meetings)
    *   `created_at`
    *   `updated_at`

*   **MeetingAttendance**: Tracks attendance for party meetings.
    *   `meeting_attendance_id` (Primary Key)
    *   `party_meeting_id` (Foreign Key to `PartyMeeting`)
    *   `user_id` (Foreign Key to `User`)
    *   `attendance_status` (e.g., 'Invited', 'Confirmed', 'Attended', 'Absent', 'Late')
    *   `joined_at` (Optional)
    *   `left_at` (Optional)
    *   `notes` (Optional, personal notes about attendance)
    *   `created_at`
    *   `updated_at`

*   **MeetingRecord**: Represents the recorded content and activities during a party meeting captured by browser extension.
    *   `meeting_record_id` (Primary Key)
    *   `party_meeting_id` (Foreign Key to `PartyMeeting`)
    *   `recorder_id` (Foreign Key to `User`)
    *   `recording_method` (e.g., 'Browser Extension', 'Manual Entry', 'Audio Transcription')
    *   `raw_content` (Raw captured content - text, audio transcript, etc.)
    *   `structured_content` (JSON formatted structured data)
    *   `topics_discussed` (JSON array of main topics)
    *   `decisions_made` (JSON array of decisions and action items)
    *   `participants_active` (JSON array of active participant IDs)
    *   `shared_resources` (JSON array of resources shared during meeting)
    *   `recording_quality` (e.g., 'High', 'Medium', 'Low', 'Partial')
    *   `processing_status` (e.g., 'Raw', 'Processing', 'Processed', 'Failed')
    *   `created_at`
    *   `updated_at`
    *   `processed_at` (Optional)

*   **MeetingSummary**: Represents AI-generated or manual summaries of party meetings.
    *   `meeting_summary_id` (Primary Key)
    *   `party_meeting_id` (Foreign Key to `PartyMeeting`)
    *   `meeting_record_id` (Foreign Key to `MeetingRecord`, optional)
    *   `summary_type` (e.g., 'AI Generated', 'Manual', 'Collaborative')
    *   `created_by` (Foreign Key to `User`, optional for AI-generated)
    *   `title`
    *   `executive_summary` (Brief overview)
    *   `key_points` (JSON array of main discussion points)
    *   `action_items` (JSON array of tasks and assignments)
    *   `next_steps` (JSON array of planned follow-up actions)
    *   `resources_mentioned` (JSON array of links, documents, references)
    *   `study_materials_covered` (JSON array of topics/materials discussed)
    *   `questions_raised` (JSON array of unresolved questions)
    *   `summary_quality_score` (Optional, for AI-generated summaries)
    *   `is_approved` (Boolean, for review workflow)
    *   `approved_by` (Foreign Key to `User`, optional)
    *   `approved_at` (Optional)
    *   `created_at`
    *   `updated_at`

*   **MeetingActionItem**: Represents specific tasks or follow-ups assigned during meetings.
    *   `meeting_action_item_id` (Primary Key)
    *   `meeting_summary_id` (Foreign Key to `MeetingSummary`)
    *   `assigned_to` (Foreign Key to `User`)
    *   `assigned_by` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `priority` (e.g., 'High', 'Medium', 'Low')
    *   `due_date` (Optional)
    *   `completion_status` (e.g., 'Pending', 'In Progress', 'Completed', 'Cancelled')
    *   `completion_notes` (Optional)
    *   `completed_at` (Optional)
    *   `created_at`
    *   `updated_at`

### Phase 3: Educator & Admin Toolkit

**Guild System (Course Management)**

*   **Guild**: Represents a course managed by a Guild Master.
    *   `guild_id` (Primary Key)
    *   `name`
    *   `description`
    *   `course_code` (Optional)
    *   `semester` (Optional)
    *   `year` (Optional)
    *   `created_at`
    *   `updated_at`
    *   `activation_status` (e.g., 'Active', 'Archived')
    *   `join_type` (e.g., 'Invite Only', 'Open', 'Code Required')
    *   `join_code` (Optional)
    *   `aggregated_progress_visible` (Boolean, for dashboard)

*   **GuildMembership**: A junction table to manage the many-to-many relationship between `User` and `Guild`.
    *   `guild_membership_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `is_creator` (Boolean)
    *   `joined_at`
    *   `account_status` (e.g., 'Active', 'Inactive')
    *   `progress_visible` (Boolean, whether progress is visible to Guild Master)

*   **GuildMaterial**: Represents course materials uploaded by a Guild Master.
    *   `guild_material_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `uploader_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `material_type` (e.g., 'Syllabus', 'Lecture Notes', 'Assignment', 'Reference')
    *   `file_url`
    *   `can_supplement_quest_lines` (Boolean)
    *   `uploaded_at`
    *   `updated_at`

**Quest Template & Assignment System**

*   **GuildQuestTemplate**: Represents a quest template created by a Guild Master.
    *   `guild_quest_template_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `creator_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `quest_type` (e.g., 'Side Quest', 'Boss Fight', 'Training Drill')
    *   `difficulty` (e.g., 'Easy', 'Medium', 'Hard')
    *   `estimated_duration`
    *   `experience_points`
    *   `reference_materials` (JSON array of material IDs used for generation)
    *   `created_at`
    *   `updated_at`
    *   `is_ai_generated` (Boolean)
    *   `ai_generation_prompt` (Optional, prompt used for AI generation)
    *   `review_status` (e.g., 'Draft', 'Reviewed', 'Approved')

*   **GuildQuest**: Represents an instance of a quest assigned to guild members.
    *   `guild_quest_id` (Primary Key)
    *   `guild_quest_template_id` (Foreign Key to `GuildQuestTemplate`)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `assigner_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `creation_status` (e.g., 'Draft', 'Published', 'Archived')
    *   `assignment_scope` (e.g., 'Entire Course', 'Selected Students')
    *   `start_date`
    *   `due_date`
    *   `created_at`
    *   `updated_at`

*   **GuildQuestAssignment**: Represents the assignment of a guild quest to a specific user.
    *   `guild_quest_assignment_id` (Primary Key)
    *   `guild_quest_id` (Foreign Key to `GuildQuest`)
    *   `user_id` (Foreign Key to `User`)
    *   `progress_status` (e.g., 'Not Started', 'In Progress', 'Completed', 'Failed')
    *   `assigned_at`
    *   `started_at`
    *   `completed_at`
    *   `score` (Optional)
    *   `feedback` (Optional feedback from Guild Master)

**Custom Skill Trees & Overlays**

*   **SkillTreeOverlay**: Represents a custom skill tree overlay created by a Guild Master.
    *   `skill_tree_overlay_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `creator_id` (Foreign Key to `User`)
    *   `name`
    *   `description`
    *   `overlay_purpose` (e.g., 'Progress Comparison', 'Course Requirements')
    *   `created_at`
    *   `updated_at`
    *   `creation_status` (e.g., 'Draft', 'Published', 'Archived')

*   **OverlaySkill**: Represents a skill in a custom skill tree overlay.
    *   `overlay_skill_id` (Primary Key)
    *   `skill_tree_overlay_id` (Foreign Key to `SkillTreeOverlay`)
    *   `name`
    *   `description`
    *   `required_level` (Required proficiency level)
    *   `icon_url`
    *   `position_x`
    *   `position_y`
    *   `prerequisites` (JSON array of prerequisite overlay skill IDs)
    *   `is_priority` (Boolean, whether this is a priority skill)

**Custom Boss Fights & Assessments**

*   **CustomBossFight**: Represents a custom boss fight created by a Guild Master.
    *   `custom_boss_fight_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `creator_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `challenge_description`
    *   `required_knowledge_nodes` (JSON array of skill/knowledge requirements)
    *   `optional_rewards` (JSON object with reward details)
    *   `deadline`
    *   `difficulty` (e.g., 'Easy', 'Medium', 'Hard')
    *   `max_attempts` (Optional)
    *   `created_at`
    *   `updated_at`
    *   `publication_status` (e.g., 'Draft', 'Published', 'Archived')

*   **CustomBossFightAssignment**: Represents assignment of custom boss fights to students.
    *   `custom_boss_fight_assignment_id` (Primary Key)
    *   `custom_boss_fight_id` (Foreign Key to `CustomBossFight`)
    *   `user_id` (Foreign Key to `User`)
    *   `assigned_at`
    *   `attempts_made`
    *   `best_score`
    *   `completion_status` (e.g., 'Not Started', 'In Progress', 'Completed', 'Failed')
    *   `rewards_earned` (JSON object with earned rewards)

**Lore & Narrative System**

*   **LoreFragment**: Represents a narrative description for a course topic.
    *   `lore_fragment_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `creator_id` (Foreign Key to `User`)
    *   `topic`
    *   `academic_concept` (The actual academic concept being described)
    *   `narrative_content` (The thematic narrative description)
    *   `connection_explanation` (How the narrative connects to the academic concept)
    *   `created_at`
    *   `updated_at`
    *   `is_active` (Boolean)

**Curriculum Alchemy & Module Management**

*   **CurriculumModule**: Represents a course module or knowledge node.
    *   `curriculum_module_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `name`
    *   `description`
    *   `module_type` (e.g., 'Lecture', 'Lab', 'Assignment', 'Knowledge Node')
    *   `prerequisites` (JSON array of prerequisite module IDs)
    *   `learning_objectives` (JSON array of learning objectives)
    *   `created_at`
    *   `updated_at`

*   **HybridQuest**: Represents a quest generated from merged curriculum modules.
    *   `hybrid_quest_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `creator_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `source_modules` (JSON array of merged module IDs)
    *   `composite_topics` (JSON array of combined topics)
    *   `alchemy_method` (Description of how modules were combined)
    *   `created_at`
    *   `updated_at`
    *   `publication_status` (e.g., 'Draft', 'Published', 'Archived')

**Guild Buffs & Collective Achievements**

*   **GuildBuff**: Represents a temporary boost or reward for guild members.
    *   `guild_buff_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `creator_id` (Foreign Key to `User`)
    *   `name`
    *   `description`
    *   `buff_type` (e.g., 'XP Boost', 'Reward', 'Achievement')
    *   `activation_condition` (JSON object describing the collective achievement needed)
    *   `current_progress` (JSON object tracking progress toward condition)
    *   `buff_effect` (JSON object describing the buff effect)
    *   `stat_boosts` (JSON object with temporary stat modifications)
    *   `start_date`
    *   `end_date`
    *   `activation_status` (e.g., 'Pending', 'Active', 'Inactive', 'Expired')
    *   `activated_at` (Optional)

**System Administration**

*   **SystemHealth**: Represents the health status of the application.
    *   `system_health_id` (Primary Key)
    *   `component` (e.g., 'API', 'Database', 'AI Service', 'RabbitMQ')
    *   `system_status` (e.g., 'Healthy', 'Degraded', 'Down')
    *   `details`
    *   `metrics` (JSON object with performance metrics)
    *   `checked_at`
    *   `alert_threshold_breached` (Boolean)

*   **GlobalEvent**: Represents a platform-wide event triggered by a Game Master.
    *   `global_event_id` (Primary Key)
    *   `creator_id` (Foreign Key to `User`, the Game Master)
    *   `name`
    *   `description`
    *   `event_type` (e.g., 'Bonus XP', 'Challenge', 'Invasion', 'Mini-Boss Challenge')
    *   `effect` (JSON object describing the event effect)
    *   `participation_scope` (e.g., 'All Players', 'All Guilds', 'Specific Guilds')
    *   `start_date`
    *   `end_date`
    *   `activation_status` (e.g., 'Scheduled', 'Active', 'Completed', 'Cancelled')

*   **EventScript**: Represents a scheduled platform-wide event.
    *   `event_script_id` (Primary Key)
    *   `creator_id` (Foreign Key to `User`, the Game Master)
    *   `name`
    *   `description`
    *   `event_type` (e.g., 'Challenge', 'PvP Event', 'Coordinated Challenge')
    *   `configuration` (JSON object with event parameters)
    *   `participation_conditions` (JSON object with conditions)
    *   `coordination_requirements` (JSON object for coordinated events)
    *   `schedule_date`
    *   `activation_status` (e.g., 'Scheduled', 'Active', 'Completed', 'Cancelled')

**Moderation & Dispute Resolution**

*   **PvPDispute**: Represents a dispute in PvP events.
    *   `pvp_dispute_id` (Primary Key)
    *   `pvp_event_id` (Foreign Key to `PvPEvent`)
    *   `reporter_id` (Foreign Key to `User`)
    *   `reported_user_id` (Foreign Key to `User`)
    *   `dispute_type` (e.g., 'Unfair Score', 'Cheating', 'Technical Issue')
    *   `description`
    *   `evidence` (JSON object with supporting evidence)
    *   `reported_at`
    *   `resolution_status` (e.g., 'Pending', 'Under Review', 'Resolved')
    *   `resolution` (Optional resolution details)
    *   `resolved_by` (Foreign Key to `User`, the Game Master)
    *   `resolved_at`

*   **AbuseReport**: Represents a report of inappropriate content or behavior.
    *   `abuse_report_id` (Primary Key)
    *   `reporter_id` (Foreign Key to `User`)
    *   `reported_user_id` (Foreign Key to `User`, optional)
    *   `reported_content_type` (e.g., 'User', 'Note', 'Party', 'Message')
    *   `reported_content_id`
    *   `abuse_type` (e.g., 'Inappropriate Content', 'Harassment', 'Spam')
    *   `reason`
    *   `details`
    *   `reported_at`
    *   `processing_status` (e.g., 'Pending', 'Processing', 'Completed')
    *   `moderation_action` (e.g., 'Warning', 'Temporary Ban', 'Permanent Ban', 'Content Removal')
    *   `resolution`
    *   `resolved_by` (Foreign Key to `User`, the Game Master)
    *   `resolved_at`

**Analytics Dashboard**

*   **AnalyticsDashboard**: Represents a comprehensive analytics dashboard for Game Masters.
    *   `analytics_dashboard_id` (Primary Key)
    *   `name`
    *   `description`
    *   `dashboard_type` (e.g., 'Feature Usage', 'User Cohort', 'Platform Health', 'Quest Popularity', 'Marketplace Analytics')
    *   `configuration` (JSON object with dashboard settings)
    *   `tracked_metrics` (JSON array of KPIs being monitored)
    *   `created_at`
    *   `updated_at`

*   **AnalyticsMetric**: Represents individual metrics tracked by the analytics system.
    *   `analytics_metric_id` (Primary Key)
    *   `analytics_dashboard_id` (Foreign Key to `AnalyticsDashboard`)
    *   `metric_name`
    *   `metric_type` (e.g., 'Quest Popularity', 'User Cohort', 'Feature Usage', 'Completion Rate')
    *   `metric_value`
    *   `measurement_period` (e.g., 'Daily', 'Weekly', 'Monthly')
    *   `recorded_at`
    *   `metadata` (JSON object with additional metric details)

*   **SeasonalLoreUpdate**: Represents a major update to the game world and lore.
    *   `seasonal_lore_update_id` (Primary Key)
    *   `creator_id` (Foreign Key to `User`, the Game Master)
    *   `title`
    *   `description`
    *   `content` (Rich text content describing the update)
    *   `world_map_updates` (JSON object with map changes)
    *   `new_skill_paths` (JSON array of new skill paths)
    *   `new_factions` (JSON array of new factions)
    *   `new_threats` (JSON array of new threats)
    *   `semester_context` (Which semester this update is for)
    *   `release_date`
    *   `creation_status` (e.g., 'Draft', 'Published', 'Archived')

### Phase 4: Advanced Social & Collaboration Features

**Advanced Party Management & Analytics**

*   **PartyAnalytics**: Represents analytics data for party performance and engagement.
    *   `party_analytics_id` (Primary Key)
    *   `party_id` (Foreign Key to `Party`)
    *   `analytics_period` (e.g., 'Daily', 'Weekly', 'Monthly')
    *   `member_engagement_scores` (JSON object with member engagement metrics)
    *   `study_session_count`
    *   `total_study_hours`
    *   `average_session_duration`
    *   `completion_rates` (JSON object with quest/task completion rates)
    *   `collaboration_score` (Overall party collaboration effectiveness)
    *   `generated_at`
    *   `period_start_date`
    *   `period_end_date`

*   **PartyRole**: Represents custom roles within parties with specific permissions.
    *   `party_role_id` (Primary Key)
    *   `party_id` (Foreign Key to `Party`)
    *   `role_name`
    *   `role_description`
    *   `permissions` (JSON object with permission settings)
    *   `can_schedule_sessions` (Boolean)
    *   `can_manage_members` (Boolean)
    *   `can_edit_shared_content` (Boolean)
    *   `can_view_analytics` (Boolean)
    *   `created_at`
    *   `updated_at`

*   **StudySessionCoordination**: Represents real-time coordination data for study sessions.
    *   `session_coordination_id` (Primary Key)
    *   `party_id` (Foreign Key to `Party`)
    *   `session_id` (Unique session identifier)
    *   `coordinator_id` (Foreign Key to `User`)
    *   `session_type` (e.g., 'Live Study', 'Virtual Meeting', 'Collaborative Work')
    *   `active_participants` (JSON array of currently active participant IDs)
    *   `session_status` (e.g., 'Starting', 'Active', 'Break', 'Ending')
    *   `shared_resources` (JSON array of shared documents/links)
    *   `real_time_notes` (JSON object with collaborative notes)
    *   `voice_channel_active` (Boolean)
    *   `screen_sharing_active` (Boolean)
    *   `recording_enabled` (Boolean)
    *   `started_at`
    *   `last_activity_at`

**Social Learning Features**

*   **StudyParty**: Represents temporary collaborative learning sessions.
    *   `study_party_id` (Primary Key)
    *   `creator_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `study_topic`
    *   `max_participants`
    *   `session_type` (e.g., 'Open Study', 'Focused Topic', 'Exam Prep')
    *   `scheduled_start_time`
    *   `scheduled_duration`
    *   `actual_start_time` (Optional)
    *   `actual_end_time` (Optional)
    *   `has_shared_whiteboard` (Boolean)
    *   `has_voice_chat` (Boolean)
    *   `quest_sync_enabled` (Boolean, for synchronized quest progress)
    *   `created_at`
    *   `session_status` (e.g., 'Scheduled', 'Active', 'Completed', 'Cancelled')

*   **StudyPartyParticipant**: Junction table for study party participation.
    *   `study_party_participant_id` (Primary Key)
    *   `study_party_id` (Foreign Key to `StudyParty`)
    *   `user_id` (Foreign Key to `User`)
    *   `joined_at`
    *   `left_at` (Optional)
    *   `participation_status` (e.g., 'Joined', 'Active', 'Left', 'Kicked')
    *   `contribution_score` (Optional, based on participation)

*   **SharedWhiteboard**: Represents collaborative whiteboard sessions.
    *   `shared_whiteboard_id` (Primary Key)
    *   `study_party_id` (Foreign Key to `StudyParty`)
    *   `whiteboard_data` (JSON object with whiteboard content)
    *   `created_at`
    *   `last_updated`
    *   `final_snapshot` (JSON object with final state)

**Competitive Learning & Knowledge Duels**

*   **KnowledgeDuel**: Represents real-time competitive quizzes or problem-solving challenges.
    *   `knowledge_duel_id` (Primary Key)
    *   `challenger_id` (Foreign Key to `User`)
    *   `opponent_id` (Foreign Key to `User`)
    *   `duel_type` (e.g., 'Quick Quiz', 'Problem Solving', 'Speed Challenge', 'Topic Mastery')
    *   `topic` (Subject area for the duel)
    *   `difficulty_level` (e.g., 'Easy', 'Medium', 'Hard')
    *   `time_limit` (Duration in minutes)
    *   `challenger_score`
    *   `opponent_score`
    *   `winner_id` (Foreign Key to `User`, optional)
    *   `xp_reward`
    *   `cosmetic_rewards` (JSON array of cosmetic rewards)
    *   `spectator_count` (Number of users watching)
    *   `duel_recording_url` (Optional recording for replay)
    *   `started_at`
    *   `completed_at`
    *   `duel_status` (e.g., 'Challenged', 'Accepted', 'In Progress', 'Completed', 'Declined')

*   **CompetitiveTournament**: Represents tournament-style competitions.
    *   `tournament_id` (Primary Key)
    *   `name`
    *   `description`
    *   `tournament_type` (e.g., 'Single Elimination', 'Round Robin', 'Swiss System')
    *   `topic_focus`
    *   `max_participants`
    *   `entry_requirements` (JSON object with participation requirements)
    *   `prize_pool` (JSON object with rewards)
    *   `registration_start`
    *   `registration_end`
    *   `tournament_start`
    *   `tournament_end`
    *   `current_round`
    *   `tournament_status` (e.g., 'Registration', 'Active', 'Completed', 'Cancelled')
    *   `created_at`

*   **TournamentParticipant**: Junction table for tournament participation.
    *   `tournament_participant_id` (Primary Key)
    *   `tournament_id` (Foreign Key to `CompetitiveTournament`)
    *   `user_id` (Foreign Key to `User`)
    *   `registration_date`
    *   `current_rank`
    *   `total_score`
    *   `matches_played`
    *   `matches_won`
    *   `participation_status` (e.g., 'Registered', 'Active', 'Eliminated', 'Withdrawn')

*   **PeerTeaching**: Represents peer-to-peer learning sessions and tutorials.
    *   `peer_teaching_id` (Primary Key)
    *   `teacher_id` (Foreign Key to `User`)
    *   `student_id` (Foreign Key to `User`, optional for group sessions)
    *   `session_type` (e.g., 'One-on-One', 'Group Session', 'Tutorial Creation')
    *   `title`
    *   `description`
    *   `content` (Rich text content of the lesson)
    *   `teaching_format` (e.g., 'Written Tutorial', 'Video', 'Interactive Demo', 'Live Session')
    *   `topic`
    *   `difficulty_level` (e.g., 'Beginner', 'Intermediate', 'Advanced')
    *   `estimated_duration`
    *   `scheduled_time` (For live sessions)
    *   `mentor_xp_earned`
    *   `student_xp_earned`
    *   `view_count`
    *   `rating_average`
    *   `created_at`
    *   `updated_at`
    *   `session_status` (e.g., 'Scheduled', 'Active', 'Completed', 'Cancelled')
    *   `publication_status` (e.g., 'Draft', 'Published', 'Archived')

*   **PeerTeachingFeedback**: Represents feedback and ratings for peer teaching sessions.
    *   `peer_teaching_feedback_id` (Primary Key)
    *   `peer_teaching_id` (Foreign Key to `PeerTeaching`)
    *   `reviewer_id` (Foreign Key to `User`)
    *   `reviewer_type` (e.g., 'Student', 'Peer', 'System')
    *   `rating` (1-5 stars)
    *   `feedback_text` (Optional text feedback)
    *   `helpful_votes` (Number of users who found this feedback helpful)
    *   `improvement_suggestions` (JSON array of suggestions)
    *   `created_at`

**Enhanced Browser Extension Integration**

*   **WebContentExtraction**: Represents advanced web content extraction and analysis.
    *   `web_content_extraction_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `source_url`
    *   `page_title`
    *   `extracted_content` (JSON object with structured content)
    *   `content_type` (e.g., 'Article', 'Video', 'PDF', 'Academic Paper', 'Tutorial')
    *   `ai_analysis` (JSON object with AI-generated insights)
    *   `auto_tags` (JSON array of automatically generated tags)
    *   `difficulty_assessment` (AI-assessed difficulty level)
    *   `related_skills` (JSON array of related skill tree nodes)
    *   `extraction_quality_score` (Quality assessment of extraction)
    *   `extracted_at`
    *   `processing_status` (e.g., 'Pending', 'Processed', 'Failed')

*   **ContextualAssistance**: Represents contextual learning assistance data.
    *   `contextual_assistance_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `trigger_content` (Text that triggered the assistance)
    *   `assistance_type` (e.g., 'Related Notes', 'Skill Connection', 'Learning Suggestion')
    *   `suggested_content` (JSON object with suggested Arsenal content)
    *   `skill_connections` (JSON array of related skill tree connections)
    *   `confidence_score` (AI confidence in suggestions)
    *   `user_interaction` (e.g., 'Viewed', 'Clicked', 'Dismissed', 'Saved')
    *   `effectiveness_rating` (User rating of assistance quality)
    *   `triggered_at`
    *   `interaction_at`

*   **CrossPlatformSync**: Represents synchronization data across devices and platforms.
    *   `sync_session_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `device_identifier`
    *   `platform_type` (e.g., 'Web', 'Mobile', 'Browser Extension')
    *   `sync_type` (e.g., 'Full Sync', 'Incremental', 'Conflict Resolution')
    *   `data_types_synced` (JSON array of synced data types)
    *   `sync_status` (e.g., 'In Progress', 'Completed', 'Failed', 'Partial')
    *   `conflicts_detected` (JSON array of sync conflicts)
    *   `conflicts_resolved` (JSON array of resolved conflicts)
    *   `sync_started_at`
    *   `sync_completed_at`
    *   `data_size_synced` (Size in bytes)

**Global Leaderboards & Rankings**

*   **GlobalLeaderboard**: Represents platform-wide ranking systems.
    *   `global_leaderboard_id` (Primary Key)
    *   `name`
    *   `description`
    *   `ranking_metric` (e.g., 'Total XP', 'Quest Completion', 'Peer Teaching', 'Knowledge Duels Won')
    *   `ranking_period` (e.g., 'All Time', 'Monthly', 'Seasonal')
    *   `privacy_level` (e.g., 'Public', 'Opt-in', 'Anonymous')
    *   `created_at`
    *   `updated_at`
    *   `is_active` (Boolean)

*   **GlobalLeaderboardEntry**: Represents user positions on global leaderboards.
    *   `global_leaderboard_entry_id` (Primary Key)
    *   `global_leaderboard_id` (Foreign Key to `GlobalLeaderboard`)
    *   `user_id` (Foreign Key to `User`)
    *   `score`
    *   `rank`
    *   `privacy_setting` (User's privacy preference for this leaderboard)
    *   `last_updated`

**Seasonal Events & Community**

*   **SeasonalEvent**: Represents limited-time challenges or themed learning experiences.
    *   `seasonal_event_id` (Primary Key)
    *   `name`
    *   `description`
    *   `theme` (e.g., 'Halloween Study Challenge', 'Spring Learning Festival')
    *   `event_type` (e.g., 'Challenge', 'Community Goal', 'Special Quest Line')
    *   `participation_requirements` (JSON object with requirements)
    *   `exclusive_rewards` (JSON array of special rewards)
    *   `community_goal` (Optional community-wide objective)
    *   `current_progress` (Progress toward community goal)
    *   `start_date`
    *   `end_date`
    *   `event_status` (e.g., 'Upcoming', 'Active', 'Completed', 'Cancelled')

*   **SeasonalEventParticipant**: Junction table for seasonal event participation.
    *   `seasonal_event_participant_id` (Primary Key)
    *   `seasonal_event_id` (Foreign Key to `SeasonalEvent`)
    *   `user_id` (Foreign Key to `User`)
    *   `participation_score`
    *   `rewards_earned` (JSON array of earned rewards)
    *   `joined_at`
    *   `participation_status` (e.g., 'Active', 'Completed', 'Dropped Out')

*   **LearningCircle**: Represents persistent, topic-focused communities.
    *   `learning_circle_id` (Primary Key)
    *   `name`
    *   `description`
    *   `topic_focus`
    *   `circle_type` (e.g., 'Study Group', 'Project Team', 'Discussion Forum')
    *   `creator_id` (Foreign Key to `User`)
    *   `max_members` (Optional)
    *   `join_requirements` (JSON object with joining criteria)
    *   `activity_level` (e.g., 'High', 'Medium', 'Low')
    *   `created_at`
    *   `updated_at`
    *   `circle_status` (e.g., 'Active', 'Archived')

*   **LearningCircleMembership**: Junction table for learning circle participation.
    *   `learning_circle_membership_id` (Primary Key)
    *   `learning_circle_id` (Foreign Key to `LearningCircle`)
    *   `user_id` (Foreign Key to `User`)
    *   `role` (e.g., 'Member', 'Moderator', 'Creator')
    *   `joined_at`
    *   `membership_status` (e.g., 'Active', 'Inactive')

### Phase 5: Marketplace & Economy

**Marketplace System**

*   **Marketplace**: Represents the platform's marketplace for study materials.
    *   `marketplace_id` (Primary Key)
    *   `name`
    *   `description`
    *   `created_at`
    *   `updated_at`
    *   `system_status` (e.g., 'Active', 'Maintenance')

*   **MarketplaceItem**: Represents an item listed in the marketplace.
    *   `marketplace_item_id` (Primary Key)
    *   `seller_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `item_type` (e.g., 'Note', 'File', 'Knowledge Pack')
    *   `content_url` (URL to the item content)
    *   `preview_url` (URL to a preview of the item)
    *   `price` (In-game currency amount)
    *   `real_money_price` (Optional, for real money transactions)
    *   `created_at`
    *   `updated_at`
    *   `market_status` (e.g., 'Listed', 'Sold', 'Removed')
    *   `tags` (JSON array of tag strings)
    *   `download_count`
    *   `rating_average`
    *   `category` (e.g., 'Computer Science', 'Mathematics', 'Physics')

*   **MarketplaceItemRating**: Represents a rating and review for a marketplace item.
    *   `marketplace_item_rating_id` (Primary Key)
    *   `marketplace_item_id` (Foreign Key to `MarketplaceItem`)
    *   `user_id` (Foreign Key to `User`)
    *   `rating` (1-5 stars)
    *   `review` (Text review)
    *   `helpful_votes` (Number of users who found this review helpful)
    *   `created_at`
    *   `updated_at`

**Currency & Transaction System**

*   **Currency**: Represents the in-game currency system.
    *   `currency_id` (Primary Key)
    *   `name` (e.g., 'Gold Coins', 'Knowledge Points')
    *   `symbol` (e.g., 'GC', 'KP')
    *   `description`
    *   `exchange_rate` (Rate for real money conversion)
    *   `created_at`
    *   `updated_at`

*   **UserWallet**: Represents a user's currency balance.
    *   `user_wallet_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `currency_id` (Foreign Key to `Currency`)
    *   `balance`
    *   `total_earned` (Lifetime earnings)
    *   `total_spent` (Lifetime spending)
    *   `created_at`
    *   `updated_at`

*   **Transaction**: Represents a currency transaction between users or with the system.
    *   `transaction_id` (Primary Key)
    *   `sender_wallet_id` (Foreign Key to `UserWallet`, optional for system transactions)
    *   `receiver_wallet_id` (Foreign Key to `UserWallet`)
    *   `amount`
    *   `transaction_type` (e.g., 'Purchase', 'Sale', 'Reward', 'System', 'Quest Completion')
    *   `marketplace_item_id` (Foreign Key to `MarketplaceItem`, optional)
    *   `quest_id` (Foreign Key to `Quest`, optional for quest rewards)
    *   `description`
    *   `created_at`
    *   `processing_status` (e.g., 'Pending', 'Completed', 'Failed', 'Refunded')
    *   `reference_id` (External transaction reference)

**Knowledge Repository**

*   **EternalCodex**: Represents the public repository of high-quality study materials.
    *   `eternal_codex_id` (Primary Key)
    *   `name`
    *   `description`
    *   `curation_criteria` (JSON object with quality standards)
    *   `created_at`
    *   `updated_at`

*   **EternalCodexEntry**: Represents an entry in the Eternal Codex.
    *   `eternal_codex_entry_id` (Primary Key)
    *   `eternal_codex_id` (Foreign Key to `EternalCodex`)
    *   `original_item_id` (Foreign Key to `MarketplaceItem`)
    *   `contributor_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `content_url`
    *   `ai_quality_score` (AI-assessed quality rating)
    *   `community_rating` (Community-assessed quality)
    *   `elevation_date` (When it was added to the Codex)
    *   `elevation_reason` (Why it was selected)
    *   `tags` (JSON array of tag strings)
    *   `access_count` (Number of times accessed)
    *   `last_updated`

*   **KnowledgePack**: Represents a curated bundle of study materials.
    *   `knowledge_pack_id` (Primary Key)
    *   `curator_id` (Foreign Key to `User`, can be AI)
    *   `title`
    *   `description`
    *   `theme` (e.g., 'Exam Preparation', 'Skill Building')
    *   `subject` (e.g., 'Computer Science', 'Mathematics')
    *   `difficulty_level` (e.g., 'Beginner', 'Intermediate', 'Advanced')
    *   `exam_type` (Optional, e.g., 'Midterm', 'Final', 'Certification')
    *   `estimated_study_time` (In hours)
    *   `created_at`
    *   `updated_at`
    *   `price` (In-game currency amount)
    *   `creation_status` (e.g., 'Draft', 'Published', 'Archived')
    *   `purchase_count`
    *   `rating_average`

*   **KnowledgePackItem**: A junction table to associate items with knowledge packs.
    *   `knowledge_pack_item_id` (Primary Key)
    *   `knowledge_pack_id` (Foreign Key to `KnowledgePack`)
    *   `item_type` (e.g., 'MarketplaceItem', 'EternalCodexEntry')
    *   `item_id` (Foreign Key to `MarketplaceItem` or `EternalCodexEntry`)
    *   `order_sequence` (For ordering items within the pack)
    *   `is_required` (Boolean, whether this item is essential)
    *   `added_at`

**Meta-Skills & Tagging**

*   **MetaSkill**: Represents a higher-level skill category for tagging materials.
    *   `meta_skill_id` (Primary Key)
    *   `name` (e.g., 'Critical Thinking', 'Synthesis', 'Memorization', 'Problem Solving')
    *   `description`
    *   `icon_url`
    *   `skill_category` (e.g., 'Cognitive', 'Technical', 'Social')
    *   `created_at`
    *   `updated_at`

*   **ItemMetaSkill**: A junction table to associate meta-skills with marketplace items or codex entries.
    *   `item_meta_skill_id` (Primary Key)
    *   `item_type` (e.g., 'MarketplaceItem', 'EternalCodexEntry', 'KnowledgePack')
    *   `item_id` (Foreign Key to the respective item type)
    *   `meta_skill_id` (Foreign Key to `MetaSkill`)
    *   `relevance_score` (How relevant the meta-skill is to the item, 1-10)
    *   `tagged_at`
    *   `tagged_by` (Foreign Key to `User`, can be AI)
    *   `confidence_level` (For AI-generated tags)

**Competitive Events & PvP**

*   **PvPEvent**: Represents a player-versus-player competition.
    *   `pvp_event_id` (Primary Key)
    *   `title`
    *   `description`
    *   `event_type` (e.g., 'Coding Challenge', 'CSS Battle', 'Algorithm Battle', 'Knowledge Quiz')
    *   `rules` (JSON object with event rules)
    *   `entry_requirements` (JSON object with participation requirements)
    *   `prize_pool` (JSON object with rewards)
    *   `start_date`
    *   `end_date`
    *   `registration_deadline`
    *   `activation_status` (e.g., 'Scheduled', 'Registration Open', 'Active', 'Completed', 'Cancelled')
    *   `max_participants`
    *   `current_participants`
    *   `created_by` (Foreign Key to `User`, Game Master)

*   **PvPParticipant**: A junction table to track participants in PvP events.
    *   `pvp_participant_id` (Primary Key)
    *   `pvp_event_id` (Foreign Key to `PvPEvent`)
    *   `user_id` (Foreign Key to `User`)
    *   `registration_date`
    *   `final_score`
    *   `final_rank`
    *   `submission_data` (JSON object with participant's submission)
    *   `participation_status` (e.g., 'Registered', 'Active', 'Completed', 'Disqualified', 'Withdrawn')
    *   `rewards_earned` (JSON array of earned rewards)

**Achievement System**

*   **Achievement**: Represents a badge or achievement that a user can earn.
    *   `achievement_id` (Primary Key)
    *   `name`
    *   `description`
    *   `icon_url`
    *   `achievement_type` (e.g., 'Milestone', 'Skill', 'Social', 'Challenge', 'Competitive')
    *   `achievement_category` (e.g., 'Learning', 'Community', 'Marketplace', 'PvP')
    *   `requirements` (JSON object with achievement requirements)
    *   `experience_points` (XP reward)
    *   `currency_reward` (Optional currency reward)
    *   `cosmetic_rewards` (JSON array of cosmetic unlocks)
    *   `rarity` (e.g., 'Common', 'Rare', 'Epic', 'Legendary')
    *   `is_hidden` (Boolean, whether achievement is visible before earning)
    *   `created_at`
    *   `updated_at`

*   **UserAchievement**: A junction table to track achievements earned by users.
    *   `user_achievement_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `achievement_id` (Foreign Key to `Achievement`)
    *   `earned_at`
    *   `progress_data` (JSON object with progress tracking data)
    *   `completion_percentage` (For progressive achievements)
    *   `is_showcased` (Boolean, whether user displays this achievement)

## Data Relationships and Constraints

### Core Relationships

*   A User can have multiple Courses, but each Course belongs to only one User.
*   A Course can have one Syllabus, and a Syllabus belongs to only one Course.
*   A User can have multiple AcademicDocuments.
*   A User can have multiple QuestLines, and each QuestLine can have multiple Quests.
*   A User can have multiple SkillTrees, and each SkillTree can have multiple Skills.
*   A User can create multiple Notes, which can be associated with Courses, Quests, or Skills.
*   A User can have multiple BossFights, and each BossFight can have multiple Questions.
*   A User has one UserProfile that extends their basic information with game-specific attributes.
*   A Route can have multiple SmallerMajors, representing specialized tracks within academic paths.

### Many-to-Many Relationships with Junction Tables

*   Users and Parties have a many-to-many relationship managed through the `PartyMembership` junction table.
*   Users and Party Meetings have a many-to-many relationship managed through the `MeetingAttendance` junction table.
*   Users and Guilds have a many-to-many relationship managed through the `GuildMembership` junction table.
*   Quests and Skills have a many-to-many relationship managed through the `QuestSkillRelationship` junction table.
*   Users and Achievements have a many-to-many relationship managed through the `UserAchievement` junction table.
*   Users and PvP Events have a many-to-many relationship managed through the `PvPParticipant` junction table.
*   Knowledge Packs and Marketplace Items have a many-to-many relationship managed through the `KnowledgePackItem` junction table.
*   Marketplace Items/Codex Entries and Meta Skills have a many-to-many relationship managed through the `ItemMetaSkill` junction table.
*   Users and Learning Circles have a many-to-many relationship managed through the `LearningCircleMembership` junction table.
*   Users and Study Parties have a many-to-many relationship managed through the `StudyPartyParticipant` junction table.
*   Users and Seasonal Events have a many-to-many relationship managed through the `SeasonalEventParticipant` junction table.

### Hierarchical and Unidirectional Relationships

*   A Party can have one PartyStash, which can contain multiple PartyStashItems.
*   A Party can have multiple PartyMeetings, each with potential MeetingRecords, Summaries, and ActionItems.
*   A Guild Master can create GuildQuestTemplates, which are instantiated as GuildQuests.
*   The EternalCodex contains CodexEntries, which reference MarketplaceItems.
*   Skills can reference other Skills as prerequisites.

### Ownership and Transaction Relationships

*   A User has one UserWallet per Currency type for Transactions.
*   Transactions can reference MarketplaceItems or Quests.
*   A User (seller) can list multiple MarketplaceItems, which can receive multiple ItemRatings.

### Administrative Relationships

*   A Game Master can create GlobalEvents, EventScripts, and SeasonalEvents.

### AI and Content Relationships

*   AI-generated content maintains references to its source materials.
*   Browser extension data flows from extraction to processing and integration.

## Data Migration and Versioning

*   The data model will be versioned.
*   Migration scripts will be created for major version changes.
*   A comprehensive backup and recovery strategy will be implemented.

## Data Security and Privacy

*   Sensitive user data will be encrypted at rest and in transit.
*   Access will be restricted by RBAC.
*   PII will be handled in compliance with relevant data protection regulations.
*   Data retention policies will be implemented.
*   Regular security audits will be conducted.

## Performance Considerations

*   Database indexes will be strategically created on frequently queried fields.
*   Query optimization strategies will be implemented.
*   Caching strategies will be implemented at multiple levels.
*   Database scaling strategies will be prepared for growth.

## Data Consistency and Integrity

*   Foreign key constraints will be enforced.
*   Check and unique constraints will be implemented.
*   Transactions will be used to ensure ACID properties.
*   Data validation will be implemented at both application and database levels.


## Scalability Architecture

*   The data model is designed to support horizontal scaling through:
    *   Microservices architecture with domain-specific databases
    *   Event-driven architecture for loose coupling between services
    *   Message queues (RabbitMQ) for asynchronous processing
*   Data partitioning strategies:
    *   User-based partitioning for user-specific data
    *   Time-based partitioning for historical data
    *   Feature-based partitioning for different functional domains
*   API design will follow RESTful principles with GraphQL for complex queries.
*   Real-time features will be implemented using WebSocket connections and event streaming.
*   Content delivery will be optimized through efficient file storage and retrieval.

## Detailed Data Validation Rules

### Core Entity Field Validation

**User Management Validation**
- `email`: RFC 5322 compliant, max 255 chars, unique globally, case-insensitive
- `username`: 3-30 chars, regex: `^[a-zA-Z0-9_-]+$`, unique globally, reserved words blocked
- `password`: Min 8 chars, must contain: uppercase, lowercase, digit, special char (!@#$%^&*)
- `character_name`: 2-50 chars, Unicode support, profanity filter, unique per user
- `birth_date`: Valid ISO 8601 date, age ≥13 years, not future date
- `profile_picture_url`: Valid HTTPS URL, max 500 chars, allowed domains whitelist
- `bio`: Max 500 chars, HTML sanitization, profanity filter
- `privacy_settings`: JSON schema validation with predefined structure
- `clerk_user_id`: External ID format validation, unique constraint

**Academic Document Validation**
- `title`: 1-200 chars, required, HTML entities escaped
- `file_url`: HTTPS URL, max 1000 chars, domain whitelist, file extension validation
- `file_size`: Max 50MB (52,428,800 bytes), positive integer
- `document_type`: Enum validation ('PDF', 'DOCX', 'TXT', 'MD', 'PPTX')
- `processing_status`: State machine validation with allowed transitions
- `extracted_text`: Max 1MB UTF-8 text, content sanitization
- `ai_analysis_result`: Valid JSON schema, confidence scores 0-1 range

**Quest System Validation**
- `title`: 5-200 chars, required, no leading/trailing whitespace
- `description`: 10-2000 chars, rich text validation, image URL validation
- `difficulty`: Enum ('Easy', 'Medium', 'Hard', 'Expert', 'Legendary')
- `experience_points`: 1-10000 range, integer, difficulty-based constraints
- `estimated_duration`: 5-480 minutes, integer, realistic time bounds
- `quest_type`: Predefined enum with extensibility support
- `prerequisites`: JSON array, circular dependency detection algorithm
- `completion_criteria`: JSON schema validation, required fields check
- `ai_generated_content`: Boolean flag with generation metadata

**Skill Tree Validation**
- `skill_name`: 2-100 chars, unique within tree, localization support
- `proficiency_level`: 0-100 integer, non-decreasing constraint
- `prerequisites`: JSON array, DAG validation, max depth limit (10 levels)
- `position_x`, `position_y`: Integer coordinates, canvas bounds (0-5000)
- `unlock_threshold`: 0-100 range, ≤ max_level, prerequisite consistency
- `skill_category`: Hierarchical taxonomy validation
- `icon_url`: Valid URL, SVG/PNG format, size limits (max 100KB)

### Advanced Validation Rules

**Business Logic Validation**
- Party membership: Max 10 active parties per user
- Quest prerequisites: Topological sort validation for dependency chains
- Skill unlocking: Prerequisite completion verification
- Experience caps: Daily (5000 XP), weekly (25000 XP) anti-gaming limits
- Currency transactions: Non-negative balance enforcement
- File uploads: Virus scanning integration, MIME type validation
- Content moderation: AI-powered inappropriate content detection
- Rate limiting: API endpoint specific limits (100-1000 req/min)

**Temporal Validation**
- `created_at`: Auto-generated UTC timestamp, immutable
- `updated_at`: Auto-updated on modification, ≥ created_at
- `due_date`: Must be future date, reasonable time bounds (max 1 year)
- `start_date` < `end_date`: Chronological order validation
- Session timestamps: Logical ordering, overlap detection
- Event scheduling: Conflict detection, timezone handling

**Data Integrity Constraints**
- Foreign key cascading: Defined cascade/restrict rules per relationship
- Soft delete implementation: `deleted_at` timestamp, filtered queries
- Audit trail: Change tracking for sensitive operations
- Concurrent modification: Optimistic locking with version numbers
- Data consistency: Cross-service transaction coordination

## Comprehensive Data Flow Specifications

### User Lifecycle Data Flows

**Registration & Onboarding Flow**
```
1. Initial Registration
   Input: {email, username, password, character_name, birth_date}
   Validation: Email uniqueness check, password strength validation
   Processing: Password hashing (bcrypt), verification token generation
   Storage: User record creation, email verification queue
   Output: {user_id, verification_token}, welcome email trigger
   
2. Email Verification
   Input: {verification_token}
   Validation: Token expiry check (24 hours), single-use verification
   Processing: Account activation, initial skill tree creation
   Storage: User status update, default preferences setup
   Output: Account activation confirmation, onboarding flow initiation
   
3. Profile Completion
   Input: {bio, profile_picture, privacy_settings, academic_info}
   Validation: Image format/size, content filtering, privacy options
   Processing: Image optimization, content moderation
   Storage: Profile data persistence, search index update
   Output: Complete profile, personalized dashboard setup
```

**Document Processing Pipeline**
```
1. Document Upload
   Input: {file_data, document_type, course_id, metadata}
   Validation: File type whitelist, size limits (50MB), virus scan
   Processing: File encryption, cloud storage upload (S3/Azure Blob)
   Storage: Document metadata, processing queue entry
   Output: {document_id, upload_url}, processing status notification
   
2. Content Extraction & Analysis
   Input: {document_id, file_url}
   Processing: 
     - OCR/text extraction (Tesseract/Azure Cognitive Services)
     - Content analysis (NLP, topic modeling)
     - Skill mapping (AI classification)
     - Quality assessment (readability, completeness)
   Storage: Extracted text, analysis results, skill associations
   Output: Processed content, skill tree updates, quest generation triggers
   
3. AI-Powered Quest Generation
   Input: {extracted_content, user_skill_level, learning_preferences}
   Processing:
     - Content chunking and difficulty analysis
     - Question generation (GPT-4, Claude)
     - Answer validation and scoring rubrics
     - Adaptive difficulty calibration
   Storage: Generated quests, difficulty metadata, success metrics
   Output: Personalized quest recommendations, skill progression updates
```

**Real-time Collaboration Flow**
```
1. Party Session Initialization
   Input: {party_id, session_type, invited_participants}
   Validation: Party membership verification, session limits
   Processing: WebSocket server allocation, shared state initialization
   Storage: Session metadata, participant tracking
   Output: Session credentials, real-time connection establishment
   
2. Synchronized Learning Activities
   Input: Real-time user actions via WebSocket
   Processing:
     - Operational transformation for concurrent edits
     - Conflict resolution algorithms
     - State synchronization across clients
     - Progress tracking and analytics
   Storage: Event sourcing, periodic state snapshots
   Output: Synchronized UI updates, progress notifications
   
3. Session Completion & Analytics
   Input: Session end trigger, final state
   Processing: 
     - Participation metrics calculation
     - Learning outcome assessment
     - Experience point distribution
     - Achievement unlock checks
   Storage: Session summary, individual progress updates
   Output: Session report, reward notifications, recommendation updates
```

### Assessment & Progression Flows

**Adaptive Assessment Pipeline**
```
1. Quest Attempt Initiation
   Input: {quest_id, user_id, attempt_context}
   Validation: Quest availability, prerequisite completion, attempt limits
   Processing: Personalized question selection, difficulty calibration
   Storage: Attempt record creation, analytics tracking
   Output: Customized quest instance, progress tracking initialization
   
2. Real-time Answer Processing
   Input: {question_id, user_response, response_time}
   Processing:
     - Answer validation and partial credit calculation
     - Response pattern analysis
     - Adaptive difficulty adjustment
     - Hint system activation
   Storage: Response data, performance metrics
   Output: Immediate feedback, next question selection
   
3. Skill Proficiency Assessment
   Input: Quest completion data, historical performance
   Processing:
     - Bayesian knowledge tracing
     - Skill mastery probability calculation
     - Learning curve analysis
     - Personalized recommendation generation
   Storage: Updated skill proficiency, learning analytics
   Output: Skill tree updates, next learning recommendations
```

**Gamification & Reward Flow**
```
1. Achievement Detection
   Input: User activity data, milestone triggers
   Processing: Achievement criteria evaluation, progress calculation
   Storage: Achievement progress updates, unlock notifications
   Output: Achievement notifications, reward distribution
   
2. Experience & Currency Management
   Input: Completed activities, performance metrics
   Processing: XP calculation, currency rewards, bonus multipliers
   Validation: Daily/weekly limits, anti-gaming measures
   Storage: User wallet updates, transaction records
   Output: Balance updates, leaderboard position changes
   
3. Social Recognition System
   Input: Achievement unlocks, milestone completions
   Processing: Social feed updates, peer notifications
   Storage: Activity feed, social interaction records
   Output: Social notifications, community engagement metrics
```

## API Integration Specifications

### Authentication & Security
- **JWT Implementation**: RS256 algorithm, 24-hour access tokens, 7-day refresh tokens
- **Rate Limiting**: Sliding window algorithm, per-endpoint limits, burst allowance
- **API Versioning**: Semantic versioning in URL path (/api/v1/), backward compatibility
- **Request Validation**: JSON Schema validation, input sanitization, CORS policies
- **Audit Logging**: Request/response logging, user action tracking, security events

### Data Exchange Standards
- **Response Format**: JSON:API specification compliance, consistent error formats
- **Pagination**: Cursor-based pagination for large datasets, configurable page sizes
- **Filtering & Sorting**: Query parameter standards, field selection, relationship inclusion
- **Caching Headers**: ETags for conditional requests, cache-control directives
- **Compression**: Gzip compression for responses >1KB, content negotiation

### Real-time Communication
- **WebSocket Protocol**: Socket.IO implementation, automatic reconnection, heartbeat monitoring
- **Event Broadcasting**: Room-based messaging, selective event subscription
- **Message Queuing**: RabbitMQ for async processing, dead letter queues, retry policies
- **Push Notifications**: Firebase/APNs integration, user preference management
- **Offline Synchronization**: Local storage queuing, conflict resolution on reconnection

## Performance & Scalability Specifications

### Database Optimization
- **Query Performance**: <200ms response time for 95th percentile, query plan optimization
- **Indexing Strategy**: Composite indexes for common queries, partial indexes for filtered data
- **Connection Management**: Connection pooling (max 100), connection lifecycle management
- **Read Scaling**: Read replicas for analytics, query routing based on operation type
- **Write Optimization**: Batch operations, async processing for non-critical updates

### Caching Architecture
- **Multi-level Caching**: L1 (application), L2 (Redis)
- **Cache Invalidation**: Event-driven invalidation, TTL-based expiration, cache warming
- **Session Management**: Redis-based session storage, session clustering
- **Static Asset Caching**: Server-side caching, versioned assets, cache busting
- **Query Result Caching**: Materialized views, computed aggregations

### Monitoring & Analytics
- **Performance Metrics**: Response times, throughput, error rates, resource utilization
- **Business Metrics**: User engagement, learning outcomes, feature adoption
- **Real-time Dashboards**: Grafana integration, alert thresholds, automated notifications
- **Log Aggregation**: Centralized logging (ELK stack), structured logging, log retention
- **Health Checks**: Service health endpoints, dependency monitoring, circuit breakers
  
