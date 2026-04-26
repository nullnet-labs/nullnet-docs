# Why AWS for Infrastructure

## Context

All technologies in the stack require infrastructure that interconnects, and the overall application requires additional infrastructure services such as page caching & Web hosting.

## Decision

Use AWS as the primary infrastructure provider.

## Why

- Mature ecosystem
- Wide range of services (compute, storage, domain management, etc.)
- Industry-standard skill exposure
- Trackable costs & scalable if needed

## Tradeoffs

- High-complexity configuration overhead
- Single point of failure / vendor lock-in; moving would be difficult
- Costs will not cap themselves, so infrastructure expenses must be monitored

## Notes

Start simple, and expand only when needed. Do not spin up architecture until the software run by that resource is ready to start being put into place.
