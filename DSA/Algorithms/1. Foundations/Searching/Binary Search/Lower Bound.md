# Lower Bound Binary Search

## Name
- Category: Search
- Summary: Variant of binary search that finds the position of the first element greater than or equal to a given target value in a sorted array.

## Preconditions / Assumptions
- Array must be sorted (typically in ascending order)
- Random access to elements (O(1) indexing)
- Comparison operation must be defined for elements
- May contain duplicate elements

## Properties
- Deterministic
- Complete (always finds the correct position)
- Returns the leftmost/first position where target could be inserted while maintaining order
- Works with duplicates (finds first occurrence)
- Returns array length if all elements are less than target

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(log n) | O(log n) | O(log n) | O(1) | Iterative implementation |
| Recursive | O(log n) | O(log n) | O(log n) | O(log n) | Stack space |
| STL-style | O(log n) | O(log n) | O(log n) | O(1) | C++ std::lower_bound style |

## When to Use / When to Avoid
- Use if:
  - Need first position where element ≥ target
  - Need to handle duplicates in sorted array
  - Implementing insertion point in sorted array
  - Finding leftmost valid position in monotonic function
- Avoid if:
  - Array is unsorted
  - Only need to check existence (standard binary search)
  - Need exact matches only
  - Need rightmost position (upper bound)

## Correctness (Proof Sketch)
- Invariant: After each iteration, if target exists in array, it is in range [low, high]
- For lower bound: All elements at positions < low are < target
- For lower bound: At least one element at position ≥ high is ≥ target (or high = array length)
- Binary decisions narrow search space by half each time
- Termination: When low = high, we've found the lower bound position
- Edge cases: Returns 0 if target ≤ first element, returns length if target > all elements

## Pseudocode (canonical)
```pseudo
function lowerBound(array, target):
    low = 0
    high = array.length
    
    while low < high:
        mid = low + (high - low) / 2  # Prevent overflow
        
        if array[mid] < target:
            low = mid + 1  # Look in right half
        else:
            high = mid  # Look in left half, including mid
    
    return low  # low == high == lower bound position
```

## Implementation Notes
- Use `low + (high - low) / 2` instead of `(low + high) / 2` to prevent integer overflow
- Ensure correct comparison: `array[mid] < target` for lower bound
- Return value is the index where target should be inserted to maintain order
- For 1-indexed arrays, adjust the return value accordingly
- Can be modified to return -1 or other sentinel if target is not present
- Unlike standard binary search, the loop condition is `low < high` not `low <= high`
- No explicit equality check in the loop

## Edge-case Checklist
- Empty array: Returns 0
- Single element: Returns 0 if element ≥ target, 1 otherwise
- Target smaller than all elements: Returns 0
- Target larger than all elements: Returns array length
- Duplicate elements: Returns index of first occurrence
- Array at integer size limits: Use overflow-safe mid calculation

## Examples
- Minimal example:
  - Input: [1, 3, 3, 5, 7], target = 3
  - Output: 1 (index of first 3)
  
- Edge case examples:
  - Input: [1, 3, 5, 7], target = 0
    - Output: 0 (insert at beginning)
  - Input: [1, 3, 5, 7], target = 9
    - Output: 4 (insert at end)
  - Input: [1, 3, 5, 7], target = 2
    - Output: 1 (insert between 1 and 3)
  - Input: [1, 1, 1, 1], target = 1
    - Output: 0 (first occurrence)

## Variants / Alternatives
- Upper bound: First element > target
- Equal range: Range of elements equal to target
- Galloping/exponential search: For unbounded or very large arrays
- Interpolation search: Uses value-based interpolation instead of midpoint
- Binary search on answer: For optimization problems with monotonic property
- Alternatives:
  - Linear search: O(n) but simpler for very small arrays
  - Hash-based search: O(1) average but doesn't provide ordering information

## References
- Knuth, Donald: "The Art of Computer Programming, Vol. 3: Sorting and Searching"
- C++ Standard Library: std::lower_bound implementation
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Sedgewick, Wayne: "Algorithms, 4th Edition"
