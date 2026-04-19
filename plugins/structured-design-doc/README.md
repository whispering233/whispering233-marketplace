# structured-design-doc

`structured-design-doc` is a documentation plugin for forcing backend and solution design output into stable, reviewable structures instead of free-form prose.

## Overview

This plugin is built around a simple pattern:

- one routing skill decides the document type
- one shared rules file defines cross-document requirements
- one specialized skill and one template define each document type

The current goal is not to cover every engineering document. It is to make a small set of common design outputs predictable enough for review and downstream implementation.

## What It Includes

- 1 routing skill: `design-doc-router`
- 3 specialized skills: `design-solution`, `db-table-design`, `api-design-spec`
- 1 shared rules file: `references/shared-design-rules.md`
- 3 standalone templates in `templates/`
- Codex metadata: `.codex-plugin/plugin.json`
- Claude metadata: `.claude-plugin/plugin.json`

## Supported Document Types

### 1. Design Solution

Use `design-solution` for requests mainly about:

- design solution
- technical solution
- system design
- module decomposition
- flow design
- upstream and downstream boundaries
- constraints, exposed interfaces, debugging requirements

This type uses a thin global overview and then expands each sub-part or sub-flow as a second-level heading. Inside each sub-part or sub-flow, the document follows a stable section order while still allowing the model to omit inapplicable standard sections.

### 2. Database Table Design

Use `db-table-design` for requests mainly about:

- relational schema
- table structure
- field design
- indexes
- constraints
- relationships

### 3. API Design Specification

Use `api-design-spec` for requests mainly about:

- HTTP or REST API design
- endpoint contracts
- request and response payloads
- parameters
- status codes
- business errors

## Routing Rules

Start with `design-doc-router`.

The router should:

- route solution-level requests to `design-solution`
- route schema-level requests to `db-table-design`
- route API-contract requests to `api-design-spec`
- split mixed requests by document type instead of blending them into one narrative

Pure database schema requests should not be routed to `design-solution`.

Pure REST contract requests should not be routed to `design-solution`.

## Shared Rules

All document types reuse `references/shared-design-rules.md`.

The shared rules enforce:

- purpose before structure
- naming consistency
- explicit constraints
- stable, scannable heading levels
- explicit risks, decisions, and open questions

Document-specific templates still control the final section order and heading depth.

## Usage Flow

1. Start with the routing skill `design-doc-router`.
2. Let the router determine whether the request belongs to design solution, database table design, API design, or a split combination of them.
3. Apply `references/shared-design-rules.md`.
4. Read the matching skill and template.
5. Generate the final document using the matching structure.

## Example Prompts

- `Write a structured technical design solution for an order fulfillment system.`
- `Write a structured database table design document for a new order system.`
- `Write a structured REST API design spec for user profile management.`
- `Split this backend request into design solution, database, and API sections with fixed templates.`

## Version 1 Scope

This plugin currently standardizes:

- design solution / technical solution / system design documents
- relational database table design documents
- HTTP/REST API design documents

It does not yet cover:

- cache design
- message queue design
- event design
- deployment design
- operations runbooks

## Extension Model

To add a new design document type later:

1. Add a new template under `templates/`.
2. Add a specialized skill under `skills/`.
3. Extend the routing rules in `skills/design-doc-router/SKILL.md`.
4. Reuse `references/shared-design-rules.md` instead of duplicating common rules.

The plugin should stay intentionally simple unless the number of document types makes the current pattern unworkable.
