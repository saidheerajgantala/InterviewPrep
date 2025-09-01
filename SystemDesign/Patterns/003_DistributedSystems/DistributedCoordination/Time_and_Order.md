# Time and Order in Distributed Systems

## 0) Metadata
- **Name**: Time and Order
- **Canonical Path**: Patterns/003_DistributedSystems/DistributedCoordination/Time_and_Order.md
- **Category**: 003 Distributed Systems
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: clocks, lamport, vector-clocks, causality, ordering

---

## 1) TL;DR (Executive Summary)
- **Problem**: No global clock; message delays and skew make ordering ambiguous.
- **Solution (essence)**: Use logical clocks (Lamport/vector), sequence numbers, or consensus to define order.

---

## 2) Concepts
- Physical clocks: NTP/PTP; drift and skew.
- Logical clocks: Lamport timestamps (happened-before), vector clocks (causality).
- Total order via consensus vs causal/partial orders.

## 3) Architecture
```mermaid
flowchart LR
  A[Process A]\n(LC:5) --> B[Process B]\n(LC:3)
  B --> A
```

---

## 4) Properties & Tradeoffs
| Mechanism | Pros | Cons |
|---|---|---|
| Physical clock | Simple | Skew; unsafe for ordering |
| Lamport | Defines partial order | No concurrency info |
| Vector | Captures causality | Grows with participants |
| Consensus | Total order | Latency, cost |

---

## 5) Implementation Notes
- Use monotonic IDs or per-shard sequence for local ordering.
- For cross-node ordering, embed logical timestamps; resolve conflicts deterministically.

---

## 6) References
- Lamport (1978), Vector clocks, Google TrueTime, Spanner.
