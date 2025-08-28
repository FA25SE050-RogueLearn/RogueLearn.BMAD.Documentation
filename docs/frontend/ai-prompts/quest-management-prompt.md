# RogueLearn Quest Management Interface Generation Prompt

## High-Level Goal
Create an engaging, intuitive quest management interface for the RogueLearn platform that allows students to discover, track, and complete educational quests while maintaining the fantasy RPG aesthetic of the platform. This interface should gamify learning activities and provide clear progression paths.

## Detailed, Step-by-Step Instructions

1. Create the Quest Dashboard main structure:
   - Design a responsive dashboard layout with filtering and sorting options
   - Implement a quest map visualization showing quest relationships and progression paths
   - Create sections for active quests, available quests, and completed quests
   - Design a quest search and filter system with tags for subjects, difficulty, and time requirements
   - Add a quest completion statistics and achievements section

2. Design the Quest Card component:
   - Create a visually appealing card with RPG-themed styling
   - Include quest title, brief description, difficulty indicator, and estimated time
   - Design progress indicators for multi-step quests
   - Add visual indicators for quest type (solo, group, timed challenge, etc.)
   - Implement reward previews (XP, skills, items, achievements)
   - Create hover and selected states with appropriate animations

3. Implement the Quest Detail View:
   - Design an immersive full-page view with rich quest narrative
   - Create sections for objectives, requirements, and rewards
   - Implement a step-by-step progress tracker for multi-stage quests
   - Add related resources and materials section
   - Design a quest timeline with deadlines and milestones
   - Create action buttons for accepting, abandoning, or submitting quests

4. Build the Quest Creation Interface (for tutors/guild masters):
   - Design a multi-step form with intuitive navigation
   - Create fields for quest details, objectives, requirements, and rewards
   - Implement a quest template system for quick creation
   - Add a quest preview mode to see student perspective
   - Design validation and submission feedback

5. Develop the Group Quest Coordination features:
   - Create a party formation interface for group quests
   - Design role assignment and responsibility tracking
   - Implement a shared progress dashboard
   - Add a group chat or communication feature
   - Create notifications for group activities and updates

6. Implement the Quest Reward System:
   - Design an inventory view for quest rewards
   - Create visual effects for receiving rewards
   - Implement skill tree integration showing skill unlocks
   - Add achievement badges and collection view
   - Design level-up animations and celebrations

7. Create the Quest Notification System:
   - Design toast notifications for quest updates
   - Implement a notification center with history
   - Create reminder alerts for approaching deadlines
   - Add progress milestone celebrations
   - Design quest invitation notifications

8. Develop the Quest Analytics Dashboard (for students):
   - Create visualizations for quest completion rates
   - Design time management insights
   - Implement skill development tracking
   - Add comparison with class averages (anonymized)
   - Create personalized quest recommendations based on performance

9. Implement responsive behavior:
   - Optimize layouts for mobile devices (320px-480px)
   - Create tablet-specific layouts (768px-1024px)
   - Design desktop layouts (1024px+)
   - Ensure touch-friendly interactions for mobile
   - Implement appropriate information density for each device size

10. Add RPG-themed visual elements:
    - Design quest cards as scrolls or parchments
    - Create map-like visualizations for quest relationships
    - Implement fantasy-themed icons for quest types and rewards
    - Add subtle animations for quest state changes
    - Design immersive background illustrations for quest narratives

11. Implement accessibility features:
    - Ensure keyboard navigability
    - Add appropriate ARIA labels and roles
    - Include high contrast mode option
    - Provide text alternatives for visual indicators
    - Support screen readers with meaningful descriptions

## Code Examples, Data Structures & Constraints

### Quest Interface
```typescript
interface Quest {
  id: string;
  title: string;
  description: string;
  narrative: string; // Rich text with RPG storytelling
  difficulty: 'novice' | 'apprentice' | 'adept' | 'expert' | 'master';
  type: 'solo' | 'group' | 'challenge' | 'exam_prep' | 'project';
  status: 'available' | 'active' | 'completed' | 'failed' | 'expired';
  tags: string[]; // Subject areas, topics
  estimatedTime: number; // In minutes
  deadline: Date | null;
  prerequisites: {
    quests: string[]; // Quest IDs
    skills: string[]; // Skill IDs
    level: number;
  };
  objectives: Array<{
    id: string;
    description: string;
    isCompleted: boolean;
    progress?: number; // 0-100 for partially completable objectives
    type: 'read' | 'watch' | 'practice' | 'create' | 'submit' | 'quiz';
    resources?: Array<{
      type: 'document' | 'video' | 'exercise' | 'link';
      url: string;
      title: string;
    }>;
  }>;
  rewards: Array<{
    type: 'xp' | 'skill' | 'item' | 'achievement' | 'currency';
    value: number | string;
    name: string;
    description: string;
    iconUrl: string;
  }>;
  groupSize?: {
    min: number;
    max: number;
  };
  groupMembers?: Array<{
    id: string;
    name: string;
    role: string;
    status: 'invited' | 'active' | 'completed';
  }>;
  createdBy: string; // Tutor/Guild Master ID
  createdAt: Date;
  updatedAt: Date;
}
```

### Component Props Examples
```typescript
interface QuestCardProps {
  quest: Quest;
  isSelected: boolean;
  onSelect: (questId: string) => void;
  onAction: (questId: string, action: 'accept' | 'abandon' | 'submit') => void;
  size: 'small' | 'medium' | 'large';
  showDetails: boolean;
}

interface QuestDashboardProps {
  activeQuests: Quest[];
  availableQuests: Quest[];
  completedQuests: Quest[];
  onQuestSelect: (questId: string) => void;
  onFilterChange: (filters: QuestFilters) => void;
  onSortChange: (sortBy: string, direction: 'asc' | 'desc') => void;
  stats: {
    totalCompleted: number;
    totalActive: number;
    completionRate: number;
    averageTime: number;
    level: number;
    xp: number;
    nextLevelXp: number;
  };
}

interface QuestFilters {
  difficulty: Array<Quest['difficulty']>;
  type: Array<Quest['type']>;
  tags: string[];
  status: Array<Quest['status']>;
  timeRange: [number, number]; // Min and max minutes
  searchTerm: string;
}
```

### Tech Stack Constraints
- React with TypeScript
- Framer Motion for animations
- Tailwind CSS for styling
- React Context API or Redux for state management
- React Query for data fetching

### Performance Requirements
- Initial dashboard load under 1.5 seconds
- Smooth animations at 60fps
- Efficient filtering and sorting (under 200ms response)
- Optimized images and assets for quick loading
- Pagination or virtualization for long quest lists

### Accessibility Requirements
- WCAG 2.1 AA compliance
- Keyboard navigable with clear focus indicators
- Screen reader friendly with appropriate announcements
- Support for reduced motion preferences
- Color contrast ratios meeting accessibility standards

### Do NOT
- Create overly complex animations that distract from content
- Implement auto-playing sounds without user control
- Design tiny touch targets that are difficult to interact with
- Use colors or patterns that could trigger photosensitive conditions
- Create interfaces that require excessive scrolling or clicking

## Define a Strict Scope

### Files to Create
- components/Quests/index.ts (barrel file)
- components/Quests/QuestDashboard.tsx (main component)
- components/Quests/QuestCard.tsx (quest preview card)
- components/Quests/QuestDetail.tsx (full quest view)
- components/Quests/QuestMap.tsx (visual quest relationships)
- components/Quests/QuestFilters.tsx (filtering interface)
- components/Quests/QuestProgress.tsx (progress tracking)
- components/Quests/QuestRewards.tsx (rewards display)
- components/Quests/QuestObjectives.tsx (objectives list)
- components/Quests/GroupQuestPanel.tsx (group coordination)
- components/Quests/QuestCreation.tsx (for tutors/guild masters)
- components/Quests/QuestStats.tsx (analytics dashboard)
- components/Quests/hooks/useQuestData.ts (data fetching)
- components/Quests/hooks/useQuestActions.ts (quest interactions)
- components/Quests/types.ts (TypeScript interfaces)
- components/Quests/utils.ts (helper functions)

### Implementation Priority
1. Basic quest dashboard with filtering and sorting
2. Quest card and detail view
3. Quest progress tracking and objectives
4. Reward system and visualization
5. Group quest coordination features
6. Quest map and relationship visualization
7. Notification system
8. Analytics dashboard
9. Responsive behavior and mobile optimization
10. Accessibility features
11. RPG-themed visual enhancements

The quest management interface should feel like an engaging game quest system while providing clear educational value. It should motivate students to complete learning activities by framing them as heroic quests with meaningful rewards and progression.