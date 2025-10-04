# Screen Specifications

## 1. Main Dashboard ("Character Sheet")

**Layout:** Single-column mobile, three-column desktop
**Key Components:**
- User avatar and level progress (top section)
- Current quest card with prominent CTA
- Quick access tiles for core features
- Upcoming events timeline
- Recent achievements/notifications

**Interactions:**
- Hover effects on all interactive elements
- Progress bars with smooth animations
- Pull-to-refresh on mobile
- Contextual tooltips for RPG terminology

**Responsive Behavior:**
- Stack components vertically on mobile
- Expand quest card to full width on small screens
- Collapse secondary information into expandable sections

## 2. Character Creation Wizard

**Layout:** Centered single-column with progress indicator
**Steps:**
1. Route Selection (Academic Curriculum)
2. Class Selection (Career Path)
3. Document Upload (Optional)
4. Confirmation & Quest Generation

**Key Features:**
- Clear progress indicator (1 of 4, 2 of 4, etc.)
- Visual previews of selections
- Drag-and-drop document upload
- Real-time validation and feedback
- Skip options for optional steps

## 3. Quest Log Interface

**Layout:** List view with filtering sidebar (desktop), stacked cards (mobile)
**Sections:**
- Active Quests (priority display)
- Upcoming Quests (locked with prerequisites)
- Completed Quests (collapsible history)

**Filtering Options:**
- By difficulty level
- By subject area
- By quest type (learning, assessment, boss fight)
- By time commitment

**Visual Indicators:**
- Difficulty stars (1-5)
- Progress bars
- Lock icons for prerequisites
- Color coding by quest type

## 4. Skill Tree Visualization

**Layout:** Interactive node-based diagram with zoom/pan controls
**Features:**
- Hierarchical skill progression
- Visual connections between related skills
- Progress indicators on each node
- Contextual information panels

**Interactions:**
- Click nodes for detailed information
- Hover for quick previews
- Zoom controls for navigation
- Search functionality for specific skills

**Mobile Adaptations:**
- Touch-friendly node sizing
- Simplified view with expandable sections
- Gesture-based navigation

## 5. Boss Fight Arena

**Layout:** Full-screen immersive experience
**Components:**
- Character vs. Boss visualization
- Readiness assessment display
- Battle objectives and rewards
- Action buttons (Start, Study More, Find Party)

**Unity Integration:**
- WebGL component for interactive battles
- Loading states and error handling
- Performance optimization for mobile
- Fallback UI for unsupported browsers