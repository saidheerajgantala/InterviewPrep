# Dijkstra's Algorithm

## Name
- Category: Graph
- Summary: Greedy algorithm that finds the shortest path from a source vertex to all other vertices in a weighted graph.

## Preconditions / Assumptions
- Connected graph (or will only find paths to reachable vertices)
- Non-negative edge weights (critical requirement)
- Can be directed or undirected
- Edge weights represent distance/cost to be minimized

## Properties
- Deterministic
- Complete (finds optimal solution for all reachable vertices)
- Greedy (always selects vertex with minimum distance)
- Single-source, all-destinations shortest path

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Array implementation | O(V²) | O(V²) | O(V²) | O(V) | Good for dense graphs |
| Binary heap | O(E + V log V) | O(E + V log V) | O(E log V) | O(V) | Good for sparse graphs |
| Fibonacci heap | O(E + V log V) | O(E + V log V) | O(E + V log V) | O(V) | Theoretically optimal |

## When to Use / When to Avoid
- Use if:
  - Need shortest paths from single source
  - All edge weights are non-negative
  - Graph is sparse (E ≈ V) with binary heap implementation
- Avoid if:
  - Graph has negative edge weights (use Bellman-Ford instead)
  - Need all-pairs shortest paths for dense graph (use Floyd-Warshall)
  - Only need to know if path exists (use BFS)

## Correctness (Proof Sketch)
- Invariant: For each vertex v in the settled set, dist[v] is the shortest distance from source
- Greedy choice: Always select vertex with minimum tentative distance
- Proof by contradiction: If a shorter path to a settled vertex existed, it would have been found
- Suboptimal paths cannot be selected due to non-negative edge weights
- Termination: Each vertex is settled exactly once

## Pseudocode (canonical)
```pseudo
function dijkstra(graph, source):
    # Initialize distances
    dist[1...V] = infinity
    dist[source] = 0
    pq = priority_queue()  # Min-priority queue
    pq.insert(source, 0)
    
    # Track settled vertices
    settled = set()
    
    while pq is not empty:
        u = pq.extract_min()  # Vertex with minimum distance
        
        if u in settled:
            continue
        
        settled.add(u)
        
        for each neighbor v of u:
            if v not in settled:
                # Relaxation step
                alt = dist[u] + weight(u, v)
                if alt < dist[v]:
                    dist[v] = alt
                    pq.insert(v, alt)
                    # Optional: prev[v] = u (to reconstruct path)
    
    return dist
```

## Implementation Notes
- Use a min-priority queue for efficient extraction of minimum distance vertex
- Binary heap implementation is practical and efficient for most cases
- Keep track of settled vertices to avoid re-processing
- For path reconstruction, maintain a prev[] array to track predecessors
- Can terminate early if only seeking path to specific target
- Consider using decrease-key operation in priority queue if available

## Edge-case Checklist
- Empty graph: Return empty result
- Single vertex: Distance to self is 0
- Unreachable vertices: Distance remains infinity
- Disconnected graph: Only computes paths to reachable vertices
- Multiple shortest paths: Algorithm finds one of them
- Cycles: Handled correctly due to settled vertex tracking

## Examples
- Minimal example:
  - Graph: A--(1)--B--(2)--C, A--(6)--C
  - Source: A
  - Output: dist[A]=0, dist[B]=1, dist[C]=3
  
- Tricky counterexample:
  - With negative edges: A--(2)--B, B--(-1)--C, A--(2)--C
  - Dijkstra would incorrectly compute dist[C]=2 instead of dist[C]=1
  - Shows why non-negative weights are required

## Variants / Alternatives
- Bidirectional Dijkstra: Search from both source and target
- A* Search: Dijkstra with heuristic function (informed search)
- Delta-stepping: Parallel implementation of Dijkstra
- Dial's algorithm: Optimization when edge weights are small integers
- Alternatives for specific cases:
  - Bellman-Ford: Handles negative weights
  - Floyd-Warshall: All-pairs shortest paths
  - BFS: Unweighted shortest paths

## References
- Cormen, Leiserson, Rivest, Stein: Introduction to Algorithms, Chapter 24.3
- Dijkstra, E.W. "A Note on Two Problems in Connexion with Graphs" (1959)
- Sedgewick, Wayne: Algorithms, 4th Edition 