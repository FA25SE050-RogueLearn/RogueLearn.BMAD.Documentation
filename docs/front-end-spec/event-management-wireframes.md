# Event Management System - Low-Fidelity Wireframes

## Overview
This document contains wireframes for the Event Management system in RogueLearn, covering event creation, approval workflows, scheduling, and participant management for Guild Masters and Verified Lecturers.

---

## 1. Event Creation Wizard (Step 1: Basic Information)

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 🎯 Create New Event - Step 1 of 4: Basic Information        [💾 Save Draft] [❌]    │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 📋 EVENT DETAILS                                                                    │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📝 BASIC INFORMATION                                                            │ │
│ │                                                                                 │ │
│ │ Event Title: *                                                                  │ │
│ │ [Winter Programming Challenge 2024_________________________________]            │ │
│ │                                                                                 │ │
│ │ Event Type: *                                                                   │ │
│ │ ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐ │ │
│ │ │ 🏆 Competition  │ │ 📚 Workshop     │ │ 🎓 Lecture      │ │ 🤝 Study Group  │ │ │
│ │ │     [✓]         │ │     [ ]         │ │     [ ]         │ │     [ ]         │ │ │
│ │ └─────────────────┘ └─────────────────┘ └─────────────────┘ └─────────────────┘ │ │
│ │                                                                                 │ │
│ │ Description: *                                                                  │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Join us for an exciting programming challenge featuring algorithm problems   │ │ │
│ │ │ and real-time coding battles. Perfect for students looking to test their    │ │ │
│ │ │ skills and learn from peers. Prizes include XP bonuses, badges, and...      │ │ │
│ │ │                                                                             │ │ │
│ │ │ [Character count: 234/500]                                                  │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ Category: *                                                                     │ │
│ │ [Algorithms & Data Structures ▼]                                               │ │
│ │                                                                                 │ │
│ │ Difficulty Level: *                                                             │ │
│ │ ○ Beginner    ● Intermediate    ○ Advanced    ○ Mixed Levels                   │ │
│ │                                                                                 │ │
│ │ Target Audience:                                                                │ │
│ │ ☑ All Students    ☑ Guild Members    ☐ Specific Courses    ☐ Invite Only      │ │
│ │                                                                                 │ │
│ │ Maximum Participants:                                                           │ │
│ │ [64___] participants (Leave blank for unlimited)                               │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎨 EVENT BRANDING                                                                   │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Event Banner:                                                                   │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │                          📷 Upload Banner                                   │ │ │
│ │ │                     (Recommended: 1200x400px)                               │ │ │
│ │ │                                                                             │ │ │
│ │ │                        [📁 Choose File]                                     │ │ │
│ │ │                     [🎨 Use Template Gallery]                               │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ Event Color Theme:                                                              │ │
│ │ ● Default    ○ Blue    ○ Green    ○ Purple    ○ Orange    ○ Custom             │ │
│ │                                                                                 │ │
│ │ Tags (for discovery):                                                           │ │
│ │ [programming] [algorithms] [competition] [winter2024] [+ Add Tag]               │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 NAVIGATION                                                                       │
│                                                                                     │
│ Progress: ████░░░░░░░░ 25% (Step 1 of 4)                                           │
│                                                                                     │
│ [🔙 Cancel] [💾 Save Draft]                    [➡️ Next: Schedule & Format]        │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 2. Event Creation Wizard (Step 2: Schedule & Format)

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 🎯 Create New Event - Step 2 of 4: Schedule & Format        [💾 Save Draft] [❌]    │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 📅 SCHEDULING                                                                       │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🗓️ EVENT TIMING                                                                 │ │
│ │                                                                                 │ │
│ │ Event Format: *                                                                 │ │
│ │ ● Single Session    ○ Multi-Day Event    ○ Recurring Series                    │ │
│ │                                                                                 │ │
│ │ Start Date & Time: *                                                            │ │
│ │ Date: [2024-12-15 ▼]    Time: [14:00 ▼]    Timezone: [UTC+7 ▼]               │ │
│ │                                                                                 │ │
│ │ Duration: *                                                                     │ │
│ │ [2] hours [30] minutes                                                          │ │
│ │                                                                                 │ │
│ │ Registration Deadline:                                                          │ │
│ │ Date: [2024-12-14 ▼]    Time: [23:59 ▼]                                       │ │
│ │ ☑ Allow late registration (until event starts)                                 │ │
│ │                                                                                 │ │
│ │ 📋 REGISTRATION PHASES                                                          │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Phase 1: Early Bird Registration                                            │ │ │
│ │ │ Start: [2024-12-01 ▼] [00:00 ▼]                                            │ │ │
│ │ │ End:   [2024-12-07 ▼] [23:59 ▼]                                            │ │ │
│ │ │ Benefits: ☑ 50% XP Bonus  ☑ Exclusive Badge  ☐ Priority Seating           │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Phase 2: Regular Registration                                               │ │ │
│ │ │ Start: [2024-12-08 ▼] [00:00 ▼]                                            │ │ │
│ │ │ End:   [2024-12-14 ▼] [23:59 ▼]                                            │ │ │
│ │ │ Benefits: ☑ 25% XP Bonus  ☐ Exclusive Badge  ☐ Priority Seating           │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ [+ Add Registration Phase]                                                      │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🏆 COMPETITION FORMAT (Competition Events Only)                                     │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Tournament Structure: *                                                         │ │
│ │ ● Single Elimination    ○ Double Elimination    ○ Round Robin    ○ Swiss       │ │
│ │                                                                                 │ │
│ │ Battle Format:                                                                  │ │
│ │ ● Head-to-Head Coding    ○ Team Battles    ○ Individual Challenges            │ │
│ │                                                                                 │ │
│ │ Round Configuration:                                                            │ │
│ │ Round Duration: [45] minutes                                                    │ │
│ │ Break Between Rounds: [15] minutes                                              │ │
│ │ Number of Problems per Round: [3]                                               │ │
│ │                                                                                 │ │
│ │ Problem Difficulty Distribution:                                                 │ │
│ │ Easy: [1] problems    Medium: [1] problems    Hard: [1] problems               │ │
│ │                                                                                 │ │
│ │ Scoring System:                                                                 │ │
│ │ ● Time-based (faster = higher score)                                           │ │
│ │ ○ Accuracy-based (correct solutions only)                                      │ │
│ │ ○ Hybrid (time + accuracy + code quality)                                      │ │
│ │                                                                                 │ │
│ │ ☑ Enable live streaming    ☑ Allow spectators    ☑ Real-time leaderboard      │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 NAVIGATION                                                                       │
│                                                                                     │
│ Progress: ████████░░░░ 50% (Step 2 of 4)                                           │
│                                                                                     │
│ [⬅️ Back: Basic Info] [💾 Save Draft]              [➡️ Next: Rewards & Rules]      │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 3. Event Creation Wizard (Step 3: Rewards & Rules)

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 🎯 Create New Event - Step 3 of 4: Rewards & Rules          [💾 Save Draft] [❌]    │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 🏆 REWARDS & PRIZES                                                                 │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🎁 PRIZE STRUCTURE                                                              │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 🥇 1st Place Winner                                                         │ │ │
│ │ │ XP Reward: [1000] XP                                                        │ │ │
│ │ │ Badges: ☑ Champion Badge  ☑ Event Winner  ☐ Custom Badge                   │ │ │
│ │ │ Special Rewards: ☑ Featured Profile  ☑ Mentor Access  ☐ Course Credits     │ │ │
│ │ │ Physical Prizes: [Gaming Keyboard + Certificate_________________]           │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 🥈 2nd Place                                                                │ │ │
│ │ │ XP Reward: [750] XP                                                         │ │ │
│ │ │ Badges: ☑ Runner-up Badge  ☑ Event Participant  ☐ Custom Badge             │ │ │
│ │ │ Special Rewards: ☑ Featured Profile  ☐ Mentor Access  ☐ Course Credits     │ │ │
│ │ │ Physical Prizes: [Gaming Mouse + Certificate____________________]           │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 🥉 3rd Place                                                                │ │ │
│ │ │ XP Reward: [500] XP                                                         │ │ │
│ │ │ Badges: ☑ Third Place Badge  ☑ Event Participant  ☐ Custom Badge           │ │ │
│ │ │ Special Rewards: ☐ Featured Profile  ☐ Mentor Access  ☐ Course Credits     │ │ │
│ │ │ Physical Prizes: [Coding Notebook Set + Certificate_____________]           │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ 🎖️ PARTICIPATION REWARDS                                                        │ │
│ │ All Participants: [100] XP + Participation Badge                               │ │
│ │ Top 10 Finishers: [250] XP + Top Performer Badge                              │ │
│ │ Most Improved: [200] XP + Growth Mindset Badge                                 │ │
│ │                                                                                 │ │
│ │ [+ Add Custom Prize Tier]                                                       │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📋 RULES & REQUIREMENTS                                                             │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📝 PARTICIPATION REQUIREMENTS                                                   │ │
│ │                                                                                 │ │
│ │ Minimum Level: [5] (Leave blank for no requirement)                            │ │
│ │ Required Skills: ☑ Basic Programming  ☑ Data Structures  ☐ Advanced Algorithms │ │
│ │ Prerequisites: ☐ Completed Course  ☐ Guild Membership  ☑ Profile Verification  │ │
│ │                                                                                 │ │
│ │ 🚫 RESTRICTIONS                                                                 │ │
│ │ ☑ No external resources during competition                                      │ │
│ │ ☑ No collaboration between participants                                         │ │
│ │ ☑ No AI assistance tools                                                       │ │
│ │ ☐ Specific IDE required                                                         │ │
│ │ ☐ Camera monitoring required                                                    │ │
│ │                                                                                 │ │
│ │ 🔧 TECHNICAL REQUIREMENTS                                                       │ │
│ │ Allowed Languages: ☑ Python  ☑ Java  ☑ C++  ☑ JavaScript  ☐ C#  ☐ Go        │ │
│ │ Code Execution Environment: ● Cloud-based  ○ Local setup required             │ │
│ │ Internet Connection: ● Required  ○ Optional  ○ Prohibited                      │ │
│ │                                                                                 │ │
│ │ 📜 CUSTOM RULES                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 1. All code must be original and written during the event                   │ │ │
│ │ │ 2. Participants must maintain professional conduct in chat                  │ │ │
│ │ │ 3. Any suspected cheating will result in immediate disqualification        │ │ │
│ │ │ 4. Technical issues should be reported immediately to organizers            │ │ │
│ │ │ 5. Final decisions on disputes rest with the event organizers               │ │ │
│ │ │                                                                             │ │ │
│ │ │ [Add more rules...]                                                         │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 NAVIGATION                                                                       │
│                                                                                     │
│ Progress: ████████████░ 75% (Step 3 of 4)                                          │
│                                                                                     │
│ [⬅️ Back: Schedule] [💾 Save Draft]                [➡️ Next: Review & Submit]      │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 4. Event Creation Wizard (Step 4: Review & Submit)

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 🎯 Create New Event - Step 4 of 4: Review & Submit          [💾 Save Draft] [❌]    │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 👀 EVENT PREVIEW                                                                    │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🏆 Winter Programming Challenge 2024                                            │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │                        [Event Banner Preview]                               │ │ │
│ │ │                     🏆 Competition • Intermediate                            │ │ │
│ │ │                    📅 Dec 15, 2024 • 2:00 PM UTC+7                         │ │ │
│ │ │                        ⏱️ Duration: 2h 30m                                  │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ 📝 Description: Join us for an exciting programming challenge featuring...      │ │
│ │ 🎯 Max Participants: 64 • 🏷️ Tags: programming, algorithms, competition        │ │
│ │ 🎁 Prizes: XP, Badges, Gaming Gear • 📋 Requirements: Level 5+                │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ ✅ SUBMISSION CHECKLIST                                                             │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📋 REQUIRED INFORMATION                                                         │ │
│ │ ✅ Event title and description                                                  │ │
│ │ ✅ Event type and category                                                      │ │
│ │ ✅ Schedule and timing                                                          │ │
│ │ ✅ Competition format (if applicable)                                           │ │
│ │ ✅ Rewards and prizes                                                           │ │
│ │ ✅ Rules and requirements                                                       │ │
│ │                                                                                 │ │
│ │ 📋 OPTIONAL ENHANCEMENTS                                                        │ │
│ │ ⚠️ Event banner (recommended for better visibility)                             │ │
│ │ ✅ Registration phases configured                                               │ │
│ │ ✅ Custom rules defined                                                         │ │
│ │ ⚠️ Promotional materials (can be added later)                                   │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🚀 SUBMISSION OPTIONS                                                               │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📤 PUBLICATION SETTINGS                                                         │ │
│ │                                                                                 │ │
│ │ Event Status:                                                                   │ │
│ │ ● Submit for Approval (Recommended)                                             │ │
│ │ ○ Save as Draft (Continue editing later)                                       │ │
│ │ ○ Publish Immediately (Guild Masters only)                                     │ │
│ │                                                                                 │ │
│ │ Approval Process:                                                               │ │
│ │ 📋 Your event will be reviewed by the Academic Committee                       │ │
│ │ ⏱️ Typical review time: 2-3 business days                                      │ │
│ │ 📧 You'll receive email notifications about status updates                     │ │
│ │ 🔄 You can make edits during the review process                                │ │
│ │                                                                                 │ │
│ │ 🔔 NOTIFICATIONS                                                                │ │
│ │ ☑ Email me when event is approved/rejected                                     │ │
│ │ ☑ Notify me of participant registrations                                       │ │
│ │ ☑ Send reminders before event starts                                           │ │
│ │ ☑ Weekly progress reports during registration                                   │ │
│ │                                                                                 │ │
│ │ 📊 ANALYTICS & TRACKING                                                         │ │
│ │ ☑ Enable detailed event analytics                                              │ │
│ │ ☑ Track participant engagement metrics                                          │ │
│ │ ☑ Generate post-event performance reports                                      │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 FINAL ACTIONS                                                                    │
│                                                                                     │
│ Progress: ████████████████ 100% (Step 4 of 4)                                      │
│                                                                                     │
│ [⬅️ Back: Rewards] [💾 Save Draft] [👁️ Preview]    [🚀 Submit for Approval]       │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 5. Event Approval Dashboard (Admin View)

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ ⚖️ Event Approval Dashboard                              [🔔 3] [⚙️] [👤 Admin]     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 📊 APPROVAL OVERVIEW                                                                │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📈 STATISTICS                                                                   │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────┐ │ │
│ │ │ 📋 PENDING REVIEW       │ │ ✅ APPROVED TODAY       │ │ ⏱️ AVG REVIEW TIME   │ │ │
│ │ │                         │ │                         │ │                     │ │ │
│ │ │        12               │ │         8               │ │      1.5 days       │ │ │
│ │ │   ⚠️ 3 urgent           │ │   🎯 2 competitions     │ │   📈 -0.3 vs last  │ │ │
│ │ │   📅 2 overdue          │ │   📚 6 workshops        │ │      week           │ │ │
│ │ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────┘ │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────┐ │ │
│ │ │ ❌ REJECTED THIS WEEK   │ │ 📊 TOTAL THIS MONTH     │ │ 👥 ACTIVE REVIEWERS │ │ │
│ │ │                         │ │                         │ │                     │ │ │
│ │ │         3               │ │        45               │ │         5           │ │ │
│ │ │   📝 Needs revision     │ │   📈 +12% vs last      │ │   🟢 4 online       │ │ │
│ │ │   ⚠️ Policy violations  │ │      month              │ │   ⏳ 1 busy         │ │ │
│ │ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────┘ │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📋 PENDING APPROVALS                                                                │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🔍 FILTERS & SEARCH                                                             │ │
│ │                                                                                 │ │
│ │ Search: [Winter Challenge___________] [🔍]                                      │ │
│ │ Filters: [All Types ▼] [All Priorities ▼] [All Submitters ▼] [Date Range ▼]   │ │
│ │ Sort by: [Submission Date ▼] [📊 Bulk Actions ▼]                               │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📋 EVENTS AWAITING REVIEW                                                       │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 🚨 URGENT: Winter Programming Challenge 2024                                │ │ │
│ │ │ Submitted by: Dr. Sarah Chen (Verified Lecturer) • 3 days ago              │ │ │
│ │ │ Type: 🏆 Competition • Category: Algorithms • Level: Intermediate           │ │ │
│ │ │ Event Date: Dec 15, 2024 • Participants: Up to 64                          │ │ │
│ │ │ Status: ⚠️ Overdue Review (SLA: 2 days)                                     │ │ │
│ │ │                                                                             │ │ │
│ │ │ 📊 Quick Stats: 💰 High-value prizes • 🎯 64 max participants • 🏆 Tournament│ │ │
│ │ │ 🔍 Issues: None detected • ✅ All requirements met                          │ │ │
│ │ │                                                                             │ │ │
│ │ │ [👁️ Review] [📝 Quick Approve] [❌ Reject] [👤 Assign Reviewer]            │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 📚 Advanced React Workshop Series                                           │ │ │
│ │ │ Submitted by: Mike Johnson (Guild Master - Code Masters) • 1 day ago       │ │ │
│ │ │ Type: 📚 Workshop • Category: Web Development • Level: Advanced             │ │ │
│ │ │ Event Date: Dec 20-22, 2024 • Participants: Up to 30                       │ │ │
│ │ │ Status: 🟡 Under Review (Assigned to: Alex Kim)                             │ │ │
│ │ │                                                                             │ │ │
│ │ │ 📊 Quick Stats: 💡 Multi-day event • 🎓 Certificate offered • 🔧 Hands-on   │ │ │
│ │ │ 🔍 Issues: ⚠️ Requires technical setup verification                         │ │ │
│ │ │                                                                             │ │ │
│ │ │ [👁️ Review] [💬 Add Comment] [📋 Request Changes] [👤 Reassign]            │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 🤝 Study Group: Data Structures Deep Dive                                  │ │ │
│ │ │ Submitted by: Emma Davis (Student Leader) • 6 hours ago                     │ │ │
│ │ │ Type: 🤝 Study Group • Category: Computer Science • Level: Beginner         │ │ │
│ │ │ Event Date: Dec 18, 2024 • Participants: Up to 15                          │ │ │
│ │ │ Status: 🟢 New Submission                                                   │ │ │
│ │ │                                                                             │ │ │
│ │ │ 📊 Quick Stats: 📖 Study materials provided • 🎯 Small group • 🔄 Recurring │ │ │
│ │ │ 🔍 Issues: None detected • ✅ Auto-approved eligible                        │ │ │
│ │ │                                                                             │ │ │
│ │ │ [👁️ Review] [⚡ Auto-Approve] [📝 Quick Approve] [👤 Assign]               │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ [📄 Show More (9 remaining)] [📊 Export List] [⚙️ Bulk Actions]                │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 ADMIN ACTIONS                                                                    │
│                                                                                     │
│ [📊 Analytics Dashboard] [⚙️ Approval Settings] [👥 Reviewer Management]           │
│ [📧 Send Notifications] [📋 Generate Reports] [🔄 Sync Calendar] [🛠️ Admin Tools]  │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 6. Event Review Interface (Detailed Review)

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 🔍 Event Review: Winter Programming Challenge 2024    [💾 Save] [🔙 Back] [❌]      │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 📋 EVENT INFORMATION                                                                │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 👤 SUBMITTER DETAILS                                                            │ │
│ │                                                                                 │ │
│ │ Name: Dr. Sarah Chen                                                            │ │
│ │ Role: Verified Lecturer • Computer Science Department                           │ │
│ │ Guild: Faculty Guild • Member since: Jan 2023                                  │ │
│ │ Previous Events: 12 approved, 0 rejected • Rating: ⭐⭐⭐⭐⭐ (4.8/5)           │ │
│ │ Contact: sarah.chen@fptu.edu.vn • Phone: +84-xxx-xxx-xxxx                     │ │
│ │                                                                                 │ │
│ │ 📊 SUBMISSION HISTORY                                                           │ │
│ │ • Last event: "Advanced Algorithms Workshop" (Nov 2024) - ✅ Approved          │ │
│ │ • Avg approval time: 1.2 days • Compliance score: 98%                          │ │
│ │ • Student feedback: 4.7/5 stars • Attendance rate: 92%                        │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📝 EVENT DETAILS REVIEW                                                             │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🎯 BASIC INFORMATION                                                            │ │
│ │                                                                                 │ │
│ │ Title: Winter Programming Challenge 2024                    ✅ Clear & Descriptive│ │
│ │ Type: Competition • Category: Algorithms & Data Structures  ✅ Appropriate      │ │
│ │ Level: Intermediate                                         ✅ Well-defined     │ │
│ │ Max Participants: 64                                        ✅ Reasonable       │ │
│ │                                                                                 │ │
│ │ Description: [View Full Text ▼]                                                 │ │
│ │ "Join us for an exciting programming challenge featuring algorithm problems..." │ │
│ │ ✅ Comprehensive • ✅ Clear objectives • ✅ Appropriate language                │ │
│ │                                                                                 │ │
│ │ 📅 SCHEDULING                                                                   │ │
│ │ Event Date: December 15, 2024, 2:00 PM UTC+7              ✅ Future date       │ │
│ │ Duration: 2 hours 30 minutes                               ✅ Appropriate       │ │
│ │ Registration Deadline: December 14, 2024, 11:59 PM        ✅ Sufficient time   │ │
│ │ Registration Phases: 2 phases configured                   ✅ Well-structured   │ │
│ │                                                                                 │ │
│ │ 🏆 COMPETITION FORMAT                                                           │ │
│ │ Tournament: Single Elimination                             ✅ Standard format   │ │
│ │ Battle Format: Head-to-Head Coding                         ✅ Engaging          │ │
│ │ Round Duration: 45 minutes                                 ✅ Reasonable        │ │
│ │ Problems per Round: 3 (Easy/Medium/Hard)                   ✅ Balanced          │ │
│ │ Scoring: Time-based                                        ✅ Fair system       │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ ⚖️ COMPLIANCE CHECK                                                                 │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📋 POLICY COMPLIANCE                                                            │ │
│ │                                                                                 │ │
│ │ ✅ Event aligns with educational objectives                                     │ │
│ │ ✅ Appropriate difficulty level for target audience                             │ │
│ │ ✅ Reasonable time commitment for participants                                  │ │
│ │ ✅ Fair and transparent judging criteria                                        │ │
│ │ ✅ Inclusive participation requirements                                         │ │
│ │ ✅ Appropriate prize structure                                                  │ │
│ │ ✅ Clear rules and guidelines                                                   │ │
│ │ ✅ Technical requirements are feasible                                          │ │
│ │                                                                                 │ │
│ │ 🔍 POTENTIAL ISSUES                                                             │ │
│ │ ⚠️ High-value physical prizes may require additional budget approval            │ │
│ │ ⚠️ 64 participants may strain server resources during peak times               │ │
│ │ ✅ No policy violations detected                                                │ │
│ │                                                                                 │ │
│ │ 📊 RISK ASSESSMENT                                                              │ │
│ │ Overall Risk Level: 🟡 Low-Medium                                               │ │
│ │ • Technical Risk: Low (standard competition format)                            │ │
│ │ • Budget Risk: Medium (physical prizes)                                        │ │
│ │ • Participation Risk: Low (popular topic, good timing)                         │ │
│ │ • Reputation Risk: Low (experienced organizer)                                 │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 💬 REVIEWER COMMENTS & ACTIONS                                                      │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📝 REVIEW NOTES                                                                 │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Internal Comments (Not visible to submitter):                               │ │ │
│ │ │ ┌───────────────────────────────────────────────────────────────────────────┐ │ │ │
│ │ │ │ This is a well-structured competition from an experienced organizer.      │ │ │ │
│ │ │ │ The format is engaging and the difficulty is appropriate. Main concern    │ │ │ │
│ │ │ │ is the budget for physical prizes - may need Finance approval.            │ │ │ │
│ │ │ │                                                                           │ │ │ │
│ │ │ │ Recommend approval with condition to confirm prize budget.                │ │ │ │
│ │ │ └───────────────────────────────────────────────────────────────────────────┘ │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Feedback to Submitter (Will be sent via email):                             │ │ │
│ │ │ ┌───────────────────────────────────────────────────────────────────────────┐ │ │ │
│ │ │ │ Great event proposal! The competition format is well-designed and         │ │ │ │
│ │ │ │ should provide an engaging experience for participants.                   │ │ │ │
│ │ │ │                                                                           │ │ │ │
│ │ │ │ Please confirm budget approval for physical prizes before final approval. │ │ │ │
│ │ │ └───────────────────────────────────────────────────────────────────────────┘ │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ 🎯 RECOMMENDED ACTION                                                           │ │
│ │ ● Approve with Conditions    ○ Approve Immediately    ○ Request Changes        │ │
│ │ ○ Reject with Feedback      ○ Escalate to Committee  ○ Request More Info      │ │
│ │                                                                                 │ │
│ │ 📋 CONDITIONS (if applicable):                                                  │ │
│ │ ☑ Confirm budget approval for physical prizes                                  │ │
│ │ ☐ Provide technical setup details                                              │ │
│ │ ☐ Submit participant safety plan                                               │ │
│ │ ☐ Other: [_________________________________]                                  │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 REVIEW ACTIONS                                                                   │
│                                                                                     │
│ [✅ Approve Event] [⚠️ Approve with Conditions] [📝 Request Changes] [❌ Reject]    │
│ [💬 Add Comment] [📧 Contact Submitter] [👥 Escalate] [📊 Generate Report]         │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## Design Notes

### Key Features Highlighted:

1. **Event Creation Wizard**
   - Step-by-step process with clear progress indicators
   - Comprehensive form fields for all event types
   - Draft saving capability throughout the process
   - Real-time validation and preview functionality

2. **Approval Workflow**
   - Dashboard overview with key metrics and statistics
   - Priority-based queue management
   - Detailed review interface with compliance checking
   - Automated risk assessment and policy validation

3. **Scheduling & Format Configuration**
   - Flexible scheduling options for different event types
   - Registration phase management with benefits
   - Competition-specific format configuration
   - Technical requirements specification

4. **Rewards & Rules Management**
   - Tiered prize structure configuration
   - Comprehensive rule definition system
   - Participation requirement specification
   - Custom rule creation capability

5. **Review & Approval System**
   - Detailed compliance checking
   - Risk assessment framework
   - Reviewer comment system
   - Conditional approval workflow

### Technical Considerations:

- **Workflow Management**: Clear progression through creation and approval stages
- **Validation**: Real-time form validation and policy compliance checking
- **Notifications**: Automated email notifications for status updates
- **Analytics**: Built-in tracking and reporting capabilities
- **Accessibility**: Clear visual indicators and status updates
- **Mobile Responsiveness**: Layouts adaptable to different screen sizes