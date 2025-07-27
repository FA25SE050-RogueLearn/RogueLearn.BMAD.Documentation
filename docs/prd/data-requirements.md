# Data Requirements

This document outlines the preliminary data model for RogueLearn, addressing a key blocker identified in the PRD checklist report.

## Core Entities and Relationships

A preliminary data model is proposed to guide the architecture design. The main entities and their relationships are as follows:

*   **User**: Represents an individual using the RogueLearn platform.
    *   `user_id` (Primary Key)
    *   `username`
    *   `email`
    *   `password_hash`
    *   `created_at`

*   **Course**: Represents a course or subject that a user is studying.
    *   `course_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `name`
    *   `description`

*   **Syllabus**: Represents the document uploaded by the user for a course.
    *   `syllabus_id` (Primary Key)
    *   `course_id` (Foreign Key to `Course`)
    *   `content` (The raw text or structured data from the uploaded file)
    *   `file_type` (e.g., PDF, DOCX)

*   **Quest**: Represents a learning task or objective generated from the syllabus.
    *   `quest_id` (Primary Key)
    *   `course_id` (Foreign Key to `Course`)
    *   `title`
    *   `description`
    *   `status` (e.g., 'Not Started', 'In Progress', 'Completed')
    *   `due_date` (Optional)

*   **Note**: Represents user-created notes associated with a course or quest.
    *   `note_id` (Primary Key)
    *   `user_id` (Foreign Key to `User`)
    *   `course_id` (Foreign Key to `Course`, optional)
    *   `quest_id` (Foreign Key to `Quest`, optional)
    *   `content`

*   **Party**: Represents a study group created by users.
    *   `party_id` (Primary Key)
    *   `name`
    *   `description`
    *   `created_by` (Foreign Key to `User`)

*   **PartyMember**: A junction table to manage the many-to-many relationship between `User` and `Party`.
    *   `user_id` (Foreign Key to `User`)
    *   `party_id` (Foreign Key to `Party`)
    *   `role` (e.g., 'Admin', 'Member')