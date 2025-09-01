# Connection Pooling

## 0) Metadata
- **Name**: Connection Pooling
- **Canonical Path**: Patterns/011_Performance/Optimization/Connection_Pooling.md
- **Category**: 011 Performance / Optimization
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: pooling, db, http, keepalive

---

## 1) TL;DR (Executive Summary)
- Reuse connections to reduce handshake overhead and control concurrency to backends.

---

## 2) Concepts
- Pool size, queue length, timeouts, keep-alives; per-host/per-user pools.
- Slow-start; circuit breaking on backend failure.

## 3) Implementation Guide
- Size based on backend capacity and app concurrency; avoid oversubscription.
- Monitor pool wait, saturation, timeouts; tune keep-alive.

---

## 4) Pitfalls
- Global pool causing head-of-line blocking; separate by dependency.
- Leaks; ensure return-to-pool on all code paths.

---

## 5) References
- Driver/library docs; SRE books; vendor best practices.
