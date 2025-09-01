# Mo's Algorithm (Query Square Root Decomposition)

## Name
- Category: Query Processing, Array Algorithms
- Summary: Offline algorithm for efficiently processing range queries by reordering them based on their left and right endpoints, minimizing the cost of transitioning between consecutive query ranges.

## Preconditions / Assumptions
- Queries are known beforehand (offline algorithm)
- Array or sequence of elements to query
- Queries are range-based (left and right endpoints)
- The state of a query can be efficiently updated incrementally
- Adding or removing a single element from the current range is efficient

## Properties
- Deterministic
- Complete (processes all queries)
- Offline (all queries must be known in advance)
- Optimal for certain types of range queries
- Exploits locality of consecutive queries after sorting
- Amortizes the cost of processing similar ranges
- Works well with queries that can be updated incrementally

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O((n+q) * sqrt(n)) | O(n+q) | n = array size, q = number of queries |
| With Hilbert order | O((n+q) * sqrt(n)) | O(n+q) | Better constant factors |
| With block size optimization | O((n+q) * sqrt(n)) | O(n+q) | Tuned for specific inputs |
| With updates | O((n+q) * sqrt(n) * log n) | O(n+q) | For array updates between queries |

## When to Use / When to Avoid
- Use if:
  - Processing multiple range queries offline
  - Query result can be updated incrementally when adding/removing elements
  - Need better than naive O(n*q) solution
  - Queries involve complex operations on ranges
  - Memory usage is not a critical constraint
- Avoid if:
  - Queries must be processed online (as they arrive)
  - Cannot efficiently update query state incrementally
  - Segment trees or other data structures are more suitable
  - Query ranges are very large
  - Memory is severely constrained

## Correctness (Proof Sketch)
- Divide the array into blocks of size sqrt(n)
- Sort queries by block of left endpoint, then by right endpoint
- This ordering minimizes the total number of operations needed
- For consecutive queries, at most O(sqrt(n)) elements change
- When left endpoints are in the same block, right endpoints are sorted
- When left endpoints move to a new block, at most O(n) operations are needed
- There are O(sqrt(n)) blocks, so total cost is O((n+q) * sqrt(n))
- Each query is processed exactly once, ensuring completeness

## Pseudocode (canonical)
```pseudo
function moAlgorithm(array, queries):
    n = array.length
    block_size = sqrt(n)
    
    # Preprocess queries by adding their original indices
    for i = 0 to queries.length - 1:
        queries[i].index = i
    
    # Sort queries by block of left endpoint, then by right endpoint
    sort queries by (queries[i].left / block_size, queries[i].right)
    
    # Initialize result array
    results = new array of size queries.length
    
    # Initialize current range and state
    current_left = 0
    current_right = -1
    current_state = initial_state
    
    # Process queries in order
    for each query in queries:
        # Move the left pointer
        while current_left < query.left:
            remove_element(array[current_left], current_state)
            current_left++
        
        while current_left > query.left:
            current_left--
            add_element(array[current_left], current_state)
        
        # Move the right pointer
        while current_right < query.right:
            current_right++
            add_element(array[current_right], current_state)
        
        while current_right > query.right:
            remove_element(array[current_right], current_state)
            current_right--
        
        # Store the result for this query
        results[query.index] = get_result(current_state)
    
    return results

# Example: Count distinct elements in range
function countDistinct(array, queries):
    n = array.length
    block_size = sqrt(n)
    
    # Preprocess queries
    for i = 0 to queries.length - 1:
        queries[i].index = i
    
    # Sort queries
    sort queries by (queries[i].left / block_size, queries[i].right)
    
    # Initialize result array
    results = new array of size queries.length
    
    # Initialize current range and state
    current_left = 0
    current_right = -1
    frequency = new array of size max_element + 1, initialized with 0
    distinct_count = 0
    
    # Process queries in order
    for each query in queries:
        # Move pointers and update state
        while current_left < query.left:
            frequency[array[current_left]]--
            if frequency[array[current_left]] == 0:
                distinct_count--
            current_left++
        
        while current_left > query.left:
            current_left--
            if frequency[array[current_left]] == 0:
                distinct_count++
            frequency[array[current_left]]++
        
        while current_right < query.right:
            current_right++
            if frequency[array[current_right]] == 0:
                distinct_count++
            frequency[array[current_right]]++
        
        while current_right > query.right:
            frequency[array[current_right]]--
            if frequency[array[current_right]] == 0:
                distinct_count--
            current_right--
        
        # Store the result
        results[query.index] = distinct_count
    
    return results
```

## Implementation Notes
- Choose block size carefully (sqrt(n) is theoretical, but tuning may help)
- Implement efficient add_element and remove_element functions
- Consider using a Hilbert curve ordering for better cache locality
- Use appropriate data structures for the specific query type
- For frequency-based queries, use arrays or hash maps to track counts
- Be careful with 0-indexed vs. 1-indexed arrays and queries
- Consider preprocessing the array if needed
- Optimize the sorting step for large numbers of queries
- Use appropriate data types to avoid overflow
- Consider memory layout for cache efficiency

## Edge-case Checklist
- Empty array: Handle appropriately
- Single element array: Trivial case
- No queries: Return empty result
- Queries with equal endpoints: Handle as single-element ranges
- Queries covering the entire array: May have optimized handling
- Duplicate queries: Process normally or optimize by caching
- Large array values: Ensure data structures can handle the range
- Large number of queries: Consider memory usage

## Examples
- Distinct elements in range:
  - Array: [1, 2, 1, 3, 2, 4, 5, 2]
  - Queries: [(0, 3), (2, 5), (1, 7)]
  - Sorted queries: [(0, 3), (2, 5), (1, 7)] (assuming block size = 2)
  - Processing:
    - Query (0, 3): Range [0, 3], distinct = {1, 2, 3} = 3
    - Query (2, 5): Update range to [2, 5], distinct = {1, 3, 2, 4} = 4
    - Query (1, 7): Update range to [1, 7], distinct = {2, 1, 3, 4, 5} = 5
  - Results: [3, 4, 5]
  
- Sum of range:
  - Array: [1, 3, 5, 2, 4, 6]
  - Queries: [(0, 2), (1, 4), (3, 5)]
  - Processing follows similar pattern
  - Results: [9, 14, 12]

## Variants / Alternatives
- Hilbert order: Better cache locality by using space-filling curves
- Block size optimization: Tuning block size for specific input patterns
- With updates: Handling array updates between queries
- 2D Mo's algorithm: For 2D range queries
- Alternatives:
  - Segment tree: For online queries and updates
  - Fenwick tree: For cumulative queries
  - Sparse table: For immutable range queries
  - Square root decomposition: For simpler range queries
  - Persistent data structures: For version-based queries

## References
- Mo, Somnath: Original author (unpublished)
- Competitive Programming resources (e.g., CP-Algorithms)
- CSES Problem Set: "Range Queries" section
- Codeforces: Various tutorials and problem discussions
- "Advanced Data Structures" by Peter Brass 