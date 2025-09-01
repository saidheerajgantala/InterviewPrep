# REST API Design

## 0) Metadata
- **Name**: REST API Design
- **Canonical Path**: Patterns/001_Fundamentals/Basics/REST_API_Design.md
- **Category**: 001 Fundamentals
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: rest, http, api, versioning, idempotency

---

## 1) TL;DR (Executive Summary)
- **Problem**: Exposing business capabilities to clients reliably and evolvably.
- **Solution (essence)**: Resource-oriented HTTP APIs with consistent methods, representations, and contracts.
- **Use when**: Public/internal services over HTTP with broad client support.
- **Avoid when**: Tight coupling/latency constraints demand RPC/binary (gRPC) or event-driven only.
- **Key tradeoff**: Simplicity and ubiquity vs chatty interactions and textual overhead.

---

## 2) Principles & Context
- Resources with stable identifiers (URIs); representations (JSON preferred).
- Uniform interface (GET, POST, PUT, PATCH, DELETE), statelessness, cacheability.
- Backward-compatible evolution via additive changes, versioning when necessary.

## 3) Decision Drivers
- Client ecosystem (browsers, mobile), caching needs, latency/SLOs.
- Security/compliance (authn/z, PII), multi-tenancy.

---

## 4) Intuition & Baseline
- Naive: Ad-hoc endpoints and payloads.
- Fails: Inconsistent semantics, breaking changes, poor client reuse.
- Insight: Consistent REST semantics + standards enable evolvability and tooling.

---

## 5) API Surface (Optimized Approach)
- **Core Concept**:
  - Nouns in paths, verbs as HTTP methods.
  - Idempotency for GET/PUT/DELETE; POST with idempotency keys where needed.
  - Standard status codes and error shapes.
- **Happy-path flow**: Client requests a resource, server validates/acts, returns representation with proper headers.
- **Degraded**: Partial errors, rate limit (429), retries with backoff (idempotent only), circuit breakers.

---

## 6) Contracts & Examples
### 6.1 Resource Modeling
```
GET /v1/users/{userId}
GET /v1/users?limit=50&cursor=...
POST /v1/users
PATCH /v1/users/{userId}
DELETE /v1/users/{userId}
```

### 6.2 Request/Response Shapes
```
POST /v1/users
Idempotency-Key: 2e41...
Content-Type: application/json

{ "email": "a@example.com", "name": "Ada" }

201 Created
Location: /v1/users/123
{ "id": "123", "email": "a@example.com", "name": "Ada" }
```

### 6.3 Errors (standard shape)
```
400 Bad Request
{ "error": { "code": "validation_failed", "message": "email invalid", "field": "email", "traceId": "..." } }
```

### 6.4 Pagination
- Prefer cursor-based: `?limit=50&cursor=abc` with response `{ items: [...], nextCursor: "..." }`.

### 6.5 Filtering & Sorting
- Whitelist fields: `?status=active&sort=-createdAt`.

---

## 7) Properties & Guarantees
- Scalability: Cacheable GETs, CDN-friendly; statelessness supports horizontal scale.
- Availability: Explicit timeouts; retries for idempotent ops.
- Consistency: Resource-specific; ETags for conditional requests.
- Latency: Keep payloads compact; batch endpoints; compression.

---

## 8) Tradeoffs
| Aspect | Pros | Cons | Notes |
|---|---|---|---|
| Simplicity | Ubiquitous tooling | Less strict contracts vs gRPC | Use OpenAPI |
| Performance | CDN caching | Verbose JSON | Gzip/Brotli, partial responses |
| Evolvability | Back-compat via additive | Breaking changes require versioning | Sunset headers |
| Security | Mature ecosystem | Public attack surface | OAuth2/OIDC, mTLS internal |

---

## 9) Implementation Guide
- OpenAPI/Swagger first; generate clients/servers.
- Consistent error shape; trace IDs; correlation headers.
- Timeouts (server < 1s typical), limits, rate limiting.
- Idempotency keys for POST that create/modify resources.

---

## 10) Common Pitfalls & Edge Cases
- Breaking changes without versioning; add-only first.
- Overfetching/underfetching; add `fields` or tailored endpoints.
- Cursor drift and duplicates; include stable sort keys.
- Missing idempotency → duplicate side effects on retries.

### Edge-case Checklist
- Validation and error shape consistency
- Conditional requests (ETag/If-None-Match)
- Pagination limits and caps
- Consistent timestamp/time zone handling (ISO-8601 UTC)

---

## 11) Observability
- Metrics: request counts, latency percentiles, error rates by code, payload sizes.
- Logs: structured; include method, path, status, userId/tenantId, traceId.
- Tracing: client and server spans, propagate IDs.

---

## 12) Security & Compliance
- Authn: OAuth2/OIDC; Authz: RBAC/ABAC; scopes/claims.
- Input validation; output encoding; rate limit per principal.
- PII handling; data minimization; audit logging.

---

## 13) Versioning & Deprecation
- Path versioning (`/v1`) or header-based; backward-compatible changes preferred.
- Deprecation policy; sunset headers; migration guidance.

---

## 14) References
- RFC 7231 (HTTP/1.1), RFC 9110; Fielding dissertation; Microsoft REST API Guidelines; JSON:API; OpenAPI.
