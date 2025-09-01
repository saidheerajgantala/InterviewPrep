# Floyd-Warshall Algorithm

## Name
- Category: Graph, Shortest Path, Dynamic Programming
- Summary: Algorithm for finding shortest paths between all pairs of vertices in a weighted graph with positive or negative edge weights (but no negative cycles).

## Preconditions / Assumptions
- Directed or undirected weighted graph
- May contain negative edge weights
- No negative cycles (or detection is required)
- All-pairs shortest paths are needed
- Graph is represented as an adjacency matrix

## Properties
- Deterministic
- Complete (finds all shortest paths or detects negative cycles)
- Dynamic programming approach
- Works with negative edge weights
- Simple implementation
- Suitable for dense graphs
- Can detect negative cycles

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(V³) | O(V²) | V is number of vertices |
| Path reconstruction | O(V³) | O(V³) | With predecessor matrix |
| Transitive closure | O(V³) | O(V²) | For reachability only |
| Johnson's algorithm | O(V² log V + VE) | O(V + E) | Better for sparse graphs |

## When to Use / When to Avoid
- Use if:
  - Need all-pairs shortest paths
  - Graph is dense (E ≈ V²)
  - Simple implementation is preferred
  - Graph may contain negative edge weights
  - Need to detect negative cycles
- Avoid if:
  - Graph is very sparse (Johnson's algorithm may be better)
  - Only single-source shortest paths are needed (use Dijkstra's or Bellman-Ford)
  - Graph is very large (V > 1000)
  - Memory constraints are severe

## Correctness (Proof Sketch)
- Dynamic programming approach with optimal substructure
- Recurrence relation: dp[k][i][j] = min(dp[k-1][i][j], dp[k-1][i][k] + dp[k-1][k][j])
  - Shortest path from i to j using vertices 0 to k as intermediates
- Base case: dp[0][i][j] = weight of edge (i,j) or ∞ if no direct edge
- Invariant: After considering vertex k, dp[k][i][j] is the shortest path from i to j using only vertices 0 to k as intermediates
- After considering all V vertices, we have the shortest paths between all pairs
- Negative cycle detection: If any diagonal element becomes negative

## Pseudocode (canonical)
```pseudo
function floydWarshall(graph):
    n = number of vertices
    
    # Initialize distance matrix
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

# With path reconstruction
function floydWarshallWithPath(graph):
    n = number of vertices
    
    # Initialize distance and next matrices
    dist = copy of adjacency matrix of graph
    next = new n×n matrix, initialized with -1
    
    # Initialize next matrix for direct edges
    for i = 0 to n-1:
        for j = 0 to n-1:
            if graph[i][j] != infinity:
                next[i][j] = j
    
    # Set diagonal elements to 0
    for i = 0 to n-1:
        dist[i][i] = 0
        next[i][i] = i
    
    # Main algorithm
    for k = 0 to n-1:
        for i = 0 to n-1:
            for j = 0 to n-1:
                if dist[i][k] != infinity and dist[k][j] != infinity and
                   dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]
                    next[i][j] = next[i][k]
    
    # Function to reconstruct path from i to j
    function getPath(i, j):
        if next[i][j] == -1:
            return []  # No path exists
        
        path = [i]
        while i != j:
            i = next[i][j]
            path.append(i)
        
        return path
    
    return dist, getPath
```

## Implementation Notes
- Use adjacency matrix representation for efficient implementation
- Infinity values should be handled carefully in relaxation step
- For path reconstruction, maintain a "next" matrix to track the next node in each path
- Can be optimized to use less memory by reusing the same matrix
- Order of loops (k, i, j) is important for correctness
- For large graphs, consider using sparse matrix representation
- Can be modified to compute transitive closure (reachability)
- Watch for overflow with large weights or many edges
- For sparse graphs, Johnson's algorithm may be more efficient

## Edge-case Checklist
- Empty graph: Return empty result
- Single vertex: Distance to self is 0
- Disconnected graph: Unreachable pairs have infinite distance
- Negative cycles: Should be detected
- Parallel edges: Use the minimum weight edge
- Self-loops: Usually set to 0 initially
- Large graphs: Watch for cubic time complexity
- Integer overflow: Use appropriate data types for large values

## Examples
- Minimal example:
  - Graph with 4 vertices and edges: 0→1 (3), 0→3 (5), 1→0 (2), 1→2 (4), 2→3 (1), 3→2 (2)
  - Initial distance matrix:
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
    [  2,  0,  4,  5]
    [ ∞, ∞,  0,  1]
    [ ∞, ∞,  2,  0]
    ```
  - After k=3:
    ```
    [  0,  3,  5,  5]
    [  2,  0,  4,  5]
    [  3,  6,  0,  1]
    [  4,  7,  2,  0]
    ```
  - Final result: All-pairs shortest paths matrix

## Variants / Alternatives
- Johnson's algorithm: More efficient for sparse graphs
- Blocked Floyd-Warshall: Better cache performance
- Parallel Floyd-Warshall: For multi-core systems
- Transitive closure: For reachability only (using boolean operations)
- Alternatives:
  - Running Dijkstra's V times: For sparse graphs
  - Bellman-Ford V times: For graphs with negative edges
  - Matrix multiplication approach: For theoretical connections
  - Johnson's algorithm: O(V² log V + VE) time for sparse graphs

## References
- Floyd, Robert W.: "Algorithm 97: Shortest Path" (1962)
- Warshall, Stephen: "A Theorem on Boolean Matrices" (1962)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 25.2
- Sedgewick, Wayne: "Algorithms, 4th Edition", Section 4.4
- Johnson, Donald B.: "Efficient algorithms for shortest paths in sparse networks" (1977)
