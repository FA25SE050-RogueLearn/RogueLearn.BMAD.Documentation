# RogueLearn Frontend Generation Prompt

## High-Level Goal
Create a responsive, RPG-themed educational platform called "RogueLearn" that gamifies the learning experience through skill trees, quests, and social features. The application should follow a mobile-first approach with an immersive fantasy RPG aesthetic while maintaining professional educational functionality.

## Detailed, Step-by-Step Instructions

1. Create a Next.js project with TypeScript and Tailwind CSS integration.

2. Implement the following core pages:
   - Dashboard/Home (character overview, active quests, notifications)
   - Skill Tree (interactive visualization of learning paths)
   - Arsenal (note management system)
   - Quest Journal (assignments and learning objectives)
   - Guild Hall (social and community features)
   - Character Creation (onboarding flow)

3. Develop these core UI components with all variants and states:
   - Button (Primary, Secondary, Tertiary/Ghost, Danger, Success)
   - Card (Standard, Interactive, Elevated, Bordered, Quest Card, Achievement Card)
   - Input Field (Text, TextArea, Password, Number, Search, Autocomplete)
   - Navigation Item (Primary, Secondary, Tab, Breadcrumb, Dropdown)
   - Error components (Inline Field Errors, Form-Level Errors, Toast Notifications, Full-Page Error States)

4. Implement the following user flows:
   - Character Creation flow (Class/career selection, Route/academic path selection, document upload)
   - Arsenal/Notes Management (create, organize, access study materials)
   - Boss Fight/Exam Preparation (view details, check readiness, access materials)
   - Social & Community Interaction (join parties, participate in events)

5. Create the AI Companion ("Familiar") interface with:
   - Customizable avatar that follows the user
   - Chat interface for asking questions
   - Contextual help panel
   - Transparency features (confidence indicators, source citations)

6. Implement the Skill Tree Generator with:
   - Document upload interface
   - Interactive graph visualization
   - Customization controls
   - Detailed skill node cards

7. Design the Browser Extension UI components:
   - Toolbar icon (default, active, and badge states)
   - Popup panel (360px × 480px) with tabbed interface
   - Content scripts (text highlighter, skill detector, quest tracker)

8. Ensure all interfaces follow the RPG theme with:
   - Fantasy-inspired UI elements (shields, scrolls, magical effects)
   - Game-like progression indicators
   - Immersive terminology (quests instead of assignments, etc.)

9. Implement responsive designs for:
   - Mobile (primary focus, 320px-480px)
   - Tablet (768px-1024px)
   - Desktop (1024px+)

10. Add animations and micro-interactions:
    - Subtle entrance/exit animations
    - Success/completion animations
    - Loading states with thematic elements

## Code Examples, Data Structures & Constraints

### Tech Stack Requirements
- Framework: Next.js with App Router
- State Management: React Context API for simple state, Redux for complex global state
- Styling: Tailwind CSS with custom theme extensions
- Animation: Framer Motion for complex animations, CSS transitions for simple ones
- Data Fetching: React Query for server state management

### Color Palette
- Primary: #4F46E5 (magical blue)
- Secondary: #6B7280 (stone gray)
- Accent: #8B5CF6 (mystical purple)
- Success: #059669 (forest green)
- Danger: #DC2626 (dragon red)
- Background: #1F2937 (dark dungeon)
- Text: #F9FAFB (scroll parchment)

### Component Structure
Follow atomic design methodology:
- atoms/ - Basic building blocks (buttons, inputs, icons)
- molecules/ - Simple combinations of atoms (form fields, cards)
- organisms/ - Complex UI sections (navigation, skill nodes)
- templates/ - Page layouts
- pages/ - Full pages with content

### Accessibility Requirements
- WCAG 2.1 AA compliance
- Keyboard navigation support
- Screen reader compatibility
- Reduced motion options
- Minimum contrast ratio of 4.5:1

### Performance Targets
- Largest Contentful Paint (LCP): < 2.5s
- First Input Delay (FID): < 100ms
- Cumulative Layout Shift (CLS): < 0.1

### Do NOT
- Use jQuery or other legacy libraries
- Create a dark/light theme toggle (use the dark theme consistently)
- Implement actual backend functionality (mock API responses instead)
- Use generic educational UI patterns without RPG theming
- Create complex 3D visualizations that might impact performance

## Define a Strict Scope

### Files to Create
- Create the Next.js project structure with TypeScript configuration
- Implement the component library in the components/ directory
- Create page templates in the app/ directory
- Add animation utilities in the utils/animations.ts file
- Implement the theme configuration in tailwind.config.js

### Implementation Priority
1. Core UI components and theme setup
2. Main page layouts and navigation
3. Character creation flow
4. Skill tree visualization
5. Arsenal/notes management
6. AI companion interface
7. Social features
8. Browser extension UI

### Deliverables
- Functioning Next.js application with all core pages
- Component library with Storybook documentation
- Responsive layouts for all screen sizes
- Interactive prototypes of key user flows
- Mock API integration for demonstration purposes

Remember to maintain the RPG theme throughout all interfaces while ensuring the educational functionality remains clear and accessible. The goal is to create an immersive learning experience that motivates students through gamification without sacrificing usability.