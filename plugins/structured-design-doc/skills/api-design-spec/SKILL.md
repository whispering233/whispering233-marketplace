---
name: api-design-spec
description: Use when a user needs an HTTP or REST API design document, endpoint specification, request and response contract, or structured interface documentation for implementation or review.
---

# API Design Spec

## Overview

Use this skill for HTTP and REST API design documents.

Read these files in order:

1. `../../references/shared-design-rules.md`
2. `../../templates/api-design-spec-template.md`

## Required Workflow

1. Clarify the business goal, caller, and scope of the endpoint set.
2. List the endpoints covered by the document.
3. Fill the template for each endpoint instead of writing free-form prose.
4. Make request, response, auth, and error contracts explicit.
5. End with risks, assumptions, and open questions.

## Required Content

The final document must cover, when relevant:

- endpoint inventory
- method and path
- caller and authentication requirements
- authorization notes
- headers
- path, query, and body parameters
- response fields
- status codes and business error codes
- idempotency
- pagination, filtering, and sorting
- compatibility and versioning strategy

## Quality Bar

- do not replace the design doc with example JSON only
- do not omit field meanings or validation rules
- do not hide missing information; mark it as `To be confirmed`
- keep caller expectations clear enough that an engineer can implement the API contract without guesswork

## Common Failure Modes

Avoid these outputs:

- request and response examples without field definitions
- endpoint lists without auth or permission rules
- success responses without error behavior
- status codes listed without business meaning
