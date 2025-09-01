# Prim's Algorithm

## Name
- Category: Graph, Minimum Spanning Tree
- Summary: Greedy algorithm that finds a minimum spanning tree for a weighted, connected, undirected graph by growing a single tree from an arbitrary starting vertex.

## Preconditions / Assumptions
- Connected, undirected graph
- Edge weights are comparable (typically real numbers)
- No negative edge weights (not strictly required, but typical)
- Graph representation allows efficient neighbor access (adjacency list preferred)

## Properties
- Deterministic
- Complete (finds optimal solution)
- Greedy (selects minimum weight edge crossing the cut)
- Results in a spanning tree with minimum total weight
- Produces same MST weight as Kruskal's (though possibly different tree)
- Grows a single tree from a starting vertex

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Array implementation | O(V²) | O(V) | Good for dense graphs |
| Binary heap | O(E log V) | O(V) | Good for sparse graphs |
| Fibonacci heap | O(E + V log V) | O(V + E) | Theoretical improvement |
| Adjacency matrix | O(V²) | O(V²) | For dense graphs |
| Adjacency list | O(E log V) | O(V + E) | For sparse graphs |

## When to Use / When to Avoid
- Use if:
  - Need minimum spanning tree of a graph
  - Graph is dense (V² ≈ E)
  - Adjacency matrix representation is available
  - Starting from a specific vertex is preferred
- Avoid if:
  - Graph is very sparse (Kruskal's may be better)
  - Graph is disconnected (will only find MST of connected component)
  - Edge list representation is more natural
  - Memory usage is severely constrained

## Correctness (Proof Sketch)
- Cut property: For any cut, the minimum weight edge crossing the cut is in MST
- Invariant: At each step, edges selected form a tree that is a subset of MST
- Greedy choice: Adding minimum weight edge crossing the cut is safe
- Termination: After V-1 edges, all vertices are connected
- Optimality: Can be proven by contradiction (if not optimal, there exists an edge that should be in MST)

## Pseudocode (canonical)
```pseudo
function primMST(graph, start):
    n = graph.vertices.size
    
    # Initialize arrays
    parent = new array of size n, filled with -1
    key = new array of size n, filled with infinity
    inMST = new array of size n, filled with false
    
    # Start with the first vertex
    key[start] = 0
    
    # Find MST with V vertices
    for count = 0 to n-1:
        # Find minimum key vertex not yet in MST
        u = extractMinKey(key, inMST)
        inMST[u] = true
        
        # Update key values of adjacent vertices
        for each neighbor v of u:
            weight = weight of edge (u, v)
            if not inMST[v] and weight < key[v]:
                parent[v] = u
                key[v] = weight
    
    # Construct MST from parent array
    mst = []
    for i = 1 to n-1:  # Skip start vertex
        if parent[i] != -1:
            mst.add(Edge(parent[i], i, weight between parent[i] and i))
    
    return mst

# Using priority queue for efficiency
function primMSTWithPriorityQueue(graph, start):
    n = graph.vertices.size
    
    # Initialize arrays
    parent = new array of size n, filled with -1
    key = new array of size n, filled with infinity
    inMST = new array of size n, filled with false
    
    # Priority queue to extract minimum key vertex
    pq = new PriorityQueue()
    
    # Start with the first vertex
    key[start] = 0
    pq.add((start, 0))  # (vertex, key)
    
    while not pq.isEmpty() and count < n:
        # Extract vertex with minimum key
        u, min_key = pq.extractMin()
        
        if inMST[u]:
            continue  # Skip if already in MST
        
        inMST[u] = true
        count++
        
        # Update key values of adjacent vertices
        for each neighbor v of u:
            weight = weight of edge (u, v)
            if not inMST[v] and weight < key[v]:
                parent[v] = u
                key[v] = weight
                pq.add((v, key[v]))
    
    # Construct MST from parent array
    mst = []
    for i = 1 to n-1:  # Skip start vertex
        if parent[i] != -1:
            mst.add(Edge(parent[i], i, weight between parent[i] and i))
    
    return mst
```

## Implementation Notes
- Use a priority queue (min-heap) for efficient extraction of minimum key vertex
- For dense graphs, simple array implementation may be faster
- Consider using a Fibonacci heap for theoretical improvement
- Be careful with graph representation (adjacency list vs. matrix)
- Handle disconnected graphs by checking if all vertices are included
- For large graphs, consider optimizing the priority queue operations
- Can be modified to find maximum spanning tree by negating weights
- Lazy implementation: don't update keys in priority queue, just add new entries

## Edge-case Checklist
- Empty graph: Return empty MST
- Single vertex: No edges in MST
- Disconnected graph: Will only find MST of component containing start vertex
- Equal weight edges: Any consistent tie-breaking works
- Negative weights: Algorithm still works correctly
- Cycles: Handled by checking if vertex is already in MST
- Different start vertices: Same total weight, possibly different tree

## Examples
- Minimal example:
  - Graph: A--(1)--B--(3)--C, A--(4)--C
  - Starting from A:
    - Add edge A-B (weight 1)
    - Add edge B-C (weight 3)
  - MST: A--(1)--B--(3)--C with weight 4
  
- Step-by-step example:
  - Graph with vertices A, B, C, D and edges:
    - A-B: 2, A-D: 1, B-C: 3, B-D: 2, C-D: 4
  - Start from A:
    - key = [0, ∞, ∞, ∞], inMST = [false, false, false, false]
    - Extract A: key = [0, 2, ∞, 1], inMST = [true, false, false, false]
    - Extract D: key = [0, 2, ∞, 1], inMST = [true, false, false, true]
    - Extract B: key = [0, 2, 3, 1], inMST = [true, true, false, true]
    - Extract C: key = [0, 2, 3, 1], inMST = [true, true, true, true]
  - MST: A-D (1), D-B (2), B-C (3) with total weight 6

## Variants / Alternatives
- Lazy Prim's: Don't decrease key, just add new entries to priority queue
- Dense graph implementation: Use array instead of priority queue
- Maximum Spanning Tree: Negate weights
- Minimum Bottleneck Spanning Tree: Minimize maximum edge weight
- Alternatives:
  - Kruskal's algorithm: Sorts edges, better for sparse graphs
  - Borůvka's algorithm: Good for parallel implementation
  - Reverse-delete algorithm: Complement of Kruskal's

## References
- Prim, R.C. "Shortest connection networks and some generalizations" (1957)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 23.2
- Sedgewick, Wayne: "Algorithms, 4th Edition", Section 4.3
- Skiena, Steven: "The Algorithm Design Manual", Chapter 6 