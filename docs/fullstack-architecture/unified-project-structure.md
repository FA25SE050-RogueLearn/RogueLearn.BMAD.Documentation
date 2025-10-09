# **Unified Project Structure**

As defined, we will use a **Multi-Repo Strategy**. Each microservice and application component has its own dedicated Git repository, promoting independent development, testing, and deployment cycles.

The definitive list of repositories and their purposes is maintained in the **[Repository Structure](./high-level-architecture.md#repository-structure)** section of the High-Level Architecture document.

Key repositories in our structure include:
- **`RogueLearn.Frontend`**: The Next.js client application.
- **Service Repositories** (e.g., `RogueLearn.User`, `RogueLearn.CodeBattle`): Each contains a single, independently deployable microservice.
- **`RogueLearn.Protos`**: A shared repository for Protobuf files, defining the contracts for inter-service communication (e.g., via gRPC).
- **`RogueLearn.Kubernetes`**: A GitOps repository for deployment configurations, managed by ArgoCD.
- **`RogueLearn.CleanArchitecture`**: A .NET template used to bootstrap new microservices, ensuring architectural consistency.