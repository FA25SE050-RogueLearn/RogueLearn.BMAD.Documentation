# Information Architecture (IA)

## Site Map / Screen Inventory

```mermaid
graph TD
    A[Landing Page] --> B[Character Creation]
    A --> C[Login/Register]
    
    B --> D[Dashboard - Character Sheet]
    C --> D
    
    D --> E[Quest Log]
    D --> F[Skill Tree]
    D --> G[Arsenal - Notes]
    D --> H[Party Management]
    D --> I[Guild Discovery]
    D --> J[Boss Fights]
    
    E --> E1[Quest Details]
    E --> E2[Quest Progress]
    
    F --> F1[Skill Node Details]
    F --> F2[Learning Resources]
    
    G --> G1[Note Editor]
    G --> G2[Document Upload]
    
    H --> H1[Create Party]
    H --> H2[Party Dashboard]
    H --> H3[Study Sessions]
    
    I --> I1[Guild Details]
    I --> I2[Join Guild]
    
    J --> J1[Boss Fight Arena]
    J --> J2[Results & Rewards]
    
    D --> K[Settings]
    K --> K1[Profile Management]
    K --> K2[Notifications]
    K --> K3[Privacy Controls]
```

## Navigation Structure

**Primary Navigation:** 
- Fixed top navigation bar with RogueLearn logo, notification bell, user avatar, and settings gear
- Main dashboard accessible from logo click
- Quick access to core features: Quest Log, Skill Tree, Arsenal, Party, Guilds

**Secondary Navigation:**
- Contextual navigation within each major section
- Breadcrumb navigation for deep content areas
- Quick action buttons for common tasks (Continue Quest, Create Party, etc.)

**Mobile Navigation:**
- Collapsible hamburger menu for primary navigation
- Bottom tab bar for core features on mobile
- Swipe gestures for quest progression and skill tree navigation