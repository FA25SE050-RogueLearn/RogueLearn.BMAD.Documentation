# Data Requirements

This document outlines the comprehensive data model for RogueLearn, covering all phases from MVP to post-MVP features.

## Core Entities and Relationships

### Phase 1: Core Student MVP

*   **User**: Represents an individual using the RogueLearn platform.
    *   `user_id` (Primary Key)
    *   `username`
    *   `email`
    *   `password_hash`
    *   `created_at`
    *   `last_login_at`
    *   `profile_image_url`
    *   `account_status` (e.g., 'Active', 'Inactive', 'Suspended')

*   **UserProfile**: Extends User with game-specific attributes.
    *   `user_profile_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `class_id` (Foreign Key to `Class`, representing career goal)
    *   `route_id` (Foreign Key to `Route`, representing academic path)
    *   `level`
    *   `experience_points`
    *   `created_at`
    *   `updated_at`

*   **Class**: Represents a career goal that a user can select.
    *   `class_id` (Primary Key)
    *   `name` (e.g., 'Full-Stack Developer', 'Data Scientist')
    *   `description`
    *   `icon_url`

*   **Route**: Represents an academic path that a user can select.
    *   `route_id` (Primary Key)
    *   `name` (e.g., 'Software Engineering', 'Computer Science')
    *   `description`
    *   `icon_url`

*   **Course**: Represents a course or subject that a user is studying.
    *   `course_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `name`
    *   `description`
    *   `created_at`
    *   `updated_at`
    *   `activation_status` (e.g., 'Active', 'Completed', 'Archived')

*   **Syllabus**: Represents the document uploaded by the user for a course.
    *   `syllabus_id` (Primary Key)
    *   `course_id` (Foreign Key to `Course`)
    *   `content` (The raw text or structured data from the uploaded file)
    *   `file_type` (e.g., PDF, DOCX)
    *   `file_url` (URL to the stored file in Supabase)
    *   `uploaded_at`
    *   `processed_at` (When the AI finished processing)
    *   `processing_status` (e.g., 'Pending', 'Processing', 'Completed', 'Failed')

*   **AcademicDocument**: Represents additional academic documents uploaded by the user.
    *   `academic_document_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `document_type` (e.g., 'GPA', 'Transcript', 'Timetable', 'Exam Schedule')
    *   `content` (The raw text or structured data from the uploaded file)
    *   `file_type` (e.g., PDF, DOCX)
    *   `file_url` (URL to the stored file in Supabase)
    *   `uploaded_at`
    *   `processed_at`
    *   `processing_status`

*   **QuestLine**: Represents the main learning path generated for a user.
    *   `quest_line_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `course_id` (Foreign Key to `Course`)
    *   `title`
    *   `description`
    *   `created_at`
    *   `updated_at`
    *   `activation_status` (e.g., 'Active', 'Completed', 'Archived')

*   **Quest**: Represents a learning task or objective generated from the syllabus.
    *   `quest_id` (Primary Key)
    *   `quest_line_id` (Foreign Key to `QuestLine`)
    *   `title`
    *   `description`
    *   `progress_status` (e.g., 'Not Started', 'In Progress', 'Completed')
    *   `due_date` (Optional)
    *   `created_at`
    *   `updated_at`
    *   `completion_date`
    *   `experience_points` (XP reward for completion)
    *   `prerequisites` (JSON array of prerequisite quest IDs)

*   **SkillTree**: Represents the user's knowledge structure.
    *   `skill_tree_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `course_id` (Foreign Key to `Course`, optional)
    *   `name`
    *   `description`
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

*   **Note**: Represents user-created notes associated with a course or quest.
    *   `note_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `course_id` (Foreign Key to `Course`, optional)
    *   `quest_id` (Foreign Key to `Quest`, optional)
    *   `skill_id` (Foreign Key to `Skill`, optional)
    *   `title`
    *   `content` (Rich text content)
    *   `created_at`
    *   `updated_at`
    *   `tags` (JSON array of tag strings)

*   **BossFight**: Represents a gamified mock exam.
    *   `boss_fight_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `quest_id` (Foreign Key to `Quest`, optional)
    *   `title`
    *   `description`
    *   `difficulty` (e.g., 'Easy', 'Medium', 'Hard')
    *   `progress_status` (e.g., 'Not Started', 'In Progress', 'Completed', 'Failed')
    *   `created_at`
    *   `started_at`
    *   `completed_at`
    *   `score`
    *   `max_score`
    *   `experience_points` (XP reward for completion)
    *   `required_skills` (JSON array of required skill IDs)

*   **Question**: Represents a question in a boss fight.
    *   `question_id` (Primary Key)
    *   `boss_fight_id` (Foreign Key to `BossFight`)
    *   `content`
    *   `question_type` (e.g., 'Multiple Choice', 'True/False', 'Short Answer')
    *   `options` (JSON array of answer options for multiple choice)
    *   `correct_answer`
    *   `points`

*   **UserAnswer**: Represents a user's answer to a question.
    *   `user_answer_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `question_id` (Foreign Key to `Question`)
    *   `boss_fight_id` (Foreign Key to `BossFight`)
    *   `answer`
    *   `is_correct`
    *   `points_earned`
    *   `submitted_at`

*   **Leaderboard**: Represents a ranking system for users.
    *   `leaderboard_id` (Primary Key)
    *   `name`
    *   `description`
    *   `leaderboard_type` (e.g., 'Class-specific', 'Global', 'PvP Event')
    *   `class_id` (Foreign Key to `Class`, optional)
    *   `created_at`
    *   `updated_at`

*   **LeaderboardEntry**: Represents a user's position on a leaderboard.
    *   `leaderboard_entry_id` (Primary Key)
    *   `leaderboard_id` (Foreign Key to `Leaderboard`)
    *   `user_id` (Foreign Key to `User`)
    *   `score`
    *   `rank`
    *   `updated_at`

*   **Notification**: Represents system notifications for users.
    *   `notification_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `title`
    *   `content`
    *   `notification_type` (e.g., 'Quest Update', 'New Learning Path', 'Achievement')
    *   `is_read`
    *   `created_at`
    *   `expires_at` (Optional)

### Phase 2: Social & Extension MVP

*   **Party**: Represents a study group created by users.
    *   `party_id` (Primary Key)
    *   `name`
    *   `description`
    *   `created_by` (Foreign Key to `User`)
    *   `created_at`
    *   `updated_at`
    *   `activation_status` (e.g., 'Active', 'Archived')
    *   `join_type` (e.g., 'Invite Only', 'Open')
    *   `max_members` (Optional)

*   **PartyMember**: A junction table to manage the many-to-many relationship between `User` and `Party`.
    *   `user_id` (Foreign Key to `User`)
    *   `party_id` (Foreign Key to `Party`)
    *   `role` (e.g., 'Leader', 'Member')
    *   `joined_at`
    *   `account_status` (e.g., 'Active', 'Inactive')

*   **PartyInvitation**: Represents an invitation to join a party.
    *   `party_invitation_id` (Primary Key)
    *   `party_id` (Foreign Key to `Party`)
    *   `inviter_id` (Foreign Key to `User`)
    *   `invitee_id` (Foreign Key to `User`)
    *   `invitation_status` (e.g., 'Pending', 'Accepted', 'Declined')
    *   `created_at`
    *   `responded_at`

*   **PartyStash**: Represents the shared resource space for a party.
    *   `party_stash_id` (Primary Key)
    *   `party_id` (Foreign Key to `Party`)
    *   `name`
    *   `description`
    *   `created_at`
    *   `updated_at`

*   **PartyStashItem**: Represents an item in the party stash.
    *   `party_stash_item_id` (Primary Key)
    *   `party_stash_id` (Foreign Key to `PartyStash`)
    *   `contributor_id` (Foreign Key to `User`)
    *   `item_type` (e.g., 'Note', 'Link', 'File')
    *   `title`
    *   `content` (For notes) or `url` (For links) or `file_url` (For files)
    *   `created_at`
    *   `updated_at`

*   **BrowserExtensionData**: Represents data extracted by the browser extension.
    *   `browser_extension_data_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `source_url`
    *   `data_type` (e.g., 'Syllabus', 'GPA', 'Timetable', 'Exam Schedule', 'Web Content')
    *   `content`
    *   `extracted_at`
    *   `processed_at`
    *   `processing_status`

*   **ExtractedNote**: Represents a note automatically created from extracted web content.
    *   `extracted_note_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `browser_extension_data_id` (Foreign Key to `BrowserExtensionData`)
    *   `title`
    *   `content`
    *   `category`
    *   `tags` (JSON array of tag strings)
    *   `created_at`
    *   `updated_at`

### Phase 3: Educator & Admin Toolkit

*   **Guild**: Represents a course managed by a Guild Master (Lecturer).
    *   `guild_id` (Primary Key)
    *   `name`
    *   `description`
    *   `created_by` (Foreign Key to `User`, the Guild Master)
    *   `created_at`
    *   `updated_at`
    *   `activation_status` (e.g., 'Active', 'Archived')
    *   `join_type` (e.g., 'Invite Only', 'Open', 'Code Required')
    *   `join_code` (Optional)

*   **GuildMember**: A junction table to manage the many-to-many relationship between `User` and `Guild`.
    *   `user_id` (Foreign Key to `User`)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `role` (e.g., 'Guild Master', 'Player', 'Guide')
    *   `joined_at`
    *   `account_status` (e.g., 'Active', 'Inactive')

*   **GuildMaterial**: Represents course materials uploaded by a Guild Master.
    *   `guild_material_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `uploader_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `material_type` (e.g., 'Syllabus', 'Lecture Notes', 'Assignment', 'Reference')
    *   `file_url`
    *   `uploaded_at`

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
    *   `created_at`
    *   `updated_at`
    *   `is_ai_generated` (Boolean)

*   **GuildQuest**: Represents an instance of a quest assigned to guild members.
    *   `guild_quest_id` (Primary Key)
    *   `guild_quest_template_id` (Foreign Key to `GuildQuestTemplate`)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `assigner_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `creation_status` (e.g., 'Draft', 'Published', 'Archived')
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

*   **SkillTreeOverlay**: Represents a custom skill tree overlay created by a Guild Master.
    *   `skill_tree_overlay_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `creator_id` (Foreign Key to `User`)
    *   `name`
    *   `description`
    *   `created_at`
    *   `updated_at`
    *   `creation_status` (e.g., 'Draft', 'Published', 'Archived')

*   **OverlaySkill**: Represents a skill in a custom skill tree overlay.
    *   `overlay_skill_id` (Primary Key)
    *   `skill_tree_overlay_id` (Foreign Key to `SkillTreeOverlay`)
    *   `name`
    *   `description`
    *   `level` (Required proficiency level)
    *   `icon_url`
    *   `position_x`
    *   `position_y`
    *   `prerequisites` (JSON array of prerequisite overlay skill IDs)

*   **LoreFragment**: Represents a narrative description for a course topic.
    *   `lore_fragment_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `creator_id` (Foreign Key to `User`)
    *   `topic`
    *   `content`
    *   `created_at`
    *   `updated_at`

*   **GuildBuff**: Represents a temporary boost or reward for guild members.
    *   `guild_buff_id` (Primary Key)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `creator_id` (Foreign Key to `User`)
    *   `name`
    *   `description`
    *   `buff_type` (e.g., 'XP Boost', 'Reward', 'Achievement')
    *   `condition` (JSON object describing the activation condition)
    *   `effect` (JSON object describing the buff effect)
    *   `start_date`
    *   `end_date`
    *   `activation_status` (e.g., 'Active', 'Inactive', 'Expired')

*   **GuideAssignment**: Represents the assignment of a Guide (Tutor) to a specific Player.
    *   `guide_assignment_id` (Primary Key)
    *   `guide_id` (Foreign Key to `User`, the Guide)
    *   `player_id` (Foreign Key to `User`, the Player)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `assigner_id` (Foreign Key to `User`, the Guild Master)
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
    *   `created_at`
    *   `assigned_at`
    *   `due_date`
    *   `progress_status` (e.g., 'Not Started', 'In Progress', 'Completed')

*   **MentorReflection**: Represents feedback or advice from a Guide to a Player.
    *   `mentor_reflection_id` (Primary Key)
    *   `guide_id` (Foreign Key to `User`)
    *   `player_id` (Foreign Key to `User`)
    *   `quest_id` (Foreign Key to `Quest`, optional)
    *   `skill_id` (Foreign Key to `Skill`, optional)
    *   `content`
    *   `media_type` (e.g., 'Text', 'Voice', 'Video')
    *   `media_url` (For voice or video reflections)
    *   `created_at`

*   **BurnoutForecast**: Represents a prediction of student disengagement.
    *   `burnout_forecast_id` (Primary Key)
    *   `player_id` (Foreign Key to `User`)
    *   `guild_id` (Foreign Key to `Guild`)
    *   `risk_level` (e.g., 'Low', 'Medium', 'High')
    *   `indicators` (JSON array of disengagement indicators)
    *   `generated_at`

*   **MentorArchetype**: Represents a thematic persona for a Guide.
    *   `mentor_archetype_id` (Primary Key)
    *   `name` (e.g., 'Sage', 'Trickster', 'Warrior')
    *   `description`
    *   `icon_url`
    *   `dialogue_tone`
    *   `available_cosmetics` (JSON array of cosmetic item IDs)

*   **GuideMentorArchetype**: A junction table to associate Guides with their chosen Mentor Archetypes.
    *   `guide_id` (Foreign Key to `User`)
    *   `mentor_archetype_id` (Foreign Key to `MentorArchetype`)
    *   `selected_at`
    *   `is_active` (Boolean)

*   **SystemHealth**: Represents the health status of the application.
    *   `system_health_id` (Primary Key)
    *   `component` (e.g., 'API', 'Database', 'AI Service')
    *   `system_status` (e.g., 'Healthy', 'Degraded', 'Down')
    *   `details`
    *   `checked_at`

*   **GlobalEvent**: Represents a platform-wide event triggered by a Game Master.
    *   `global_event_id` (Primary Key)
    *   `creator_id` (Foreign Key to `User`, the Game Master)
    *   `name`
    *   `description`
    *   `event_type` (e.g., 'Bonus XP', 'Challenge', 'Invasion')
    *   `effect` (JSON object describing the event effect)
    *   `start_date`
    *   `end_date`
    *   `activation_status` (e.g., 'Scheduled', 'Active', 'Completed', 'Cancelled')

*   **EventScript**: Represents a scheduled platform-wide event.
    *   `event_script_id` (Primary Key)
    *   `creator_id` (Foreign Key to `User`, the Game Master)
    *   `name`
    *   `description`
    *   `event_type` (e.g., 'Challenge', 'PvP Event')
    *   `configuration` (JSON object with event parameters)
    *   `participation_conditions` (JSON object with conditions)
    *   `schedule_date`
    *   `activation_status` (e.g., 'Scheduled', 'Active', 'Completed', 'Cancelled')

*   **AbuseReport**: Represents a report of inappropriate content or behavior.
    *   `abuse_report_id` (Primary Key)
    *   `reporter_id` (Foreign Key to `User`)
    *   `reported_user_id` (Foreign Key to `User`, optional)
    *   `reported_content_type` (e.g., 'User', 'Note', 'Party', 'Message')
    *   `reported_content_id`
    *   `reason`
    *   `details`
    *   `reported_at`
    *   `processing_status` (e.g., 'Pending', 'Processing', 'Completed')
    *   `resolution`
    *   `resolved_by` (Foreign Key to `User`, the Game Master)
    *   `resolved_at`

*   **AnalyticsDashboard**: Represents a comprehensive analytics dashboard for Game Masters.
    *   `analytics_dashboard_id` (Primary Key)
    *   `name`
    *   `description`
    *   `dashboard_type` (e.g., 'Feature Usage', 'User Cohort', 'Platform Health')
    *   `configuration` (JSON object with dashboard settings)
    *   `created_at`
    *   `updated_at`

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
    *   `release_date`
    *   `creation_status` (e.g., 'Draft', 'Published', 'Archived')

### Phase 4: Living Ecosystem & Social

*   **AISpellSuggestion**: Represents a study aid suggested by the AI.
    *   `ai_spell_suggestion_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `title`
    *   `description`
    *   `spell_type` (e.g., 'Flashcard', 'Summary', 'Practice Quiz')
    *   `content`
    *   `source_note_ids` (JSON array of source note IDs)
    *   `suggested_at`
    *   `suggestion_status` (e.g., 'Suggested', 'Accepted', 'Rejected')

*   **ClassRecording**: Represents an audio/video recording of a class.
    *   `class_recording_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `guild_id` (Foreign Key to `Guild`, optional)
    *   `title`
    *   `description`
    *   `file_url`
    *   `duration`
    *   `uploaded_at`
    *   `processing_status` (e.g., 'Pending', 'Processing', 'Completed', 'Failed')

*   **RecordingSummary**: Represents a text summary generated from a class recording.
    *   `recording_summary_id` (Primary Key)
    *   `recording_id` (Foreign Key to `ClassRecording`)
    *   `content`
    *   `key_points` (JSON array of key points)
    *   `generated_at`

*   **AtRiskPlayer**: Represents a player identified as at risk of falling behind.
    *   `at_risk_player_id` (Primary Key)
    *   `player_id` (Foreign Key to `User`)
    *   `guild_id` (Foreign Key to `Guild`, optional)
    *   `risk_level` (e.g., 'Low', 'Medium', 'High')
    *   `risk_factors` (JSON array of risk factors)
    *   `identified_at`

*   **GuildVsGuildCompetition**: Represents a competition between guilds.
    *   `guild_vs_guild_competition_id` (Primary Key)
    *   `initiator_guild_id` (Foreign Key to `Guild`)
    *   `opponent_guild_id` (Foreign Key to `Guild`)
    *   `title`
    *   `description`
    *   `competition_type` (e.g., 'Quiz', 'Challenge', 'Project')
    *   `rules` (JSON object with competition rules)
    *   `start_date`
    *   `end_date`
    *   `activation_status` (e.g., 'Scheduled', 'Active', 'Completed')
    *   `winner_guild_id` (Foreign Key to `Guild`, optional)
    *   `initiator_score`
    *   `opponent_score`

*   **Achievement**: Represents a badge or achievement that a user can earn.
    *   `achievement_id` (Primary Key)
    *   `name`
    *   `description`
    *   `icon_url`
    *   `achievement_type` (e.g., 'Milestone', 'Skill', 'Social', 'Challenge')
    *   `requirements` (JSON object with achievement requirements)
    *   `experience_points`

*   **UserAchievement**: A junction table to track achievements earned by users.
    *   `user_id` (Foreign Key to `User`)
    *   `achievement_id` (Foreign Key to `Achievement`)
    *   `earned_at`
    *   `progress` (For achievements with progress tracking)

*   **PvPEvent**: Represents a player-versus-player competition.
    *   `pvp_event_id` (Primary Key)
    *   `title`
    *   `description`
    *   `event_type` (e.g., 'Coding Challenge', 'CSS Battle', 'Algorithm Battle')
    *   `rules` (JSON object with event rules)
    *   `start_date`
    *   `end_date`
    *   `activation_status` (e.g., 'Scheduled', 'Active', 'Completed')
    *   `max_participants`

*   **PvPParticipant**: A junction table to track participants in PvP events.
    *   `pvp_participant_id` (Primary Key)
    *   `pvp_event_id` (Foreign Key to `PvPEvent`)
    *   `user_id` (Foreign Key to `User`)
    *   `score`
    *   `rank`
    *   `joined_at`
    *   `participation_status` (e.g., 'Registered', 'Active', 'Completed', 'Disqualified')

### Phase 5: Marketplace & Economy

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

*   **MarketPlaceItemRating**: Represents a rating and review for a marketplace item.
    *   `marketplace_item_rating_id` (Primary Key)
    *   `marketplace_item_id` (Foreign Key to `MarketplaceItem`)
    *   `user_id` (Foreign Key to `User`)
    *   `rating` (1-5 stars)
    *   `review` (Text review)
    *   `created_at`
    *   `updated_at`

*   **Currency**: Represents the in-game currency system.
    *   `currency_id` (Primary Key)
    *   `name`
    *   `symbol`
    *   `description`
    *   `created_at`
    *   `updated_at`

*   **UserWallet**: Represents a user's currency balance.
    *   `user_wallet_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `currency_id` (Foreign Key to `Currency`)
    *   `balance`
    *   `created_at`
    *   `updated_at`

*   **Transaction**: Represents a currency transaction between users or with the system.
    *   `transaction_id` (Primary Key)
    *   `sender_wallet_id` (Foreign Key to `UserWallet`)
    *   `receiver_wallet_id` (Foreign Key to `UserWallet`)
    *   `amount`
    *   `transaction_type` (e.g., 'Purchase', 'Sale', 'Reward', 'System')
    *   `marketplace_item_id` (Foreign Key to `MarketplaceItem`, optional)
    *   `description`
    *   `created_at`
    *   `processing_status` (e.g., 'Pending', 'Completed', 'Failed', 'Refunded')

*   **EternalCodex**: Represents the public repository of high-quality study materials.
    *   `eternal_codex_id` (Primary Key)
    *   `name`
    *   `description`
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
    *   `ai_rating`
    *   `elevation_date`
    *   `tags` (JSON array of tag strings)

*   **KnowledgePack**: Represents a curated bundle of study materials.
    *   `knowledge_pack_id` (Primary Key)
    *   `curator_id` (Foreign Key to `User`, can be AI)
    *   `title`
    *   `description`
    *   `theme`
    *   `subject`
    *   `exam_type` (Optional)
    *   `created_at`
    *   `updated_at`
    *   `price` (In-game currency amount)
    *   `creation_status` (e.g., 'Draft', 'Published', 'Archived')

*   **KnowledgePackItem**: A junction table to associate items with knowledge packs.
    *   `knowledge_pack_id` (Foreign Key to `KnowledgePack`)
    *   `marketplace_item_id` (Foreign Key to `MarketplaceItem` or `EternalCodexEntry`)
    *   `order` (For ordering items within the pack)
    *   `added_at`

*   **MetaSkill**: Represents a higher-level skill category for tagging materials.
    *   `meta_skill_id` (Primary Key)
    *   `name` (e.g., 'Critical Thinking', 'Synthesis', 'Memorization')
    *   `description`
    *   `icon_url`
    *   `created_at`

*   **ItemMetaSkill**: A junction table to associate meta-skills with marketplace items or codex entries.
    *   `item_type` (e.g., 'MarketplaceItem', 'EternalCodexEntry')
    *   `item_id` (Foreign Key to `MarketplaceItem` or `EternalCodexEntry`)
    *   `meta_skill_id` (Foreign Key to `MetaSkill`)
    *   `relevance_score` (How relevant the meta-skill is to the item)
    *   `tagged_at`
    *   `tagged_by` (Foreign Key to `User`, can be AI)

## Data Relationships and Constraints

*   A User can have multiple Courses, but each Course belongs to only one User.
*   A Course can have one Syllabus, and a Syllabus belongs to only one Course.
*   A User can have multiple AcademicDocuments.
*   A User can have multiple QuestLines, and each QuestLine can have multiple Quests.
*   A User can have multiple SkillTrees, and each SkillTree can have multiple Skills.
*   A User can create multiple Notes, which can be associated with Courses, Quests, or Skills.
*   A User can have multiple BossFights, and each BossFight can have multiple Questions.
*   A User can create or join multiple Parties, and a Party can have multiple PartyMembers.
*   A Party can have one PartyStash, which can contain multiple StashItems.
*   A User can be a Guild Master for multiple Guilds, and a Guild can have multiple GuildMembers.
*   A Guild Master can create multiple GuildQuestTemplates, which can be instantiated as GuildQuests.
*   A Guide can be assigned to multiple Players, and a Player can have multiple Guides.
*   A Game Master can create multiple GlobalEvents and EventScripts.
*   A User can earn multiple Achievements, and an Achievement can be earned by multiple Users.
*   A User can list multiple MarketplaceItems, and a MarketplaceItem can have multiple ItemRatings.
*   A User has one UserWallet, which can be involved in multiple Transactions.
*   The EternalCodex can contain multiple CodexEntries, which are elevated from MarketplaceItems.
*   A KnowledgePack can contain multiple PackItems, which reference MarketplaceItems or CodexEntries.
*   A MarketplaceItem or CodexEntry can be tagged with multiple MetaSkills.

## Data Migration and Versioning

*   The data model will be versioned to track changes over time.
*   Migration scripts will be created for each major version change to ensure data integrity.
*   A backup and recovery strategy will be implemented to protect against data loss.

## Data Security and Privacy

*   All sensitive user data will be encrypted at rest and in transit.
*   Access to user data will be restricted based on role and permission.
*   Data retention policies will be implemented to comply with GDPR and other relevant regulations.
*   Regular security audits will be conducted to identify and address potential vulnerabilities.

## Performance Considerations

*   Indexes will be created on frequently queried fields to improve performance.
*   Denormalization will be considered for performance-critical queries.
*   Caching strategies will be implemented for frequently accessed data.
*   Database sharding and partitioning will be considered for scaling as the user base grows.