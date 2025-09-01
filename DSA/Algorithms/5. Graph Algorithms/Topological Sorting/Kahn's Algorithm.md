# Kahn's Algorithm (Topological Sort)

## Name
- Category: Graph Algorithm
- Summary: Iterative algorithm for topological sorting that repeatedly removes nodes with no incoming edges from a directed acyclic graph.

## Preconditions / Assumptions
- Directed Acyclic Graph (DAG)
- Graph representation available (adjacency list preferred)
- No cycles (algorithm can detect cycles)
- Need to find a linear ordering of vertices

## Properties
- Deterministic
- Complete (finds a valid topological ordering if one exists)
- Can detect cycles in the graph
- Non-recursive (uses queue instead of recursion stack)
- May produce different valid orderings for the same graph

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(V + E) | O(V) | V vertices, E edges |
| With cycle detection | O(V + E) | O(V) | Returns empty if cycle exists |
| Priority Queue | O(E log V) | O(V) | Prioritizes vertices by some criteria |
| Lexicographically smallest | O(E log V) | O(V) | Using min-heap for smallest vertex ID |

## When to Use / When to Avoid
- Use if:
  - Need a topological ordering of a DAG
  - Want to detect cycles in a directed graph
  - Prefer iterative approach over recursive
  - Need to process nodes in order of dependencies
- Avoid if:
  - Graph is not directed (topological sort undefined)
  - Graph may contain cycles (use cycle detection variant)
  - Need to enumerate all possible topological sorts
  - DFS-based approach is more suitable for the application

## Correctness (Proof Sketch)
- Invariant: Vertices with no incoming edges can be placed next in the ordering
- Each iteration removes a vertex with no dependencies
- If the graph is a DAG, there is always at least one vertex with no incoming edges
- Removing a vertex and its edges maintains the DAG property
- Termination: All vertices are processed exactly once
- Cycle detection: If unable to process all vertices, a cycle exists

## Pseudocode (canonical)
```pseudo
function kahnTopologicalSort(graph):
    # Count incoming edges for each vertex
    inDegree = new map()
    for each vertex v in graph:
        inDegree[v] = 0
    
    for each vertex v in graph:
        for each neighbor u of v:
            inDegree[u]++
    
    # Enqueue vertices with no incoming edges
    queue = new Queue()
    for each vertex v in graph:
        if inDegree[v] == 0:
            queue.enqueue(v)
    
    # Process vertices in topological order
    result = new list()
    while queue is not empty:
        vertex = queue.dequeue()
        result.add(vertex)
        
        for each neighbor of vertex:
            inDegree[neighbor]--
            if inDegree[neighbor] == 0:
                queue.enqueue(neighbor)
    
    # Check if topological sort is possible
    if result.size != graph.vertices.size:
        return "Graph has a cycle"
    
    return result
```

## Implementation Notes
- Use adjacency list representation for efficiency
- Can use priority queue instead of regular queue for specific ordering criteria
- For cycle detection, check if all vertices are included in the result
- Can be modified to return partial ordering even if cycles exist
- Consider using a set for faster lookup of zero in-degree vertices
- For large graphs, optimize in-degree calculation
- Can be extended to find all possible topological sorts (with backtracking)

## Edge-case Checklist
- Empty graph: Return empty result
- Single vertex: Return the vertex
- Disconnected DAG: Still produces valid topological ordering
- Graph with cycle: No valid topological ordering (algorithm detects this)
- Multiple valid orderings: Algorithm produces one of them
- Isolated vertices: Included in the result (can be placed anywhere)

## Examples
- Minimal example:
  - Graph: A→B→C, A→C, D→C
  - In-degree: A:0, B:1, C:3, D:0
  - Result: [A,D,B,C] or [D,A,B,C]
  
- Cycle detection example:
  - Graph: A→B→C→A (cycle)
  - After processing: No vertices with in-degree 0 remain
  - Result: Incomplete (cycle detected)

## Variants / Alternatives
- Priority Queue variant: Process vertices in specific order
- Lexicographically smallest topological sort: Use min-heap
- All topological sorts: Use backtracking
- Partial topological sort: Allow some cycles
- Online topological sort: Handle dynamic graph changes
- Alternatives:
  - DFS-based topological sort: Uses recursion and finishing times
  - Tarjan's algorithm: Combines topological sort with SCC detection
  - Critical path method: Topological sort with weights

## References
- Kahn, Arthur B. "Topological sorting of large networks" (1962)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 22.4
- Sedgewick, Wayne: "Algorithms, 4th Edition", Section 4.2
- Skiena, Steven: "The Algorithm Design Manual", Chapter 5 