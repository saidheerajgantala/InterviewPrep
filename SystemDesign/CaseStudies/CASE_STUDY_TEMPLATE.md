# 📝 System Design Case Study Template

## **Problem Statement**

* Clearly define the system to be designed.
* Key requirements and constraints.
* Scale considerations (users, data volume, traffic).
* Example use cases.

---

## **Context & Goals**

* Business objectives and success criteria (e.g., conversion rate, latency targets).
* Primary user personas and their key workflows.
* KPIs/SLIs to move (e.g., p99 latency, availability, cost/request).

---

## **Constraints & Decision Drivers**

* Hard constraints (regulatory, platform, regions, data residency, SSO/IAM).
* SLOs/SLA targets (latency, availability, durability, RTO/RPO).
* Growth assumptions (traffic CAGR, data growth, feature roadmap).
* Budget/operational constraints (cost ceiling, team skillset, tooling).

---

## **Step 1: Requirements Clarification**

* Functional requirements (what the system should do).
* Non-functional requirements (performance, reliability, availability).
* Out of scope items.
* Assumptions made.

---

## **Step 2: Back-of-the-envelope Estimation**

* Traffic estimates (QPS, read/write ratio).
* Storage estimates (data size, growth rate).
* Bandwidth estimates.
* Memory requirements.
* Peak factor and diurnal patterns (avg → p95 multiplier).
* Retention policy (cold vs hot data; TTLs).
* Core formulas (example):
  * Requests/day = DAU × actions/user
  * Average QPS = Requests/day ÷ 86,400
  * Peak QPS ≈ Average QPS × peak_multiplier
  * Storage/year = daily_ingest × 365 × (replication_factor)

---

## **Step 3: System Interface Definition**

* API endpoints.
* Request/response formats.
* Error handling.
* Versioning strategy (e.g., URI versioning v1/v2, backward compatibility policy).
* Idempotency (idempotency keys for POST/PUT; retry semantics).
* Pagination and filtering (cursor vs offset; limits; consistency implications).
* Rate limits and quotas (by API key/user/IP; burst vs steady-state).

---

## **Step 4: High-Level Design**

* Core components.
* Data flow between components.
* Initial architecture diagram.

---

## **Step 5: Database Design**

* Data model.
* Schema design.
* Database choice (SQL vs NoSQL).
* Indexing strategy.
* Data partitioning approach.
* Partition/shard keys and hot-partition mitigation.
* Consistency model (strong/eventual/causal) and read/write paths.

---

## **Step 6: Detailed Component Design**

### **Component 1: [Name]**

* Purpose and responsibilities.
* Internal design.
* Algorithms and data structures used.
* Scaling considerations.
* Dependencies and failure handling.

### **Component 2: [Name]**

* Purpose and responsibilities.
* Internal design.
* Algorithms and data structures used.
* Scaling considerations.
* Dependencies and failure handling.

*(Repeat for all major components)*

---

## **Step 7: Identifying and Resolving Bottlenecks**

* Potential bottlenecks.
* Single points of failure.
* Consistency vs. availability tradeoffs.
* Mitigation strategies.

---

## **Step 8: Scaling the Design**

### **Horizontal Scaling**

* Stateless components.
* Database sharding.
* Load balancing strategy.

### **Caching Strategy**

* What to cache.
* Cache invalidation approach.
* Cache placement.
* TTL, eviction policy, warming strategy.

### **Load Balancing**

* Algorithm choice.
* Session handling.
* Health checks.

---

## **Step 9: Monitoring and Alerting**

* Key metrics to track.
* Alerting thresholds.
* Logging strategy.
* Debugging approaches.
* SLOs and targets (examples): p99 latency ≤ [X] ms; availability ≥ [Y]%; error rate ≤ [Z]%.
* Alert examples:
  * Page: p99 latency > [X]×1.2 for 10m AND error_rate > [Z]×2.
  * Ticket: cache hit ratio < [HIT%] for 30m.

---

## **Step 10: Security Considerations**

* Authentication and authorization.
* Data encryption.
* Rate limiting.
* DDoS protection.
* Secrets management and key rotation.
* Privacy/PII classification, retention, and deletion (compliance: GDPR/HIPAA).
* Audit logging and access controls.

---

## **Step 11: Deployment, Migration & Rollout**

* Deployment strategy (blue/green, canary, rolling).
* Schema migrations and backward compatibility.
* Data backfill and dual-write/dual-read plan.
* Feature flags and kill switches.
* Multi-region/DR strategy (RTO/RPO, failover drills).

---

## **Step 12: Reliability (SLIs, SLOs, Error Budgets)**

* Core SLIs (latency, availability, durability, correctness).
* SLO targets and error budget policy.
* Degradation and fallback behaviors.
* Incident response process and escalation.

---

## **Step 13: Cost Model & Capacity Planning**

* Monthly cost estimate by component (compute, storage, egress, managed services).
* Cost-control levers (caching, tiered storage, compression, reserved instances).
* Capacity headroom targets and autoscaling triggers.

---

## **Step 14: Testing & Chaos**

* Unit, integration, load, and soak testing plans.
* Fault injection and chaos experiments.
* Rollback strategy and verification gates.

---

## **Runbooks**

* Common operational playbooks and on-call actions.
* Standard dashboards and queries.

---

## **Risks & Open Questions**

* Key risks, mitigations, and owners.
* Open questions, decisions pending, and timelines.

---

## **Tradeoff Summary**

| Design Decision | Pros | Cons | Alternatives Considered |
|---|---|---|---|
| Decision 1 |  |  |  |
| Decision 2 |  |  |  |
| Decision 3 |  |  |  |

---

## **Final Architecture Diagram**

```
[Placeholder for final architecture diagram]
```

---

## **Real-world Examples**

* Similar systems in production.
* How they solved similar challenges.
* Lessons learned from existing implementations.

---

## **Checklist**

* Requirements, constraints, and non-goals documented.
* Estimations and capacity plan with peak factors.
* Interfaces versioned; idempotency and rate limits defined.
* Data model, partitioning, and consistency tradeoffs addressed.
* Caching, load balancing, and scaling strategies defined.
* SLOs, alerts, and dashboards defined.
* Security, privacy, and compliance considerations covered.
* Deployment, migration, and rollback plan ready.
* Cost model and optimization levers identified.
* Runbooks and incident response in place.

---

## **Summary**

* Key design decisions.
* How the design meets requirements.
* Future improvements and considerations. 