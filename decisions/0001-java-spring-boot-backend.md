# Why Java Spring Boot for Backend

## Context

I need a powerful flexible backend Web server technology to build a high-quality product-level system that integrates cleanly into a deployed fullstack project.

This technology must be able to act as a REST API server and database communication layer.

I also want business logic to be kept out of the frontend, so this layer must be able to handle any processing that's not related to rendering & UI.

## Decision

Use Java Spring Boot for the backend.

## Why

- I have several years of professional experience building enterprise applications in Java Spring Boot, so leveraging this experience allows for several benefits, such as:
  - faster development
  - higher out-of-the-box quality
  - more effective debugging
  - better maintainability practices
  - greater familiarity with the existing development ecosystem
- Strong support for REST APIs & designs that may need to scale
- Manageable development experience that still maintains the required flexibility
- Mature ecosystem for configuration, security, persistence (JPA), and other supporting sub-systems

## Tradeoffs

- Complex initial setup
- Verbose business logic
- Slower development iterations (from both verbosity & rebuild speed standing in the way of checking the next available output)

## Notes

Developer capability is a part of system design. I've used other backend technologies in a learning & demonstration capacity, but I want a professional-level product, which is best guaranteed by investing proven professional-level skill.
