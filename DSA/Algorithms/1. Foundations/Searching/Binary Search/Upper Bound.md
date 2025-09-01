# Upper Bound Binary Search

## Name
- Category: Search
- Summary: Variant of binary search that finds the position of the first element greater than a given target value in a sorted array.

## Preconditions / Assumptions
- Array must be sorted (typically in ascending order)
- Random access to elements (O(1) indexing)
- Comparison operation must be defined for elements
- May contain duplicate elements

## Properties
- Deterministic
- Complete (always finds the correct position)
- Returns the first position where an element > target exists
- Works with duplicates (finds position after last occurrence)
- Returns array length if no element is greater than target

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(log n) | O(log n) | O(log n) | O(1) | Iterative implementation |
| Recursive | O(log n) | O(log n) | O(log n) | O(log n) | Stack space |
| STL-style | O(log n) | O(log n) | O(log n) | O(1) | C++ std::upper_bound style |

## When to Use / When to Avoid
- Use if:
  - Need first position where element > target
  - Need to find position after last occurrence of target
  - Implementing range queries in sorted array
  - Finding rightmost valid position in monotonic function
- Avoid if:
  - Array is unsorted
  - Only need to check existence (standard binary search)
  - Need exact matches only
  - Need leftmost position (lower bound)

## Correctness (Proof Sketch)
- Invariant: After each iteration, if an element > target exists, it is in range [low, high]
- For upper bound: All elements at positions < low are ≤ target
- For upper bound: All elements at positions ≥ high are > target (or high = array length)
- Binary decisions narrow search space by half each time
- Termination: When low = high, we've found the upper bound position
- Edge cases: Returns array length if all elements ≤ target

## Pseudocode (canonical)
```pseudo
function upperBound(array, target):
    low = 0
    high = array.length
    
    while low < high:
        mid = low + (high - low) / 2  # Prevent overflow
        
        if array[mid] <= target:
            low = mid + 1  # Look in right half
        else:
            high = mid  # Look in left half
    
    return low  # low == high == upper bound position
```

## Implementation Notes
- Use `low + (high - low) / 2` instead of `(low + high) / 2` to prevent integer overflow
- Ensure correct comparison: `array[mid] <= target` for upper bound
- Return value is the index of the first element greater than target
- For 1-indexed arrays, adjust the return value accordingly
- Can be modified to return -1 or other sentinel if no element > target exists
- Unlike standard binary search, the loop condition is `low < high` not `low <= high`
- No explicit equality check in the loop

## Edge-case Checklist
- Empty array: Returns 0
- Single element: Returns 0 if element ≤ target, 1 otherwise
- Target smaller than all elements: Returns 0
- Target larger than all elements: Returns array length
- Duplicate elements: Returns position after last occurrence
- Array at integer size limits: Use overflow-safe mid calculation

## Examples
- Minimal example:
  - Input: [1, 3, 3, 5, 7], target = 3
  - Output: 3 (index after last 3)
  
- Edge case examples:
  - Input: [1, 3, 5, 7], target = 0
    - Output: 0 (all elements are greater)
  - Input: [1, 3, 5, 7], target = 9
    - Output: 4 (no elements are greater)
  - Input: [1, 3, 5, 7], target = 4
    - Output: 2 (position of 5)
  - Input: [1, 1, 1, 1], target = 1
    - Output: 4 (position after all 1s)

## Variants / Alternatives
- Lower bound: First element ≥ target
- Equal range: Range of elements equal to target (combine lower and upper bound)
- Galloping/exponential search: For unbounded or very large arrays
- Interpolation search: Uses value-based interpolation instead of midpoint
- Binary search on answer: For optimization problems with monotonic property
- Alternatives:
  - Linear search: O(n) but simpler for very small arrays
  - Hash-based search: O(1) average but doesn't provide ordering information

## References
- Knuth, Donald: "The Art of Computer Programming, Vol. 3: Sorting and Searching"
- C++ Standard Library: std::upper_bound implementation
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Sedgewick, Wayne: "Algorithms, 4th Edition"
