# DFS-based Topological Sorting

## Name
- Category: Graph, Topological Sorting
- Summary: Algorithm that finds a linear ordering of vertices in a directed acyclic graph (DAG) such that for every directed edge (u, v), vertex u comes before v in the ordering, using depth-first search.

## Preconditions / Assumptions
- Directed acyclic graph (DAG)
- Graph is represented as an adjacency list
- No cycles (or cycle detection is required)
- Need to find a valid topological ordering of vertices

## Properties
- Deterministic
- Complete (finds a valid topological ordering if one exists)
- Linear time complexity
- Uses depth-first search
- Detects cycles if present
- Produces ordering in reverse postorder of DFS

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(V+E) | O(V+E) | V vertices, E edges |
| Iterative | O(V+E) | O(V+E) | Using explicit stack |
| With cycle detection | O(V+E) | O(V+E) | Additional bookkeeping |
| Lexicographically smallest | O(V+E log V) | O(V+E) | Using priority queue |

## When to Use / When to Avoid
- Use if:
  - Need to find dependencies or ordering in a DAG
  - Graph is guaranteed to be acyclic
  - Need to detect cycles in a directed graph
  - DFS is already being used for other purposes
  - Simplicity and linear time complexity are important
- Avoid if:
  - Graph may contain cycles (use cycle detection first)
  - Graph is undirected (topological sort not defined)
  - Need lexicographically smallest ordering (consider Kahn's algorithm)
  - Memory is extremely constrained

## Correctness (Proof Sketch)
- Key insight: In a DAG, vertices with no outgoing edges should come last in the ordering
- DFS explores paths to completion before backtracking
- Adding vertices to the result in reverse order of completion ensures dependencies are satisfied
- If a back edge is detected during DFS, the graph contains a cycle and no valid topological order exists
- Proof relies on properties of DFS and the definition of a DAG

## Pseudocode (canonical)
```pseudo
function topologicalSortDFS(graph):
    n = number of vertices
    
    # Initialize data structures
    visited = new boolean array of size n, initialized with false
    temp_visited = new boolean array of size n, initialized with false  # For cycle detection
    ordering = new empty list
    
    # Process all vertices
    for v = 0 to n-1:
        if not visited[v]:
            if hasCycle(graph, v, visited, temp_visited, ordering):
                return "Graph has a cycle, no topological ordering exists"
    
    # Reverse the ordering to get the topological sort
    return reverse(ordering)

function hasCycle(graph, v, visited, temp_visited, ordering):
    # Mark as temporarily visited (for cycle detection)
    temp_visited[v] = true
    
    # Visit all neighbors
    for each neighbor u of v in graph:
        if temp_visited[u]:
            # Back edge found, cycle detected
            return true
        
        if not visited[u]:
            if hasCycle(graph, u, visited, temp_visited, ordering):
                return true
    
    # Mark as permanently visited
    visited[v] = true
    temp_visited[v] = false
    
    # Add to ordering (in reverse order of completion)
    ordering.add(v)
    
    return false

# Iterative implementation
function topologicalSortDFSIterative(graph):
    n = number of vertices
    
    # Initialize data structures
    visited = new boolean array of size n, initialized with false
    ordering = new empty list
    
    # Process all vertices
    for v = 0 to n-1:
        if not visited[v]:
            dfsIterative(graph, v, visited, ordering)
    
    # Reverse the ordering to get the topological sort
    return reverse(ordering)

function dfsIterative(graph, start, visited, ordering):
    stack = new Stack()
    stack.push(start)
    
    while not stack.isEmpty():
        v = stack.peek()
        
        if all neighbors of v are visited or processed:
            stack.pop()
            if not visited[v]:
                visited[v] = true
                ordering.add(v)
        else:
            # Find an unvisited neighbor
            for each neighbor u of v in graph:
                if not visited[u] and u not on stack:
                    stack.push(u)
                    break
```

## Implementation Notes
- Use adjacency list representation for efficient graph traversal
- For cycle detection, maintain two visited arrays: permanent and temporary
- The result should be reversed after DFS to get the correct topological order
- For large graphs, consider iterative implementation to avoid stack overflow
- The ordering can be stored in an array or linked list
- Multiple valid topological orderings may exist; DFS produces one of them
- To get lexicographically smallest ordering, process vertices in sorted order
- Can be combined with strongly connected components to handle cyclic graphs
- For applications requiring dynamic updates, consider incremental algorithms

## Edge-case Checklist
- Empty graph: Return empty ordering
- Single vertex: Ordering contains just that vertex
- Disconnected DAG: All components will be included in the ordering
- Graph with cycles: No valid topological ordering exists
- Complete DAG: Only one valid topological ordering exists
- Linear chain: Only one valid topological ordering exists
- Multiple valid orderings: DFS will produce one of them

## Examples
- Minimal example:
  - Graph: 5→0, 5→2, 4→0, 4→1, 2→3, 3→1
  - DFS execution:
    - Start at 0: visited=[0], ordering=[0]
    - Start at 1: visited=[0,1], ordering=[0,1]
    - Start at 2: visit 3, visited=[0,1,2,3], ordering=[0,1,3,2]
    - Start at 4: visit 0 (already visited), visit 1 (already visited), visited=[0,1,2,3,4], ordering=[0,1,3,2,4]
    - Start at 5: visit 0 (already visited), visit 2 (already visited), visited=[0,1,2,3,4,5], ordering=[0,1,3,2,4,5]
  - Reverse ordering: [5,4,2,3,1,0]
  - This is a valid topological sort
  
- Visualization:
  ```
    5 → 0
    ↓   ↑
    2 → 3 → 1
        ↑
        4 →
  ```

## Variants / Alternatives
- Iterative implementation: Uses explicit stack instead of recursion
- Lexicographically smallest ordering: Process vertices in sorted order
- Kahn's algorithm: Alternative approach using in-degrees
- Tarjan's algorithm: Can be adapted to find SCCs and topological sort
- Alternatives:
  - Kahn's algorithm: Often preferred for simplicity
  - Dynamic topological sort: For graphs with incremental updates
  - Parallel topological sort: For large graphs
  - Layer-based approach: For visualizing DAGs

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 22.4
- Sedgewick, Wayne: "Algorithms, 4th Edition", Section 4.2
- Skiena, Steven: "The Algorithm Design Manual", Chapter 5
- Tarjan, Robert: "Depth-first search and linear graph algorithms" (1972)
- Kahn, Arthur B.: "Topological sorting of large networks" (1962)
