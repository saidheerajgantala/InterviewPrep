# Bulkhead

## 0) Metadata
- **Name**: Bulkhead
- **Canonical Path**: Patterns/005_ResiliencePatterns/Bulkhead.md
- **Category**: 005 Resilience Patterns
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: isolation, concurrency-limits, resource-pools, blast-radius

---

## 1) TL;DR (Executive Summary)
- **Problem**: One noisy dependency or tenant can exhaust shared resources and take down the whole system.
- **Solution (essence)**: Isolate resources into compartments (concurrency pools/queues) so a failure is contained.

---

## 2) Concepts
- Separate queues/pools per dependency, endpoint, or tenant tier.
- Limit concurrent work per bulkhead; overflow is queued, rejected, or shed.

## 3) Architecture
```mermaid
flowchart LR
  IN[Incoming Requests] --> CL1[Pool A (N workers)]
  IN --> CL2[Pool B (M workers)]
  CL1 --> S1[Dependency A]
  CL2 --> S2[Dependency B]
```

---

## 4) Properties & Tradeoffs
| Aspect | Pros | Cons | Notes |
|---|---|---|---|
| Isolation | Contains failures | Requires sizing | Start conservative |
| Availability | Protects core paths | May reject some traffic | Prioritize critical tiers |
| Performance | Predictable under load | Less utilization if imbalanced | Dynamic rebalancing helps |

---

## 5) Implementation Guide
- Identify compartments: by dependency, endpoint, tenant tier (gold/silver/bronze).
- For each: set max concurrency, queue capacity, queue timeout.
- On overflow: reject fast with friendly errors; or degrade (reduced features).
- Combine with circuit breakers and timeouts.

---

## 6) Pitfalls & Edge Cases
- Overly static limits cause starvation; add autosizing or SLO-based adjusters.
- Shared thread pools defeat isolation; ensure separate executors/pools.
- Head-of-line blocking in large queues; keep queues short.

### Edge-case Checklist
- Graceful degradation per bulkhead (fallbacks).
- Separate metrics per compartment.
- Backpressure signals to callers.

---

## 7) Observability
- Metrics: in-flight per pool, queue depth, queue wait, reject rate, success/error per pool.
- Alerts: sustained rejects in critical pool, queue wait > threshold.

---

## 8) References
- Nygard "Release It!"; resilience engineering blogs; service mesh concurrency limits.
