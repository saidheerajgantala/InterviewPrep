# Minimum-Cost Flow

## Name
- Category: Graph Algorithms, Network Flow, Optimization
- Summary: Algorithm for finding the flow of minimum cost in a flow network, where each edge has both a capacity and a cost per unit flow, with applications in transportation, assignment, and resource allocation problems.

## Preconditions / Assumptions
- Directed graph with a single source and sink
- Each edge has a non-negative capacity
- Each edge has a cost per unit flow (can be negative)
- No negative-cost cycles in the residual graph (or they are detected)
- Flow conservation at each vertex (except source and sink)
- Graph is represented as an adjacency list with capacity and cost information

## Properties
- Deterministic
- Complete (finds the optimal solution)
- Combines shortest path algorithms with flow augmentation
- Can handle negative edge costs (but not negative cycles)
- Can be used to solve various optimization problems
- More general than maximum flow or shortest path alone
- Can be extended to handle multiple sources and sinks

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Successive Shortest Path | O(F · (E + V log V)) | O(V+E) | F = max flow, V vertices, E edges |
| Cycle Canceling | O(F · C · V · E) | O(V+E) | C = max edge cost |
| Cost Scaling | O(E² log V log(VC)) | O(V+E) | C = max edge cost |
| Network Simplex | O(E² V log(VC)) | O(V+E) | Practical performance often better |
| Capacity Scaling | O(E log V · log(VU)) | O(V+E) | U = max capacity |

## When to Use / When to Avoid
- Use if:
  - Need to find flow with minimum cost
  - Problem involves both capacity constraints and costs
  - Problem can be modeled as a min-cost flow
  - Need to solve assignment, transportation, or circulation problems
  - Maximum flow alone is not sufficient
- Avoid if:
  - Only maximum flow is needed (use specialized max flow algorithms)
  - Only shortest path is needed (use specialized shortest path algorithms)
  - Graph is very large and dense (consider approximation algorithms)
  - Memory is severely constrained
  - Problem has additional constraints not expressible in the model

## Correctness (Proof Sketch)
- For Successive Shortest Path algorithm:
  - Start with zero flow
  - Repeatedly find shortest augmenting paths in the residual graph
  - Augment flow along these paths until maximum flow is reached
  - Each augmentation increases flow and maintains minimum cost
  - When no more augmenting paths exist, the flow is maximum and minimum cost
  - Correctness follows from the optimality principle of shortest paths
- For Cycle Canceling algorithm:
  - Start with a maximum flow (using any max flow algorithm)
  - Repeatedly find and cancel negative-cost cycles in the residual graph
  - When no negative cycles remain, the flow is of minimum cost
  - Correctness follows from the negative cycle optimality condition

## Pseudocode (canonical)
```pseudo
# Successive Shortest Path Algorithm
function minCostFlow(graph, source, sink, target_flow):
    n = number of vertices
    flow = 0
    cost = 0
    
    # Initialize flow to zero
    for each edge (u, v) in graph:
        edge.flow = 0
    
    # Augment flow along shortest paths until target flow is reached
    while flow < target_flow:
        # Find shortest path in residual graph using Dijkstra or Bellman-Ford
        dist, parent = shortestPath(graph, source, sink)
        
        if dist[sink] == infinity:
            break  # No more augmenting paths
        
        # Find bottleneck capacity
        path_flow = min(target_flow - flow, bottleneck(graph, source, sink, parent))
        
        # Augment flow
        augmentFlow(graph, source, sink, parent, path_flow)
        
        flow += path_flow
        cost += path_flow * dist[sink]
    
    return flow, cost

function shortestPath(graph, source, sink):
    n = number of vertices
    dist = new array of size n, initialized with infinity
    parent = new array of size n, initialized with -1
    
    dist[source] = 0
    
    # Use Dijkstra's algorithm for non-negative costs
    # or Bellman-Ford for potentially negative costs
    priority_queue = new PriorityQueue()
    priority_queue.add(source, 0)
    
    while not priority_queue.isEmpty():
        u = priority_queue.extractMin()
        
        for each edge (u, v) in graph:
            if edge.capacity - edge.flow > 0 and dist[u] + edge.cost < dist[v]:
                dist[v] = dist[u] + edge.cost
                parent[v] = u
                priority_queue.add(v, dist[v])
    
    return dist, parent

function bottleneck(graph, source, sink, parent):
    path_flow = infinity
    v = sink
    
    while v != source:
        u = parent[v]
        edge = graph.getEdge(u, v)
        path_flow = min(path_flow, edge.capacity - edge.flow)
        v = u
    
    return path_flow

function augmentFlow(graph, source, sink, parent, flow):
    v = sink
    
    while v != source:
        u = parent[v]
        edge = graph.getEdge(u, v)
        edge.flow += flow
        reverse_edge = graph.getEdge(v, u)  # Reverse edge for residual graph
        reverse_edge.flow -= flow
        v = u

# Cycle Canceling Algorithm
function minCostFlowCycleCanceling(graph, source, sink):
    # First find maximum flow
    max_flow = fordFulkerson(graph, source, sink)
    
    # Then repeatedly cancel negative cycles
    while true:
        # Find negative cycle in residual graph
        cycle, cycle_cost = findNegativeCycle(graph)
        
        if cycle is empty:
            break  # No more negative cycles
        
        # Find bottleneck capacity in the cycle
        bottleneck = infinity
        for i = 0 to cycle.length - 1:
            u = cycle[i]
            v = cycle[(i + 1) % cycle.length]
            edge = graph.getEdge(u, v)
            bottleneck = min(bottleneck, edge.capacity - edge.flow)
        
        # Augment flow along the cycle
        for i = 0 to cycle.length - 1:
            u = cycle[i]
            v = cycle[(i + 1) % cycle.length]
            edge = graph.getEdge(u, v)
            edge.flow += bottleneck
            reverse_edge = graph.getEdge(v, u)
            reverse_edge.flow -= bottleneck
    
    # Calculate total cost
    cost = 0
    for each edge (u, v) in graph:
        cost += edge.flow * edge.cost
    
    return max_flow, cost

function findNegativeCycle(graph):
    # Use Bellman-Ford to find negative cycle
    n = number of vertices
    dist = new array of size n, initialized with 0
    parent = new array of size n, initialized with -1
    
    # Relax all edges n times
    for i = 1 to n:
        for each edge (u, v) in graph:
            if edge.capacity - edge.flow > 0 and dist[u] + edge.cost < dist[v]:
                dist[v] = dist[u] + edge.cost
                parent[v] = u
    
    # Check for negative cycle
    for each edge (u, v) in graph:
        if edge.capacity - edge.flow > 0 and dist[u] + edge.cost < dist[v]:
            # Found negative cycle, reconstruct it
            cycle = []
            visited = new set
            
            while u not in visited:
                visited.add(u)
                u = parent[u]
            
            # u is now part of the cycle
            start = u
            cycle.add(u)
            u = parent[u]
            
            while u != start:
                cycle.add(u)
                u = parent[u]
            
            cycle_cost = 0
            for i = 0 to cycle.length - 1:
                u = cycle[i]
                v = cycle[(i + 1) % cycle.length]
                edge = graph.getEdge(u, v)
                cycle_cost += edge.cost
            
            return cycle, cycle_cost
    
    return [], 0  # No negative cycle found
```

## Implementation Notes
- Use adjacency list representation with forward and reverse edges
- Implement residual graph efficiently using edge objects with flow and capacity
- For negative costs, use Bellman-Ford or SPFA for shortest paths
- For non-negative costs, use Dijkstra's algorithm for better performance
- Consider using potential functions to convert negative costs to non-negative
- Handle parallel edges by merging them (adding capacities)
- Be careful with integer overflow for large capacities or costs
- For multiple sources/sinks, add a super-source/super-sink
- Consider using capacity scaling for better performance
- Implement efficient data structures for priority queue and cycle detection

## Edge-case Checklist
- Empty graph: Return 0 flow, 0 cost
- Single vertex (source = sink): Return 0 flow, 0 cost
- No path from source to sink: Return 0 flow, 0 cost
- Negative-cost cycle: Detect and handle appropriately
- Zero-capacity edges: Can be removed or ignored
- Zero-cost edges: Handle normally
- Large capacities or costs: Watch for integer overflow
- Multiple sources/sinks: Use super-source/super-sink
- Infeasible flow demand: Return appropriate indicator

## Examples
- Minimal example:
  - Graph: s→A (capacity 3, cost 1), s→B (capacity 2, cost 2), A→B (capacity 2, cost 1), A→t (capacity 3, cost 3), B→t (capacity 2, cost 1)
  - Execution:
    - Iteration 1:
      - Shortest path: s→A→t, cost = 1+3 = 4, flow = 3
    - Iteration 2:
      - Shortest path: s→B→t, cost = 2+1 = 3, flow = 2
    - Total flow: 5, Total cost: 3*4 + 2*3 = 12 + 6 = 18
  
- Transportation problem:
  - Suppliers with capacities, consumers with demands
  - Edges represent transportation routes with costs
  - Min-cost flow finds optimal allocation

## Variants / Alternatives
- Successive Shortest Path: Augment along shortest paths
- Cycle Canceling: Cancel negative cycles in residual graph
- Cost Scaling: Scale costs to improve efficiency
- Capacity Scaling: Scale capacities to improve efficiency
- Network Simplex: Adaptation of linear programming simplex method
- Alternatives:
  - Linear programming: More general but potentially slower
  - Hungarian algorithm: For assignment problems
  - Transportation simplex: For transportation problems
  - Approximation algorithms: For very large instances

## References
- Ahuja, Ravindra K., Magnanti, Thomas L., and Orlin, James B.: "Network Flows: Theory, Algorithms, and Applications" (1993)
- Goldberg, Andrew V. and Tarjan, Robert E.: "Finding Minimum-Cost Circulations by Successive Approximation" (1990)
- Kleinberg, Jon and Tardos, Eva: "Algorithm Design", Chapter 7
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Schrijver, Alexander: "Combinatorial Optimization: Polyhedra and Efficiency"
