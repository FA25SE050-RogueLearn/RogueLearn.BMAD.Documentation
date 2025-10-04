# Phase 2: Social & Collaboration Wireframes

## 1. Party Creation Interface

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Social > Create Party  🔔 👤 ⚙️    │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🎉 CREATE YOUR LEARNING PARTY                      │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Party Name *                                    │ │
│ │ [JavaScript Mastery Squad____________]          │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Description                                     │ │
│ │ [Focused on mastering React, Node.js, and      │ │
│ │  modern JavaScript frameworks. Weekly study     │ │
│ │  sessions and collaborative projects.           │ │
│ │  ________________________________]             │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ Party Size: ○ 2-4 members  ● 5-6 members  ○ 7-8   │
│                                                     │
│ Focus Areas (Select up to 3):                      │
│ ☑ Frontend Development  ☑ Backend Development      │
│ ☐ Database Design      ☐ DevOps & Deployment      │
│ ☐ Mobile Development   ☐ AI/ML                     │
│                                                     │
│ Privacy Settings:                                   │
│ ● Public (Anyone can request to join)              │
│ ○ Invite Only (Members must be invited)            │
│ ○ Guild Members Only (Restricted to your guild)    │
│                                                     │
│ ┌─────────────────┐ ┌─────────────────┐           │
│ │   [Cancel]      │ │ [Create Party]  │           │
│ └─────────────────┘ └─────────────────┘           │
└─────────────────────────────────────────────────────┘
```

## 2. Party Dashboard

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > My Party: JS Mastery Squad 🔔 👤 ⚙️│
├─────────────────────────────────────────────────────┤
│                                                     │
│ 👥 PARTY MEMBERS (5/6)                 [⚙️ Settings]│
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 👑 Alex Chen (Party Leader)        Level 12     │ │
│ │    Online • Last active: Now                    │ │
│ │    Current Quest: Master React Hooks            │ │
│ │                                    [Message] ──►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 👤 Sarah Kim                       Level 10     │ │
│ │    Online • Last active: 5 min ago              │ │
│ │    Current Quest: Node.js Fundamentals          │ │
│ │                                    [Message] ──►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 👤 Mike Rodriguez                  Level 11     │ │
│ │    Away • Last active: 2 hours ago              │ │
│ │    Current Quest: Database Design Basics        │ │
│ │                                    [Message] ──►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [+ Invite Members]                                  │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📅 UPCOMING MEETINGS                                │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📚 Weekly Study Session                         │ │
│ │ Tomorrow, 7:00 PM - 9:00 PM                     │ │
│ │ Topic: React Hooks Deep Dive                    │ │
│ │ 👥 3/5 confirmed  📍 Virtual Room #JS-001       │ │
│ │                              [Join] [Details] ──►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [📅 Schedule New Meeting]                           │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📦 PARTY STASH                     [🔍 Search] [📁]│
│                                                     │
│ Recent Additions:                                   │
│ • 📄 React Hooks Cheat Sheet (Sarah, 2h ago)       │
│ • 🔗 Advanced Node.js Tutorial (Mike, 1d ago)      │
│ • 📝 Study Notes: Async/Await (Alex, 2d ago)       │
│                                                     │
│ [📤 Upload Resource] [📋 View All Resources]        │
└─────────────────────────────────────────────────────┘
```

## 3. Party Stash (Shared Resources)

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Party Stash               🔔 👤 ⚙️  │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 📦 JS MASTERY SQUAD - SHARED RESOURCES              │
│                                                     │
│ 🔍 [Search resources...] 📊 Filter ▼ 📤 [Upload]   │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📁 FOLDERS                                          │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📂 React Resources (12 items)                   │ │
│ │    Last updated: 2 hours ago by Sarah           │ │
│ │                                    [Open] ─────►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📂 Node.js & Backend (8 items)                  │ │
│ │    Last updated: 1 day ago by Mike              │ │
│ │                                    [Open] ─────►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📂 Study Notes & Summaries (15 items)           │ │
│ │    Last updated: 3 hours ago by Alex            │ │
│ │                                    [Open] ─────►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📄 RECENT FILES                                     │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📄 React Hooks Cheat Sheet.pdf                  │ │
│ │ 👤 Sarah Kim • 2 hours ago • 2.3 MB             │ │
│ │ ⭐⭐⭐⭐⭐ (5 ratings) 💬 3 comments             │ │
│ │                    [Download] [View] [Comment] ──►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🔗 Advanced Node.js Tutorial Series             │ │
│ │ 👤 Mike Rodriguez • 1 day ago • External Link   │ │
│ │ ⭐⭐⭐⭐⭐ (3 ratings) 💬 1 comment              │ │
│ │                      [Open Link] [Comment] ─────►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📝 Async/Await Study Notes.md                   │ │
│ │ 👤 Alex Chen • 2 days ago • 1.1 MB              │ │
│ │ ⭐⭐⭐⭐⭐ (4 ratings) 💬 2 comments             │ │
│ │                    [Download] [Edit] [Comment] ──►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ 📊 Storage Used: 45.2 MB / 500 MB                  │
└─────────────────────────────────────────────────────┘
```

## 4. Meeting Scheduler

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Schedule Meeting           🔔 👤 ⚙️  │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 📅 SCHEDULE PARTY MEETING                           │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Meeting Title *                                 │ │
│ │ [React Hooks Deep Dive Session_______]          │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Description                                     │ │
│ │ [Let's dive deep into React Hooks - useState,   │ │
│ │  useEffect, and custom hooks. Bring your       │ │
│ │  questions and code examples!                   │ │
│ │  ________________________________]             │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ 📅 Date & Time                                      │
│ ┌─────────────────┐ ┌─────────────────┐           │
│ │ [📅 Tomorrow   ▼]│ │ [🕰️ 7:00 PM   ▼]│           │
│ └─────────────────┘ └─────────────────┘           │
│                                                     │
│ ⏱️ Duration: [2 hours ▼]                           │
│                                                     │
│ 📍 Meeting Type:                                    │
│ ● Virtual (Video call)                             │
│ ○ In-person (Location required)                    │
│ ○ Hybrid (Both virtual and in-person)              │
│                                                     │
│ 👥 INVITE PARTY MEMBERS                             │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ ☑ Sarah Kim          ☑ Mike Rodriguez           │ │
│ │ ☑ Emma Wilson        ☐ David Park               │ │
│ │ ☑ Lisa Chen                                     │ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ 📋 Agenda Items (Optional)                          │
│ • Introduction to Hooks (15 min)                   │
│ • useState Deep Dive (30 min)                      │
│ • useEffect Patterns (45 min)                      │
│ • Custom Hooks Workshop (30 min)                   │
│ [+ Add Agenda Item]                                 │
│                                                     │
│ 🔔 Reminder Settings:                               │
│ ☑ 1 day before  ☑ 1 hour before  ☐ 15 min before  │
│                                                     │
│ ┌─────────────────┐ ┌─────────────────┐           │
│ │   [Cancel]      │ │ [Schedule Meeting]│           │
│ └─────────────────┘ └─────────────────┘           │
└─────────────────────────────────────────────────────┘
```

## 5. Live Meeting Interface

```
┌─────────────────────────────────────────────────────┐
│ 🎥 React Hooks Deep Dive Session - LIVE    [🔴 REC] │
├─────────────────────────────────────────────────────┤
│                                                     │
│ ┌─────────────────┐ ┌─────────────────┐           │
│ │ 👤 Alex Chen    │ │ 👤 Sarah Kim    │           │
│ │ (Host)          │ │ 🎤 Speaking     │           │
│ │ 🎤 🎥 📺        │ │ 🎤 🎥           │           │
│ └─────────────────┘ └─────────────────┘           │
│                                                     │
│ ┌─────────────────┐ ┌─────────────────┐           │
│ │ 👤 Mike Rodriguez│ │ 👤 Emma Wilson  │           │
│ │ 🔇 🎥           │ │ 🎤 🎥           │           │
│ └─────────────────┘ └─────────────────┘           │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 📋 MEETING AGENDA                    ⏱️ 45:30 / 120:00│
│                                                     │
│ ✅ Introduction to Hooks (15 min) - Completed       │
│ ✅ useState Deep Dive (30 min) - Completed          │
│ 🔄 useEffect Patterns (45 min) - In Progress        │
│ ⏳ Custom Hooks Workshop (30 min) - Upcoming        │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 💬 CHAT & NOTES                              [📝 📤]│
│                                                     │
│ Sarah: Great explanation of useEffect cleanup! 🎉   │
│ Mike: Can you share that code example?              │
│ Alex: Sure! I'll add it to our Party Stash         │
│ Emma: Question about dependency arrays...           │
│                                                     │
│ 📝 SHARED NOTES (Live collaborative editing)        │
│ • useEffect runs after every render by default     │
│ • Dependency array controls when effect runs       │
│ • Empty array [] = run once (componentDidMount)    │
│ • Cleanup function prevents memory leaks           │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🎮 MEETING CONTROLS                                 │
│                                                     │
│ [🎤 Mute] [🎥 Video] [📺 Share] [📝 Notes] [⚙️]     │
│                                                     │
│ [📤 Share to Stash] [📅 Schedule Follow-up] [🔚 End]│
└─────────────────────────────────────────────────────┘
```

## 6. Party Discovery & Join Interface

```
┌─────────────────────────────────────────────────────┐
│ ☰ RogueLearn > Find Parties              🔔 👤 ⚙️  │
├─────────────────────────────────────────────────────┤
│                                                     │
│ 🔍 DISCOVER LEARNING PARTIES                        │
│                                                     │
│ 🔍 [Search parties...] 📊 Filter ▼ 🎯 [My Interests]│
│                                                     │
│ Filters: Focus Area ▼ | Size ▼ | Activity ▼ | More ▼│
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🌟 RECOMMENDED FOR YOU                              │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🎯 JavaScript Mastery Squad                     │ │
│ │ 👥 5/6 members • 🎯 Frontend, Backend           │ │
│ │ 📈 Very Active • ⭐ 4.8/5 rating               │ │
│ │ "Focused on React, Node.js, modern JS..."      │ │
│ │ 🏆 Recent: Completed React Fundamentals Quest   │ │
│ │                              [Request Join] ───►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🤖 AI/ML Study Group                            │ │
│ │ 👥 3/8 members • 🎯 AI/ML, Python              │ │
│ │ 📈 Active • ⭐ 4.6/5 rating                    │ │
│ │ "Deep learning, neural networks, TensorFlow"   │ │
│ │ 🏆 Recent: Started Deep Learning Fundamentals   │ │
│ │                              [Request Join] ───►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
├─────────────────────────────────────────────────────┤
│ 🔥 MOST ACTIVE PARTIES                              │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 🚀 Full-Stack Developers United                 │ │
│ │ 👥 8/8 members • 🎯 Full-Stack                  │ │
│ │ 📈 Extremely Active • ⭐ 4.9/5 rating          │ │
│ │ "Complete full-stack development journey"      │ │
│ │ 🔒 Invite Only • 📅 3 meetings this week        │ │
│ │                              [View Details] ───►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ ┌─────────────────────────────────────────────────┐ │
│ │ 📱 Mobile Dev Enthusiasts                       │ │
│ │ 👥 6/8 members • 🎯 Mobile, React Native       │ │
│ │ 📈 Very Active • ⭐ 4.7/5 rating               │ │
│ │ "iOS, Android, cross-platform development"     │ │
│ │ 🏆 Recent: Built 3 mobile apps together        │ │
│ │                              [Request Join] ───►│ │
│ └─────────────────────────────────────────────────┘ │
│                                                     │
│ [🎉 Create Your Own Party]                          │
└─────────────────────────────────────────────────────┘
```