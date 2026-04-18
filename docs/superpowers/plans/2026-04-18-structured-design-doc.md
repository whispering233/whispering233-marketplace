# Structured Design Doc Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a new marketplace plugin named `structured-design-doc` with shared design rules, standalone templates, three skills, and Codex plus Claude marketplace metadata.

**Architecture:** Build the plugin as a small documentation-first package. Keep reusable writing constraints in `references/`, keep output shapes in `templates/`, and keep skill behavior in `skills/` so future document types can extend the plugin without reworking the routing layer.

**Tech Stack:** Markdown, JSON, local marketplace metadata files

---

### Task 1: Create specification and implementation scaffolding

**Files:**
- Create: `docs/superpowers/specs/2026-04-18-structured-design-doc-design.md`
- Create: `docs/superpowers/plans/2026-04-18-structured-design-doc.md`

- [ ] **Step 1: Confirm current absence of the plugin**

Run: `Test-Path .\plugins\structured-design-doc`
Expected: `False`

- [ ] **Step 2: Save the approved design**

Write the approved scope, plugin layout, routing rules, shared rules, template ownership, and marketplace update requirements into the spec file.

- [ ] **Step 3: Save this implementation plan**

Write the execution tasks, file list, and validation commands into the plan file.

- [ ] **Step 4: Verify both docs exist**

Run: `Get-ChildItem .\docs\superpowers\specs,.\docs\superpowers\plans`
Expected: both new Markdown files are listed

### Task 2: Create plugin manifests and documentation shell

**Files:**
- Create: `plugins/structured-design-doc/.codex-plugin/plugin.json`
- Create: `plugins/structured-design-doc/.claude-plugin/plugin.json`
- Create: `plugins/structured-design-doc/README.md`

- [ ] **Step 1: Create the Codex manifest**

Add plugin identity, author, license, keywords, skills path, and Codex `interface` metadata.

- [ ] **Step 2: Create the Claude manifest**

Add plugin-level metadata for Claude Code discovery and description.

- [ ] **Step 3: Create the plugin README**

Document plugin purpose, included files, usage flow, and extension model.

- [ ] **Step 4: Parse manifest JSON**

Run:
```powershell
Get-Content -Raw '.\plugins\structured-design-doc\.codex-plugin\plugin.json' | ConvertFrom-Json | Out-Null
Get-Content -Raw '.\plugins\structured-design-doc\.claude-plugin\plugin.json' | ConvertFrom-Json | Out-Null
```
Expected: no errors

### Task 3: Create the shared rules and standalone templates

**Files:**
- Create: `plugins/structured-design-doc/references/shared-design-rules.md`
- Create: `plugins/structured-design-doc/templates/db-table-design-template.md`
- Create: `plugins/structured-design-doc/templates/api-design-spec-template.md`

- [ ] **Step 1: Write the shared rules document**

Capture cross-document requirements: scope, completeness, naming, explicit constraints, formatting, risks, and open questions.

- [ ] **Step 2: Write the DB template**

Create a reusable Markdown structure for relational table design, including table details, indexes, constraints, and compatibility sections.

- [ ] **Step 3: Write the API template**

Create a reusable Markdown structure for REST endpoint design, including request and response fields, auth, status codes, and compatibility sections.

- [ ] **Step 4: Verify files exist**

Run: `Get-ChildItem -Recurse .\plugins\structured-design-doc\references,.\plugins\structured-design-doc\templates`
Expected: three Markdown files are listed

### Task 4: Create the routing and specialized skills

**Files:**
- Create: `plugins/structured-design-doc/skills/structured-design-doc/SKILL.md`
- Create: `plugins/structured-design-doc/skills/db-table-design/SKILL.md`
- Create: `plugins/structured-design-doc/skills/api-design-spec/SKILL.md`

- [ ] **Step 1: Create the routing skill**

Document intent detection, routing rules, and the requirement to enforce structured output.

- [ ] **Step 2: Create the DB skill**

Document how to apply shared rules and the DB template without collapsing into SQL-only output.

- [ ] **Step 3: Create the API skill**

Document how to apply shared rules and the API template without collapsing into example-only output.

- [ ] **Step 4: Verify skill paths**

Run: `Get-ChildItem -Recurse .\plugins\structured-design-doc\skills`
Expected: three skill directories, each containing `SKILL.md`

### Task 5: Update marketplace indexes and run final validation

**Files:**
- Modify: `.agents/plugins/marketplace.json`
- Modify: `.claude-plugin/marketplace.json`

- [ ] **Step 1: Add the Codex marketplace entry**

Append a new plugin object for `structured-design-doc` with local source path, default installation policy, default authentication policy, and category.

- [ ] **Step 2: Add the Claude marketplace entry**

Append a new plugin object with source path, description, version, author, category, and tags.

- [ ] **Step 3: Parse both marketplace JSON files**

Run:
```powershell
Get-Content -Raw '.\.agents\plugins\marketplace.json' | ConvertFrom-Json | Out-Null
Get-Content -Raw '.\.claude-plugin\marketplace.json' | ConvertFrom-Json | Out-Null
```
Expected: no errors

- [ ] **Step 4: Run a final structural check**

Run:
```powershell
Test-Path '.\plugins\structured-design-doc'
$codex = (Get-Content -Raw '.\.agents\plugins\marketplace.json' | ConvertFrom-Json).plugins.name -contains 'structured-design-doc'
$claude = (Get-Content -Raw '.\.claude-plugin\marketplace.json' | ConvertFrom-Json).plugins.name -contains 'structured-design-doc'
"codex=$codex claude=$claude"
```
Expected:
- plugin path exists
- `codex=True`
- `claude=True`
