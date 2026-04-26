# Why Two Repos

## Context

I need a scalable but manageable fullstack repo structure for managing the application code.

## Decision

Use one monolithic repo for the frontend, and use another modular monorepo for the backend.

## Why

- Independent scaling and deployment
- CI/CD processes & repo-specific behaviors (such as hooks & secrets) can have clear delineations that require less custom configurations and are much less prone to crossing wires
- Front & back repos have independent reasoning that warrant their own decision rationales:
  - The app router structure of the frontend Next.js build lends well toward keeping the whole frontend application in one file structure, especially if it's not being used for API
  - The Spring Boot backend is sufficiently served as a monorepo to start with

## Tradeoffs

- Cross-deployment communication overhead
- Backend features have limited ability to scale independently from one another

## Notes

Spring Boot is able to be used for a microservices architecture or SOA. However, doing so is a particularly high-intensity commitment for solo development. In light of this, the Spring Boot project may be its own monorepo for now, but it should be ready to split in the future if needed. For this, the Spring Boot project should be structurally modular (i.e., independent features separated into their own packages).
