# Consistent Hashing

## 0) Metadata
- **Name**: Consistent Hashing
- **Canonical Path**: Patterns/008_LoadBalancingPatterns/Consistent_Hashing.md
- **Category**: 008 Load Balancing Patterns
- **Status**: Stable
- **Last Updated**: YYYY-MM-DD
- **Tags**: hashing, ring, virtual-nodes, affinity, sharding

---

## 1) TL;DR (Executive Summary)
- **Problem**: Adding/removing nodes causes massive remapping under naive hashing.
- **Solution (essence)**: Hash keys and nodes onto a ring; each node owns a segment. Use many virtual nodes to balance.

---

## 2) Architecture
```mermaid
flowchart TB
  RING((Hash Ring))
  K1[Key Hash] --> RING
  N1[Node A (vn)] --> RING
  N2[Node B (vn)] --> RING
  N3[Node C (vn)] --> RING
```

---

## 3) Properties & Tradeoffs
| Aspect | Pros | Cons | Notes |
|---|---|---|---|
| Stability | Minimal remap | Some imbalance | Use many virtual nodes |
| Affinity | Cache/shard locality | Hotspots possible | Spread keys |

---

## 4) Implementation Guide
- Choose strong hash (e.g., xxHash) and many virtual nodes per physical node.
- Use rendezvous hashing as an alternative with similar properties.

---

## 5) Pitfalls & Edge Cases
- Non-uniform keys create hotspots; consider salt/prefix spreading.
- Coordinate with client caches to avoid split-brain mappings.

---

## 6) References
- Consistent hashing papers; Dynamo; Rendezvous hashing.
