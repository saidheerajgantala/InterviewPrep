# DP on Graphs (Bellman-Ford, Floyd-Warshall)

## Name
- Category: Dynamic Programming, Graph Algorithms
- Summary: Algorithms that use dynamic programming principles to solve shortest path problems on weighted graphs, with Bellman-Ford handling single-source shortest paths with negative edges and Floyd-Warshall finding all-pairs shortest paths.

## Preconditions / Assumptions
### Bellman-Ford
- Directed weighted graph (can be adapted for undirected)
- May contain negative edge weights
- No negative cycles (or detection is required)
- Single source vertex

### Floyd-Warshall
- Directed weighted graph (can be adapted for undirected)
- May contain negative edge weights
- No negative cycles (or detection is required)
- All-pairs shortest paths required

## Properties
### Bellman-Ford
- Deterministic
- Complete (finds shortest paths or detects negative cycles)
- Handles negative edge weights
- Detects negative cycles
- Less efficient than Dijkstra's for non-negative weights

### Floyd-Warshall
- Deterministic
- Complete (finds all shortest paths or detects negative cycles)
- Handles negative edge weights
- Simple implementation using DP
- Works well for dense graphs

## Complexity (per variant)
| Algorithm | Variant | Time | Space | Notes |
|---|---|---|---|---|
| Bellman-Ford | Standard | O(VE) | O(V) | V vertices, E edges |
| Bellman-Ford | SPFA | O(VE) | O(V) | Average case often better |
| Bellman-Ford | Early termination | O(VE) | O(V) | Can stop if no relaxation |
| Floyd-Warshall | Standard | O(V³) | O(V²) | Simple cubic algorithm |
| Floyd-Warshall | Path reconstruction | O(V³) | O(V³) | With predecessor matrix |

## When to Use / When to Avoid
### Bellman-Ford
- Use if:
  - Single-source shortest paths needed
  - Graph may contain negative edges
  - Need to detect negative cycles
  - Graph is not too large
- Avoid if:
  - All edge weights are non-negative (use Dijkstra's)
  - Graph is very large (V and E are large)
  - Only specific paths are needed

### Floyd-Warshall
- Use if:
  - All-pairs shortest paths needed
  - Graph is dense (E ≈ V²)
  - Graph is small to medium size
  - Simple implementation preferred
- Avoid if:
  - Graph is very large (V > 1000)
  - Graph is sparse (Johnson's algorithm may be better)
  - Only single-source paths are needed

## Correctness (Proof Sketch)
### Bellman-Ford
- Principle: After i iterations, finds shortest paths using at most i edges
- Invariant: After i iterations, dist[v] is the shortest path from source to v using at most i edges
- If no negative cycles, shortest path contains at most V-1 edges
- Thus, V-1 iterations are sufficient
- Extra iteration detects negative cycles (if any distance decreases)

### Floyd-Warshall
- Principle: Dynamic programming with intermediate vertices
- Recurrence: dp[k][i][j] = min(dp[k-1][i][j], dp[k-1][i][k] + dp[k-1][k][j])
  - Shortest path from i to j using vertices 0 to k as intermediates
- Base case: dp[0][i][j] = weight of edge (i,j) or ∞ if no direct edge
- After considering all V vertices as intermediates, we have shortest paths
- Negative cycle detection: If any diagonal element becomes negative

## Pseudocode (canonical)
```pseudo
# Bellman-Ford Algorithm
function bellmanFord(graph, source):
    # Initialize distances
    dist = new array of size |V|, filled with infinity
    dist[source] = 0
    
    # Relax all edges V-1 times
    for i = 1 to |V| - 1:
        for each edge (u, v) with weight w in graph:
            if dist[u] != infinity and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
    
    # Check for negative cycles
    for each edge (u, v) with weight w in graph:
        if dist[u] != infinity and dist[u] + w < dist[v]:
            # Negative cycle exists
            return "Negative cycle detected"
    
    return dist

# Floyd-Warshall Algorithm
function floydWarshall(graph):
    # Initialize distance matrix
    n = |V|
    dist = copy of adjacency matrix of graph
    
    # Set diagonal elements to 0
    for i = 0 to n-1:
        dist[i][i] = 0
    
    # Consider each vertex as an intermediate
    for k = 0 to n-1:
        for i = 0 to n-1:
            for j = 0 to n-1:
                if dist[i][k] != infinity and dist[k][j] != infinity:
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    
    # Check for negative cycles (optional)
    for i = 0 to n-1:
        if dist[i][i] < 0:
            return "Negative cycle detected"
    
    return dist

# Floyd-Warshall with path reconstruction
function floydWarshallWithPath(graph):
    n = |V|
    dist = copy of adjacency matrix of graph
    next = new n×n matrix, initialized with -1
    
    # Initialize next matrix
    for i = 0 to n-1:
        for j = 0 to n-1:
            if graph[i][j] != infinity:
                next[i][j] = j
    
    # Set diagonal elements to 0
    for i = 0 to n-1:
        dist[i][i] = 0
    
    # Main algorithm
    for k = 0 to n-1:
        for i = 0 to n-1:
            for j = 0 to n-1:
                if dist[i][k] != infinity and dist[k][j] != infinity and
                   dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]
                    next[i][j] = next[i][k]
    
    # Path reconstruction function
    function getPath(i, j):
        if next[i][j] == -1:
            return []  # No path exists
        
        path = [i]
        while i != j:
            i = next[i][j]
            path.add(i)
        
        return path
    
    return dist, getPath
```

## Implementation Notes
### Bellman-Ford
- Early termination: Stop if no relaxation in an iteration
- Queue-based optimization (SPFA): Only process vertices whose distance changed
- Can be modified to reconstruct paths using a predecessor array
- Handle overflow for large weights or many edges
- Initialize distances carefully (source to 0, others to infinity)

### Floyd-Warshall
- Can be optimized to use O(V²) space by reusing the same matrix
- For path reconstruction, store the next vertex in the path
- Handle infinity values carefully in relaxation step
- Can detect negative cycles by checking diagonal elements
- Consider using sparse matrix representation for large, sparse graphs
- Order of loops (k, i, j) is important for correctness

## Edge-case Checklist
### Bellman-Ford
- Empty graph: Return empty result
- Single vertex: Distance to self is 0
- Disconnected graph: Unreachable vertices have infinite distance
- Negative cycles: Should be detected
- Parallel edges: Use the minimum weight edge
- Self-loops: Usually ignored unless negative

### Floyd-Warshall
- Empty graph: Return empty matrix
- Single vertex: Distance to self is 0
- Disconnected graph: Unreachable pairs have infinite distance
- Negative cycles: Should be detected
- Parallel edges: Use the minimum weight edge
- Self-loops: Usually set to 0 initially

## Examples
### Bellman-Ford
- Minimal example:
  - Graph: 0→1 (weight 5), 0→2 (weight 2), 1→3 (weight 1), 2→1 (weight -3), 2→3 (weight 6)
  - Source: 0
  - Iterations:
    - Init: [0, ∞, ∞, ∞]
    - After edges in iteration 1: [0, 5, 2, ∞]
    - After edges in iteration 2: [0, -1, 2, 0]
    - After edges in iteration 3: [0, -1, 2, 0]
  - No change in iteration 3, so no negative cycle
  - Result: [0, -1, 2, 0]

### Floyd-Warshall
- Minimal example:
  - Graph with 4 vertices and edges: 0→1 (3), 0→3 (5), 1→0 (2), 1→2 (4), 2→3 (1), 3→2 (2)
  - Initial dist matrix:
    ```
    [  0,  3, ∞,  5]
    [  2,  0,  4, ∞]
    [ ∞, ∞,  0,  1]
    [ ∞, ∞,  2,  0]
    ```
  - After k=0: Same as initial
  - After k=1:
    ```
    [  0,  3,  7,  5]
    [  2,  0,  4,  7]
    [ ∞, ∞,  0,  1]
    [ ∞, ∞,  2,  0]
    ```
  - After k=2:
    ```
    [  0,  3,  7,  5]
    [  2,  0,  4,  7]
    [ ∞, ∞,  0,  1]
    [ ∞, ∞,  2,  0]
    ```
  - After k=3:
    ```
    [  0,  3,  6,  5]
    [  2,  0,  3,  4]
    [ ∞, ∞,  0,  1]
    [ ∞, ∞,  2,  0]
    ```
  - Final result:
    ```
    [  0,  3,  5,  5]
    [  2,  0,  3,  4]
    [  3,  6,  0,  1]
    [  4,  7,  2,  0]
    ```

## Variants / Alternatives
### Bellman-Ford
- SPFA (Shortest Path Faster Algorithm): Queue-based optimization
- Yen's algorithm: Finds k-shortest paths
- Distributed Bellman-Ford: For distributed systems
- Alternatives:
  - Dijkstra's algorithm: More efficient for non-negative weights
  - Johnson's algorithm: For sparse graphs with negative weights

### Floyd-Warshall
- Johnson's algorithm: More efficient for sparse graphs
- Blocked Floyd-Warshall: Better cache performance
- Parallel Floyd-Warshall: For multi-core systems
- Alternatives:
  - Running Dijkstra's V times: For sparse graphs
  - Matrix multiplication approach: For theoretical connections

## References
- Bellman, Richard: "On a Routing Problem" (1958)
- Ford, L.R.: "Network Flow Theory" (1956)
- Floyd, Robert W.: "Algorithm 97: Shortest Path" (1962)
- Warshall, Stephen: "A Theorem on Boolean Matrices" (1962)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapters 24.1, 25.2
- Sedgewick, Wayne: "Algorithms, 4th Edition", Sections 4.4
