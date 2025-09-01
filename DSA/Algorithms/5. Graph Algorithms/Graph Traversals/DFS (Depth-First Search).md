# Depth-First Search (DFS)

## Name
- Category: Graph Traversal
- Summary: Recursive algorithm that explores a graph by going as deep as possible along each branch before backtracking.

## Preconditions / Assumptions
- Graph representation available (adjacency list/matrix)
- Memory sufficient for recursion stack or explicit stack
- Can be applied to directed or undirected graphs
- Can handle disconnected graphs with multiple source vertices

## Properties
- Deterministic
- Complete (finds all reachable vertices)
- Memory-efficient (only stores path from root to current node)
- Explores one branch fully before exploring siblings
- Naturally recursive, though can be implemented iteratively

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Adjacency List | O(V+E) | O(V+E) | O(V+E) | O(V) | V vertices, E edges |
| Adjacency Matrix | O(V²) | O(V²) | O(V²) | O(V) | Dominated by matrix traversal |
| Recursive | O(V+E) | O(V+E) | O(V+E) | O(V) | Call stack space |
| Iterative | O(V+E) | O(V+E) | O(V+E) | O(V) | Explicit stack space |
| Implicit Graph | O(b^d) | O(b^d) | O(b^d) | O(d) | b: branching factor, d: depth |

## When to Use / When to Avoid
- Use if:
  - Need to explore all paths or branches
  - Detecting cycles in a graph
  - Topological sorting
  - Finding strongly connected components
  - Memory is more constrained than BFS requires
- Avoid if:
  - Need shortest path in unweighted graph (use BFS)
  - Graph is very deep (stack overflow risk)
  - Level-by-level traversal required
  - Graph is implicit and very large

## Correctness (Proof Sketch)
- Invariant: Visited set contains all discovered vertices
- Each vertex is processed exactly once
- Stack (implicit or explicit) ensures backtracking to explore all branches
- Termination: Each vertex is visited at most once
- Completeness: All reachable vertices from source are eventually visited
- Cycle detection: Back edges indicate cycles in the graph

## Pseudocode (canonical)
```pseudo
# Recursive DFS
function DFS(graph, vertex, visited):
    # Mark current vertex as visited
    visited.add(vertex)
    
    # Process vertex (e.g., print)
    process(vertex)
    
    # Explore all adjacent vertices
    for each neighbor of vertex:
        if neighbor not in visited:
            DFS(graph, neighbor, visited)

# Iterative DFS
function DFS_iterative(graph, start):
    # Initialize data structures
    stack = new Stack()
    visited = new Set()
    
    # Start from source
    stack.push(start)
    
    while stack is not empty:
        vertex = stack.pop()
        
        if vertex not in visited:
            # Mark and process vertex
            visited.add(vertex)
            process(vertex)
            
            # Add all unvisited neighbors to stack
            for each neighbor of vertex:
                if neighbor not in visited:
                    stack.push(neighbor)
```

## Implementation Notes
- Can be implemented recursively or iteratively with explicit stack
- Watch for stack overflow in recursive implementation with deep graphs
- For cycle detection, track vertices in current recursion path
- For topological sort, add vertices to result when backtracking
- Pre-order, post-order, and in-order traversals possible (for trees)
- Can track discovery and finish times for advanced algorithms
- Consider using colors (white, gray, black) instead of visited set for more information

## Edge-case Checklist
- Empty graph: Nothing to process
- Single vertex: Trivially processed
- Disconnected graph: Need multiple DFS calls from different sources
- Cycles: Handled by visited set (prevents infinite recursion)
- Self-loops: Also handled by visited set
- Deep graphs: Watch for stack overflow in recursive implementation
- Dense graphs: Adjacency list more efficient than matrix

## Examples
- Minimal example:
  - Graph: A--B--C, A--D--E
  - Source: A
  - DFS order: A, B, C, D, E (or A, D, E, B, C depending on neighbor order)
  
- Cycle detection example:
  - Graph: A--B--C--A (triangle)
  - DFS with additional tracking detects cycle when exploring C's neighbor A

## Variants / Alternatives
- Pre-order DFS: Process vertex before children
- Post-order DFS: Process vertex after children
- DFS with timestamps: Track discovery and finish times
- DFS for cycle detection: Track vertices in current path
- DFS for topological sorting: Add vertices to result when backtracking
- DFS for strongly connected components (Kosaraju's algorithm)
- Alternatives:
  - BFS: Level-by-level exploration (uses queue)
  - Iterative deepening DFS: Combines benefits of BFS and DFS
  - Bidirectional search: For finding paths between two vertices

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 22.3
- Sedgewick, Wayne: "Algorithms, 4th Edition", Section 4.1
- Skiena, Steven: "The Algorithm Design Manual", Chapter 5
