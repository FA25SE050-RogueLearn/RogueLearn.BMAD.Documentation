# UC-05: Admin Event Approval and Content Management

**Actor:** System Admin

---

## Transactions

### Part A: Event Approval (Transactions 1-5)

1. **Admin reviews pending events**
   - Logs into Admin Panel
   - Navigates to "Event Management" → "Pending Approvals"
   - Sees 3 pending events
   - Clicks "Spring Java Championship 2025"

2. **Admin validates event details**
   - Reviews event overview, participation, structure
   - Clicks "Validate Problem Set"
   - System checks: Spring Boot (15 available), Algorithms (22), Database (8), Design Patterns (6) - All sufficient
   - Reviews Guild history: 2 events, 100% success, 4.7/5 rating

3. **Admin checks budget and prizes**
   - Reviews projected revenue: 20M VND
   - Platform fee: 3M, Prize pool: 17M
   - Validates prize allocation: 11.5M (67.6% of pool)
   - System confirms within guidelines

4. **Admin verifies resources**
   - Checks server capacity: 85% available (needs 5%)
   - Reviews calendar: no conflicts for March 15
   - Staff availability: 3 moderators, 2 engineers
   - System recommends: "Ready for approval"

5. **Admin approves and publishes**
   - Reviews checklist, adds approval notes
   - Clicks "Approve and Publish"
   - System creates Event ID, public page, competition infrastructure
   - Notifies Guild Master and publishes to 250+ students

---

### Part B: Content Management (Transactions 6-10)

6. **Admin accesses content system**
   - Navigates to "Content Management"
   - Sees modules: Course Data, Game Library, Problem Bank, AI Generator
   - Notice: "FLM updated 2 days ago - sync required"
   - Clicks "Course Data"

7. **Admin syncs FAP/FLM data**
   - Clicks "Sync FAP Data Now"
   - System crawls: 157 courses updated, 3 new added
   - Clicks "Import FLM Updates" for PRJ301 syllabus v3.2
   - System parses PDF: 12 weeks, 10 topics, 15 learning objectives
   - Flags: "Agile Quest needs update"

8. **Admin reviews game mappings**
   - Opens "Game Library" → "Agile Quest"
   - Current: mapped to PRJ301 v3.1, last updated 30 days ago
   - System compares with new syllabus: 8/10 topics matched (80%)
   - 2 new topics: "DevOps Integration", "Remote Collaboration"
   - 3 modified topics with expanded outcomes

9. **Admin generates new content with AI**
   - Clicks "Generate Content Updates with AI"
   - Configures: 2 new chapters, 3 modified chapters, +10% difficulty
   - System AI generates chapters, challenges, quizzes in 2 minutes
   - Outputs JSON structure with new content
   - Admin reviews and makes manual adjustments

10. **Admin publishes game update**
    - Approves AI content, adds release notes
    - Selects staged rollout: 10% → 50% → 100% over 7 days
    - Clicks "Publish Game Update"
    - System deploys v2.2, migrates player progress
    - Notifies 300 students (first 10%)
    - Monitors: 45 players accessed, 4.6/5 satisfaction, 0 errors