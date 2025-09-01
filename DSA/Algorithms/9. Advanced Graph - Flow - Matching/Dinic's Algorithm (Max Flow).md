# Dinic's Algorithm (Max Flow)

## Name
- Category: Graph Algorithms, Network Flow
- Summary: Efficient algorithm for computing the maximum flow in a flow network using a combination of level graphs (for finding blocking flows) and multiple shortest augmenting paths, achieving polynomial time complexity.

## Preconditions / Assumptions
- Directed graph with a single source and sink
- Each edge has a non-negative capacity
- Flow conservation at each vertex (except source and sink)
- No self-loops or parallel edges (can be handled with preprocessing)
- Graph is represented as an adjacency list with capacity information

## Properties
- Deterministic
- Complete (finds the maximum flow)
- Polynomial time complexity
- Uses level graphs to find shortest augmenting paths
- Combines BFS (for level graph construction) and DFS (for finding blocking flows)
- More efficient than Ford-Fulkerson and Edmonds-Karp algorithms
- Can be extended to solve minimum-cost flow and bipartite matching problems

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(V²E) | O(V+E) | V vertices, E edges |
| Unit capacity | O(min(V²/³, E¹/²) · E) | O(V+E) | Better bound for unit capacities |
| Unit network | O(V½E) | O(V+E) | For unit networks |
| With scaling | O(E log U) | O(V+E) | U = maximum capacity |

## When to Use / When to Avoid
- Use if:
  - Need to find maximum flow in a network
  - Network is large and dense
  - Efficiency is important
  - Ford-Fulkerson or Edmonds-Karp is too slow
  - Implementation complexity is acceptable
- Avoid if:
  - Simplicity is more important than efficiency
  - Network is very small (simpler algorithms may suffice)
  - Need to find minimum-cost flow (use specialized algorithms)
  - Memory is severely constrained
  - Graph structure changes frequently

## Correctness (Proof Sketch)
- The algorithm builds a level graph using BFS, which contains only edges that lead from the source to the sink along shortest paths
- In each phase, it finds a blocking flow in the level graph using DFS
- A blocking flow is a flow that saturates at least one edge on every path from source to sink in the level graph
- After at most O(V) phases, no more augmenting paths exist
- Each phase increases the length of the shortest augmenting path
- The maximum number of phases is O(V) because the shortest path length cannot exceed V
- In each phase, the algorithm finds a blocking flow in O(VE) time
- Therefore, the overall time complexity is O(V²E)
- Correctness follows from the max-flow min-cut theorem

## Pseudocode (canonical)
```pseudo
function dinic(graph, source, sink):
    n = number of vertices
    flow = 0
    
    # Create residual graph
    residual = copy of graph
    
    while true:
        # Build level graph using BFS
        level = new array of size n, initialized with -1
        level[source] = 0
        
        queue = new Queue()
        queue.enqueue(source)
        
        while not queue.isEmpty():
            u = queue.dequeue()
            
            for each edge (u, v) in residual:
                if level[v] == -1 and residual[u][v] > 0:
                    level[v] = level[u] + 1
                    queue.enqueue(v)
        
        # If sink is not reachable, we're done
        if level[sink] == -1:
            break
        
        # Find blocking flow using DFS
        next = new array of size n, initialized with 0
        
        function dfs(u, flow_so_far):
            if u == sink:
                return flow_so_far
            
            while next[u] < residual[u].size:
                v = residual[u][next[u]].vertex
                
                if level[v] == level[u] + 1 and residual[u][v] > 0:
                    curr_flow = min(flow_so_far, residual[u][v])
                    temp_flow = dfs(v, curr_flow)
                    
                    if temp_flow > 0:
                        residual[u][v] -= temp_flow
                        residual[v][u] += temp_flow  # Reverse edge
                        return temp_flow
                
                next[u]++
            
            return 0
        
        # Augment flow while possible
        while true:
            flow_increment = dfs(source, infinity)
            if flow_increment == 0:
                break
            flow += flow_increment
    
    return flow

# Implementation with adjacency list and edge objects
function dinicAdjList(graph, source, sink):
    n = number of vertices
    flow = 0
    
    # Create edge objects with capacities and reverse edge references
    edges = []
    adj = array of n empty lists
    
    for each edge (u, v, capacity) in graph:
        # Create forward and reverse edges
        forward = new Edge(u, v, capacity, 0)
        reverse = new Edge(v, u, 0, 0)
        
        # Set reverse edge references
        forward.reverse = reverse
        reverse.reverse = forward
        
        # Add to adjacency list
        adj[u].add(forward)
        adj[v].add(reverse)
        
        edges.add(forward)
    
    while true:
        # Build level graph using BFS
        level = new array of size n, initialized with -1
        level[source] = 0
        
        queue = new Queue()
        queue.enqueue(source)
        
        while not queue.isEmpty():
            u = queue.dequeue()
            
            for each edge in adj[u]:
                if level[edge.to] == -1 and edge.capacity > edge.flow:
                    level[edge.to] = level[u] + 1
                    queue.enqueue(edge.to)
        
        # If sink is not reachable, we're done
        if level[sink] == -1:
            break
        
        # Find blocking flow using DFS
        function dfs(u, flow_so_far):
            if u == sink:
                return flow_so_far
            
            for each edge in adj[u]:
                if level[edge.to] == level[u] + 1 and edge.capacity > edge.flow:
                    curr_flow = min(flow_so_far, edge.capacity - edge.flow)
                    temp_flow = dfs(edge.to, curr_flow)
                    
                    if temp_flow > 0:
                        edge.flow += temp_flow
                        edge.reverse.flow -= temp_flow
                        return temp_flow
            
            return 0
        
        # Augment flow while possible
        while true:
            flow_increment = dfs(source, infinity)
            if flow_increment == 0:
                break
            flow += flow_increment
    
    return flow
```

## Implementation Notes
- Use adjacency list representation for efficiency
- Implement residual graph using forward and reverse edges
- Consider using a current-edge optimization to avoid revisiting edges
- Be careful with integer overflow for large capacities
- For bipartite matching, construct the appropriate flow network
- Consider using scaling techniques for better performance with large capacities
- Handle parallel edges by merging them (adding capacities)
- Handle self-loops by removing them (they don't contribute to source-sink flow)
- Consider using a struct or class for edges with capacity and flow information
- Implement BFS and DFS efficiently to avoid redundant computations

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
    - Phase 1:
      - Level graph: s(0), A(1), B(1), t(2)
      - Augmenting path: s→A→t, flow = 3
      - Augmenting path: s→B→t, flow = 2
    - Phase 2:
      - Level graph: s(0), A(1), B(1), t(2)
      - Augmenting path: s→A→B→t, flow = 0 (A→B is saturated)
    - Total flow: 5
  
- Complex example:
  - Network with multiple paths and bottlenecks
  - Each phase finds a blocking flow in the level graph
  - Algorithm terminates when no more augmenting paths exist
  - Maximum flow equals minimum cut capacity

## Variants / Alternatives
- Unit capacity networks: Simplified implementation with better bounds
- Scaling Dinic's: For better performance with large capacities
- Dynamic tree implementation: For faster blocking flow computation
- Alternatives:
  - Ford-Fulkerson: Simpler but potentially slower
  - Edmonds-Karp: Uses BFS for augmenting paths
  - Push-relabel algorithm: Different approach with better worst-case
  - MPM algorithm: Similar to Dinic's with different blocking flow computation

## References
- Dinic, Yefim: "Algorithm for Solution of a Problem of Maximum Flow in Networks with Power Estimation" (1970)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Ahuja, Magnanti, Orlin: "Network Flows: Theory, Algorithms, and Applications"
- Kleinberg, Jon and Tardos, Eva: "Algorithm Design"
- Competitive Programming resources (e.g., CP-Algorithms) 