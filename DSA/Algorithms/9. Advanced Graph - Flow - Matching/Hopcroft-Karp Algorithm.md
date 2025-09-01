# Hopcroft-Karp Algorithm

## Name
- Category: Graph Algorithms, Bipartite Matching
- Summary: Efficient algorithm for finding a maximum cardinality matching in bipartite graphs, using alternating paths to incrementally improve the matching, with a time complexity better than the standard augmenting path approach.

## Preconditions / Assumptions
- Graph is bipartite (can be divided into two sets U and V, with edges only between sets)
- Graph is represented as an adjacency list
- No self-loops or parallel edges (can be handled with preprocessing)
- The goal is to find a matching with maximum cardinality (number of edges)

## Properties
- Deterministic
- Complete (finds the maximum matching)
- More efficient than the standard augmenting path algorithm
- Uses breadth-first search to find shortest augmenting paths
- Uses depth-first search to find multiple disjoint augmenting paths
- Can be extended to solve related problems like minimum vertex cover in bipartite graphs

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(E√V) | O(V+E) | E edges, V vertices |
| Dense graphs | O(V²·√V) | O(V²) | When E ≈ V² |
| With heuristics | O(E√V) | O(V+E) | Better average case |
| Flow-based | O(E√V) | O(V+E) | Implementation using max flow |

## When to Use / When to Avoid
- Use if:
  - Need to find maximum bipartite matching
  - Graph is large and dense
  - Efficiency is important
  - Standard augmenting path algorithm is too slow
  - Implementation complexity is acceptable
- Avoid if:
  - Graph is not bipartite (use general matching algorithms)
  - Simplicity is more important than efficiency
  - Graph is very small (simpler algorithms may suffice)
  - Need weighted matching (use Hungarian algorithm)
  - Memory is severely constrained

## Correctness (Proof Sketch)
- The algorithm finds augmenting paths of increasing length
- In each phase, it finds a maximal set of shortest augmenting paths
- An augmenting path is a path that alternates between unmatched and matched edges, starting and ending with unmatched vertices
- After augmenting along such paths, the matching size increases
- The key insight: After O(√V) phases, the algorithm terminates
- Each phase takes O(E) time using BFS and DFS
- Therefore, the overall time complexity is O(E√V)
- Correctness follows from the augmenting path theorem: a matching is maximum if and only if there are no augmenting paths

## Pseudocode (canonical)
```pseudo
function hopcroftKarp(graph, u, v):
    # Initialize empty matching
    match_u = new array of size u, initialized with -1
    match_v = new array of size v, initialized with -1
    
    # Distance array for BFS
    dist = new array of size u+1, initialized with infinity
    
    # Counter for matched edges
    matching = 0
    
    while bfs(graph, match_u, match_v, dist):
        # Find a set of disjoint augmenting paths using DFS
        for i = 0 to u-1:
            if match_u[i] == -1 and dfs(graph, i, match_u, match_v, dist):
                matching++
    
    return matching

function bfs(graph, match_u, match_v, dist):
    queue = new Queue()
    
    # Add all unmatched vertices from U to the queue
    for u = 0 to u_size-1:
        if match_u[u] == -1:
            dist[u] = 0
            queue.enqueue(u)
        else:
            dist[u] = infinity
    
    # Special value for NIL vertex
    dist[nil] = infinity
    
    # BFS to find shortest augmenting paths
    while not queue.isEmpty():
        u = queue.dequeue()
        
        if dist[u] < dist[nil]:
            for each v in graph[u]:
                # Follow unmatched edges forward and matched edges backward
                if dist[match_v[v]] == infinity:
                    dist[match_v[v]] = dist[u] + 1
                    queue.enqueue(match_v[v])
    
    # Return true if an augmenting path was found
    return dist[nil] != infinity

function dfs(graph, u, match_u, match_v, dist):
    if u == nil:
        return true
    
    for each v in graph[u]:
        # Follow shortest augmenting path
        if dist[match_v[v]] == dist[u] + 1 and dfs(graph, match_v[v], match_u, match_v, dist):
            match_v[v] = u
            match_u[u] = v
            return true
    
    # No augmenting path found from u
    dist[u] = infinity
    return false

# Implementation with explicit NIL handling
function hopcroftKarpExplicit(graph, u_size, v_size):
    # Initialize empty matching
    match_u = new array of size u_size, initialized with -1
    match_v = new array of size v_size, initialized with -1
    
    # Distance array for BFS
    dist = new array of size u_size+1, initialized with infinity
    
    # Counter for matched edges
    matching = 0
    
    # NIL is represented as u_size
    nil = u_size
    
    while bfsExplicit(graph, match_u, match_v, dist, nil, u_size, v_size):
        # Find a set of disjoint augmenting paths using DFS
        for i = 0 to u_size-1:
            if match_u[i] == -1 and dfsExplicit(graph, i, match_u, match_v, dist, nil):
                matching++
    
    return matching

function bfsExplicit(graph, match_u, match_v, dist, nil, u_size, v_size):
    queue = new Queue()
    
    # Add all unmatched vertices from U to the queue
    for u = 0 to u_size-1:
        if match_u[u] == -1:
            dist[u] = 0
            queue.enqueue(u)
        else:
            dist[u] = infinity
    
    dist[nil] = infinity
    
    while not queue.isEmpty():
        u = queue.dequeue()
        
        if u != nil and dist[u] < dist[nil]:
            for each v in graph[u]:
                # v is matched to match_v[v]
                next_u = match_v[v]
                
                if next_u == -1:
                    next_u = nil
                
                if dist[next_u] == infinity:
                    dist[next_u] = dist[u] + 1
                    queue.enqueue(next_u)
    
    return dist[nil] != infinity

function dfsExplicit(graph, u, match_u, match_v, dist, nil):
    if u == nil:
        return true
    
    for each v in graph[u]:
        # v is matched to match_v[v]
        next_u = match_v[v]
        
        if next_u == -1:
            next_u = nil
        
        if dist[next_u] == dist[u] + 1 and dfsExplicit(graph, next_u, match_u, match_v, dist, nil):
            match_v[v] = u
            match_u[u] = v
            return true
    
    dist[u] = infinity
    return false
```

## Implementation Notes
- Use adjacency list representation for efficiency
- Represent NIL (unmatched) vertex carefully, either as -1 or as a special index
- Consider using a more explicit representation for bipartite graph (separate U and V)
- Implement BFS and DFS efficiently to avoid redundant computations
- Consider using bitsets for dense graphs to save memory
- For weighted matching, use the Hungarian algorithm instead
- Handle parallel edges by keeping only the best edge
- Implement proper queue and stack data structures for BFS and DFS
- Consider using iterative versions of BFS and DFS for very large graphs
- Use appropriate data structures for match_u, match_v, and dist arrays

## Edge-case Checklist
- Empty graph: Return 0 matching
- Single vertex: No matching possible
- No edges: Return 0 matching
- Complete bipartite graph: Matching size is min(|U|, |V|)
- Disconnected graph: Algorithm works correctly
- Unbalanced bipartite graph (|U| ≠ |V|): Algorithm works correctly
- Parallel edges: Keep only one edge between each pair
- Isolated vertices: Can be ignored

## Examples
- Minimal example:
  - Bipartite graph: U = {0, 1, 2}, V = {0, 1, 2}
  - Edges: (0,0), (0,1), (1,1), (1,2), (2,2)
  - Execution:
    - Initial matching: {}
    - Phase 1:
      - BFS finds augmenting paths of length 1
      - DFS augments along paths: 0→0, 1→1, 2→2
      - Matching: {(0,0), (1,1), (2,2)}
    - No more augmenting paths
    - Maximum matching size: 3
  
- Complex example:
  - Bipartite graph with multiple augmenting paths
  - Each phase finds shortest augmenting paths
  - Algorithm terminates when no more augmenting paths exist
  - Maximum matching equals minimum vertex cover in bipartite graphs

## Variants / Alternatives
- Flow-based implementation: Using max flow algorithms
- Incremental matching: For dynamic graphs
- Approximation algorithms: For very large graphs
- Alternatives:
  - Ford-Fulkerson algorithm: For general max flow
  - Hungarian algorithm: For weighted bipartite matching
  - Edmonds' Blossom algorithm: For general graph matching
  - Greedy matching: Simpler but suboptimal

## References
- Hopcroft, John E. and Karp, Richard M.: "An n^(5/2) Algorithm for Maximum Matchings in Bipartite Graphs" (1973)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Kleinberg, Jon and Tardos, Eva: "Algorithm Design"
- Galil, Zvi: "Efficient Algorithms for Finding Maximum Matching in Graphs" (1986)
- Competitive Programming resources (e.g., CP-Algorithms)
