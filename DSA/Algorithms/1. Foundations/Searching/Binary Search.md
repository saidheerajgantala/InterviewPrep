# Binary Search

## Name
- Category: Search
- Summary: Efficiently find a target value in a sorted collection by repeatedly dividing the search space in half.

## Preconditions / Assumptions
- Data must be sorted (ascending or descending)
- Random access to elements (O(1) indexing)
- Comparison operation must be defined for elements
- For standard binary search: no duplicates if exact position matters

## Properties
- Deterministic
- Not in-place (requires additional variables for bounds)
- Complete (will find the element if it exists)
- Optimal for searching in sorted arrays (information-theoretic lower bound)

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(1) | O(log n) | O(log n) | O(1) | Target found immediately |
| Recursive | O(1) | O(log n) | O(log n) | O(log n) | Stack space |
| Lower Bound | O(log n) | O(log n) | O(log n) | O(1) | First element ≥ target |
| Upper Bound | O(log n) | O(log n) | O(log n) | O(1) | First element > target |

## When to Use / When to Avoid
- Use if: 
  - Data is already sorted
  - Random access is available
  - Search operation is frequent (justifies sorting cost)
  - Memory constraints prevent hash tables
- Avoid if:
  - Data is unsorted and sorting cost isn't justified
  - Data structure doesn't support random access (linked lists)
  - Data is very small (linear search may be faster due to simplicity)
  - Hash table lookup is viable (O(1) average)

## Correctness (Proof Sketch)
- Invariant: Target, if present, is always within the search interval [low, high]
- Each iteration reduces search space by half while maintaining invariant
- Termination: Search space eventually becomes empty or target is found
- Edge cases: Empty array handled by initial check; single element directly compared

## Pseudocode (canonical)
```pseudo
function binarySearch(array, target):
    # invariant: if target exists, it's in array[low...high]
    low = 0
    high = array.length - 1
    
    while low <= high:
        mid = low + (high - low) / 2  # prevents overflow
        
        if array[mid] == target:
            return mid  # target found
        else if array[mid] < target:
            low = mid + 1  # search right half
        else:
            high = mid - 1  # search left half
            
    return -1  # target not found
```

## Implementation Notes
- Use `low + (high - low) / 2` instead of `(low + high) / 2` to prevent integer overflow
- Be careful with boundary conditions (<=, <, etc.)
- For floating-point comparisons, use epsilon for equality
- For lower/upper bound variants, adjust the return condition but keep shrinking logic
- Watch for off-by-one errors in boundary calculations

## Edge-case Checklist
- Empty array: Return -1 or appropriate indicator
- Single element: Direct comparison
- Target smaller than all elements: Returns -1
- Target larger than all elements: Returns -1
- Duplicate elements: Returns position of any matching element (not guaranteed which one)
- Array at integer size limits: Use overflow-safe mid calculation

## Examples
- Minimal example:
  - Input: [1, 3, 5, 7, 9], target = 5
  - Output: 2 (index of target)
  
- Tricky counterexample:
  - Input: [1, 3, 3, 3, 5], target = 3
  - Output: Could be 1, 2, or 3 (standard binary search doesn't guarantee which)
  - For lower bound variant: Output would be 1 (first occurrence)

## Variants / Alternatives
- Lower bound: Find first element >= target
- Upper bound: Find first element > target
- Approximate binary search: Find closest element to target
- Exponential search: Combine galloping with binary search for unbounded arrays
- Interpolation search: O(log log n) average for uniformly distributed data
- Fractional cascading: Speeds up binary search across multiple arrays

## References
- CLRS (Introduction to Algorithms), Chapter 2.3
- Knuth, The Art of Computer Programming, Vol. 3: Sorting and Searching
- https://en.wikipedia.org/wiki/Binary_search_algorithm 