# Code Battle Service Database Schema (Decommissioned)

## ARCHITECTURAL NOTE: DECOMMISSIONED SERVICE

As per the architectural review on **October 18, 2025**, the `Code Battle Service` as a standalone microservice has been **decommissioned**.

Its responsibilities and database tables have been fully merged into the new, consolidated **`Event Service`**.

-   **Problem Catalog (`code_problems`, `languages`, `test_cases`, `tags`):** Now owned and managed by the `Event Service`.
-   **Submissions (`submissions`):** Now owned and managed by the `Event Service`.
-   **Events, Rooms, and Leaderboards:** Now owned and managed by the `Event Service`.

The `Executor Service` will be called by the `Event Service` to handle the secure compilation and execution of code submissions.

**This document is retained for historical purposes only. For the current and authoritative schema regarding code battles, problems, and events, refer to the `Event Service Database Schema` document.**