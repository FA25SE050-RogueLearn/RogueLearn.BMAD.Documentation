# Quest Wireframes

## Overview

This document outlines the wireframes for the quest system interface, focusing on the core user experience for quest discovery, tracking, and completion. The quest system integrates with the skill tree and provides structured learning paths for students.

## Quest Line Interface

The Quest Line Interface provides a visual progression path through structured learning chapters, showing the student's journey from foundational concepts to advanced mastery.

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Questline               🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📚 Data Structures & Algorithms - Chapter 1     │ │
│ │ ─────────────────────────────────────────────── │ │
│ │ 🔥 7-day streak  📈 3/10 quests  ⭐ 2,450 XP   │ │
│ │ 🎯 Current: Arrays & Sorting     ⏱️ 2h 15m     │ │
│ │ 💡 "Master the fundamentals before complexity"  │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│                Quest Line Interface                 │
│                                                     │
│              ┌─────────────────────┐                │
│              │                     │                │
│              │    Quest Chapter 1  │                │
│              │    (Semester 1)     │                │
│              │                     │                │
│              └─────────────────────┘                │
│                         │                           │
│                    ┌─────────┐                      │
│                    │    1    │ ←── progress ring    │
│                    │   ✓✓✓   │     (completed)      │
│                    └─────────┘                      │
│                         │                           │
│                    ┌─────────┐                      │
│                    │    2    │                      │
│                    │   ✓✓○   │                      │
│                    └─────────┘                      │
│                         │                           │
│                    ┌─────────┐                      │
│                    │    3    │                      │
│                    │   ✓○○   │                      │
│                    └─────────┘                      │
│                         │                           │
│                    ┌─────────┐                      │
│                    │    4    │                      │
│                    │   ○○○   │                      │
│                    └─────────┘                      │
│                         │                           │
│                    ┌─────────┐                      │
│                    │    5    │                      │
│                    │   ○○○   │                      │
│                    └─────────┘                      │
│                         │                           │
│                    ┌─────────┐                      │
│                    │    6    │                      │
│                    │   ○○○   │                      │
│                    └─────────┘                      │
│                         │                           │
│                    ┌─────────┐                      │
│                    │    7    │                      │
│                    │   ○○○   │                      │
│                    └─────────┘                      │
│                         │                           │
│                    ┌─────────┐                      │
│                    │    8    │                      │
│                    │   ○○○   │                      │
│                    └─────────┘                      │
│                         │                           │
│                    ┌─────────┐                      │
│                    │    9    │                      │
│                    │   ○○○   │                      │
│                    └─────────┘                      │
│                         │                           │
│                    ┌─────────┐                      │
│                    │   10    │                      │
│                    │   ○○○   │                      │
│                    └─────────┘                      │
│                         │                           │
│              ┌─────────────────────┐                │
│              │                     │                │
│              │    Quest Chapter 2  │                │
│              │    (Semester 2)     │                │
│              │                     │                │
│              └─────────────────────┘                │
│                                                     │
│ Legend:                                             │
│ ✓ = Completed Quest    ○ = Locked Quest             │
│ 🔄 = Current Quest     ⏳ = Available Quest         │
│                                                     │
│ [📊 Progress Overview] [🎯 Jump to Current]         │
└─────────────────────────────────────────────────────┘
```

### Quest Line Features

**Chapter Recap Section:**
- **Chapter Title:** Current learning module with clear academic context
- **Streak Counter:** Visual fire emoji with consecutive days of activity
- **Progress Metrics:** Quest completion ratio and total XP earned
- **Current Focus:** Active quest or learning topic with time invested
- **Motivational Quote:** Contextual learning wisdom or encouragement
- **Visual Design:** Contained card with subtle borders and organized layout

**Visual Progress Tracking:**
- Circular quest nodes numbered sequentially (1-10 per chapter)
- Progress indicators within each node (✓✓✓, ✓✓○, ✓○○, ○○○)
- Clear visual connection between sequential quests
- Chapter separators for semester/course boundaries

**Interactive Elements:**
- Tap/click any unlocked quest to view details
- Progress ring animation for current quest
- Smooth scrolling between chapters
- Quick navigation to current active quest

**Quest States:**
- **Completed (✓✓✓):** All objectives finished, full XP earned
- **In Progress (✓✓○):** Some objectives completed
- **Available (✓○○):** Prerequisites met, ready to start
- **Locked (○○○):** Prerequisites not yet completed

**Chapter Organization:**
- Each chapter represents a semester or major learning module
- 10 quests per chapter (adjustable based on curriculum)
- Clear visual separation between academic periods
- Chapter titles indicate the learning focus or time period

## Quest Log Interface

The main quest dashboard where students can view and manage their learning quests.

## Quest Detail View

Detailed view of an individual quest showing multiple subjects, their objectives, progress, and related resources with full Arsenal integration. Features an **Enhanced Knowledge Graph System** that dynamically generates micro-objectives based on relationship types (is_alternative_to, complements, is_foundation_for, is_applied_as) to bridge curriculum gaps with industry-relevant skills.

### Key Features:
- **🧩 Connect the Dots**: Complementary technologies that work together
- **🧭 Expand Your Horizons**: Alternative technologies and broader concepts  
- **🚀 Level Up**: Practical applications of learned concepts
- **🎮 Knowledge Graph System**: AI-driven objective generation with clear progression logic
- **🚀 Mission Control**: Full-screen workspace for Local Project and IDE Assignment objectives

```
┌─────────────────────────────────────────────────────┐
│ ← Back to Quest Log    🎯 Data Structures Mastery   │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 📚 Quest 3 • 🏆 150 XP • ⏱️ Due: Dec 20, 2024     │
│ ████████░░ 75% Complete • 9/12 Objectives           │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📋 QUEST SUBJECTS & OBJECTIVES                      │
│                                                     │
│ 📘 Subject 1: Array Algorithms (PRF192)            │
│ ████████░░ 80% • 4/5 objectives                     │
│                                                     │
│ ✅ 1.1 Understand array data structure              │
│    💡 Completed: Dec 8 • 🏆 15 XP earned           │
│    👤 Manual objective                              │
│                                                     │
│ ✅ 1.2 Learn linear search algorithm                │
│    💡 Completed: Dec 9 • 🏆 15 XP earned           │
│    🤖 AI-generated from curriculum analysis        │
│                                                     │
│ ✅ 1.3 Implement binary search                      │
│    💡 Completed: Dec 10 • 🏆 20 XP earned          │
│    🤖 AI-generated from curriculum analysis        │
│                                                     │
│ ✅ 1.4 Master selection sort                        │
│    💡 Completed: Dec 12 • 🏆 15 XP earned          │
│    👤 Manual objective                              │
│                                                     │
│ 🎯 1.5 Implement bubble sort algorithm              │
│    📍 Current objective • 🏆 25 XP pending          │
│    📝 Write bubble sort in C with complexity        │
│    ⏱️ Estimated time: 45 minutes                   │
│    🤖 AI-generated from gap analysis               │
│    [⚡ Start Now] [📚 Study Materials]             │
│                                                     │
│ ─────────────────────────────────────────────────── │
│                                                     │
│ 📗 Subject 2: Data Structure Theory (MAD101)       │
│ ██████████ 100% • 4/4 objectives                   │
│                                                     │
│ ✅ 2.1 Define abstract data types                   │
│    💡 Completed: Dec 5 • 🏆 20 XP earned           │
│    👤 Manual objective                              │
│                                                     │
│ ✅ 2.2 Compare array vs linked list                 │
│    💡 Completed: Dec 6 • 🏆 20 XP earned           │
│    🤖 AI-generated from curriculum analysis        │
│                                                     │
│ ✅ 2.3 Analyze time complexity                      │
│    💡 Completed: Dec 7 • 🏆 25 XP earned           │
│    🤖 AI-generated from curriculum analysis        │
│                                                     │
│ ✅ 2.4 Document performance trade-offs              │
│    💡 Completed: Dec 8 • 🏆 15 XP earned           │
│    👤 Manual objective                              │
│                                                     │
│ ─────────────────────────────────────────────────── │
│                                                     │
│ 📘 Subject 3: Web API Project (PRN212)             │
│ ░░░░░░░░░░ 0% • 0/3 objectives                      │
│                                                     │
│ 🚀 3.1 Build REST API with 3 endpoints              │
│    📍 Local Project • 🏆 250 XP pending            │
│    📝 Create Web API following assignment specs     │
│    ⏱️ Estimated time: 8-12 hours                   │
│    🎯 IDE Assignment (Visual Studio required)       │
│    [🚀 Launch Mission Control] [📚 Resources]      │
│                                                     │
│ 📝 3.2 Document API architecture                    │
│    📍 Note-Taking Objective • 🏆 50 XP pending     │
│    📝 Create comprehensive notes on MVC pattern     │
│    ⏱️ Estimated time: 2 hours                      │
│    🎯 Knowledge Documentation Required              │
│    [🚀 Launch Mission Control] [📝 Start Notes]    │
│                                                     │
│ 🧪 3.3 Write unit tests for endpoints               │
│    📍 Local Project • 🏆 100 XP pending            │
│    📝 Implement comprehensive test coverage         │
│    ⏱️ Estimated time: 4-6 hours                    │
│    🎯 IDE Assignment (Testing framework required)   │
│    [🚀 Launch Mission Control] [📚 Testing Guide]  │
│                                                     │
│ ─────────────────────────────────────────────────── │
│                                                     │
│ ✨ YOUR NEXT STEPS (Recommended based on progress) │
│ ─────────────────────────────────────────────────── │
│                                                     │
│ 🧩 CONNECT THE DOTS                                │
│ You learned about Microservices. Now learn how     │
│ they are deployed in the real world.               │
│                                                     │
│ [System Objective] 🔲 What is a container?         │
│    🤖 Knowledge Graph: microservices → complements │
│    ⏱️ Est: 10 min read • 🏆 5 XP                   │
│    📚 Relationship: is_foundation_for → Docker     │
│                                                     │
│ [System Objective] 🔲 Install Docker on your machine│
│    🤖 Knowledge Graph: containers → is_applied_as  │
│    ⏱️ Est: 15 min setup • 🏆 10 XP                 │
│    📚 Relationship: is_applied_as → containerization│
│                                                     │
│ ─────────────────────────────────────────────────── │
│                                                     │
│ 🧭 EXPAND YOUR HORIZONS                            │
│ You've mastered SQL. See what other database       │
│ types are used in modern applications.             │
│                                                     │
│ [System Objective] 🔲 Document vs Key-Value DBs    │
│    🤖 Knowledge Graph: sql → is_alternative_to     │
│    ⏱️ Est: 15 min read • 🏆 8 XP                   │
│    📚 Relationship: sql-databases → nosql-databases│
│                                                     │
│ [System Objective] 🔲 Learn about CAP Theorem      │
│    🤖 Knowledge Graph: databases → is_foundation_for│
│    ⏱️ Est: 20 min read • 🏆 12 XP                  │
│    📚 Relationship: is_foundation_for → distributed│
│                                                     │
│ ─────────────────────────────────────────────────── │
│                                                     │
│ 🚀 LEVEL UP                                        │
│ Apply your sorting knowledge to solve real         │
│ programming challenges.                             │
│                                                     │
│ [System Objective] 🔲 Build a "Hello World" API    │
│    🤖 Knowledge Graph: algorithms → is_applied_as  │
│    ⏱️ Est: 30 min coding • 🏆 15 XP                │
│    📚 Relationship: theory → practical-application │
│                                                     │
│ ─────────────────────────────────────────────────── │
│                                                     │
│ 🎮 KNOWLEDGE GRAPH SYSTEM                          │
│ How these objectives are generated:                 │
│                                                     │
│ 1️⃣ Trigger: You completed "Microservice Architecture"│
│ 2️⃣ Graph Traversal: System finds microservices node│
│ 3️⃣ Rule Application (Priority Order):              │
│    • Priority 1: Find Complements → Containerization│
│    • Priority 2: Find Alternatives → Event-Driven  │
│    • Priority 3: Find Applications → API Building  │
│ 4️⃣ Filtering: Cross-reference with Backend roadmap │
│ 5️⃣ Micro-Generation: Create introductory objectives│
│                                                     │
│ 🔄 Dynamic Updates: New objectives appear as you   │
│ complete current ones, building your knowledge web. │
│                                                     │
│ [🧠 View Full Graph] [⚙️ Customize Priorities]     │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎯 CURRENT FOCUS                                    │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔥 Active: Bubble Sort Implementation (1.5)     │ │
│ │                                                 │ │
│ │ 📝 Task: Create a bubble sort function that:   │ │
│ │ • Takes an array of integers as input          │ │
│ │ • Sorts in ascending order                     │ │
│ │ • Returns the sorted array                     │ │
│ │ • Includes time complexity analysis            │ │
│ │                                                 │ │
│ │ 💡 Hints Available: 3 remaining                │ │
│ │ 🎯 Success Criteria: Code compiles & passes    │ │
│ │    all test cases with correct complexity      │ │
│ │                                                 │ │
│ │ [💻 Open Code Editor] [💡 Get Hint]            │ │
│ │ [📖 Algorithm Guide] [🧪 Run Tests]            │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📋 Also Due: Supplementary Quest Plan (3.2)    │ │
│ │                                                 │ │
│ │ 📊 Task: Bridge curriculum-industry gaps:      │ │
│ │ • Compare CS curriculum vs Backend roadmap.sh  │ │
│ │ • Identify missing industry technologies       │ │
│ │ • Create supplementary learning modules        │ │
│ │ • Align with career specialization goals       │ │
│ │                                                 │ │
│ │ 📈 Progress: Curriculum gap analysis complete  │ │
│ │ 🎯 Next: Generate industry-focused quests      │ │
│ │                                                 │ │
│ │ [📝 Open Planner] [📊 View Roadmap.sh]         │ │
│ │ [🛠️ Industry Tools] [⏰ Set Milestones]        │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📚 ARSENAL INTEGRATION                              │
│                                                     │
│ 🔗 Quest-Linked Study Materials:                   │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📋 Array Sorting Algorithms                     │ │
│ │ ⭐ Favorited • 🟡 algorithms                    │ │
│ │ Last updated: 2 hours ago                       │ │
│ │ [👁️ View] [✏️ Edit] [🔗 Unlink]                │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📊 Time Complexity Cheat Sheet                 │ │
│ │ 🟢 exam-prep • 🟡 algorithms                    │ │
│ │ Last updated: 1 day ago                         │ │
│ │ [👁️ View] [✏️ Edit] [🔗 Unlink]                │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ 🤖 AI Smart Suggestions:                           │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📝 C Programming Best Practices                 │ │
│ │ Relevance: 85% • PRF192                         │ │
│ │ "Contains coding standards for arrays"          │ │
│ │ [🔗 Link to Quest] [👁️ Preview]                │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🧪 Lab 3: Sorting Implementation                │ │
│ │ Relevance: 92% • PRF192                         │ │
│ │ "Step-by-step sorting lab solution"            │ │
│ │ [🔗 Link to Quest] [👁️ Preview]                │ │
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
│ │ Template: [📋 AI Gap Analysis Report ▼]        │ │
│ │ • Curriculum vs Industry Gap Template          │ │
│ │ • Technology Stack Comparison                   │ │
│ │ • Supplementary Quest Planning                  │ │
│ │ • Career Roadmap Alignment                      │ │
│ │                                                 │ │
│ │ 🤖 AI-Generated Objective Examples:            │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ "Master Docker Containerization"            │ │ │
│ │ │ 📚 Bridge: CS Theory → Industry Practice   │ │ │
│ │ │ 🎯 Skills: Container orchestration, DevOps │ │ │
│ │ │ 📈 XP: 20 • ⏱️ Est: 3 hours               │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ "Build REST API with Express.js"            │ │ │
│ │ │ 📚 Bridge: Database Theory → Web APIs      │ │ │
│ │ │ 🎯 Skills: Node.js, HTTP protocols, JSON   │ │ │
│ │ │ 📈 XP: 25 • ⏱️ Est: 4 hours               │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │                                                 │ │
│ │ Gap Analysis Criteria:                          │ │
│ │ 🔧 Missing: Docker, Kubernetes, AWS            │ │
│ │ 📱 Missing: React, Vue.js, TypeScript          │ │
│ │ 🔄 Missing: CI/CD, Git workflows, Testing      │ │
│ │ 📊 Missing: Agile, Scrum, Project management   │ │
│ │                                                 │ │
│ │ Auto-tags: 🎯 gap-analysis, 🤖 ai-generated    │ │
│ │ Auto-link: ✅ Link to roadmap.sh specialization │ │
│ │                                                 │ │
│ │ [🤖 Generate More] [📝 Create Report] [❌ Cancel] │ │
│ └─────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────┘
```

---

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Quest Log               🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🎯 ACTIVE QUESTS                                    │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔥 URGENT: Final Exam Preparation               │ │
│ │ 📅 Due: Dec 15, 2024 (3 days left)             │ │
│ │ Progress: ████████░░ 80%                        │ │
│ │                                                 │ │
│ │ Objectives:                                     │ │
│ │ ✅ Complete practice tests (5/5)                │ │
│ │ ✅ Review lecture notes                         │ │
│ │ ⏳ Create summary sheets (2/3)                  │ │
│ │ ⏳ Form study group                             │ │
│ │                                                 │ │
│ │ Rewards: 500 XP, "Exam Master" badge           │ │
│ │ [📋 View Details] [⚡ Continue]                 │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 💻 Code Challenge: Binary Search Trees          │ │
│ │ 📅 Due: Dec 20, 2024 (8 days left)             │ │
│ │ Progress: ████░░░░░░ 40%                        │ │
│ │                                                 │ │
│ │ Objectives:                                     │ │
│ │ ✅ Understand BST concepts                      │ │
│ │ ⏳ Implement insertion method                   │ │
│ │ ⏳ Implement search method                      │ │
│ │ ⏳ Handle edge cases                            │ │
│ │                                                 │ │
│ │ Rewards: 300 XP, unlock "Tree Master" quest    │ │
│ │ [📋 View Details] [⚡ Continue]                 │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📚 AVAILABLE QUESTS                                 │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🧮 Mathematics Mastery: Calculus Fundamentals   │ │
│ │ 📊 Difficulty: ⭐⭐⭐⭐☆                        │ │
│ │ ⏱️ Estimated: 2 weeks                           │ │
│ │ 🎁 Rewards: 400 XP, "Math Wizard" badge        │ │
│ │ [🎯 Accept Quest] [ℹ️ More Info]                │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🌐 Web Development: React Hooks Deep Dive       │ │
│ │ 📊 Difficulty: ⭐⭐⭐☆☆                        │ │
│ │ ⏱️ Estimated: 1 week                            │ │
│ │ 🎁 Rewards: 250 XP, unlock "Frontend Master"   │ │
│ │ [🎯 Accept Quest] [ℹ️ More Info]                │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🏆 COMPLETED QUESTS                                 │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ ✅ Array Algorithms Mastery                     │ │
│ │ 📅 Completed: Dec 8, 2024                       │ │
│ │ 🎁 Earned: 350 XP, "Array Master" badge        │ │
│ │ [📊 View Results] [🔄 Retry for Better Score]   │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [🔍 Browse All Quests] [📊 Quest Statistics]       │
└─────────────────────────────────────────────────────┘
```

---

## AI-Driven Questline System

Note: This AI-focused section is now maintained in a dedicated document for clarity.

➡️ View the full details in [AI Quest Wireframes](./ai-quest-wireframes.md#ai-driven-questline-system).

### Quest Chapter Overview

Content moved to the dedicated AI document.

### AI Quest Generation Interface

Content moved to the dedicated AI document.

---

## Adaptive Quest Logic (Student Types)

Content moved to the dedicated AI document.

---

## Manual Assignments

### ADD ASSIGNMENT TO QUEST SYSTEM

Content moved to the dedicated AI document.

---

## AI Quest Matching

Note: This matching system has moved to the dedicated AI wireframes.

➡️ See [AI Quest Matching](./ai-quest-wireframes.md#ai-quest-matching).

Content moved to the dedicated AI document.

---

## Related Files

- [Main Wireframes](./low-fidelity-wireframes.md) - Contains non-quest related wireframes
- [User Flows](./user-flows-wireframes.md) - Quest-related user flow diagrams
- [Screen Specifications](./screen-specifications.md) - Detailed quest screen specs

---

*This document is part of the RogueLearn front-end specification suite. For questions or updates, please refer to the main documentation index.*


---

## Quest Objective Preview

```
┌─────────────────────────────────────────────────────┐
│ ☰ Quest Preview > Binary Search Trees  🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🌳 BINARY SEARCH TREE MASTERY                       │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📋 QUEST OVERVIEW                                   │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Learning Objectives:                         │ │
│ │                                                 │ │
│ │ By completing this quest, you will:             │ │
│ │ ✅ Understand BST properties and structure      │ │
│ │ ✅ Implement insertion and search operations    │ │
│ │ ✅ Master tree traversal algorithms             │ │
│ │ ✅ Handle edge cases and error conditions       │ │
│ │ ✅ Analyze time and space complexity            │ │
│ │                                                 │ │
│ │ 📊 Difficulty: ⭐⭐⭐⭐☆ (Advanced)            │ │
│ │ ⏱️ Estimated Time: 4-6 hours                    │ │
│ │ 🎓 Prerequisites: Arrays, Recursion, Pointers  │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎯 DETAILED OBJECTIVES                              │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Phase 1: Foundation (25% - 100 XP)              │ │
│ │                                                 │ │
│ │ 🎯 Objective 1.1: BST Theory                    │ │
│ │ • Understand BST properties                     │ │
│ │ • Compare with other tree structures            │ │
│ │ • Identify use cases and applications           │ │
│ │ Success Criteria: Pass theory quiz (80%+)       │ │
│ │                                                 │ │
│ │ 🎯 Objective 1.2: Node Structure Design         │ │
│ │ • Define TreeNode class/structure               │ │
│ │ • Implement proper data encapsulation          │ │
│ │ • Add necessary helper methods                  │ │
│ │ Success Criteria: Code review approval          │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Phase 2: Core Operations (40% - 160 XP)         │ │
│ │                                                 │ │
│ │ 🎯 Objective 2.1: Insertion Algorithm           │ │
│ │ • Implement recursive insertion                 │ │
│ │ • Handle duplicate values                       │ │
│ │ • Maintain BST properties                       │ │
│ │ Success Criteria: Pass 15/15 test cases        │ │
│ │                                                 │ │
│ │ 🎯 Objective 2.2: Search Operations             │ │
│ │ • Implement efficient search                    │ │
│ │ • Add contains() method                         │ │
│ │ • Handle not-found cases gracefully             │ │
│ │ Success Criteria: O(log n) average complexity   │ │
│ │                                                 │ │
│ │ 🎯 Objective 2.3: Tree Traversals               │ │
│ │ • Implement inorder traversal                   │ │
│ │ • Add preorder and postorder                    │ │
│ │ • Support both recursive and iterative          │ │
│ │ Success Criteria: Correct output for all types  │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Phase 3: Advanced Features (25% - 100 XP)       │ │
│ │                                                 │ │
│ │ 🎯 Objective 3.1: Deletion Algorithm            │ │
│ │ • Handle all three deletion cases               │ │
│ │ • Maintain tree structure integrity             │ │
│ │ • Implement successor/predecessor finding       │ │
│ │ Success Criteria: Complex deletion scenarios    │ │
│ │                                                 │ │
│ │ 🎯 Objective 3.2: Tree Statistics               │ │
│ │ • Calculate tree height                         │ │
│ │ • Count total nodes                             │ │
│ │ • Find minimum and maximum values               │ │
│ │ Success Criteria: Efficient implementations     │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Phase 4: Testing & Analysis (10% - 40 XP)       │ │
│ │                                                 │ │
│ │ 🎯 Objective 4.1: Comprehensive Testing         │ │
│ │ • Write unit tests for all methods              │ │
│ │ • Test edge cases and error conditions          │ │
│ │ • Achieve 95%+ code coverage                    │ │
│ │ Success Criteria: All tests pass                │ │
│ │                                                 │ │
│ │ 🎯 Objective 4.2: Performance Analysis          │ │
│ │ • Analyze time complexity of operations         │ │
│ │ • Compare with array-based alternatives         │ │
│ │ • Document space complexity                     │ │
│ │ Success Criteria: Accurate complexity analysis  │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎁 REWARDS & ACHIEVEMENTS                           │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🏆 Completion Rewards:                          │ │
│ │ • 400 XP (Base reward)                          │ │
│ │ • "Tree Master" Achievement Badge               │ │
│ │ • Unlock: Advanced Tree Algorithms questline   │ │
│ │ • Unlock: Graph Theory fundamentals             │ │
│ │                                                 │ │
│ │ 🌟 Bonus Achievements:                          │ │
│ │ • "Speed Demon" - Complete in <4 hours (+50 XP) │ │
│ │ • "Perfectionist" - 100% test coverage (+25 XP) │ │
│ │ • "Code Artist" - Clean, documented code (+25 XP) │ │
│ │ • "Helper" - Assist 3+ classmates (+30 XP)     │ │
│ │                                                 │ │
│ │ 🔓 Unlocked Content:                            │ │
│ │ • AVL Trees and Balancing                       │ │
│ │ • Red-Black Trees                               │ │
│ │ • B-Trees and Database Indexing                 │ │
│ │ • Tree-based Problem Solving Patterns          │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [🎯 Accept Quest] [📚 View Prerequisites] [❌ Back] │
└─────────────────────────────────────────────────────┘
```

---

## Quest Memory System & Continuity

The quest system maintains comprehensive learning continuity across semesters and adapts to individual learning patterns.

### Semester Progression Tracking

```
┌─────────────────────────────────────────────────────────────────┐
│ 📊 LEARNING JOURNEY TIMELINE                                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ Fall 2023    Spring 2024    Summer 2024    Fall 2024          │
│ ────●────────────●──────────────●──────────────●──────         │
│     │            │              │              │               │
│ Foundations  OOP Mastery   Data Structures  Algorithms         │
│ 15 quests    12 quests     8 quests        6 quests           │
│ completed    completed     completed       in progress         │
│                                                                 │
│ 🎯 Current Focus: Advanced Sorting & Search Algorithms         │
│ 📈 Skill Growth: +45 levels since start                       │
│ 🏆 Achievements: 12 badges earned                             │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│ SEMESTER TRANSITIONS                                            │
│ • Automatic quest archiving and progress preservation          │
│ • Skill level carryover with decay modeling                   │
│ • Prerequisite validation for advanced courses                 │
│ • Learning pattern analysis for course recommendations         │
└─────────────────────────────────────────────────────────────────┘
```

### Learning Pattern Analysis

```
┌─────────────────────────────────────────────────────────────────┐
│ 🧠 LEARNING ANALYTICS DASHBOARD                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ Your Learning Profile:                                          │
│ • Visual Learner (85% confidence)                             │
│ • Prefers step-by-step breakdowns                             │
│ • Best performance: Morning sessions (9-11 AM)                │
│ • Average quest completion: 3.2 sessions                      │
│                                                                 │
│ 📊 Performance Patterns:                                       │
│ ┌─────────────────────────────────────────────────────────────┐ │
│ │ Quest Success Rate by Type:                                 │ │
│ │ ████████████████████████████████████████ Coding (92%)      │ │
│ │ ████████████████████████████████ Theory (78%)              │ │
│ │ ████████████████████████████████████ Problem Solving (85%) │ │
│ │ ████████████████████████ Group Projects (65%)              │ │
│ └─────────────────────────────────────────────────────────────┘ │
│                                                                 │
│ 🎯 Optimization Suggestions:                                   │
│ • Schedule theory quests after coding practice                 │
│ • Use visual aids for complex algorithms                       │
│ • Join study groups for collaborative projects                 │
└─────────────────────────────────────────────────────────────────┘
```

### Adaptive Review System

```
┌─────────────────────────────────────────────────────────────────┐
│ 🔄 SPACED REPETITION & KNOWLEDGE RETENTION                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ 📅 Review Schedule (Next 7 Days):                              │
│                                                                 │
│ Today:     🔴 Bubble Sort (Critical - 30 days since review)    │
│ Tomorrow:  🟡 Binary Search (Due - 14 days since review)       │
│ Wed:       🟢 Recursion Basics (Scheduled review)              │
│ Thu:       🟡 OOP Principles (Due - 21 days since review)      │
│ Fri:       🟢 Data Types (Scheduled review)                    │
│                                                                 │
│ 🧠 Knowledge Decay Modeling:                                   │
│ • Bubble Sort: 45% retention (needs immediate review)          │
│ • Binary Search: 72% retention (review recommended)            │
│ • Recursion: 88% retention (well retained)                     │
│                                                                 │
│ 🎯 Smart Review Quests:                                        │
│ • "Quick Sort Challenge" (reinforces bubble sort concepts)     │
│ • "Search Algorithm Comparison" (strengthens binary search)    │
│ • "Recursive Problem Set" (maintains recursion skills)         │
└─────────────────────────────────────────────────────────────────┘
```

## Mission Control Dashboard (Local IDE Projects)

For complex university projects that require a local IDE (Visual Studio, IntelliJ, VS Code), RogueLearn provides a **Mission Control Dashboard** - a persistent, full-screen workspace that keeps users anchored to the platform while working in their local development environment.

### When Mission Control Activates:
- **Local Project** objectives (coding assignments, software projects)
- **IDE Assignment** objectives (complex development tasks)
- **Note-Taking** objectives (knowledge documentation requirements)

### Mission Control Layout:

```
┌────────────────────────────────────────────────────────────────────────────┐
│ 🚀 Mission: PRN212 Assignment 1 - Web API Project                          │
├──────────────────────────────────────┬─────────────────────────────────────┤
│                                      │                                     │
│   🎯 OBJECTIVE TRACKER               │   📚 ARSENAL - CONTEXT NOTES          │
│   --------------------------------   │   ----------------------------      │
│                                      │                                     │
│   [Status: In Progress ⏳]           │   [+] Create New Note               │
│                                      │                                     │
│   THE TASK:                          │   Your Notes on "API Controllers":  │
│   Build a 3-endpoint Web API         │    - A controller handles HTTP      │
│   following the requirements in      │      requests.                      │
│   the provided PDF.                  │    - Use [HttpGet], [HttpPost]...   │
│                                      │                                     │
│   RESOURCES:                         │   Your Notes on "Dependency Inj...":│
│   - [🔗 Assignment1.pdf]             │    - Services are registered in     │
│   - [🔗 Microsoft Docs: Controllers] │      Program.cs.                    │
│                                      │                                     │
│   REWARDS:                           │   [Search all notes...]             │
│   - +250 XP                          │                                     │
│   - +50 in `ASP.NET Core`            │                                     │
│                                      │                                     │
├──────────────────────────────────────┴─────────────────────────────────────┤
│                                                                            │
│   ✅ SUBMISSION & VERIFICATION                                             │
│   --------------------------------                                         │
│   Once your assignment is complete and pushed, submit it for verification. │
│                                                                            │
│   [Link to your GitHub Repository for this assignment] [__________________] │
│                                                                            │
│   (Optional) Or upload your project zip file: [Browse...]                  │
│                                                                            │
│   ┌────────────────────────┐                                               │
│   │  SUBMIT FOR COMPLETION │                                               │
│   └────────────────────────┘                                               │
│                                                                            │
└────────────────────────────────────────────────────────────────────────────┘
```

### Mission Control Features:

#### 🎯 Objective Tracker (Left Panel)
- **Persistent Goal Visibility**: Task description, requirements, and rewards always visible
- **Resource Integration**: Direct links to assignment PDFs, documentation, tutorials
- **Progress Indicators**: Real-time status updates and completion tracking
- **Reward Preview**: XP and skill points to be earned upon completion

#### 📚 Arsenal - Context Notes (Right Panel)
- **Personal Knowledge Base**: Create, edit, and organize notes while coding
- **Contextual Note-Taking**: Add insights, code snippets, and learning points
- **Search Functionality**: Quickly find relevant notes across all subjects
- **Knowledge Retention**: Notes become part of permanent learning arsenal

#### ✅ Submission & Verification (Bottom Panel)
- **Smart Submission**: GitHub repository links or file uploads
- **AI-Powered Verification**: Automated project structure and code analysis
- **Intelligent Feedback**: Specific guidance when verification fails
- **Completion Automation**: Automatic objective completion when verification passes

### AI Verification Process:

```
┌─────────────────────────────────────────────────────────────────┐
│ 🤖 VERIFICATION IN PROGRESS...                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ ✅ Repository cloned successfully                               │
│ ✅ Project structure validated                                  │
│ ✅ Required files found: Controllers/, Models/, Program.cs      │
│ ✅ Code analysis: API endpoints detected (3/3)                  │
│ ✅ Dependency injection implementation found                    │
│ ⏳ Running basic functionality tests...                         │
│                                                                 │
│ 📊 Verification Score: 85/100                                  │
│                                                                 │
│ ⚠️  Minor Issues Found:                                         │
│ • Missing XML documentation on ProductsController              │
│ • Consider adding error handling in GetProduct method          │
│                                                                 │
│ [✅ Accept Completion] [🔄 Fix Issues First]                    │
└─────────────────────────────────────────────────────────────────┘
```

### Note Verification System

For knowledge objectives requiring note creation, Mission Control includes intelligent note verification:

```
┌─────────────────────────────────────────────────────────────────┐
│ 📝 NOTE VERIFICATION: "Understanding MVC Pattern"              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ 🎯 Objective: Create comprehensive notes on MVC architecture    │
│                                                                 │
│ ✅ Content Analysis:                                            │
│ • Key concepts covered: Model, View, Controller (3/3)          │
│ • Examples provided: 2 code snippets found                     │
│ • Depth score: 78/100 (Good understanding demonstrated)        │
│ • Personal insights: 3 reflection points identified            │
│                                                                 │
│ 📊 Note Quality Metrics:                                       │
│ • Completeness: ████████░░ 80%                                 │
│ • Clarity: ██████████ 100%                                     │
│ • Examples: ██████░░░░ 60%                                      │
│ • Personal Connection: ████████░░ 80%                          │
│                                                                 │
│ 💡 Suggestions for Improvement:                                │
│ • Add one more practical example                               │
│ • Include a diagram or visual representation                   │
│                                                                 │
│ [✅ Accept Notes] [📝 Improve First] [💡 Get Suggestions]       │
└─────────────────────────────────────────────────────────────────┘
```

### Mission Control Benefits:

1. **Ecosystem Retention**: Users never feel like they've left RogueLearn
2. **Contextual Learning**: Notes and resources available during development
3. **Automated Verification**: Reduces manual grading while ensuring quality
4. **Persistent Motivation**: Rewards and progress always visible
5. **Knowledge Integration**: Learning becomes part of the development workflow

### Technical Implementation Notes:

- **Responsive Design**: Optimized for second monitor or split-screen usage
- **Real-time Sync**: Notes and progress sync across devices
- **Offline Capability**: Core functionality available without internet
- **IDE Integration**: Optional browser extension for deeper IDE integration
- **Security**: Secure handling of repository access and file uploads

---

*Note: For AI-driven quest generation and matching features, see [AI Quest Wireframes](./ai-quest-wireframes.md).*