# Structured Design Doc Plugin Design

**Date:** 2026-04-18
**Plugin:** `structured-design-doc`
**Status:** Approved for implementation

## Goal

Create a marketplace plugin that forces project design documents into stable, reviewable structures instead of free-form prose. Version 1 covers two document types:

- relational database table design
- HTTP/REST API design

## Scope

The plugin will ship with:

- 1 routing skill: `design-doc-router`
- 2 specialized skills: `db-table-design`, `api-design-spec`
- 1 shared rules document
- 2 standalone Markdown templates
- 1 Codex plugin manifest
- 1 Claude plugin manifest
- marketplace entries for both Codex and Claude Code

The plugin will not ship with:

- SQL or OpenAPI generators
- hooks, scripts, or apps
- example business documents
- additional design domains such as cache, queue, or event modeling

## File Layout

```text
plugins/structured-design-doc/
  .codex-plugin/plugin.json
  .claude-plugin/plugin.json
  README.md
  references/shared-design-rules.md
  templates/db-table-design-template.md
  templates/api-design-spec-template.md
  skills/design-doc-router/SKILL.md
  skills/db-table-design/SKILL.md
  skills/api-design-spec/SKILL.md
```

## Design Decisions

### Shared rules layer

`references/shared-design-rules.md` defines the common contract across all structured design outputs:

- document purpose and non-goals
- naming consistency
- completeness requirements
- explicit constraint recording
- risk and open-question handling
- output formatting expectations

This file does not own template-specific sections.

### Template layer

Templates remain standalone Markdown files so skills can stay small and composable.

- `templates/db-table-design-template.md` defines the required structure for relational schema and table design documents.
- `templates/api-design-spec-template.md` defines the required structure for REST API design documents.

### Skill layer

- `design-doc-router` identifies intent and routes to the appropriate specialized skill.
- `db-table-design` enforces the shared rules plus the database template.
- `api-design-spec` enforces the shared rules plus the API template.

The routing skill does not own the templates and does not restate all domain rules.

## Routing Rules

The routing skill uses intent plus keywords:

- route to `db-table-design` for requests about schema, tables, fields, indexes, constraints, or table structure
- route to `api-design-spec` for requests about API docs, endpoints, request or response bodies, routes, or error codes
- if both are requested, require two clearly separated sections instead of a merged free-form document
- if intent is broad but not ambiguous, pick the most immediate document type and note what remains uncovered

## Metadata

### Codex manifest

The Codex manifest should include:

- plugin identity
- author and license metadata
- keywords
- `skills: "./skills/"`
- UI-facing `interface` metadata

### Claude manifest

The Claude manifest should exist at `plugins/structured-design-doc/.claude-plugin/plugin.json` and provide plugin-level descriptive metadata. Version 1 keeps it intentionally small and aligned with the repository's existing Claude marketplace metadata style.

## Marketplace updates

Both marketplace indexes must gain a `structured-design-doc` entry:

- `.agents/plugins/marketplace.json`
- `.claude-plugin/marketplace.json`

## Validation

Implementation is complete when:

- the plugin directory exists with the approved file set
- both marketplace indexes include the new plugin
- all JSON files parse successfully
- skill, reference, and template paths resolve correctly within the plugin
