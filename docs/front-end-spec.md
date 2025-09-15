# **RogueLearn UI/UX Specification**

## **Introduction**

This document defines the user experience goals, information architecture, user flows, and visual design specifications for RogueLearn's user interface. It serves as the foundation for visual design and frontend development, ensuring a cohesive and user-centered experience.

### **Overall UX Goals & Principles**

#### **Target User Personas**

- **Student (Player):** The primary user. They need a clear path to their learning goals, an engaging interface that motivates them, and intuitive tools to manage their academic journey.
- **Guild Master / Verified Lecturer:** Educators or community leaders. They require simple tools to manage their community ("Guild"), share materials, and monitor student progress without being overwhelmed by administrative complexity.
- **Admin (Game Master):** A privileged user focused on platform health and content management. Their interface should be functional, data-rich, and efficient.

#### **Usability Goals**

- **Engaging Onboarding:** A new user can create their account, understand the core concepts, and start their first quest within 10 minutes.
- **Effortless Focus:** The UI should minimize distractions, allowing students to easily focus on their current quest or learning task.
- **Clarity of Progress:** Users should be able to understand their current academic standing, what they need to do next, and how their tasks connect to their long-term goals at a glance.
- **High Memorability:** An infrequent user can return to the platform after a week and immediately pick up where they left off without needing to re-learn the interface.

#### **Design Principles**

1. **Immersive & Thematic:** Every element, from buttons to backgrounds, should reinforce the "Adventurer's Journal" or "Magical Scribe" theme to make learning an enchanting experience.
2. **Clarity Over Cleverness:** The primary goal is education. UI patterns should be intuitive and clear, even if it means being less novel.
3. **Progressive Disclosure:** Show users only what they need for their current task. Keep advanced options or detailed stats accessible but not intrusive to avoid overwhelming them.
4. **Actionable Feedback:** Every user action, especially in a learning context (like answering a question), must have immediate, clear, and encouraging visual and textual feedback.
5. **Accessible by Default:** The design must meet WCAG 2.1 AA standards to ensure that all learners, regardless of ability, can use the platform effectively.

## **Information Architecture (IA)**

### **Site Map / Screen Inventory**

This diagram shows the primary screens of the application and how they are accessed.

```mermaid
graph TD
    A[Unauthenticated User] --> B[/welcome];
    B --> C[/login];
    B --> D[/signup];
    
    subgraph Authenticated Experience
        E[User signs in] --> F[/dashboard];
        F --> G[/quests];
        F --> H[/skill-tree/...];
        F --> I[/arsenal];
        F --> J[/community/guilds];
        F --> K[/settings];
        
        G --> G1[/quests/[questId]];
        G1 --> G2["/quests/boss-fight/[fightId] (Unity Embed)"];
        
        I --> I1[/arsenal/notes/[noteId]];
        
        J --> J1[/community/guilds/[guildId]];
    end
    
    D --> L[/onboarding];
    L --> F;

```

### **Navigation Structure**

- **Primary Navigation (Authenticated):** After logging in, the user will have a persistent sidebar or top navigation bar with direct links to the main sections:
  - Dashboard
  - Quests
  - Skill Tree
  - Arsenal
  - Community (Guilds)
  - Settings
- **Breadcrumb Strategy:** Breadcrumbs will be used on nested pages (like viewing a specific quest or guild) to show the user's current location within the hierarchy and allow for easy navigation back to parent pages. Example: `Community > Guilds > The Art of Code > Announcements`.

## **User Flows**

### **Flow 1: New User Onboarding ("Character Creation")**

**User Goal:** To create an account, set up their initial academic and career profile, and be ready to start their first learning quest.

**Entry Point:** User clicks the "Sign Up" button on the `/welcome` page.

**Success Criteria:** The user has successfully created an account, completed the multi-step onboarding process, and is redirected to their personalized `/dashboard`. A `UserProfile` record is created and fully populated in the database.

#### **Flow Diagram**

```mermaid
graph TD
    A[Start: Click 'Sign Up'] --> B[Screen: Create Account];
    B --> C{Submit Credentials};
    C -- Success --> D[Supabase Auth Creates User];
    D --> E[DB Trigger Creates Profile];
    E --> F[Redirect to /onboarding];
    
    subgraph Onboarding Wizard
        direction LR
        F --> G[Step 1: Welcome & Tutorial];
        G --> H[Step 2: Select 'Class' (Career Goal)];
        H --> I[Step 3: Select 'Curriculum' (Academic Path)];
        I --> J[Step 4: Optional Document Upload];
    end

    J --> K[Screen: Onboarding Complete!];
    K --> L[Redirect to /dashboard];
    L --> M[End: View Personalized Dashboard];
    
    C -- Failure --> N[Show Error: e.g., 'Email already in use'];
    N --> B;
```

#### **Edge Cases & Error Handling:**

- **User closes browser mid-onboarding:** The system should save their progress. Upon next login, the user should be prompted to resume onboarding from the last completed step.
- **Back navigation:** The user must be able to navigate back to previous steps in the wizard to change their selections.
- **Document upload failure:** If an optional document upload fails, the user should be clearly notified, but this should not block them from completing the onboarding process.

## **Wireframes & Mockups**

**Primary Design Files:** [Link to Figma/Sketch/XD Project - TBD]

### **Key Screen Layouts**

#### **Screen: Dashboard ("Character Sheet")**

**Purpose:** To serve as the user's central hub, providing an at-a-glance overview of their character, current progress, and immediate next steps. This screen should feel personal and motivating.

**Key Elements (Mobile-First Layout):**

1. **Header:**
    - User Avatar & Username
    - Level and XP Progress Bar
2. **"Active Quest" Card:**
    - A prominent card displaying the title of their most immediate or pinned quest.
    - A brief description of the quest objective.
    - A clear "Continue Quest" call-to-action button.
3. **"Character Stats" Section:**
    - Displays the user's selected Class (e.g., "Full-Stack Developer").
    - Displays the user's selected Curriculum (e.g., "Computer Science").
    - Shows key game-like stats derived from their academic data.
4. **"Upcoming Events" List:**
    - A small section listing the next 2-3 upcoming due dates or events (e.g., "Exam: Data Structures - in 3 days").
5. **"Skill Tree Snapshot" Visual:**
    - A mini-map or a summary graphic showing recent progress in their primary Skill Tree.

**Interaction Notes:**

- The layout should be a single, scrollable column on mobile.
- On desktop, the layout could expand to a two or three-column grid, allowing more information to be visible simultaneously.
- All elements, especially the XP bar and quest card, should be highly interactive and provide feedback on hover or tap.

**Design File Reference:** [Link to specific Dashboard frame/artboard in Figma - TBD]

## **Component Library / Design System**

**Design System Approach:** We will use **Shadcn/UI** as the foundation for our component library. This is not a traditional component library package, but a collection of reusable, unstyled components that we will install as needed. We will then apply our custom "Magical Scribe" theme to these components to create our unique design system. This approach gives us full ownership and control over the code and styling.

### **Core Components**

The following is an initial list of foundational components we will need to build or customize to create the RogueLearn interface.

#### **Component: Button**

- **Purpose:** The primary interactive element for all actions.
- **Variants:**
  - `Primary`: For main calls-to-action (e.g., "Start Quest").
  - `Secondary`: For less important actions (e.g., "View Details").
  - `Destructive`: For actions that delete data (e.g., "Delete Note").
- **States:** `Default`, `Hover`, `Pressed`, `Disabled`, `Loading`.
- **Usage Guidelines:** Primary buttons should be used sparingly to guide the user to the most important action on a screen.

#### **Component: Card**

- **Purpose:** A container for grouping related information, styled to look like aged parchment or a panel from an adventurer's journal.
- **Variants:**
  - `QuestCard`: Used on the dashboard and quest log.
  - `NoteCard`: Used in the Arsenal view.
  - `StatCard`: Used on the dashboard to display character stats.
- **States:** `Default`, `Hover` (may have a subtle glow or lift effect).
- **Usage Guidelines:** Cards form the basic layout grid of our application and must be responsive.

#### **Component: Progress Bar**

- **Purpose:** To visually represent the user's progress, primarily for XP and Levels.
- **Variants:**
  - `XPBar`: A prominent bar, possibly with a magical or glowing effect.
  - `QuestProgressBar`: A smaller, simpler bar to show completion of a multi-step quest.
- **States:** `Default`, `Animating` (when XP is added).
- **Usage Guidelines:** The main XP bar should be a central and satisfying element of the dashboard.

#### **Component: Input & Form Elements**

- **Purpose:** For all user data entry, including login, signup, and settings.
- **Variants:** `TextInput`, `PasswordInput`, `SelectDropdown`, `Checkbox`.
- **States:** `Default`, `Focused`, `Filled`, `Error`, `Disabled`.
- **Usage Guidelines:** Forms should be styled thematically but must prioritize readability and accessibility, with clear labels and error messages.

## **Branding & Style Guide**

### **Visual Identity**

**Brand Guidelines:** The visual identity is anchored by the "R" logo provided. The aesthetic is that of a "Magical Scribe's Journal"—scholarly, mystical, and clean. It combines the gravitas of ancient texts with the clarity of a modern digital interface.

### **Color Palette**

| Color Type | Hex Code    | Usage                                                   |
| :--------- | :---------- | :------------------------------------------------------ |
| Primary    | `#5C0029`   | Main backgrounds, headers, footers, primary buttons.    |
| Secondary  | `#3D001B`   | Darker accents, card borders, secondary UI elements.    |
| Accent     | `#DDCBA4`   | Call-to-action buttons, highlights, links, active text. |
| Success    | `#2E7D32`   | Confirmation messages, success states, completed tasks. |
| Warning    | `#FFC107`   | Important notices, non-critical alerts.               |
| Error      | `#C62828`   | Error messages, destructive action confirmations.       |
| Neutral    | `#F5F5F5` (light text), `#1A1A1A` (dark bg element) | Body text, borders, subtle backgrounds. |

### **Typography**

#### **Font Families**

- **Primary (Headings):** **Lora** (A well-balanced serif with a calligraphic feel, evoking a classic, scholarly tone).
- **Secondary (Body):** **Nunito Sans** (A clean, highly readable sans-serif that ensures accessibility for all text content).
- **Monospace (Code):** **Source Code Pro** (For any code snippets or technical text).

#### **Type Scale**

| Element | Size   | Weight   | Line Height |
| :------ | :----- | :------- | :---------- |
| H1      | 48px   | 700      | 1.2         |
| H2      | 36px   | 700      | 1.3         |
| H3      | 24px   | 600      | 1.4         |
| Body    | 16px   | 400      | 1.6         |
| Small   | 14px   | 400      | 1.5         |

### **Iconography**

**Icon Library:** **Lucide Icons**. This is a clean, modern icon set that is the default for Shadcn/UI. We will style the icons with our accent color to ensure they fit our theme while remaining clear and recognizable.

### **Spacing & Layout**

**Grid System:** The layout will be based on a standard **8-point grid system**. All spacing (margins, padding) will use multiples of 8px (8, 16, 24, 32, etc.). This ensures a consistent and harmonious rhythm across the entire interface.

## **Accessibility Requirements**

### **Compliance Target**

**Standard:** This project will adhere to the **Web Content Accessibility Guidelines (WCAG) 2.1 Level AA**. All components and user flows must meet this standard before they are considered "done."

### **Key Requirements**

**Visual:**

- **Color Contrast:** All text must have a contrast ratio of at least **4.5:1** against its background. Large text (18pt/24px or 14pt/18.5px bold) must have a ratio of at least **3:1**. The chosen palette of `#DDCBA4` on `#5C0029` meets this for large text, and we will use a lighter variant like `#F5F5F5` for body text to ensure compliance.
- **Focus Indicators:** All interactive elements (links, buttons, form fields) MUST have a clear and highly visible focus state when navigated to via a keyboard.
- **Information Not Conveyed by Color Alone:** Color will not be the only method used to convey information. For example, error fields will have both a red border and an error icon/text.

**Interaction:**

- **Keyboard Navigation:** All functionality must be completely usable with a keyboard alone. The navigation order must be logical and intuitive.
- **Screen Reader Support:** The application will be tested for compatibility with major screen readers (JAWS, NVDA, VoiceOver). Semantic HTML and ARIA attributes will be used correctly to describe roles, states, and properties of all UI elements.
- **Touch Targets:** All interactive elements on mobile devices will have a minimum touch target size of 44x44 pixels.

**Content:**

- **Alternative Text:** All meaningful images will have descriptive `alt` text. Decorative images will have an empty `alt=""` attribute.
- **Heading Structure:** All pages will use a logical and hierarchical heading structure (`<h1>`, `<h2>`, etc.) to facilitate navigation and understanding.
- **Form Labels:** All form inputs will have a programmatically associated `<label>` to ensure clarity for all users.

### **Testing Strategy**

Accessibility will be tested at multiple stages:

1. **Automated Testing:** We will integrate an automated accessibility checker (like `axe-core`) into our CI/CD pipeline to catch common issues.
2. **Manual Keyboard Testing:** QA will perform manual keyboard-only navigation tests on all new features.
3. **Screen Reader Testing:** Key user flows will be manually tested using a screen reader before major releases.

## **Responsiveness Strategy**

Our design approach will be **mobile-first**. This means we will design the simplest, single-column layout for small screens first, and then progressively enhance the layout for larger screens.

### **Breakpoints**

We will use a standard set of breakpoints that are commonly used with Tailwind CSS to manage our responsive layouts.

| Breakpoint | Min Width | Target Devices                                     |
| :--------- | :-------- | :------------------------------------------------- |
| **sm**     | 640px     | Large phones, small tablets (in portrait)          |
| **md**     | 768px     | Tablets (in portrait), small laptops               |
| **lg**     | 1024px    | Tablets (in landscape), standard laptops & desktops |
| **xl**     | 1280px    | Large desktops                                     |
| **2xl**    | 1536px    | Very large desktops, high-resolution monitors      |

### **Adaptation Patterns**

- **Layout Changes:** On mobile, the app will primarily use a single-column layout. As the screen size increases past the `md` and `lg` breakpoints, layouts will adapt to use multi-column grids (e.g., the Dashboard may shift from a single column of cards to a two or three-column grid).
- **Navigation Changes:** On mobile (`< md`), the primary navigation will be collapsed into a hamburger menu. On tablet and desktop screens (`>= md`), the primary navigation will be displayed persistently as a sidebar or a horizontal top bar.
- **Content Priority:** The mobile-first approach forces us to prioritize the most essential content to be displayed first. Less critical information or supplementary controls may be hidden behind "show more" toggles on smaller screens and become visible by default on larger screens.
- **Interaction Changes:** Hover-based interactions designed for desktop will have equivalent tap-based interactions on touch devices. Touch targets will be large and easy to press on all screen sizes.

## **Animation & Micro-interactions**

Motion will be used purposefully to guide the user, provide feedback, and enhance the "magical" theme of the application. Animations should be smooth, performant, and never get in the way of usability.

### **Motion Principles**

- **Purposeful Motion:** Animation should have a clear purpose, such as drawing attention to a change, indicating a state transition, or providing feedback for an action. Gratuitous animation will be avoided.
- **Thematic Flair:** Motion should feel magical and fluid. Think of smooth fades, glowing effects, and subtle particle bursts rather than harsh, mechanical movements.
- **Performant & Responsive:** All animations must be highly performant, primarily using CSS transforms and opacity to ensure they run smoothly even on less powerful devices. They should feel like an immediate response to user input.

### **Key Animations & Micro-interactions**

- **Button & Card Hover:** Interactive elements will have a subtle "glow" or "lift" effect on hover to indicate they are clickable, reinforcing the magical theme.
- **XP Gain:** When a user earns experience points, the XP bar on the dashboard will animate smoothly to the new value, accompanied by a brief, celebratory particle effect.
- **Quest Completion:** Completing a quest will trigger a satisfying "Quest Complete" modal with a flourish animation, similar to completing a level in an RPG.
- **Page Transitions:** Navigating between major pages will use a quick, subtle cross-fade transition to create a smoother, more seamless experience than an instant screen change.
- **Loading States:** Loading skeletons and spinners will have a subtle "breathing" or "pulsing" animation to show that the system is active and working.

## **Performance Considerations**

A fast, responsive application is critical for maintaining user engagement, especially in a learning environment. The user experience should feel quick and fluid.

### **Performance Goals (User-Centric Metrics)**

- **Core Web Vitals:** The application must aim for a "Good" score (Green) across all three Core Web Vitals (LCP, FID/INP, CLS) as measured by Google's PageSpeed Insights.
- **Interaction Response:** All interactive elements (buttons, tabs, inputs) must respond to user input in under **100ms**.
- **Animation FPS:** All animations must maintain a consistent **60 frames per second (FPS)** to feel smooth and fluid.

### **Design Strategies to Enhance Performance**

- **Optimistic UI Updates:** For actions that are likely to succeed (e.g., adding a simple note), the UI will update immediately while the request is being sent to the server. An undo or error state will be shown if the server request fails.
- **Prioritize Above-the-Fold Content:** The initial view of any page must load and become interactive as quickly as possible. Content that is "below the fold" (off-screen) will be lazy-loaded as the user scrolls.
- **Loading Skeletons:** When fetching data, the application will display loading "skeletons" that mimic the shape of the final content. This makes the wait time feel shorter and prevents jarring layout shifts when the content arrives.
- **Minimize Heavy Assets:** Decorative images and custom fonts will be optimized and compressed to ensure they do not significantly impact page load times.


## **Next Steps**

This UI/UX Specification document provides the foundational design direction for the RogueLearn platform. The following steps should be taken to move the project forward.

### **Immediate Actions**
1.  **Stakeholder Review:** This document should be reviewed and approved by all key stakeholders, including the Product Manager and Lead Architect, to ensure alignment across teams.
2.  **High-Fidelity Mockups:** A visual designer should now use this specification to create high-fidelity mockups in Figma (or the chosen design tool). The mockups should adhere to the Branding & Style Guide and implement the layouts defined in the Wireframes section.
3.  **Frontend Architecture Handoff:** Once mockups are in progress, this document will be handed off to the Architect to create the detailed `front-end-architecture.md` document, which will define the technical implementation plan for the frontend.
4.  **Prototype Critical Flows:** Consider creating interactive prototypes in Figma for the most complex user flows (like Onboarding and the Boss Fight) to test and refine the user experience before development begins.

### **Design Handoff Checklist**
This checklist ensures a smooth transition from UX design to frontend development. The following items must be complete before the development team begins implementation:

- [x] All user flows documented and approved.
- [x] Core component inventory is defined.
- [x] Accessibility requirements (WCAG 2.1 AA) are specified.
- [x] The responsiveness strategy and breakpoints are clear.
- [x] The branding and style guide is complete.
- [x] Performance goals have been established.
- [ ] **(Pending)** High-fidelity mockups for all core screens are complete and available in Figma.
- [ ] **(Pending)** A detailed `front--architecture.md` document has been created and approved.