# Quest Wireframes

## Overview

This document outlines the wireframes for the quest system interface, focusing on the core user experience for quest discovery, tracking, and completion. The quest system integrates with the skill tree and provides structured learning paths for students.

## Quest Log Interface

The main quest dashboard where students can view and manage their learning quests.

## Quest Detail View

Detailed view of an individual quest showing objectives, progress, and related resources with full Arsenal integration.

```
┌─────────────────────────────────────────────────────┐
│ ← Back to Quest Log    🎯 Master Array Algorithms   │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 📚 PRF192 • 🏆 75 XP • ⏱️ Due: Dec 15, 2024       │
│ ████████░░ 80% Complete • 4/5 Objectives           │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📋 QUEST OBJECTIVES                                 │
│                                                     │
│ ✅ 1. Understand array data structure               │
│    💡 Completed: Dec 8 • 🏆 15 XP earned           │
│                                                     │
│ ✅ 2. Learn linear search algorithm                 │
│    💡 Completed: Dec 9 • 🏆 15 XP earned           │
│                                                     │
│ ✅ 3. Implement binary search                       │
│    💡 Completed: Dec 10 • 🏆 20 XP earned          │
│                                                     │
│ ✅ 4. Master selection sort                         │
│    💡 Completed: Dec 12 • 🏆 15 XP earned          │
│                                                     │
│ 🎯 5. Implement bubble sort algorithm               │
│    📍 Current objective • 🏆 10 XP pending          │
│    📝 Write bubble sort in C with time complexity  │
│    ⏱️ Estimated time: 45 minutes                   │
│    [⚡ Start Now] [📚 Study Materials]             │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎯 CURRENT FOCUS                                    │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔥 Implement Bubble Sort                        │ │
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

---

*Note: For AI-driven quest generation and matching features, see [AI Quest Wireframes](./ai-quest-wireframes.md).*