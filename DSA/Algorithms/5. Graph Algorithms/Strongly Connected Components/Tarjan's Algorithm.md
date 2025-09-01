# Tarjan's Algorithm

## Name
- Category: Graph, Strongly Connected Components
- Summary: Single-pass algorithm that finds all strongly connected components (SCCs) in a directed graph using depth-first search with additional bookkeeping to track discovery times and lowlink values.

## Preconditions / Assumptions
- Directed graph
- Graph is represented as an adjacency list
- Need to identify all strongly connected components
- A strongly connected component is a maximal subgraph where every vertex is reachable from every other vertex

## Properties
- Deterministic
- Complete (finds all strongly connected components)
- Linear time complexity
- Single depth-first search pass
- Uses stack to track vertices in current SCC
- More efficient in practice than Kosaraju's algorithm
- Identifies components in reverse topological order

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(V+E) | O(V+E) | V vertices, E edges |
| Iterative | O(V+E) | O(V+E) | Using explicit stack |
| With component graph | O(V+E) | O(V+E) | Builds condensation graph |
| Path-based | O(V+E) | O(V+E) | Alternative implementation |

## When to Use / When to Avoid
- Use if:
  - Need to find strongly connected components in a directed graph
  - Need to detect cycles in a directed graph
  - Need to convert a directed graph to a directed acyclic graph (DAG)
  - Efficiency in practice is important
  - Single-pass algorithm is preferred
- Avoid if:
  - Graph is undirected (use connected components algorithm)
  - Simplicity is more important than efficiency
  - Memory is extremely constrained
  - Implementation complexity is a concern

## Correctness (Proof Sketch)
- Key insight: Vertices in the same SCC have the same set of reachable vertices
- Uses DFS with discovery time (ids) and lowlink values
- Discovery time: Order in which vertices are visited
- Lowlink: Lowest discovery time reachable from the vertex through the DFS tree and at most one back edge
- Vertex is root of an SCC if its lowlink equals its discovery time
- Stack ensures vertices are only included in their correct SCC
- Proof relies on properties of DFS and the definition of lowlink values

## Pseudocode (canonical)
```pseudo
function tarjan(graph):
    n = number of vertices
    
    # Initialize data structures
    ids = new array of size n, initialized with -1
    low_links = new array of size n, initialized with -1
    on_stack = new boolean array of size n, initialized with false
    stack = new empty stack
    
    id = 0  # Counter for discovery times
    components = []  # List to store SCCs
    
    # Process all vertices
    for v = 0 to n-1:
        if ids[v] == -1:  # Not yet visited
            dfs(graph, v, ids, low_links, on_stack, stack, id, components)
    
    return components

function dfs(graph, v, ids, low_links, on_stack, stack, id, components):
    # Set discovery time and lowlink
    ids[v] = id
    low_links[v] = id
    id++
    
    # Add to stack
    stack.push(v)
    on_stack[v] = true
    
    # Visit all neighbors
    for each neighbor u of v in graph:
        if ids[u] == -1:  # Not yet visited
            dfs(graph, u, ids, low_links, on_stack, stack, id, components)
            low_links[v] = min(low_links[v], low_links[u])
        elif on_stack[u]:  # Back edge to vertex in current SCC
            low_links[v] = min(low_links[v], ids[u])
    
    # If v is a root node, pop the stack and create an SCC
    if low_links[v] == ids[v]:
        component = []
        
        while true:
            u = stack.pop()
            on_stack[u] = false
            component.add(u)
            
            if u == v:
                break
        
        components.add(component)

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
- Be careful with the id counter (pass by reference or use a global variable)
- For large graphs, consider iterative implementation to avoid stack overflow
- The stack can be implemented as an array with a pointer
- Consider using a vector of vectors to store components
- For component graph construction, use a set to avoid duplicate edges
- Vertex numbering should be consistent (typically 0 to n-1)
- Can be combined with topological sort of the component graph
- To detect if a graph is strongly connected, check if there's only one component
- Lowlink updates are critical for correctness - ensure both cases are handled

## Edge-case Checklist
- Empty graph: Return empty result
- Single vertex: Forms its own SCC
- Disconnected graph: Each disconnected part has its own SCCs
- Acyclic graph: Each vertex is its own SCC
- Complete graph: The entire graph is one SCC
- Self-loops: Don't affect SCCs
- Parallel edges: Don't affect SCCs (can be treated as single edges)
- Deep recursion: May cause stack overflow in recursive implementation

## Examples
- Minimal example:
  - Graph: 0→1→2→0, 2→3, 3→4, 4→5, 5→3
  - DFS execution:
    - Visit 0: id=0, lowlink=0, stack=[0]
    - Visit 1: id=1, lowlink=1, stack=[0,1]
    - Visit 2: id=2, lowlink=0, stack=[0,1,2]
    - Back edge 2→0 updates lowlink of 2 to 0
    - Visit 3: id=3, lowlink=3, stack=[0,1,2,3]
    - Visit 4: id=4, lowlink=3, stack=[0,1,2,3,4]
    - Visit 5: id=5, lowlink=3, stack=[0,1,2,3,4,5]
    - Back edge 5→3 updates lowlink of 5 to 3
    - Back edge 4→3 updates lowlink of 4 to 3
    - SCC {3,4,5} formed (lowlink[3]=3=id[3])
    - SCC {0,1,2} formed (lowlink[0]=0=id[0])
  - SCCs: {0, 1, 2}, {3, 4, 5}
  
- Component graph:
  - Vertices: C0 = {0, 1, 2}, C1 = {3, 4, 5}
  - Edges: C0→C1 (from edge 2→3)
  - This is a DAG

## Variants / Alternatives
- Iterative implementation: Uses explicit stack instead of recursion
- Path-based SCC algorithm: Alternative approach using path information
- Gabow's algorithm: Similar to Tarjan's but uses two stacks
- Kosaraju's algorithm: Two-pass algorithm for finding SCCs
- Alternatives:
  - Kosaraju's algorithm: Simpler but less efficient in practice
  - Incremental SCC algorithms: For dynamic graphs
  - Union-find based approaches: For specific applications
  - BFS-based approaches: For specific graph structures

## References
- Tarjan, Robert: "Depth-first search and linear graph algorithms" (1972)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 22.5
- Sedgewick, Wayne: "Algorithms, 4th Edition", Section 4.2
- Skiena, Steven: "The Algorithm Design Manual", Chapter 5
- Aho, Hopcroft, and Ullman: "The Design and Analysis of Computer Algorithms" 