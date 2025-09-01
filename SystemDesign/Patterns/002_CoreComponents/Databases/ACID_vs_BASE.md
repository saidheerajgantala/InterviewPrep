# ACID vs BASE

## 0) Metadata
- **Name**: ACID vs BASE
- **Canonical Path**: Patterns/002_CoreComponents/Databases/ACID_vs_BASE.md
- **Category**: 002 Core Components
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: transactions, consistency, availability, semantics

---

## 1) TL;DR (Executive Summary)
- **Problem**: Choosing transaction semantics affects correctness, performance, and availability.
- **Solution (essence)**: ACID ensures strict correctness; BASE trades strict consistency for availability and scale.
- **Use when**: ACID for money/orders; BASE for high-scale, tolerant-of-staleness systems.
- **Key tradeoff**: Latency/availability vs strict consistency.

---

## 2) Definitions
- **ACID**: Atomicity, Consistency, Isolation, Durability.
- **BASE**: Basically Available, Soft state, Eventual consistency.
- Relation to CAP/PACELC: consistency vs availability under partitions and normal ops.

---

## 3) Intuition & Examples
- ACID: bank transfers, inventory decrements.
- BASE: social feeds, analytics counters, caches.

---

## 4) Properties & Guarantees
- ACID: serializable/strong guarantees (depending on isolation level).
- BASE: convergent state; clients observe staleness and anomalies.

---

## 5) Tradeoffs
| Aspect | ACID | BASE | Notes |
|---|---|---|---|
| Consistency | strong | eventual | Causal variants exist |
| Latency | higher | lower | Coordination vs async |
| Availability | lower under partition | higher | Retry-based |
| Complexity | schema, migrations | conflict resolution | CRDTs may help |

---

## 6) Implementation Guide
- Choose isolation level carefully (RC/RR/SI/Serializable) per workload.
- For BASE: design idempotent operations; use versioning and reconciliation.
- Communicate guarantees to clients (docs, SLOs).

---

## 7) Pitfalls & Edge Cases
- Misassumed guarantees; bugs from stale reads.
- Cross-service transactions; prefer sagas/compensation.

---

## 8) References
- DDIA; Spanner/Calvin/Percolator papers; Jepsen analyses on consistency.
