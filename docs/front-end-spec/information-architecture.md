# Information Architecture (IA)

## Site Map / Screen Inventory

```mermaid
graph TD
    A[Landing Page] --> B[Character Creation]
    A --> C[Login/Register]
    
    B --> D[Dashboard - Character Sheet]
    C --> D
    
    D --> E[Learning Progress]
    D --> F[Skill Tree]
    D --> G[Arsenal - Notes]
    D --> H[Party Management]
    D --> I[Guild Discovery]
    D --> J[Boss Fights]
    D --> K[Event Competitions]
    
    E --> E1[Learning Module Details]
    E --> E2[Learning Progress Tracking]
    E --> E3[AI-Driven Learning Paths]
    
    F --> F1[Skill Node Details]
    F --> F2[Learning Resources]
    
    G --> G1[Note Editor]
    G --> G2[Document Upload]
    G --> G3[Study Material Library]
    G --> G4[Personal Knowledge Base]
    
    H --> H1[Create Party]
    H --> H2[Party Dashboard]
    H --> H3[Study Sessions]
    
    I --> I1[Guild Details]
    I --> I2[Join Guild]
    I --> I3[Guild Leaderboards]
    
    J --> J1[Boss Fight Arena]
    J --> J2[Results & Rewards]
    
    K --> K1[Live Free-for-All Events]
    K --> K2[Guild Leaderboards]
    K --> K3[Individual Rankings]
    K --> K4[Event History]
    K --> K5[Live Event Dashboard]
    K --> K6[Post-Event Analytics]
    
    D --> L[Settings]
    L --> L1[Profile Management]
    L --> L2[Notifications]
    L --> L3[Privacy Controls]
```

## Navigation Structure

**Primary Navigation:** 
- Fixed top navigation bar with RogueLearn logo, notification bell, user avatar, and settings gear
- Main dashboard accessible from logo click
- Quick access to core features: Learning Progress, Skill Tree, Arsenal, Party, Guilds, Code Battles

**Secondary Navigation:**
- Contextual navigation within each major section
- Breadcrumb navigation for deep content areas
- Quick action buttons for common tasks (Continue Learning, Create Party, etc.)

**Mobile Navigation:**
- Collapsible hamburger menu for primary navigation
- Bottom tab bar for core features on mobile
- Swipe gestures for learning progression and skill tree navigation