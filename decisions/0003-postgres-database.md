# Why Postgres Detabase

## Context

I need a structured relational database for holding records of the content being referenced, user details, curation structures, post-specific UI presentation, and more as the application requires.

## Decision

Use a PostgreSQL database.

## Why

- Strong relational data support
- Better guaranteed consistency
- Easy to enforce schema rules
- Well-built interfacing & hosting support from other technologies in the stack
- Leverages developer familiarity

#### Environment / Constraints

- Deployed to AWS RDS

## Tradeoffs

- Upfront schema design
- Slower iteration compared to document DBs

## Notes

Consider other database technologies if project shifts to require more unstructured data.
