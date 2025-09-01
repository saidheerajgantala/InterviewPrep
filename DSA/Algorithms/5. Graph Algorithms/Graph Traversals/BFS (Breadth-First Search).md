# Breadth-First Search (BFS)

## Name
- Category: Graph Traversal
- Summary: Systematically explore a graph by visiting all vertices at the current depth before moving to vertices at the next depth level.

## Preconditions / Assumptions
- Graph representation available (adjacency list/matrix)
- Memory sufficient to store queue and visited set
- Can be applied to directed or undirected graphs
- Can handle disconnected graphs with multiple source vertices

## Properties
- Deterministic
- Complete (finds all reachable vertices)
- Optimal for unweighted shortest paths
- Level-by-level exploration
- Not recursive (typically implemented iteratively with a queue)

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Adjacency List | O(V+E) | O(V+E) | O(V+E) | O(V) | V vertices, E edges |
| Adjacency Matrix | O(V²) | O(V²) | O(V²) | O(V) | Dominated by matrix traversal |
| Implicit Graph | O(b^d) | O(b^d) | O(b^d) | O(b^d) | b: branching factor, d: depth |
| Bidirectional | O(b^(d/2)) | O(b^(d/2)) | O(b^(d/2)) | O(b^(d/2)) | Meets in the middle |

## When to Use / When to Avoid
- Use if:
  - Need shortest path in unweighted graph
  - Level-by-level traversal required
  - Finding all vertices at a given distance
  - Memory is sufficient for queue
- Avoid if:
  - Graph is weighted (use Dijkstra instead)
  - Need depth-first properties (use DFS)
  - Memory is severely constrained
  - Graph is implicit and very large

## Correctness (Proof Sketch)
- Invariant: At each step, all vertices at distance d are processed before any at distance d+1
- Queue ensures FIFO processing order, maintaining level-by-level exploration
- Visited set prevents cycles and redundant processing
- Termination: Each vertex is enqueued at most once
- Shortest path: First discovery of a vertex is always via shortest path in unweighted graphs

## Pseudocode (canonical)
```pseudo
function BFS(graph, source):
    # Initialize data structures
    queue = new Queue()
    visited = new Set()
    
    # Start from source
    queue.enqueue(source)
    visited.add(source)
    
    while queue is not empty:
        vertex = queue.dequeue()
        
        # Process vertex (e.g., print)
        process(vertex)
        
        # Explore neighbors
        for each neighbor of vertex:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.enqueue(neighbor)
                # Optional: parent[neighbor] = vertex (for path reconstruction)
```

## Implementation Notes
- Use an efficient queue implementation
- For shortest paths, track distances with a separate array
- For path reconstruction, maintain parent/predecessor links
- Can be extended to handle multiple sources (add all to initial queue)
- For large graphs, consider using a more compact visited representation (bit vector)
- Early termination possible if searching for specific target

## Edge-case Checklist
- Empty graph: Nothing to process
- Single vertex: Trivially processed
- Disconnected graph: Only processes reachable vertices from source
- Cycles: Handled by visited set
- Self-loops: Also handled by visited set
- Isolated vertices: Never discovered unless used as source

## Examples
- Minimal example:
  - Graph: A--B--C, A--D--E
  - Source: A
  - BFS order: A, B, D, C, E
  
- Shortest path example:
  - Graph: A--B--C--E, A--D--E
  - Source: A, Target: E
  - BFS finds path A--D--E (length 2) instead of A--B--C--E (length 3)

## Variants / Alternatives
- 0-1 BFS: For graphs with edge weights 0 or 1 (using deque)
- Multi-source BFS: Start from multiple sources simultaneously
- Bidirectional BFS: Search from both source and target
- Level-aware BFS: Explicitly track depth levels
- Grid BFS: Specialized for 2D/3D grids with implicit edges
- Alternatives:
  - DFS: Depth-first exploration (uses stack)
  - Dijkstra: For weighted shortest paths
  - A*: Informed search with heuristic

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 22.2
- Sedgewick, Wayne: "Algorithms, 4th Edition", Section 4.1
- Skiena, Steven: "The Algorithm Design Manual", Chapter 5
