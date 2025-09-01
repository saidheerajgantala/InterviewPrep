# Fenwick Tree (Binary Indexed Tree)

## Name
- Category: Advanced Data Structure
- Summary: Space-efficient tree structure that provides efficient prefix sum queries and point updates in logarithmic time.

## Preconditions / Assumptions
- Array-based input data
- Operations are limited to prefix sums and point updates
- Operations must be invertible (e.g., addition/subtraction)
- 1-indexed array for simplest implementation

## Properties
- Deterministic
- Implicit binary tree structure
- Logarithmic query and update time
- More space-efficient than segment tree
- Each index responsible for a specific range determined by its least significant bit

## Complexity (per variant)
| Variant | Build | Query | Update | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(n log n) | O(log n) | O(log n) | O(n) | n is array size |
| Optimized Build | O(n) | O(log n) | O(log n) | O(n) | Using bottom-up construction |
| 2D BIT | O(n² log² n) | O(log² n) | O(log² n) | O(n²) | For 2D range queries |
| Range Update | O(n) | O(log n) | O(log n) | O(n) | Using two BITs |
| Persistent | O(n) | O(log n) | O(log n) | O(n log n) | Keeps history of changes |

## When to Use / When to Avoid
- Use if:
  - Need prefix sum queries and point updates
  - Memory efficiency is important
  - Simple implementation preferred over segment trees
  - Operations are invertible (like addition/subtraction)
- Avoid if:
  - Need more complex range queries (min, max, GCD)
  - Need range updates (without additional structures)
  - Operations are not invertible
  - Need to query arbitrary ranges frequently

## Correctness (Proof Sketch)
- Structure: Each index i is responsible for elements in range [i - LSB(i) + 1, i]
- Query: Sum elements by iteratively removing LSB to cover entire prefix
- Update: Propagate change to all ranges that include the updated position
- Invariant: BIT[i] stores sum of elements in range [i - LSB(i) + 1, i]
- LSB property: Efficiently navigate tree structure using bit manipulation
- Termination: Both operations have clear termination conditions (index becomes 0 or exceeds array size)

## Pseudocode (canonical)
```pseudo
# LSB (Least Significant Bit)
function LSB(x):
    return x & (-x)

# Build Fenwick Tree
function build(arr):
    n = arr.length
    bit = new array of size n+1, initialized with 0
    
    for i = 1 to n:
        update(bit, i, arr[i-1])
    
    return bit

# Optimized build in O(n)
function build_optimized(arr):
    n = arr.length
    bit = new array of size n+1
    
    # Copy array to BIT
    for i = 1 to n:
        bit[i] = arr[i-1]
    
    # Perform bottom-up construction
    for i = 1 to n:
        parent = i + LSB(i)
        if parent <= n:
            bit[parent] += bit[i]
    
    return bit

# Query prefix sum: sum of elements from 1 to idx
function query(bit, idx):
    sum = 0
    while idx > 0:
        sum += bit[idx]
        idx -= LSB(idx)  # Remove least significant bit
    return sum

# Range query: sum of elements from left to right
function rangeQuery(bit, left, right):
    return query(bit, right) - query(bit, left - 1)

# Update: add val to element at idx
function update(bit, idx, val):
    n = bit.length - 1
    while idx <= n:
        bit[idx] += val
        idx += LSB(idx)  # Add least significant bit
```

## Implementation Notes
- Use 1-indexed arrays for simplest implementation
- For 0-indexed interface, add/subtract 1 when calling functions
- Can be extended to 2D with nested BITs
- For range updates and point queries, use difference array
- For range updates and range queries, use two BITs
- LSB calculation can be optimized with intrinsic functions
- Consider using long integers to avoid overflow in sums

## Edge-case Checklist
- Empty array: Initialize with size 1 (unused)
- Single element: Trivial case
- Large values: Watch for integer overflow in sums
- 0-indexed vs 1-indexed: Be consistent
- Out-of-bounds queries: Handle appropriately
- Update with index 0: Invalid in 1-indexed implementation
- Query with index 0: Should return 0 (empty prefix)

## Examples
- Minimal example:
  - Array: [3, 2, -1, 6, 5, 4]
  - BIT: [0, 3, 5, -1, 10, 5, 9] (1-indexed)
  - Query(4): Returns 10 (sum of first 4 elements: 3+2+(-1)+6)
  - Update(2, 3): Add 3 to position 2
  - BIT after update: [0, 3, 8, -1, 13, 5, 12]
  - Query(4): Now returns 13
  
- Visualization of ranges:
  - BIT[1]: Responsible for element 1
  - BIT[2]: Responsible for elements 1-2
  - BIT[3]: Responsible for element 3
  - BIT[4]: Responsible for elements 1-4
  - BIT[5]: Responsible for element 5
  - BIT[6]: Responsible for elements 5-6
  - BIT[7]: Responsible for element 7
  - BIT[8]: Responsible for elements 1-8

## Variants / Alternatives
- 2D Fenwick Tree: For 2D range queries
- Range Update BIT: Using difference arrays
- Persistent BIT: Maintains history of all versions
- Order statistic BIT: Find kth smallest element
- Alternatives:
  - Segment Tree: More versatile but more complex
  - Prefix Sum Array: For static arrays with range queries
  - Square Root Decomposition: Simpler implementation with O(√n) complexity
  - Sparse Table: For static arrays with range min/max queries

## References
- Fenwick, Peter M. "A New Data Structure for Cumulative Frequency Tables" (1994)
- Competitive Programmer's Handbook by Antti Laaksonen
- CP-Algorithms: "Fenwick Tree" (https://cp-algorithms.com/data_structures/fenwick.html)
- Topcoder Tutorial on Binary Indexed Trees
