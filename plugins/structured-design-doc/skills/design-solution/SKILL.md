---
name: design-solution
description: Use when a user needs a structured design solution, technical solution, or system design document with stable decomposition into sub-parts or sub-flows.
---

# Design Solution

## Overview

Use this skill for structured design-solution documents.

Read these files in order:

1. `../../references/shared-design-rules.md`
2. `../../templates/design-solution-template.md`

## Required Workflow

1. Clarify the business goal, scope, non-goals, target readers, and assumptions.
2. List the sub-parts or sub-flows covered by the document before expanding them.
3. Fill the template sections instead of writing a free-form narrative.
4. Expand each sub-part or sub-flow using the approved section order.
5. Preserve risks, assumptions, and open questions.

## Required Content

The final document must cover, when relevant:

- solution summary
- sub-part or sub-flow inventory
- design goals
- design principles
- design approach
- input and output structure
- handling flow
- constraints
- upstream and downstream interaction boundaries
- exposed interfaces
- debugging requirements
- examples
- unresolved issues

## Quality Bar

- do not replace the document with free-form prose
- do not list only module names without structured expansion
- do not describe only process flow without input, output, boundary, or constraint definitions
- do not hide missing information; mark it as `To be confirmed`
- allow omission of inapplicable standard sections, but preserve the overall order
- allow extra sections only in the designated `【X...】` position

## Common Failure Modes

Avoid these outputs:

- a solution summary without expanded sub-parts or sub-flows
- a flow-only description with no boundary or contract information
- mixed DB schema or REST contract details embedded into the solution structure without separation
- reordered standard sections inside a sub-part or sub-flow
