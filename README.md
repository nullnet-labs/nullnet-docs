# Nullnet Documentation

This repository contains engineering & development documentation for the Nullnet project, that doesn't belong in code repositories. This includes documentation regarding architecture, design decisions, development documentation, methodologies, stretch-goal features & feature ideation, lessons learned, and background research / notes.

At the time of writing, I have intent to include documentation that's exhaustive or imperative, in the sense that anyone would be able to follow my footsteps to see how exactly this project was created. This is for traceability, replicability, demonstrability, and openness.

# Application Overview

**Key project goal** - _Provide a publicly available user-friendly human-curated discovery platform for the outer Web._

The Nullnet project is a fullstack Web application built with:
- **Frontend:** Next.js (React TypeScript, for UI & SSR)
- **Backend:** Spring Boot (Java 21, for REST API & business logic)
- **Database:** PostgreSQL (on AWS RDS)
- **Deployment:** Amazon Web Services

Intermediate & ancillary project goals:
- System Design
  - Ensure complex business logic is contained in the backend layer, avoiding non-UI logic in the SSR layer
  - Deliver performant server-rendered UI for a smooth browsing experience
- Development & Maintainability
  - Keep this application's creation process replicable, to maximize its learning value as a reference for future projects
  - Maintain a structure that supports independent development for each layer
- Deployment & Operation
  - Establish fully automated CI/CD to deploy updates directly from project repos
  - Once deployed, watch costs to ensure cost-sustainability
