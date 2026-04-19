# Design Solution Document Type Design

**Goal:** Add `design-solution` as the third independent document type in `structured-design-doc`, so the plugin can route and generate structured design-solution documents for requests such as "设计方案", "方案设计", "技术方案", and "系统设计".

**Scope:** This spec covers the new document type, its routing boundary, its template structure, the related skill behavior, and the minimal metadata updates required to expose the new capability.

**Non-goals:** This spec does not redesign the existing DB or API templates, does not introduce a config-driven template framework, and does not add support for cache, queue, event, or deployment design.

**Target Readers:** Engineers maintaining the plugin, and agent workers using the plugin to generate structured design documents.

## 1. Current State

The plugin currently uses a stable structure:

- shared cross-document rules in `references/shared-design-rules.md`
- one routing skill in `skills/design-doc-router/SKILL.md`
- two specialized skills in `skills/db-table-design/SKILL.md` and `skills/api-design-spec/SKILL.md`
- two standalone templates in `templates/`

This structure is intentionally simple and should be preserved.

## 2. Problem Statement

The plugin can currently structure database design and REST API design, but it cannot reliably constrain broader design-solution outputs. Requests like "写一个技术方案" or "给我系统设计文档" tend to drift into free-form prose unless there is a dedicated template and routing path.

The new document type must solve this by:

- making "design solution" a first-class routed document type
- enforcing a stable heading hierarchy
- forcing each sub-part or sub-flow to expand in a structured way
- allowing omission of inapplicable standard sections without breaking overall order
- preserving room for model-added sections in a controlled position

## 3. Design Decisions

### 3.1 Keep the existing extension model

The plugin will continue to use:

- one shared-rules file
- one routing skill
- one specialized skill per document type
- one standalone template per document type

`design-solution` will be added as a new specialized skill plus a new template, instead of introducing a generic framework abstraction.

### 3.2 Treat design solution as an independent document type

`design-solution` is not a wrapper around DB or API design. It is a third independent type with its own routing boundary and its own template.

It is intended for requests mainly about:

- 设计方案
- 方案设计
- 技术方案
- 系统设计

It is not intended for:

- pure database field or table design
- pure HTTP or REST interface contracts
- deployment or operations runbooks

### 3.3 Preserve separation for mixed requests

If a request clearly contains:

- design-solution level structure, and
- DB schema details, and/or
- API contract details

the output must stay split by document type instead of collapsing into one blended narrative.

`design-solution` owns:

- overall solution summary
- module or sub-flow decomposition
- processing logic
- constraints
- upstream and downstream boundaries
- exposed interface notes at the solution level
- debugging requirements

`db-table-design` owns table-level schema structure.

`api-design-spec` owns API contract details.

## 4. Design Solution Template

The new template file will be `templates/design-solution-template.md`.

### 4.1 Global document structure

The document keeps a thin global layer before detailed expansion:

```md
# Design Solution Document

## 1. Document Overview

## 2. Solution Overview

## 3. <Sub-part or Sub-flow A>

## 4. <Sub-part or Sub-flow B>
```

The global layer must stay intentionally thin. It exists to establish context, not to absorb detailed design that belongs in the sub-parts or sub-flows.

### 4.2 Document Overview

This section must cover:

- document title
- business goal
- scope
- non-goals
- target readers
- assumptions

### 4.3 Solution Overview

This section must cover:

- solution summary
- sub-part or sub-flow inventory
- key design decisions
- overall constraints
- dependency notes
- risks and open confirmation items

### 4.4 Sub-part or sub-flow expansion rules

Each sub-part or sub-flow must be a second-level heading.

Inside each sub-part or sub-flow, the model should expand downward into lower-level headings using the following standard order:

1. 设计目标
2. 设计原则
3. 设计思路
4. 输入结构
5. 输出结构
6. 处理流程
7. 设计规格和约束
8. 与上下游的交互边界
9. 【X...】按需追加章节
10. 透出的接口设计
11. 调试要求
12. 示例
13. 待处理的问题

Rules:

- the overall order is fixed
- the model may omit standard sections it judges inapplicable
- omitted sections must be removed cleanly rather than replaced with empty prose
- the model may insert additional sections only in the 【X...】 position
- the model must not reorder the standard sections

### 4.5 Applicability of sub-parts vs sub-flows

If the request is naturally modular rather than process-oriented, the document may use only sub-parts.

If the request is naturally process-oriented rather than modular, the document may use only sub-flows.

If both are useful, the model may mix them at the same second-level heading layer as long as each heading remains an independently structured unit.

## 5. Specialized Skill Behavior

The new skill file will be `skills/design-solution/SKILL.md`.

It must instruct the model to:

1. read `../../references/shared-design-rules.md`
2. read `../../templates/design-solution-template.md`
3. identify whether the request belongs to design solution design
4. clarify business goal, scope, non-goals, readers, and assumptions
5. list the sub-parts or sub-flows before expanding them
6. expand each sub-part or sub-flow using the template order
7. preserve risks, assumptions, and open questions

The skill must also explicitly forbid:

- replacing the document with free-form prose
- listing only module names without structured expansion
- describing only process flow without input, output, boundary, or constraint definitions
- silently dropping ambiguity instead of labeling it

Missing information should be marked as `To be confirmed`.

## 6. Router Changes

`skills/design-doc-router/SKILL.md` will be extended with a third route.

Route to `design-solution` when the request is mainly about:

- 设计方案
- 方案设计
- 技术方案
- 系统设计

This route should be described as focusing on:

- solution decomposition
- module or flow structure
- design reasoning
- input and output structure
- process handling
- constraints
- interaction boundaries
- exposed interfaces
- debugging requirements

The router must also make these distinctions explicit:

- do not route pure database schema requests to `design-solution`
- do not route pure REST contract requests to `design-solution`
- if the request includes multiple document types, keep them separated by type

## 7. Shared Rules Changes

`references/shared-design-rules.md` should receive only a light update.

Add a generic hierarchy rule such as:

- every document must use stable, scannable heading levels
- when a document-type template requires fixed section order or specific heading levels, that template controls the final structure
- do not mix section skeletons across document types

This keeps the shared rules generic while still supporting the new template.

## 8. Documentation and Metadata Changes

### 8.1 README

Update `README.md` to reflect:

- one routing skill
- three specialized skills
- three standalone templates
- support for design solution / technical solution / system design documents
- the same extension model, now with `design-solution` as an example

### 8.2 Codex plugin metadata

Update `.codex-plugin/plugin.json` to reflect:

- expanded description
- additional keywords for design solution and system design
- updated interface descriptions
- at least one default prompt for structured design-solution generation

### 8.3 Claude plugin metadata

Update `.claude-plugin/plugin.json` to reflect:

- expanded description
- additional keywords for design solution and system design

## 9. File-Level Change Plan

Create:

- `skills/design-solution/SKILL.md`
- `templates/design-solution-template.md`

Modify:

- `skills/design-doc-router/SKILL.md`
- `references/shared-design-rules.md`
- `README.md`
- `.codex-plugin/plugin.json`
- `.claude-plugin/plugin.json`

## 10. Risks

- The router may over-match generic "设计" requests if the boundary language is too broad.
- The model may still collapse sub-parts into prose if the new skill and template are not explicit enough.
- Mixed requests may blur type boundaries if the router does not clearly require split output.

## 11. Open Questions

- Whether future versions should support separate design-solution subtypes such as module design, flow design, or deployment design remains intentionally out of scope for this change.
- Whether examples in the design-solution template should be expressed as bullet lists, tables, or mixed format can remain flexible as long as the heading structure is preserved.
