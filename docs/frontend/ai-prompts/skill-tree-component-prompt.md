# RogueLearn Skill Tree Component Generation Prompt

## High-Level Goal
Create an interactive, visually engaging skill tree visualization component for the RogueLearn platform that displays a student's learning path as an RPG-style skill tree. The component should be responsive, accessible, and maintain the fantasy RPG aesthetic while providing clear educational value.

## Detailed, Step-by-Step Instructions

1. Create a React component structure for the skill tree visualization:
   - SkillTreeContainer (main wrapper)
   - SkillTreeGraph (visualization area)
   - SkillNode (individual skill representation)
   - SkillConnection (visual connection between nodes)
   - SkillTreeControls (zoom, pan, filter controls)
   - SkillNodeDetails (expanded view of a selected skill)

2. Implement the core visualization using a graph library:
   - Use D3.js or Vis.js for the graph rendering engine
   - Create a force-directed layout with customizable forces
   - Implement smooth animations for node expansion and navigation
   - Add zoom and pan capabilities with appropriate constraints

3. Design the SkillNode component with the following states:
   - Locked: Grayed out with a lock icon (prerequisites not met)
   - Available: Highlighted and interactive (can be started)
   - In Progress: Partially filled progress indicator
   - Completed: Fully highlighted with completion indicator
   - Selected: Enlarged with focus ring when clicked

4. Create the visual connections between nodes:
   - Implement directional arrows showing prerequisite relationships
   - Style connections based on status (locked, available, completed)
   - Add subtle animations for connections when nodes are hovered
   - Ensure connections reflow smoothly when the graph is manipulated

5. Implement the SkillNodeDetails panel:
   - Show detailed information when a node is selected
   - Include skill name, description, estimated time to master
   - List required and recommended resources
   - Show prerequisite and dependent skills
   - Include progress tracking and next steps

6. Add interactive controls:
   - Zoom in/out buttons with keyboard shortcuts
   - Reset view button to return to default state
   - Search functionality to find specific skills
   - Filters for skill categories, difficulty, and completion status
   - Toggle for different view modes (tree, radial, list)

7. Implement responsive behavior:
   - Optimize layout for mobile devices (320px-480px)
   - Adjust node size and spacing for different screen sizes
   - Implement touch-friendly interactions for mobile
   - Create a simplified view for very small screens

8. Add RPG-themed visual elements:
   - Design nodes as magical runes or spell icons
   - Style connections as glowing magical energy flows
   - Add particle effects for completed skills
   - Include thematic background elements that don't distract

9. Implement accessibility features:
   - Ensure keyboard navigability between nodes
   - Add appropriate ARIA labels and roles
   - Include high contrast mode option
   - Provide text alternatives for visual indicators
   - Support screen readers with meaningful descriptions

10. Optimize performance:
    - Implement virtualization for large skill trees
    - Use React.memo and useMemo for expensive calculations
    - Lazy load node details and resources
    - Add loading states for data fetching operations

## Code Examples, Data Structures & Constraints

### Skill Node Data Structure
```typescript
interface SkillNode {
  id: string;
  name: string;
  description: string;
  status: 'locked' | 'available' | 'in_progress' | 'completed';
  progress: number; // 0-100
  category: string;
  difficulty: 'beginner' | 'intermediate' | 'advanced';
  estimatedHours: number;
  prerequisites: string[]; // Array of skill IDs
  dependents: string[]; // Array of skill IDs
  resources: {
    required: Array<{ type: string; title: string; url: string }>;
    recommended: Array<{ type: string; title: string; url: string }>;
  };
  position?: { x: number; y: number }; // Optional saved position
}
```

### Component Props
```typescript
interface SkillTreeProps {
  skills: SkillNode[];
  connections: Array<{ source: string; target: string }>;
  initialSelectedSkill?: string;
  onSkillSelect?: (skillId: string) => void;
  onStatusChange?: (skillId: string, newStatus: SkillNode['status']) => void;
  viewMode?: 'tree' | 'radial' | 'list';
  readOnly?: boolean;
  showControls?: boolean;
}
```

### Tech Stack Constraints
- React with TypeScript
- D3.js or Vis.js for graph visualization
- Framer Motion for animations
- Tailwind CSS for styling
- React Context API for local state management

### Performance Requirements
- Must handle up to 100 nodes smoothly
- Animations must maintain 60fps on modern devices
- Initial load time under 1.5 seconds
- Responsive to user interactions within 100ms

### Accessibility Requirements
- WCAG 2.1 AA compliance
- Keyboard navigable with clear focus indicators
- Screen reader friendly with appropriate announcements
- Support for reduced motion preferences

### Do NOT
- Use Canvas exclusively (SVG preferred for accessibility)
- Create fixed-size visualizations that don't scale
- Implement complex 3D effects that impact performance
- Use small click targets that are difficult to interact with
- Overload the visualization with too many visual effects

## Define a Strict Scope

### Files to Create
- components/SkillTree/index.ts (barrel file)
- components/SkillTree/SkillTreeContainer.tsx (main component)
- components/SkillTree/SkillTreeGraph.tsx (visualization)
- components/SkillTree/SkillNode.tsx (node component)
- components/SkillTree/SkillConnection.tsx (edge component)
- components/SkillTree/SkillTreeControls.tsx (UI controls)
- components/SkillTree/SkillNodeDetails.tsx (details panel)
- components/SkillTree/hooks/useSkillTreeLayout.ts (layout logic)
- components/SkillTree/hooks/useSkillTreeInteractions.ts (interaction logic)
- components/SkillTree/types.ts (TypeScript interfaces)
- components/SkillTree/utils.ts (helper functions)

### Implementation Priority
1. Basic graph rendering with nodes and connections
2. Node states and visual styling
3. Interactive selection and details panel
4. Zoom, pan, and navigation controls
5. Responsive behavior and mobile optimization
6. Accessibility features
7. Performance optimizations
8. RPG-themed visual enhancements

The skill tree should be the centerpiece of the student experience, visually representing their learning journey in an engaging RPG format while providing clear educational value and intuitive navigation.