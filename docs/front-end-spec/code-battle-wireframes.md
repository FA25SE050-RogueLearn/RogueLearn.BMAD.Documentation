# Code Battle Competition Wireframes - Phase 3

## 1. Code Battle Event Creation Interface

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ ⚔️ Create Code Battle Event                                      [🔙 Back] [💾]    │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 📋 EVENT DETAILS                                                                    │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Event Name: [Winter Code Championship 2024_________________________]            │ │
│ │                                                                                 │ │
│ │ Description:                                                                    │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Join us for the ultimate coding competition! Test your algorithmic skills   │ │ │
│ │ │ against fellow students in real-time coding challenges. Prizes for top     │ │ │
│ │ │ performers and recognition for creative solutions.                          │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ Event Type: ● Guild Free-for-All  ○ Practice Battle  ○ Team Competition        │ │
│ │                                                                                 │ │
│ │ 📅 Schedule:                                                                    │ │
│ │ Registration Opens: [Dec 20, 2024] [09:00 AM]                                  │ │
│ │ Registration Closes: [Jan 15, 2025] [11:59 PM]                                 │ │
│ │ Event Date: [Jan 20, 2025] [02:00 PM] Duration: [3 hours]                      │ │
│ │                                                                                 │ │
│ │ 👥 Participants:                                                                │ │
│ │ Max Participants: [64] Min Level: [8] Target Audience: [All Guilds ▼]          │ │
│ │                                                                                 │ │
│ │ 🏆 Prizes & Recognition:                                                        │ │
│ │ 1st Place: [5000 XP + Champion Badge + Certificate]                            │ │
│ │ 2nd Place: [3000 XP + Runner-up Badge]                                         │ │
│ │ 3rd Place: [2000 XP + Bronze Badge]                                            │ │
│ │ Participation: [500 XP + Participant Badge]                                    │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎯 COMPETITION FORMAT                                                               │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Event Structure:                                                                │ │
│ │ ● Free-for-All Battle  ○ Timed Challenges  ○ Progressive Difficulty            │ │
│ │                                                                                 │ │
│ │ Battle Format:                                                                  │ │
│ │ ● Individual Competition  ○ Guild vs Guild  ○ Mixed Teams                      │ │
│ │                                                                                 │ │
│ │ Event Configuration:                                                            │ │
│ │ Event Duration: [3 hours] Problem Sets: [12 problems]                          │ │
│ │ Submission Limit: [Unlimited] Time per Problem: [15 minutes]                   │ │
│ │                                                                                 │ │
│ │ Problem Difficulty Distribution:                                                │ │
│ │ Easy (4 problems): ████░░░░░░░░                                                 │ │
│ │ Medium (5 problems): ██████░░░░░░                                               │ │
│ │ Hard (3 problems): ████████░░░░                                                 │ │
│ │                                                                                 │ │
│ │ Scoring System:                                                                 │ │
│ │ ☑ Correctness (50%) ☑ Speed (30%) ☑ Code Quality (20%)                        │ │
│ │ ☑ Test case coverage ☑ Memory efficiency ☑ Guild bonus points                 │ │
│ │                                                                                 │ │
│ │ Guild Participation:                                                            │ │
│ │ Min Guild Members: [3] Max per Guild: [20] Guild Bonus: [10% extra points]     │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 💻 TECHNICAL SETTINGS                                                               │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Allowed Languages:                                                              │ │
│ │ ☑ Python ☑ Java ☑ C++ ☑ JavaScript ☑ C# ☐ Go ☐ Rust ☐ Kotlin                │ │
│ │                                                                                 │ │
│ │ Problem Categories:                                                             │ │
│ │ ☑ Algorithms ☑ Data Structures ☑ Dynamic Programming ☑ Graph Theory          │ │
│ │ ☑ String Processing ☑ Mathematics ☐ Machine Learning ☐ System Design         │ │
│ │                                                                                 │ │
│ │ Code Execution:                                                                 │ │
│ │ Time Limit: [5 seconds] Memory Limit: [256 MB] Max File Size: [1 MB]           │ │
│ │                                                                                 │ │
│ │ Testing Environment:                                                            │ │
│ │ ☑ Real-time compilation ☑ Custom test cases ☑ Hidden test cases               │ │
│ │ ☑ Performance metrics ☑ Code similarity detection                              │ │
│ │                                                                                 │ │
│ │ IDE Features:                                                                   │ │
│ │ ☑ Syntax highlighting ☑ Auto-completion ☑ Debugging tools                     │ │
│ │ ☑ Code templates ☐ AI assistance ☑ Collaborative editing                      │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎥 STREAMING & SPECTATOR SETTINGS                                                   │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Live Streaming:                                                                 │ │
│ │ ☑ Enable live streaming ☑ Public viewing ☑ Commentary mode                    │ │
│ │ ☑ Replay recording ☑ Highlight generation                                      │ │
│ │                                                                                 │ │
│ │ Spectator Features:                                                             │ │
│ │ ☑ Real-time leaderboard ☑ Code viewing (delayed) ☑ Chat interaction           │ │
│ │ ☑ Problem analysis ☑ Statistics dashboard ☑ Prediction polls                  │ │
│ │                                                                                 │ │
│ │ Privacy Settings:                                                               │ │
│ │ Code Visibility: [Delayed 5 min ▼] Chat Moderation: [Auto + Manual ▼]         │ │
│ │ Participant Anonymity: ○ Full Names ● Usernames ○ Anonymous                   │ │
│ │                                                                                 │ │
│ │ Commentary Team:                                                                │ │
│ │ Main Commentator: [Prof. Sarah Kim] Co-Commentator: [David Park]               │ │
│ │ Technical Analyst: [Alex Chen] [➕ Add More]                                    │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🚀 ACTIONS                                                                          │
│                                                                                     │
│ [📝 Save Draft] [👁️ Preview Event] [📤 Publish Event] [🎯 Generate Problems]       │
│ [📧 Invite Participants] [📊 Setup Analytics] [⚙️ Advanced Settings]               │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 2. Guild & Individual Leaderboard Interface

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 🏆 Winter Code Championship 2024 - Free-for-All Event         [🔙 Back] [⚙️]       │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 📊 TOURNAMENT OVERVIEW                                                              │
│                                                                                     │
│ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────────┐ │
│ │ 📈 CURRENT STATUS       │ │ ⏰ SCHEDULE             │ │ 👥 PARTICIPANTS         │ │
│ │                         │ │                         │ │                         │ │
│ │ Round: Quarterfinals    │ │ Current: Round 5/6      │ │ Registered: 64/64       │ │
│ │ Active Battles: 4       │ │ Started: 2:00 PM        │ │ Active: 8               │ │
│ │ Completed: 56           │ │ Next Round: 4:30 PM     │ │ Eliminated: 56          │ │
│ │ Status: 🟢 Live         │ │ Est. Finish: 6:45 PM    │ │ Spectators: 234         │ │
│ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🏆 GUILD LEADERBOARD                                                                │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🔍 [Search Guilds...] 📊 View: ● Overall ○ Monthly ○ Weekly    🔄 Refresh      │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Rank │ Guild Name        │ Members │ Total Points │ Avg Score │ Battles Won │ │ │
│ │ │──────┼───────────────────┼─────────┼──────────────┼───────────┼─────────────│ │ │
│ │ │  🥇1  │ Code Crusaders   │   18    │   15,420     │   856.7   │     47      │ │ │
│ │ │  🥈2  │ Debug Dragons    │   15    │   14,890     │   992.7   │     42      │ │ │
│ │ │  🥉3  │ Syntax Samurai   │   22    │   14,230     │   646.8   │     39      │ │ │
│ │ │   4   │ Logic Lords      │   12    │   13,560     │  1130.0   │     35      │ │ │
│ │ │   5   │ Binary Beasts    │   20    │   12,890     │   644.5   │     33      │ │ │
│ │ │   6   │ Algorithm Aces   │   16    │   12,340     │   771.3   │     31      │ │ │
│ │ │   7   │ Function Force   │   14    │   11,780     │   841.4   │     28      │ │ │
│ │ │   8   │ Variable Vikings │   19    │   11,220     │   590.5   │     26      │ │ │
│ │ │   9   │ Loop Legends     │   13    │   10,890     │   837.7   │     24      │ │ │
│ │ │  10   │ Recursion Rebels │   17    │   10,340     │   608.2   │     22      │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ Guild Performance Metrics:                                                      │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 📈 Top Performing Guild: Code Crusaders                                    │ │ │
│ │ │ 🎯 Most Active Guild: Syntax Samurai (22 active members)                   │ │ │
│ │ │ ⚡ Highest Avg Score: Logic Lords (1130.0 points/member)                   │ │ │
│ │ │ 🏅 Most Battles Won: Code Crusaders (47 victories)                         │ │ │
│ │ │ 📊 Total Active Guilds: 47 | Total Battles This Month: 234                 │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ [View Guild Details] [Join Guild] [Create New Guild] [Guild Battle History]    │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 👤 INDIVIDUAL LEADERBOARD                                                           │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🔍 [Search Players...] 📊 View: ● Overall ○ Guild Only ○ Friends   🔄 Refresh  │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Rank │ Player Name      │ Guild           │ Total Points │ Battles │ Win Rate│ │ │
│ │ │──────┼──────────────────┼─────────────────┼──────────────┼─────────┼─────────│ │ │
│ │ │  🥇1  │ CodeMaster_2024  │ Code Crusaders │    2,340     │   156   │  87.2%  │ │ │
│ │ │  🥈2  │ DebugNinja       │ Debug Dragons  │    2,290     │   142   │  84.5%  │ │ │
│ │ │  🥉3  │ AlgoWizard       │ Logic Lords    │    2,180     │   134   │  89.6%  │ │ │
│ │ │   4   │ SyntaxSensei     │ Syntax Samurai │    2,120     │   167   │  78.4%  │ │ │
│ │ │   5   │ BinaryBeast      │ Binary Beasts  │    2,050     │   129   │  82.9%  │ │ │
│ │ │   6   │ FunctionFury     │ Function Force │    1,980     │   145   │  76.6%  │ │ │
│ │ │   7   │ LoopLegend       │ Loop Legends   │    1,920     │   138   │  81.2%  │ │ │
│ │ │   8   │ RecursiveRogue   │ Recursion Rebels│    1,860     │   124   │  85.5%  │ │ │
│ │ │   9   │ VariableViper    │ Variable Vikings│    1,790     │   151   │  73.5%  │ │ │
│ │ │  10   │ AlgorithmAce     │ Algorithm Aces │    1,720     │   118   │  88.1%  │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ Player Performance Metrics:                                                     │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 🏆 Top Player: CodeMaster_2024 (2,340 points)                              │ │ │
│ │ │ 🎯 Highest Win Rate: AlgoWizard (89.6%)                                    │ │ │
│ │ │ ⚡ Most Active: SyntaxSensei (167 battles)                                 │ │ │
│ │ │ 🔥 Current Streak Leader: CodeMaster_2024 (12 wins)                        │ │ │
│ │ │ 📊 Total Active Players: 1,247 | Battles Today: 89                         │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ [View Player Profile] [Challenge Player] [Add Friend] [Battle History]         │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🔥 LIVE FREE-FOR-ALL BATTLE                                                        │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🎯 Current Problem: "Binary Tree Maximum Path Sum"          ⏰ 18:45 remaining  │ │
│ │ Difficulty: Hard | Points: 150 | Participants: 47 guild members                │ │
│ │                                                                                 │ │
│ │ 🏆 LIVE RANKINGS (Top 10)                                                      │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ Pos │ Player           │ Guild           │ Status      │ Score │ Time      │ │ │
│ │ │─────┼──────────────────┼─────────────────┼─────────────┼───────┼───────────│ │ │
│ │ │  1  │ CodeMaster_2024  │ Code Crusaders │ ✅ Solved   │  150  │ 12:34     │ │ │
│ │ │  2  │ AlgoWizard       │ Logic Lords    │ ✅ Solved   │  148  │ 13:21     │ │ │
│ │ │  3  │ DebugNinja       │ Debug Dragons  │ ✅ Solved   │  145  │ 14:07     │ │ │
│ │ │  4  │ SyntaxSensei     │ Syntax Samurai │ 🔄 Testing  │   -   │ 16:42     │ │ │
│ │ │  5  │ BinaryBeast      │ Binary Beasts  │ 🔄 Testing  │   -   │ 17:15     │ │ │
│ │ │  6  │ FunctionFury     │ Function Force │ 💻 Coding   │   -   │ 18:03     │ │ │
│ │ │  7  │ LoopLegend       │ Loop Legends   │ 💻 Coding   │   -   │ 18:12     │ │ │
│ │ │  8  │ RecursiveRogue   │ Recursion Rebels│ 💻 Coding   │   -   │ 18:25     │ │ │
│ │ │  9  │ VariableViper    │ Variable Vikings│ 💻 Coding   │   -   │ 18:33     │ │ │
│ │ │ 10  │ AlgorithmAce     │ Algorithm Aces │ 💻 Coding   │   -   │ 18:41     │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ 📊 GUILD BATTLE STATS                                                          │ │
│ │ Code Crusaders: ████████████████████░ 5 solved, 3 testing, 10 coding          │ │
│ │ Debug Dragons:  ████████████████░░░░░ 4 solved, 2 testing, 9 coding           │ │
│ │ Logic Lords:    ████████████████░░░░░ 4 solved, 1 testing, 7 coding           │ │
│ │ Syntax Samurai: ████████████░░░░░░░░░ 3 solved, 4 testing, 15 coding          │ │
│ │                                                                                 │ │
│ │ [👁️ Watch Top Players] [📊 Full Rankings] [💬 Guild Chat] [🎥 Replay Mode]     │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🥊 QUARTERFINAL 2                                          ⏰ 23:45 remaining   │ │
│ │ Emma Davis (Level 13) 🆚 Sophie Chen (Level 12)                                │ │
│ │ Problem: "Longest Increasing Subsequence"                                       │ │
│ │                                                                                 │ │
│ │ Emma Davis:   ██████████████████░░ 90% ✅ 4/5 test cases  ⏱️ 19:56            │ │
│ │ Sophie Chen:  ████████████░░░░░░░░ 60% ✅ 2/5 test cases  ⏱️ 25:12            │ │
│ │                                                                                 │ │
│ │ [👁️ Watch Live] [📊 Details] [💬 Chat (89)]                                    │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📊 LEADERBOARD & STATISTICS                                                         │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🏆 TOP PERFORMERS                                                               │ │
│ │                                                                                 │ │
│ │ Rank │ Player        │ Wins │ Avg Time │ Success Rate │ Points │ Status        │ │
│ │ ──────┼───────────────┼──────┼──────────┼──────────────┼────────┼─────────────  │ │
│ │  1   │ Alex Chen     │  5   │  16:23   │    100%      │  2,450 │ 🔴 Fighting  │ │
│ │  2   │ Emma Davis    │  5   │  18:45   │    100%      │  2,380 │ 🔴 Fighting  │ │
│ │  3   │ David Park    │  4   │  19:12   │     80%      │  1,920 │ 🔴 Fighting  │ │
│ │  4   │ Sophie Chen   │  4   │  22:34   │     80%      │  1,840 │ 🔴 Fighting  │ │
│ │  5   │ Lisa Wang     │  4   │  20:56   │     80%      │  1,760 │ ❌ Eliminated │ │
│ │  6   │ Sarah Kim     │  3   │  21:23   │     75%      │  1,350 │ ❌ Eliminated │ │
│ │  7   │ Maria Lopez   │  3   │  23:45   │     75%      │  1,290 │ ❌ Eliminated │ │
│ │  8   │ Kevin Lee     │  3   │  25:12   │     75%      │  1,230 │ ❌ Eliminated │ │
│ │                                                                                 │ │
│ │ [📊 Full Leaderboard] [📈 Performance Trends] [🏆 Hall of Fame]                 │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 EVENT CONTROLS                                                                   │
│                                                                                     │
│ [⏸️ Pause Event] [🔄 Reset Rankings] [📊 Generate Report]                          │
│ [📧 Send Updates] [🎥 Manage Stream] [⚙️ Event Settings]                           │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 3. Live Free-for-All Battle Interface

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ ⚔️ Live Free-for-All Battle - Winter Code Championship 2024   [🔙 Exit] [⚙️]      │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ ⏰ BATTLE STATUS                                                                    │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Problem: "Binary Tree Maximum Path Sum"                    ⏰ 23:45 remaining   │ │
│ │ Difficulty: Hard 🔴  Category: Trees & Graphs  Points: 500                     │ │
│ │                                                                                 │ │
│ │ 🏆 TOP 3 LIVE PERFORMERS                                                       │ │
│ │ 🥇 Alex Chen (Level 15)     Progress: ████████████████████░ 95%  ⏱️ 18:23     │ │
│ │ 🥈 Emma Davis (Level 13)    Progress: ████████████████░░░░ 85%   ⏱️ 19:56     │ │
│ │ 🥉 David Park (Level 14)    Progress: ████████████████░░░░░ 80%  ⏱️ 21:07     │ │
│ │                                                                                 │ │
│ │ 👥 Active Participants: 47/64  🏃‍♂️ Still Coding: 32  ✅ Completed: 15        │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📝 PROBLEM STATEMENT                                                                │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🎯 Binary Tree Maximum Path Sum                                                 │ │
│ │                                                                                 │ │
│ │ A path in a binary tree is a sequence of nodes where each pair of adjacent     │ │
│ │ nodes in the sequence has an edge connecting them. A node can only appear in   │ │
│ │ the sequence at most once. Note that the path does not need to pass through    │ │
│ │ the root.                                                                       │ │
│ │                                                                                 │ │
│ │ The path sum of a path is the sum of the node's values in the path.            │ │
│ │                                                                                 │ │
│ │ Given the root of a binary tree, return the maximum path sum of any non-empty  │ │
│ │ path.                                                                           │ │
│ │                                                                                 │ │
│ │ Example 1:                                                                      │ │
│ │     1                                                                           │ │
│ │    / \                                                                          │ │
│ │   2   3                                                                         │ │
│ │ Input: root = [1,2,3]                                                           │ │
│ │ Output: 6                                                                       │ │
│ │ Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6 │ │
│ │                                                                                 │ │
│ │ Constraints:                                                                    │ │
│ │ • The number of nodes in the tree is in the range [1, 3 * 10^4]               │ │
│ │ • -1000 <= Node.val <= 1000                                                    │ │
│ │                                                                                 │ │
│ │ [📋 Copy Problem] [🔗 Similar Problems] [💡 Hints (0/3 used)]                  │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 💻 SPLIT CODE VIEW                                                                  │
│                                                                                     │
│ ┌─────────────────────────────────────┬───────────────────────────────────────────┐ │
│ │ 👤 Alex Chen - Python              │ 👤 David Park - Java                     │ │
│ │ ┌─────────────────────────────────┐ │ ┌─────────────────────────────────────┐ │ │
│ │ │ class TreeNode:                 │ │ │ class TreeNode {                    │ │ │
│ │ │     def __init__(self, val=0):  │ │ │     int val;                        │ │ │
│ │ │         self.val = val          │ │ │     TreeNode left;                  │ │ │
│ │ │         self.left = None        │ │ │     TreeNode right;                 │ │ │
│ │ │         self.right = None       │ │ │     TreeNode() {}                   │ │ │
│ │ │                                 │ │ │     TreeNode(int val) {             │ │ │
│ │ │ class Solution:                 │ │ │         this.val = val;             │ │ │
│ │ │     def maxPathSum(self, root): │ │ │     }                               │ │ │
│ │ │         self.max_sum = float(   │ │ │ }                                   │ │ │
│ │ │             '-inf')             │ │ │                                     │ │ │
│ │ │                                 │ │ │ class Solution {                    │ │ │
│ │ │         def helper(node):       │ │ │     private int maxSum = Integer.   │ │ │
│ │ │             if not node:        │ │ │         MIN_VALUE;                  │ │ │
│ │ │                 return 0        │ │ │                                     │ │ │
│ │ │                                 │ │ │     public int maxPathSum(TreeNode │ │ │
│ │ │             left = max(0,       │ │ │         root) {                     │ │ │
│ │ │                 helper(node.    │ │ │         helper(root);               │ │ │
│ │ │                 left))          │ │ │         return maxSum;              │ │ │
│ │ │             right = max(0,      │ │ │     }                               │ │ │
│ │ │                 helper(node.    │ │ │                                     │ │ │
│ │ │                 right))         │ │ │     private int helper(TreeNode    │ │ │
│ │ │                                 │ │ │         node) {                     │ │ │
│ │ │             current_max = node. │ │ │         if (node == null) return 0; │ │ │
│ │ │                 val + left +    │ │ │                                     │ │ │
│ │ │                 right           │ │ │         int left = Math.max(0,      │ │ │
│ │ │             self.max_sum = max( │ │ │             helper(node.left));     │ │ │
│ │ │                 self.max_sum,   │ │ │         int right = Math.max(0,     │ │ │
│ │ │                 current_max)    │ │ │             helper(node.right));    │ │ │
│ │ │                                 │ │ │                                     │ │ │
│ │ │             return node.val +   │ │ │         maxSum = Math.max(maxSum,   │ │ │
│ │ │                 max(left, right)│ │ │             node.val + left +       │ │ │
│ │ │                                 │ │ │             right);                 │ │ │
│ │ │         helper(root)            │ │ │                                     │ │ │
│ │ │         return self.max_sum     │ │ │         return node.val + Math.max( │ │ │
│ │ │                                 │ │ │             left, right);           │ │ │
│ │ │ # Status: ✅ Compiling...       │ │ │     }                               │ │ │
│ │ │ # Test 4/5 passed              │ │ │ }                                   │ │ │
│ │ │                                 │ │ │ // Status: ❌ Runtime Error        │ │ │
│ │ │                                 │ │ │ // Test 3/5 passed                │ │ │
│ │ └─────────────────────────────────┘ │ └─────────────────────────────────────┘ │ │
│ │                                     │                                       │ │
│ │ [▶️ Run] [📤 Submit] [🔄 Reset]      │ [▶️ Run] [📤 Submit] [🔄 Reset]       │ │
│ └─────────────────────────────────────┴───────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📊 REAL-TIME METRICS                                                                │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📈 PERFORMANCE COMPARISON                                                       │ │
│ │                                                                                 │ │
│ │ Metric           │ Alex Chen        │ David Park       │ Advantage            │ │
│ │ ─────────────────┼──────────────────┼──────────────────┼──────────────────    │ │
│ │ Speed            │ 18:23 ⚡         │ 21:07            │ Alex (+2:44)         │ │
│ │ Accuracy         │ 4/5 tests ✅     │ 3/5 tests ⚠️     │ Alex (+1 test)       │ │
│ │ Code Quality     │ 92/100 🟢        │ 87/100 🟡        │ Alex (+5 pts)        │ │
│ │ Submissions      │ 2 attempts       │ 3 attempts       │ Alex (-1 attempt)    │ │
│ │ Memory Usage     │ 45.2 MB          │ 52.1 MB          │ Alex (-6.9 MB)       │ │
│ │ Runtime          │ 68ms             │ Error            │ Alex (Working)       │ │
│ │                                                                                 │ │
│ │ 🏆 Current Leader: Alex Chen (+15 points)                                      │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 💬 LIVE COMMENTARY & CHAT                                                           │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🎙️ COMMENTARY                                   💬 SPECTATOR CHAT (127 online) │ │
│ │                                                                                 │ │
│ │ Prof. Sarah Kim: "Alex is showing excellent    │ CodingFan23: Alex's approach   │ │
│ │ recursive thinking here. Notice how he's       │ is so clean! 🔥                │ │
│ │ handling the edge cases first."                │                                 │ │
│ │                                                │ DevMaster: David's debugging    │ │
│ │ David Park (Analyst): "David is struggling     │ skills are impressive though    │ │
│ │ with a null pointer exception. This is a       │                                 │ │
│ │ common pitfall in tree problems."              │ AlgoQueen: Who do you think    │ │
│ │                                                │ will win? 🤔                   │ │
│ │ Prof. Sarah Kim: "Both solutions use the       │                                 │ │
│ │ same algorithmic approach - post-order         │ TreeLover: Alex has this! His   │ │
│ │ traversal with memoization. The difference     │ solution is almost perfect      │ │
│ │ is in implementation details."                 │                                 │ │
│ │                                                │ JavaDev: Come on David! Fix     │ │
│ │ [🔇 Mute] [⚙️ Commentary Settings]             │ that NPE! 💪                   │ │
│ │                                                │                                 │ │
│ │                                                │ [Type message...] [📤 Send]     │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 SPECTATOR CONTROLS                                                               │
│                                                                                     │
│ [👁️ Focus Alex] [👁️ Focus David] [⚖️ Split View] [📊 Show Metrics]                 │
│ [🎥 Record Highlight] [📱 Share Battle] [🔔 Notifications] [⚙️ Settings]            │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 4. Spectator Mode Interface

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 👁️ Spectator Mode - Winter Code Championship 2024            [🔙 Back] [⚙️]       │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 🎥 LIVE STREAM DASHBOARD                                                            │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📺 MAIN STREAM                                          👥 234 viewers online   │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │                                                                             │ │ │
│ │ │                        🔴 LIVE - Free-for-All Battle                       │ │ │
│ │ │                                                                             │ │ │
│ │ │                     Winter Code Championship 2024                          │ │ │
│ │ │                                                                             │ │ │
│ │ │                    [▶️ PLAY] [🔊 Audio] [⚙️ Quality]                        │ │ │
│ │ │                                                                             │ │ │
│ │ │                        ⏰ 23:45 remaining                                   │ │ │
│ │ │                                                                             │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ Stream Quality: [1080p ▼] [🔊 100%] [📱 Mobile View] [🎥 Picture-in-Picture]   │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🏆 LIVE EVENT STATUS                                                                │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🔥 GUILD PARTICIPATION                                                          │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────┐ │ │
│ │ │ 🏛️ DRAGON CODERS        │ │ 🌟 STAR DEVELOPERS      │ │ ⚡ CODE LIGHTNING    │ │ │
│ │ │ Active: 12/15 members   │ │ Active: 8/12 members    │ │ Active: 10/18 members│ │ │
│ │ │ 👥 89 viewers           │ │ 👥 67 viewers           │ │ 👥 78 viewers       │ │ │
│ │ │ 🏆 Leading: Alex Chen   │ │ 🏆 Leading: Emma Davis  │ │ 🏆 Leading: Mike Kim │ │ │
│ │ │ [👁️ Watch] [📊 Stats]   │ │ [👁️ Watch] [📊 Stats]   │ │ [👁️ Watch] [📊 Stats]│ │ │
│ │ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────┘ │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────┐ │ │
│ │ │ 🚀 ROCKET PROGRAMMERS   │ │ 🎯 PRECISION CODERS     │ │ 🔥 FIRE ALGORITHMS  │ │ │
│ │ │ Active: 7/10 members    │ │ Active: 9/14 members    │ │ Active: 6/8 members │ │ │
│ │ │ 👥 45 viewers           │ │ 👥 52 viewers           │ │ 👥 38 viewers       │ │ │
│ │ │ 🏆 Leading: Sarah Kim   │ │ 🏆 Leading: David Park  │ │ 🏆 Leading: Lisa W. │ │ │
│ │ │ [👁️ Watch] [📊 Stats]   │ │ [👁️ Watch] [📊 Stats]   │ │ [👁️ Watch] [📊 Stats]│ │ │
│ │ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────┘ │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📊 REAL-TIME LEADERBOARD                                                            │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🏆 TOP 8 REMAINING COMPETITORS                                                  │ │
│ │                                                                                 │ │
│ │ Rank │ Player        │ Status      │ Current Battle    │ Win Rate │ Avg Time   │ │
│ │ ──────┼───────────────┼─────────────┼───────────────────┼──────────┼──────────  │ │
│ │  1   │ Alex Chen     │ 🔴 Fighting │ vs David Park     │   100%   │  16:23     │ │
│ │  2   │ Emma Davis    │ 🔴 Fighting │ vs Sophie Chen    │   100%   │  18:45     │ │
│ │  3   │ David Park    │ 🔴 Fighting │ vs Alex Chen      │    80%   │  19:12     │ │
│ │  4   │ Sophie Chen   │ 🔴 Fighting │ vs Emma Davis     │    80%   │  22:34     │ │
│ │  5   │ Mike Johnson  │ ⏳ Waiting  │ QF3 (15 min)      │    75%   │  20:45     │ │
│ │  6   │ Lisa Park     │ ⏳ Waiting  │ QF3 (15 min)      │    75%   │  21:23     │ │
│ │  7   │ Ryan Chen     │ ⏳ Waiting  │ QF4 (15 min)      │    75%   │  23:12     │ │
│ │  8   │ Amy Wilson    │ ⏳ Waiting  │ QF4 (15 min)      │    75%   │  24:56     │ │
│ │                                                                                 │ │
│ │ [📊 Full Statistics] [📈 Performance Trends] [🎯 Predictions]                   │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 💬 COMMUNITY INTERACTION                                                            │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🗳️ LIVE PREDICTIONS                              💬 GLOBAL CHAT (234 online)   │ │
│ │                                                                                 │ │
│ │ Who will be the top performer?                  │ CodingMaster: Alex's solution │ │
│ │ ● Alex Chen (45%) ████████████████░░░░░         │ is so elegant! 🔥            │ │
│ │ ● Emma Davis (32%) ████████████░░░░░░░░         │                               │ │
│ │ ● David Park (15%) ██████░░░░░░░░░░░░░░         │ AlgoFan: Emma is catching up  │ │
│ │ ● Mike Kim (8%) ████░░░░░░░░░░░░░░░░░░░         │ fast! 💪                     │ │
│ │                                                 │                               │ │
│ │ Which guild will dominate?                      │ TreeExpert: This problem is   │ │
│ │ ● Dragon Coders (38%) ███████████████░░░       │ a classic! Love seeing        │ │
│ │ ● Star Developers (28%) ██████████░░░░░░       │ different approaches          │ │
│ │ ● Code Lightning (20%) ████████░░░░░░░░        │                               │ │
│ │ ● Rocket Programmers (14%) ██████░░░░░░        │ DevStudent: Dragon Coders     │ │
│ │                                                 │ looking strong! 🐉           │ │
│ │ [🗳️ Vote] [📊 Results] [🎁 Rewards]             │                               │ │
│ │                                                 │ Moderator: 15 minutes left!  │ │
│ │ 🎁 PREDICTION REWARDS                           │ The competition is heating up │ │
│ │ Correct top performer: +100 XP                  │                               │ │
│ │ Correct guild winner: +150 XP                   │ CodeNinja: Go Dragon Coders! │ │
│ │ Perfect predictions: +500 XP + Badge           │ 🚀                           │ │
│ │                                                 │                               │ │
│ │                                                 │ [Type message...] [📤 Send]   │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📈 ANALYTICS & INSIGHTS                                                             │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📊 BATTLE ANALYTICS                                                             │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────┐ │ │
│ │ │ 🎯 PROBLEM DIFFICULTY   │ │ 💻 LANGUAGE USAGE       │ │ ⏱️ SOLVING PATTERNS  │ │ │
│ │ │                         │ │                         │ │                     │ │ │
│ │ │ Easy: 15% ████░░░░░░░░░ │ │ Python: 45% ████████░░░ │ │ Fast Solvers: 25%   │ │ │
│ │ │ Medium: 45% ████████░░░ │ │ Java: 30% ██████░░░░░░░ │ │ Steady: 60%         │ │ │
│ │ │ Hard: 35% ███████░░░░░░ │ │ C++: 20% ████░░░░░░░░░░ │ │ Last Minute: 15%    │ │ │
│ │ │ Expert: 5% █░░░░░░░░░░░ │ │ JavaScript: 5% █░░░░░░░ │ │                     │ │ │
│ │ │                         │ │                         │ │ Avg Solve: 22:34    │ │ │
│ │ │ [📊 Details] ──────────►│ │ [📊 Details] ──────────►│ │ [📊 Details] ──────►│ │ │
│ │ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────┘ │ │
│ │                                                                                 │ │
│ │ 🏆 FREE-FOR-ALL EVENT INSIGHTS                                                  │ │
│ │ • Most popular algorithm: Dynamic Programming (67% of solutions)               │ │
│ │ • Highest success rate: Tree traversal problems (89%)                          │ │
│ │ • Biggest upset: Sophie Chen defeating Kevin Lee (Level 15)                    │ │
│ │ • Closest battle: Maria Lopez vs Chris Brown (2-second difference)             │ │
│ │                                                                                 │ │
│ │ [📊 Full Analytics Dashboard] [📈 Historical Comparisons] [🎯 Player Profiles]  │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 SPECTATOR FEATURES                                                               │
│                                                                                     │
│ [🔔 Battle Notifications] [📱 Mobile App] [🎥 Replay Center] [📊 Statistics Hub]    │
│ [🏆 Hall of Fame] [📚 Problem Archive] [👥 Follow Players] [⚙️ Preferences]        │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 5. Post-Event Results & Analytics

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ 🏆 Event Results: Winter Code Championship 2024 - Final Rankings [🔙 Back] [📊]   │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 🎉 EVENT OUTCOME                                                                    │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │                            🏆 TOP PERFORMERS                                    │ │
│ │                                                                                 │ │
│ │ 🥇 1st Place: Alex Chen (Dragon Coders)     Score: 485/500  ⏱️ 18:23          │ │
│ │ 🥈 2nd Place: Emma Davis (Star Developers)  Score: 478/500  ⏱️ 19:56          │ │
│ │ 🥉 3rd Place: Mike Kim (Code Lightning)     Score: 465/500  ⏱️ 20:12          │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────┐                   ┌─────────────────────────┐       │ │
│ │ │ 🏆 GUILD CHAMPION       │                   │ 🏅 INDIVIDUAL WINNER    │       │ │
│ │ │ Dragon Coders           │                   │ Alex Chen               │       │ │
│ │ │ Total Score: 2,847      │                   │ Final Score: 485/500    │       │ │
│ │ │ Active Members: 12/15   │                   │ Level: 15 → 16          │       │ │
│ │ │ Avg Performance: 89%    │                   │ XP Earned: +750         │       │ │
│ │ │ Top Performer: Alex C.  │                   │ New Badge: Champion     │       │ │
│ │ │                         │                   │                         │       │ │
│ │ │ Guild Reward: +2000 XP  │                   │ Individual Reward:      │       │ │
│ │ │ Special Badge: Dominion │                   │ +500 XP + Crown Badge   │       │ │
│ │ └─────────────────────────┘                   └─────────────────────────┘       │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 📊 DETAILED PERFORMANCE ANALYSIS                                                    │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📈 TOP 10 FINAL RANKINGS                                                        │ │
│ │                                                                                 │ │
│ │ Rank │ Player           │ Guild            │ Score   │ Time    │ Quality │ XP   │ │
│ │ ─────┼──────────────────┼──────────────────┼─────────┼─────────┼─────────┼───── │ │
│ │ 🥇 1 │ Alex Chen        │ Dragon Coders    │ 485/500 │ 18:23   │ 92%     │ +750 │ │
│ │ 🥈 2 │ Emma Davis       │ Star Developers  │ 478/500 │ 19:56   │ 89%     │ +600 │ │
│ │ 🥉 3 │ Mike Kim         │ Code Lightning   │ 465/500 │ 20:12   │ 91%     │ +500 │ │
│ │   4  │ Sarah Johnson    │ Dragon Coders    │ 452/500 │ 21:45   │ 88%     │ +400 │ │
│ │   5  │ David Park       │ Byte Warriors    │ 448/500 │ 22:03   │ 85%     │ +350 │ │
│ │   6  │ Lisa Wong        │ Star Developers  │ 435/500 │ 23:12   │ 87%     │ +300 │ │
│ │   7  │ Tom Rodriguez    │ Code Lightning   │ 428/500 │ 24:56   │ 83%     │ +250 │ │
│ │   8  │ Amy Foster       │ Dragon Coders    │ 420/500 │ 25:34   │ 86%     │ +200 │ │
│ │   9  │ Jake Miller      │ Byte Warriors    │ 415/500 │ 26:18   │ 82%     │ +150 │ │
│ │  10  │ Nina Patel       │ Star Developers  │ 408/500 │ 27:45   │ 84%     │ +100 │ │
│ │                                                                                 │ │
│ │ 🎯 EVENT STATISTICS                                                             │ │
│ │ • Total Participants: 47 students across 8 guilds                              │ │
│ │ • Average Score: 312/500 (62.4%)                                               │ │
│ │ • Completion Rate: 89% (42/47 submitted solutions)                             │ │
│ │ • Most Common Algorithm: Dynamic Programming (67% of solutions)                │ │
│ │ • Average Completion Time: 28 minutes 15 seconds                               │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 💻 CODE COMPARISON & REVIEW                                                         │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🔍 TOP SOLUTION SHOWCASE                                                        │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────┐                   ┌─────────────────────────┐       │ │
│ │ │ 🥇 WINNING SOLUTION     │                   │ 🥈 RUNNER-UP SOLUTION   │       │ │
│ │ │ Alex Chen (Dragon C.)   │                   │ Emma Davis (Star Dev.)  │       │ │
│ │ │                         │                   │                         │       │ │
│ │ │ ✅ Strengths:           │                   │ ✅ Strengths:           │       │ │
│ │ │ • Clean recursive logic │                   │ • Elegant DP approach   │       │ │
│ │ │ • Proper null handling  │                   │ • Excellent optimization│       │ │
│ │ │ • Efficient memory use  │                   │ • Clear documentation   │       │ │
│ │ │ • Fast implementation   │                   │ • Robust error handling │       │ │
│ │ │                         │                   │                         │       │ │
│ │ │ ⚠️ Minor improvements:  │                   │ ⚠️ Minor improvements:  │       │ │
│ │ │ • Could add more        │                   │ • Slightly slower       │       │ │
│ │ │   comments              │                   │ • More memory usage     │       │ │
│ │ │ • Variable naming       │                   │ • Complex logic flow    │       │ │
│ │ │                         │                   │                         │       │ │
│ │ │ 📊 Complexity: O(n)     │                   │ 📊 Complexity: O(n²)    │       │ │
│ │ │ 💾 Space: O(h)          │                   │ 💾 Space: O(n)          │       │ │
│ │ └─────────────────────────┘                   └─────────────────────────┘       │ │
│ │                                                                                 │ │
│ │ [👁️ View All Solutions] [📋 Copy Top 3] [💬 Expert Analysis] [📚 Learn More]  │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎥 EVENT HIGHLIGHTS                                                                 │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📺 KEY MOMENTS                                                                  │ │
│ │                                                                                 │ │
│ │ ⏰ 00:00 - Free-for-all event begins! 47 participants start coding             │ │
│ │ ⏰ 05:30 - First submission by Alex Chen passes initial tests                  │ │
│ │ ⏰ 08:15 - Emma Davis takes early lead with optimized solution                 │ │
│ │ ⏰ 12:45 - Mike Kim climbs to top 3 with creative approach                     │ │
│ │ ⏰ 18:23 - Alex Chen secures victory with perfect score! 🎉                    │ │
│ │ ⏰ 19:56 - Emma Davis finishes strong in 2nd place                             │ │
│ │ ⏰ 25:00 - Final submissions close, rankings finalized                         │ │
│ │ ⏰ 30:00 - Dragon Coders guild celebrates team victory                         │ │
│ │                                                                                 │ │
│ │ 🏆 EVENT HIGHLIGHTS                                                             │ │
│ │ • Dragon Coders dominated with 3 players in top 10                            │ │
│ │ • 89% completion rate - highest in RogueLearn history!                        │ │
│ │ • Most diverse solution approaches ever seen in a single event                │ │
│ │ • Real-time guild chat created amazing collaborative energy                   │ │
│ │                                                                                 │ │
│ │ [🎥 Watch Replay] [📱 Share Highlights] [💾 Download Video]                     │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 💬 COMMUNITY REACTION                                                               │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📊 SPECTATOR STATISTICS                          💬 TOP COMMENTS               │ │
│ │                                                                                 │ │
│ │ Peak Viewers: 127                               │ CodingMaster: "What a battle! │ │
│ │ Chat Messages: 1,247                            │ Alex's solution was poetry in  │ │
│ │ Prediction Accuracy: 67%                        │ motion 🔥"                     │ │
│ │ Highlight Clips: 23                             │                                │ │
│ │                                                 │ AlgoExpert: "David showed      │ │
│ │ 🗳️ VIEWER PREDICTIONS                           │ great debugging skills. That   │ │
│ │ Predicted Alex: 67% ✅                          │ NPE was tricky!"               │ │
│ │ Predicted David: 33% ❌                         │                                │ │
│ │                                                 │ TreeLover: "Both solutions     │ │
│ │ 🏆 PREDICTION REWARDS                           │ used the same approach. The    │ │
│ │ Correct predictions: 84 users                   │ difference was execution!"     │ │
│ │ XP distributed: 4,200                           │                                │ │
│ │ New badges earned: 12                           │ DevStudent: "I learned so much │ │
│ │                                                 │ watching this! 📚"             │ │
│ │ [📊 Full Stats] [🎁 Claim Rewards] [💬 Join Discussion]                        │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🚀 NEXT STEPS                                                                       │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🏆 TOURNAMENT PROGRESSION                                                       │ │
│ │                                                                                 │ │
│ │ Alex Chen advances to:                                                          │ │
│ │ 🥊 Semifinal 1 vs Winner of (Emma Davis vs Sophie Chen)                        │ │
│ │ 📅 Scheduled: Today at 5:00 PM                                                 │ │
│ │ 🎯 Prize Pool: 1,000 XP + Semifinalist Badge                                   │ │
│ │                                                                                 │ │
│ │ David Park receives:                                                            │ │
│ │ 🏅 Quarterfinalist Badge + 250 XP                                              │ │
│ │ 📊 Performance Report & Improvement Suggestions                                 │ │
│ │ 🎓 Access to Premium Training Materials                                         │ │
│ │                                                                                 │ │
│ │ [🔔 Set Semifinal Reminder] [📚 Study Resources] [🎥 Watch Other Battles]       │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 ACTIONS                                                                          │
│                                                                                     │
│ [🏆 View Tournament Bracket] [📊 Full Analytics] [🎥 Battle Replay]                 │
│ [💬 Join Discussion] [📱 Share Results] [📚 Learning Resources] [⚙️ Settings]      │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 6. Tournament Management Dashboard (Admin View)

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ ⚙️ Tournament Management - Winter Code Championship 2024      [🔙 Back] [💾]       │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│ 📊 TOURNAMENT OVERVIEW                                                              │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🏆 EVENT STATUS                                                                 │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────┐ │ │
│ │ │ 📈 PROGRESS             │ │ 👥 PARTICIPANTS         │ │ 🎥 STREAMING        │ │ │
│ │ │                         │ │                         │ │                     │ │ │
│ │ │ Current: Quarterfinals  │ │ Total: 64/64            │ │ Live Viewers: 234   │ │ │
│ │ │ Completed: 56/63 battles│ │ Active: 8               │ │ Peak: 312           │ │ │
│ │ │ Progress: 89% ████████░ │ │ Eliminated: 56          │ │ Streams: 4 active   │ │ │
│ │ │ Est. Finish: 6:45 PM    │ │ Spectators: 234         │ │ Quality: 1080p      │ │ │
│ │ │                         │ │                         │ │                     │ │ │
│ │ │ Status: 🟢 Running      │ │ No-shows: 0             │ │ Status: 🟢 Stable   │ │ │
│ │ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────┘ │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 ACTIVE BATTLE MONITORING                                                         │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🔥 LIVE BATTLES                                                                 │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 🥊 QF1: Alex Chen vs David Park                        ⏰ 23:45 remaining   │ │ │
│ │ │ Problem: Binary Tree Maximum Path Sum                  👥 127 viewers        │ │ │
│ │ │ Alex: ████████████████████░ 95% | David: ████████████████░░░░░ 80%          │ │ │
│ │ │ Status: 🟢 Normal | Issues: None                                            │ │ │
│ │ │ [👁️ Monitor] [⏸️ Pause] [🚨 Flag Issue] [💬 Moderate Chat]                  │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────────────────────────────────────────────────────────┐ │ │
│ │ │ 🥊 QF2: Emma Davis vs Sophie Chen                      ⏰ 23:45 remaining   │ │ │
│ │ │ Problem: Longest Increasing Subsequence               👥 89 viewers         │ │ │
│ │ │ Emma: ██████████████████░░ 90% | Sophie: ████████████░░░░░░░░ 60%           │ │ │
│ │ │ Status: 🟡 Attention | Issues: Sophie struggling                           │ │ │
│ │ │ [👁️ Monitor] [⏸️ Pause] [🚨 Flag Issue] [💬 Moderate Chat]                  │ │ │
│ │ └─────────────────────────────────────────────────────────────────────────────┘ │ │
│ │                                                                                 │ │
│ │ 📋 UPCOMING BATTLES                                                             │ │
│ │ • QF3: Mike Johnson vs Lisa Park (Starting in 15 min)                          │ │
│ │ • QF4: Ryan Chen vs Amy Wilson (Starting in 15 min)                            │ │
│ │ • SF1: TBD vs TBD (Scheduled: 5:00 PM)                                         │ │
│ │ • SF2: TBD vs TBD (Scheduled: 5:00 PM)                                         │ │
│ │                                                                                 │ │
│ │ [⏭️ Start Next Round] [⏸️ Pause All] [📊 Battle Analytics] [⚙️ Settings]        │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🚨 SYSTEM MONITORING & ALERTS                                                       │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 📊 SYSTEM HEALTH                                                                │ │
│ │                                                                                 │ │
│ │ ┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────┐ │ │
│ │ │ 🖥️ SERVER STATUS        │ │ 💾 DATABASE             │ │ 🌐 NETWORK          │ │ │
│ │ │                         │ │                         │ │                     │ │ │
│ │ │ CPU: 45% ████████░░░░░░ │ │ Connections: 234/500    │ │ Latency: 23ms       │ │ │
│ │ │ Memory: 67% ████████░░░ │ │ Query Time: 12ms avg    │ │ Bandwidth: 89%      │ │ │
│ │ │ Storage: 23% ████░░░░░░ │ │ Active Queries: 45      │ │ Packet Loss: 0.1%   │ │ │
│ │ │ Status: 🟢 Healthy      │ │ Status: 🟢 Optimal      │ │ Status: 🟢 Stable   │ │ │
│ │ └─────────────────────────┘ └─────────────────────────┘ └─────────────────────┘ │ │
│ │                                                                                 │ │
│ │ 🚨 RECENT ALERTS                                                                │ │
│ │ • 14:23 - High viewer influx detected (+50 viewers in 2 min) ✅ Resolved      │ │
│ │ • 14:15 - Code execution timeout for User #47 ✅ Resolved                      │ │
│ │ • 14:08 - Chat spam detected and auto-moderated ✅ Resolved                    │ │
│ │                                                                                 │ │
│ │ [🔔 Alert Settings] [📊 Performance Logs] [🛠️ System Tools]                    │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 👥 PARTICIPANT MANAGEMENT                                                           │
│                                                                                     │
│ ┌─────────────────────────────────────────────────────────────────────────────────┐ │
│ │ 🔍 PARTICIPANT SEARCH & FILTER                                                  │ │
│ │                                                                                 │ │
│ │ Search: [Alex Chen_______________] [🔍] Filters: [Active ▼] [Level ▼] [Guild ▼] │ │
│ │                                                                                 │ │
│ │ 📋 ACTIVE PARTICIPANTS                                                          │ │
│ │                                                                                 │ │
│ │ Name          │ Status      │ Current Battle    │ Level │ Guild        │ Actions │ │
│ │ ──────────────┼─────────────┼───────────────────┼───────┼──────────────┼─────────│ │
│ │ Alex Chen     │ 🔴 Fighting │ QF1 vs David P.   │  15   │ Code Masters │ [👁️][💬] │ │
│ │ David Park    │ 🔴 Fighting │ QF1 vs Alex C.    │  14   │ Algo Wizards │ [👁️][💬] │ │
│ │ Emma Davis    │ 🔴 Fighting │ QF2 vs Sophie C.  │  13   │ Data Ninjas  │ [👁️][💬] │ │
│ │ Sophie Chen   │ 🔴 Fighting │ QF2 vs Emma D.    │  12   │ Logic Lords  │ [👁️][💬] │ │
│ │ Mike Johnson  │ ⏳ Waiting  │ QF3 (15 min)      │  14   │ Code Masters │ [👁️][💬] │ │
│ │ Lisa Park     │ ⏳ Waiting  │ QF3 (15 min)      │  13   │ Byte Busters │ [👁️][💬] │ │
│ │ Ryan Chen     │ ⏳ Waiting  │ QF4 (15 min)      │  12   │ Script Squad │ [👁️][💬] │ │
│ │ Amy Wilson    │ ⏳ Waiting  │ QF4 (15 min)      │  11   │ Debug Demons │ [👁️][💬] │ │
│ │                                                                                 │ │
│ │ [📧 Message All] [🚨 Send Alert] [📊 Export Data] [⚙️ Bulk Actions]             │ │
│ └─────────────────────────────────────────────────────────────────────────────────┘ │
│                                                                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ 🎮 TOURNAMENT CONTROLS                                                              │
│                                                                                     │
│ [⏸️ Emergency Pause] [⏭️ Force Next Round] [🔄 Restart Battle] [📊 Generate Report] │
│ [📧 Broadcast Message] [🎥 Stream Controls] [⚙️ Tournament Settings] [🛠️ Admin Tools]│
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## Design Notes

### Key Features Highlighted:

1. **Event Creation Interface**
   - Comprehensive tournament setup with scheduling, prizes, and technical configurations
   - Flexible tournament formats and scoring systems
   - Streaming and spectator settings integration

2. **Tournament Bracket System**
   - Visual bracket representation with real-time updates
   - Live battle monitoring with progress indicators
   - Spectator engagement features

3. **Live Coding Interface**
   - Split-screen code viewing for head-to-head battles
   - Real-time performance metrics and comparisons
   - Integrated commentary and chat systems

4. **Spectator Experience**
   - Multi-stream viewing with battle selection
   - Live predictions and community interaction
   - Comprehensive analytics and insights

5. **Post-Battle Analysis**
   - Detailed performance breakdowns and code reviews
   - Battle highlights and key moments
   - Community reactions and learning resources

6. **Admin Management**
   - Real-time tournament monitoring and control
   - System health tracking and alert management
   - Participant management and communication tools

### Technical Considerations:

- **Real-time Updates**: All interfaces support live data streaming
- **Scalability**: Designed to handle multiple concurrent battles
- **Accessibility**: Clear visual indicators and status updates
- **Mobile Responsiveness**: Layouts adaptable to different screen sizes
- **Performance Monitoring**: Built-in analytics and system health tracking