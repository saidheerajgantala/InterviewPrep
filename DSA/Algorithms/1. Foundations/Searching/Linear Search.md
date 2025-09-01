# Linear Search

## Name
- Category: Searching Algorithms
- Summary: Simple search algorithm that sequentially checks each element of a collection until the target element is found or the entire collection has been searched, with applications in small datasets and unsorted collections.

## Preconditions / Assumptions
- Input is a collection of elements (array, list, etc.)
- Target element is well-defined and comparable
- No specific ordering of elements is required
- Collection fits in memory
- Elements can be accessed sequentially
- Equality comparison is well-defined for the elements

## Properties
- Deterministic
- Complete (always finds the target if it exists)
- Time complexity: O(n)
- Space complexity: O(1)
- Simple implementation
- No preprocessing required
- Works on any collection that supports sequential access
- Can be easily modified to find all occurrences

## Complexity (per variant)
| Variant | Average | Worst | Best | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(n) | O(n) | O(1) | O(1) | Target at beginning is best case |
| Sentinel | O(n) | O(n) | O(1) | O(1) | Eliminates one comparison per iteration |
| Early return | O(n) | O(n) | O(1) | O(1) | Returns as soon as target is found |
| All occurrences | O(n) | O(n) | O(n) | O(k) | k = number of occurrences |

## When to Use / When to Avoid
- Use if:
  - Collection is small
  - Collection is unsorted
  - Simplicity is more important than efficiency
  - Collection is rarely searched
  - Collection doesn't support random access
  - Need to find all occurrences of the target
- Avoid if:
  - Collection is large and sorted (use binary search)
  - Collection is frequently searched (use hash table or other data structure)
  - Performance is critical
  - Collection supports more efficient search operations

## Correctness (Proof Sketch)
- The algorithm examines each element exactly once
- If the target exists, it will eventually be found
- If the target doesn't exist, all elements will be examined
- No element is skipped, ensuring completeness
- The algorithm terminates after examining at most n elements
- Time complexity is O(n) in the worst case
- Space complexity is O(1) as only a constant amount of extra space is used

## Pseudocode (canonical)
```pseudo
function linearSearch(array, target):
    for i = 0 to array.length - 1:
        if array[i] == target:
            return i  # Target found, return its index
    
    return -1  # Target not found

# Sentinel linear search (eliminates one comparison per iteration)
function sentinelLinearSearch(array, target):
    n = array.length
    
    # Save the last element
    last = array[n-1]
    
    # Place the target as sentinel at the end
    array[n-1] = target
    
    i = 0
    # No need to check for array bounds
    while array[i] != target:
        i++
    
    # Restore the last element
    array[n-1] = last
    
    # Check if we found the target or just the sentinel
    if i < n-1 or last == target:
        return i
    else:
        return -1

# Find all occurrences
function findAllOccurrences(array, target):
    result = []
    
    for i = 0 to array.length - 1:
        if array[i] == target:
            result.add(i)
    
    return result
```

## Implementation Notes
- Use early return to avoid unnecessary comparisons
- Consider using a sentinel value to eliminate boundary check
- For large arrays, consider parallel processing
- Be careful with edge cases like empty arrays
- For objects, ensure equality comparison is properly defined
- Consider using a more efficient algorithm if the array is sorted
- For repeated searches, consider preprocessing the data
- Be aware of cache locality for better performance
- For very large datasets, consider streaming or chunking
- Implement proper error handling for invalid inputs

## Edge-case Checklist
- Empty array: Return appropriate indicator (e.g., -1)
- Single element array: Check that element directly
- Target at the beginning: Best case, found immediately
- Target at the end: Worst case, requires n comparisons
- Target not in array: Worst case, requires n comparisons
- Multiple occurrences: Return first or all as required
- Null or invalid input: Handle appropriately
- Special values (NaN, null, etc.): Define equality behavior

## Examples
- Minimal example:
  - Array: [5, 2, 9, 1, 7]
  - Target: 9
  - Iterations: Check 5, check 2, check 9
  - Result: Found at index 2
  
- Complex example:
  - Array: [10, 20, 30, 40, 30, 50]
  - Target: 30
  - Find all occurrences
  - Result: Found at indices 2 and 4

## Variants / Alternatives
- Sentinel linear search: Reduces comparisons
- Find all occurrences: Returns all positions of the target
- Early termination: Returns as soon as target is found
- With predicate: Searches for elements satisfying a condition
- Bidirectional search: Searches from both ends simultaneously
- Alternatives:
  - Binary search: O(log n) for sorted arrays
  - Hash table lookup: O(1) average case with preprocessing
  - Interpolation search: O(log log n) for uniformly distributed sorted arrays
  - Jump search: O(√n) for sorted arrays

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Knuth, Donald: "The Art of Computer Programming, Volume 3: Sorting and Searching"
- Sedgewick, Robert and Wayne, Kevin: "Algorithms, 4th Edition"
- Introduction to algorithms courses and textbooks
- Computer science fundamentals
