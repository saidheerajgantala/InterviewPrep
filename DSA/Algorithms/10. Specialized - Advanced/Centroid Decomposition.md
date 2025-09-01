# Centroid Decomposition

## Name
- Category: Tree Algorithms, Divide and Conquer
- Summary: Hierarchical decomposition technique for trees that recursively partitions a tree by removing centroids, creating a balanced decomposition with logarithmic height, enabling efficient solutions for various tree problems.

## Preconditions / Assumptions
- Input is a tree (connected acyclic graph)
- Tree is represented as an adjacency list
- Nodes are numbered from 0 to n-1 (or 1 to n)
- Tree has at least one node
- The tree is static (not changing during queries)

## Properties
- Deterministic
- Complete (processes the entire tree)
- Creates a centroid tree with logarithmic height
- Divides the tree into balanced partitions
- Enables O(log n) solutions for many tree problems
- Preserves connectivity information between subtrees
- Can be combined with other data structures for complex queries

## Complexity (per variant)
| Variant | Preprocessing Time | Space | Query Time | Notes |
|---|---|---|---|---|
| Basic | O(n log n) | O(n) | O(log n) | n = number of nodes |
| With path compression | O(n log n) | O(n) | O(log n) | Better constant factors |
| With auxiliary data structures | O(n log n) | O(n log n) | O(log n) | For complex queries |
| Dynamic | O(n log² n) | O(n log n) | O(log² n) | Supporting updates |

## When to Use / When to Avoid
- Use if:
  - Need to answer queries on paths in a tree efficiently
  - Problem involves distances between nodes in a tree
  - Need to process subtrees with balanced sizes
  - Tree is too large for naive approaches
  - Problem exhibits divide-and-conquer structure on trees
- Avoid if:
  - Graph is not a tree (contains cycles)
  - Tree is very small (simpler algorithms may suffice)
  - Memory is severely constrained
  - Problem can be solved with simpler tree algorithms
  - Tree structure changes frequently (consider dynamic algorithms)

## Correctness (Proof Sketch)
- A centroid of a tree is a node whose removal results in subtrees of size at most n/2
- Every tree has at least one centroid (can be proven by considering the sizes of subtrees)
- When we remove a centroid, we get multiple smaller trees
- We recursively apply the same process to each subtree
- This creates a centroid tree where:
  - Each node represents a centroid of some subtree in the original tree
  - Parent-child relationships represent the order of centroid removal
  - The height of this tree is O(log n) because each step reduces the tree size by at least half
- The logarithmic height property enables efficient divide-and-conquer algorithms on trees

## Pseudocode (canonical)
```pseudo
function buildCentroidDecomposition(tree):
    n = number of nodes in tree
    centroid_tree = new empty graph with n nodes
    
    # Build the centroid tree
    buildCentroidTree(tree, -1, 0, centroid_tree)
    
    return centroid_tree

function buildCentroidTree(tree, parent_centroid, current_tree_root, centroid_tree):
    # Find the centroid of the current subtree
    centroid = findCentroid(tree, current_tree_root, getSubtreeSize(tree, current_tree_root))
    
    # Connect to parent in centroid tree
    if parent_centroid != -1:
        centroid_tree.addEdge(parent_centroid, centroid)
    
    # Mark the centroid as removed
    removed = tree.removeNode(centroid)
    
    # Process each connected component (subtree) after centroid removal
    for each neighbor in removed.neighbors:
        # neighbor is now the root of a subtree
        buildCentroidTree(tree, centroid, neighbor, centroid_tree)
    
    # Restore the removed centroid for future operations
    tree.restoreNode(centroid, removed)

function findCentroid(tree, root, total_size):
    centroid = root
    is_centroid = true
    visited = new boolean array of size n, initialized with false
    
    function dfs(node, parent):
        subtree_size = 1
        max_subtree_size = 0
        
        for each neighbor of node:
            if neighbor != parent and not visited[neighbor]:
                child_size = dfs(neighbor, node)
                subtree_size += child_size
                max_subtree_size = max(max_subtree_size, child_size)
        
        # Check the size of the parent component
        max_subtree_size = max(max_subtree_size, total_size - subtree_size)
        
        # Update centroid if this node is better
        if max_subtree_size < total_size/2 or 
           (max_subtree_size == total_size/2 and is_centroid):
            centroid = node
            is_centroid = false
        
        return subtree_size
    
    dfs(root, -1)
    return centroid

function getSubtreeSize(tree, root):
    size = 0
    visited = new boolean array of size n, initialized with false
    
    function dfs(node, parent):
        size++
        visited[node] = true
        
        for each neighbor of node:
            if neighbor != parent and not visited[neighbor]:
                dfs(neighbor, node)
    
    dfs(root, -1)
    return size

# Example: Find distance between two nodes efficiently
function preprocessDistances(original_tree, centroid_tree):
    n = number of nodes
    distances = new 2D array of size n×n, initialized with infinity
    
    # Compute distances in original tree
    for i = 0 to n-1:
        computeDistancesFromNode(original_tree, i, distances[i])
    
    return distances

function distanceQuery(u, v, centroid_tree, distances):
    lca = findLCA(centroid_tree, u, v)
    return distances[lca][u] + distances[lca][v]
```

## Implementation Notes
- Use adjacency list representation for efficiency
- Consider using a more efficient LCA algorithm for distance queries
- Instead of physically removing nodes, use a visited array to mark removed centroids
- Cache subtree sizes to avoid redundant calculations
- For distance queries, precompute distances from each centroid to nodes in its subtree
- Consider using a flat array representation for the centroid tree
- Be careful with node indexing (0-indexed vs. 1-indexed)
- For weighted trees, adjust distance calculations accordingly
- Consider using path compression techniques for faster queries
- Implement efficient data structures for specific query types

## Edge-case Checklist
- Single node tree: The node itself is the centroid
- Two-node tree: Either node can be the centroid
- Linear tree (path): Middle node is the centroid
- Star tree: Center node is the centroid
- Balanced binary tree: Root is typically the centroid
- Skewed tree: Still produces a logarithmic height decomposition
- Disconnected graph: Not a valid input (must be a tree)
- Multiple centroids: Choose any one (typically the first found)

## Examples
- Minimal example:
  - Tree: 0—1—2—3—4
  - Centroid: 2
  - After removing 2: Trees {0—1} and {3—4}
  - Centroid of {0—1}: 1
  - Centroid of {3—4}: 3
  - Centroid Tree: 2—1—0 and 2—3—4
  - Height: 2 (logarithmic in the number of nodes)
  
- Complex example:
  - Balanced binary tree with 15 nodes
  - Root is the centroid
  - Removing root creates two subtrees of size 7
  - Each subtree's centroid becomes a child of the root in the centroid tree
  - Process continues recursively
  - Final centroid tree has height 4 (log₂15 ≈ 3.91)

## Variants / Alternatives
- Virtual centroid tree: Avoid physically modifying the original tree
- Weighted centroid decomposition: For trees with node weights
- Dynamic centroid decomposition: Supporting tree modifications
- Centroid decomposition with auxiliary data structures:
  - With segment trees for range queries
  - With Fenwick trees for cumulative data
  - With hash tables for path queries
- Alternatives:
  - Heavy-light decomposition: Different tree decomposition technique
  - Link-cut trees: For dynamic tree operations
  - Euler tour technique: For specific tree queries
  - Tree diameter decomposition: Alternative balanced decomposition

## References
- Demaine, Erik D., et al.: "Centroid Decomposition of Trees and Applications"
- Competitive Programming resources (e.g., CP-Algorithms)
- CSES Problem Set: "Tree Algorithms" section
- Tarjan, Robert E.: "Data Structures and Network Algorithms"
- Sedgewick, Robert and Wayne, Kevin: "Algorithms, 4th Edition"
