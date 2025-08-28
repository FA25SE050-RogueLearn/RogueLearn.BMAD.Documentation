# RogueLearn AI Companion ("Familiar") Interface Generation Prompt

## High-Level Goal
Create an engaging, intuitive AI companion interface for the RogueLearn platform that takes the form of a magical "Familiar" character. This interface should provide contextual help, answer questions, and guide students through their learning journey while maintaining the fantasy RPG aesthetic of the platform.

## Detailed, Step-by-Step Instructions

1. Create the main Familiar component structure:
   - FamiliarContainer (main wrapper)
   - FamiliarAvatar (animated character representation)
   - FamiliarChatInterface (expandable conversation UI)
   - FamiliarContextualHelp (slide-in panel for relevant resources)
   - FamiliarSettings (customization options)

2. Design the FamiliarAvatar component:
   - Create an animated character with multiple visual states (idle, thinking, speaking, celebrating)
   - Implement smooth transitions between states
   - Design variations based on user's class/career path
   - Add subtle ambient animations for the idle state
   - Ensure the avatar is responsive and scales appropriately on different devices

3. Implement the persistent chat bubble:
   - Create a floating, draggable chat bubble that's always accessible
   - Design collapsed and expanded states
   - Add subtle notification indicators for new information
   - Implement smooth expansion animation when clicked
   - Ensure it doesn't interfere with main content

4. Build the FamiliarChatInterface:
   - Create a conversational UI with message history
   - Implement typing indicators and response animations
   - Support text, voice, and image inputs
   - Design message bubbles with the RPG aesthetic
   - Add quick action buttons for common requests
   - Include a clear conversation history option

5. Develop the FamiliarContextualHelp panel:
   - Create a slide-in panel that appears when activated
   - Design sections for relevant resources based on current activity
   - Implement dismiss and "tell me more" options
   - Add visual indicators for resource types (video, article, quiz)
   - Ensure smooth animations for panel entrance/exit

6. Implement AI transparency features:
   - Create visual confidence indicators for AI responses
   - Design a source citation component for references
   - Implement a feedback mechanism for rating responses
   - Add clarification request UI for when the AI needs more information
   - Include an "explain this answer" option

7. Create the FamiliarSettings component:
   - Design UI for customizing the Familiar's appearance
   - Implement options for behavior preferences (proactive vs. reactive)
   - Add controls for notification frequency
   - Include accessibility settings
   - Create a preview of changes in real-time

8. Implement responsive behavior:
   - Optimize layout for mobile devices (320px-480px)
   - Adjust positioning and size for different screen sizes
   - Create touch-friendly interactions for mobile
   - Ensure the Familiar doesn't obscure important content on small screens

9. Add RPG-themed visual elements:
   - Design the Familiar as a magical creature (e.g., dragon, phoenix, owl)
   - Add magical particle effects for transitions
   - Use scroll and parchment visuals for the chat interface
   - Include thematic icons and decorations

10. Implement accessibility features:
    - Ensure keyboard navigability
    - Add appropriate ARIA labels and roles
    - Include high contrast mode option
    - Provide text alternatives for visual indicators
    - Support screen readers with meaningful descriptions

## Code Examples, Data Structures & Constraints

### Familiar State Interface
```typescript
interface FamiliarState {
  visualState: 'idle' | 'thinking' | 'speaking' | 'celebrating' | 'confused';
  isExpanded: boolean;
  isMuted: boolean;
  currentConversation: Message[];
  confidenceLevel: number; // 0-100
  characterVariant: string; // based on user class
  position: { x: number; y: number }; // for draggable positioning
  settings: {
    proactivityLevel: 'passive' | 'balanced' | 'proactive';
    notificationFrequency: 'low' | 'medium' | 'high';
    voiceEnabled: boolean;
    animationLevel: 'minimal' | 'standard' | 'elaborate';
  };
}

interface Message {
  id: string;
  sender: 'user' | 'familiar';
  content: string;
  timestamp: Date;
  attachments?: Array<{ type: string; url: string }>;
  confidence?: number; // only for familiar messages
  sources?: Array<{ title: string; url: string }>; // only for familiar messages
  feedbackRating?: 'helpful' | 'not_helpful'; // user feedback
}
```

### Component Props
```typescript
interface FamiliarProps {
  initialState?: Partial<FamiliarState>;
  currentContext?: {
    page: string;
    activity: string;
    relevantSkills: string[];
    currentContent?: string;
  };
  onMessageSend?: (message: string) => Promise<Message>;
  onFeedbackSubmit?: (messageId: string, rating: 'helpful' | 'not_helpful') => void;
  onSettingsChange?: (newSettings: FamiliarState['settings']) => void;
  availableCharacterVariants: string[];
  isDisabled?: boolean;
}
```

### Tech Stack Constraints
- React with TypeScript
- Framer Motion for animations
- Tailwind CSS for styling
- React Context API for state management
- Web Speech API for voice input/output (optional)

### Performance Requirements
- Initial load time under 1 second
- Smooth animations at 60fps
- Minimal impact on main application performance
- Efficient memory usage for long conversations

### Accessibility Requirements
- WCAG 2.1 AA compliance
- Keyboard navigable with clear focus indicators
- Screen reader friendly with appropriate announcements
- Support for reduced motion preferences
- Alternative text-only mode for all interactions

### Do NOT
- Create overly intrusive animations that distract from learning
- Implement auto-playing sounds without user control
- Design tiny touch targets that are difficult to interact with
- Create fixed-position elements that can't be moved or minimized
- Use colors or patterns that could trigger photosensitive conditions

## Define a Strict Scope

### Files to Create
- components/Familiar/index.ts (barrel file)
- components/Familiar/FamiliarContainer.tsx (main component)
- components/Familiar/FamiliarAvatar.tsx (animated character)
- components/Familiar/FamiliarChatInterface.tsx (chat UI)
- components/Familiar/FamiliarContextualHelp.tsx (help panel)
- components/Familiar/FamiliarSettings.tsx (settings UI)
- components/Familiar/FamiliarMessage.tsx (message component)
- components/Familiar/hooks/useFamiliarState.ts (state management)
- components/Familiar/hooks/useFamiliarAnimation.ts (animation logic)
- components/Familiar/types.ts (TypeScript interfaces)
- components/Familiar/utils.ts (helper functions)

### Implementation Priority
1. Basic floating avatar with expand/collapse functionality
2. Chat interface with message history
3. Avatar animations and state transitions
4. Contextual help panel
5. AI transparency features
6. Settings and customization
7. Responsive behavior and mobile optimization
8. Accessibility features
9. RPG-themed visual enhancements

The Familiar should feel like a helpful magical companion that enhances the learning experience without being distracting or intrusive. It should provide genuine educational value while reinforcing the RPG theme of the platform.