# Event Sourcing (See Canonical)

This entry defers to the canonical architectural guide:

- `Patterns/004_ArchitecturalPatterns/Event_Sourcing.md`

Notes for messaging context:
- Often implemented atop a durable log (e.g., Kafka) with append-only streams per aggregate.
- Projections consume events to build read models; ensure idempotency and replay safety.
