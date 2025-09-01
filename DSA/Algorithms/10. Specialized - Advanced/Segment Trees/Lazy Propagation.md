# Segment Trees with Lazy Propagation

## Name
- Category: Data Structures, Range Queries
- Summary: Extension of segment trees that efficiently handles range updates by deferring the propagation of updates to children nodes until necessary, achieving logarithmic time complexity for both range queries and range updates.

## Preconditions / Assumptions
- Input is an array of elements
- Operations include range queries (e.g., sum, min, max) and range updates
- Update operations can be applied lazily (deferred until needed)
- Elements can be of any type that supports the required operations
- Array indices are 0-based (or can be adapted to 1-based)
- Memory usage is not a critical constraint

## Properties
- Deterministic
- Complete (handles all valid range queries and updates)
- Query time: O(log n)
- Update time: O(log n) for range updates
- Build time: O(n)
- Space complexity: O(n)
- Supports both point and range updates efficiently
- Can be adapted for various associative operations
- Lazy evaluation strategy for propagating updates

## Complexity (per variant)
| Variant | Build Time | Query Time | Update Time | Space | Notes |
|---|---|---|---|---|---|
| Standard Lazy | O(n) | O(log n) | O(log n) | O(n) | Basic implementation |
| Iterative Lazy | O(n) | O(log n) | O(log n) | O(n) | Non-recursive implementation |
| Persistent Lazy | O(n) | O(log n) | O(log n) | O(n log n) | With version history |
| 2D Lazy | O(nm) | O(log n * log m) | O(log n * log m) | O(nm) | For 2D range queries |
| Implicit Lazy | O(q log n) | O(log n) | O(log n) | O(q log n) | Dynamic allocation, q = queries |

## When to Use / When to Avoid
- Use if:
  - Need efficient range updates and queries
  - Operations are associative and can be applied lazily
  - Logarithmic time complexity is acceptable
  - Flexibility is important (can adapt to different operations)
  - Memory usage is not a critical constraint
- Avoid if:
  - Only need point updates (standard segment tree may be simpler)
  - Only need range queries on a static array (consider sparse table)
  - Memory is severely constrained
  - Operations cannot be applied lazily
  - Update operations are not associative or commutative

## Correctness (Proof Sketch)
- The lazy segment tree maintains two invariants:
  1. Each node correctly represents its range (after applying its lazy value)
  2. Lazy values are properly propagated before accessing children
- For range updates:
  - When a range completely covers a node's range, we update the node's value and store the update in its lazy field
  - We don't immediately update its children (lazy evaluation)
- For range queries:
  - Before accessing a node, we ensure its lazy value is applied to itself
  - Before accessing a node's children, we propagate its lazy value to them
- The correctness follows from maintaining these invariants
- The time complexity remains O(log n) because:
  - We still visit O(log n) nodes per query/update
  - Each propagation operation is O(1)
  - Lazy propagation ensures each node is updated at most once

## Pseudocode (canonical)
```pseudo
function buildSegmentTree(array):
    n = array.length
    tree = new array of size 4*n
    lazy = new array of size 4*n, initialized with 0
    build(array, tree, 1, 0, n-1)
    return tree, lazy

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

# Propagate lazy updates to children
function propagate(tree, lazy, node, start, end):
    if lazy[node] != 0:
        # Apply lazy value to current node
        tree[node] += (end - start + 1) * lazy[node]
        
        if start != end:
            # Propagate to children
            lazy[2*node] += lazy[node]
            lazy[2*node+1] += lazy[node]
        
        # Reset lazy value
        lazy[node] = 0

function updateRange(tree, lazy, node, start, end, left, right, value):
    # First propagate lazy updates
    propagate(tree, lazy, node, start, end)
    
    if left > end or right < start:
        # Range completely outside the update range
        return
    
    if left <= start and end <= right:
        # Range completely inside the update range
        tree[node] += (end - start + 1) * value
        
        if start != end:
            # Mark children for lazy update
            lazy[2*node] += value
            lazy[2*node+1] += value
        
        return
    
    # Range partially overlaps with the update range
    mid = (start + end) / 2
    updateRange(tree, lazy, 2*node, start, mid, left, right, value)
    updateRange(tree, lazy, 2*node+1, mid+1, end, left, right, value)
    
    # Update current node
    tree[node] = tree[2*node] + tree[2*node+1]

function queryRange(tree, lazy, node, start, end, left, right):
    if left > end or right < start:
        # Range completely outside the query range
        return 0
    
    # First propagate lazy updates
    propagate(tree, lazy, node, start, end)
    
    if left <= start and end <= right:
        # Range completely inside the query range
        return tree[node]
    
    # Range partially overlaps with the query range
    mid = (start + end) / 2
    left_sum = queryRange(tree, lazy, 2*node, start, mid, left, right)
    right_sum = queryRange(tree, lazy, 2*node+1, mid+1, end, left, right)
    
    return left_sum + right_sum

# Example: Range assignment update (set all elements in range to a value)
function updateRangeAssignment(tree, lazy, node, start, end, left, right, value):
    # Clear previous lazy updates when doing assignment
    propagate(tree, lazy, node, start, end)
    
    if left > end or right < start:
        return
    
    if left <= start and end <= right:
        # Set the value and mark for lazy propagation
        tree[node] = (end - start + 1) * value
        
        if start != end:
            # For assignment, we overwrite previous lazy values
            lazy[2*node] = value
            lazy[2*node+1] = value
        
        return
    
    mid = (start + end) / 2
    updateRangeAssignment(tree, lazy, 2*node, start, mid, left, right, value)
    updateRangeAssignment(tree, lazy, 2*node+1, mid+1, end, left, right, value)
    
    tree[node] = tree[2*node] + tree[2*node+1]
```

## Implementation Notes
- Use 1-based indexing for the tree array to simplify child node calculation
- For 0-based array input, adjust the indices accordingly
- Consider using an iterative implementation to avoid stack overflow
- Use appropriate data types to avoid overflow
- For multiple types of updates, use a more complex lazy value structure
- Consider using a class/struct to encapsulate the segment tree and lazy array
- For multiple types of queries, store additional information at each node
- Be careful with the semantics of lazy updates (e.g., assignment vs. increment)
- Ensure proper propagation before accessing any node
- For range updates with different operations, define clear semantics for combining operations

## Edge-case Checklist
- Empty array: Return appropriate default value
- Single element array: Tree degenerates to a single node
- Query/update with invalid range: Handle appropriately
- Query/update outside array bounds: Return error or default value
- Multiple range updates before query: Ensure proper lazy propagation
- Overlapping range updates: Define clear semantics for combining updates
- Large array values: Watch for overflow in calculations
- Power-of-two sized arrays: No special handling needed
- Updates that don't change values: Can optimize by checking first

## Examples
- Range Sum Query with Range Addition:
  - Array: [1, 3, 5, 7, 9, 11]
  - Operations:
    - Add 2 to range [1, 3]: [1, 5, 7, 9, 9, 11]
    - Query sum of range [0, 4]: 1 + 5 + 7 + 9 + 9 = 31
    - Add 3 to range [2, 5]: [1, 5, 10, 12, 12, 14]
    - Query sum of range [1, 4]: 5 + 10 + 12 + 12 = 39
  
- Range Minimum Query with Range Assignment:
  - Array: [5, 8, 6, 3, 2, 7]
  - Operations:
    - Set range [1, 3] to 4: [5, 4, 4, 4, 2, 7]
    - Query minimum of range [0, 4]: min(5, 4, 4, 4, 2) = 2
    - Set range [3, 5] to 1: [5, 4, 4, 1, 1, 1]
    - Query minimum of range [2, 5]: min(4, 1, 1, 1) = 1

## Variants / Alternatives
- Different update operations:
  - Range addition: Add a value to all elements in a range
  - Range assignment: Set all elements in a range to a value
  - Range multiplication: Multiply all elements in a range by a value
- Different query operations:
  - Range sum: Sum of all elements in a range
  - Range minimum/maximum: Minimum/maximum element in a range
  - Range GCD/LCM: GCD/LCM of all elements in a range
- Alternatives:
  - Fenwick Tree (Binary Indexed Tree): For simpler range sum queries
  - Square Root Decomposition: Simpler but less efficient
  - Sparse Table: For static arrays with fast queries
  - Persistent Segment Tree: For maintaining version history

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Competitive Programming resources (e.g., CP-Algorithms)
- CSES Problem Set: "Range Queries" section
- Sedgewick, Robert and Wayne, Kevin: "Algorithms, 4th Edition"
- Edelkamp, Stefan and Elmasry, Amr: "Data Structures and Algorithms with Python"
