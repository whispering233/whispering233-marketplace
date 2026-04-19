---
name: design-doc-router
description: Use when a user needs a structured backend design document, design solution, database table design, API specification, or a broader request that should be routed into the correct template.
---

# Design Doc Router

## Overview

Use this skill as the entry point for backend design-document requests that need stable structure instead of free-form prose.

Always read `../../references/shared-design-rules.md` first. Then route to the correct specialized skill.

## Routing Rules

Route to `db-table-design` when the request is mainly about:

- database schema
- table structure
- field design
- indexes
- constraints
- table relationships

Route to `api-design-spec` when the request is mainly about:

- REST API design
- endpoint specification
- request or response payloads
- request parameters
- status codes
- business error handling

Route to `design-solution` when the request is mainly about:

- 设计方案
- 方案设计
- 技术方案
- 系统设计
- solution decomposition
- module or flow structure
- design reasoning
- input and output structure
- constraints
- interaction boundaries
- exposed interface design
- debugging requirements

If the request clearly includes multiple document types such as design solution, database design, and API design:

- keep the output split into separate parts by document type
- apply the shared rules to each part
- do not collapse them into one blended narrative

Do not route these requests to `design-solution`:

- pure database schema or table design
- pure REST or HTTP contract design

## Required Behavior

- enforce structured output
- prefer the specialized skill over writing everything directly here
- preserve missing sections and mark them as `To be confirmed`
- require explicit risks and open questions
- avoid pure prose, SQL-only, or JSON-only output

## Output Contract

Before finalizing the document:

1. identify the document type
2. apply `../../references/shared-design-rules.md`
3. apply the matching template
4. verify the result still has risks, assumptions, and open questions

## Do Not

- duplicate the full design-solution, DB, or API template in this skill
- invent implementation details when context is missing
- hide ambiguity instead of labeling it
