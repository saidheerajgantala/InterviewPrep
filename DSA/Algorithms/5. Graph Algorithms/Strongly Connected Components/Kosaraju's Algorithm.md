# Kosaraju's Algorithm

## Name
- Category: Graph, Strongly Connected Components
- Summary: Algorithm to find all strongly connected components (SCCs) in a directed graph using two depth-first searches, first to compute a finishing order and then to identify the components.

## Preconditions / Assumptions
- Directed graph
- Graph is represented as an adjacency list
- Need to identify all strongly connected components
- A strongly connected component is a maximal subgraph where every vertex is reachable from every other vertex

## Properties
- Deterministic
- Complete (finds all strongly connected components)
- Linear time complexity
- Uses two depth-first searches
- Requires the transpose (reversed) graph
- Identifies components in topological order between components

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(V+E) | O(V+E) | V vertices, E edges |
| Iterative | O(V+E) | O(V+E) | Using explicit stack |
| With component graph | O(V+E) | O(V+E) | Builds condensation graph |
| Tarjan's alternative | O(V+E) | O(V+E) | Single pass but more complex |

## When to Use / When to Avoid
- Use if:
  - Need to find strongly connected components in a directed graph
  - Need to detect cycles in a directed graph
  - Need to convert a directed graph to a directed acyclic graph (DAG)
  - Linear time complexity is important
  - Simplicity is preferred over optimized constants
- Avoid if:
  - Graph is undirected (use connected components algorithm)
  - Only need to check if graph is strongly connected (simpler algorithm exists)
  - Memory is extremely constrained (though still O(V+E))
  - Tarjan's algorithm is preferred for better constant factors

## Correctness (Proof Sketch)
- Key insight: Vertices in the same SCC have the same set of reachable vertices
- First DFS: Computes finishing times which orders vertices by their position in the component graph
- Transpose graph: Reverses all edges, preserving SCCs but changing reachability between components
- Second DFS: Starting from highest finishing time, identifies each SCC
- Vertices visited in a single DFS call of the second pass form an SCC
- Proof relies on properties of DFS and the relationship between a graph and its transpose

## Pseudocode (canonical)
```pseudo
function kosaraju(graph):
    n = number of vertices
    
    # First DFS to compute finishing order
    visited = new boolean array of size n, initialized with false
    finish_order = new empty stack
    
    for each vertex v in graph:
        if not visited[v]:
            dfs_first(graph, v, visited, finish_order)
    
    # Create transpose graph (reverse all edges)
    transpose = reverseGraph(graph)
    
    # Second DFS to find SCCs
    visited = new boolean array of size n, initialized with false
    components = []
    
    while not finish_order.isEmpty():
        v = finish_order.pop()
        if not visited[v]:
            current_component = []
            dfs_second(transpose, v, visited, current_component)
            components.add(current_component)
    
    return components

function dfs_first(graph, v, visited, finish_order):
    visited[v] = true
    
    for each neighbor u of v in graph:
        if not visited[u]:
            dfs_first(graph, u, visited, finish_order)
    
    finish_order.push(v)

function dfs_second(graph, v, visited, component):
    visited[v] = true
    component.add(v)
    
    for each neighbor u of v in graph:
        if not visited[u]:
            dfs_second(graph, u, visited, component)

function reverseGraph(graph):
    n = number of vertices
    transpose = new graph with n vertices and no edges
    
    for each vertex v in graph:
        for each neighbor u of v in graph:
            transpose.addEdge(u, v)  # Reverse edge direction
    
    return transpose

# Building the component graph (condensation)
function buildComponentGraph(graph, components):
    n = number of components
    component_graph = new graph with n vertices and no edges
    
    # Map each vertex to its component
    vertex_to_component = new map
    for i = 0 to n-1:
        for each vertex v in components[i]:
            vertex_to_component[v] = i
    
    # Add edges between components
    for i = 0 to n-1:
        for each vertex v in components[i]:
            for each neighbor u of v in original graph:
                j = vertex_to_component[u]
                if i != j:  # Avoid self-loops
                    component_graph.addEdge(i, j)
    
    return component_graph
```

## Implementation Notes
- Use adjacency list representation for efficient graph traversal
- Stack can be implemented as an array with a pointer
- For large graphs, consider iterative DFS to avoid stack overflow
- The transpose graph can be built during the first pass to save memory
- Consider using a vector of vectors to store components
- For component graph construction, use a set to avoid duplicate edges
- Vertex numbering should be consistent (typically 0 to n-1)
- Can be combined with topological sort of the component graph
- To detect if a graph is strongly connected, check if there's only one component

## Edge-case Checklist
- Empty graph: Return empty result
- Single vertex: Forms its own SCC
- Disconnected graph: Each disconnected part has its own SCCs
- Acyclic graph: Each vertex is its own SCC
- Complete graph: The entire graph is one SCC
- Self-loops: Don't affect SCCs
- Parallel edges: Don't affect SCCs (can be treated as single edges)

## Examples
- Minimal example:
  - Graph: 0→1→2→0, 2→3, 3→4, 4→5, 5→3
  - First DFS finishing order: 5, 4, 3, 2, 1, 0 (stack top to bottom)
  - Transpose: 0→2, 1→0, 2→1, 3→2, 3→5, 4→3, 5→4
  - Second DFS:
    - Start from 0: Component {0, 1, 2}
    - Start from 3: Component {3, 4, 5}
  - SCCs: {0, 1, 2}, {3, 4, 5}
  
- Component graph:
  - Vertices: C0 = {0, 1, 2}, C1 = {3, 4, 5}
  - Edges: C0→C1 (from edge 2→3)
  - This is a DAG

## Variants / Alternatives
- Iterative implementation: Uses explicit stack instead of recursion
- Tarjan's algorithm: Single-pass algorithm for finding SCCs
- Gabow's algorithm: Similar to Tarjan's but uses two stacks
- Path-based algorithm: Alternative approach using path information
- Alternatives:
  - Tarjan's algorithm: Often preferred in practice
  - Incremental SCC algorithms: For dynamic graphs
  - Union-find based approaches: For specific applications
  - BFS-based approaches: For specific graph structures

## References
- Kosaraju, S. Rao (unpublished, discovered independently by several researchers)
- Sharir, M.: "A strong-connectivity algorithm and its applications in data flow analysis" (1981)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 22.5
- Sedgewick, Wayne: "Algorithms, 4th Edition", Section 4.2
- Skiena, Steven: "The Algorithm Design Manual", Chapter 5 