# Ford-Fulkerson Algorithm

## Name
- Category: Graph Algorithms, Network Flow
- Summary: Method for computing maximum flow in a flow network by repeatedly finding augmenting paths from source to sink and increasing the flow along these paths until no more augmenting paths exist.

## Preconditions / Assumptions
- Directed graph with a single source and sink
- Each edge has a non-negative integer capacity
- Flow conservation at each vertex (except source and sink)
- No self-loops or parallel edges (can be handled with preprocessing)
- Graph is represented as an adjacency list or matrix with capacity information

## Properties
- Deterministic (if augmenting paths are chosen deterministically)
- Complete (finds the maximum flow)
- Time complexity: O(Ef) where f is the maximum flow value
- Space complexity: O(V+E)
- Uses depth-first search (typically) to find augmenting paths
- Terminates for networks with integer capacities
- Forms the basis for more efficient algorithms like Edmonds-Karp
- Can be extended to solve minimum-cost flow and bipartite matching problems

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Basic | O(Ef) | O(V+E) | f = maximum flow, potentially exponential |
| Edmonds-Karp | O(VE²) | O(V+E) | Uses BFS for shortest augmenting paths |
| Capacity scaling | O(E² log U) | O(V+E) | U = maximum capacity |
| With dynamic trees | O(VE log V) | O(V+E) | Advanced data structure |

## When to Use / When to Avoid
- Use if:
  - Need to find maximum flow in a network
  - Network has small integer capacities
  - Implementation simplicity is preferred
  - Network is small to moderate in size
  - Educational or illustrative purposes
- Avoid if:
  - Network has large capacities (use Edmonds-Karp or other variants)
  - Network is very large (use more efficient algorithms)
  - Real-valued capacities are present (may not terminate)
  - Performance is critical (use Dinic's or Push-Relabel)
  - Guaranteed polynomial time is required (use Edmonds-Karp)

## Correctness (Proof Sketch)
- The algorithm maintains a valid flow at each step
- Each augmentation increases the flow by at least 1 unit (for integer capacities)
- The residual graph represents remaining capacity and potential for flow reversal
- When no augmenting paths exist, the flow is maximum by the max-flow min-cut theorem
- For integer capacities, the algorithm terminates after at most f augmentations
- The final flow value equals the capacity of the minimum cut
- The algorithm correctly handles flow conservation at each vertex

## Pseudocode (canonical)
```pseudo
function fordFulkerson(graph, source, sink):
    n = number of vertices
    flow = 0
    
    # Create residual graph
    residual = copy of graph
    
    # While there is an augmenting path
    while true:
        # Find an augmenting path using DFS
        path = findAugmentingPath(residual, source, sink)
        
        # If no augmenting path exists, we're done
        if path is empty:
            break
        
        # Find the bottleneck capacity
        bottleneck = infinity
        for i = 0 to path.length - 2:
            u = path[i]
            v = path[i+1]
            bottleneck = min(bottleneck, residual[u][v])
        
        # Update residual capacities
        for i = 0 to path.length - 2:
            u = path[i]
            v = path[i+1]
            residual[u][v] -= bottleneck
            residual[v][u] += bottleneck  # Add reverse edge for residual graph
        
        # Increase flow
        flow += bottleneck
    
    return flow

function findAugmentingPath(residual, source, sink):
    visited = new array of size n, initialized with false
    path = []
    
    function dfs(u, current_path):
        visited[u] = true
        current_path.push(u)
        
        # If we reached the sink, we found a path
        if u == sink:
            return true
        
        # Try all neighboring vertices
        for each vertex v adjacent to u:
            if not visited[v] and residual[u][v] > 0:
                if dfs(v, current_path):
                    return true
        
        # If no path to sink from this vertex
        current_path.pop()
        return false
    
    # Start DFS from source
    if dfs(source, path):
        return path
    else:
        return []  # No augmenting path found

# Implementation with adjacency list
function fordFulkersonAdjList(graph, source, sink):
    n = number of vertices
    flow = 0
    
    # Create residual graph using adjacency list
    residual = new array of n empty lists
    capacity = new map to store capacities
    
    # Initialize residual graph
    for each edge (u, v, cap) in graph:
        residual[u].add(v)
        residual[v].add(u)  # Add reverse edge
        capacity[u][v] = cap
        capacity[v][u] = 0  # Reverse edge has zero capacity initially
    
    while true:
        # Find augmenting path
        parent = new array of size n, initialized with -1
        visited = new array of size n, initialized with false
        
        function dfs(u):
            visited[u] = true
            
            if u == sink:
                return true
            
            for each neighbor v in residual[u]:
                if not visited[v] and capacity[u][v] > 0:
                    parent[v] = u
                    if dfs(v):
                        return true
            
            return false
        
        # If no augmenting path exists, we're done
        if not dfs(source):
            break
        
        # Find bottleneck capacity
        path_flow = infinity
        v = sink
        while v != source:
            u = parent[v]
            path_flow = min(path_flow, capacity[u][v])
            v = u
        
        # Update residual capacities
        v = sink
        while v != source:
            u = parent[v]
            capacity[u][v] -= path_flow
            capacity[v][u] += path_flow
            v = u
        
        flow += path_flow
    
    return flow
```

## Implementation Notes
- Use adjacency list representation for efficiency
- Implement residual graph using forward and reverse edges
- Be careful with the choice of augmenting path selection strategy
- Consider using BFS instead of DFS for better performance (Edmonds-Karp)
- Handle parallel edges by merging them (adding capacities)
- Handle self-loops by removing them (they don't contribute to source-sink flow)
- Be careful with integer overflow for large capacities
- Consider using capacity scaling for better performance with large capacities
- Implement proper error handling for invalid inputs
- For educational purposes, visualize the residual graph and augmenting paths

## Edge-case Checklist
- Empty graph: Return 0 flow
- Single vertex (source = sink): Return 0 flow
- No path from source to sink: Return 0 flow
- Disconnected graph: Algorithm works correctly
- Parallel edges: Merge them by adding capacities
- Self-loops: Remove them as they don't affect the maximum flow
- Large capacities: Watch for integer overflow
- Zero-capacity edges: Can be removed or ignored
- Single edge from source to sink: Flow is the capacity of that edge

## Examples
- Minimal example:
  - Graph: s→A (capacity 3), s→B (capacity 2), A→B (capacity 2), A→t (capacity 3), B→t (capacity 2)
  - Execution:
    - Iteration 1: Path s→A→t, flow = 3
    - Iteration 2: Path s→B→t, flow = 2
    - Iteration 3: No more augmenting paths
    - Total flow: 5
  
- Complex example:
  - Network with multiple paths and bottlenecks
  - Different path selection strategies may lead to different number of iterations
  - Algorithm terminates when no more augmenting paths exist
  - Maximum flow equals minimum cut capacity

## Variants / Alternatives
- Edmonds-Karp: Uses BFS for shortest augmenting paths, O(VE²)
- Capacity scaling: Start with large augmentations, O(E² log U)
- Dinic's algorithm: Uses level graphs, O(V²E)
- Alternatives:
  - Push-relabel algorithm: Different approach, O(V²E) or O(V³)
  - MPM algorithm: Similar to Dinic's, O(V³)
  - Preflow-push algorithms: More efficient for dense graphs

## References
- Ford, L.R. and Fulkerson, D.R.: "Maximal Flow Through a Network" (1956)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Ahuja, Magnanti, Orlin: "Network Flows: Theory, Algorithms, and Applications"
- Kleinberg, Jon and Tardos, Eva: "Algorithm Design"
- Competitive Programming resources (e.g., CP-Algorithms)
