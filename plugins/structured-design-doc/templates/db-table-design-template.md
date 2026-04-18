# Database Table Design Template

## 1. Document Overview

| Item | Content |
| --- | --- |
| Document title | `<project or module name>` |
| Business goal | `<why this schema is needed>` |
| Scope | `<covered tables and relationships>` |
| Non-goals | `<what is intentionally excluded>` |
| Related systems | `<upstream or downstream dependencies>` |

## 2. Table Inventory

| Table name | Purpose | Core relationship |
| --- | --- | --- |
| `<table_name>` | `<what the table stores>` | `<main linked table or entity>` |

Repeat one row per table.

## 3. Single Table Design

Repeat this section for each table.

### 3.x `<table_name>`

#### 3.x.1 Table Summary

| Item | Content |
| --- | --- |
| Table name | `<table_name>` |
| Purpose | `<why the table exists>` |
| Primary key | `<primary key column>` |
| Write pattern | `<insert/update characteristics>` |
| Read pattern | `<query characteristics>` |

#### 3.x.2 Field Definitions

| Column | Type | Nullable | Default | Key role | Description |
| --- | --- | --- | --- | --- | --- |
| `<column_name>` | `<db_type>` | `<yes/no>` | `<default>` | `<pk/unique/index/normal>` | `<field meaning and business rule>` |

#### 3.x.3 Index Design

| Index name | Type | Columns | Purpose |
| --- | --- | --- | --- |
| `<index_name>` | `<primary/unique/normal>` | `<column list>` | `<why the index exists>` |

#### 3.x.4 Constraints and Data Rules

| Rule type | Content |
| --- | --- |
| Unique constraints | `<details>` |
| Foreign key or logical relation | `<details>` |
| Enum or status values | `<details>` |
| Data validation | `<details>` |
| Soft delete strategy | `<details or N/A>` |
| Audit fields | `<created_at, updated_at, operator, etc.>` |

#### 3.x.5 Lifecycle and Capacity

| Item | Content |
| --- | --- |
| Retention | `<retention rule>` |
| Growth expectation | `<estimated scale>` |
| Archival strategy | `<if applicable>` |
| Partition or sharding note | `<if applicable>` |

#### 3.x.6 Risks and Open Questions

- `<risk or tradeoff>`
- `<open question>`

## 4. Cross-Table Relationships

| From table | To table | Relationship | Notes |
| --- | --- | --- | --- |
| `<table_a>` | `<table_b>` | `<1:1 / 1:N / N:N>` | `<business meaning>` |

## 5. Consistency and Transaction Notes

- transaction boundaries
- eventual consistency concerns
- concurrent update considerations

## 6. Migration and Compatibility Impact

- existing table changes
- backward compatibility concerns
- rollout or migration notes

## 7. Appendix

Optional:

- sample SQL
- sample seed data
- naming glossary

Appendix material must not replace the structured sections above.
