# Asynchronous Processing

## 0) Metadata
- **Name**: Asynchronous Processing
- **Canonical Path**: Patterns/011_Performance/Optimization/Asynchronous_Processing.md
- **Category**: 011 Performance / Optimization
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: async, queues, background, latency

---

## 1) TL;DR (Executive Summary)
- Move non-user-critical work off the request path to reduce tail latency and increase throughput.

---

## 2) Approaches
- Queues for background jobs; batch processing; scheduled tasks; outbox.
- Immediate response with job IDs; callbacks/webhooks when done.

## 3) Implementation Guide
- Identify deferrable work; ensure idempotency; retries with backoff.
- Backpressure and rate limits for workers; DLQs for poison jobs.

---

## 4) Tradeoffs
- Lower latency on critical path vs delayed eventual completion; adds complexity.

---

## 5) References
- SRE; job queue systems; async design patterns.
