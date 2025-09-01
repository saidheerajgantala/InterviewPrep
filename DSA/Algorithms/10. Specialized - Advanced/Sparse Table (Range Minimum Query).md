# Sparse Table (Range Minimum Query)

## Name
- Category: Data Structures, Range Queries
- Summary: Preprocessed data structure that enables efficient range queries (especially minimum/maximum) on static arrays, with O(n log n) preprocessing time and O(1) query time for idempotent operations.

## Preconditions / Assumptions
- Input is a static array (not changing after preprocessing)
- Query operation is idempotent (applying it multiple times gives same result)
- Common operations: minimum, maximum, GCD, bitwise AND, bitwise OR
- Queries are range-based (left and right endpoints)
- Memory usage is not a critical constraint

## Properties
- Deterministic
- Complete (handles all valid range queries)
- Preprocessing: O(n log n) time
- Query: O(1) time for idempotent operations, O(log n) otherwise
- Space complexity: O(n log n)
- Immutable (doesn't support updates)
- Based on precomputing results for power-of-two sized ranges
- Particularly efficient for Range Minimum Query (RMQ)

## Complexity (per variant)
| Variant | Preprocessing Time | Space | Query Time | Notes |
|---|---|---|---|---|
| Standard | O(n log n) | O(n log n) | O(1) | For idempotent operations |
| Non-idempotent | O(n log n) | O(n log n) | O(log n) | For sum, product, etc. |
| 2D Sparse Table | O(nm log n log m) | O(nm log n log m) | O(1) | For 2D range queries |
| Disjoint Sparse Table | O(n log n) | O(n log n) | O(log n) | For non-idempotent operations |

## When to Use / When to Avoid
- Use if:
  - Need fast range queries on a static array
  - Query operation is idempotent (min, max, gcd, etc.)
  - Preprocessing time is acceptable
  - Memory usage is not a critical constraint
  - Many queries will be performed
- Avoid if:
  - Array is frequently updated
  - Memory is severely constrained
  - Query operation is not idempotent and O(log n) is too slow
  - Only a few queries will be performed
  - Segment trees or other structures are more suitable

## Correctness (Proof Sketch)
- The sparse table precomputes results for ranges of size 2^k
- Any range [L, R] can be covered by two overlapping power-of-two sized ranges
- For idempotent operations, the overlap doesn't affect the result
- The two ranges are: [L, L+2^k-1] and [R-2^k+1, R], where k = log₂(R-L+1)
- These ranges can be looked up directly in the sparse table
- The result is the operation applied to these two precomputed values
- This gives O(1) query time for idempotent operations
- For non-idempotent operations, the range must be divided into non-overlapping power-of-two ranges

## Pseudocode (canonical)
```pseudo
function buildSparseTable(array):
    n = array.length
    log_n = log₂(n) + 1
    
    # Initialize sparse table
    st = new table of size n × log_n
    
    # Fill in values for ranges of size 1
    for i = 0 to n-1:
        st[i][0] = array[i]
    
    # Build the sparse table
    for j = 1 to log_n-1:
        for i = 0 to n - (1 << j):
            st[i][j] = operation(st[i][j-1], st[i + (1 << (j-1))][j-1])
    
    return st

# Query for idempotent operations (min, max, gcd, etc.)
function queryIdempotent(st, left, right):
    length = right - left + 1
    k = log₂(length)  # Largest power of 2 not exceeding length
    
    return operation(st[left][k], st[right - (1 << k) + 1][k])

# Query for non-idempotent operations (sum, product, etc.)
function queryNonIdempotent(st, left, right):
    result = identity_element  # 0 for sum, 1 for product, etc.
    
    for j = log_n-1 down to 0:
        if left + (1 << j) - 1 <= right:
            result = operation(result, st[left][j])
            left += 1 << j
    
    return result

# Range Minimum Query example
function buildRMQ(array):
    n = array.length
    log_n = log₂(n) + 1
    
    # Initialize sparse table
    st = new table of size n × log_n
    
    # Fill in values for ranges of size 1
    for i = 0 to n-1:
        st[i][0] = array[i]
    
    # Build the sparse table
    for j = 1 to log_n-1:
        for i = 0 to n - (1 << j):
            st[i][j] = min(st[i][j-1], st[i + (1 << (j-1))][j-1])
    
    return st

function queryRMQ(st, left, right):
    length = right - left + 1
    k = log₂(length)  # Largest power of 2 not exceeding length
    
    return min(st[left][k], st[right - (1 << k) + 1][k])
```

## Implementation Notes
- Precompute logarithms for efficiency
- Use bit manipulation for power-of-two calculations
- Consider using 1D arrays with appropriate indexing for better cache locality
- For RMQ, also consider the Segment Tree or Fischer-Heun structure
- Be careful with 0-indexed vs. 1-indexed arrays and queries
- For multiple types of queries, build separate sparse tables
- Consider memory layout for cache efficiency
- Use appropriate data types to avoid overflow
- For very large arrays, consider sparse representations
- Implement the operation function efficiently

## Edge-case Checklist
- Empty array: Return appropriate default value
- Single element array: Return the element itself
- Query on single element: Return the element
- Query covering the entire array: Can be optimized
- Invalid range (left > right): Handle appropriately
- Large array values: Ensure data types can handle the range
- Power-of-two sized arrays: No special handling needed
- Large number of queries: Sparse table is well-suited

## Examples
- Range Minimum Query:
  - Array: [4, 2, 3, 7, 1, 5, 3, 8]
  - Sparse Table:
    - st[i][0]: [4, 2, 3, 7, 1, 5, 3, 8]
    - st[i][1]: [2, 2, 3, 1, 1, 3, 3]
    - st[i][2]: [2, 1, 1, 1]
    - st[i][3]: [1]
  - Query RMQ(1, 5):
    - length = 5, k = 2
    - min(st[1][2], st[4][2]) = min(2, 1) = 1
  - Result: 1
  
- Range GCD Query:
  - Array: [12, 18, 24, 30, 36]
  - Query GCD(1, 3): gcd(gcd(18, 24), 30) = 6
  - With sparse table: gcd(st[1][1], st[2][1]) = gcd(6, 6) = 6
  - Result: 6

## Variants / Alternatives
- 2D Sparse Table: For 2D range queries
- Disjoint Sparse Table: For non-idempotent operations
- Fischer-Heun structure: Combines sparse table with block decomposition
- Alternatives:
  - Segment Tree: For dynamic arrays with updates
  - Fenwick Tree: For cumulative operations
  - Range Trees: For multidimensional queries
  - Sqrt Decomposition: Simpler but less efficient
  - Cartesian Tree: For RMQ with O(n) preprocessing

## References
- Bender, Michael A. and Farach-Colton, Martin: "The LCA Problem Revisited" (2000)
- Competitive Programming resources (e.g., CP-Algorithms)
- CSES Problem Set: "Range Queries" section
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Sedgewick, Robert and Wayne, Kevin: "Algorithms, 4th Edition"
