# Nullnet Documentation

This repository contains engineering & development documentation for the Nullnet project.

This documentation aims to make the system traceable, replicable, demonstrable, and transparent, in both how it works and how it was created.

# Application Overview

**Key project goal** - _Provide a publicly available user-friendly human-curated discovery platform for the outer Web._

**Tech Stack:**
- **Frontend:** Next.js (React TypeScript, for UI & SSR)
- **Backend:** Spring Boot (Java 21, for REST API & business logic)
- **Database:** PostgreSQL (on AWS RDS)
- **Deployment:** Amazon Web Services

**Intermediate goals:**
- System Design
  - Ensure complex business logic is contained in the backend layer, avoiding non-UI logic in the SSR layer
  - Deliver performant server-rendered UI for a smooth browsing experience
- Development & Maintainability
  - Keep this application's creation process replicable, to maximize its learning value as a reference for future projects
  - Maintain a structure that supports independent development for each layer
- Deployment & Operation
  - Establish fully automated CI/CD to deploy updates directly from project repos
  - Once deployed, watch costs to ensure cost-sustainability
