# RogueLearn Browser Extension UI Generation Prompt

## High-Level Goal
Create a cohesive, intuitive browser extension UI for RogueLearn that seamlessly integrates with web browsing, allowing students to capture notes, track skills, and manage quests while maintaining the fantasy RPG aesthetic of the main platform.

## Detailed, Step-by-Step Instructions

1. Design the Browser Extension Icon:
   - Create a distinctive toolbar icon with normal, active, and notification states
   - Design a badge indicator for notifications/quest progress
   - Ensure the icon is recognizable at 16x16, 32x32, and 48x48 pixel sizes
   - Implement subtle animation for the active state

2. Create the Popup Panel main structure:
   - Design a responsive panel (350-400px width, variable height)
   - Implement a tabbed interface with RPG-themed tab designs
   - Create a consistent header with user info and sync status
   - Design a footer with quick actions and settings access
   - Ensure smooth opening/closing animations

3. Implement the Current Quest section:
   - Design a quest card with progress indicator
   - Create a timer/deadline display if applicable
   - Implement quest objectives checklist
   - Add related resources section
   - Design quest reward visualization

4. Build the Notes Arsenal interface:
   - Create a searchable notes list with filtering options
   - Design note cards with preview, tags, and timestamp
   - Implement a quick-capture form for new notes
   - Add categorization and organization controls
   - Create a note detail view with formatting options

5. Develop the Skill Tree mini-view:
   - Design a compact skill tree visualization
   - Create interactive node highlighting for relevant skills
   - Implement zoom and focus controls
   - Add progress indicators for skills in development
   - Create a link to full skill tree in main application

6. Create the Settings panel:
   - Design toggles for extension features
   - Implement sync frequency controls
   - Add notification preferences
   - Create theme/appearance options
   - Design keyboard shortcut configuration

7. Implement the Text Highlighter content script UI:
   - Design a highlight tooltip that appears on text selection
   - Create category/tag selection dropdown
   - Implement color coding for different note types
   - Add quick-action buttons (save, share, relate to skill)
   - Ensure the tooltip is non-intrusive

8. Develop the Skill Detector overlay:
   - Create subtle indicators that appear near relevant content
   - Design an information panel that appears on hover/click
   - Implement confidence level visualization
   - Add "Add to my skills" action button
   - Ensure the overlay doesn't interfere with normal browsing

9. Build the Quest Tracker sidebar:
   - Design a collapsible sidebar that shows current quest progress
   - Create progress indicators for objectives
   - Implement a timer/countdown if applicable
   - Add quick-action buttons for quest management
   - Ensure the sidebar can be easily toggled on/off

10. Implement responsive behavior:
    - Optimize all UI elements for different browser window sizes
    - Create compact layouts for when browser window is narrow
    - Ensure touch-friendly targets for mobile browsers
    - Implement scrollable sections for overflow content

11. Add RPG-themed visual elements:
    - Use scroll and parchment textures for backgrounds
    - Implement fantasy-themed icons and buttons
    - Add subtle magical effects for interactions
    - Create thematic loading animations
    - Use the RogueLearn color palette consistently

12. Implement accessibility features:
    - Ensure keyboard navigability for all functions
    - Add appropriate ARIA labels and roles
    - Implement high contrast mode
    - Ensure screen reader compatibility
    - Add text alternatives for all visual indicators

## Code Examples, Data Structures & Constraints

### Extension State Interface
```typescript
interface ExtensionState {
  isActive: boolean;
  currentUser: {
    id: string;
    name: string;
    characterClass: string;
    level: number;
    avatarUrl: string;
  } | null;
  currentQuest: {
    id: string;
    title: string;
    description: string;
    deadline: Date | null;
    objectives: Array<{
      id: string;
      description: string;
      isCompleted: boolean;
      progress?: number; // 0-100
    }>;
    rewards: Array<{
      type: 'xp' | 'item' | 'skill' | 'achievement';
      value: number | string;
      iconUrl: string;
    }>;
  } | null;
  notes: Array<{
    id: string;
    content: string;
    source: string; // URL
    timestamp: Date;
    tags: string[];
    relatedSkills: string[];
    color: string;
  }>;
  settings: {
    highlighterEnabled: boolean;
    skillDetectorEnabled: boolean;
    questTrackerEnabled: boolean;
    notificationsEnabled: boolean;
    syncFrequency: 'realtime' | 'hourly' | 'daily';
    theme: 'light' | 'dark' | 'auto';
    keyboardShortcuts: Record<string, string>;
  };
  syncStatus: 'synced' | 'syncing' | 'error' | 'offline';
  lastSyncTime: Date | null;
}
```

### Component Props Examples
```typescript
interface PopupPanelProps {
  state: ExtensionState;
  onTabChange: (tabName: string) => void;
  onSyncRequest: () => Promise<void>;
  onSettingsChange: (settings: Partial<ExtensionState['settings']>) => void;
  onNoteCreate: (note: Omit<ExtensionState['notes'][0], 'id' | 'timestamp'>) => Promise<void>;
  onQuestAction: (questId: string, action: 'complete' | 'abandon' | 'focus') => Promise<void>;
}

interface TextHighlighterProps {
  enabled: boolean;
  availableTags: string[];
  availableSkills: Array<{ id: string; name: string }>;
  onHighlight: (selection: string, url: string, tags: string[], skills: string[]) => Promise<void>;
  colorMap: Record<string, string>; // tag -> color
}
```

### Tech Stack Constraints
- Browser Extension Manifest V3
- React with TypeScript for popup and options pages
- CSS Modules or Tailwind CSS for styling
- Browser Storage API for local data
- Fetch API for communication with RogueLearn backend

### Browser Compatibility Requirements
- Chrome 88+
- Firefox 86+
- Edge 88+

### Performance Requirements
- Popup should open in under 300ms
- Content scripts should not impact page load time by more than 200ms
- Smooth animations at 60fps
- Efficient memory usage (< 50MB)

### Accessibility Requirements
- WCAG 2.1 AA compliance
- Keyboard navigable with clear focus indicators
- Screen reader friendly with appropriate announcements
- Support for reduced motion preferences
- Color contrast ratios meeting accessibility standards

### Do NOT
- Create UI elements that cover important webpage content
- Implement features that significantly slow down browsing
- Design tiny touch targets that are difficult to interact with
- Use colors or patterns that could trigger photosensitive conditions
- Create automatic popups that interrupt the user's browsing flow

## Define a Strict Scope

### Files to Create
- manifest.json (extension configuration)
- popup/
  - index.html
  - Popup.tsx (main popup component)
  - QuestPanel.tsx
  - NotesPanel.tsx
  - SkillTreePanel.tsx
  - SettingsPanel.tsx
  - Header.tsx
  - Footer.tsx
- content-scripts/
  - TextHighlighter.tsx
  - SkillDetector.tsx
  - QuestTracker.tsx
- background/
  - background.ts (service worker)
- options/
  - index.html
  - Options.tsx (detailed settings page)
- shared/
  - types.ts (TypeScript interfaces)
  - state.ts (state management)
  - api.ts (backend communication)
  - theme.ts (styling constants)
  - icons/ (SVG icons directory)

### Implementation Priority
1. Basic extension structure and popup panel
2. Text highlighter functionality
3. Notes management interface
4. Current quest tracking
5. Skill detector overlay
6. Mini skill tree visualization
7. Settings and customization
8. RPG-themed visual enhancements
9. Accessibility features
10. Browser compatibility testing

The browser extension should feel like a natural extension of the RogueLearn platform, maintaining consistent branding, terminology, and RPG theming while providing genuine utility for capturing learning materials and tracking progress during web browsing.