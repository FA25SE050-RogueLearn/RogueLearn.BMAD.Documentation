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
    *   `role` (e.g., 'Guild Master', 'Guide', 'Player', 'Party Leader', 'Game Master')
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

*   **BossFight**: Represents a gamified mock exam with difficulty-based scoring.
    *   `boss_fight_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `quest_id` (Foreign Key to `Quest`, optional)
    *   `title`
    *   `description`
    *   `difficulty` (e.g., 'Easy', 'Medium', 'Hard')
    *   `progress_status` (e.g., 'Not Started', 'In Progress', 'Completed', 'Failed')
    *   `generation_source` (e.g., 'User Input', 'AI Generated')
    *   `upcoming_test_date` (Optional, from user input)
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

*   **Guild**: Represents a course managed by a Guild Master (Lecturer).
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

**Guide System (Tutoring)**

*   **GuideAssignment**: Represents the assignment of a Guide (Tutor) to a specific Player.
    *   `guide_assignment_id` (Primary Key)
    *   `guide_id` (Foreign Key to `User`, the Guide)
    *   `player_id` (Foreign Key to `User`, the Player)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `assigner_id` (Foreign Key to `User`, the Guild Master)
    *   `assignment_scope` (e.g., 'Full Course', 'Specific Topics')
    *   `access_permissions` (JSON object defining what Guide can access)
    *   `assigned_at`
    *   `account_status` (e.g., 'Active', 'Inactive')

*   **TrainingDrill**: Represents a custom training exercise created by a Guide.
    *   `training_drill_id` (Primary Key)
    *   `guide_id` (Foreign Key to `User`)
    *   `player_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `training_drill_type` (e.g., 'Practice', 'Review', 'Challenge')
    *   `content`
    *   `target_micro_skill` (Specific skill being targeted)
    *   `is_graded` (Boolean, always false for training drills)
    *   `created_at`
    *   `assigned_at`
    *   `due_date`
    *   `progress_status` (e.g., 'Not Started', 'In Progress', 'Completed')

*   **TrainingGround**: Represents focused drills targeting single micro-skills.
    *   `training_ground_id` (Primary Key)
    *   `guide_id` (Foreign Key to `User`)
    *   `player_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `micro_skill_target` (e.g., 'recursion', 'essay structure', 'grammar rules')
    *   `drill_content`
    *   `expected_duration` (In minutes)
    *   `difficulty_level` (e.g., 'Beginner', 'Intermediate', 'Advanced')
    *   `created_at`
    *   `assigned_at`
    *   `completion_status` (e.g., 'Not Started', 'In Progress', 'Completed')

*   **MentorReflection**: Represents feedback or advice from a Guide to a Player.
    *   `mentor_reflection_id` (Primary Key)
    *   `guide_id` (Foreign Key to `User`)
    *   `player_id` (Foreign Key to `User`)
    *   `quest_id` (Foreign Key to `Quest`, optional)
    *   `skill_id` (Foreign Key to `Skill`, optional)
    *   `reflection_type` (e.g., 'Motivational', 'Advisory', 'Feedback')
    *   `content`
    *   `media_type` (e.g., 'Text', 'Voice', 'Video')
    *   `media_url` (For voice or video reflections)
    *   `is_tied_to_progress` (Boolean, whether tied to specific progress)
    *   `created_at`

**Student Analytics & Burnout Prevention**

*   **BurnoutForecast**: Represents a prediction of student disengagement.
    *   `burnout_forecast_id` (Primary Key)
    *   `player_id` (Foreign Key to `User`)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `risk_level` (e.g., 'Low', 'Medium', 'High')
    *   `risk_factors` (JSON array of disengagement indicators)
    *   `prediction_confidence` (Confidence score of the prediction)
    *   `recommended_interventions` (JSON array of suggested interventions)
    *   `generated_at`
    *   `expires_at`
    *   `is_active` (Boolean)

*   **ArsenalLoadout**: Represents a student's current Arsenal organization for Guide review.
    *   `arsenal_loadout_id` (Primary Key)
    *   `player_id` (Foreign Key to `User`)
    *   `guide_id` (Foreign Key to `User`)
    *   `snapshot_date`
    *   `note_organization` (JSON object showing how notes are organized)
    *   `skill_connections` (JSON object showing note-skill relationships)
    *   `missing_connections` (JSON array of suggested note-skill links)
    *   `guide_feedback` (Optional feedback from Guide)
    *   `feedback_date` (Optional)

**Mentor Archetype System**

*   **MentorArchetype**: Represents a thematic persona for a Guide.
    *   `mentor_archetype_id` (Primary Key)
    *   `name` (e.g., 'Sage', 'Trickster', 'Warrior')
    *   `description`
    *   `icon_url`
    *   `dialogue_tone`
    *   `advice_style` (JSON object describing advice characteristics)
    *   `available_cosmetics` (JSON array of cosmetic item IDs)
    *   `thematic_dialogue_options` (JSON array of dialogue templates)
    *   `created_at`
    *   `updated_at`

*   **GuideMentorArchetype**: A junction table to associate Guides with their chosen Mentor Archetypes.
    *   `guide_mentor_archetype_id` (Primary Key)
    *   `guide_id` (Foreign Key to `User`)
    *   `mentor_archetype_id` (Foreign Key to `MentorArchetype`)
    *   `selected_at`
    *   `is_active` (Boolean)
    *   `customizations` (JSON object with personalized archetype modifications)

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

### Phase 4: Living Ecosystem & Social

**Advanced AI Features**

*   **AISpellSuggestion**: Represents a study aid suggested by the AI from Arsenal scanning.
    *   `ai_spell_suggestion_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `spell_type` (e.g., 'Flashcard', 'Summary', 'Practice Quiz', 'Mind Map')
    *   `content`
    *   `source_note_ids` (JSON array of source note IDs from Arsenal)
    *   `ai_confidence_score` (Confidence in the suggestion quality)
    *   `suggested_at`
    *   `suggestion_status` (e.g., 'Suggested', 'Accepted', 'Rejected', 'Modified')
    *   `user_modifications` (JSON object with user changes to the suggestion)

*   **ClassRecording**: Represents an audio/video recording of a class.
    *   `class_recording_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `guild_id` (Foreign Key to `Guild`, optional)
    *   `title`
    *   `description`
    *   `file_url`
    *   `duration`
    *   `file_size`
    *   `recording_quality` (e.g., 'High', 'Medium', 'Low')
    *   `uploaded_at`
    *   `processing_status` (e.g., 'Pending', 'Processing', 'Completed', 'Failed')

*   **RecordingSummary**: Represents a text summary generated from a class recording.
    *   `recording_summary_id` (Primary Key)
    *   `class_recording_id` (Foreign Key to `ClassRecording`)
    *   `content`
    *   `key_points` (JSON array of key points)
    *   `topics_covered` (JSON array of topics discussed)
    *   `important_timestamps` (JSON array of significant moments with timestamps)
    *   `summary_quality_score` (AI confidence in summary quality)
    *   `generated_at`
    *   `processing_time` (Time taken to generate summary)

*   **AtRiskPlayer**: Represents a player identified as at risk of falling behind.
    *   `at_risk_player_id` (Primary Key)
    *   `player_id` (Foreign Key to `User`)
    *   `guild_id` (Foreign Key to `Guild`, optional)
    *   `risk_level` (e.g., 'Low', 'Medium', 'High')
    *   `risk_factors` (JSON array of risk factors)
    *   `prediction_model_version` (Version of the AI model used)
    *   `confidence_score` (Confidence in the prediction)
    *   `recommended_interventions` (JSON array of suggested actions)
    *   `identified_at`
    *   `last_updated`
    *   `intervention_taken` (Boolean, whether action was taken)

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

**Competitive Learning**

*   **KnowledgeDuel**: Represents real-time competitive quizzes or problem-solving challenges.
    *   `knowledge_duel_id` (Primary Key)
    *   `challenger_id` (Foreign Key to `User`)
    *   `opponent_id` (Foreign Key to `User`)
    *   `duel_type` (e.g., 'Quick Quiz', 'Problem Solving', 'Speed Challenge')
    *   `topic` (Subject area for the duel)
    *   `difficulty_level` (e.g., 'Easy', 'Medium', 'Hard')
    *   `time_limit` (Duration in minutes)
    *   `challenger_score`
    *   `opponent_score`
    *   `winner_id` (Foreign Key to `User`, optional)
    *   `xp_reward`
    *   `cosmetic_rewards` (JSON array of cosmetic rewards)
    *   `started_at`
    *   `completed_at`
    *   `duel_status` (e.g., 'Challenged', 'Accepted', 'In Progress', 'Completed', 'Declined')

*   **PeerTeaching**: Represents mini-lessons or tutorials created by players.
    *   `peer_teaching_id` (Primary Key)
    *   `teacher_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `content` (Rich text content of the lesson)
    *   `teaching_format` (e.g., 'Written Tutorial', 'Video', 'Interactive Demo')
    *   `topic`
    *   `difficulty_level` (e.g., 'Beginner', 'Intermediate', 'Advanced')
    *   `estimated_duration`
    *   `mentor_xp_earned`
    *   `view_count`
    *   `rating_average`
    *   `created_at`
    *   `updated_at`
    *   `publication_status` (e.g., 'Draft', 'Published', 'Archived')

*   **PeerTeachingRating**: Represents ratings for peer teaching content.
    *   `peer_teaching_rating_id` (Primary Key)
    *   `peer_teaching_id` (Foreign Key to `PeerTeaching`)
    *   `rater_id` (Foreign Key to `User`)
    *   `rating` (1-5 stars)
    *   `review` (Optional text review)
    *   `helpful_votes` (Number of users who found this review helpful)
    *   `created_at`

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

*   Users and Parties have a many-to-many relationship managed through the `PartyMembership` junction table, which stores additional relationship attributes like permission levels and join dates.
*   Users and Party Meetings have a many-to-many relationship managed through the `MeetingAttendance` junction table, which tracks attendance status, join/leave times, and personal notes.
*   Users and Guilds have a many-to-many relationship managed through the `GuildMembership` junction table, which tracks membership status and join dates.
*   Quests and Skills have a many-to-many relationship managed through the `QuestSkillRelationship` junction table, which defines the type of relationship (requires, rewards, enhances).
*   Users and Achievements have a many-to-many relationship managed through the `UserAchievement` junction table, which tracks when achievements were earned and progress data.
*   Users and PvP Events have a many-to-many relationship managed through the `PvPParticipant` junction table, which tracks scores, ranks, and participation status.
*   Guides and Mentor Archetypes have a many-to-many relationship managed through the `GuideMentorArchetype` junction table, which tracks selected archetypes and their active status.
*   Knowledge Packs and Marketplace Items have a many-to-many relationship managed through the `KnowledgePackItem` junction table, which tracks the order of items within packs.
*   Marketplace Items/Codex Entries and Meta Skills have a many-to-many relationship managed through the `ItemMetaSkill` junction table, which tracks relevance scores and tagging information.
*   Users and Learning Circles have a many-to-many relationship managed through the `LearningCircleMembership` junction table.
*   Users and Study Parties have a many-to-many relationship managed through the `StudyPartyParticipant` junction table.
*   Users and Seasonal Events have a many-to-many relationship managed through the `SeasonalEventParticipant` junction table.

### Hierarchical and Unidirectional Relationships

*   A Party can have one PartyStash, which can contain multiple PartyStashItems, creating a hierarchical relationship that prevents circular dependencies.
*   A Party can have multiple PartyMeetings, each of which can have one MeetingRecord and multiple MeetingSummaries, with MeetingSummaries containing multiple MeetingActionItems, creating a clear hierarchical structure for meeting data.
*   A Guild Master (User) can create multiple GuildQuestTemplates, which can be instantiated as GuildQuests, maintaining a unidirectional flow of relationships.
*   Guide-Player relationships are managed through the `GuideAssignment` entity, which creates a structured relationship between Users in different roles without direct circular references.
*   The EternalCodex can contain multiple CodexEntries, which reference MarketplaceItems through a unidirectional relationship, preventing circular dependencies.
*   Classes suggest Routes through a JSON array, maintaining a flexible relationship without requiring junction tables.
*   Skills can reference other Skills as prerequisites through JSON arrays, creating a directed acyclic graph structure.

### Ownership and Transaction Relationships

*   A User has one UserWallet per Currency type, which can be involved in multiple Transactions as either sender or receiver.
*   Transactions can reference MarketplaceItems, Quests, or other entities through optional foreign keys, creating unidirectional relationships that prevent circular dependencies.
*   A User can list multiple MarketplaceItems as a seller, and a MarketplaceItem can have multiple ItemRatings from different Users, with clear directional relationships that avoid circularity.
*   Currency transactions maintain audit trails through the Transaction entity, ensuring financial integrity.

### Administrative Relationships

*   A Game Master (User) can create multiple GlobalEvents, EventScripts, and SeasonalEvents, maintaining unidirectional relationships that prevent circular dependencies.
*   System health monitoring is tracked through SystemHealth entities that reference system components without creating circular relationships.
*   Analytics dashboards aggregate data from multiple sources through the AnalyticsDashboard and AnalyticsMetric entities.

### AI and Content Relationships

*   AI-generated content (suggestions, summaries, etc.) maintains references to source materials through foreign keys and JSON arrays.
*   Browser extension data flows unidirectionally from extraction to processing to integration with user content.
*   Lore fragments and narrative content are associated with academic concepts through the LoreFragment entity.

## Data Migration and Versioning

*   The data model will be versioned to track changes over time using semantic versioning (e.g., v1.0.0, v1.1.0, v2.0.0).
*   Migration scripts will be created for each major version change to ensure data integrity and backward compatibility.
*   A comprehensive backup and recovery strategy will be implemented to protect against data loss, including:
    *   Daily automated backups of the entire database
    *   Point-in-time recovery capabilities
    *   Cross-region backup replication for disaster recovery
*   Database schema changes will be managed through migration files that can be applied incrementally.
*   Data validation rules will be implemented to ensure consistency during migrations.

## Data Security and Privacy

*   All sensitive user data will be encrypted at rest using AES-256 encryption and in transit using TLS 1.3.
*   Access to user data will be restricted based on role-based access control (RBAC) and the principle of least privilege.
*   Personal identifiable information (PII) will be handled in compliance with GDPR, CCPA, and other relevant data protection regulations.
*   Data retention policies will be implemented to automatically purge data that is no longer needed:
    *   User account data: Retained for 7 years after account deletion
    *   Learning progress data: Retained for 5 years after course completion
    *   System logs: Retained for 1 year
    *   Financial transaction records: Retained for 7 years for compliance
*   Regular security audits will be conducted quarterly to identify and address potential vulnerabilities.
*   User consent management will be implemented for data collection and processing activities.
*   Data anonymization techniques will be applied for analytics and research purposes.

## Performance Considerations

*   Database indexes will be strategically created on frequently queried fields to improve performance:
    *   Primary keys and foreign keys (automatic)
    *   User lookup fields (email, username, clerk_user_id)
    *   Timestamp fields for chronological queries
    *   Status and type fields for filtering
    *   JSON field paths for frequently accessed nested data
*   Query optimization strategies will be implemented:
    *   Denormalization for read-heavy operations (e.g., user statistics, leaderboards)
    *   Materialized views for complex aggregations
    *   Query result caching for frequently accessed data
*   Caching strategies will be implemented at multiple levels:
    *   Application-level caching using Redis for session data and frequently accessed objects
    *   Database query result caching
    *   CDN caching for static assets and file content
*   Database scaling strategies will be prepared for growth:
    *   Read replicas for distributing read operations
    *   Database sharding by user ID for horizontal scaling
    *   Partitioning of large tables by date or other logical boundaries
*   Connection pooling will be implemented to manage database connections efficiently.
*   Monitoring and alerting will be set up to track database performance metrics and identify bottlenecks.

## Data Consistency and Integrity

*   Foreign key constraints will be enforced to maintain referential integrity.
*   Check constraints will be implemented for data validation (e.g., valid email formats, positive numeric values).
*   Unique constraints will prevent duplicate data where appropriate.
*   Transaction boundaries will be carefully designed to ensure ACID properties.
*   Eventual consistency patterns will be used for non-critical data synchronization across services.
*   Data validation will be implemented at both the application and database levels.
*   Audit trails will be maintained for critical data changes through dedicated audit tables.

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
*   Content delivery will be optimized through CDN integration for file storage and retrieval.
  
