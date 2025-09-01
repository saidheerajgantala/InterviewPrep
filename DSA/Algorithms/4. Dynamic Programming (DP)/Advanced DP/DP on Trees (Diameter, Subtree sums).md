# DP on Trees (Diameter, Subtree sums)

## Name
- Category: Dynamic Programming, Tree Algorithms
- Summary: Algorithms that apply dynamic programming principles to tree structures, solving problems like finding tree diameter, computing subtree sums, and other tree-based optimizations by utilizing the hierarchical nature of trees.

## Preconditions / Assumptions
- Input is a tree (connected acyclic graph)
- Tree can be rooted or unrooted
- Tree can be represented as adjacency list or parent array
- Nodes may have values/weights associated with them
- Edge weights may be uniform or variable (problem-specific)

## Properties
- Deterministic
- Complete (finds optimal solutions)
- Exploits tree structure for efficient computation
- Often uses post-order traversal (bottom-up DP)
- Sometimes uses pre-order traversal (top-down DP)
- Can be combined with tree re-rooting technique for certain problems

## Complexity (per variant)
| Problem | Time | Space | Notes |
|---|---|---|---|
| Tree Diameter | O(n) | O(n) | Two DFS passes or DP |
| Subtree Sum | O(n) | O(n) | Single DFS pass |
| Maximum Path Sum | O(n) | O(n) | Similar to diameter |
| Subtree Count | O(n) | O(n) | Single DFS pass |
| Tree DP with Re-rooting | O(n) | O(n) | Two passes |

## When to Use / When to Avoid
- Use if:
  - Problem involves optimizing over paths or subtrees
  - Need to compute properties for all subtrees
  - Tree structure allows for efficient bottom-up computation
  - Problem exhibits optimal substructure on trees
- Avoid if:
  - Graph is not a tree (contains cycles)
  - Problem doesn't have optimal substructure
  - More efficient specialized algorithms exist
  - Memory constraints are severe

## Correctness (Proof Sketch)
### Tree Diameter
- Principle: The diameter is the longest path between any two nodes
- Two-pass approach:
  1. Start DFS from any node, find farthest node x
  2. Start DFS from x, find farthest node y
  3. Path from x to y is a diameter
- DP approach:
  - For each node, compute height of subtree
  - For each node, diameter passing through it is sum of two longest paths to leaves
  - Overall diameter is maximum of these values

### Subtree Sum
- Principle: Sum of a subtree = node value + sum of all child subtrees
- Base case: Leaf nodes have subtree sum equal to their value
- Inductive step: For internal nodes, add value to sum of all child subtrees
- Post-order traversal ensures children are processed before parents

## Pseudocode (canonical)
```pseudo
# Tree Diameter using two DFS passes
function treeDiameter(tree):
    # First DFS to find farthest node
    farthest_node, max_dist = dfs(tree, 0, -1)
    
    # Second DFS from the farthest node
    _, diameter = dfs(tree, farthest_node, -1)
    
    return diameter

function dfs(tree, node, parent):
    max_dist = 0
    farthest_node = node
    
    for each neighbor of node:
        if neighbor != parent:
            child_farthest, child_dist = dfs(tree, neighbor, node)
            child_dist += 1  # Add edge weight (1 in this case)
            
            if child_dist > max_dist:
                max_dist = child_dist
                farthest_node = child_farthest
    
    return farthest_node, max_dist

# Tree Diameter using DP
function treeDiameterDP(tree, root):
    diameter = 0
    
    function height(node, parent):
        nonlocal diameter
        
        # Initialize heights of two longest paths
        max_height1 = 0
        max_height2 = 0
        
        for each neighbor of node:
            if neighbor != parent:
                h = height(neighbor, node)
                
                # Update the two longest paths
                if h > max_height1:
                    max_height2 = max_height1
                    max_height1 = h
                elif h > max_height2:
                    max_height2 = h
        
        # Update diameter (longest path passing through this node)
        diameter = max(diameter, max_height1 + max_height2)
        
        # Return height of subtree rooted at node
        return max_height1 + 1
    
    height(root, -1)
    return diameter

# Subtree Sum
function subtreeSum(tree, values, root):
    subtree_sums = new array of size n
    
    function dfs(node, parent):
        subtree_sums[node] = values[node]
        
        for each neighbor of node:
            if neighbor != parent:
                dfs(neighbor, node)
                subtree_sums[node] += subtree_sums[neighbor]
    
    dfs(root, -1)
    return subtree_sums

# Tree DP with Re-rooting (e.g., sum of distances to all nodes)
function sumOfDistances(tree, n):
    # Arrays to store results
    count = new array of size n, initialized with 1  # Subtree size
    result = new array of size n, initialized with 0  # Answer for each node
    
    # First pass: Post-order traversal to compute subtree results
    function dfs1(node, parent):
        for each child of node:
            if child != parent:
                dfs1(child, node)
                count[node] += count[child]
                result[node] += result[child] + count[child]
    
    # Second pass: Pre-order traversal to compute final results
    function dfs2(node, parent):
        for each child of node:
            if child != parent:
                # Re-root from node to child
                result[child] = result[node] - count[child] + (n - count[child])
                dfs2(child, node)
    
    dfs1(0, -1)  # Compute for root 0
    dfs2(0, -1)  # Re-root for all nodes
    
    return result
```

## Implementation Notes
- Use adjacency list representation for efficient tree traversal
- For large trees, avoid recursion to prevent stack overflow
- Consider iterative implementations with explicit stack
- Cache results to avoid recomputation in recursive approaches
- For weighted edges, modify distance calculations accordingly
- Tree re-rooting technique is powerful for computing results from different root perspectives
- Be careful with 0-indexed vs. 1-indexed node numbering
- For disconnected graphs, run algorithm on each connected component

## Edge-case Checklist
- Empty tree: Return appropriate default values
- Single node tree: Diameter is 0, subtree sum is node value
- Linear tree (path): Diameter is n-1 for unweighted
- Star tree: Diameter is 2 for unweighted
- Trees with negative weights: May require special handling
- Trees with very large values: Watch for integer overflow
- Deep trees: May cause stack overflow with recursive implementations

## Examples
### Tree Diameter
- Minimal example:
  - Tree: 0—1—2—3—4
  - First DFS from node 0: Farthest node is 4, distance 4
  - Second DFS from node 4: Farthest node is 0, distance 4
  - Diameter: 4
  
- Tree DP approach:
  - For node 2:
    - Longest paths to leaves: 2→3→4 (length 2) and 2→1→0 (length 2)
    - Local diameter: 2 + 2 = 4
  - Overall diameter: 4

### Subtree Sum
- Example with node values [3, 2, 1, 6, 5]:
  - Tree: 0—1—2, 0—3—4
  - Subtree sums:
    - Node 2: 1
    - Node 1: 2 + 1 = 3
    - Node 4: 5
    - Node 3: 6 + 5 = 11
    - Node 0: 3 + 3 + 11 = 17

## Variants / Alternatives
- Maximum Path Sum: Similar to diameter but with node values
- Subtree Count: Count nodes in each subtree
- Tree Coloring DP: Optimal coloring of tree nodes
- Maximum Independent Set on Trees: DP with two states per node
- Tree Isomorphism: Determine if two trees are structurally identical
- Alternatives:
  - Centroid Decomposition: For more complex tree problems
  - Heavy-Light Decomposition: For path queries on trees
  - Link-Cut Trees: For dynamic tree operations

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Kleinberg, Tardos: "Algorithm Design", Chapter 6
- Competitive Programming resources (e.g., CP-Algorithms)
- LeetCode Problem #543: "Diameter of Binary Tree"
- LeetCode Problem #834: "Sum of Distances in Tree"
- CSES Problem Set: "Tree Algorithms" section
