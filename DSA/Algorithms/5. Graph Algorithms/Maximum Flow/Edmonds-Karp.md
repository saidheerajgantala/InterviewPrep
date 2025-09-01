# Edmonds-Karp Algorithm

## Name
- Category: Graph Algorithms, Network Flow
- Summary: Implementation of the Ford-Fulkerson method for computing maximum flow in a flow network, using breadth-first search to find augmenting paths, guaranteeing a polynomial time complexity of O(VE²).

## Preconditions / Assumptions
- Directed graph with a single source and sink
- Each edge has a non-negative capacity
- Flow conservation at each vertex (except source and sink)
- No self-loops or parallel edges (can be handled with preprocessing)
- Graph is represented as an adjacency list or matrix with capacity information

## Properties
- Deterministic
- Complete (finds the maximum flow)
- Time complexity: O(VE²)
- Space complexity: O(V+E)
- Uses breadth-first search to find shortest augmenting paths
- Guarantees termination in polynomial time
- Improves upon the basic Ford-Fulkerson method
- Can be extended to solve minimum-cost flow and bipartite matching problems

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(VE²) | O(V+E) | V vertices, E edges |
| With scaling | O(E² log C) | O(V+E) | C = maximum capacity |
| With capacity scaling | O(E² log U) | O(V+E) | U = maximum capacity |
| With dynamic trees | O(VE log V) | O(V+E) | Advanced data structure |

## When to Use / When to Avoid
- Use if:
  - Need to find maximum flow in a network
  - Polynomial time complexity is required
  - Implementation simplicity is preferred over theoretical efficiency
  - Network has moderate size
  - Guaranteed termination is important
- Avoid if:
  - Network is very large (consider Dinic's or Push-Relabel)
  - Theoretical efficiency is critical (consider more advanced algorithms)
  - Network has very large capacities (consider scaling algorithms)
  - Memory is severely constrained
  - Specialized algorithms for the specific problem exist

## Correctness (Proof Sketch)
- The algorithm maintains a valid flow at each step
- BFS ensures that each augmenting path is the shortest available path
- Each augmentation increases the flow by at least 1 unit
- The number of augmentations is bounded by O(VE)
- Each BFS takes O(E) time
- Therefore, the total time complexity is O(VE²)
- The algorithm terminates when no more augmenting paths exist
- By the max-flow min-cut theorem, the flow is maximum when no augmenting paths exist
- BFS finds paths with the minimum number of edges, which leads to faster convergence

## Pseudocode (canonical)
```pseudo
function edmondsKarp(graph, source, sink):
    n = number of vertices
    flow = 0
    
    # Create residual graph
    residual = copy of graph
    
    while true:
        # Find an augmenting path using BFS
        parent = new array of size n, initialized with -1
        parent[source] = -2  # Special value to mark the source
        
        # BFS queue
        queue = new Queue()
        queue.enqueue(source)
        
        while not queue.isEmpty() and parent[sink] == -1:
            u = queue.dequeue()
            
            for each neighbor v of u in residual:
                if parent[v] == -1 and residual[u][v] > 0:
                    parent[v] = u
                    queue.enqueue(v)
        
        # If sink is not reachable, we're done
        if parent[sink] == -1:
            break
        
        # Find the bottleneck capacity
        path_flow = infinity
        v = sink
        while v != source:
            u = parent[v]
            path_flow = min(path_flow, residual[u][v])
            v = u
        
        # Update residual capacities
        v = sink
        while v != source:
            u = parent[v]
            residual[u][v] -= path_flow
            residual[v][u] += path_flow  # Add reverse edge for residual graph
            v = u
        
        flow += path_flow
    
    return flow

# Implementation with adjacency list
function edmondsKarpAdjList(graph, source, sink):
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
        # Find augmenting path using BFS
        parent = new array of size n, initialized with -1
        queue = new Queue()
        queue.enqueue(source)
        parent[source] = -2
        
        while not queue.isEmpty() and parent[sink] == -1:
            u = queue.dequeue()
            
            for each neighbor v in residual[u]:
                if parent[v] == -1 and capacity[u][v] > 0:
                    parent[v] = u
                    queue.enqueue(v)
        
        # If sink is not reachable, we're done
        if parent[sink] == -1:
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
- Be careful with integer overflow for large capacities
- Consider using a struct or class for edges with capacity and flow information
- Optimize BFS implementation to avoid redundant computations
- For large networks, consider using more efficient data structures
- Handle parallel edges by merging them (adding capacities)
- Handle self-loops by removing them (they don't contribute to source-sink flow)
- Consider using capacity scaling for better performance with large capacities
- Implement proper error handling for invalid inputs

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
    - Iteration 3: Path s→A→B→t, flow = 0 (A→B is saturated)
    - Total flow: 5
  
- Complex example:
  - Network with multiple paths and bottlenecks
  - BFS finds shortest augmenting paths first
  - Algorithm terminates when no more augmenting paths exist
  - Maximum flow equals minimum cut capacity

## Variants / Alternatives
- Capacity scaling: Start with large augmentations
- Dynamic trees: For faster augmenting path finding
- Alternatives:
  - Ford-Fulkerson: More general method, potentially non-polynomial
  - Dinic's algorithm: Uses level graphs for better performance
  - Push-relabel algorithm: Different approach with better worst-case
  - MPM algorithm: Similar to Dinic's with different implementation

## References
- Edmonds, Jack and Karp, Richard M.: "Theoretical Improvements in Algorithmic Efficiency for Network Flow Problems" (1972)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Ahuja, Magnanti, Orlin: "Network Flows: Theory, Algorithms, and Applications"
- Kleinberg, Jon and Tardos, Eva: "Algorithm Design"
- Competitive Programming resources (e.g., CP-Algorithms)
