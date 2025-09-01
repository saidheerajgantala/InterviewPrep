# Latency

## 0) Metadata
- **Name**: Latency
- **Canonical Path**: Patterns/011_Performance/Metrics/Latency.md
- **Category**: 011 Performance / Metrics
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: latency, percentiles, tail, SLO

---

## 1) TL;DR (Executive Summary)
- Track latency percentiles (p50/p95/p99) and set SLOs; optimize tail latency.

---

## 2) Concepts
- Distribution matters; averages hide pain.
- Tail latency amplification in microservices chains.
- Budgets per hop; timeouts to bound overall latency.

## 3) Implementation Guide
- Measure server and client latencies separately.
- Use histograms with sufficient buckets; record per-route/per-dependency.
- SLOs: e.g., p99 ≤ 300ms for 30d; error budget for breaches.

---

## 4) Observability
- Dashboards per service and critical paths; alert on sustained p99 breaches + error spikes.

---

## 5) References
- The Tail at Scale (Dean/Barroso); SRE book.
