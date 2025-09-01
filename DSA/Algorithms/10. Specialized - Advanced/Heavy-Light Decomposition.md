# Heavy-Light Decomposition

## Name
- Category: Tree Algorithms, Data Structures
- Summary: Technique that decomposes a tree into a collection of disjoint paths (heavy paths) to efficiently perform operations on paths between nodes, with applications in path queries, updates, and lowest common ancestor problems.

## Preconditions / Assumptions
- Input is a tree (connected acyclic graph)
- Tree is represented as an adjacency list
- Nodes are numbered from 0 to n-1 (or 1 to n)
- Tree is rooted (or can be rooted at any node)
- The tree structure is static (not changing during queries)

## Properties
- Deterministic
- Complete (covers the entire tree)
- Decomposes tree into O(log n) heavy paths
- Any root-to-leaf path intersects O(log n) heavy paths
- Enables O(log² n) or O(log n) operations on tree paths
- Can be combined with various data structures for different query types
- Preserves the tree structure while optimizing path operations

## Complexity (per variant)
| Variant | Preprocessing Time | Space | Query Time | Notes |
|---|---|---|---|---|
| Standard | O(n) | O(n) | O(log² n) | With segment trees |
| With lazy propagation | O(n) | O(n) | O(log² n) | For range updates |
| With path compression | O(n) | O(n) | O(log n) | For specific operations |
| With link-cut trees | O(n) | O(n) | O(log n) | For dynamic trees |

## When to Use / When to Avoid
- Use if:
  - Need to perform operations on paths in a tree
  - Operations include range queries or updates
  - Tree is large and naive approaches are too slow
  - Problem involves path aggregation (sum, min, max, etc.)
  - Need to handle LCA queries efficiently
- Avoid if:
  - Graph is not a tree (contains cycles)
  - Tree is very small (simpler algorithms may suffice)
  - Only need to perform simple operations (e.g., single-node queries)
  - Memory is severely constrained
  - Tree structure changes frequently (consider link-cut trees)

## Correctness (Proof Sketch)
- A heavy edge connects a node to its child with the largest subtree
- A light edge is any edge that is not heavy
- Each node has at most one heavy edge to a child
- Heavy edges form disjoint paths (heavy paths)
- Key property: For any node, the number of light edges on the path to the root is O(log n)
  - Proof: Each light edge at least doubles the subtree size as we go up
  - Since tree has n nodes, there can be at most log₂(n) light edges
- This bounds the number of heavy paths intersected by any root-to-node path to O(log n)
- Path operations can be decomposed into operations on O(log n) heavy paths
- Each heavy path can be processed efficiently using array-based data structures

## Pseudocode (canonical)
```pseudo
function buildHLD(tree, root):
    n = number of nodes in tree
    
    # Arrays to store HLD information
    size = new array of size n  # Subtree sizes
    depth = new array of size n  # Depth in the tree
    parent = new array of size n  # Parent in the tree
    heavy = new array of size n, initialized with -1  # Heavy child
    head = new array of size n  # Head of heavy path
    pos = new array of size n  # Position in segment tree array
    
    # First DFS: Calculate subtree sizes and identify heavy edges
    dfsSize(tree, root, -1, size, depth, parent)
    
    # Second DFS: Build heavy paths
    current_pos = 0
    dfsHeavyPath(tree, root, -1, heavy, head, pos, current_pos)
    
    return size, depth, parent, heavy, head, pos

function dfsSize(tree, node, parent_node, size, depth, parent):
    size[node] = 1
    parent[node] = parent_node
    
    if parent_node != -1:
        depth[node] = depth[parent_node] + 1
    else:
        depth[node] = 0
    
    for each child in tree[node]:
        if child != parent_node:
            dfsSize(tree, child, node, size, depth, parent)
            size[node] += size[child]

function dfsHeavyPath(tree, node, parent_node, heavy, head, pos, current_pos):
    # Find the heavy child
    heavy_child = -1
    max_size = -1
    
    for each child in tree[node]:
        if child != parent_node and size[child] > max_size:
            heavy_child = child
            max_size = size[child]
    
    heavy[node] = heavy_child
    
    # Assign position in array
    pos[node] = current_pos
    current_pos++
    
    # Process heavy edge first to keep heavy path contiguous
    if heavy_child != -1:
        head[heavy_child] = head[node]  # Same heavy path
        dfsHeavyPath(tree, heavy_child, node, heavy, head, pos, current_pos)
    
    # Process light edges
    for each child in tree[node]:
        if child != parent_node and child != heavy_child:
            head[child] = child  # Start of a new heavy path
            dfsHeavyPath(tree, child, node, heavy, head, pos, current_pos)

# Query on path between two nodes
function pathQuery(u, v, tree, depth, parent, head, pos, segment_tree):
    result = identity_element  # Depends on the operation (e.g., 0 for sum, INF for min)
    
    # Bring u and v to the same depth
    while head[u] != head[v]:
        # Ensure u is deeper in the tree
        if depth[head[u]] < depth[head[v]]:
            swap(u, v)
        
        # Query on the current heavy path of u
        result = combine(result, querySegmentTree(segment_tree, pos[head[u]], pos[u]))
        
        # Move up to the parent of the head
        u = parent[head[u]]
    
    # Now u and v are on the same heavy path
    # Ensure u is not deeper than v
    if depth[u] > depth[v]:
        swap(u, v)
    
    # Query the final segment
    result = combine(result, querySegmentTree(segment_tree, pos[u], pos[v]))
    
    return result

# Update value on path between two nodes
function pathUpdate(u, v, value, tree, depth, parent, head, pos, segment_tree):
    while head[u] != head[v]:
        # Ensure u is deeper in the tree
        if depth[head[u]] < depth[head[v]]:
            swap(u, v)
        
        # Update on the current heavy path of u
        updateSegmentTree(segment_tree, pos[head[u]], pos[u], value)
        
        # Move up to the parent of the head
        u = parent[head[u]]
    
    # Now u and v are on the same heavy path
    # Ensure u is not deeper than v
    if depth[u] > depth[v]:
        swap(u, v)
    
    # Update the final segment
    updateSegmentTree(segment_tree, pos[u], pos[v], value)
```

## Implementation Notes
- Use adjacency list representation for efficiency
- Implement a segment tree or other range query data structure for each heavy path
- Consider using a single segment tree for the entire tree with appropriate mapping
- Be careful with node indexing (0-indexed vs. 1-indexed)
- For path queries, handle the LCA correctly
- Consider using binary lifting for faster LCA computation
- Cache intermediate results to avoid redundant calculations
- For weighted trees, adjust path operations accordingly
- Consider using lazy propagation for range updates
- Implement efficient data structures for specific query types

## Edge-case Checklist
- Single node tree: Trivial case, handle directly
- Linear tree (path): Single heavy path
- Star tree: Multiple heavy paths (one per edge)
- Balanced binary tree: O(log n) heavy paths
- Skewed tree: Still O(log n) heavy paths
- Root selection: Different roots may lead to different decompositions
- Path queries between the same node: Return identity element
- Disconnected graph: Not a valid input (must be a tree)

## Examples
- Minimal example:
  - Tree: 0—1—2—3—4 (linear path)
  - Root: 0
  - Subtree sizes: [5, 4, 3, 2, 1]
  - Heavy edges: (0,1), (1,2), (2,3), (3,4)
  - Heavy paths: [0,1,2,3,4] (single heavy path)
  - Query path(0,4): Single segment tree query
  
- Complex example:
  - Balanced binary tree with 7 nodes
  - Root: 0, Children: 1, 2
  - Subtree sizes: [7, 3, 3, 1, 1, 1, 1]
  - Heavy edges: (0,1), (0,2), (1,3), (1,4), (2,5), (2,6)
  - Heavy paths: [0,1,3], [0,2,5], [4], [6]
  - Query path(3,6): Decomposed into queries on paths [3,1,0] and [0,2,6]

## Variants / Alternatives
- Path compression techniques for faster operations
- Combined with link-cut trees for dynamic operations
- With various segment tree implementations:
  - Lazy propagation for range updates
  - Persistent segment trees for versioning
  - Sparse segment trees for large ranges
- Alternatives:
  - Centroid decomposition: Different tree decomposition approach
  - Link-cut trees: For dynamic tree operations
  - Euler tour technique: For subtree queries
  - Binary lifting: For LCA and simple path queries

## References
- Sleator, Daniel D. and Tarjan, Robert E.: "A Data Structure for Dynamic Trees" (1983)
- Tarjan, Robert E.: "Data Structures and Network Algorithms"
- Competitive Programming resources (e.g., CP-Algorithms)
- CSES Problem Set: "Tree Algorithms" section
- Sedgewick, Robert and Wayne, Kevin: "Algorithms, 4th Edition"
