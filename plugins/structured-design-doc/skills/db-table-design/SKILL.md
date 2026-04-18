---
name: db-table-design
description: Use when a user needs a relational database table design, schema specification, field and index design, or structured table documentation for implementation or review.
---

# DB Table Design

## Overview

Use this skill for relational database design documents.

Read these files in order:

1. `../../references/shared-design-rules.md`
2. `../../templates/db-table-design-template.md`

## Required Workflow

1. Clarify the business goal and scope of the schema.
2. Identify all tables that must appear in the current document.
3. Fill the template sections instead of writing a free-form narrative.
4. Make constraints explicit at field, index, and relationship level.
5. End with risks, assumptions, and open questions.

## Required Content

The final document must cover, when relevant:

- table inventory
- field definitions
- primary and unique keys
- index design
- relationships
- enum or status values
- audit fields
- soft delete strategy
- lifecycle or retention notes
- migration and compatibility impact

## Quality Bar

- do not replace design reasoning with raw SQL
- do not omit non-functional notes such as scale, retention, or compatibility when they matter
- do not hide missing information; mark it as `To be confirmed`
- keep field semantics clear enough that an engineer can implement the schema without guessing

## Common Failure Modes

Avoid these outputs:

- only a `CREATE TABLE` statement
- a field list without purpose or constraints
- indexes listed without explaining why they exist
- relationships mentioned without identifying the owning tables
