# Guild Management Wireframes - Phase 3

## 1. Guild Master Dashboard

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 🏰 RogueLearn - Guild Master Dashboard                    👤 Prof. Sarah Kim [⚙️] │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 🏛️ GUILD: "Advanced Software Engineering"                           🎯 Level 15    │
│ 👥 Members: 127/150    📈 Activity: High    🏆 Rank: #3 in University              │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📊 GUILD OVERVIEW                                                                   │
│                                                                                     │
│ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────────┐ │
│ │ 📈 WEEKLY ACTIVITY      │ │ 🎯 ACTIVE LEARNING PATHS    │ │ 🏆 RECENT ACHIEVEMENTS  │ │
│ │                         │ │                         │ │                         │ │
│ │ ████████████░░░ 85%     │ │ • Design Patterns (23)  │ │ • Code Review Master   │ │
│ │ 108/127 members active  │ │ • Testing Strategies(15)│ │ • Algorithm Wizard     │ │
│ │                         │ │ • System Architecture(8)│ │ • Team Collaboration   │ │
│ │ 📅 This week: +12%      │ │ • Database Design (31)  │ │ • Problem Solver       │ │
│ │ 🔥 Streak: 23 days      │ │                         │ │                         │ │
│ │                         │ │ [📋 View All Paths] ────►│ │ [🏆 View All] ────────►│ │
│ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 👥 MEMBER MANAGEMENT                                                                │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🔍 [Search members...] [📊 Analytics] [📤 Bulk Actions] [➕ Invite Members]     │ │
│ │                                                                                 │ │
│ │ Filter: [All Members ▼] [Active ▼] [By Progress ▼] [By Role ▼]                 │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 👤 Alex Chen                    🎯 Level 12    📊 Progress: 78%    🔥 7d    │ │ │
│ │ │ 📧 alex.chen@fpt.edu.vn         🏷️ Advanced    📅 Joined: Sep 2024         │ │ │
│ │ │ 🎯 Current: React Architecture   ⭐ 2,340 XP    💬 Last active: 2h ago     │ │ │
│ │ │ [📊 View Profile] [💬 Message] [🎯 Assign Learning] [⚙️ Manage] [🚫 Remove]   │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 👤 Maria Rodriguez              🎯 Level 8     📊 Progress: 45%    🔥 3d    │ │ │
│ │ │ 📧 maria.r@fpt.edu.vn           🏷️ Beginner    📅 Joined: Oct 2024         │ │ │
│ │ │ 🎯 Current: Java Fundamentals   ⭐ 1,120 XP    💬 Last active: 1d ago     │ │ │
│ │ │ [📊 View Profile] [💬 Message] [🎯 Assign Learning] [⚙️ Manage] [🚫 Remove]   │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 👤 David Park                   🎯 Level 15    📊 Progress: 92%    🔥 12d   │ │ │
│ │ │ 📧 david.park@fpt.edu.vn        🏷️ Expert      📅 Joined: Aug 2024         │ │ │
│ │ │ 🎯 Current: Microservices       ⭐ 4,580 XP    💬 Last active: 30m ago    │ │ │
│ │ │ [📊 View Profile] [💬 Message] [🎯 Assign Learning] [⚙️ Manage] [👑 Promote]  │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ 📄 Showing 3 of 127 members                              [◀️ Prev] [Next ▶️]   │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🚀 QUICK ACTIONS                                                                    │
│                                                                                     │
│ [🎯 Create Learning Path] [📅 Schedule Event] [📢 Send Announcement] [📊 View Analytics]    │
│ [🏆 Manage Achievements] [📚 Update Resources] [⚙️ Guild Settings]                  │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 2. Guild Member Management Interface

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 👥 Member Management - Advanced Software Engineering Guild        [🔙 Back] [⚙️]   │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 🔍 MEMBER SEARCH & FILTERS                                                          │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🔍 Search: [alex chen________________] [🔍 Search] [🗑️ Clear]                   │ │
│ │                                                                                 │ │
│ │ Filters:                                                                        │ │
│ │ Status: [All ▼] Level: [Any ▼] Progress: [Any ▼] Role: [All ▼] Activity: [Any ▼]│ │
│ │ Joined: [Any time ▼] Last Active: [Any time ▼] Skills: [All skills ▼]          │ │
│ │                                                                                 │ │
│ │ [🔄 Apply Filters] [🗑️ Reset] [💾 Save Filter Preset]                          │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📊 MEMBER ANALYTICS                                                                 │
│                                                                                     │
│ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────────┐ │
│ │ 📈 ENGAGEMENT TRENDS    │ │ 🎯 SKILL DISTRIBUTION   │ │ 🏆 TOP PERFORMERS       │ │
│ │                         │ │                         │ │                         │ │
│ │ Active: 108 (85%)       │ │ Frontend: 45 members    │ │ 1. David Park (4,580)   │ │
│ │ Inactive: 19 (15%)      │ │ Backend: 38 members     │ │ 2. Lisa Wang (4,120)    │ │
│ │ New this week: 5        │ │ DevOps: 22 members      │ │ 3. Alex Chen (2,340)    │ │
│ │ At risk: 12             │ │ Mobile: 18 members      │ │ 4. Sarah Kim (2,100)    │ │
│ │                         │ │ Data Science: 15        │ │ 5. Mike Johnson (1,980) │ │
│ │ [📊 Detailed Report] ──►│ │ [📊 View Details] ─────►│ │ [🏆 View All] ────────►│ │
│ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 👤 DETAILED MEMBER VIEW                                                             │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 👤 Alex Chen                                                    🎯 Level 12     │ │
│ │ 📧 alex.chen@fpt.edu.vn                                        🏷️ Advanced      │ │
│ │ 🆔 Student ID: SE161234                                        📅 Joined: Sep 15│ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────┐ │ │
│ │ │ 📊 PROGRESS OVERVIEW    │ │ 🎯 CURRENT ACTIVITIES   │ │ 🏆 ACHIEVEMENTS     │ │ │
│ │ │                         │ │                         │ │                     │ │ │
│ │ │ Overall: 78% ████████░░ │ │ • React Architecture    │ │ • Code Warrior     │ │ │
│ │ │ XP: 2,340/3,000         │ │ • Database Design       │ │ • Team Player      │ │ │
│ │ │ Streak: 7 days 🔥       │ │ • Testing Best Practice │ │ • Quick Learner    │ │ │
│ │ │ Learning paths completed: 12/15 │ │                         │ │ • Problem Solver   │ │ │
│ │ │                         │ │ Party: JS Mastery Squad │ │                     │ │ │
│ │ │ [📈 View Details] ─────►│ │ [🎯 View All] ────────►│ │ [🏆 View All] ────►│ │ │
│ │ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────┘ │ │
│ │                                                                                 │ │
│ │ 💬 COMMUNICATION HISTORY                                                        │ │
│ │ • Dec 10: Completed React Hooks learning path - Great work!                            │ │
│ │ • Dec 8: Asked for help with state management - Provided resources             │ │
│ │ • Dec 5: Joined party "JS Mastery Squad" - Welcome message sent                │ │
│ │                                                                                 │ │
│ │ 🎯 ASSIGNED LEARNING PATHS                                                              │ │
│ │ • React Architecture Patterns (Due: Dec 20) - In Progress                      │ │
│ │ • Advanced Testing Strategies (Due: Dec 25) - Not Started                      │ │
│ │ • Database Optimization (Due: Jan 5) - Not Started                             │ │
│ │                                                                                 │ │
│ │ [💬 Send Message] [🎯 Assign Learning Path] [📊 View Full Profile] [⚙️ Edit Settings]   │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🔧 BULK ACTIONS                                                                     │
│                                                                                     │
│ ☐ Select All  |  Selected: 0 members                                              │
│                                                                                     │
│ [📤 Send Bulk Message] [🎯 Assign Learning to Selected] [🏷️ Add Tags]                 │
│ [📊 Export Data] [🚫 Remove Selected] [⚙️ Bulk Settings]                           │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 3. Guild Progress Tracking Dashboard

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 📊 Guild Progress Tracking - Advanced Software Engineering        [🔙 Back] [⚙️]   │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 📈 OVERALL GUILD PERFORMANCE                                                        │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🎯 COMPLETION RATES                                                             │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────┐ │ │
│ │ │ 📚 QUEST COMPLETION     │ │ 🏆 ACHIEVEMENT UNLOCKS  │ │ 📈 SKILL PROGRESSION│ │ │
│ │ │                         │ │                         │ │                     │ │ │
│ │ │ This Month: 89%         │ │ This Month: 156         │ │ Avg Level: 9.2      │ │ │
│ │ │ ████████████████████░   │ │ 🥇 Gold: 23             │ │ Level 10+: 45%      │ │ │
│ │ │                         │ │ 🥈 Silver: 67           │ │ Level 15+: 12%      │ │ │
│ │ │ Last Month: 82% (+7%)   │ │ 🥉 Bronze: 66           │ │ Beginners: 23%      │ │ │
│ │ │ Target: 85% ✅          │ │ Target: 120 ✅          │ │ Target: 8.5 ✅      │ │ │
│ │ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────┘ │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎯 QUEST ANALYTICS                                                                  │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📊 ACTIVE QUESTS BREAKDOWN                                                      │ │
│ │                                                                                 │ │
│ │ Quest Name                    │ Assigned │ In Progress │ Completed │ Success %  │ │
│ │ ─────────────────────────────┼──────────┼─────────────┼───────────┼──────────  │ │
│ │ React Architecture Patterns  │    45    │     23      │    18     │   78%      │ │
│ │ Database Design Fundamentals │    38    │     15      │    20     │   87%      │ │
│ │ Testing Strategies           │    31    │     12      │    15     │   83%      │ │
│ │ System Architecture          │    28    │      8      │    12     │   75%      │ │
│ │ DevOps & Deployment          │    22    │      9      │     8     │   72%      │ │
│ │ Mobile Development           │    18    │      7      │     6     │   68%      │ │
│ │                                                                                 │ │
│ │ [📊 Detailed Quest Analytics] [📈 Trend Analysis] [🎯 Quest Performance]        │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 👥 MEMBER PROGRESS INSIGHTS                                                         │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🚨 MEMBERS NEEDING ATTENTION                                                    │ │
│ │                                                                                 │ │
│ │ ⚠️ At Risk (12 members):                                                        │ │
│ │ • Maria Rodriguez - No activity for 5 days, struggling with Java basics       │ │
│ │ • John Smith - 3 overdue quests, last login 1 week ago                        │ │
│ │ • Lisa Chen - Consistently low quest scores, may need mentoring               │ │
│ │                                                                                 │ │
│ │ 🌟 High Performers (15 members):                                               │ │
│ │ • David Park - 92% completion rate, mentor potential                          │ │
│ │ • Sarah Kim - Consistent high scores, leadership qualities                     │ │
│ │ • Alex Chen - Rapid progress, good collaboration skills                       │ │
│ │                                                                                 │ │
│ │ [📧 Send Intervention Messages] [🎯 Assign Mentors] [🏆 Recognize Achievers]   │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📅 TIMELINE & MILESTONES                                                            │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🎯 UPCOMING DEADLINES                                                           │ │
│ │                                                                                 │ │
│ │ Dec 20: React Architecture Patterns (23 members) - 🟡 Medium Priority          │ │
│ │ Dec 25: Advanced Testing Strategies (15 members) - 🔴 High Priority            │ │
│ │ Jan 5:  Database Optimization (31 members) - 🟢 Low Priority                   │ │
│ │ Jan 15: System Design Capstone (All members) - 🔴 Critical                     │ │
│ │                                                                                 │ │
│ │ 🏆 MILESTONE PROGRESS                                                           │ │
│ │ • Semester Goal: 85% average completion ████████████████████░ 89% ✅           │ │
│ │ • Skill Mastery: 70% members at Level 10+ ████████████░░░░░░ 45% 🟡           │ │
│ │ • Collaboration: 90% in active parties ████████████████████░ 78% 🟡           │ │
│ │                                                                                 │ │
│ │ [📅 Adjust Deadlines] [🎯 Set New Milestones] [📊 Progress Report]             │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📊 EXPORT & REPORTING                                                               │
│                                                                                     │
│ [📄 Generate Progress Report] [📊 Export Analytics] [📧 Email Summary]              │
│ [📈 Trend Analysis] [🎯 Custom Dashboard] [⚙️ Report Settings]                      │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 4. Guild Communication Center

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 💬 Guild Communication Center - Advanced Software Engineering     [🔙 Back] [⚙️]   │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 📢 ANNOUNCEMENTS & BROADCASTS                                                       │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ ✏️ CREATE NEW ANNOUNCEMENT                                                      │ │
│ │                                                                                 │ │
│ │ Title: [New Quest Available: Microservices Architecture_____________]           │ │
│ │                                                                                 │ │
│ │ Message:                                                                        │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Hello everyone! 👋                                                          │ │ │
│ │ │                                                                             │ │ │
│ │ │ I'm excited to announce a new advanced quest on Microservices              │ │ │
│ │ │ Architecture. This quest will cover:                                       │ │ │
│ │ │ • Service decomposition strategies                                         │ │ │
│ │ │ • API gateway patterns                                                     │ │ │
│ │ │ • Data consistency in distributed systems                                  │ │ │
│ │ │                                                                             │ │ │
│ │ │ Prerequisites: Complete "System Design Fundamentals" quest                 │ │ │
│ │ │ Deadline: January 30, 2025                                                 │ │ │
│ │ │                                                                             │ │ │
│ │ │ Good luck, and happy learning! 🚀                                          │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ Target Audience:                                                                │ │
│ │ ☑ All Guild Members  ☐ Advanced Only  ☐ Specific Groups  ☐ Custom Selection   │ │
│ │                                                                                 │ │
│ │ Priority: ○ Low  ● Medium  ○ High  ○ Urgent                                    │ │
│ │                                                                                 │ │
│ │ Delivery Options:                                                               │ │
│ │ ☑ In-app notification  ☑ Email  ☐ SMS  ☑ Discord/Slack integration            │ │
│ │                                                                                 │ │
│ │ Schedule: ● Send now  ○ Schedule for later: [Date] [Time]                      │ │
│ │                                                                                 │ │
│ │ [📝 Save Draft] [👁️ Preview] [📤 Send Announcement]                            │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📬 RECENT ANNOUNCEMENTS                                                             │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📅 Dec 15, 2024 - 2:30 PM                                    🔴 High Priority  │ │
│ │ 📢 "Final Project Guidelines Released"                                          │ │
│ │ 👥 Sent to: All Members (127)  📊 Read: 89%  💬 Replies: 12                   │ │
│ │ [👁️ View] [📊 Analytics] [💬 Replies] [📝 Edit] [🗑️ Delete]                   │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📅 Dec 12, 2024 - 10:15 AM                                   🟡 Medium Priority│ │
│ │ 📢 "New Study Groups Forming"                                                  │ │
│ │ 👥 Sent to: All Members (127)  📊 Read: 76%  💬 Replies: 8                    │ │
│ │ [👁️ View] [📊 Analytics] [💬 Replies] [📝 Edit] [🗑️ Delete]                   │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📅 Dec 10, 2024 - 4:45 PM                                    🟢 Low Priority   │ │
│ │ 📢 "Weekly Leaderboard Update"                                                 │ │
│ │ 👥 Sent to: All Members (127)  📊 Read: 92%  💬 Replies: 3                    │ │
│ │ [👁️ View] [📊 Analytics] [💬 Replies] [📝 Edit] [🗑️ Delete]                   │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 💬 DIRECT MESSAGING                                                                 │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🔍 COMPOSE MESSAGE                                                              │ │
│ │                                                                                 │ │
│ │ To: [Search members...] [📋 Select from list] [👥 Select group]                │ │
│ │                                                                                 │ │
│ │ Selected Recipients:                                                            │ │
│ │ • Alex Chen (alex.chen@fpt.edu.vn)                                             │ │
│ │ • Maria Rodriguez (maria.r@fpt.edu.vn)                                         │ │
│ │                                                                                 │ │
│ │ Subject: [Follow-up on React Architecture Quest_______________]                 │ │
│ │                                                                                 │ │
│ │ Message:                                                                        │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Hi Alex and Maria,                                                          │ │ │
│ │ │                                                                             │ │ │
│ │ │ I noticed you both are working on the React Architecture quest.            │ │ │
│ │ │ I wanted to check in and see if you need any additional resources          │ │ │
│ │ │ or guidance.                                                                │ │ │
│ │ │                                                                             │ │ │
│ │ │ Feel free to reach out if you have any questions!                          │ │ │
│ │ │                                                                             │ │ │
│ │ │ Best regards,                                                               │ │ │
│ │ │ Prof. Sarah Kim                                                             │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ Attachments: [📎 Add files] [🔗 Add links] [🎯 Link quests]                    │ │
│ │                                                                                 │ │
│ │ Options:                                                                        │ │
│ │ ☑ Request read receipt  ☐ High priority  ☑ Allow replies                      │ │
│ │                                                                                 │ │
│ │ [📝 Save Draft] [📤 Send Message]                                              │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📊 COMMUNICATION ANALYTICS                                                          │
│                                                                                     │
│ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────────┐ │
│ │ 📈 ENGAGEMENT METRICS   │ │ 💬 MESSAGE STATISTICS   │ │ 🎯 RESPONSE RATES       │ │
│ │                         │ │                         │ │                         │ │
│ │ Avg. Read Rate: 84%     │ │ Messages sent: 45       │ │ Announcements: 78%      │ │
│ │ Avg. Response: 23%      │ │ Direct messages: 28     │ │ Direct messages: 92%    │ │
│ │ Active discussions: 12  │ │ Group messages: 17      │ │ Quest updates: 65%      │ │
│ │ Member participation:   │ │ Avg. response time: 4h  │ │ Event invites: 89%      │ │
│ │ High: 67% Medium: 28%   │ │                         │ │                         │ │
│ │                         │ │ [📊 Detailed Stats] ───►│ │ [📈 Trend Analysis] ───►│ │
│ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ ⚙️ COMMUNICATION SETTINGS                                                           │
│                                                                                     │
│ [🔔 Notification Settings] [📧 Email Templates] [🎨 Message Formatting]             │
│ [👥 Group Management] [📊 Analytics Dashboard] [🔒 Privacy Settings]                │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 5. Guild Resource Management

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 📚 Guild Resource Management - Advanced Software Engineering      [🔙 Back] [⚙️]   │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 📁 RESOURCE LIBRARY                                                                 │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🔍 [Search resources...] [🏷️ Filter by tags] [📊 Sort by: Recent ▼] [➕ Add]    │ │
│ │                                                                                 │ │
│ │ 📂 Folder Structure:                                                            │ │
│ │ ├── 📁 Course Materials (45 items)                                             │ │
│ │ │   ├── 📁 Lectures & Slides (23)                                              │ │
│ │ │   ├── 📁 Assignment Templates (12)                                           │ │
│ │ │   └── 📁 Reference Materials (10)                                            │ │
│ │ ├── 📁 Code Examples & Projects (67 items)                                     │ │
│ │ │   ├── 📁 React Projects (25)                                                 │ │
│ │ │   ├── 📁 Backend Examples (22)                                               │ │
│ │ │   └── 📁 Full-Stack Applications (20)                                        │ │
│ │ ├── 📁 Study Guides & Cheat Sheets (34 items)                                 │ │
│ │ ├── 📁 Video Tutorials (28 items)                                              │ │
│ │ └── 📁 External Resources & Links (156 items)                                  │ │
│ │                                                                                 │ │
│ │ 📊 Storage Usage: ████████░░ 8.2GB / 10GB                                      │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📄 RECENT RESOURCES                                                                 │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📄 React Architecture Patterns Guide.pdf                    📅 Dec 15, 2:30 PM │ │
│ │ 👤 Added by: Prof. Sarah Kim  📊 Downloads: 23  ⭐ Rating: 4.8/5               │ │
│ │ 🏷️ Tags: #react #architecture #patterns #advanced                              │ │
│ │ [👁️ Preview] [📥 Download] [📊 Analytics] [✏️ Edit] [🗑️ Delete]                │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🎥 Microservices Design Patterns - Video Series             📅 Dec 14, 4:15 PM │ │
│ │ 👤 Added by: David Park  📊 Views: 45  ⭐ Rating: 4.9/5                        │ │
│ │ 🏷️ Tags: #microservices #design-patterns #video #tutorial                      │ │
│ │ [▶️ Watch] [📋 Transcript] [📊 Analytics] [✏️ Edit] [🗑️ Delete]                 │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 💻 Database Optimization Code Examples                       📅 Dec 13, 1:45 PM │ │
│ │ 👤 Added by: Alex Chen  📊 Downloads: 18  ⭐ Rating: 4.6/5                     │ │
│ │ 🏷️ Tags: #database #optimization #sql #performance                             │ │
│ │ [👁️ Preview] [📥 Download] [📊 Analytics] [✏️ Edit] [🗑️ Delete]                │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ ➕ ADD NEW RESOURCE                                                                 │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Resource Type:                                                                  │ │
│ │ ● File Upload  ○ External Link  ○ Video URL  ○ Text Content                    │ │
│ │                                                                                 │ │
│ │ 📁 Upload File: [Choose File] [📎 Drag & Drop Area]                            │ │
│ │ Supported formats: PDF, DOC, PPT, MP4, ZIP, etc. (Max: 100MB)                  │ │
│ │                                                                                 │ │
│ │ Title: [Advanced React Patterns Workshop Materials_____________]                │ │
│ │                                                                                 │ │
│ │ Description:                                                                    │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Comprehensive workshop materials covering advanced React patterns           │ │ │
│ │ │ including render props, higher-order components, and custom hooks.         │ │ │
│ │ │ Includes practical examples and exercises.                                  │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ Category: [Course Materials ▼]                                                 │ │
│ │ Folder: [React Projects ▼] [📁 Create New Folder]                              │ │
│ │                                                                                 │ │
│ │ Tags: [react, patterns, workshop, advanced, hooks]                             │ │
│ │                                                                                 │ │
│ │ Access Level:                                                                   │ │
│ │ ● All Guild Members  ○ Advanced Only  ○ Specific Groups  ○ Private             │ │
│ │                                                                                 │ │
│ │ Quest Integration:                                                              │ │
│ │ ☑ Link to "React Architecture Patterns" quest                                  │ │
│ │ ☐ Create new quest from this resource                                          │ │
│ │                                                                                 │ │
│ │ [📝 Save Draft] [👁️ Preview] [📤 Publish Resource]                             │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📊 RESOURCE ANALYTICS                                                               │
│                                                                                     │
│ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────────┐ │
│ │ 📈 USAGE STATISTICS     │ │ ⭐ TOP RATED RESOURCES  │ │ 🔥 TRENDING CONTENT     │ │
│ │                         │ │                         │ │                         │ │
│ │ Total resources: 330    │ │ 1. React Hooks Guide    │ │ 1. Microservices Video  │ │
│ │ Downloads this week: 89 │ │    ⭐ 4.9/5 (67 votes)  │ │    📈 +45 views        │ │
│ │ Most accessed: PDF      │ │ 2. SQL Optimization     │ │ 2. System Design       │ │
│ │ Avg. rating: 4.7/5      │ │    ⭐ 4.8/5 (43 votes)  │ │    📈 +32 downloads    │ │
│ │ Storage used: 82%       │ │ 3. Testing Strategies   │ │ 3. React Patterns      │ │
│ │                         │ │    ⭐ 4.8/5 (38 votes)  │ │    📈 +28 views        │ │
│ │ [📊 Full Report] ──────►│ │ [⭐ View All] ────────►│ │ [🔥 View Trends] ─────►│ │
│ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ ⚙️ RESOURCE MANAGEMENT                                                              │
│                                                                                     │
│ [📁 Organize Folders] [🏷️ Manage Tags] [👥 Access Control] [📊 Analytics Dashboard] │
│ [🔄 Sync External] [💾 Backup Resources] [⚙️ Settings]                              │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 6. Guild Settings & Configuration

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ ⚙️ Guild Settings - Advanced Software Engineering                 [🔙 Back] [💾]   │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 🏛️ GUILD INFORMATION                                                                │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Guild Name: [Advanced Software Engineering_________________________]            │ │
│ │                                                                                 │ │
│ │ Description:                                                                    │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ A comprehensive guild focused on advanced software engineering concepts,    │ │ │
│ │ │ including system design, architecture patterns, and modern development     │ │ │
│ │ │ practices. Perfect for students looking to master enterprise-level         │ │ │
│ │ │ programming skills and prepare for senior developer roles.                 │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ Guild Icon: [🏛️ Current] [📁 Upload New] [🎨 Choose from Library]               │ │
│ │                                                                                 │ │
│ │ Tags: [software-engineering, advanced, system-design, architecture]            │ │
│ │                                                                                 │ │
│ │ Visibility: ● Public  ○ Private  ○ Invite Only                                │ │
│ │                                                                                 │ │
│ │ Member Limit: [150] (Current: 127)                                             │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 👥 MEMBERSHIP SETTINGS                                                              │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Join Requirements:                                                              │ │
│ │ ☑ Minimum Level: [5] ☑ Completed Prerequisites: [Programming Fundamentals]     │ │
│ │ ☑ FPTU Student Status ☐ Manual Approval Required                               │ │
│ │                                                                                 │ │
│ │ Member Roles & Permissions:                                                     │ │
│ │                                                                                 │ │
│ │ 👑 Guild Master (1):                                                           │ │
│ │ ☑ Full administrative access ☑ Member management ☑ Resource management        │ │
│ │ ☑ Quest creation ☑ Event management ☑ Analytics access                        │ │
│ │                                                                                 │ │
│ │ 🛡️ Moderators (3):                                                             │ │
│ │ ☑ Member management ☑ Resource moderation ☑ Quest assignment                  │ │
│ │ ☐ Analytics access ☑ Communication tools ☐ Guild settings                     │ │
│ │                                                                                 │ │
│ │ ⭐ Senior Members (15):                                                         │ │
│ │ ☑ Mentor new members ☑ Create study groups ☑ Share resources                  │ │
│ │ ☐ Quest creation ☑ Event participation ☐ Member management                    │ │
│ │                                                                                 │ │
│ │ 👤 Regular Members (108):                                                      │ │
│ │ ☑ Access resources ☑ Join quests ☑ Participate in events                     │ │
│ │ ☑ Create parties ☑ Share progress ☐ Administrative functions                  │ │
│ │                                                                                 │ │
│ │ [👥 Manage Roles] [🔒 Edit Permissions] [📊 Role Analytics]                    │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎯 QUEST & PROGRESS SETTINGS                                                        │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Quest Creation:                                                                 │ │
│ │ ☑ Allow moderators to create quests ☐ Allow senior members to create quests   │ │
│ │ ☑ Require approval for new quests ☑ Auto-assign based on skill level          │ │
│ │                                                                                 │ │
│ │ Progress Tracking:                                                              │ │
│ │ ☑ Public leaderboards ☑ Progress sharing ☑ Achievement notifications          │ │
│ │ ☑ Weekly progress reports ☑ Milestone celebrations                             │ │
│ │                                                                                 │ │
│ │ Grading & Assessment:                                                           │ │
│ │ ☑ Peer review system ☑ Automated testing ☑ Manual review required             │ │
│ │ ☑ Multiple submission attempts ☑ Partial credit allowed                       │ │
│ │                                                                                 │ │
│ │ Default Quest Settings:                                                         │ │
│ │ Difficulty: [Adaptive ▼] Duration: [2 weeks ▼] Retries: [3 ▼]                 │ │
│ │                                                                                 │ │
│ │ [🎯 Quest Templates] [📊 Assessment Rubrics] [⚙️ Advanced Settings]            │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🔔 NOTIFICATION SETTINGS                                                            │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Guild-wide Notifications:                                                       │ │
│ │ ☑ New member joins ☑ Quest completions ☑ Achievement unlocks                  │ │
│ │ ☑ Event announcements ☑ Milestone celebrations ☐ Daily activity summaries     │ │
│ │                                                                                 │ │
│ │ Member Notifications:                                                           │ │
│ │ ☑ Quest assignments ☑ Deadline reminders ☑ Progress updates                   │ │
│ │ ☑ Party invitations ☑ Mentor assignments ☑ Resource recommendations           │ │
│ │                                                                                 │ │
│ │ Delivery Channels:                                                              │ │
│ │ ☑ In-app notifications ☑ Email notifications ☐ SMS alerts                     │ │
│ │ ☑ Discord integration ☑ Slack integration ☐ Microsoft Teams                   │ │
│ │                                                                                 │ │
│ │ Frequency Settings:                                                             │ │
│ │ Urgent: Immediate  High: Every 2 hours  Medium: Daily  Low: Weekly             │ │
│ │                                                                                 │ │
│ │ [🔔 Test Notifications] [📧 Email Templates] [⚙️ Advanced Settings]            │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🔒 PRIVACY & SECURITY                                                               │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Data Privacy:                                                                   │ │
│ │ ☑ Member profile visibility controls ☑ Progress sharing opt-in                 │ │
│ │ ☑ Anonymous analytics ☐ External data sharing                                  │ │
│ │                                                                                 │ │
│ │ Content Moderation:                                                             │ │
│ │ ☑ Automatic content filtering ☑ Report system ☑ Manual review queue           │ │
│ │ ☑ Plagiarism detection ☑ Code similarity checking                              │ │
│ │                                                                                 │ │
│ │ Access Control:                                                                 │ │
│ │ ☑ Two-factor authentication ☑ Session timeout: [2 hours ▼]                    │ │
│ │ ☑ IP restriction for admin functions ☑ Audit logging                          │ │
│ │                                                                                 │ │
│ │ Data Retention:                                                                 │ │
│ │ Member data: [2 years ▼] Activity logs: [1 year ▼] Backups: [6 months ▼]      │ │
│ │                                                                                 │ │
│ │ [🔒 Security Audit] [📋 Privacy Policy] [🗑️ Data Cleanup]                      │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🔗 INTEGRATIONS                                                                     │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ FPTU Portal: ✅ Connected                                                       │ │
│ │ • Automatic grade sync ☑ • Course calendar sync ☑ • Student verification ☑   │ │
│ │ [⚙️ Configure] [🔄 Sync Now] [🔌 Disconnect]                                   │ │
│ │                                                                                 │ │
│ │ Discord: ✅ Connected                                                           │ │
│ │ • Guild channel: #advanced-software-eng • Notifications ☑ • Role sync ☑      │ │
│ │ [⚙️ Configure] [🔄 Sync Roles] [🔌 Disconnect]                                 │ │
│ │                                                                                 │ │
│ │ GitHub: ❌ Not Connected                                                        │ │
│ │ • Code repository integration • Automated testing • Project submissions       │ │
│ │ [🔌 Connect] [📚 Learn More]                                                   │ │
│ │                                                                                 │ │
│ │ Slack: ❌ Not Connected                                                         │ │
│ │ • Team communication • Progress updates • Event notifications                  │ │
│ │ [🔌 Connect] [📚 Learn More]                                                   │ │
│ │                                                                                 │ │
│ │ [🔌 Browse Integrations] [⚙️ API Settings] [📊 Integration Analytics]          │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 💾 BACKUP & EXPORT                                                                  │
│                                                                                     │
│ [💾 Create Backup] [📤 Export Data] [📊 Usage Analytics] [🗑️ Delete Guild]         │
│ [🔄 Reset Settings] [📋 Audit Log] [❓ Help & Support]                              │
└─────────────────────────────────────────────────────────────────────────────────────┘
```