# Shared Design Rules

These rules apply to every document produced through `structured-design-doc`, including database table design and REST API design.

## 1. Purpose Before Structure

Every document must open with:

- business goal
- scope
- explicit non-goals
- target readers or callers when relevant

Do not start with SQL, JSON, or field tables before explaining why the design exists.

## 2. Naming Consistency

Keep naming consistent within the same document:

- IDs use one stable pattern such as `user_id`, `order_id`
- timestamps use one stable pattern such as `created_at`, `updated_at`
- boolean fields use readable predicates such as `is_active`, `is_deleted`
- enum values stay uppercase or lowercase consistently
- API resource names, path nouns, and field names must align with database entities where they represent the same concept

## 3. Completeness Requirements

A document is incomplete if it omits any of these:

- background or business goal
- core entities or resources
- required constraints
- error or risk notes
- open questions when information is missing

If information is missing, keep the section and mark it as `To be confirmed` instead of silently skipping it.

## 4. Explicit Constraint Recording

Record constraints in structured form, not buried in prose.

Examples of constraints that must be explicit when relevant:

- required vs optional
- nullable vs non-null
- default values
- uniqueness
- indexes
- transaction boundaries
- authentication
- authorization
- idempotency
- pagination
- filtering
- sorting
- compatibility impact

## 5. Output Format

Required output style:

- structured headings
- tables for field-level definitions
- short explanatory paragraphs where tradeoffs matter
- dedicated sections for risks and open questions

Do not deliver final output as:

- pure free-form prose
- SQL only
- JSON only
- example payloads without field semantics

## 6. Risks and Decisions

Every document must include:

- design decisions or tradeoffs
- known risks
- open questions

If a decision depends on unknown context, state the assumption directly.

## 7. Separation of Rules and Templates

- this file defines cross-document rules
- templates define required sections for a document type
- skills decide which template to use and enforce these rules

Do not duplicate full templates here.
