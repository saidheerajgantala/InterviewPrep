# Throughput

## 0) Metadata
- **Name**: Throughput
- **Canonical Path**: Patterns/011_Performance/Metrics/Throughput.md
- **Category**: 011 Performance / Metrics
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: throughput, rps, qps, saturation

---

## 1) TL;DR (Executive Summary)
- Measure requests per second (RPS/QPS) and bytes/s; track versus capacity and saturation.

---

## 2) Concepts
- Little’s Law: L = λ W; relate concurrency, arrival rate, and latency.
- Backpressure and queue depth as early warning.

## 3) Implementation Guide
- Track per-endpoint and per-dependency throughput; separate reads vs writes.
- Correlate with CPU/mem/IO utilization; identify bottlenecks.

---

## 4) Observability
- Alerts on sustained saturation with rising latency/error rates.

---

## 5) References
- SRE book; performance engineering guides.
