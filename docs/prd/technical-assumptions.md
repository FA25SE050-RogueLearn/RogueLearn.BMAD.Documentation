# **Technical Assumptions**

- **Repository Structure:** Multi-Repo (separate repositories for frontend and backend).
- **Service Architecture:** Decoupled Frontend/Backend model.
- **Testing Requirements:** A combination of Unit and Integration tests.
- **Shared Code Strategy:** A private, versioned package will be used to share code (e.g., TypeScript types) between repositories.
- **High-Risk Areas:** The most technically challenging aspects are expected to be:
   - The AI-driven processing of both structured (syllabi, schedules) and unstructured (web content) data.
   - The development of a secure and reliable browser extension. This includes not only scraping and parsing but also implementing an efficient, real-time search of the user's "Arsenal" via a secure API.
   - The logic for mapping diverse academic data and learned knowledge into a coherent, dynamic, and visually representative skill tree.
   - The implementation of a player discovery and party matchmaking system. This involves managing user privacy, defining public-facing stats, and creating a robust invitation and permissions system.
   - Implementing a performant, real-time leaderboard system that can handle frequent updates and queries.
   - Developing or integrating a feature-rich, secure, and reliable rich-text editor for the "Arsenal."
   - The AI logic for dynamically adjusting questlines, which requires a sophisticated understanding of dependencies, user preferences, and learning pace.
   - Creating a secure system for lecturers to upload and manage their own materials within Guilds, and for students to integrate these into their learning paths.
   - Ensuring the secure and private handling of sensitive student data across all components of the system.