# Data Architecture

## Database Selection

**Primary Database:** Azure SQL Database
**Secondary/Specialized Databases:**
- Azure Cosmos DB (Graph API) for skill tree relationships
- Azure Redis Cache for session management and caching

Rationale:
1. Azure SQL Database provides robust relational data storage for user profiles, content metadata, and structured data
2. Cosmos DB's Graph API is ideal for the complex relationships in the skill tree
3. Redis Cache improves performance for frequently accessed data

## Data Models

Based on the PRD's data requirements, the system will implement the following core entities:

1. **User Domain**
   - User (authentication and basic info)
   - UserProfile (game-specific attributes)
   - UserRole (roles in different contexts)
   - Class (career goals)
   - Route (academic paths)

2. **Content Domain**
   - Arsenal (user's collection of learning materials)
   - ArsenalItem (individual learning materials)
   - Tag (categorization of content)
   - Source (origin of learning materials)

3. **Learning Domain**
   - Skill (knowledge areas in the skill tree)
   - SkillRelationship (connections between skills)
   - Quest (learning objectives)
   - BossFight (exams and major assessments)
   - UserProgress (tracking completion and achievements)

4. **Social Domain**
   - Party (study groups)
   - PartyMember (user's role in a party)
   - Message (communication between users)
   - Notification (system and user alerts)

## Data Flow

1. **User Registration and Onboarding**
   - User creates account
   - User selects Class (career goal) and Route (academic path)
   - System generates initial skill tree and quests

2. **Content Capture (Browser Extension)**
   - User highlights content on web pages
   - Extension captures and categorizes content
   - Content is stored in user's Arsenal
   - Skills are updated based on captured content

3. **Learning Path Generation**
   - System analyzes user's academic data and goals
   - AI generates personalized learning path
   - Path is visualized as a skill tree
   - Quests and Boss Fights are scheduled

4. **Progress Tracking**
   - User completes quests and Boss Fights
   - System updates user's progress and stats
   - Skill tree is updated to reflect new knowledge
   - Recommendations for next steps are generated