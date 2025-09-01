# Consistency (SLIs for Data Correctness)

## 0) Metadata
- **Name**: Consistency
- **Canonical Path**: Patterns/011_Performance/Metrics/Consistency.md
- **Category**: 011 Performance / Metrics
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: consistency, sli, staleness, correctness

---

## 1) TL;DR (Executive Summary)
- Define SLIs for data freshness and correctness (staleness windows, read-your-writes) and track violations.

---

## 2) Concepts
- Freshness SLI: max age of data served; staleness distribution.
- Read-after-write success rate; monotonic read guarantees.

## 3) Implementation Guide
- Emit timestamps/version tokens; measure time between write and visible read.
- Record consistency-related errors (stale reads, duplicates).

---

## 4) Observability
- Dashboards for freshness percentiles per endpoint; alerts on SLO breaches.

---

## 5) References
- DDIA consistency; SRE on SLIs beyond latency/availability.
