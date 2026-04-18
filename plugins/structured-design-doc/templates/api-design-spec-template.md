# REST API Design Specification Template

## 1. Document Overview

| Item | Content |
| --- | --- |
| Document title | `<project or module name>` |
| Business goal | `<why this API exists>` |
| Scope | `<covered endpoints>` |
| Non-goals | `<what is intentionally excluded>` |
| Target callers | `<client, internal service, admin console, etc.>` |

## 2. API Inventory

| API name | Method | Path | Purpose |
| --- | --- | --- | --- |
| `<api_name>` | `<GET/POST/PUT/PATCH/DELETE>` | `<path>` | `<what the endpoint does>` |

Repeat one row per endpoint.

## 3. Single API Design

Repeat this section for each endpoint.

### 3.x `<api_name>`

#### 3.x.1 Basic Information

| Item | Content |
| --- | --- |
| API name | `<api_name>` |
| Method | `<HTTP method>` |
| Path | `<endpoint path>` |
| Purpose | `<business purpose>` |
| Caller | `<expected caller>` |
| Authentication | `<auth requirement>` |
| Authorization | `<permission requirement>` |
| Idempotency | `<yes/no and rule>` |

#### 3.x.2 Request Headers

| Header | Required | Example | Description |
| --- | --- | --- | --- |
| `<header_name>` | `<yes/no>` | `<example>` | `<meaning>` |

#### 3.x.3 Path Parameters

| Name | Type | Required | Example | Description |
| --- | --- | --- | --- | --- |
| `<param_name>` | `<type>` | `<yes/no>` | `<example>` | `<meaning>` |

#### 3.x.4 Query Parameters

| Name | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `<param_name>` | `<type>` | `<yes/no>` | `<default>` | `<meaning>` |

#### 3.x.5 Request Body

| Field | Type | Required | Example | Description |
| --- | --- | --- | --- | --- |
| `<field_name>` | `<type>` | `<yes/no>` | `<example>` | `<meaning>` |

#### 3.x.6 Response Body

| Field | Type | Always present | Example | Description |
| --- | --- | --- | --- | --- |
| `<field_name>` | `<type>` | `<yes/no>` | `<example>` | `<meaning>` |

#### 3.x.7 Status Codes and Business Errors

| Code | Condition | Response meaning |
| --- | --- | --- |
| `<status or error code>` | `<when it happens>` | `<how the caller should interpret it>` |

#### 3.x.8 Pagination, Filtering, and Sorting

| Item | Content |
| --- | --- |
| Pagination | `<rule or N/A>` |
| Filtering | `<supported filters or N/A>` |
| Sorting | `<supported sort options or N/A>` |
| Rate limiting | `<rule or N/A>` |

#### 3.x.9 Examples

- request example
- success response example
- error response example

#### 3.x.10 Risks and Open Questions

- `<risk or tradeoff>`
- `<open question>`

## 4. Shared Resource Model Notes

Document any resource fields reused across multiple endpoints.

## 5. Common Error Response Format

| Field | Type | Description |
| --- | --- | --- |
| `<field_name>` | `<type>` | `<meaning>` |

## 6. Compatibility and Versioning

- backward compatibility policy
- path or header versioning policy
- rollout constraints

## 7. Appendix

Optional:

- curl examples
- sample client calls
- glossary

Appendix material must not replace the structured sections above.
