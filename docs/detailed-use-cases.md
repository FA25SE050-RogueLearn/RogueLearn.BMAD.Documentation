# **Detailed Use Cases**

This document provides comprehensive use case specifications for the RogueLearn platform, covering all major user interactions and system functionalities. Each use case includes detailed scenarios, requirement mappings, and visual flowcharts to support development, testing, and stakeholder communication.

---

## UC-001: User Registration and Curriculum-Career Onboarding

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | User Registration and Route/Class Character Creation |
| **Description** | New users create accounts and complete the curriculum-first character creation process to establish their academic-career gaming profile with Route (curriculum) and Class (career specialization) integration |
| **Actor(s)** | Primary: **New Player**<br>Secondary: AI System, Supabase Authentication, Roadmap.sh Integration |
| **Preconditions** | User has access to the platform and curriculum information |
| **Postconditions (Success Guarantee)** | User account created with curriculum-based character profile, gap analysis completed, and integrated skill tree with career enhancement |
| **Related Requirements** | **FRs:** FR1 (Route-Class Creation), FR2 (Curriculum Processing), FR4 (Gap Analysis), FR4A (Roadmap.sh Integration), FR10 (Character Dashboard), FR48 (FPTU Student Verification), FR49 (Quest Memory & Continuity)<br>**NFRs:** NFR2 (Supabase Auth), NFR13 (Security), NFR14 (Data Privacy) |
| **Main Success Scenario** | 1. User navigates to registration page<br>2. System presents curriculum-first character creation flow<br>3. User selects Route (academic curriculum) from available programs<br>4. User chooses Class (career specialization) from roadmap.sh options<br>5. System performs gap analysis between curriculum and career requirements<br>6. AI generates curriculum-based quest lines with roadmap.sh supplements<br>7. User optionally uploads academic documents for skill tree enhancement<br>8. System creates account via Supabase Authentication<br>9. User accesses personalized dashboard with Route/Class integration |
| **Alternative Flows** | **A1:** Social login - User authenticates via Google/GitHub<br>**A2:** Skip document upload - System uses curriculum and roadmap.sh data only<br>**A3:** Gap analysis review - User reviews and approves curriculum-career integration plan |
| **Exception Flows** | **E1:** Authentication failure - System provides alternative registration methods<br>**E2:** Gap analysis error - System uses default roadmap.sh integration<br>**E3:** Network interruption - System saves Route/Class selection and allows resumption |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[User navigates to registration] --> B[System presents curriculum-first creation]
    B --> C[User selects Route - academic curriculum]
    C --> D[User chooses Class - career specialization]
    D --> E[System performs gap analysis]
    E --> F{Gap analysis successful?}
    
    %% Main Success Flow
    F -->|Yes| G[AI generates curriculum + roadmap.sh quests]
    G --> H[User optionally uploads documents]
    H --> I{Authentication method?}
    I -->|Standard| J[System creates account via Supabase]
    I -->|Social Login - A1| K[User authenticates via Google/GitHub]
    K --> J
    J --> L{Account creation successful?}
    L -->|Yes| M[User accesses Route/Class dashboard]
    
    %% Alternative Flows
    H -->|Skip upload - A2| J
    E -->|Gap analysis review - A3| N[User reviews curriculum-career integration plan]
    N --> O{User approves plan?}
    O -->|Yes| G
    O -->|No| P[User modifies Route/Class selection]
    P --> D
    
    %% Exception Flows
    F -->|No - E2| Q[System uses default roadmap.sh integration]
    Q --> G
    L -->|No - E1| R[System provides alternative registration methods]
    R --> S{Alternative successful?}
    S -->|Yes| M
    S -->|No| T[Registration fails - user can retry]
    T --> A
    
    %% Network interruption handling - E3
    D -.->|Network interruption| U[System saves Route/Class selection]
    U -.-> V[Allow resumption when connection restored]
    V -.-> E
    
    %% Styling
    classDef mainFlow fill:#e1f5fe
    classDef altFlow fill:#f3e5f5
    classDef excFlow fill:#ffebee
    classDef decision fill:#fff3e0
    
    class A,B,C,D,E,G,H,J,M mainFlow
    class K,N,O,P altFlow
    class Q,R,S,T,U,V excFlow
    class F,I,L,O,S decision
```

---

## UC-002: Academic Document Enhancement and Skill Tree Integration

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Academic Document Upload for Skill Tree Enhancement |
| **Description** | Students optionally upload academic documents to enhance their curriculum-based skill tree visualization and arsenal management, not for quest generation |
| **Actor(s)** | Primary: **Player**<br>Secondary: AI Processing System |
| **Preconditions** | User has completed Route/Class setup and has academic documents available |
| **Postconditions (Success Guarantee)** | Document processed and integrated to enhance skill tree visualization and arsenal organization |
| **Related Requirements** | **FRs:** FR2 (Document Enhancement), FR8 (Skill Tree Enhancement), FR16 (Arsenal Management), FR48 (FPTU Student Verification)<br>**NFRs:** NFR6 (Supabase Storage), NFR9 (Performance), NFR20 (Content Management) |
| **Main Success Scenario** | 1. Player selects "Enhance Skill Tree" from dashboard<br>2. Player uploads academic document (PDF, DOCX, TXT) for skill tree enhancement<br>3. System validates file format, size, and academic content relevance<br>4. AI analyzes document to identify skill connections and knowledge areas<br>5. Player monitors processing progress via real-time updates<br>6. AI enhances existing curriculum-based skill tree with document insights<br>7. System improves skill tree visualization with document-derived connections<br>8. Document content is organized in Arsenal for enhanced knowledge management<br>9. Skill tree displays enhanced visualization with academic document integration |
| **Alternative Flows** | **A1:** Multiple document enhancement - System processes documents to improve skill tree comprehensively<br>**A2:** Document categorization - System organizes documents by curriculum topics and career areas<br>**A3:** Skill connection mapping - AI identifies and visualizes connections between document content and curriculum skills |
| **Exception Flows** | **E1:** Irrelevant document content - System provides feedback on document relevance to curriculum<br>**E2:** Enhancement processing fails - System maintains original curriculum-based skill tree<br>**E3:** Document too large - System suggests content extraction for skill tree enhancement |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[Player selects Enhance Skill Tree] --> B[Player uploads academic document]
    B --> C{File validation successful?}
    
    %% Main Success Flow
    C -->|Yes| D[AI analyzes for skill connections]
    D --> E[Player monitors enhancement progress]
    E --> F{Processing successful?}
    F -->|Yes| G[AI enhances curriculum skill tree]
    G --> H[Document organized in Arsenal]
    H --> I{Enhancement type?}
    I -->|Single document| J[Enhanced skill tree visualization]
    I -->|Multiple documents - A1| K[System processes documents comprehensively]
    K --> L[Comprehensive skill tree improvement]
    L --> J
    
    %% Alternative Flows
    H -->|Document categorization - A2| M[System organizes by curriculum topics and career areas]
    M --> N[Categorized Arsenal organization]
    N --> J
    
    G -->|Skill connection mapping - A3| O[AI identifies and visualizes connections]
    O --> P[Enhanced skill connections displayed]
    P --> H
    
    %% Exception Flows
    C -->|No - File issues| Q{Issue type?}
    Q -->|Irrelevant content - E1| R[System provides feedback on document relevance]
    Q -->|Document too large - E3| S[System suggests content extraction]
    R --> T[Player can upload different document]
    S --> U[Player extracts relevant content]
    T --> B
    U --> B
    
    F -->|No - E2| V[Enhancement processing fails]
    V --> W[System maintains original curriculum-based skill tree]
    W --> X[Player notified of processing failure]
    X --> Y{Retry?}
    Y -->|Yes| D
    Y -->|No| Z[Keep original skill tree]
    
    %% Styling
    classDef mainFlow fill:#e1f5fe
    classDef altFlow fill:#f3e5f5
    classDef excFlow fill:#ffebee
    classDef decision fill:#fff3e0
    
    class A,B,D,E,G,H,J mainFlow
    class K,L,M,N,O,P altFlow
    class Q,R,S,T,U,V,W,X,Y,Z excFlow
    class C,F,I,Q,Y decision
```

---

## UC-003: Curriculum-Based Skill Tree Visualization and Career Navigation

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Curriculum-Career Skill Tree Visualization and Navigation |
| **Description** | Students interact with their curriculum-based skill tree enhanced with career specialization to track academic progress and navigate Route/Class learning pathways |
| **Actor(s)** | Primary: **Player**<br>Secondary: System, Gap Analysis Engine |
| **Preconditions** | User has completed Route/Class setup and has curriculum-based skill tree data |
| **Postconditions (Success Guarantee)** | User successfully navigates curriculum-career integrated skill tree and understands academic-career progression |
| **Related Requirements** | **FRs:** FR8 (Curriculum Skill Tree), FR4A (Career Integration), FR10 (Route/Class Dashboard)<br>**NFRs:** NFR1 (Responsiveness), NFR5 (Next.js Frontend) |
| **Main Success Scenario** | 1. User accesses skill tree from Route/Class dashboard<br>2. System displays curriculum-based skill tree with career enhancement visualization<br>3. User explores curriculum branches and career specialization pathways<br>4. System shows academic progress, completed curriculum skills, and career development areas<br>5. User selects specific skills to view curriculum requirements and career relevance<br>6. System displays skill prerequisites, learning resources, and related curriculum/career quests<br>7. User identifies next academic objectives and career enhancement opportunities from gap analysis |
| **Alternative Flows** | **A1:** Filter by Route/Class - User focuses on curriculum requirements vs. career specialization<br>**A2:** Gap analysis view - User explores curriculum-career gaps and roadmap.sh supplements<br>**A3:** Progress comparison - User compares academic progress with career readiness metrics |
| **Exception Flows** | **E1:** Skill tree loading error - System provides cached curriculum version with sync notification<br>**E2:** Large curriculum tree performance - System implements progressive loading by academic year/semester |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[User accesses skill tree from Route/Class dashboard] --> B[System displays curriculum-based skill tree with career enhancement visualization]
    B --> C[User explores curriculum branches and career specialization pathways]
    C --> D[System shows academic progress, completed curriculum skills, and career development areas]
    D --> E[User selects specific skills to view curriculum requirements and career relevance]
    E --> F[System displays skill prerequisites, learning resources, and related curriculum/career quests]
    F --> G[User identifies next academic objectives and career enhancement opportunities from gap analysis]
    
    %% Alternative Flows
    C -->|Filter by Route/Class - A1| H[User focuses on curriculum requirements vs. career specialization]
    H --> I[System displays filtered view by Route or Class]
    I --> D
    
    D -->|Gap analysis view - A2| J[User explores curriculum-career gaps and roadmap.sh supplements]
    J --> K[System highlights gap areas and recommended supplements]
    K --> G
    
    F -->|Progress comparison - A3| L[User compares academic progress with career readiness metrics]
    L --> M[System displays comparative analytics]
    M --> G
    
    %% Exception Flows
    B -->|Skill tree loading error - E1| N[System provides cached curriculum version with sync notification]
    N --> O[User notified of sync status]
    O --> P{Retry sync?}
    P -->|Yes| B
    P -->|No| Q[Continue with cached version]
    Q --> C
    
    C -->|Large curriculum tree performance - E2| R[System implements progressive loading by academic year/semester]
    R --> S[Load curriculum sections progressively]
    S --> D
    
    %% Styling
    classDef mainFlow fill:#e1f5fe
    classDef altFlow fill:#f3e5f5
    classDef excFlow fill:#ffebee
    classDef decision fill:#fff3e0
    
    class A,B,C,D,E,F,G mainFlow
    class H,I,J,K,L,M altFlow
    class N,O,Q,R,S excFlow
    class P decision
```

---

## UC-004: AI-Powered Curriculum Quest Generation and Career Enhancement

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | AI-Powered Curriculum Quest Generation and Career Enhancement |
| **Description** | System uses AI to analyze selected curriculum (Route) and career specialization (Class) to generate personalized learning quests with gap analysis and roadmap.sh integration for comprehensive academic and career development |
| **Actor(s)** | Primary: AI Quest Engine<br>Secondary: **Player**, System, Roadmap.sh API |
| **Preconditions** | User has completed curriculum-career onboarding with Route/Class selection |
| **Postconditions (Success Guarantee)** | Curriculum-based quests generated with career enhancement integration, gap analysis completed, and progress tracking enabled for both academic and career tracks |
| **Related Requirements** | **FRs:** FR9 (Quest Generation), FR11 (Quest Completion), FR20 (Quest Modification), FR4 (Gap Analysis), FR4A (Roadmap.sh Integration), FR49 (Quest Memory & Continuity), FR51 (Adaptive Quest Generation for Failed Students)<br>**NFRs:** NFR8 (Golang Backend), NFR9 (Performance) |
| **Main Success Scenario** | 1. AI analyzes selected Route (curriculum) and Class (career specialization)<br>2. System performs gap analysis between curriculum requirements and career goals<br>3. AI integrates roadmap.sh data for career-specific skill requirements<br>4. System generates curriculum-based primary quests aligned with academic objectives<br>5. AI creates career enhancement quests based on gap analysis and roadmap.sh<br>6. System presents integrated quest sequences in dashboard with clear academic/career distinctions<br>7. User engages with curriculum and career quests<br>8. AI evaluates responses and provides feedback for both tracks<br>9. System updates progress for academic curriculum and career specialization<br>10. AI adapts future quests based on performance in both domains |
| **Alternative Flows** | **A1:** Curriculum-only focus - User temporarily disables career enhancement quests<br>**A2:** Career-priority mode - System emphasizes gap-filling and roadmap.sh quests<br>**A3:** Document-enhanced quests - Optional academic documents provide additional context for quest generation |
| **Exception Flows** | **E1:** Roadmap.sh API failure - System uses cached career data and continues with curriculum quests<br>**E2:** Gap analysis incomplete - System generates basic curriculum quests while processing continues<br>**E3:** Quest completion error - System saves progress separately for academic and career tracks |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[AI analyzes Route/Class selection] --> B[Perform gap analysis between curriculum and career]
    B --> C{Gap analysis successful?}
    
    %% Main Success Flow
    C -->|Yes| D[Integrate roadmap.sh data for career requirements]
    D --> E{Roadmap.sh integration successful?}
    E -->|Yes| F[Generate curriculum-based primary quests]
    F --> G[Create career enhancement quests from gap analysis]
    G --> H[Present integrated quest dashboard]
    H --> I{User quest preference?}
    I -->|Both tracks| J[User engages with curriculum and career quests]
    I -->|Curriculum focus - A1| K[User focuses on academic quests only]
    I -->|Career priority - A2| L[User emphasizes career enhancement quests]
    J --> M[AI evaluates responses for both academic and career tracks]
    K --> N[AI evaluates academic progress]
    L --> O[AI evaluates career development progress]
    M --> P[Update progress for both domains]
    N --> Q[Update academic curriculum progress]
    O --> R[Update career specialization progress]
    P --> S[AI adapts future quests based on dual-track performance]
    Q --> S
    R --> S
    
    %% Alternative Flows
    H -->|Document enhancement - A3| T[User uploads academic documents for context]
    T --> U[AI incorporates document insights into quest generation]
    U --> V[Enhanced quests with document-specific content]
    V --> J
    
    %% Exception Flows
    E -->|No - Roadmap.sh failure - E1| W[System uses cached career data]
    W --> X[Continue with curriculum quests and cached career info]
    X --> Y[User notified of limited career integration]
    Y --> H
    
    C -->|No - Gap analysis incomplete - E2| Z[System generates basic curriculum quests]
    Z --> AA[Gap analysis continues in background]
    AA --> BB[User starts with curriculum-only quests]
    BB --> CC{Gap analysis completed?}
    CC -->|Yes| D
    CC -->|No| DD[Continue with curriculum focus]
    DD --> N
    
    M -->|Quest completion error - E3| EE[System saves progress separately]
    EE --> FF{Which track failed?}
    FF -->|Academic| GG[Save career progress, retry academic]
    FF -->|Career| HH[Save academic progress, retry career]
    FF -->|Both| II[Save partial progress, retry both]
    GG --> P
    HH --> P
    II --> P
    
    %% Styling
    classDef mainFlow fill:#e1f5fe
    classDef altFlow fill:#f3e5f5
    classDef excFlow fill:#ffebee
    classDef decision fill:#fff3e0
    
    class A,B,D,F,G,H,J,M,P,S mainFlow
    class K,L,N,O,Q,R,T,U,V altFlow
    class W,X,Y,Z,AA,BB,DD,EE,GG,HH,II excFlow
    class C,E,I,CC,FF decision
```

---

## UC-005: Curriculum-Based Party Creation and Career Collaboration

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Curriculum-Based Party Creation and Career Collaboration |
| **Description** | Students form learning parties based on shared curriculum (Route) and complementary career specializations (Class) for collaborative academic and career development |
| **Actor(s)** | Primary: **Player** (Party Leader)<br>Secondary: **Players** (Party Members), Party Management System |
| **Preconditions** | User has completed curriculum-career onboarding and has active quest progression |
| **Postconditions (Success Guarantee)** | Learning party created with curriculum alignment, career diversity, shared resources, and collaborative progress tracking |
| **Related Requirements** | **FRs:** FR12 (Party Creation), FR13 (Party Stash), FR14 (Meeting Management), FR15 (Collaborative Learning)<br>**NFRs:** NFR11 (Real-time Updates), NFR12 (Social Features) |
| **Main Success Scenario** | 1. Player accesses party creation interface from curriculum dashboard<br>2. System suggests potential party members based on Route compatibility and Class diversity<br>3. User sets party parameters (curriculum focus, career goals, collaboration preferences)<br>4. System sends invitations to selected players with curriculum/career alignment details<br>5. Invited players review party goals and curriculum-career fit<br>6. System creates party with shared curriculum quests and career enhancement activities<br>7. Party establishes shared Party Stash for curriculum resources and career development materials<br>8. System enables collaborative features for both academic and career progression tracking |
| **Alternative Flows** | **A1:** Cross-curriculum parties - Players from different Routes collaborate on interdisciplinary projects<br>**A2:** Career-focused parties - Emphasis on Class-based skill development and industry preparation<br>**A3:** Study group conversion - Existing study groups upgrade to full party status with gamification |
| **Exception Flows** | **E1:** No compatible members - System suggests broader search criteria or solo progression options<br>**E2:** Party size limits - System manages waiting lists and suggests alternative parties<br>**E3:** Curriculum conflicts - System provides mediation tools for different academic schedules |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[Player accesses party creation from curriculum dashboard] --> B[System analyzes player's Route/Class profile]
    B --> C{Compatible members available?}
    
    %% Main Success Flow
    C -->|Yes| D[System suggests potential members based on Route compatibility and Class diversity]
    D --> E[Player reviews suggested members and sets party parameters]
    E --> F[Player configures curriculum focus, career goals, and collaboration preferences]
    F --> G[System sends invitations with curriculum/career alignment details]
    G --> H[Invited players review party goals and curriculum-career fit]
    H --> I{Invitation responses?}
    I -->|Accepted| J[System creates party with shared curriculum quests and career activities]
    J --> K[Party establishes shared Party Stash for resources]
    K --> L[System enables collaborative features for academic and career progression tracking]
    
    %% Alternative Flows
    E -->|Cross-curriculum parties - A1| M[Player selects members from different Routes for interdisciplinary collaboration]
    M --> N[System adjusts quest generation for interdisciplinary projects]
    N --> G
    
    F -->|Career-focused parties - A2| O[Player emphasizes Class-based skill development and industry preparation]
    O --> P[System prioritizes career enhancement activities]
    P --> G
    
    L -->|Study group conversion - A3| Q[Existing study groups request upgrade to full party status]
    Q --> R[System adds gamification features to existing collaboration]
    R --> S[Enhanced party with game mechanics]
    
    %% Exception Flows
    C -->|No - No compatible members - E1| T[System suggests broader search criteria]
    T --> U{Player choice?}
    U -->|Broaden search| V[Expand search parameters]
    U -->|Solo progression| W[System provides solo learning path options]
    V --> D
    W --> X[Player continues individual curriculum progression]
    
    I -->|Party size limits - E2| Y[System manages waiting lists]
    Y --> Z[System suggests alternative parties with availability]
    Z --> AA{Player decision?}
    AA -->|Join alternative| BB[Player joins suggested alternative party]
    AA -->|Wait for original| CC[Player added to waiting list]
    BB --> J
    CC --> DD[System notifies when space available]
    DD --> I
    
    J -->|Curriculum conflicts - E3| EE[System detects academic schedule conflicts]
    EE --> FF[System provides mediation tools for different schedules]
    FF --> GG[Party establishes flexible collaboration schedule]
    GG --> K
    
    %% Styling
    classDef mainFlow fill:#e1f5fe
    classDef altFlow fill:#f3e5f5
    classDef excFlow fill:#ffebee
    classDef decision fill:#fff3e0
    
    class A,B,D,E,F,G,H,J,K,L mainFlow
    class M,N,O,P,Q,R,S altFlow
    class T,V,W,X,Y,Z,BB,CC,DD,EE,FF,GG excFlow
    class C,I,U,AA decision
```

---

## UC-006: Meeting Scheduling and Management

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Enhanced Meeting Scheduling and Management |
| **Description** | Party Leaders schedule, conduct, and manage study meetings with AI-powered content capture and summary generation |
| **Actor(s)** | Primary: **Party Leader**<br>Secondary: **Party Members**, AI Processing System |
| **Preconditions** | User is Party Leader with active party members |
| **Postconditions (Success Guarantee)** | Meeting scheduled, conducted, and comprehensive summary generated with actionable insights |
| **Related Requirements** | **FRs:** FR14 (Meeting Management), FR15 (Meeting Recording)<br>**NFRs:** NFR17 (Real-time Communication), NFR11 (Real-time Updates) |
| **Main Success Scenario** | 1. Party Leader accesses meeting scheduling interface<br>2. User sets meeting details (title, description, type, participants)<br>3. System sends invitations and manages RSVPs<br>4. Party Leader conducts meeting with content capture options<br>5. System records meeting content via multiple methods (manual, audio)<br>6. AI processes captured content and generates comprehensive summary<br>7. System provides structured summary options (executive summary, action items, key points)<br>8. Party Leader shares results with party members |
| **Alternative Flows** | **A1:** Recurring meetings - System schedules series with templates<br>**A2:** External participant invites - System manages non-party member access<br>**A3:** Meeting recording - System provides playback and transcript features |
| **Exception Flows** | **E1:** Audio processing failure - System falls back to manual summary tools<br>**E2:** Network interruption - System saves partial content with recovery options<br>**E3:** AI summary error - System provides manual editing tools |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[Party Leader accesses meeting scheduling interface] --> B[User sets meeting details and participants]
    B --> C[System sends invitations and manages RSVPs]
    C --> D{Meeting type?}
    
    %% Main Success Flow
    D -->|Standard meeting| E[Party Leader conducts meeting with content capture options]
    E --> F[System records meeting content via multiple methods]
    F --> G{Content capture method?}
    G -->|Manual notes| H[Leader inputs key points manually]
    G -->|Audio recording| I[System captures audio for processing]
    H --> K[AI processes captured content]
    I --> K
    K --> L{AI processing successful?}
    L -->|Yes| M[AI generates comprehensive summary]
    M --> N[System provides structured summary options]
    N --> O[Party Leader reviews and shares results with party members]
    
    %% Alternative Flows
    D -->|Recurring meetings - A1| P[System schedules series with templates]
    P --> Q[Template applied to meeting series]
    Q --> E
    
    C -->|External participant invites - A2| R[System manages non-party member access]
    R --> S[External participants receive special access links]
    S --> E
    
    N -->|Meeting recording - A3| T[System provides playback and transcript features]
    T --> U[Enhanced summary with multimedia references]
    U --> O
    
    %% Exception Flows
    I -->|Audio processing failure - E1| V[System falls back to manual summary tools]
    V --> W[Leader uses manual editing interface]
    W --> X[Manual summary created]
    X --> O
    
    F -->|Network interruption - E2| Y[System saves partial content]
    Y --> Z[Recovery options provided]
    Z --> AA{Recovery successful?}
    AA -->|Yes| K
    AA -->|No| BB[Continue with partial content]
    BB --> V
    
    L -->|No - AI summary error - E3| CC[System provides manual editing tools]
    CC --> DD[Leader manually creates summary]
    DD --> O
    
    %% Styling
    classDef mainFlow fill:#e1f5fe
    classDef altFlow fill:#f3e5f5
    classDef excFlow fill:#ffebee
    classDef decision fill:#fff3e0
    
    class A,B,C,E,F,H,I,J,K,M,N,O mainFlow
    class P,Q,R,S,T,U altFlow
    class V,W,X,Y,Z,BB,CC,DD excFlow
    class D,G,L,AA decision
```

---

## UC-007: Browser Extension Integration

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Browser Extension Content Extraction |
| **Description** | Students use browser extension to extract and organize academic content from web pages into their Arsenal knowledge base |
| **Actor(s)** | Primary: **Player**<br>Secondary: Browser Extension, System |
| **Preconditions** | User has browser extension installed and is authenticated |
| **Postconditions (Success Guarantee)** | Web content extracted, organized in Arsenal, and available for contextual assistance |
| **Related Requirements** | **FRs:** FR22 (Browser Extension), FR23 (Content Organization), FR24 (Contextual Notes), FR50 (FPTU Portal Integration)<br>**NFRs:** NFR19 (Integration Capabilities), NFR20 (Content Management) |
| **Main Success Scenario** | 1. User browses academic content on university portals or educational websites<br>2. Extension automatically scans page for relevant academic information<br>3. User highlights specific text or content sections<br>4. Extension extracts highlighted content and metadata<br>5. System automatically organizes content in user's Arsenal by category<br>6. Extension displays relevant existing notes based on highlighted keywords<br>7. User saves content with tags and links to existing knowledge base |
| **Alternative Flows** | **A1:** Bulk content extraction - Extension processes entire page or document<br>**A2:** Manual categorization - User overrides automatic organization<br>**A3:** Offline extraction - Extension queues content for later synchronization |
| **Exception Flows** | **E1:** Content blocked - Extension provides manual text input option<br>**E2:** Sync failure - Extension stores locally with retry mechanism<br>**E3:** Duplicate content - System merges with existing Arsenal entries |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[User browses academic content on university portals or educational websites] --> B[Extension automatically scans page for relevant academic information]
    B --> C[User highlights specific text or content sections]
    C --> D[Extension extracts highlighted content and metadata]
    D --> E{Content extraction successful?}
    
    %% Main Success Flow
    E -->|Yes| F[System automatically organizes content in user's Arsenal by category]
    F --> G[Extension displays relevant existing notes based on highlighted keywords]
    G --> H[User saves content with tags and links to existing knowledge base]
    
    %% Alternative Flows
    C -->|Bulk content extraction - A1| I[Extension processes entire page or document]
    I --> J[System extracts comprehensive page content]
    J --> K[User reviews bulk extracted content]
    K --> F
    
    F -->|Manual categorization - A2| L[User overrides automatic organization]
    L --> M[User manually selects categories and tags]
    M --> N[System applies user-defined organization]
    N --> G
    
    D -->|Offline extraction - A3| O[Extension queues content for later synchronization]
    O --> P[Content stored locally in extension]
    P --> Q{Network available?}
    Q -->|Yes| R[Extension syncs queued content]
    Q -->|No| S[Content remains in local queue]
    R --> F
    S --> T[User notified of pending sync]
    T --> Q
    
    %% Exception Flows
    E -->|No - Content blocked - E1| U[Extension provides manual text input option]
    U --> V[User manually inputs content]
    V --> W[Extension processes manual input]
    W --> F
    
    F -->|Sync failure - E2| X[Extension stores locally with retry mechanism]
    X --> Y[Automatic retry attempts]
    Y --> Z{Retry successful?}
    Z -->|Yes| F
    Z -->|No| AA[User notified of sync issues]
    AA --> BB[Manual sync option provided]
    BB --> F
    
    F -->|Duplicate content - E3| CC[System detects existing Arsenal entries]
    CC --> DD[System merges with existing content]
    DD --> EE[User notified of merge]
    EE --> G
    
    %% Styling
    classDef mainFlow fill:#e1f5fe
    classDef altFlow fill:#f3e5f5
    classDef excFlow fill:#ffebee
    classDef decision fill:#fff3e0
    
    class A,B,C,D,F,G,H mainFlow
    class I,J,K,L,M,N,O,P,R,T altFlow
    class U,V,W,X,Y,AA,BB,CC,DD,EE excFlow
    class E,Q,Z decision
```

---

## UC-008: Guild Management System

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Guild Management and Educational Content Delivery |
| **Description** | Guild Masters manage educational guilds with member oversight, content sharing, and progress monitoring capabilities |
| **Actor(s)** | Primary: **Guild Master**<br>Secondary: **Players** (Guild Members), **Verified Lecturers** |
| **Preconditions** | User has guild creation privileges or verified lecturer status |
| **Postconditions (Success Guarantee)** | Guild created with active member management, content sharing, and educational oversight |
| **Related Requirements** | **FRs:** FR35 (Guild Creation), FR36 (Verified Lecturer Process), FR37 (Guild Materials), FR38 (Guild Dashboard)<br>**NFRs:** NFR15 (Microservices), NFR13 (Security) |
| **Main Success Scenario** | 1. User creates guild through standard process or verified lecturer streamlined flow<br>2. Guild Master configures guild settings, rules, and member permissions<br>3. System provides member invitation and management tools<br>4. Guild Master uploads reference materials and educational resources<br>5. System enables content sharing with guild members<br>6. Guild Master accesses dashboard showing aggregated, anonymized member progress<br>7. System highlights topics where significant percentage of members struggle<br>8. Guild Master creates announcements and communications for guild |
| **Alternative Flows** | **A1:** Verified Lecturer privileges - Enhanced guild creation with institutional validation<br>**A2:** Guild collaboration - Multiple Guild Masters for large courses<br>**A3:** Guild events - Specialized competitions and collaborative activities |
| **Exception Flows** | **E1:** Member limit exceeded - System provides upgrade options or waiting list<br>**E2:** Content moderation - System flags inappropriate materials for review<br>**E3:** Inactive guild - System suggests revival strategies or archival |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[User creates guild through standard process or verified lecturer streamlined flow] --> B{User type?}
    
    %% Main Success Flow
    B -->|Standard user| C[Guild Master configures guild settings, rules, and member permissions]
    B -->|Verified lecturer - A1| D[Enhanced guild creation with institutional validation]
    D --> E[Streamlined setup with lecturer privileges]
    E --> C
    
    C --> F[System provides member invitation and management tools]
    F --> G[Guild Master uploads reference materials and educational resources]
    G --> H{Content moderation check?}
    H -->|Approved| I[System enables content sharing with guild members]
    H -->|Flagged - E2| J[System flags inappropriate materials for review]
    J --> K[Content review process]
    K --> L{Review result?}
    L -->|Approved| I
    L -->|Rejected| M[Guild Master notified to revise content]
    M --> G
    
    I --> N[Guild Master accesses dashboard showing aggregated, anonymized member progress]
    N --> O[System highlights topics where significant percentage of members struggle]
    O --> P[Guild Master creates announcements and communications for guild]
    
    %% Alternative Flows
    C -->|Guild collaboration - A2| Q[Multiple Guild Masters for large courses]
    Q --> R[System manages co-leadership permissions]
    R --> F
    
    P -->|Guild events - A3| S[Guild Master creates specialized competitions and collaborative activities]
    S --> T[System facilitates guild events]
    T --> U[Enhanced member engagement]
    U --> N
    
    %% Exception Flows
    F -->|Member limit exceeded - E1| V[System provides upgrade options or waiting list]
    V --> W{Guild Master choice?}
    W -->|Upgrade| X[Guild capacity increased]
    W -->|Waiting list| Y[New members added to queue]
    X --> I
    Y --> Z[System notifies when space available]
    Z --> F
    
    N -->|Inactive guild - E3| AA[System detects low activity]
    AA --> BB[System suggests revival strategies or archival]
    BB --> CC{Guild Master decision?}
    CC -->|Revival| DD[Implement revival strategies]
    CC -->|Archive| EE[Guild archived with data preservation]
    DD --> P
    EE --> FF[Members notified of archival]
    
    %% Styling
    classDef mainFlow fill:#e1f5fe
    classDef altFlow fill:#f3e5f5
    classDef excFlow fill:#ffebee
    classDef decision fill:#fff3e0
    
    class A,C,F,G,I,N,O,P mainFlow
    class D,E,Q,R,S,T,U altFlow
    class J,K,M,V,X,Y,Z,AA,BB,DD,EE,FF excFlow
    class B,H,L,W,CC decision
```

---

## UC-009: Curriculum-Career Boss Fight Assessment System

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Curriculum-Career Boss Fight Assessment System |
| **Description** | Students engage in gamified assessments through Unity WebGL boss fight experiences that evaluate both curriculum mastery and career readiness with adaptive difficulty and integrated progression tracking |
| **Actor(s)** | Primary: **Player**<br>Secondary: Unity WebGL System, Assessment Engine, Career Evaluation System |
| **Preconditions** | User has completed prerequisite curriculum quests and career enhancement activities |
| **Postconditions (Success Guarantee)** | Assessment completed with performance evaluation for both academic curriculum and career specialization progression |
| **Related Requirements** | **FRs:** FR16 (Boss Fight System), FR17 (Unity WebGL Integration), FR18 (Career Assessment)<br>**NFRs:** NFR10 (Unity WebGL), NFR9 (Performance) |
| **Main Success Scenario** | 1. Player encounters boss fight trigger based on curriculum milestone or career checkpoint<br>2. System launches Unity WebGL boss fight interface with Route/Class-specific challenges<br>3. Player engages with curriculum-based questions and career scenario challenges<br>4. System provides real-time visual feedback for both academic and career performance<br>5. Boss fight adapts difficulty based on Route requirements and Class specialization<br>6. Player completes assessment demonstrating curriculum mastery and career readiness<br>7. System updates character stats for both academic progression and career specialization<br>8. Player receives rewards aligned with Route advancement and Class development |
| **Alternative Flows** | **A1:** Curriculum-focused boss fights - Emphasis on academic content mastery<br>**A2:** Career-specialized challenges - Industry-specific scenarios based on Class selection<br>**A3:** Collaborative boss fights - Party members tackle curriculum and career challenges together |
| **Exception Flows** | **E1:** Unity WebGL loading failure - System provides alternative assessment format maintaining Route/Class evaluation<br>**E2:** Performance issues - System adjusts graphics while preserving curriculum-career assessment integrity<br>**E3:** Network interruption - System saves progress separately for academic and career tracks |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[Student encounters Route/Class milestone] --> B{Unity WebGL Available?}
    B -->|Yes| C[System launches Unity WebGL interface]
    B -->|No| D[Alternative assessment format]
    
    C --> E[Load curriculum/career challenges]
    D --> E
    
    E --> F{Challenge Type?}
    F -->|Curriculum Focus| G[Academic content mastery challenges]
    F -->|Career Specialized| H[Industry-specific scenarios]
    F -->|Collaborative| I[Party-based challenges]
    
    G --> J[Real-time feedback for academic track]
    H --> K[Real-time feedback for career track]
    I --> L[Real-time feedback for both tracks]
    
    J --> M{Performance Issues?}
    K --> M
    L --> M
    
    M -->|Yes| N[Adjust graphics/maintain assessment integrity]
    M -->|No| O[Boss adapts to Route/Class requirements]
    N --> O
    
    O --> P{Network Interruption?}
    P -->|Yes| Q[Save progress for academic and career tracks]
    P -->|No| R[Complete curriculum-career assessment]
    Q --> S[Resume when connection restored]
    S --> R
    
    R --> T[Update character stats for both tracks]
    T --> U[Receive Route/Class-aligned rewards]
    U --> V[Assessment Complete]

    %% Styling
    classDef mainFlow fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef altFlow fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef exceptionFlow fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    classDef decisionFlow fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class A,C,E,J,K,L,O,R,T,U,V mainFlow
    class G,H,I,N altFlow
    class D,Q,S exceptionFlow
    class B,F,M,P decisionFlow
```

---

## UC-010: Collaborative Study Sessions

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Real-time Collaborative Study Sessions |
| **Description** | Party members engage in synchronized study sessions with shared resources, real-time communication, and collaborative learning activities |
| **Actor(s)** | Primary: **Party Members**<br>Secondary: **Party Leader**, System |
| **Preconditions** | User is party member with access to collaborative features |
| **Postconditions (Success Guarantee)** | Study session completed with shared learning outcomes and progress tracking |
| **Related Requirements** | **FRs:** FR13 (Party Stash), FR14 (Meeting Management), FR18 (Collaborative Features)<br>**NFRs:** NFR17 (Real-time Communication), NFR4 (RESTful Communication) |
| **Main Success Scenario** | 1. Party Leader initiates collaborative study session<br>2. System notifies party members and provides session access<br>3. Members join shared virtual study space with real-time synchronization<br>4. System enables collaborative tools (shared whiteboard, document editing, screen sharing)<br>5. Party members work together on learning objectives and assignments<br>6. System tracks individual and group progress during session<br>7. Members share resources from Party Stash and personal Arsenal<br>8. Session concludes with summary and individual progress updates |
| **Alternative Flows** | **A1:** Asynchronous collaboration - Members contribute at different times<br>**A2:** Study group rotation - Different members lead sessions<br>**A3:** Cross-party collaboration - Multiple parties work together on projects |
| **Exception Flows** | **E1:** Connection issues - System provides offline mode with later synchronization<br>**E2:** Resource conflicts - System manages concurrent access with version control<br>**E3:** Member unavailability - System records session for later review |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[Party Leader initiates collaborative study session] --> B[System notifies party members]
    B --> C{Members Available?}
    C -->|Yes| D[Members join shared virtual study space]
    C -->|No| E[Record session for later review]
    
    D --> F[Real-time synchronization established]
    F --> G{Session Type?}
    G -->|Synchronous| H[Enable collaborative tools]
    G -->|Asynchronous| I[Set up contribution timeline]
    G -->|Cross-party| J[Coordinate multiple parties]
    
    H --> K[Shared whiteboard, document editing, screen sharing]
    I --> L[Members contribute at different times]
    J --> M[Multi-party collaboration setup]
    
    K --> N[Work together on learning objectives]
    L --> N
    M --> N
    
    N --> O{Connection Issues?}
    O -->|Yes| P[Activate offline mode with sync]
    O -->|No| Q[Track individual and group progress]
    P --> R[Later synchronization when online]
    R --> Q
    
    Q --> S{Resource Conflicts?}
    S -->|Yes| T[Manage concurrent access with version control]
    S -->|No| U[Share resources from Party Stash and Arsenal]
    T --> U
    
    U --> V{Study Group Rotation?}
    V -->|Yes| W[Different member leads next session]
    V -->|No| X[Current leader continues]
    W --> Y[Session concludes with summary]
    X --> Y
    
    Y --> Z[Individual progress updates distributed]
    E --> AA[Session available for review]
    Z --> BB[Study Session Complete]
    AA --> BB

    %% Styling
    classDef mainFlow fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef altFlow fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef exceptionFlow fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    classDef decisionFlow fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class A,B,D,F,H,K,N,Q,U,Y,Z,BB mainFlow
    class I,J,L,M,T,W,X altFlow
    class E,P,R,AA exceptionFlow
    class C,G,O,S,V decisionFlow
```

---

## UC-011: Event Management with Game Master Approval

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Event Management with Game Master Authority |
| **Description** | Game Masters have the highest administrative authority for creating, managing, and executing platform-wide events, with Guild Masters able to create event requests that require Game Master approval for quality assurance and educational oversight |
| **Actor(s)** | Primary: **Game Master**, **Guild Master**<br>Secondary: **Students**, **Verified Lecturers**, Room Assignment Service, Quality Assurance System |
| **Preconditions** | User has appropriate role-based event creation privileges and system access |
| **Postconditions (Success Guarantee)** | Event created through proper approval workflow, quality assured, guilds registered, room assignments completed, and activities conducted with full audit trail |
| **Related Requirements** | **FRs:** FR43 (Event Management), FR44 (Code Battle System), FR47 (Enhanced Approval Workflow), FR47A (Game Master Event Creation)<br>**NFRs:** NFR15 (Microservices), NFR21 (Scalability), NFR13 (Security) |
| **Main Success Scenario** | 1. **Event Creation Phase**: Game Master accesses event creation wizard with enhanced toolkit<br>2. **Event Configuration**: User selects event type and configures parameters (standard events auto-approved, custom events flagged for review)<br>3. **Quality Assurance**: System performs automated quality checks and content validation<br>4. **Approval Workflow**: Event follows streamlined approval (Guild Master requests require Game Master approval for quality assurance)<br>5. **Event Setup**: Approved event configured with room assignments, participation criteria, and guild requirements<br>6. **Registration Phase**: System opens guild registration with prerequisite checking and capacity management<br>7. **Event Execution**: Event proceeds with real-time monitoring, guild ranking, and performance tracking<br>8. **Post-Event**: System generates comprehensive analytics, audit trail, and distributes rewards<br>9. **Review Dashboard**: All stakeholders access review dashboard for event performance and feedback |
| **Alternative Flows** | **A1:** Game Master Direct Creation - Game Masters can create and activate events directly with their administrative authority<br>**A2:** Guild Master Event Request - Guild Masters submit event requests that require Game Master approval for quality assurance<br>**A3:** Custom Event Review - Manual peer review process for innovative or complex event designs<br>**A4:** Cross-guild Collaborative Events - Multi-guild events requiring enhanced coordination and approval<br>**A5:** Recurring Event Series - Streamlined approval for recurring educational event patterns |
| **Exception Flows** | **E1:** Approval rejection - System provides detailed feedback and revision guidance for resubmission<br>**E2:** Quality assurance failure - System flags content issues and provides improvement recommendations<br>**E3:** Capacity overflow - System manages waiting lists and provides alternative event scheduling<br>**E4:** Technical issues during approval - System maintains approval state and provides recovery mechanisms |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[Game Master accesses event creation wizard] --> B[Configure event type and parameters]
    B --> C[System performs automated quality checks]
    C --> D{Event Creator Role?}
    D -->|Game Master| E[Direct creation with administrative authority]
    D -->|Guild Master| F[Submit event request for approval]
    
    E --> G[Event setup with room assignments]
    F --> H{Game Master Review}
    H -->|Approved| I[Quality assurance passed]
    H -->|Rejected| J[Provide detailed feedback and revision guidance]
    
    I --> G
    J --> K[Guild Master receives feedback]
    K --> L{Resubmit?}
    L -->|Yes| B
    L -->|No| M[Event Request Cancelled]
    
    G --> N{Event Type?}
    N -->|Standard| O[Auto-approved event activation]
    N -->|Custom| P[Manual peer review process]
    N -->|Cross-guild| Q[Enhanced coordination setup]
    N -->|Recurring| R[Streamlined recurring pattern approval]
    
    O --> S[Guild registration opens with prerequisite checking]
    P --> T{Review Result?}
    T -->|Approved| S
    T -->|Needs Revision| J
    Q --> U[Multi-guild coordination established]
    R --> V[Recurring series configured]
    U --> S
    V --> S
    
    S --> W{Capacity Issues?}
    W -->|Overflow| X[Manage waiting lists and alternative scheduling]
    W -->|Normal| Y[Event execution with real-time monitoring]
    X --> Y
    
    Y --> Z{Technical Issues?}
    Z -->|Yes| AA[Maintain approval state and provide recovery]
    Z -->|No| BB[Guild ranking and performance tracking]
    AA --> BB
    
    BB --> CC[Post-event analytics and comprehensive audit trail]
    CC --> DD[Distribute rewards based on performance]
    DD --> EE[Review dashboard access for all stakeholders]
    EE --> FF[Event Management Complete]
    M --> FF

    %% Styling
    classDef mainFlow fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef altFlow fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef exceptionFlow fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    classDef decisionFlow fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class A,B,C,E,G,I,O,S,Y,BB,CC,DD,EE,FF mainFlow
    class P,Q,R,U,V,X altFlow
    class J,K,M,AA exceptionFlow
    class D,H,L,N,T,W,Z decisionFlow
```

---

## UC-012: Code Battle Participation

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Real-time Code Battle Participation |
| **Description** | Students participate in competitive coding challenges within guild events featuring live evaluation, spectator mode, and skill assessment |
| **Actor(s)** | Primary: **Student**<br>Secondary: **Spectators**, Code Battle System |
| **Preconditions** | User is registered for code battle event and has access to coding environment |
| **Postconditions (Success Guarantee)** | Code battle completed with performance evaluation, skill assessment, and leaderboard updates |
| **Related Requirements** | **FRs:** FR43 (Code Battle System), FR44 (Real-time Execution Environment)<br>**NFRs:** NFR8 (Golang Backend), NFR17 (Real-time Communication) |
| **Main Success Scenario** | 1. Student joins code battle at scheduled time within assigned room<br>2. System provides secure coding environment with multiple language support<br>3. Student receives problem statement, test cases, and time constraints<br>4. Student writes and tests solution in real-time development environment<br>5. System provides live compilation feedback and automated test results<br>6. Spectators can view progress through spectator mode with live updates<br>7. System evaluates solution for correctness, efficiency, and code quality<br>8. Student submits final solution before time limit<br>9. System updates guild rankings and individual performance metrics |
| **Alternative Flows** | **A1:** Team-based battles - Multiple students collaborate on solutions<br>**A2:** Progressive challenges - Battles with increasing difficulty levels<br>**A3:** Educational mode - Battles focused on learning rather than competition |
| **Exception Flows** | **E1:** Technical issues - System provides backup submission methods<br>**E2:** Network problems - System offers offline coding with delayed submission<br>**E3:** Time extension - System handles exceptional circumstances with fair adjustments |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[Student joins code battle at scheduled time] --> B[System provides secure coding environment]
    B --> C{Multiple Language Support?}
    C -->|Yes| D[Select preferred programming language]
    C -->|Default| E[Use default environment setup]
    
    D --> F[Receive problem statement and constraints]
    E --> F
    
    F --> G{Battle Type?}
    G -->|Individual| H[Write and test solution independently]
    G -->|Team-based| I[Collaborate with team members on solution]
    G -->|Progressive| J[Start with basic difficulty level]
    G -->|Educational| K[Focus on learning rather than competition]
    
    H --> L[System provides live compilation feedback]
    I --> M[Team coordination and shared development]
    J --> N[Advance through increasing difficulty levels]
    K --> O[Guided learning with hints and explanations]
    
    M --> L
    N --> L
    O --> L
    
    L --> P[Automated test results displayed]
    P --> Q{Spectator Mode Active?}
    Q -->|Yes| R[Spectators view progress with live updates]
    Q -->|No| S[Continue development without spectators]
    
    R --> T[System evaluates solution quality]
    S --> T
    
    T --> U{Technical Issues?}
    U -->|Yes| V[Provide backup submission methods]
    U -->|Network Problems| W[Offer offline coding with delayed submission]
    U -->|No| X[Submit final solution before time limit]
    
    V --> Y{Issue Resolved?}
    W --> Z{Connection Restored?}
    Y -->|Yes| X
    Y -->|No| AA[Request time extension for exceptional circumstances]
    Z -->|Yes| BB[Upload offline solution]
    Z -->|No| CC[Submit via alternative method]
    
    BB --> X
    CC --> X
    AA --> DD{Extension Approved?}
    DD -->|Yes| X
    DD -->|No| EE[Submit current progress]
    
    X --> FF[System updates guild rankings and individual metrics]
    EE --> FF
    FF --> GG[Code Battle Complete]

    %% Styling
    classDef mainFlow fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef altFlow fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef exceptionFlow fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    classDef decisionFlow fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class A,B,D,E,F,H,L,P,S,T,X,FF,GG mainFlow
    class I,J,K,M,N,O,R altFlow
    class V,W,AA,BB,CC,EE exceptionFlow
    class C,G,Q,U,Y,Z,DD decisionFlow
```

---

## UC-013: Real-time Notifications and Updates

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Comprehensive Notification System |
| **Description** | System delivers real-time notifications across multiple channels to keep users engaged and informed of relevant platform activities |
| **Actor(s)** | Primary: Notification System<br>Secondary: **Students**, **Guild Masters** |
| **Preconditions** | User has active account with configured notification preferences |
| **Postconditions (Success Guarantee)** | Users receive timely, relevant notifications through preferred channels |
| **Related Requirements** | **FRs:** FR39 (In-app Notifications), FR21 (Notification System)<br>**NFRs:** NFR11 (Real-time Updates), NFR17 (Real-time Communication) |
| **Main Success Scenario** | 1. System detects trigger event (quest update, party invitation, event notification, guild announcement)<br>2. System checks user notification preferences and delivery channels<br>3. System generates appropriate notification content based on event type<br>4. System delivers notification via configured channels (in-app, email, push notifications)<br>5. User receives notification and can interact with actionable content<br>6. System tracks notification delivery status and user engagement<br>7. System adjusts future notification timing and content based on user behavior |
| **Alternative Flows** | **A1:** Batch notifications - System groups related notifications for better user experience<br>**A2:** Priority notifications - Critical events bypass normal delivery schedules<br>**A3:** Notification history - Users can review past notifications and missed updates |
| **Exception Flows** | **E1:** Delivery failure - System retries with alternative channels<br>**E2:** Service unavailable - System queues notifications for later delivery<br>**E3:** User preferences conflict - System uses default settings with user notification |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[System detects trigger event] --> B{Event Type?}
    B -->|Quest Update| C[Quest-related notification content]
    B -->|Party Invitation| D[Party invitation notification]
    B -->|Event Notification| E[Event announcement content]
    B -->|Guild Announcement| F[Guild-specific notification]
    
    C --> G[Check user notification preferences]
    D --> G
    E --> G
    F --> G
    
    G --> H{Notification Priority?}
    H -->|Critical| I[Bypass normal delivery schedules]
    H -->|Normal| J[Follow standard delivery timing]
    H -->|Batch| K[Group related notifications]
    
    I --> L[Generate priority notification content]
    J --> M[Generate standard notification content]
    K --> N[Generate batch notification content]
    
    L --> O{Delivery Channels Available?}
    M --> O
    N --> O
    
    O -->|Yes| P[Deliver via configured channels]
    O -->|No| Q[Retry with alternative channels]
    
    P --> R[In-app, email, push notifications sent]
    Q --> S{Alternative Channels Available?}
    S -->|Yes| T[Use backup delivery methods]
    S -->|No| U[Queue notifications for later delivery]
    
    T --> R
    U --> V[Service unavailable - queue maintained]
    
    R --> W{User Interaction?}
    W -->|Yes| X[User interacts with actionable content]
    W -->|No| Y[Track delivery status only]
    
    X --> Z[Track notification delivery and engagement]
    Y --> Z
    
    Z --> AA{User Preferences Conflict?}
    AA -->|Yes| BB[Use default settings with user notification]
    AA -->|No| CC[Adjust future notifications based on behavior]
    
    BB --> DD[Notification history updated]
    CC --> DD
    
    DD --> EE{User Requests History?}
    EE -->|Yes| FF[Provide notification history and missed updates]
    EE -->|No| GG[Notification Process Complete]
    
    V --> GG
    FF --> GG

    %% Styling
    classDef mainFlow fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef altFlow fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef exceptionFlow fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    classDef decisionFlow fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class A,G,J,M,P,R,Y,Z,CC,DD,GG mainFlow
    class I,K,L,N,X,FF altFlow
    class Q,T,U,V,BB exceptionFlow
    class B,H,O,S,W,AA,EE decisionFlow
```

---

## UC-014: Performance Analytics and Monitoring

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Comprehensive Analytics and Performance Monitoring |
| **Description** | Game Masters (Admins) and Guild Masters access detailed analytics to monitor platform health, user engagement, and educational outcomes |
| **Actor(s)** | Primary: **Game Master (Admin)**, **Guild Master**<br>Secondary: **Verified Lecturers**, Analytics System |
| **Preconditions** | User has appropriate administrative privileges and analytics data is available |
| **Postconditions (Success Guarantee)** | Analytics dashboard provides actionable insights with comprehensive reporting and monitoring capabilities |
| **Related Requirements** | **FRs:** FR40 (Performance Monitoring), FR41 (Data Architecture), FR42 (Content Delivery)<br>**NFRs:** NFR23 (Analytics & Monitoring), NFR9 (Performance) |
| **Main Success Scenario** | 1. Administrator accesses comprehensive analytics dashboard<br>2. System displays real-time performance metrics, user engagement data, and educational outcomes<br>3. User selects specific time periods, user segments, and performance indicators<br>4. System generates detailed reports on platform health and learning effectiveness<br>5. Administrator identifies trends, performance bottlenecks, and improvement opportunities<br>6. System provides automated recommendations and alerts for critical thresholds<br>7. Administrator exports reports for stakeholder communication and decision-making<br>8. System schedules automated monitoring and alerting for continuous oversight |
| **Alternative Flows** | **A1:** Guild-specific analytics - Guild Masters view community-focused metrics<br>**A2:** Custom report creation - Users build specialized reports for specific needs<br>**A3:** Predictive analytics - System provides forecasting and trend analysis |
| **Exception Flows** | **E1:** Data processing delays - System provides estimated completion times<br>**E2:** Report generation failure - System offers alternative data export formats<br>**E3:** Analytics service unavailable - System provides cached data with timestamps |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[Administrator accesses comprehensive analytics dashboard] --> B[System displays real-time performance metrics]
    B --> C{User Role?}
    C -->|Game Master| D[Access full platform analytics]
    C -->|Guild Master| E[Access guild-specific analytics]
    C -->|Verified Lecturer| F[Access educational outcome metrics]
    
    D --> G[Select time periods, user segments, indicators]
    E --> H[Select guild community metrics]
    F --> I[Select learning effectiveness data]
    
    G --> J[Generate detailed platform health reports]
    H --> K[Generate guild-focused reports]
    I --> L[Generate educational outcome reports]
    
    J --> M{Report Type?}
    K --> M
    L --> M
    
    M -->|Standard| N[System generates standard reports]
    M -->|Custom| O[User builds specialized reports]
    M -->|Predictive| P[System provides forecasting and trend analysis]
    
    N --> Q[Identify trends and performance bottlenecks]
    O --> R[Custom analysis with specific parameters]
    P --> S[Predictive insights and recommendations]
    
    R --> Q
    S --> Q
    
    Q --> T{Data Processing Status?}
    T -->|Processing| U[System provides estimated completion times]
    T -->|Complete| V[System provides automated recommendations]
    
    U --> W[Wait for processing completion]
    W --> V
    
    V --> X[Administrator reviews improvement opportunities]
    X --> Y{Export Required?}
    Y -->|Yes| Z[Export reports for stakeholder communication]
    Y -->|No| AA[Review reports in dashboard]
    
    Z --> BB{Export Success?}
    BB -->|Yes| CC[Reports exported successfully]
    BB -->|No| DD[System offers alternative data export formats]
    
    DD --> EE{Alternative Format Available?}
    EE -->|Yes| FF[Export in alternative format]
    EE -->|No| GG[Analytics service unavailable - provide cached data]
    
    FF --> CC
    CC --> HH[Schedule automated monitoring and alerting]
    AA --> HH
    GG --> II[Cached data provided with timestamps]
    
    HH --> JJ[Continuous oversight established]
    II --> JJ
    JJ --> KK[Analytics Process Complete]

    %% Styling
    classDef mainFlow fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef altFlow fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef exceptionFlow fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    classDef decisionFlow fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class A,B,D,G,J,N,Q,V,X,AA,CC,HH,JJ,KK mainFlow
    class E,F,H,I,K,L,O,P,R,S,Z altFlow
    class U,W,DD,FF,GG,II exceptionFlow
    class C,M,T,Y,BB,EE decisionFlow
```

---

## UC-015: System Integration and API Management

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | External System Integration and API Management |
| **Description** | Game Masters (Admins) manage external integrations, API access controls, and system configurations to ensure platform functionality and security |
| **Actor(s)** | Primary: **Game Master (Admin)**<br>Secondary: External Systems, API Clients |
| **Preconditions** | Game Master has system-level access and integration requirements are defined |
| **Postconditions (Success Guarantee)** | Integrations configured and functional with proper security controls and monitoring |
| **Related Requirements** | **FRs:** FR42 (Content Delivery), FR41 (Data Architecture)<br>**NFRs:** NFR13 (Security), NFR21 (Integration Capabilities), NFR19 (Integration Capabilities) |
| **Main Success Scenario** | 1. Game Master accesses system integration management dashboard<br>2. Game Master configures external service connections (LMS, calendar systems, university portals)<br>3. System validates integration credentials, permissions, and security requirements<br>4. Game Master sets up API access controls, rate limiting, and usage monitoring<br>5. System tests integration functionality and reports connection status<br>6. Game Master monitors integration health, performance metrics, and error rates<br>7. System logs all integration activities for security audit and compliance<br>8. Game Master receives automated alerts for integration issues and performance degradation |
| **Alternative Flows** | **A1:** Bulk integration setup - Game Master configures multiple services simultaneously<br>**A2:** Custom API development - Game Master creates specialized integration endpoints<br>**A3:** Integration migration - Game Master updates existing integrations to new versions |
| **Exception Flows** | **E1:** Authentication failure - System provides troubleshooting guidance and fallback options<br>**E2:** Rate limits exceeded - System implements intelligent queuing and retry logic<br>**E3:** External service unavailable - System activates fallback procedures and user notifications |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[Game Master accesses system integration management dashboard] --> B[Configure external service connections]
    B --> C{Integration Type?}
    C -->|Single Service| D[Configure individual LMS/calendar/portal connection]
    C -->|Bulk Setup| E[Configure multiple services simultaneously]
    C -->|Custom API| F[Create specialized integration endpoints]
    C -->|Migration| G[Update existing integrations to new versions]
    
    D --> H[System validates integration credentials]
    E --> I[Bulk validation of multiple credentials]
    F --> J[Custom API development and validation]
    G --> K[Migration validation and compatibility check]
    
    I --> H
    J --> H
    K --> H
    
    H --> L{Validation Result?}
    L -->|Success| M[Validate permissions and security requirements]
    L -->|Authentication Failure| N[System provides troubleshooting guidance]
    
    N --> O{Fallback Options Available?}
    O -->|Yes| P[Activate fallback procedures]
    O -->|No| Q[Manual intervention required]
    
    P --> M
    Q --> R[Administrator resolves authentication issues]
    R --> H
    
    M --> S[Set up API access controls and rate limiting]
    S --> T[Configure usage monitoring and logging]
    T --> U[System tests integration functionality]
    U --> V{Integration Test Result?}
    V -->|Success| W[Report connection status as operational]
    V -->|Failure| X[Identify and report integration issues]
    
    X --> Y{Issue Resolvable?}
    Y -->|Yes| Z[Apply fixes and retest]
    Y -->|No| AA[Escalate to technical support]
    
    Z --> U
    AA --> BB[Technical support intervention]
    BB --> U
    
    W --> CC[Monitor integration health and performance metrics]
    CC --> DD{Performance Issues?}
    DD -->|Rate Limits Exceeded| EE[Implement intelligent queuing and retry logic]
    DD -->|External Service Unavailable| FF[Activate fallback procedures and user notifications]
    DD -->|Normal| GG[Continue monitoring error rates]
    
    EE --> GG
    FF --> GG
    
    GG --> HH[System logs all integration activities]
    HH --> II[Security audit and compliance logging]
    II --> JJ{Critical Issues Detected?}
    JJ -->|Yes| KK[Send automated alerts for integration issues]
    JJ -->|No| LL[Routine monitoring continues]
    
    KK --> MM[Game Master receives performance degradation alerts]
    LL --> NN[Integration Management Complete]
    MM --> NN

    %% Styling
    classDef mainFlow fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef altFlow fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef exceptionFlow fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    classDef decisionFlow fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class A,B,D,H,M,S,T,U,W,CC,GG,HH,II,LL,NN mainFlow
    class E,F,G,I,J,K,P altFlow
    class N,Q,R,X,AA,BB,EE,FF,KK,MM exceptionFlow
    class C,L,O,V,Y,DD,JJ decisionFlow
```

---

## UC-016: Elective Library Curation & Approval (Admin-Owned)

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | Elective Library Curation & Approval (Admin-Owned) |
| **Description** | Game Masters curate and approve elective courses within the University's educational ecosystem, ensuring quality standards and alignment with educational objectives |
| **Actor(s)** | Primary: **Game Master (Admin)**<br>Secondary: Faculty, Content Providers |
| **Preconditions** | Game Master has administrative privileges; elective content is submitted and pending review |
| **Postconditions (Success Guarantee)** | Elective library updated with approved courses, categorized and available for student selection |
| **Related Requirements** | **FRs:** FR52 (Elective Library Curation)<br>**NFRs:** NFR13 (Security), NFR15 (Microservices) |
| **Main Success Scenario** | 1. Game Master accesses elective library dashboard<br>2. System lists pending elective submissions<br>3. Game Master reviews content, metadata, and alignment with standards<br>4. System provides approve/reject workflow with feedback<br>5. Game Master categorizes approved electives (subject, level, prerequisites)<br>6. System publishes approved electives to catalog<br>7. Game Master monitors performance and engagement metrics<br>8. System generates curation reports |
| **Alternative Flows** | **A1:** Bulk approvals for multiple electives<br>**A2:** Conditional approval with required modifications<br>**A3:** Faculty collaboration in review |
| **Exception Flows** | **E1:** Content quality issues - rejected with recommendations<br>**E2:** Duplicate electives flagged for consolidation<br>**E3:** Workflow failure - retain pending status and alert admin |

### **Comprehensive Flow Flowchart**

```mermaid
flowchart TD
    A[Game Master accesses elective library dashboard] --> B[System lists pending elective submissions]
    B --> C{Submission Type?}
    C -->|Single Elective| D[Review individual elective content]
    C -->|Bulk Submissions| E[Review multiple electives for bulk approval]
    C -->|Faculty Collaboration| F[Collaborate with faculty in review process]
    
    D --> G[Review content, metadata, and alignment with standards]
    E --> H[Bulk review process with standardized criteria]
    F --> I[Faculty input and collaborative evaluation]
    
    H --> G
    I --> G
    
    G --> J{Content Quality Assessment?}
    J -->|Meets Standards| K[System provides approve workflow]
    J -->|Needs Improvement| L[System provides reject workflow with feedback]
    J -->|Conditional| M[Conditional approval with required modifications]
    
    K --> N[Game Master categorizes approved electives]
    L --> O[Provide detailed feedback and recommendations]
    M --> P[Specify required modifications for approval]
    
    N --> Q{Categorization Type?}
    Q -->|Subject-based| R[Categorize by academic subject]
    Q -->|Level-based| S[Categorize by difficulty level]
    Q -->|Prerequisite-based| T[Categorize by prerequisite requirements]
    
    R --> U[System publishes approved electives to catalog]
    S --> U
    T --> U
    
    O --> V[Notify content provider of rejection]
    P --> W[Notify provider of conditional approval requirements]
    
    V --> X{Provider Response?}
    W --> Y{Modifications Completed?}
    X -->|Resubmit| B
    X -->|No Response| Z[Elective remains rejected]
    Y -->|Yes| AA[Re-review modified content]
    Y -->|No| BB[Conditional approval expires]
    
    AA --> G
    BB --> Z
    
    U --> CC{Duplicate Detection?}
    CC -->|Duplicates Found| DD[Flag duplicate electives for consolidation]
    CC -->|No Duplicates| EE[Monitor performance and engagement metrics]
    
    DD --> FF[Consolidation process initiated]
    FF --> EE
    
    EE --> GG{Workflow Issues?}
    GG -->|Technical Failure| HH[Retain pending status and alert admin]
    GG -->|Normal Operation| II[System generates curation reports]
    
    HH --> JJ[Technical support resolves workflow issues]
    JJ --> II
    
    II --> KK[Elective Library Curation Complete]
    Z --> KK

    %% Styling
    classDef mainFlow fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef altFlow fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef exceptionFlow fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    classDef decisionFlow fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class A,B,D,G,K,N,U,EE,II,KK mainFlow
    class E,F,H,I,M,P,R,S,T,W altFlow
    class L,O,V,Z,DD,FF,HH,JJ exceptionFlow
    class C,J,Q,X,Y,CC,GG decisionFlow
```

---

## UC-017: University Curriculum Import & Administration (Admin-Owned)

| **Field** | **Details** |
|-----------|-------------|
| **Use Case Name** | University Curriculum Import & Administration (Admin-Owned) |
| **Description** | Game Masters import, configure, and administer university curricula, mapping programs to platform structures and ensuring academic integrity |
| **Actor(s)** | Primary: **Game Master (Admin)**<br>Secondary: University Registrar, Academic Departments |
| **Preconditions** | Administrative access granted; curriculum data available |
| **Postconditions (Success Guarantee)** | Curriculum imported, mapped to Route/Class, and available for student selection with accurate requirements |
| **Related Requirements** | **FRs:** FR53 (University Curriculum Import)<br>**NFRs:** NFR13 (Security), NFR8 (Backend Performance) |
| **Main Success Scenario** | 1. Game Master initiates import from university sources<br>2. System validates structure, prerequisites, relationships<br>3. Game Master maps curriculum to Route/Class<br>4. System generates skill trees and pathways<br>5. Game Master configures assessments and progression rules<br>6. System integrates with quests and assessments<br>7. Game Master tests student progression flows<br>8. System publishes curriculum to route selection |
| **Alternative Flows** | **A1:** Incremental curriculum updates<br>**A2:** Multi-university management<br>**A3:** Custom program creation |
| **Exception Flows** | **E1:** Data validation failure<br>**E2:** Import interruption with recovery<br>**E3:** Mapping conflicts requiring resolution |

### **Main Success Scenario Flowchart**

```mermaid
flowchart TD
    A[Game Master initiates curriculum import from university sources] --> B[System validates curriculum structure and data integrity]
    B --> C{Validation Results?}
    C -->|Valid Data| D[System processes curriculum structure]
    C -->|Data Issues| E[System flags validation errors and provides correction guidance]
    C -->|Partial Validation| F[System processes valid portions and flags issues]
    
    D --> G[Game Master maps curriculum to Route/Class structure]
    E --> H[Game Master corrects data issues]
    F --> I[Game Master reviews partial import and addresses flagged issues]
    
    H --> B
    I --> G
    
    G --> J{Mapping Complexity?}
    J -->|Standard Mapping| K[System generates skill trees and learning pathways]
    J -->|Complex Mapping| L[Game Master creates custom mapping rules]
    J -->|Multi-Program| M[Game Master manages multiple program mappings]
    
    L --> N[System applies custom mapping rules]
    M --> O[System processes multi-program structure]
    
    N --> K
    O --> K
    
    K --> P[Game Master configures assessments and progression rules]
    P --> Q{Assessment Configuration?}
    Q -->|Standard Rules| R[Apply default progression rules]
    Q -->|Custom Rules| S[Configure custom assessment criteria]
    Q -->|Adaptive Rules| T[Set up adaptive progression system]
    
    R --> U[System integrates curriculum with quests and assessments]
    S --> U
    T --> U
    
    U --> V[Game Master tests student progression flows]
    V --> W{Testing Results?}
    W -->|All Tests Pass| X[System publishes curriculum to route selection]
    W -->|Issues Found| Y[Game Master fixes identified issues]
    W -->|Partial Success| Z[Game Master addresses specific flow problems]
    
    Y --> AA[Re-run comprehensive testing]
    Z --> BB[Targeted testing of fixed components]
    
    AA --> W
    BB --> W
    
    X --> CC{Integration Requirements?}
    CC -->|LMS Integration| DD[Sync with Learning Management System]
    CC -->|Registrar Integration| EE[Connect with university registrar systems]
    CC -->|Standalone| FF[Complete internal integration]
    
    DD --> GG[Monitor integration performance and data sync]
    EE --> GG
    FF --> GG
    
    GG --> HH{Ongoing Management?}
    HH -->|Incremental Updates| II[Process curriculum updates and changes]
    HH -->|Multi-University| JJ[Manage multiple university curricula]
    HH -->|Maintenance Mode| KK[Regular maintenance and optimization]
    
    II --> LL[Update existing mappings and pathways]
    JJ --> MM[Coordinate cross-university program management]
    KK --> NN[Curriculum administration complete]
    
    LL --> NN
    MM --> NN
    
    %% Exception Flows
    OO{Import Interruption?} --> PP[System saves progress and provides recovery options]
    PP --> QQ[Resume import from last checkpoint]
    QQ --> G
    
    RR{Mapping Conflicts?} --> SS[System identifies conflicting requirements]
    SS --> TT[Game Master resolves conflicts with academic input]
    TT --> G
    
    %% Connect exception flows
    B --> OO
    G --> RR

    %% Styling
    classDef mainFlow fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef altFlow fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef exceptionFlow fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    classDef decisionFlow fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class A,B,D,G,K,P,R,U,V,X,GG,NN mainFlow
    class F,I,L,M,N,O,S,T,Z,BB,DD,EE,II,JJ,LL,MM altFlow
    class E,H,Y,AA,PP,QQ,SS,TT exceptionFlow
    class C,J,Q,W,CC,HH,OO,RR decisionFlow
```

---

## Summary

This restructured use case document provides comprehensive coverage for the RogueLearn platform with eliminated duplicates and proper alignment to current requirements. The use cases now reflect:

- **Guild-based event system** instead of tournament brackets
- **Current functional requirements** from the updated PRD
- **Proper numbering** without duplicates (UC-001 through UC-015)
- **Comprehensive coverage** of all major user interactions
- **Updated technical specifications** aligned with the current architecture

Each use case includes detailed scenarios, requirement mappings, and visual flowcharts to support development, testing, and stakeholder communication throughout the platform development lifecycle.