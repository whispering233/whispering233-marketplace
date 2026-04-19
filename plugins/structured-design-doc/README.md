# structured-design-doc

`structured-design-doc` is a documentation plugin for forcing backend design output into stable, reviewable structures.

## What it includes

- 1 routing skill: `design-doc-router`
- 3 specialized skills: `design-solution`, `db-table-design`, `api-design-spec`
- 1 shared rules file: `references/shared-design-rules.md`
- 3 standalone templates in `templates/`
- Codex metadata: `.codex-plugin/plugin.json`
- Claude metadata: `.claude-plugin/plugin.json`

## Version 1 scope

This plugin currently standardizes:

- design solution / technical solution / system design documents
- relational database table design documents
- HTTP/REST API design documents

It does not yet cover cache, queue, event, or deployment design.

## Usage flow

1. Start with the routing skill `design-doc-router`.
2. Let the skill decide whether the request belongs to design solution, database table design, API design, or a split combination of them.
3. Apply `references/shared-design-rules.md`.
4. Produce output using the matching standalone template under `templates/`.

## Extension model

To add a new design document type later:

1. Add a new template under `templates/`.
2. Add a specialized skill under `skills/`.
3. Extend the routing rules in `skills/design-doc-router/SKILL.md`.
4. Reuse `references/shared-design-rules.md` instead of duplicating common rules.
