# Bellman-Ford Algorithm

## Name
- Category: Graph, Shortest Path, Dynamic Programming
- Summary: Algorithm for finding the shortest paths from a single source vertex to all other vertices in a weighted graph, capable of handling negative edge weights and detecting negative cycles.

## Preconditions / Assumptions
- Directed or undirected weighted graph
- May contain negative edge weights
- No negative cycles reachable from source (or detection is required)
- Single source vertex
- Graph is represented as an edge list or adjacency list

## Properties
- Deterministic
- Complete (finds all shortest paths from source or detects negative cycles)
- Dynamic programming approach
- Works with negative edge weights
- Detects negative cycles
- Less efficient than Dijkstra's for non-negative weights
- Simpler than Johnson's algorithm

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(VE) | O(V) | V vertices, E edges |
| SPFA | O(VE) | O(V) | Average case often better |
| Early termination | O(VE) | O(V) | Can stop if no relaxation |
| Yen's improvement | O(VE) | O(V) | With two arrays |
| Parallel | O(VE/P) | O(V) | P processors |

## When to Use / When to Avoid
- Use if:
  - Single-source shortest paths needed
  - Graph may contain negative edges
  - Need to detect negative cycles
  - Graph is not too large
  - Simplicity is preferred over optimization
- Avoid if:
  - All edge weights are non-negative (use Dijkstra's)
  - Graph is very large (V and E are large)
  - Only specific paths are needed
  - All-pairs shortest paths are needed (use Floyd-Warshall or Johnson's)

## Correctness (Proof Sketch)
- Principle: After i iterations, finds shortest paths using at most i edges
- Invariant: After i iterations, dist[v] is the shortest path from source to v using at most i edges
- If no negative cycles, shortest path contains at most V-1 edges
- Thus, V-1 iterations are sufficient
- Extra iteration detects negative cycles (if any distance decreases)
- Proof by induction on the number of edges in the shortest path

## Pseudocode (canonical)
```pseudo
function bellmanFord(graph, source):
    # Initialize distances
    dist = new array of size |V|, filled with infinity
    dist[source] = 0
    
    # Initialize predecessor array for path reconstruction
    pred = new array of size |V|, filled with null
    
    # Relax all edges V-1 times
    for i = 1 to |V| - 1:
        for each edge (u, v) with weight w in graph:
            if dist[u] != infinity and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                pred[v] = u
    
    # Check for negative cycles
    for each edge (u, v) with weight w in graph:
        if dist[u] != infinity and dist[u] + w < dist[v]:
            # Negative cycle exists
            return "Negative cycle detected"
    
    return dist, pred

# Path reconstruction
function reconstructPath(pred, target):
    if target == null:
        return []
    
    path = [target]
    while pred[target] != null:
        target = pred[target]
        path.prepend(target)
    
    return path

# SPFA (Shortest Path Faster Algorithm) variant
function spfa(graph, source):
    # Initialize distances
    dist = new array of size |V|, filled with infinity
    dist[source] = 0
    
    # Initialize predecessor array
    pred = new array of size |V|, filled with null
    
    # Initialize queue and in-queue status
    queue = new Queue()
    queue.enqueue(source)
    inQueue = new boolean array of size |V|, filled with false
    inQueue[source] = true
    
    # Count number of times a vertex is processed
    count = new array of size |V|, filled with 0
    
    while not queue.isEmpty():
        u = queue.dequeue()
        inQueue[u] = false
        
        for each neighbor v of u with weight w:
            if dist[u] != infinity and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                pred[v] = u
                
                if not inQueue[v]:
                    queue.enqueue(v)
                    inQueue[v] = true
                    count[v]++
                    
                    # Check for negative cycle
                    if count[v] >= |V|:
                        return "Negative cycle detected"
    
    return dist, pred
```

## Implementation Notes
- Edge list representation is simplest for implementation
- For path reconstruction, maintain a predecessor array
- Early termination: Stop if no relaxation in an iteration
- Queue-based optimization (SPFA): Only process vertices whose distance changed
- Be careful with overflow for large weights or many edges
- Initialize distances carefully (source to 0, others to infinity)
- For detecting the actual negative cycle, additional bookkeeping is needed
- Can be extended to find shortest paths with exactly k edges
- Consider using SPFA for better average-case performance

## Edge-case Checklist
- Empty graph: Return empty result
- Single vertex: Distance to self is 0
- Disconnected graph: Unreachable vertices have infinite distance
- Negative cycles: Should be detected
- Parallel edges: Use the minimum weight edge
- Self-loops: Usually ignored unless negative
- Large graphs: Watch for quadratic time complexity
- Integer overflow: Use appropriate data types for large values

## Examples
- Minimal example:
  - Graph: 0→1 (weight 5), 0→2 (weight 2), 1→3 (weight 1), 2→1 (weight -3), 2→3 (weight 6)
  - Source: 0
  - Iterations:
    - Init: [0, ∞, ∞, ∞]
    - After iteration 1: [0, 5, 2, ∞]
    - After iteration 2: [0, -1, 2, 6]
    - After iteration 3: [0, -1, 2, 0]
    - No change in iteration 4, so no negative cycle
  - Result: [0, -1, 2, 0]
  - Shortest paths:
    - 0 to 0: Direct (cost 0)
    - 0 to 1: 0→2→1 (cost -1)
    - 0 to 2: 0→2 (cost 2)
    - 0 to 3: 0→2→1→3 (cost 0)

- Negative cycle example:
  - Graph: 0→1 (weight 1), 1→2 (weight 2), 2→3 (weight 3), 3→1 (weight -7)
  - Cycle: 1→2→3→1 with total weight -2
  - Algorithm detects negative cycle in the check phase

## Variants / Alternatives
- SPFA (Shortest Path Faster Algorithm): Queue-based optimization
- Yen's improvement: Uses two arrays to reduce constant factor
- Early termination: Stop if no relaxation in an iteration
- Parallel Bellman-Ford: Distributes edge relaxations
- Alternatives:
  - Dijkstra's algorithm: More efficient for non-negative weights
  - Johnson's algorithm: For all-pairs shortest paths with negative weights
  - Floyd-Warshall: For all-pairs shortest paths (dense graphs)
  - Delta-stepping: Compromise between Dijkstra's and Bellman-Ford

## References
- Bellman, Richard: "On a Routing Problem" (1958)
- Ford, L.R.: "Network Flow Theory" (1956)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 24.1
- Sedgewick, Wayne: "Algorithms, 4th Edition", Section 4.4
- Fanding Duan: "SPFA: Shortest Path Faster Algorithm"
