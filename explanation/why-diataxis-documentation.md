# Why Diátaxis?

### The Problem

Producing documentation that's consistent, maintainable, and friendly for outside & future viewing is a slow process for a solo developer & author. I would have to produce the documentation as well as its structure, with meta-decisions about scaling and information retrieval.

There are several non-standard documents to have on file, such as how I configured & initialized areas of the project down to the commands used, or how I would define "human-curated" as a measurable design goal in the context of this project. Knowing which documents to write does not inform where & how those documents should be organized in a way that's future-proof against the ongoing documentation needs of this project.

### The Goal

I want an organizational structure that provides clear categorization for even non-standard documents, that also remains resilient to the evolution of the project. If possible, want to avoid coming up with this structure ad-hoc, as that bears the risk of needing documentation restructures in the future. I want an approach that's tried and true.

### The Decision

I've adopted [Diátaxis](https://diataxis.fr/) as a structural framework.

This separates documentation by my intent:
- Tutorials - Actionable guided learning material
- How-to guides - Goal-oriented task execution for lookup & direct use
- Reference - The facts of the application's systems & behaviors
- Explanation - Conceptual models for the how & why of the system's design

### Why

It's a structure that's already known to scale independently of codebase boundaries, with future navigation kept manageable as the project evolves. Just by understanding this structure & the flexibility of each category, every document can have a place to fit, reducing the need to spend time making ongoing meta-decisions about the documentation structure.

### Tradeoffs

- Some documents may not fit cleanly into one category or may contain duplicate information
- The "tutorial" category may be neglected, due to this application's use case
- Correct application of this approach comes with a learning curve

### Notes

Diátaxis is used to reduce ambiguity in documentation structure & improve long-term maintainability, and this document is not an assertion of comparison over other documentation models. It's simply what fits the needs I foresee for this application's documentation.
