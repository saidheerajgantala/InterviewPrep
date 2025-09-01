# Vertical vs Horizontal Scaling

## 0) Metadata
- **Name**: Vertical vs Horizontal Scaling
- **Canonical Path**: Patterns/001_Fundamentals/ScalabilityConcepts/Vertical_vs_Horizontal_Scaling.md
- **Category**: 001 Fundamentals
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: scaling, capacity, elasticity, cost

---

## 1) TL;DR (Executive Summary)
- **Problem**: Handling growth in traffic/data without violating SLOs or budgets.
- **Solutions**:
  - **Vertical (Scale Up)**: Bigger machine (CPU/RAM/IO).
  - **Horizontal (Scale Out)**: More machines behind a load balancer/partitioner.
- **Use when**:
  - Vertical: Simple, fast win; stateful monoliths; licensing per node.
  - Horizontal: High availability, elasticity, large-scale throughput.
- **Key tradeoff**: Simplicity vs elasticity and fault tolerance.

---

## 2) Problem & Context
- Systems face variable load (diurnal peaks, launches) and growth trends.
- Need capacity planning and a scaling strategy that matches architecture.

## 3) Decision Drivers
- SLOs: p99 latency, availability target, error budget.
- Workload: CPU vs memory vs IO bound; read vs write heavy; statefulness.
- Constraints: Vendor licensing, vertical SKU ceilings, cloud primitives.
- Cost: Per-core licensing vs fleet management; ops maturity.

---

## 4) Intuition & Baseline
- Naive: One server, keep upgrading.
- Fails: Vertical limits, single point of failure, long maintenance windows.
- Insight: Distribute across nodes for resilience and parallelism.

---

## 5) Approaches
- **Vertical Scaling**:
  - Upgrade instance size; faster CPU, more RAM/IO.
  - Minimal code changes; best for stateful workloads.
- **Horizontal Scaling**:
  - Add nodes; share-nothing; stateless services; partition state.
  - Requires LB, session affinity or externalizing state.

---

## 6) Architecture
```mermaid
flowchart LR
  subgraph Vertical (Scale Up)
    A[App vCPU x64 RAM x512] --> DS[(DB)]
  end
  subgraph Horizontal (Scale Out)
    LB[(Load Balancer)] --> N1[App Node]
    LB --> N2[App Node]
    N1 --> DS2[(DB/Cache)]
    N2 --> DS2
  end
```

---

## 7) Properties & Guarantees
- Vertical: lower latency in-process, simpler ops; limited HA.
- Horizontal: HA via redundancy; elasticity; may add coordination complexity.

---

## 8) Tradeoffs
| Aspect | Vertical (Up) | Horizontal (Out) | Notes |
|---|---|---|---|
| Simplicity | Very high | Moderate | Fleet mgmt adds complexity |
| Elasticity | Limited | High | Autoscaling groups |
| Availability | Low (SPOF) | High | Multi-AZ nodes |
| Cost | Large instances pricey | Pay-per-node | Right-size and autoscale |
| Limits | Hard SKU caps | Scaling curve smoother | Data partitioning needed |

---

## 9) Implementation Guide
- Start vertical for quick wins; instrument to find true bottlenecks.
- Prepare for horizontal: stateless services, externalize session/state to caches/DBs.
- Autoscaling policies: target utilization, cooldowns, predictive scaling.

---

## 10) Pitfalls & Edge Cases
- Vertical: hit SKU ceiling; long restarts; noisy neighbor on shared hosts.
- Horizontal: stateful session stickiness; skew/hot shards; coordination storms.

### Edge-case Checklist
- Health checks and graceful drain before scale-in.
- Warm-up and connection pre-seeding.
- Idempotent handlers to tolerate retries.

---

## 11) Observability
- Metrics: utilization (CPU/mem/IO), queue depth, p95/p99 latency, error rate.
- Alerts: sustained saturation + error spikes; oscillating autoscaler.

---

## 12) References
- Nygard, Release It!; SRE Book (capacity planning); cloud provider instance docs.
