# Why Java 21 Spring Boot for Backend

## Context

I need a powerful flexible backend Web server technology to build a high-quality product-level system that integrates cleanly into a deployed fullstack project.

This technology must be able to act as a REST API server and database communication layer.

I also want business logic to be kept out of the frontend, so this layer must be able to handle any processing that's not related to rendering & UI.

## Decision

Use Spring Boot (Java 21) for the backend.

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

#### Environment / Constraints

- Java 21 is the latest LTS JDK version that's fully supported by powerful development tools such as Lombok
- Spring Boot 4.0.6 is the latest stable Spring Boot version at the time of writing
- Maven is the build tool with which I'm the most familiar

## Tradeoffs

- Complex initial setup
- Verbose business logic
- Slower development iterations (from both verbosity & rebuild speed standing in the way of checking the next available output)

## Notes

Developer capability is a part of system design. I've used other backend technologies in a learning & demonstration capacity, but I want a professional-level product, which is best guaranteed by applying proven professional-level skill.
