# Why Next.JS for Frontend

## Context

I need a performant frontend technology to deliver a high-quality product-level UI that integrates cleanly into a deployed fullstack project.
Routing, pages with frequently (but not necessarily real-time) updating content, and API integration are necessary for a content-driven platform.

## Decision

Use Next.js with an SSR strategy for the frontend.

## Why

- SSR allows for cacheable HTML rendering that will be performant for this application
- Accelerated development from built-in SSR & routing and modular component-based development flow
- Industry-standard UI technology with an established ecosystem of tooling & support
- Leverages & builds upon my development experience using Javascript & Javascript-based tooling (including React)
- Provides portfolio-building transferable skill demonstration as an industry-relevant technology

#### Environment / Constraints

- **Typescript** will aid in reducing error potential during development
- The "app router" routing paradigm will be used in place of the "pages router" paradigm that's currently being phased out

## Tradeoffs

- Requires discipline & deliberate component choices to maintain server-side UI rendering while ensuring any necessary interactivity
- Adds a running cost & resource management pipeline for the frontend
- Includes framework complexity beyond SPA renderers like client-side React, along with more specialized server processing & developer cognitive load than server-side HTML templating engines like Thymeleaf

## Notes

This decision prioritizes both manageable development for a smooth developer experience and non-browser-intensive rendering for a smooth user experience.
