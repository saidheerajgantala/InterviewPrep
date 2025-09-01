# Data Denormalization

## 0) Metadata
- **Name**: Data Denormalization
- **Canonical Path**: Patterns/011_Performance/Optimization/Data_Denormalization.md
- **Category**: 011 Performance / Optimization
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: denormalization, materialized-views, projections

---

## 1) TL;DR (Executive Summary)
- Precompute and store read-optimized structures to avoid costly joins/aggregations.

---

## 2) Approaches
- Materialized views, summary tables, pre-joined documents, secondary indexes.
- Event-driven projections via outbox/CDC.

## 3) Implementation Guide
- Identify hot queries; design read models tailored to them.
- Keep source of truth authoritative; rebuild projections when needed.

---

## 4) Tradeoffs
- Faster reads vs write amplification and complexity.

---

## 5) References
- DDIA; CQRS/Projection patterns.
