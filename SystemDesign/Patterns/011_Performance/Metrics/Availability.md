# Availability

## 0) Metadata
- **Name**: Availability
- **Canonical Path**: Patterns/011_Performance/Metrics/Availability.md
- **Category**: 011 Performance / Metrics
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: availability, sla, slo, error-budget

---

## 1) TL;DR (Executive Summary)
- Define SLOs for availability (successful requests / total) and manage error budgets.

---

## 2) Concepts
- SLA vs SLO vs SLI; uptime vs success-based availability.
- Error budget = 1 - SLO; use to govern changes.

## 3) Implementation Guide
- Compute rolling success rate for key user journeys.
- Align alerting to SLO burn rates, not single spikes.

---

## 4) Observability
- Dashboards per product/region; report monthly SLO compliance.

---

## 5) References
- SRE book on SLIs/SLOs/SLAs.
