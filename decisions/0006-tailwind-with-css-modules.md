# Why Tailwind with CSS Modules for Frontend Styling

## Context

The frontend needs a styling system that includes both flexible maintainable UI development and support for custom visual design.

## Decision

Style my UI using Tailwind CSS in combination with CSS modules.

## Why

- Tailwind comes out of the box with a maintainable developer-friendly class system for defining baseline UI stylings & simple responsiveness features
- CSS modules provide strong tooling for custom visuals, while providing appropriate scoping & reusability for stylings to target specific parts of the UI that require it
- Tailwind's relatively weak support for custom visuals and CSS modules' relatively slow development flows are each covered by the other styling tool

#### Environment / Constraints

- On initial development, React Components will default to Tailwind for most stylings, except in cases where CSS modules supply a clear benefit
- Global CSS will be minimized, reserved for base styles only

## Tradeoffs

- Requires a dual-styling mental model
- If Tailwind is used well beyond the scope of providing baseline style support, it can potentially send large CSS packages to the frontend

## Notes

The need to support "custom visuals" is in line with how I know I've made UI decisions in the past, with visual flair including [shapes, glows & lightings, patterns, and animations](https://nickhz.live/cyber/) that Tailwind wouldn't support very well. Not that elements like this would be included on a large portion of the pages; there should just remain well-defined support for custom UI decisions to be possible, as I know that at least a few of those decisions are probable.

Some of my more expressive UI element designs may only be possible in Tailwind by using it as a syntax wrapper around raw CSS. Therefore, CSS modules are an appropriate complement to Tailwind, allowing standard UI elements to undergo speedy development while more exceptional parts of the UI can remain cleanly within the application's predefined styling systems.
