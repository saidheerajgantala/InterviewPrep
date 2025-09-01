# Query Optimization

## 0) Metadata
- **Name**: Query Optimization
- **Canonical Path**: Patterns/011_Performance/Optimization/Query_Optimization.md
- **Category**: 011 Performance / Optimization
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: sql, indexing, plans, caching

---

## 1) TL;DR (Executive Summary)
- Use indexes, avoid N+1, examine plans, and cache results for hotspots.

---

## 2) Techniques
- Proper indexing (covering, composite), selective predicates, limit scans.
- Denormalize/materialize for heavy reads; avoid functions on indexed columns.
- Paginate with cursors; batch queries; use prepared statements.

## 3) Implementation Guide
- EXPLAIN/ANALYZE; monitor slow query logs.
- Revise schema/indexes based on top queries; test with production-like data.

---

## 4) Observability
- Metrics: query latency percentiles, buffer hit ratio, temp disk usage.

---

## 5) References
- Vendor docs; "Use the Index, Luke!"; DDIA.
