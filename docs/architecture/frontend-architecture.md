# Frontend Architecture

## Component Structure

Following the Atomic Design methodology as specified in the frontend spec:

1. **Atoms**
   - Buttons, inputs, icons, typography
   - RPG-themed UI elements (health bars, experience indicators)

2. **Molecules**
   - Form groups, card components, navigation items
   - Skill nodes, quest cards, arsenal item displays

3. **Organisms**
   - Navigation bars, forms, complex widgets
   - Skill tree visualization, quest logs, party management

4. **Templates**
   - Page layouts and structures
   - Dashboard templates, profile layouts, arsenal views

5. **Pages**
   - Complete screens combining multiple organisms and templates
   - Dashboard, profile, skill tree, arsenal, party pages

## State Management

1. **React Context API**
   - User authentication state
   - Theme and preferences
   - Simple application state

2. **Redux**
   - Complex global state
   - Skill tree data and relationships
   - Quest and progress tracking

3. **React Query**
   - Server state management
   - Data fetching and caching
   - Optimistic updates

## Routing and Navigation

Next.js routing will be used with the following structure:

- `/` - Landing page and onboarding
- `/dashboard` - Main user dashboard
- `/skills` - Skill tree visualization
- `/arsenal` - Learning materials collection
- `/quests` - Current and upcoming quests
- `/bossfights` - Exam preparation and tracking
- `/parties` - Study group management
- `/profile` - User profile and settings

## Browser Extension Architecture

The browser extension will be built using the WebExtension API and will consist of:

1. **Background Script**
   - Handles authentication and API communication
   - Manages extension state and settings
   - Coordinates between content scripts and popup

2. **Content Scripts**
   - Analyze web page content for relevant skills
   - Provide highlighting and capture functionality
   - Track progress on learning materials

3. **Popup UI**
   - Shows current quests and progress
   - Provides quick access to arsenal
   - Displays mini skill tree for current content

4. **Storage**
   - Local storage for offline capability
   - Synced storage for user preferences
   - IndexedDB for larger content storage