# Segment Trees (Range Sum Queries)

## Name
- Category: Data Structures, Range Queries
- Summary: Versatile tree-based data structure that enables efficient range queries and updates on arrays, particularly for range sum queries, with logarithmic time complexity for both operations.

## Preconditions / Assumptions
- Input is an array of elements
- Operations include range queries (e.g., sum, min, max) and point/range updates
- Elements can be of any type that supports the required operations
- Array indices are 0-based (or can be adapted to 1-based)
- Memory usage is not a critical constraint

## Properties
- Deterministic
- Complete (handles all valid range queries and updates)
- Query time: O(log n)
- Update time: O(log n)
- Build time: O(n)
- Space complexity: O(n)
- Supports both point and range updates
- Can be adapted for various associative operations (sum, min, max, GCD, etc.)
- Binary tree structure with efficient recursive implementation

## Complexity (per variant)
| Variant | Build Time | Query Time | Update Time | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(n) | O(log n) | O(log n) | O(n) | Basic implementation |
| Lazy Propagation | O(n) | O(log n) | O(log n) | O(n) | For range updates |
| Persistent | O(n) | O(log n) | O(log n) | O(n log n) | Preserves history |
| Iterative | O(n) | O(log n) | O(log n) | O(n) | Non-recursive implementation |
| 2D Segment Tree | O(nm) | O(log n * log m) | O(log n * log m) | O(nm) | For 2D range queries |

## When to Use / When to Avoid
- Use if:
  - Need both range queries and updates
  - Operations are associative (can be combined)
  - Logarithmic time complexity is acceptable
  - Flexibility is important (can adapt to different operations)
  - Memory usage is not a critical constraint
- Avoid if:
  - Only need range queries on a static array (consider sparse table)
  - Only need cumulative sum queries (consider prefix sum array)
  - Memory is severely constrained (consider Fenwick tree)
  - Operations are not associative
  - Need persistence with many versions (consider persistent segment tree)

## Correctness (Proof Sketch)
- The segment tree is a binary tree where:
  - Leaf nodes represent individual elements of the array
  - Internal nodes represent the result of an operation on their children
  - The root represents the result of the operation on the entire array
- For range queries:
  - The algorithm traverses the tree from root to leaves
  - It only visits nodes whose range overlaps with the query range
  - At most O(log n) nodes need to be visited
  - The result is the combination of values from these nodes
- For point updates:
  - The algorithm updates the leaf node corresponding to the element
  - Then updates all ancestor nodes up to the root
  - At most O(log n) nodes need to be updated
- The correctness follows from the tree structure and the associativity of the operation

## Pseudocode (canonical)
```pseudo
function buildSegmentTree(array):
    n = array.length
    tree = new array of size 4*n
    build(array, tree, 1, 0, n-1)
    return tree

function build(array, tree, node, start, end):
    if start == end:
        # Leaf node
        tree[node] = array[start]
    else:
        mid = (start + end) / 2
        
        # Recursively build left and right subtrees
        build(array, tree, 2*node, start, mid)
        build(array, tree, 2*node+1, mid+1, end)
        
        # Internal node stores sum of children
        tree[node] = tree[2*node] + tree[2*node+1]

function querySum(tree, node, start, end, left, right):
    if left > end or right < start:
        # Range completely outside the query range
        return 0
    
    if left <= start and end <= right:
        # Range completely inside the query range
        return tree[node]
    
    # Range partially overlaps with the query range
    mid = (start + end) / 2
    left_sum = querySum(tree, 2*node, start, mid, left, right)
    right_sum = querySum(tree, 2*node+1, mid+1, end, left, right)
    
    return left_sum + right_sum

function updatePoint(tree, node, start, end, idx, val):
    if start == end:
        # Leaf node
        tree[node] = val
    else:
        mid = (start + end) / 2
        
        if start <= idx and idx <= mid:
            # Update in left subtree
            updatePoint(tree, 2*node, start, mid, idx, val)
        else:
            # Update in right subtree
            updatePoint(tree, 2*node+1, mid+1, end, idx, val)
        
        # Update current node
        tree[node] = tree[2*node] + tree[2*node+1]

# Range update with lazy propagation
function updateRange(tree, lazy, node, start, end, left, right, val):
    # Propagate pending lazy updates
    if lazy[node] != 0:
        tree[node] += (end - start + 1) * lazy[node]
        
        if start != end:
            # Propagate to children
            lazy[2*node] += lazy[node]
            lazy[2*node+1] += lazy[node]
        
        lazy[node] = 0
    
    if left > end or right < start:
        # Range completely outside the update range
        return
    
    if left <= start and end <= right:
        # Range completely inside the update range
        tree[node] += (end - start + 1) * val
        
        if start != end:
            # Mark children for lazy update
            lazy[2*node] += val
            lazy[2*node+1] += val
        
        return
    
    # Range partially overlaps with the update range
    mid = (start + end) / 2
    updateRange(tree, lazy, 2*node, start, mid, left, right, val)
    updateRange(tree, lazy, 2*node+1, mid+1, end, left, right, val)
    
    # Update current node
    tree[node] = tree[2*node] + tree[2*node+1]

function queryLazy(tree, lazy, node, start, end, left, right):
    # Propagate pending lazy updates
    if lazy[node] != 0:
        tree[node] += (end - start + 1) * lazy[node]
        
        if start != end:
            # Propagate to children
            lazy[2*node] += lazy[node]
            lazy[2*node+1] += lazy[node]
        
        lazy[node] = 0
    
    if left > end or right < start:
        # Range completely outside the query range
        return 0
    
    if left <= start and end <= right:
        # Range completely inside the query range
        return tree[node]
    
    # Range partially overlaps with the query range
    mid = (start + end) / 2
    left_sum = queryLazy(tree, lazy, 2*node, start, mid, left, right)
    right_sum = queryLazy(tree, lazy, 2*node+1, mid+1, end, left, right)
    
    return left_sum + right_sum
```

## Implementation Notes
- Use 1-based indexing for the tree array to simplify child node calculation
- For 0-based array input, adjust the indices accordingly
- Consider using an iterative implementation to avoid stack overflow
- Use appropriate data types to avoid overflow
- For range updates, implement lazy propagation
- Consider using a class/struct to encapsulate the segment tree
- For multiple types of queries, store additional information at each node
- Optimize memory usage by calculating the exact size needed
- Consider using a more cache-friendly layout
- For large arrays, consider a sparse segment tree implementation

## Edge-case Checklist
- Empty array: Return appropriate default value
- Single element array: Tree degenerates to a single node
- Query/update with invalid range: Handle appropriately
- Query/update outside array bounds: Return error or default value
- Range updates with lazy propagation: Ensure proper propagation
- Large array values: Watch for overflow in sum calculations
- Power-of-two sized arrays: No special handling needed
- Updates that don't change the value: Can optimize by checking first

## Examples
- Range Sum Query:
  - Array: [1, 3, 5, 7, 9, 11]
  - Segment Tree:
    - Node 1 (root): Sum of [0, 5] = 36
    - Node 2: Sum of [0, 2] = 9
    - Node 3: Sum of [3, 5] = 27
    - Node 4: Sum of [0, 1] = 4
    - Node 5: Sum of [2, 2] = 5
    - And so on...
  - Query Sum(1, 4):
    - Overlaps with [0, 2] and [3, 5]
    - Need to go deeper in both subtrees
    - Eventually combines values from nodes representing [1, 1], [2, 2], [3, 3], and [4, 4]
    - Result: 3 + 5 + 7 + 9 = 24
  - Update: Set A[2] = 10
    - Update leaf node for index 2
    - Update ancestors: Node 5, Node 2, Node 1
    - New values: Node 5 = 10, Node 2 = 14, Node 1 = 41

## Variants / Alternatives
- Lazy Propagation: For efficient range updates
- Persistent Segment Tree: Preserves history of updates
- 2D Segment Tree: For 2D range queries
- Iterative Segment Tree: Non-recursive implementation
- Alternatives:
  - Fenwick Tree (Binary Indexed Tree): More memory efficient
  - Sparse Table: For static arrays with fast queries
  - Square Root Decomposition: Simpler but less efficient
  - Wavelet Tree: For more complex range queries
  - Treap or Balanced BST: For dynamic ranges

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Competitive Programming resources (e.g., CP-Algorithms)
- CSES Problem Set: "Range Queries" section
- Sedgewick, Robert and Wayne, Kevin: "Algorithms, 4th Edition"
- Edelkamp, Stefan and Elmasry, Amr: "Data Structures and Algorithms with Python"
