# Quest System Wireframes

This document contains all wireframes and interface designs related to the quest system functionality in RogueLearn.

## Table of Contents

1. [Quest Log Interface](#quest-log-interface)
2. [AI-Driven Questline System](#ai-driven-questline-system)
3. [Adaptive Quest Logic](#adaptive-quest-logic-student-types)
4. [Manual Assignment Integration](#manual-assignments)
5. [AI Quest Matching](#ai-quest-matching)
6. [Quest Objective Preview](#quest-objective-preview)
7. [Quest Memory System & Continuity](#quest-memory-system--continuity)
8. [Arsenal Quest Integration](#arsenal-quest-integration)
9. [Quest Integration & Smart Suggestions](#quest-integration--smart-suggestions)
10. [Boss Fight Arena](#boss-fight-arena)

---

## Quest Log Interface

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

### Quest Chapter Overview

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Quest Designer          🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🎯 QUESTLINE: Data Structures & Algorithms          │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📖 CHAPTER PROGRESSION                              │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Chapter 1: Foundations ✅                       │ │
│ │ ├─ Quest 1.1: Array Basics ✅                   │ │
│ │ ├─ Quest 1.2: String Manipulation ✅            │ │
│ │ └─ Quest 1.3: Basic Sorting ✅                  │ │
│ │ Completion: 100% • XP Earned: 450               │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Chapter 2: Linear Data Structures 🔄           │ │
│ │ ├─ Quest 2.1: Linked Lists ✅                   │ │
│ │ ├─ Quest 2.2: Stacks & Queues ⏳               │ │
│ │ └─ Quest 2.3: Dynamic Arrays ⏳                │ │
│ │ Completion: 33% • XP Progress: 150/450          │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Chapter 3: Trees & Graphs 🔒                   │ │
│ │ ├─ Quest 3.1: Binary Trees 🔒                  │ │
│ │ ├─ Quest 3.2: Graph Traversal 🔒               │ │
│ │ └─ Quest 3.3: Advanced Trees 🔒                │ │
│ │ Unlock Requirement: Complete Chapter 2          │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🤖 AI QUEST GENERATION                              │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🧠 Intelligent Quest Creation                   │ │
│ │                                                 │ │
│ │ Based on your progress and learning patterns:   │ │
│ │                                                 │ │
│ │ 📊 Strengths Identified:                        │ │
│ │ • Array manipulation (95% accuracy)            │ │
│ │ • Basic algorithms (88% accuracy)              │ │
│ │                                                 │ │
│ │ 🎯 Areas for Improvement:                       │ │
│ │ • Recursion concepts (65% accuracy)            │ │
│ │ • Time complexity analysis (70% accuracy)      │ │
│ │                                                 │ │
│ │ 💡 Recommended Next Quest:                      │ │
│ │ "Recursive Problem Solving"                     │ │
│ │ • Difficulty: Adaptive (Medium → Hard)         │ │
│ │ • Focus: Recursion + Complexity Analysis       │ │
│ │ • Estimated Time: 3-5 days                     │ │
│ │                                                 │ │
│ │ [🎯 Generate Quest] [⚙️ Customize Parameters]   │ │
│ └─────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────┘
```

### AI Quest Generation Interface

```
┌─────────────────────────────────────────────────────┐
│ ☰ Quest Generator > Custom Quest       🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🤖 AI-POWERED QUEST CREATION                        │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📋 QUEST PARAMETERS                                 │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Learning Objectives                          │ │
│ │ [Understand binary search algorithms     ]      │ │
│ │                                                 │ │
│ │ 📚 Subject Area                                 │ │
│ │ [Data Structures & Algorithms ▼]               │ │
│ │                                                 │ │
│ │ 📊 Difficulty Level                             │ │
│ │ ○ Beginner  ◉ Intermediate  ○ Advanced         │ │
│ │                                                 │ │
│ │ ⏱️ Time Commitment                              │ │
│ │ [2-3 hours ▼]                                   │ │
│ │                                                 │ │
│ │ 🎮 Quest Type                                   │ │
│ │ ☑️ Coding Challenge                             │ │
│ │ ☑️ Theory Questions                             │ │
│ │ ☐ Project-based                                │ │
│ │ ☐ Peer Collaboration                           │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🧠 AI ANALYSIS & SUGGESTIONS                        │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📊 Your Learning Profile Analysis:              │ │
│ │                                                 │ │
│ │ • Strong in: Arrays, Basic Sorting             │ │
│ │ • Improving: Recursion, Tree Structures        │ │
│ │ • Needs Work: Graph Algorithms, DP             │ │
│ │                                                 │ │
│ │ 💡 AI Recommendations:                          │ │
│ │ "Based on your binary search interest and       │ │
│ │ current skill level, I suggest including:      │ │
│ │                                                 │ │
│ │ • Iterative vs Recursive implementations       │ │
│ │ • Edge case handling                           │ │
│ │ • Time complexity analysis                     │ │
│ │ • Real-world applications                      │ │
│ │                                                 │ │
│ │ Estimated completion time: 2.5 hours"          │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎯 GENERATED QUEST PREVIEW                          │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔍 "Binary Search Mastery Challenge"            │ │
│ │                                                 │ │
│ │ 📋 Quest Objectives:                            │ │
│ │ 1. Implement iterative binary search           │ │
│ │ 2. Implement recursive binary search           │ │
│ │ 3. Handle edge cases (empty array, not found)  │ │
│ │ 4. Analyze time/space complexity               │ │
│ │ 5. Apply to real-world search problem          │ │
│ │                                                 │ │
│ │ 🎁 Rewards:                                     │ │
│ │ • 200 XP                                        │ │
│ │ • "Search Algorithm Expert" badge              │ │
│ │ • Unlock: Advanced Search Algorithms           │ │
│ │                                                 │ │
│ │ 📊 Success Criteria:                            │ │
│ │ • All test cases pass (Required)               │ │
│ │ • Code efficiency score ≥ 80%                  │ │
│ │ • Complexity analysis accuracy ≥ 90%           │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [🎯 Create Quest] [🔄 Regenerate] [⚙️ Fine-tune]    │
└─────────────────────────────────────────────────────┘
```

---

## Adaptive Quest Logic (Student Types)

```
┌─────────────────────────────────────────────────────┐
│ ☰ Quest System > Adaptive Logic        🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🧠 STUDENT TYPE ANALYSIS                            │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 👤 CURRENT STUDENT PROFILE                          │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Alex Chen - Learning Profile                 │ │
│ │                                                 │ │
│ │ 📊 Dominant Learning Style: Visual Learner      │ │
│ │ ├─ Prefers: Diagrams, flowcharts, visual aids  │ │
│ │ ├─ Strength: Pattern recognition                │ │
│ │ └─ Challenge: Abstract concepts without visuals │ │
│ │                                                 │ │
│ │ 🎮 Engagement Pattern: Achievement-Oriented      │ │
│ │ ├─ Motivated by: Badges, leaderboards, XP      │ │
│ │ ├─ Prefers: Clear goals, measurable progress   │ │
│ │ └─ Responds to: Competitive elements            │ │
│ │                                                 │ │
│ │ ⏱️ Study Behavior: Consistent Short Sessions    │ │
│ │ ├─ Optimal: 25-45 minute sessions              │ │
│ │ ├─ Peak times: 2-4 PM, 7-9 PM                 │ │
│ │ └─ Completion rate: 87% for <1 hour quests     │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎯 ADAPTIVE QUEST CUSTOMIZATION                     │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔄 Current Quest: "Binary Search Trees"         │ │
│ │                                                 │ │
│ │ 📊 Adaptations Applied:                         │ │
│ │                                                 │ │
│ │ 🎨 Visual Learning Enhancements:                │ │
│ │ ✅ Interactive tree visualization               │ │
│ │ ✅ Step-by-step animation for insertions       │ │
│ │ ✅ Color-coded node relationships               │ │
│ │ ✅ Flowchart for algorithm logic                │ │
│ │                                                 │ │
│ │ 🏆 Achievement-Oriented Elements:                │ │
│ │ ✅ Progress bar for each sub-objective          │ │
│ │ ✅ Immediate feedback on code submissions       │ │
│ │ ✅ Bonus XP for optimal solutions               │ │
│ │ ✅ "Tree Master" badge milestone at 80%        │ │
│ │                                                 │ │
│ │ ⏱️ Session Structure Optimization:              │ │
│ │ ✅ Quest broken into 4 × 30-minute segments    │ │
│ │ ✅ Natural break points with save functionality │ │
│ │ ✅ Optional extended sessions for flow state    │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🔬 LEARNING ANALYTICS                               │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📈 Performance Trends                           │ │
│ │                                                 │ │
│ │ 📊 Recent Quest Completion Rates:               │ │
│ │ • Visual-heavy quests: 92% completion          │ │
│ │ • Text-only quests: 73% completion             │ │
│ │ • Collaborative quests: 85% completion         │ │
│ │                                                 │ │
│ │ ⏱️ Engagement Patterns:                         │ │
│ │ • Average session: 38 minutes                  │ │
│ │ • Preferred difficulty: Intermediate           │ │
│ │ • Drop-off point: After 1 hour                 │ │
│ │                                                 │ │
│ │ 🎯 Recommendation Engine Output:                │ │
│ │ "Increase visual elements by 25%, add more     │ │
│ │ intermediate checkpoints, consider gamification │ │
│ │ elements for motivation maintenance."           │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [📊 View Full Analytics] [⚙️ Adjust Preferences]    │
└─────────────────────────────────────────────────────┘
```

---

## Manual Assignments

### ADD ASSIGNMENT TO QUEST SYSTEM

```
┌─────────────────────────────────────────────────────┐
│ ☰ Instructor Panel > Quest Creator     🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 📝 CONVERT ASSIGNMENT TO QUEST                      │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📋 ASSIGNMENT DETAILS                               │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Assignment Title:                               │ │
│ │ [Data Structure Implementation Project]         │ │
│ │                                                 │ │
│ │ Course: [PRF192 - Programming Fundamentals ▼]   │ │
│ │ Due Date: [December 20, 2024]                   │ │
│ │ Points: [100 points]                            │ │
│ │                                                 │ │
│ │ Description:                                    │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ Students will implement a complete binary   │ │ │
│ │ │ search tree with insertion, deletion,      │ │ │
│ │ │ search, and traversal methods. The         │ │ │
│ │ │ implementation should handle edge cases     │ │ │
│ │ │ and include comprehensive testing.          │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎮 GAMIFICATION SETTINGS                            │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Quest Transformation                         │ │
│ │                                                 │ │
│ │ Quest Name:                                     │ │
│ │ [🌳 Tree Master Challenge]                      │ │
│ │                                                 │ │
│ │ Difficulty Level:                               │ │
│ │ ○ Novice  ○ Apprentice  ◉ Expert  ○ Master     │ │
│ │                                                 │ │
│ │ XP Reward: [300 XP] (Auto-calculated)          │ │
│ │                                                 │ │
│ │ Achievement Badges:                             │ │
│ │ ☑️ "Tree Architect" - Complete implementation  │ │
│ │ ☑️ "Bug Hunter" - Pass all test cases          │ │
│ │ ☑️ "Code Craftsman" - Clean, documented code   │ │
│ │ ☐ "Speed Demon" - Submit early (bonus)         │ │
│ │                                                 │ │
│ │ Quest Type:                                     │ │
│ │ ◉ Individual Challenge                          │ │
│ │ ○ Team Quest (2-4 players)                     │ │
│ │ ○ Guild Competition                             │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📊 QUEST STRUCTURE                                  │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Auto-Generated Sub-Objectives:               │ │
│ │                                                 │ │
│ │ ✅ Phase 1: Design & Planning (20 XP)           │ │
│ │ ├─ Create UML class diagram                     │ │
│ │ ├─ Define method signatures                     │ │
│ │ └─ Plan test cases                              │ │
│ │                                                 │ │
│ │ ⏳ Phase 2: Core Implementation (100 XP)        │ │
│ │ ├─ Implement Node structure                     │ │
│ │ ├─ Create insertion method                      │ │
│ │ ├─ Create search method                         │ │
│ │ └─ Implement traversal methods                  │ │
│ │                                                 │ │
│ │ ⏳ Phase 3: Advanced Features (80 XP)           │ │
│ │ ├─ Implement deletion method                    │ │
│ │ ├─ Handle edge cases                            │ │
│ │ └─ Add tree balancing (bonus)                   │ │
│ │                                                 │ │
│ │ ⏳ Phase 4: Testing & Documentation (100 XP)    │ │
│ │ ├─ Write comprehensive tests                    │ │
│ │ ├─ Add code documentation                       │ │
│ │ └─ Performance analysis                         │ │
│ │                                                 │ │
│ │ [✏️ Edit Phases] [🔄 Auto-Generate More]        │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎁 REWARD SYSTEM                                    │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🏆 Completion Rewards:                          │ │
│ │ • Base XP: 300                                  │ │
│ │ • Grade Points: 100 (maps to assignment grade) │ │
│ │ • Achievement Badges: Up to 4 badges           │ │
│ │                                                 │ │
│ │ 🌟 Bonus Opportunities:                         │ │
│ │ • Early Submission: +50 XP                     │ │
│ │ • Perfect Test Score: +25 XP                   │ │
│ │ • Code Quality Excellence: +25 XP              │ │
│ │ • Help Classmates: +10 XP per help             │ │
│ │                                                 │ │
│ │ 🔓 Unlocks:                                     │ │
│ │ • Advanced Data Structures questline           │ │
│ │ • Algorithm Optimization challenges             │ │
│ │ • Peer Code Review opportunities                │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [🎯 Create Quest] [👁️ Preview] [💾 Save Draft]      │
└─────────────────────────────────────────────────────┘
```

---

## AI Quest Matching

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Quest Recommendations   🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🤖 AI QUEST MATCHING SYSTEM                         │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎯 PERSONALIZED QUEST SUGGESTIONS                   │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔥 HIGHLY RECOMMENDED FOR YOU                   │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 🧮 "Advanced Recursion Mastery"             │ │ │
│ │ │ Match Score: 94% 🎯                         │ │ │
│ │ │                                             │ │ │
│ │ │ Why this quest?                             │ │ │
│ │ │ • Builds on your strong array skills       │ │ │
│ │ │ • Addresses recursion knowledge gap        │ │ │
│ │ │ • Matches your preferred difficulty        │ │ │
│ │ │ • Aligns with upcoming exam topics         │ │ │
│ │ │                                             │ │ │
│ │ │ 📊 Difficulty: ⭐⭐⭐⭐☆                   │ │ │
│ │ │ ⏱️ Time: 4-6 hours                          │ │ │


## Boss Fight Arena

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Boss Fight Arena        🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ ⚔️ ALGORITHM BOSS BATTLE                            │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🐉 CURRENT BOSS: The Sorting Dragon                 │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │           🐲                                    │ │
│ │      ╔══════════╗                               │ │
│ │      ║ BOSS HP  ║                               │ │
│ │      ║ ████████░░ 80%                          │ │
│ │      ╚══════════╝                               │ │
│ │                                                 │ │
│ │ 🐉 The Sorting Dragon                           │ │
│ │ Level: 15 • Type: Algorithm Master              │ │
│ │                                                 │ │
│ │ 💪 Boss Abilities:                              │ │
│ │ • Complexity Confusion - Scrambles time analysis │ │
│ │ • Memory Leak - Reduces available space        │ │
│ │ • Edge Case Chaos - Throws unexpected inputs   │ │
│ │ • Performance Pressure - Time limits on solutions │ │
│ │                                                 │ │
│ │ 🛡️ Weaknesses:                                  │ │
│ │ • Optimal algorithms deal 2x damage            │ │
│ │ • Clean code reduces boss attack power         │ │
│ │ • Proper testing prevents critical hits        │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ ⚔️ BATTLE OBJECTIVES                                │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Phase 1: Warm-up Challenges (20% damage)     │ │
│ │                                                 │ │
│ │ ✅ Implement Bubble Sort                        │ │
│ │ ✅ Implement Selection Sort                     │ │
│ │ ✅ Analyze time complexity                      │ │
│ │ Damage Dealt: 20% • XP Earned: 50              │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Phase 2: Intermediate Attacks (40% damage)   │ │
│ │                                                 │ │
│ │ ⏳ Implement Merge Sort                         │ │
│ │ ⏳ Implement Quick Sort                         │ │
│ │ ⏳ Handle edge cases (empty, single element)    │ │
│ │ ⏳ Optimize for best/worst case scenarios       │ │
│ │ Progress: ████░░░░░░ 40%                        │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 Phase 3: Final Strike (40% damage)           │ │
│ │                                                 │ │
│ │ ⏳ Implement Heap Sort                          │ │
│ │ ⏳ Create hybrid sorting algorithm              │ │
│ │ ⏳ Performance benchmarking                     │ │
│ │ ⏳ Memory usage optimization                    │ │
│ │ Status: 🔒 Locked until Phase 2 complete       │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 👥 PARTY STATUS                                     │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🧙‍♂️ Alex Chen (You) - Algorithm Apprentice      │ │
│ │ HP: ████████░░ 80% • MP: ██████░░░░ 60%         │ │
│ │ Equipped: "Debugger Sword", "Logic Shield"     │ │
│ │ Special: Code Review (Heals party)              │ │
│ │                                                 │ │
│ │ 🏹 Sarah Kim - Code Archer                      │ │
│ │ HP: ██████████ 100% • MP: ████████░░ 80%       │ │
│ │ Equipped: "Syntax Bow", "Performance Arrows"   │ │
│ │ Special: Optimization Shot (High damage)        │ │
│ │                                                 │ │
│ │ 🛡️ Mike Johnson - Testing Paladin               │ │
│ │ HP: ████████░░ 80% • MP: ██████████ 100%       │ │
│ │ Equipped: "Unit Test Shield", "Coverage Armor" │ │
│ │ Special: Bug Barrier (Prevents critical hits)   │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎁 VICTORY REWARDS                                  │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🏆 Boss Defeat Rewards:                         │ │
│ │ • 1000 XP (Massive XP boost!)                   │ │
│ │ • "Dragon Slayer" Legendary Badge               │ │
│ │ • "Sorting Master" Title                        │ │
│ │ • Unlock: Advanced Algorithm Bosses             │ │
│ │                                                 │ │
│ │ 💎 Rare Drops (Random):                         │ │
│ │ • "Complexity Crystal" (Reveals all time complexities) │ │
│ │ • "Optimization Orb" (Auto-suggests improvements) │ │
│ │ • "Memory Gem" (Shows space complexity visually) │ │
│ │                                                 │ │
│ │ 🎖️ Performance Bonuses:                         │ │
│ │ • Flawless Victory: +200 XP                     │ │
│ │ • Speed Run (<30 min): +150 XP                  │ │
│ │ • No Hints Used: +100 XP                        │ │
│ │ • Help Teammates: +50 XP per assist             │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [⚔️ Continue Battle] [🛡️ Use Item] [🏃 Retreat]     │
└─────────────────────────────────────────────────────┘
```

---

## Related Files

- [Main Wireframes](./low-fidelity-wireframes.md) - Contains non-quest related wireframes
- [User Flows](./user-flows-wireframes.md) - Quest-related user flow diagrams
- [Screen Specifications](./screen-specifications.md) - Detailed quest screen specs

---

*This document is part of the RogueLearn front-end specification suite. For questions or updates, please refer to the main documentation index.*
│ │ │ 🎁 Reward: 400 XP + "Recursion Master"     │ │ │
│ │ │                                             │ │ │
│ │ │ [🎯 Start Quest] [📋 More Details]          │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 🌐 "React Hooks Deep Dive"                  │ │ │
│ │ │ Match Score: 87% 🎯                         │ │ │
│ │ │                                             │ │ │
│ │ │ Why this quest?                             │ │ │
│ │ │ • Continues your React learning path       │ │ │
│ │ │ • Popular among similar students           │ │ │
│ │ │ • High engagement rate (92%)               │ │ │
│ │ │ • Unlocks advanced frontend quests         │ │ │
│ │ │                                             │ │ │
│ │ │ 📊 Difficulty: ⭐⭐⭐☆☆                   │ │ │
│ │ │ ⏱️ Time: 3-4 hours                          │ │ │
│ │ │ 🎁 Reward: 300 XP + "Hook Master"          │ │ │
│ │ │                                             │ │ │
│ │ │ [🎯 Start Quest] [📋 More Details]          │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📊 MATCHING CRITERIA                                │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🧠 AI Analysis Factors:                         │ │
│ │                                                 │ │
│ │ 📈 Your Performance Data:                       │ │
│ │ • Strong: Arrays (95%), Basic Algorithms (88%) │ │
│ │ • Developing: Recursion (65%), Trees (70%)     │ │
│ │ • Weak: Graph Theory (45%), Dynamic Programming │ │
│ │                                                 │ │
│ │ 🎯 Learning Goals:                              │ │
│ │ • Improve recursion understanding              │ │
│ │ • Prepare for final exams                      │ │
│ │ • Build portfolio projects                     │ │
│ │                                                 │ │
│ │ ⏱️ Time Preferences:                            │ │
│ │ • Preferred session: 30-45 minutes             │ │
│ │ • Available time: 2-3 hours/day                │ │
│ │ • Peak performance: Afternoon sessions         │ │
│ │                                                 │ │
│ │ 🎮 Engagement Style:                            │ │
│ │ • Motivated by: Achievements, progress bars    │ │
│ │ • Prefers: Visual learning, hands-on coding   │ │
│ │ • Responds to: Clear objectives, immediate feedback │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🔍 BROWSE MORE QUESTS                               │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📚 Filter by Category:                          │ │
│ │ [All Categories ▼] [Difficulty ▼] [Time ▼]      │ │
│ │                                                 │ │
│ │ 🏷️ Popular Tags:                                │ │
│ │ [algorithms] [data-structures] [web-dev]        │ │
│ │ [mathematics] [databases] [machine-learning]    │ │
│ │                                                 │ │
│ │ 🔥 Trending This Week:                          │ │
│ │ • "Machine Learning Basics" (+15% popularity)  │ │
│ │ • "System Design Fundamentals" (+12%)          │ │
│ │ • "Advanced SQL Queries" (+8%)                 │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [🔄 Refresh Recommendations] [⚙️ Customize Matching] │
└─────────────────────────────────────────────────────┘
```

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

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Quest Memory System     🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🧠 LEARNING CONTINUITY & MEMORY SYSTEM              │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📅 SEMESTER PROGRESSION TRACKING                    │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎓 Academic Journey: Fall 2024                  │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ Week 1-4: Foundation Building               │ │ │
│ │ │ ✅ Basic Programming Concepts               │ │ │
│ │ │ ✅ Variables and Data Types                 │ │ │
│ │ │ ✅ Control Structures                       │ │ │
│ │ │ Knowledge Retention: 92%                    │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ Week 5-8: Data Structures Introduction     │ │ │
│ │ │ ✅ Arrays and Strings                       │ │ │
│ │ │ ✅ Functions and Recursion                  │ │ │
│ │ │ 🔄 Linked Lists (In Progress)               │ │ │
│ │ │ Knowledge Retention: 85%                    │ │ │
│ │ │ 💡 Needs Review: Pointer concepts           │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ Week 9-12: Advanced Topics (Upcoming)      │ │ │
│ │ │ ⏳ Trees and Graph Structures               │ │ │
│ │ │ ⏳ Algorithm Analysis                       │ │ │
│ │ │ ⏳ Final Project                            │ │ │
│ │ │ Predicted Success Rate: 78%                 │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🔍 LEARNING PATTERN ANALYSIS                        │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📊 Your Learning Patterns                       │ │
│ │                                                 │ │
│ │ 🎯 Strength Areas:                              │ │
│ │ • Visual Learning: 95% effectiveness           │ │
│ │ • Hands-on Coding: 90% retention               │ │
│ │ • Step-by-step Tutorials: 88% completion       │ │
│ │                                                 │ │
│ │ ⚠️ Challenge Areas:                             │ │
│ │ • Abstract Concepts: 65% initial understanding │ │
│ │ • Theory-heavy Content: 70% engagement         │ │
│ │ • Long Sessions: 60% completion rate           │ │
│ │                                                 │ │
│ │ 🔄 Forgetting Curve Analysis:                   │ │
│ │ • Day 1: 100% retention                        │ │
│ │ • Day 7: 75% retention (Above average!)        │ │
│ │ • Day 30: 60% retention                        │ │
│ │ • Day 90: 45% retention                        │ │
│ │                                                 │ │
│ │ 💡 AI Recommendation:                           │ │
│ │ "Schedule review sessions every 2 weeks for    │ │
│ │ optimal retention. Focus on visual aids for    │ │
│ │ abstract concepts."                             │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🔄 ADAPTIVE REVIEW SYSTEM                           │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📚 Scheduled Review Quests                      │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 🔄 "Array Fundamentals Refresher"           │ │ │
│ │ │ Last Completed: 3 weeks ago                 │ │ │
│ │ │ Retention Score: 78% (Declining)            │ │ │
│ │ │ Recommended: This week                      │ │ │
│ │ │ Duration: 15 minutes                        │ │ │
│ │ │ [📚 Start Review] [⏰ Schedule Later]       │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 🔄 "Recursion Concepts Review"              │ │ │
│ │ │ Last Completed: 1 week ago                  │ │ │
│ │ │ Retention Score: 85% (Stable)               │ │ │
│ │ │ Recommended: Next week                      │ │ │
│ │ │ Duration: 20 minutes                        │ │ │
│ │ │ [📚 Start Review] [⏰ Schedule Later]       │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 🔄 "Sorting Algorithms Practice"            │ │ │
│ │ │ Last Completed: 5 days ago                  │ │ │
│ │ │ Retention Score: 92% (Strong)               │ │ │
│ │ │ Recommended: In 2 weeks                     │ │ │
│ │ │ Duration: 25 minutes                        │ │ │
│ │ │ [📚 Start Review] [⏰ Schedule Later]       │ │ │
│ │ └─────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎯 KNOWLEDGE GRAPH VISUALIZATION                    │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🕸️ Your Learning Network                        │ │
│ │                                                 │ │
│ │     [Variables] ──────┐                         │ │
│ │         │             │                         │ │
│ │         ▼             ▼                         │ │
│ │    [Arrays] ────► [Functions]                   │ │
│ │         │             │                         │ │
│ │         ▼             ▼                         │ │
│ │   [Sorting] ◄──── [Recursion]                   │ │
│ │         │             │                         │ │
│ │         ▼             ▼                         │ │
│ │ [Search Alg.] ──► [Trees] ──► [Graphs]          │ │
│ │                                                 │ │
│ │ 🟢 Strong Connection (90%+)                     │ │
│ │ 🟡 Moderate Connection (70-89%)                 │ │
│ │ 🔴 Weak Connection (<70%)                       │ │
│ │                                                 │ │
│ │ 💡 Gap Analysis:                                │ │
│ │ "Strengthen recursion → trees connection        │ │
│ │ through more tree traversal practice"           │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [📊 View Full Analytics] [⚙️ Adjust Review Schedule] │
└─────────────────────────────────────────────────────┘
```

---

## Arsenal Quest Integration

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

---

## Quest Integration & Smart Suggestions

```
┌─────────────────────────────────────────────────────┐
│ ☰ Arsenal > Quest Integration           🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🤖 AI QUEST MATCHING SYSTEM                         │ │
├─────────────────────────────────────────────────────┤
│ 🎯 PERSONALIZED QUEST SUGGESTIONS                   │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔥 HIGHLY RECOMMENDED FOR YOU                   │ │
│ │                                                 │ │
│ │ ┌─────────────────────────────────────────────┐ │ │
│ │ │ 🧮 "Advanced Recursion Mastery"             │ │ │
│ │ │ Match Score: 94% 🎯                         │ │ │
│ │ │                                             │ │ │
│ │ │ Why this quest?                             │ │ │
│ │ │ • Builds on your strong array skills       │ │ │
│ │ │ • Addresses recursion knowledge gap        │ │ │
│ │ │ • Matches your preferred difficulty        │ │ │
│ │ │ • Aligns with upcoming exam topics         │ │ │
│ │ │                                             │ │ │
│ │ │ 📊 Difficulty: ⭐⭐⭐⭐☆                   │ │ │
│ │ │ ⏱️ Time: 4-6 hours                          │ │ │
│ │ │