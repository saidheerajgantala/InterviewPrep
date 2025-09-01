# 📝 Distributed Database (OLTP) Case Study

## **Problem Statement**

* Design a globally distributed, highly available relational database service for OLTP workloads (orders/payments/profile) with low-latency reads and strongly consistent writes.
* Key requirements and constraints.
* Scale considerations (millions of users, multi-region, bursty traffic).
* Example use cases: ecommerce orders, payments, user profiles.

---

## **Context & Goals**

* Business objectives and success criteria:
  * p99 write latency ≤ 150 ms within region; multi-region read p99 ≤ 200 ms.
  * Availability ≥ 99.99% per month; zero RPO within region; RPO ≤ 5s cross-region.
  * Strong consistency for write transactions; bounded-staleness or strong reads configurable.
  * Online schema changes; multi-tenant isolation; cost-efficiency.
* KPIs/SLIs: p50/p95/p99 latency, error rate, write commit rate, replication lag, leader failover time.

---

## **Constraints & Decision Drivers**

* Regulatory: data residency (EU), PCI for payments, PII encryption, audit logging.
* SLOs/SLA: latency targets above; durability ≥ 11x9s for committed writes (region), cross-region RPO 5s.
* Growth: 3× YoY traffic; weekly peaks 2.5× average.
* Budget: prefer commodity instances; managed storage; ops ≤ 2 FTE.

---

## **Step 1: Requirements Clarification**

* Functional: SQL interface, transactions (ACID), indexes, secondary indexes, CDC stream, point-in-time restore.
* Non-functional: availability 99.99%, durability, strong consistency for writes, horizontal scalability.
* Out of scope: HTAP analytics; full-text search; graph queries.
* Assumptions: client apps in 3 regions; majority of writes region-local; cross-region reads common.

---

## **Step 2: Back-of-the-envelope Estimation**

* Traffic estimates:
  * DAU 5M; 4 write tx/user/day → 20M writes/day ≈ 231.5 writes/sec avg.
  * Peak factor 10× → 2.3k writes/sec peak. Read:write = 8:1 → ~18k reads/sec peak.
* Storage estimates:
  * Avg row 1.5 KB; 20M/day → 30 GB/day logical; ×3 replicas → 90 GB/day physical.
  * 1-year hot retention 11 TB physical; cold archive beyond.
* Bandwidth: 2.3k writes/sec × 1.5 KB × 3 replicas ≈ 10.4 MB/s cross-node.
* Memory: working set ≈ hot keys (last 7 days) ~210 GB logical → cache per shard ~8–16 GB.

---

## **Step 3: System Interface Definition**

* SQL gateway: Postgres-compatible wire protocol; CRUD; transactions; prepared statements.
* Error handling: retriable errors (40001 serialization), non-retriable (23505 unique_violation), 5xx for internal.
* Versioning: rolling upgrades; backwards-compatible schema changes.
* Idempotency: client-supplied idempotency key for POST-like operations; dedupe window 24h.
* Rate limits/quotas: per-tenant QPS, connection caps.

---

## **Step 4: High-Level Design**

* Components:
  * SQL Gateways (stateless) with connection pooling.
  * Transaction Coordinator (per-shard Raft leader) with 2 followers.
  * Storage Nodes (LSM-based KV + MVCC; per-shard log-structured store).
  * Placement/Metadata Service (shard map; Raft or etcd quorum).
  * Change Data Capture (CDC) pipeline to stream replicas/analytics.
  * Backup/Restore service (incremental + snapshots; PITR via WAL).

* Data flow:
  * Client → Gateway → Route by shard key → Raft leader → commit → replicate → ack → CDC/backup async.

---

## **Step 5: Database Design**

* Data model: relational (tables, PK, FKs, secondary indexes); MVCC for snapshots.
* Schema design: narrow hot tables (orders, payments), normalized + selective denorm for read paths.
* Sharding: consistent hashing on tenant_id + entity_id; rebalancer keeps shard sizes within 20%.
* Indexes: PK clustered, secondary indexes per query shape; write amplification tracked.
* Consistency model:
  * Writes: linearizable within shard (Raft). Cross-shard: 2PC with coordinator; prefer saga for business workflows.
  * Reads: default follower-reads (bounded-staleness); option for read-your-writes (route to leader).
* Read/Write paths:
  * Write: txn begin → lock intents → Raft log append → quorum commit → index updates → ack.
  * Read: route to nearest replica; if strong read requested, route to leader or use lease-read.

---

## **Step 6: Detailed Component Design**

### SQL Gateway
* Purpose: connection mgmt, authn/z, SQL parse/plan (lightweight), routing, retry on leader changes.
* Scaling: stateless; autoscale on CPU/connections.
* Failure handling: retry with backoff on NotLeader; circuit breaker on shard hotspots.

### Transaction/Storage Nodes
* Purpose: Raft group per shard; MVCC storage; snapshot/compaction; secondary index maintenance.
* Data structures: LSM tree, WAL, tombstones, per-row MVCC versions.
* Scaling: split/merge shards; relocate leaders; follower-reads.

### Metadata/Placement Service
* Purpose: shard map; Raft membership; lease allocation; rebalancing decisions.
* Failure: quorum loss triggers read-only; bootstrap and snapshot recovery.

---

## **Step 7: Identifying and Resolving Bottlenecks**

* Hot shards (skewed keys) → key-salting, auto-split, request shedding.
* Raft leader CPU bound → move leader, raise raft tick, batch appends.
* Write amplification from many secondary indexes → limit indexes; async index builds.
* Large multi-shard transactions → favor saga/compensation; design idempotent steps.

---

## **Step 8: Scaling the Design**

### Horizontal Scaling
* Add shards; split/merge; place leaders near traffic; read replicas per region.

### Caching Strategy
* Gateway result cache for hot reads (TTL 100–500 ms) with invalidation via CDC.

### Load Balancing
* Anycast VIP to gateways; shard-aware routing; health checks; graceful drain.

---

## **Step 9: Monitoring and Alerting**

* Metrics: raft.commit_latency, raft.append_queue_len, wal.fs_sync_ms, repl.lag_ms, tx.abort_rate, p99_sql_latency, qps_by_tenant, cache.hit_ratio.
* Alerts:
  * Page: p99_sql_latency > 1.2×SLO for 10m AND tx.abort_rate > 5%.
  * Ticket: repl.lag_ms > 5000 for 30m.
* Logging/tracing: structured logs, trace IDs from gateway to storage.

---

## **Step 10: Security Considerations**

* Authn/z: IAM/OAuth to gateway; RBAC at database/schema/table; row-level policies.
* Encryption: TLS in transit; TDE at rest; KMS-managed keys; per-tenant keys optional.
* Rate limiting & quotas; audit logs; least-privilege service accounts.

---

## **Step 11: Deployment, Migration & Rollout**

* Deploy: blue/green gateways; rolling storage upgrades (leader stepdown first).
* Schema changes: online (ADD COLUMN, backfill with throttling); online index build.
* Data migration: dual writes during cutover; CDC backfill; rollback plan.

---

## **Step 12: Reliability (SLIs, SLOs, Error Budgets)**

* SLIs: latency p50/p95/p99, availability, replication lag, commit success rate.
* SLOs: write p99 ≤ 150 ms (regional), read p99 ≤ 200 ms; availability ≥ 99.99%.
* Error budget policy: slow-roll features when budget burn > threshold.

---

## **Step 13: Cost Model & Capacity Planning**

* Costs: compute (leaders > followers), storage IOPS, cross-region egress, backups, monitoring.
* Levers: TTLs, compression (zstd), compaction schedules, follower-reads to reduce leader load.
* Headroom: 40% CPU and IOPS per shard; autoscaling triggers.

---

## **Step 14: Testing & Chaos**

* Tests: Jepsen-style consistency checks; bank transfers (invariant); load/soak; failover drills.
* Chaos: kill leader/followers, drop packets, partition networks, slow disks.
* Rollback: versioned configs; WAL-based PITR validation.

---

## **Runbooks**

* Elevated abort rate → check hot shards, increase backoff, move leaders.
* High repl lag → throttle writes, prioritize follower catch-up, pause schema jobs.
* Leader flapping → inspect quorum health; replace bad node; adjust election timeouts.

---

## **Risks & Open Questions**

* Cross-shard transaction throughput limits; saga adoption coverage.
* Regulatory changes on data residency.
* Auto-rebalancer tuning for spiky traffic.

---

## **Tradeoff Summary**

| Design Decision | Pros | Cons | Alternatives Considered |
|---|---|---|---|
| Raft per shard | Strong consistency, simpler model | More leaders to manage, CPU overhead | Multi-Paxos, CRDTs for some tables |
| Follower reads | Offload leaders, low latency | Stale reads possible | Lease reads, quorum reads |
| 2PC for x-shard | Strong tx semantics | Latency and aborts under contention | Sagas for business processes |

---

## **Final Architecture Diagram**

```mermaid
flowchart LR
  subgraph Client
    App
  end
  App --> GW[SQL Gateway]
  GW -->|route by shard| L1[Shard Leader A]
  GW --> L2[Shard Leader B]
  L1 --> F1A[Follower A]
  L1 --> F1B[Follower B]
  L2 --> F2A[Follower A]
  L2 --> F2B[Follower B]
  subgraph Meta
    PL[Placement/Metadata (Raft)]
  end
  L1 -.-> PL
  L2 -.-> PL
  L1 --> CDC[(CDC/Backup)]
```

---

## **Real-world Examples**

* Google Spanner, CockroachDB, YugabyteDB.

---

## **Checklist**

* Requirements/constraints documented; capacity plan complete.
* Shard/placement strategy set; SLOs and alerts defined.
* Security (RBAC, encryption), DR (RPO/RTO) planned; runbooks ready.

---

## **Summary**

* A shard-per-Raft-group OLTP store with follower reads, leader writes, online schema changes, and robust observability meets the availability, latency, and durability goals while controlling cost and operational complexity.
