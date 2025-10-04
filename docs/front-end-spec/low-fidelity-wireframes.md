# Low-Fidelity Wireframes

## 1. Main Dashboard ("Character Sheet")

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn                    🔔 👤 ⚙️           │
├─────────────────────────────────────────────────────┤
│                                                     │
│  👤 [Avatar] Alex Chen          Level 12 ████▒▒▒   │
│  Class: Full-Stack Developer    XP: 2,340/3,000    │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎯 ACTIVE QUEST                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Master React Hooks                              │ │
│ │ Learn useState, useEffect, and custom hooks     │ │
│ │ Progress: ████████▒▒ 80%                        │ │
│ │                    [Continue Quest] ──────────► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ QUICK ACCESS                                        │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│ │ 📋      │ │ 🌳      │ │ ⚔️      │ │ 👥      │   │
│ │ Quests  │ │ Skills  │ │ Arsenal │ │ Party   │   │
│ └─────────┘ └─────────┘ └─────────┘ └─────────┘   │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📅 UPCOMING EVENTS                                  │
│ • Boss Fight: React Assessment - in 2 days         │
│ • Party Study Session - Tomorrow 7PM               │
│ • Guild Meeting: Code Warriors - Friday            │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🏆 RECENT ACHIEVEMENTS                              │
│ • JavaScript Fundamentals Mastered                 │
│ • First Boss Fight Victory                          │
│ • Study Streak: 7 days                             │
└─────────────────────────────────────────────────────┘
```

## 2. Character Creation Wizard

### Step 1: Route Selection (Curriculum)

```
┌─────────────────────────────────────────────────────┐
│ ← RogueLearn Character Creation            Step 1/4  │
├─────────────────────────────────────────────────────┤
│                                                     │
│ Progress: ██████▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ 25%               │
│                                                     │
│           🎓 Choose Your Academic Route             │
│            (University Curriculum)                  │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 💻 Software Engineering                         │ │
│ │ Focus on software development lifecycle         │ │
│ │ Core: Programming, Testing, Architecture        │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔬 Computer Science                             │ │
│ │ Theoretical foundations and algorithms          │ │
│ │ Core: Data Structures, Algorithms, Theory       │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🌐 Information Technology                       │ │
│ │ Systems management and infrastructure           │ │
│ │ Core: Networks, Security, System Admin          │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📊 Data Science                                 │ │
│ │ Analytics, statistics, and machine learning     │ │
│ │ Core: Statistics, ML, Data Analysis             │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│                              [Other Programs ▼]    │
│                                                     │
│                              [Skip for Now]        │
└─────────────────────────────────────────────────────┘
```

### Step 2: Class Selection (Roadmap.sh Specialization)

```
┌─────────────────────────────────────────────────────┐
│ ← RogueLearn Character Creation            Step 2/4  │
├─────────────────────────────────────────────────────┤
│                                                     │
│ Progress: ████████████▒▒▒▒▒▒▒▒ 50%                 │
│                                                     │
│ Selected Route: 💻 Software Engineering             │
│                                                     │
│           🎯 Choose Your Career Class               │
│         (Roadmap.sh Specialization)                 │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔧 Backend Developer                            │ │
│ │ Server-side logic and database management       │ │
│ │ Skills: APIs, Databases, Server Architecture    │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎨 Frontend Developer                           │ │
│ │ User interfaces and client-side experiences     │ │
│ │ Skills: React, CSS, UX/UI Design               │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 💻 Full-Stack Developer                         │ │
│ │ Master both frontend and backend technologies   │ │
│ │ Skills: React, Node.js, Databases              │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ ⚙️ DevOps Engineer                              │ │
│ │ Infrastructure, deployment, and automation      │ │
│ │ Skills: Docker, AWS, CI/CD, Monitoring         │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│                    [← Back]    [More Options ▼]    │
└─────────────────────────────────────────────────────┘
```

### Step 3: FPTU Specialization & Skill-based Roadmap Selection

```
┌─────────────────────────────────────────────────────┐
│ ← RogueLearn Character Creation            Step 3/4  │
├─────────────────────────────────────────────────────┤
│                                                     │
│ Progress: ██████████████████▒▒▒▒ 75%               │
│                                                     │
│ 🎯 Choose Your FPTU Specialization (Semester 5+)   │
│                                                     │
│ Route: 💻 Software Engineering                      │
│ Class: 💻 Full-Stack Developer                      │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🏫 FPTU SPECIALIZATION TRACKS                       │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔷 .NET Development                             │ │
│ │ Microsoft ecosystem & enterprise solutions      │ │
│ │ 🛣️ Roadmap: ASP.NET Core → Azure → C#          │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ ☕ Java Development                             │ │
│ │ Enterprise Java & Spring ecosystem              │ │
│ │ 🛣️ Roadmap: Spring Boot → Microservices → JVM  │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔒 DevSecOps                                    │ │
│ │ Security-focused development & operations       │ │
│ │ 🛣️ Roadmap: Docker → Kubernetes → Security     │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎮 Game Development                             │ │
│ │ Unity, Unreal Engine & game programming        │ │
│ │ 🛣️ Roadmap: Unity → C# → Game Design          │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📱 Mobile Development                           │ │
│ │ iOS, Android & cross-platform solutions        │ │
│ │ 🛣️ Roadmap: React Native → Flutter → Native   │ │
│ │                                    [Select] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│                    [← Back]    [More Options ▼]    │
└─────────────────────────────────────────────────────┘
```

### Step 3b: Detailed Roadmap Preview (After Selection)

```
┌─────────────────────────────────────────────────────┐
│ ← RogueLearn Character Creation            Step 3/4  │
├─────────────────────────────────────────────────────┤
│                                                     │
│ Progress: ██████████████████▒▒▒▒ 75%               │
│                                                     │
│ 📋 INTEGRATED LEARNING PATH PREVIEW                 │
│                                                     │
│ Route: 💻 Software Engineering                      │
│ Class: 💻 Full-Stack Developer                      │
│ Specialization: ☕ Java Development                 │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎓 FPTU CURRICULUM TIMELINE                         │
│                                                     │
│ 📅 Semester 1-4: Foundation                        │
│ ┌─────────────────────────────────────────────────┐ │
│ │ • Programming Fundamentals (Java/C#)            │ │
│ │ • Data Structures & Algorithms                  │ │
│ │ • Database Management Systems                   │ │
│ │ • Software Engineering Principles               │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ 📅 Semester 5-8: Java Specialization               │
│ ┌─────────────────────────────────────────────────┐ │
│ │ • Advanced Java Programming                     │ │
│ │ • Spring Framework & Spring Boot                │ │
│ │ • Microservices Architecture                    │ │
│ │ • Enterprise Integration Patterns               │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🛣️ ROADMAP.SH SKILL INTEGRATION                    │
│                                                     │
│ 🔧 Backend Java Developer Track                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Core: Java → Spring → Spring Boot               │ │
│ │ Database: JPA/Hibernate → PostgreSQL/MySQL      │ │
│ │ Testing: JUnit → Mockito → Integration Tests    │ │
│ │ Build: Maven/Gradle → CI/CD → Docker           │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ 🌐 Full-Stack Enhancement                           │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Frontend: React → TypeScript → Next.js          │ │
│ │ API: REST → GraphQL → OpenAPI                   │ │
│ │ Cloud: AWS/Azure → Kubernetes → Monitoring      │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🔗 SKILL PROGRESSION MAP                            │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ FPTU Java Basics ──► Roadmap Advanced Java      │ │
│ │ Database Theory ──► Spring Data JPA             │ │
│ │ Software Design ──► Microservices Patterns      │ │
│ │ Web Development ──► Spring MVC → React          │ │
│ │ DevOps Concepts ──► Docker → Kubernetes         │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ 🎯 RECOMMENDED QUEST SEQUENCE                       │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 1. Java Fundamentals (Semester 1-2)             │ │
│ │ 2. Spring Framework Mastery (Semester 5)        │ │
│ │ 3. Database & JPA Deep Dive (Semester 6)        │ │
│ │ 4. Microservices Architecture (Semester 7)      │ │
│ │ 5. Cloud Deployment & DevOps (Semester 8)       │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ⚡ Timeline: 4 years (FPTU) + Continuous Learning   │
│ 🏆 Career Outcome: Senior Java Full-Stack Engineer │
│                                                     │
│                [← Back] [Customize Path] [Next]    │
└─────────────────────────────────────────────────────┘
```

### Step 4: Character Confirmation

```
┌─────────────────────────────────────────────────────┐
│ ← RogueLearn Character Creation            Step 4/4  │
├─────────────────────────────────────────────────────┤
│                                                     │
│ Progress: ████████████████████████ 100%            │
│                                                     │
│           🎉 Your Character is Ready!               │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │                                                 │ │
│ │    👤 [Avatar Preview]                          │ │
│ │                                                 │ │
│ │    Alex Chen                                    │ │
│ │    Level 1 Full-Stack Developer                 │ │
│ │    Software Engineering Student                 │ │
│ │                                                 │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ 📊 CHARACTER SUMMARY                                │
│                                                     │
│ 🎓 Academic Route: Software Engineering             │
│ 🎯 Career Class: Full-Stack Developer               │
│ 📚 Learning Path: 4-year Integrated Program        │
│                                                     │
│ 🚀 STARTING QUESTS UNLOCKED                        │
│ • Programming Fundamentals (Java)                  │
│ • Web Development Basics (HTML/CSS)                │
│ • Version Control with Git                         │
│                                                     │
│ 🏆 INITIAL ACHIEVEMENTS                             │
│ • Character Created                                 │
│ • Learning Path Established                        │
│ • Ready for Adventure                              │
│                                                     │
│ ┌─────────────────┐ ┌─────────────────┐           │
│ │ [← Customize]   │ │ [BEGIN JOURNEY] │           │
│ │   More Changes  │ │   Start Learning│           │
│ └─────────────────┘ └─────────────────┘           │
│                                                     │
│ 💡 You can always adjust your path in Settings     │
└─────────────────────────────────────────────────────┘
```

## 3. Quest Log Interface

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Quest Log               🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🔍 [Search quests...]              📊 Filter ▼     │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎯 ACTIVE QUESTS (2)                               │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ ⭐⭐⭐ Master React Hooks                        │ │
│ │ 📚 Learning Quest • Frontend Development        │ │
│ │ Progress: ████████▒▒ 80% • Due: 2 days         │ │
│ │                              [Continue] ──────► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ ⭐⭐⭐⭐ Database Design Fundamentals             │ │
│ │ 📝 Assessment Quest • Backend Development       │ │
│ │ Progress: ██▒▒▒▒▒▒▒▒ 20% • Due: 1 week         │ │
│ │                              [Continue] ──────► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🔒 UPCOMING QUESTS (3)                             │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔒 ⭐⭐⭐⭐⭐ Advanced React Patterns            │ │
│ │ Requires: React Hooks Mastery                   │ │
│ │ Unlocks in: Complete current React quest        │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ ✅ COMPLETED QUESTS (12) [Show All ▼]              │
│                                                     │
│ • JavaScript Fundamentals ⭐⭐⭐                    │
│ • HTML & CSS Mastery ⭐⭐                           │
│ • Git Version Control ⭐⭐⭐                        │
└─────────────────────────────────────────────────────┘
```

## 4. Skill Tree Visualization

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Skill Tree              🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🔍 [Search skills...]    🎯 Full-Stack Path        │
│                                                     │
│ [−] [+] Zoom Controls           📍 Reset View       │
│                                                     │
├─────────────────────────────────────────────────────┤
│                                                     │
│                    ┌─────────┐                     │
│                    │   WEB   │                     │
│                    │ BASICS  │ ✅                  │
│                    │ HTML/CSS│                     │
│                    └────┬────┘                     │
│                         │                          │
│              ┌──────────┼──────────┐               │
│              │          │          │               │
│         ┌────▼───┐ ┌────▼───┐ ┌────▼───┐          │
│         │FRONTEND│ │BACKEND │ │DATABASE│          │
│         │ BASICS │ │ BASICS │ │ BASICS │          │
│         │React/JS│ │Node.js │ │  SQL   │ 🔒       │
│         └────┬───┘ └────┬───┘ └────────┘          │
│              │ ⚡80%    │ 🔒                       │
│              │          │                          │
│         ┌────▼───┐ ┌────▼───┐                     │
│         │ADVANCED│ │API DEV │                     │
│         │ REACT  │ │EXPRESS │                     │
│         │ HOOKS  │ │& REST  │ 🔒                  │
│         └────────┘ └────────┘                     │
│                                                     │
│ Legend: ✅ Mastered  ⚡ In Progress  🔒 Locked     │
│                                                     │
│ 💡 Click any node for details and learning paths   │
└─────────────────────────────────────────────────────┘
```

## 5. AI-Driven Questline System

### Quest Chapter Overview (Semester View)

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Quest Chapters          🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 📚 SEMESTER SPRING 2024 - SOFTWARE ENGINEERING     │
│ 🗓️ Jan 15 - May 15, 2024 • FPTU Academic Calendar │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📊 CHAPTER PROGRESS                                 │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📖 Chapter 1: Foundation Quests                 │ │
│ │ Weeks 1-10 • 7/10 Quests Complete ████████▒▒   │ │
│ │                                                 │ │
│ │ 🎯 Active Subjects:                             │ │
│ │ • PRF192 - Programming Fundamentals             │ │
│ │ • MAD101 - Discrete Mathematics                 │ │
│ │ • CEA201 - Computer Organization                │ │
│ │ • SSG104 - Communication Skills                 │ │
│ │                              [View Details] ──► │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📝 Exam Preparation Period                      │ │
│ │ Weeks 11-12 • Boss Fight Preparation           │ │
│ │ Status: 🔒 Unlocks after Quest 10              │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔄 Retake Period (Month 4)                      │ │
│ │ Recovery Quests for Failed Courses              │ │
│ │ Status: 🔒 Generated if needed                  │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🤖 AI QUEST GENERATION STATUS                      │
│                                                     │
│ ✅ FPTU Calendar Synced • Last Update: 2 hours ago │
│ ✅ Syllabus Integration Active                      │
│ ✅ Assignment Deadlines Mapped                      │
│ ⚡ Next Generation: Week 8 Quests (in 2 days)      │
│                                                     │
│ [🔄 Refresh Calendar] [⚙️ AI Settings] [📋 Manual] │
└─────────────────────────────────────────────────────┘
```

### AI Quest Generation Interface

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > AI Quest Generator      🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🤖 GENERATING WEEK 8 QUESTS...                     │
│                                                     │
│ Progress: ████████████▒▒▒▒ 75%                     │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📊 DATA SOURCES                                     │
│                                                     │
│ ✅ FPTU Academic Calendar                           │
│ • Week 8: March 4-10, 2024                         │
│ • No holidays this week                            │
│ • Midterm period approaching                       │
│                                                     │
│ ✅ Course Syllabus Integration                      │
│ • PRF192: Arrays & Functions (Week 8 content)      │
│ • MAD101: Graph Theory Basics                      │
│ • CEA201: Memory Management                        │
│ • SSG104: Presentation Skills                      │
│                                                     │
│ ✅ Student Profile Analysis                         │
│ • Type: 1st Time Student                           │
│ • Performance: Above Average (85%)                 │
│ • Learning Style: Visual + Hands-on                │
│ • Previous Quest Completion: 95%                   │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎯 GENERATED QUEST PREVIEW                          │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📋 Quest 8: "Array Master Challenge"            │ │
│ │                                                 │ │
│ │ 🎯 Main Objectives:                             │ │
│ │ • PRF192: Implement array sorting algorithms    │ │
│ │ • MAD101: Solve graph traversal problems        │ │
│ │ • CEA201: Optimize memory usage in C programs   │ │
│ │ • SSG104: Prepare technical presentation        │ │
│ │                                                 │ │
│ │ 📅 Timeline: March 4-10, 2024                   │ │
│ │ 🏆 Rewards: 250 XP + Algorithm Badge            │ │
│ │                                                 │ │
│ │ [✏️ Edit] [✅ Approve] [🔄 Regenerate]          │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [📋 Manual Assignments] [🔧 Advanced Settings]     │
└─────────────────────────────────────────────────────┘
```

### Adaptive Quest Logic (Student Types)

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Student Profile         🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 👤 Alex Chen - Semester 3                          │
│ 🎓 Software Engineering • Full-Stack Developer     │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🧠 ADAPTIVE QUEST LOGIC                             │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🆕 1ST TIME STUDENT MODE                        │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ ✅ Fresh Quest Generation                   │ │ │
│ │ │ • Based on curriculum + FPTU portal data    │ │ │
│ │ │ • Standard difficulty progression           │ │ │
│ │ │ • Full syllabus coverage                    │ │ │
│ │ │ • Introductory learning paths               │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔄 RETURNING STUDENT MODE                       │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 🧠 Quest Memory System Active               │ │ │
│ │ │ • References previous semester performance  │ │ │
│ │ │ • Adapts to learning patterns               │ │ │
│ │ │ • Builds on mastered concepts               │ │ │
│ │ │ • Personalized difficulty scaling           │ │ │
│ │ │                                             │ │ │
│ │ │ 📊 Previous Performance:                    │ │ │
│ │ │ • Semester 2: 87% average                  │ │ │
│ │ │ • Strong: Programming, Weak: Math           │ │ │
│ │ │ • Preferred: Hands-on projects             │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🚨 FAILED COURSE RECOVERY MODE                  │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 🎯 Specialized Remediation Quests           │ │ │
│ │ │ • Targets specific knowledge gaps           │ │ │
│ │ │ • Intensive practice modules                │ │ │
│ │ │ • Additional support resources              │ │ │
│ │ │ • Mentor assignment integration             │ │ │
│ │ │                                             │ │ │
│ │ │ 📋 Failed Course: MAD101 (Grade: D)         │ │ │
│ │ │ • Gap Analysis: Graph Theory, Logic         │ │ │
│ │ │ • Recovery Timeline: 4 weeks intensive     │ │ │
│ │ │ • Success Rate: 78% with this approach     │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [⚙️ Switch Mode] [📊 Performance History] [🎯 Goals]│
└─────────────────────────────────────────────────────┘
```

### Manual Assignment Integration

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Manual Assignments      🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 📝 ADD ASSIGNMENT TO QUEST SYSTEM                   │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📋 ASSIGNMENT DETAILS                               │
│                                                     │
│ Subject: [PRF192 - Programming Fundamentals ▼]     │
│                                                     │
│ Assignment Name:                                    │
│ [Array Sorting Implementation Lab        ]          │
│                                                     │
│ Description:                                        │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Implement bubble sort, selection sort, and     │ │
│ │ quick sort algorithms in C. Compare             │ │
│ │ performance and analyze time complexity.        │ │
│ │                                                 │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ Due Date: [March 8, 2024 ▼] [11:59 PM ▼]          │
│                                                     │
│ Weight: [15%] of final grade                       │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🤖 AI QUEST MATCHING                                │
│                                                     │
│ ✅ Automatic Quest Assignment Detected              │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Suggested Quest: Week 8 - Array Master       │ │
│ │                                                 │ │
│ │ Match Confidence: ████████████████▒▒ 95%        │ │
│ │                                                 │ │
│ │ Reasoning:                                      │ │
│ │ • Due date aligns with Week 8 (March 4-10)     │ │
│ │ • Content matches syllabus: Arrays & Sorting   │ │
│ │ • Difficulty appropriate for quest level       │ │
│ │ • Integrates with existing objectives          │ │
│ │                                                 │ │
│ │ [✅ Accept Match] [✏️ Modify] [❌ Reject]       │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ Alternative Suggestions:                            │
│ • Week 7 Quest (Late addition) - 60% match         │
│ • Week 9 Quest (Early preparation) - 40% match     │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎯 QUEST OBJECTIVE PREVIEW                          │
│                                                     │
│ New Objective Added to Quest 8:                     │
│ "Complete Array Sorting Lab assignment with        │
│ performance analysis and submit by March 8"        │
│                                                     │
│ Reward: +50 XP + Programming Badge Progress         │
│                                                     │
│ [💾 Save Assignment] [🔄 Reset] [❌ Cancel]         │
└─────────────────────────────────────────────────────┘
```

### Quest Memory System & Continuity

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Quest Memory            🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🧠 QUEST HISTORY & LEARNING CONTINUITY             │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📊 SEMESTER PROGRESSION                             │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📚 Semester 1 (Completed) - Fall 2023           │ │
│ │ ████████████████████ 100%                      │ │
│ │                                                 │ │
│ │ 🎯 Key Achievements:                            │ │
│ │ • Mastered: Basic Programming (A+)              │ │
│ │ • Struggled: Mathematics (C+)                   │ │
│ │ • Learning Style: Visual + Practice             │ │
│ │ • Completion Rate: 98% (49/50 quests)          │ │
│ │                                                 │ │
│ │ 🏆 Earned Badges: 12 • XP Gained: 2,450        │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📚 Semester 2 (Completed) - Spring 2024         │ │
│ │ ████████████████▒▒▒▒ 87%                       │ │
│ │                                                 │ │
│ │ 🎯 Performance Analysis:                        │ │
│ │ • Improved: Data Structures (B+ → A-)          │ │
│ │ • Maintained: Programming Skills (A+)          │ │
│ │ • Challenge: Advanced Math (C → C+)            │ │
│ │ • Completion Rate: 87% (43/50 quests)          │ │
│ │                                                 │ │
│ │ 🏆 Earned Badges: 8 • XP Gained: 2,100         │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📚 Semester 3 (Current) - Fall 2024             │ │
│ │ ████████▒▒▒▒▒▒▒▒▒▒▒▒ 40%                       │ │
│ │                                                 │ │
│ │ 🤖 AI Adaptations Based on History:            │ │
│ │ • Math Support: Extra practice quests added    │ │
│ │ • Programming: Advanced challenges unlocked    │ │
│ │ • Learning Style: More visual aids included    │ │
│ │ • Pacing: Adjusted for 87% completion target   │ │
│ │                                                 │ │
│ │ 🎯 Current Progress: 20/50 quests              │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🔍 LEARNING PATTERN ANALYSIS                        │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📈 Performance Trends                           │ │
│ │                                                 │ │
│ │ Programming Skills:     ████████████████████    │ │
│ │ Consistent Excellence • Trend: ↗️ Improving     │ │
│ │                                                 │ │
│ │ Mathematical Concepts:  ████████▒▒▒▒▒▒▒▒▒▒▒▒    │ │
│ │ Needs Support • Trend: ↗️ Slowly Improving      │ │
│ │                                                 │ │
│ │ Project Management:     ████████████████▒▒▒▒    │ │
│ │ Above Average • Trend: ➡️ Stable               │ │
│ │                                                 │ │
│ │ Communication Skills:   ████████████▒▒▒▒▒▒▒▒    │ │
│ │ Room for Growth • Trend: ↗️ Improving           │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Personalized Recommendations                 │ │
│ │                                                 │ │
│ │ Based on 3-semester learning history:          │ │
│ │                                                 │ │
│ │ ✅ Continue advanced programming challenges     │ │
│ │ 🔧 Add extra math tutoring sessions            │ │
│ │ 📊 Include more visual learning materials       │ │
│ │ 👥 Suggest peer study groups for weak areas    │ │
│ │ ⏰ Optimize quest timing for better completion  │ │
│ │                                                 │ │
│ │ [📋 Apply Recommendations] [⚙️ Customize]      │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [📊 Detailed Analytics] [🔄 Reset Preferences]     │
└─────────────────────────────────────────────────────┘
```

## 6. Arsenal Management (Personal Study Notes)

### Arsenal Main Interface

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Arsenal                 🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 📚 MY STUDY ARSENAL                                 │
│ Personal Knowledge Base & Study Notes               │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🔍 SEARCH & FILTERS                                 │
│                                                     │
│ [🔍 Search notes, tags, content...        ] [🔎]   │
│                                                     │
│ 📂 Quick Filters:                                   │
│ [📋 All Notes] [⭐ Favorites] [📅 Recent] [🏷️ Tags] │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📁 ORGANIZATION SIDEBAR          │ 📝 NOTES GRID    │
│                                  │                  │
│ ┌──────────────────────────────┐ │ ┌──────────────┐ │
│ │ 📚 SUBJECTS                  │ │ │ 📋 Data Str.. │ │
│ │ ├─ 💻 Programming (45)       │ │ │ PRF192 • 2d   │ │
│ │ │  ├─ Arrays & Pointers      │ │ │ ⭐ 🏷️ arrays  │ │
│ │ │  ├─ Functions              │ │ │              │ │
│ │ │  └─ Data Structures        │ │ └──────────────┘ │
│ │ ├─ 🧮 Mathematics (23)       │ │                  │
│ │ │  ├─ Discrete Math          │ │ ┌──────────────┐ │
│ │ │  └─ Linear Algebra         │ │ │ 🔗 Graph Th.. │ │
│ │ ├─ 🏗️ System Design (12)     │ │ │ MAD101 • 5d   │ │
│ │ └─ 💬 Communication (8)      │ │ │ 🏷️ theory    │ │
│ │                              │ │ │              │ │
│ │ 🏷️ TAGS                      │ │ └──────────────┘ │
│ │ • important (15)             │ │                  │
│ │ • exam-prep (8)              │ │ ┌──────────────┐ │
│ │ • quick-ref (22)             │ │ │ 📊 Memory M.. │ │
│ │ • examples (31)              │ │ │ CEA201 • 1w   │ │
│ │                              │ │ │ ⭐ 🏷️ systems │ │
│ │ 📅 RECENT                    │ │ │              │ │
│ │ • Today (3)                  │ │ └──────────────┘ │
│ │ • This Week (12)             │ │                  │
│ │ • This Month (28)            │ │ [+ New Note]     │ │
│ └──────────────────────────────┘ │                  │
├─────────────────────────────────────────────────────┤
│ 🎯 QUEST INTEGRATION                                │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📋 Current Quest: Array Master Challenge        │ │
│ │                                                 │ │
│ │ 📝 Suggested Notes to Review:                   │ │
│ │ • Array Sorting Algorithms (⭐ Favorited)       │ │
│ │ • Time Complexity Analysis                      │ │
│ │ • C Programming Best Practices                  │ │
│ │                                                 │ │
│ │ 💡 Quick Actions:                               │ │
│ │ [📝 Create Quest Note] [🔗 Link to Objective]  │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [📊 Analytics] [⚙️ Settings] [📤 Export] [🔄 Sync] │
└─────────────────────────────────────────────────────┘
```

### Rich Text Editor (Notion-like Interface)

```
┌─────────────────────────────────────────────────────┐
│ ☰ Arsenal > Data Structures Notes      🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│ 📝 EDITOR TOOLBAR                                   │
│                                                     │
│ [💾] [↶] [↷] │ [B] [I] [U] [S] │ [🎨] [🔗] [📷] │ │
│ Save Undo Redo   Bold Italic Under Strike Color Link Image │
│                                                     │
│ [H1▼] [📝▼] [📋▼] │ [≡] [•] [1.] [☑] │ [💬] [📊] │ │
│ Heading Style Block   Align List Num Check Comment Chart │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📄 DOCUMENT HEADER                                  │
│                                                     │
│ 📋 Array Sorting Algorithms                         │
│ 🏷️ arrays, sorting, algorithms, PRF192             │
│ 📅 Created: March 5, 2024 • Modified: 2 hours ago  │
│ 🎯 Linked to: Quest 8 - Array Master Challenge     │
│                                                     │
├─────────────────────────────────────────────────────┤
│ ✏️ CONTENT EDITOR                                   │
│                                                     │
│ # Array Sorting Algorithms                          │
│                                                     │
│ ## Overview                                         │
│ Sorting algorithms are fundamental to computer      │
│ science and essential for the PRF192 course...     │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 💡 Key Insight                                  │ │
│ │ Time complexity is crucial when choosing        │ │
│ │ between different sorting algorithms             │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ## Bubble Sort                                      │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ ```c                                            │ │
│ │ void bubbleSort(int arr[], int n) {             │ │
│ │     for (int i = 0; i < n-1; i++) {            │ │
│ │         for (int j = 0; j < n-i-1; j++) {      │ │
│ │             if (arr[j] > arr[j+1]) {           │ │
│ │                 // Swap elements               │ │
│ │                 int temp = arr[j];             │ │
│ │                 arr[j] = arr[j+1];             │ │
│ │                 arr[j+1] = temp;               │ │
│ │             }                                   │ │
│ │         }                                       │ │
│ │     }                                           │ │
│ │ }                                               │ │
│ │ ```                                             │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ⏱️ **Time Complexity:** O(n²)                      │
│ 💾 **Space Complexity:** O(1)                      │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📊 Performance Comparison                       │ │
│ │                                                 │ │
│ │ | Algorithm    | Best | Average | Worst |      │ │
│ │ |-------------|------|---------|-------|       │ │
│ │ | Bubble Sort | O(n) | O(n²)   | O(n²) |      │ │
│ │ | Quick Sort  | O(n log n) | O(n log n) | O(n²) | │ │
│ │ | Merge Sort  | O(n log n) | O(n log n) | O(n log n) | │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [+ Add Block ▼] [📷 Image] [📊 Table] [💬 Comment] │
└─────────────────────────────────────────────────────┘
```

### Note Organization & Management System

```
┌─────────────────────────────────────────────────────┐
│ ☰ Arsenal > Organization Manager       🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🗂️ ADVANCED ORGANIZATION TOOLS                      │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🏷️ TAG MANAGEMENT                                   │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎨 Create New Tag                               │ │
│ │                                                 │ │
│ │ Tag Name: [algorithms           ]               │ │
│ │ Color: [🔴] [🟠] [🟡] [🟢] [🔵] [🟣] [⚫]        │ │
│ │ Description: [Sorting and search algorithms]    │ │
│ │                                                 │ │
│ │ [💾 Create Tag] [❌ Cancel]                     │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ 📊 Existing Tags:                                   │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔴 important (15 notes) [✏️] [🗑️]              │ │
│ │ 🟢 exam-prep (8 notes) [✏️] [🗑️]               │ │
│ │ 🔵 quick-ref (22 notes) [✏️] [🗑️]              │ │
│ │ 🟡 examples (31 notes) [✏️] [🗑️]               │ │
│ │ 🟠 algorithms (12 notes) [✏️] [🗑️]             │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📁 FOLDER STRUCTURE                                 │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📚 Semester 3 - Fall 2024                      │ │
│ │ ├─ 💻 PRF192 - Programming Fundamentals         │ │
│ │ │  ├─ 📝 Lecture Notes (12)                    │ │
│ │ │  ├─ 🧪 Lab Exercises (8)                     │ │
│ │ │  ├─ 📋 Assignment Solutions (5)              │ │
│ │ │  └─ 🎯 Exam Preparation (3)                  │ │
│ │ ├─ 🧮 MAD101 - Discrete Mathematics            │ │
│ │ │  ├─ 📖 Theory Notes (15)                     │ │
│ │ │  ├─ 🔢 Problem Sets (10)                     │ │
│ │ │  └─ 📊 Formula References (7)                │ │
│ │ ├─ 🏗️ CEA201 - Computer Organization           │ │
│ │ │  ├─ 💾 Memory Management (6)                 │ │
│ │ │  ├─ ⚡ CPU Architecture (4)                  │ │
│ │ │  └─ 🔧 Assembly Language (8)                 │ │
│ │ └─ 💬 SSG104 - Communication Skills            │ │
│ │    ├─ 🎤 Presentation Tips (5)                 │ │
│ │    └─ ✍️ Writing Guidelines (3)                │ │
│ │                                                 │ │
│ │ [📁 New Folder] [📝 New Note] [🔄 Reorganize]  │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🔍 ADVANCED SEARCH                                  │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔎 Search Query: [bubble sort algorithm]        │ │
│ │                                                 │ │
│ │ 📂 Filter by:                                   │ │
│ │ Subject: [PRF192 ▼] Date: [Last 30 days ▼]     │ │
│ │ Tags: [🟡 algorithms] [🔴 important]           │ │
│ │ Type: [📝 Notes] [📷 Images] [📊 Tables]       │ │
│ │                                                 │ │
│ │ 🎯 Search Results (8 found):                    │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 📋 Bubble Sort Implementation               │ │ │
│ │ │ PRF192 • March 5 • 🟡 algorithms 🔴 important │ │ │
│ │ │ "...void bubbleSort(int arr[], int n) {...  │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 📊 Sorting Algorithm Comparison             │ │ │
│ │ │ PRF192 • March 3 • 🟡 algorithms 🟢 exam-prep │ │ │
│ │ │ "...Time complexity comparison table for... │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [🔄 Clear Filters] [💾 Save Search] [📤 Export]    │
└─────────────────────────────────────────────────────┘
```

### Quest Integration & Smart Suggestions

```
┌─────────────────────────────────────────────────────┐
│ ☰ Arsenal > Quest Integration           🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🎯 QUEST-LINKED STUDY MATERIALS                     │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📋 CURRENT QUEST CONTEXT                            │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Active Quest: Array Master Challenge          │ │
│ │ 📅 Due: March 10, 2024 (3 days remaining)       │ │
│ │                                                 │ │
│ │ 📚 Quest Objectives:                            │ │
│ │ ✅ Understand array fundamentals                │ │
│ │ 🔄 Implement sorting algorithms                 │ │
│ │ ⏳ Analyze time complexity                      │ │
│ │ ⏳ Complete lab assignment                      │ │
│ │                                                 │ │
│ │ 🤖 AI Suggestions:                              │ │
│ │ "Based on your quest progress, you should       │ │
│ │ review your sorting algorithm notes and         │ │
│ │ create a complexity analysis summary."          │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📝 LINKED NOTES & MATERIALS                         │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔗 Directly Linked to Quest                     │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 📋 Array Sorting Algorithms                 │ │ │
│ │ │ ⭐ Favorited • 🟡 algorithms                │ │ │
│ │ │ Last updated: 2 hours ago                   │ │ │
│ │ │ [👁️ View] [✏️ Edit] [🔗 Unlink]            │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 📊 Time Complexity Cheat Sheet             │ │ │
│ │ │ 🟢 exam-prep • 🟡 algorithms                │ │ │
│ │ │ Last updated: 1 day ago                     │ │ │
│ │ │ [👁️ View] [✏️ Edit] [🔗 Unlink]            │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 💡 Smart Suggestions                            │ │
│ │                                                 │ │
│ │ Based on quest objectives and your notes:       │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 📝 C Programming Best Practices             │ │ │
│ │ │ Relevance: 85% • PRF192                     │ │ │
│ │ │ "Contains coding standards for arrays"      │ │ │
│ │ │ [🔗 Link to Quest] [👁️ Preview]            │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 🧪 Lab 3: Sorting Implementation            │ │ │
│ │ │ Relevance: 92% • PRF192                     │ │ │
│ │ │ "Step-by-step sorting lab solution"        │ │ │
│ │ │ [🔗 Link to Quest] [👁️ Preview]            │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🚀 QUICK ACTIONS                                    │
│                                                     │
│ [📝 Create Quest Note] [🔗 Link Existing Note]     │
│ [📋 Generate Study Plan] [📊 Progress Summary]     │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Create Quest-Specific Note                   │ │
│ │                                                 │ │
│ │ Template: [📋 Quest Summary ▼]                  │ │
│ │ • Quest Summary Template                        │ │
│ │ • Problem-Solution Template                     │ │
│ │ • Code Implementation Template                  │ │
│ │ • Study Guide Template                          │ │
│ │                                                 │ │
│ │ Auto-tags: 🎯 quest-8, 🟡 algorithms           │ │
│ │ Auto-link: ✅ Link to current quest             │ │
│ │                                                 │ │
│ │ [📝 Create Note] [❌ Cancel]                    │ │
│ └─────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────┘
```

### Collaboration & Sharing Features

```
┌─────────────────────────────────────────────────────┐
│ ☰ Arsenal > Collaboration Hub          🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 👥 STUDY GROUP COLLABORATION                        │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🏠 MY STUDY GROUPS                                  │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 👥 PRF192 Study Squad                           │ │
│ │ 👤 5 members • 📝 23 shared notes               │ │
│ │ 🔴 Active • Last activity: 2 hours ago          │ │
│ │                                                 │ │
│ │ Recent Activity:                                │ │
│ │ • 👤 Alice shared "Pointer Basics"             │ │
│ │ • 👤 Bob commented on "Array Methods"          │ │
│ │ • 👤 Carol added "Memory Management Tips"      │ │
│ │                                                 │ │
│ │ [👁️ View Group] [📝 Share Note] [💬 Chat]      │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 👥 MAD101 Math Masters                         │ │
│ │ 👤 8 members • 📝 31 shared notes               │ │
│ │ 🟡 Moderate • Last activity: 1 day ago         │ │
│ │                                                 │ │
│ │ Recent Activity:                                │ │
│ │ • 👤 David shared "Graph Theory Proofs"        │ │
│ │ • 👤 Emma updated "Logic Gates Summary"        │ │
│ │                                                 │ │
│ │ [👁️ View Group] [📝 Share Note] [💬 Chat]      │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [➕ Create Group] [🔍 Find Groups] [📤 Invitations] │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📤 SHARING & PERMISSIONS                            │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📋 Share Note: "Binary Search Trees"           │ │
│ │                                                 │ │
│ │ 🎯 Share with:                                  │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 👥 Study Groups:                            │ │ │
│ │ │ ☑️ PRF192 Study Squad                       │ │ │
│ │ │ ☐ Advanced Algorithms Club                  │ │ │
│ │ │ ☐ CS Fundamentals Group                     │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │                                                 │ │
│ │ 👤 Individual Users:                            │ │
│ │ [alice.nguyen@fpt.edu.vn        ] [➕ Add]     │ │
│ │                                                 │ │
│ │ 🔒 Permission Level:                            │ │
│ │ ◉ View Only                                     │ │
│ │ ○ Comment & Suggest                             │ │
│ │ ○ Edit & Collaborate                            │ │
│ │                                                 │ │
│ │ 📅 Expiration: [Never ▼]                       │ │
│ │ 🔔 Notify recipients: ☑️                       │ │
│ │                                                 │ │
│ │ [📤 Share Note] [❌ Cancel]                     │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 💬 COLLABORATIVE FEATURES                           │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📝 Shared Note: "Data Structures Overview"     │ │
│ │ 👤 Originally by: You • 👥 3 collaborators     │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 💬 Comments & Discussions (5)               │ │ │
│ │ │                                             │ │ │
│ │ │ 👤 Alice • 2 hours ago                      │ │ │
│ │ │ "Great explanation of linked lists! Could   │ │ │
│ │ │ you add examples for doubly linked lists?"  │ │ │
│ │ │ [💬 Reply] [👍 2] [❤️ 1]                   │ │ │
│ │ │                                             │ │ │
│ │ │ 👤 Bob • 1 hour ago                         │ │ │
│ │ │ "I added a section on stack implementation  │ │ │
│ │ │ using arrays. Please review!"               │ │ │
│ │ │ [💬 Reply] [👍 1]                          │ │ │
│ │ │                                             │ │ │
│ │ │ [💬 Add Comment]                            │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 📝 Suggested Edits (2 pending)             │ │ │
│ │ │                                             │ │ │
│ │ │ 👤 Carol suggested:                         │ │ │
│ │ │ "Add time complexity for tree operations"   │ │ │
│ │ │ Line 45-47                                  │ │ │
│ │ │ [✅ Accept] [❌ Reject] [💬 Discuss]       │ │ │
│ │ │                                             │ │ │
│ │ │ 👤 David suggested:                         │ │ │
│ │ │ "Fix typo in heap definition"               │ │ │
│ │ │ Line 23                                     │ │ │
│ │ │ [✅ Accept] [❌ Reject] [💬 Discuss]       │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📊 COLLABORATION ANALYTICS                          │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📈 Your Sharing Impact                          │ │
│ │                                                 │ │
│ │ 📝 Notes Shared: 47                             │ │
│ │ 👥 People Helped: 23                            │ │
│ │ 👍 Total Likes: 156                             │ │
│ │ 💬 Comments Received: 89                        │ │
│ │                                                 │ │
│ │ 🏆 Top Shared Notes:                            │ │
│ │ 1. "C Programming Cheat Sheet" (45 views)      │ │
│ │ 2. "Algorithm Complexity Guide" (38 views)     │ │
│ │ 3. "Database Design Patterns" (32 views)       │ │
│ │                                                 │ │
│ │ [📊 View Full Analytics]                        │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [🔄 Sync Changes] [📱 Mobile App] [⚙️ Settings]    │
└─────────────────────────────────────────────────────┘
```

## 6. Boss Fight Arena

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Boss Fight              🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│           🏰 THE REACT TEMPLE CHALLENGE             │
│                                                     │
│  👤 Alex Chen (Lv.12)        VS        🐉 React    │
│  Full-Stack Developer                   Dragon      │
│  HP: ████████████ 100%                 HP: ███████ │
│  Ready: ⚡ 85%                         Lv. 15      │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📋 BATTLE OBJECTIVES                                │
│                                                     │
│ ✅ Complete React Hooks Quest (Done)               │
│ ⚡ Score 80%+ on Practice Tests (85% - Ready!)     │
│ 🔒 Form a Party (Optional - Bonus XP)              │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎁 VICTORY REWARDS                                  │
│                                                     │
│ • 500 XP + Level Up Bonus                          │
│ • "React Master" Achievement Badge                  │
│ • Unlock: Advanced React Patterns Quest            │
│ • Unlock: Next Boss Fight (Node.js Kraken)         │
│                                                     │
├─────────────────────────────────────────────────────┤
│ ⚔️ BATTLE ACTIONS                                   │
│                                                     │
│ ┌─────────────────┐ ┌─────────────────┐           │
│ │  [ENTER BATTLE] │ │ [STUDY MORE]    │           │
│ │   ⚡ Ready!     │ │  📚 Improve     │           │
│ └─────────────────┘ └─────────────────┘           │
│                                                     │
│ ┌─────────────────┐ ┌─────────────────┐           │
│ │ [FIND PARTY]    │ │ [POSTPONE]      │           │
│ │  👥 Team Up     │ │  ⏰ Later       │           │
│ └─────────────────┘ └─────────────────┘           │
│                                                     │
│ 💡 Tip: Team battles provide bonus XP and support! │
└─────────────────────────────────────────────────────┘
```