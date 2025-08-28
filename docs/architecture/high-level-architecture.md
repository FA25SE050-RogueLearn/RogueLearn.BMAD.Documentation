# High Level Architecture

## Technical Summary

RogueLearn employs a decoupled frontend/backend architecture with Next.js for the frontend and .NET Core for the backend services. The system follows a multi-repo approach to ensure separation of concerns and independent deployment pipelines. The frontend will be deployed to Vercel for optimal Next.js integration, while the backend services will be containerized using Docker and deployed to Azure. A shared TypeScript package will maintain type consistency between frontend and backend. This architecture supports the gamified learning experience with real-time updates, dynamic skill trees, and seamless browser extension integration required by the PRD.

## Platform and Infrastructure Choice

**Platform:** Microsoft Azure
**Key Services:** Azure App Service, Azure SQL Database, Azure Blob Storage, Azure Functions, Azure API Management, Azure Active Directory B2C, Azure Container Registry
**Deployment Host and Regions:** Multi-region deployment with primary in US East and secondary in Europe West

Rationale:
1. The backend is built on .NET Core, which has first-class support in Azure
2. Azure provides comprehensive services for both the web application and the browser extension components
3. Azure Active Directory B2C offers robust authentication and authorization capabilities
4. Azure's global presence supports the potential international user base
5. Strong enterprise-grade security and compliance features

## Repository Structure

**Structure:** Multi-Repo

The project will use a multi-repo approach as specified in the PRD's technical guidance:

1. **Frontend Repository**
   - Next.js application
   - Browser extension code
   - Storybook for component documentation
   - Frontend tests

2. **Backend Repository**
   - .NET Core API services
   - Database migrations and scripts
   - Backend tests

3. **Shared Types Repository**
   - TypeScript interfaces and types
   - Published as a private NPM package
   - Versioned for consistent integration

This approach allows for independent deployment pipelines and clear separation of concerns while maintaining consistency through the shared types package.