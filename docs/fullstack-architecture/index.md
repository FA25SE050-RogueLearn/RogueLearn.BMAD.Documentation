# RogueLearn Fullstack Architecture Document

## Overview

This document provides a comprehensive architectural overview of the RogueLearn platform, a gamified learning management system designed to enhance student engagement through quest-based learning, social collaboration, and competitive programming challenges.

## Table of Contents

- [Introduction](#introduction)
- [High Level Architecture](#high-level-architecture)
- [Tech Stack](#tech-stack)
- [Database Architecture](#database-architecture)
  - [Database Schema](./database-schema.md) - Complete PostgreSQL and ChromaDB schema definitions
  - [Database Per Service](./database-per-service.md) - Service-specific data organization and cross-service patterns
- [Data Models](#data-models)
  - [Data Models](./data-models.md) - TypeScript interfaces and data structures
- [API Specification](#api-specification)
- [System Components](#system-components)
  - [Components](./components.md) - Reusable UI and system components
- [External Integrations](#external-integrations)
  - [External APIs](./external-apis.md) - Third-party service integrations
- [Core Workflows](#core-workflows)
- [Architecture Layers](#architecture-layers)
  - [Frontend Architecture](./frontend-architecture.md) - Client-side architecture and patterns
  - [Backend Architecture](./backend-architecture.md) - Server-side microservices architecture
- [Project Organization](#project-organization)
  - [Unified Project Structure](./unified-project-structure.md) - Codebase organization and conventions

## Key Features Covered

### 🎓 **Academic Management**
- **Curriculum Versioning**: Multi-version curriculum support with effective year tracking
- **Subject Organization**: Hierarchical subject structure with prerequisites
- **Student Enrollment**: Term-based enrollment with progress tracking
- **Assessment Integration**: Comprehensive assessment and grading system

### 🎮 **Gamified Learning**
- **Quest System**: Chapter-based quests with complex dependency management
- **Skill Trees**: Hierarchical skill development with experience requirements
- **Achievement System**: Comprehensive achievement tracking and rewards
- **Progress Analytics**: Detailed learning analytics and engagement metrics

### 👥 **Social Collaboration**
- **Party System**: Small study groups with role-based management
- **Guild Communities**: Large-scale knowledge sharing communities
- **Meeting Management**: Comprehensive meeting analytics and engagement tracking
- **Collaborative Tools**: Real-time collaboration and knowledge sharing

### 🏆 **Competition Platform**
- **Event Management**: Multi-format competitive events and workshops
- **Code Battle System**: Programming competitions with comprehensive judging
- **Leaderboards**: Individual and guild ranking systems
- **Performance Analytics**: Detailed competition performance tracking

### 🔧 **Technical Infrastructure**
- **Microservices Architecture**: Service-oriented design with clear boundaries
- **Cross-Service Communication**: Efficient data synchronization patterns
- **Real-time Features**: WebSocket-based real-time updates
- **Scalable Data Storage**: PostgreSQL and ChromaDB for different data types

## Architecture Principles

1. **Microservices Design**: Clear service boundaries with dedicated databases
2. **Event-Driven Architecture**: Asynchronous communication between services
3. **Data Consistency**: ACID compliance within services, eventual consistency across services
4. **Scalability**: Horizontal scaling capabilities for high-load scenarios
5. **Security**: Multi-layer security with authentication, authorization, and data protection
6. **Performance**: Optimized queries, caching strategies, and efficient data structures
7. **Maintainability**: Clean code principles, comprehensive documentation, and testing

## Getting Started

For detailed information about specific aspects of the architecture, please refer to the individual documents linked in the table of contents above. Each document provides in-depth coverage of its respective domain with practical implementation details and best practices.
