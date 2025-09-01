# CAP Theorem

## 0) Metadata
- **Name**: CAP Theorem
- **Canonical Path**: Patterns/003_DistributedSystems/ConsistencyModels/CAP_Theorem.md
- **Category**: 003 Distributed Systems
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: cap, availability, consistency, partition-tolerance

---

## 1) TL;DR (Executive Summary)
- **Claim**: In the presence of a network partition, a distributed system must choose between Consistency and Availability.
- **Implication**: All practical systems must be Partition-tolerant; you trade C vs A during partitions.

---

## 2) Definitions
- **Partition Tolerance (P)**: System continues despite message loss/delay between nodes.
- **Consistency (C)**: All clients see the same data at the same time (linearizable view).
- **Availability (A)**: Every request receives a non-error response (not necessarily the latest data).

---

## 3) Intuition
- When the network splits, replicas can’t coordinate timely; either reject requests (favor C) or serve possibly stale data (favor A).

---

## 4) Architecture Illustration
```mermaid
flowchart LR
  subgraph Region A
    A1[(Replica)]
  end
  subgraph Region B
    B1[(Replica)]
  end
  A1 -.-x-.- B1
  U1[Client A] --> A1
  U2[Client B] --> B1
```

---

## 5) Properties & Guarantees
- During normal ops, systems can offer both high C and A; CAP focuses on partitions.
- Choosing C under partition → some requests fail/time out.
- Choosing A under partition → clients may observe divergent states (eventual repair).

---

## 6) Tradeoffs & Examples
| Posture | Behavior | Example |
|---|---|---|
| CP | Prefer consistent state; may reject/timeout | Spanner (CP with high availability via sync replication) |
| AP | Prefer availability; converge later | Dynamo/Cassandra (tunable quorum) |

---

## 7) Implementation Notes
- Design explicit partition behavior (error budgets, degraded modes).
- Provide client semantics: read-your-writes, monotonic reads when possible.

---

## 8) References
- Gilbert & Lynch (Brewer’s conjecture and the feasibility of consistent, available, partition-tolerant web services), Brewer’s keynote.
