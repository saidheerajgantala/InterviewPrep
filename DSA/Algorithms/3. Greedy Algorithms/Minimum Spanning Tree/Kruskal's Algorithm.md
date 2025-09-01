# Kruskal's Algorithm

## Name
- Category: Graph, Greedy
- Summary: Finds a minimum spanning tree by adding edges in order of increasing weight, avoiding cycles.

## Preconditions / Assumptions
- Undirected connected graph
- Edge weights can be compared (typically numeric)
- No self-loops or parallel edges (can be handled with modifications)
- Graph is represented as a set of edges with weights

## Properties
- Deterministic
- Optimal (always finds minimum spanning tree)
- Greedy (always selects the globally minimum weight edge that doesn't create a cycle)
- Results in a spanning tree with minimum total weight

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(E log E) | O(E log E) | O(E log E) | O(V + E) | Dominated by sorting edges |
| With optimized DSU | O(E log E) | O(E log E) | O(E log E) | O(V) | Using path compression and union by rank |

## When to Use / When to Avoid
- Use if:
  - Need minimum spanning tree of a graph
  - Graph is sparse (E ≈ V)
  - Edge list representation is available
- Avoid if:
  - Graph is dense (Prim's algorithm may be better)
  - Need to grow MST from specific vertex (use Prim's)
  - Graph is disconnected (will need to find MST for each component)

## Correctness (Proof Sketch)
- Based on the cut property: For any partition of vertices, the minimum weight edge crossing the partition must be in the MST
- Greedy choice is always safe: Adding the minimum weight edge that doesn't create a cycle cannot lead to a suboptimal solution
- Invariant: At each step, the selected edges form a forest that is part of some MST
- Termination: Algorithm terminates after selecting V-1 edges, forming a spanning tree

## Pseudocode (canonical)
```pseudo
function kruskal(graph):
    # Initialize empty MST
    mst = empty set
    
    # Sort all edges by weight (ascending)
    edges = sort_edges_by_weight(graph.edges)
    
    # Initialize disjoint set for each vertex
    dsu = DisjointSetUnion(graph.vertices)
    
    # Process edges in order of increasing weight
    for each edge (u, v, weight) in edges:
        # If adding edge doesn't create cycle
        if dsu.find(u) != dsu.find(v):
            # Add edge to MST
            mst.add(edge)
            # Merge components
            dsu.union(u, v)
            
            # Early termination check (optional)
            if mst.size == graph.vertices.size - 1:
                break
    
    return mst
```

## Implementation Notes
- Efficient implementation of Disjoint-Set Union (DSU) is crucial:
  - Use path compression and union by rank for near-constant time operations
- Sorting dominates the time complexity
- Can terminate early after finding V-1 edges
- For dense graphs, consider using Prim's algorithm instead
- Can be implemented with priority queue instead of sorting all edges upfront

## Edge-case Checklist
- Empty graph: Return empty MST
- Single vertex: Return empty MST (no edges needed)
- Disconnected graph: Will find minimum spanning forest (MST for each component)
- Multiple edges with same weight: Tie-breaking doesn't affect optimality
- Equal-weight MSTs: Will find one of the possible MSTs
- Negative weights: Algorithm works correctly with negative weights

## Examples
- Minimal example:
  - Graph: A--(1)--B--(2)--C, A--(3)--C
  - Output MST: A--(1)--B--(2)--C with total weight 3
  
- Tricky example:
  - Graph with multiple equal-weight MSTs:
  - A--(1)--B--(1)--C, A--(1)--C
  - Kruskal's will select any two edges with weight 1

## Variants / Alternatives
- Reverse-Delete Algorithm: Dual of Kruskal's, removes edges in descending weight order
- Borůvka's Algorithm: Another MST algorithm, grows multiple trees simultaneously
- Prim's Algorithm: Alternative MST algorithm, grows single tree from a starting vertex
- Maximum Spanning Tree: Modify to sort edges in descending order
- Minimum Bottleneck Spanning Tree: Minimize maximum edge weight (can be found using Kruskal's)

## References
- Kruskal, J.B. "On the Shortest Spanning Subtree of a Graph and the Traveling Salesman Problem" (1956)
- Cormen, Leiserson, Rivest, Stein: Introduction to Algorithms, Chapter 23.2
- Sedgewick, Wayne: Algorithms, 4th Edition