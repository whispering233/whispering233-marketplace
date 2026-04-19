# Design Solution Extension Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add `design-solution` as the third structured document type in the plugin, including routing, specialized guidance, template support, and metadata updates.

**Architecture:** Keep the existing plugin shape unchanged: shared rules, one router, one specialized skill per document type, and one standalone template per type. Add a new `design-solution` skill and template, then wire them through the router and plugin documentation.

**Tech Stack:** Markdown templates, skill manifests, JSON plugin metadata, Git verification commands.

---

### Task 1: Add the Design Solution Template

**Files:**
- Create: `templates/design-solution-template.md`
- Test: shell verification commands from repository root

- [ ] **Step 1: Verify the template does not exist yet**

Run: `Test-Path 'templates/design-solution-template.md'`
Expected: `False`

- [ ] **Step 2: Create the template with the approved heading skeleton**

Create `templates/design-solution-template.md` with:

```md
# Design Solution Template

## 1. Document Overview

| Item | Content |
| --- | --- |
| Document title | `<project or solution name>` |
| Business goal | `<why this solution exists>` |
| Scope | `<covered modules or flows>` |
| Non-goals | `<what is intentionally excluded>` |
| Target readers | `<engineers, reviewers, operators, etc.>` |
| Assumptions | `<known assumptions>` |

## 2. Solution Overview

| Item | Content |
| --- | --- |
| Solution summary | `<high-level summary>` |
| Sub-parts or sub-flows | `<inventory list or short summary>` |
| Key design decisions | `<decisions and tradeoffs>` |
| Overall constraints | `<global constraints>` |
| Dependencies | `<upstream, downstream, or platform dependencies>` |
| Risks and open confirmation items | `<risks and To be confirmed items>` |

## 3. `<Sub-part or Sub-flow Name>`

Repeat this section for each sub-part or sub-flow. Each sub-part or sub-flow must be a second-level heading.

### 3.x 设计目标

### 3.x 设计原则

### 3.x 设计思路

### 3.x 输入结构

### 3.x 输出结构

### 3.x 处理流程

### 3.x 设计规格和约束

### 3.x 与上下游的交互边界

### 3.x `<按需追加章节 X>`

Only add extra sections here when needed. Keep inserted sections between `与上下游的交互边界` and `透出的接口设计`.

### 3.x 透出的接口设计

### 3.x 调试要求

### 3.x 示例

### 3.x 待处理的问题

Rules:

- standard sections may be omitted when not applicable
- standard section order must be preserved
- missing information should be marked as `To be confirmed`
- the document may use sub-parts only, sub-flows only, or a mix of both
```

- [ ] **Step 3: Verify the template file now exists**

Run: `Test-Path 'templates/design-solution-template.md'`
Expected: `True`

- [ ] **Step 4: Verify the key headings are present**

Run: `Get-Content 'templates/design-solution-template.md' | Select-String -Pattern '^## 1\. Document Overview$','^## 2\. Solution Overview$','^### 3\.x 设计目标$','^### 3\.x 透出的接口设计$'`
Expected: all four patterns are matched

- [ ] **Step 5: Commit**

```bash
git add templates/design-solution-template.md
git commit -m "feat: add design-solution template"
```

### Task 2: Add the Design Solution Skill

**Files:**
- Create: `skills/design-solution/SKILL.md`
- Test: shell verification commands from repository root

- [ ] **Step 1: Verify the skill file does not exist yet**

Run: `Test-Path 'skills/design-solution/SKILL.md'`
Expected: `False`

- [ ] **Step 2: Create the specialized skill**

Create `skills/design-solution/SKILL.md` with:

```md
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
```

- [ ] **Step 3: Verify the skill file now exists**

Run: `Test-Path 'skills/design-solution/SKILL.md'`
Expected: `True`

- [ ] **Step 4: Verify the skill references the shared rules and template**

Run: `Get-Content 'skills/design-solution/SKILL.md' | Select-String -Pattern 'shared-design-rules','design-solution-template','sub-parts or sub-flows'`
Expected: all three patterns are matched

- [ ] **Step 5: Commit**

```bash
git add skills/design-solution/SKILL.md
git commit -m "feat: add design-solution skill"
```

### Task 3: Wire the New Type Through the Router and Shared Rules

**Files:**
- Modify: `skills/design-doc-router/SKILL.md`
- Modify: `references/shared-design-rules.md`
- Test: shell verification commands from repository root

- [ ] **Step 1: Write failing verification for missing router support**

Run: `Get-Content 'skills/design-doc-router/SKILL.md' | Select-String -Pattern 'design-solution','技术方案','系统设计'`
Expected: no matches

- [ ] **Step 2: Update the router to include the third document type**

Update `skills/design-doc-router/SKILL.md` so it:

- routes design-solution requests to `design-solution`
- covers `设计方案`, `方案设计`, `技术方案`, and `系统设计`
- distinguishes design-solution from pure DB and pure API requests
- requires split output for mixed requests
- avoids duplicating the full design-solution template

- [ ] **Step 3: Write failing verification for missing shared hierarchy rule**

Run: `Get-Content 'references/shared-design-rules.md' | Select-String -Pattern 'heading levels','section skeletons'`
Expected: no matches

- [ ] **Step 4: Add the light hierarchy rule to shared rules**

Update `references/shared-design-rules.md` with a short rule that:

- requires stable, scannable heading levels
- defers fixed order or heading depth to the document template
- forbids mixing skeletons across document types

- [ ] **Step 5: Verify router and shared rules changes**

Run: `Get-Content 'skills/design-doc-router/SKILL.md','references/shared-design-rules.md' | Select-String -Pattern 'design-solution','技术方案','系统设计','heading levels','section skeletons'`
Expected: all patterns are matched

- [ ] **Step 6: Commit**

```bash
git add skills/design-doc-router/SKILL.md references/shared-design-rules.md
git commit -m "feat: route design-solution documents"
```

### Task 4: Update README and Plugin Metadata

**Files:**
- Modify: `README.md`
- Modify: `.codex-plugin/plugin.json`
- Modify: `.claude-plugin/plugin.json`
- Test: shell verification commands from repository root

- [ ] **Step 1: Write failing verification for missing public references**

Run: `Get-Content 'README.md','.codex-plugin/plugin.json','.claude-plugin/plugin.json' | Select-String -Pattern 'design solution','technical solution','system design'`
Expected: no matches

- [ ] **Step 2: Update the README**

Update `README.md` so it:

- lists three specialized skills
- lists three standalone templates
- includes design solution / technical solution / system design in supported scope
- keeps the current extension model unchanged

- [ ] **Step 3: Update Codex plugin metadata**

Update `.codex-plugin/plugin.json` so it:

- expands the description
- adds keywords for `design-solution` and `system-design`
- updates the interface descriptions
- adds a default prompt for structured design-solution output

- [ ] **Step 4: Update Claude plugin metadata**

Update `.claude-plugin/plugin.json` so it:

- expands the description
- adds keywords for `design-solution` and `system-design`

- [ ] **Step 5: Verify public references are present**

Run: `Get-Content 'README.md','.codex-plugin/plugin.json','.claude-plugin/plugin.json' | Select-String -Pattern 'design solution','technical solution','system design','design-solution','system-design'`
Expected: all patterns are matched

- [ ] **Step 6: Run final repository verification**

Run: `Get-ChildItem -Recurse -File skills,templates,references | Select-Object -ExpandProperty FullName`
Expected: output includes `skills\design-solution\SKILL.md` and `templates\design-solution-template.md`

- [ ] **Step 7: Commit**

```bash
git add README.md .codex-plugin/plugin.json .claude-plugin/plugin.json
git commit -m "docs: publish design-solution capability"
```
