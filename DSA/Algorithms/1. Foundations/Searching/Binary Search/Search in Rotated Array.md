# Search in Rotated Sorted Array

## Name
- Category: Search
- Summary: Modified binary search algorithm to find an element in a sorted array that has been rotated around an unknown pivot point.

## Preconditions / Assumptions
- Array was originally sorted (typically in ascending order)
- Array has been rotated around a pivot (i.e., shifted circularly)
- Random access to elements (O(1) indexing)
- Comparison operation must be defined for elements
- No duplicate elements (for simplicity; variants exist for duplicates)

## Properties
- Deterministic
- Complete (finds the element if it exists)
- Logarithmic time complexity despite rotation
- Works by identifying which subarray is properly sorted
- Returns index of target or indicator that target doesn't exist

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(log n) | O(log n) | O(log n) | O(1) | No duplicates |
| With duplicates | O(log n) | O(log n) | O(n) | O(1) | Worst case when many duplicates |
| Find pivot first | O(log n) | O(log n) | O(log n) | O(1) | Two binary searches |
| Recursive | O(log n) | O(log n) | O(log n) | O(log n) | Stack space |

## When to Use / When to Avoid
- Use if:
  - Array is known to be sorted and rotated
  - Need to find specific element efficiently
  - Random access is available
  - Memory constraints prevent unrotating the array
- Avoid if:
  - Array is not sorted or rotation pattern is complex
  - Array contains many duplicates (may degrade to O(n))
  - Simpler to unrotate the array first
  - Frequent searches (consider unrotating once)

## Correctness (Proof Sketch)
- Key insight: Any rotation divides array into two sorted subarrays
- In each step, at least one half of the array must be sorted
- By comparing target with endpoints of the sorted half, we can determine if target lies in that half
- If target is in sorted half, apply standard binary search
- If target is not in sorted half, search the other half
- Termination: Search space reduces by half in each iteration
- Invariant: If target exists, it remains in the search space

## Pseudocode (canonical)
```pseudo
function searchInRotatedArray(arr, target):
    left = 0
    right = arr.length - 1
    
    while left <= right:
        mid = left + (right - left) / 2  # Prevent overflow
        
        if arr[mid] == target:
            return mid  # Found target
        
        # Check which half is sorted
        if arr[left] <= arr[mid]:  # Left half is sorted
            # Check if target is in the sorted left half
            if arr[left] <= target and target < arr[mid]:
                right = mid - 1  # Search left half
            else:
                left = mid + 1  # Search right half
        else:  # Right half is sorted
            # Check if target is in the sorted right half
            if arr[mid] < target and target <= arr[right]:
                left = mid + 1  # Search right half
            else:
                right = mid - 1  # Search left half
    
    return -1  # Target not found
```

## Implementation Notes
- Use `left + (right - left) / 2` instead of `(left + right) / 2` to prevent integer overflow
- Be careful with boundary conditions and equality checks
- Handle edge cases like empty array or single element array
- For arrays with duplicates, worst-case time complexity may degrade to O(n)
- Can be implemented recursively, but iterative version avoids stack overflow
- Consider finding the rotation pivot first, then performing two standard binary searches

## Edge-case Checklist
- Empty array: Return -1 or appropriate indicator
- Single element: Direct comparison
- No rotation (pivot at start/end): Works like standard binary search
- Full rotation (back to original): Works like standard binary search
- Target not in array: Return -1 or appropriate indicator
- Duplicates: May require linear search in worst case
- Rotation at midpoint: Both halves appear sorted

## Examples
- Minimal example:
  - Input: [4, 5, 6, 7, 0, 1, 2], target = 0
  - Analysis:
    - mid = 3, arr[mid] = 7
    - Left half [4, 5, 6, 7] is sorted
    - target = 0 is not in left half
    - Search right half [0, 1, 2]
    - mid = 5, arr[mid] = 1
    - Right half [0, 1, 2] is sorted
    - target = 0 is in right half
    - Search left half [0]
    - mid = 4, arr[mid] = 0
    - Found target at index 4
  
- Edge case example:
  - Input: [3, 1], target = 1
  - Analysis:
    - mid = 0, arr[mid] = 3
    - Left half is just [3], which is sorted
    - target = 1 is not in left half
    - Search right half [1]
    - mid = 1, arr[mid] = 1
    - Found target at index 1

## Variants / Alternatives
- Search in rotated array with duplicates: Requires additional handling
- Find minimum in rotated sorted array: Related problem to find the rotation point
- Find rotation count: Equivalent to finding the minimum element's index
- Circular binary search: More general approach for circular arrays
- Alternatives:
  - Linear search: O(n) time, simpler but inefficient
  - Unrotate first, then binary search: O(n) + O(log n) = O(n)
  - Two binary searches (find pivot, then search): O(log n) but more complex

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- LeetCode Problem #33: "Search in Rotated Sorted Array"
- LeetCode Problem #81: "Search in Rotated Sorted Array II" (with duplicates)
- Skiena, Steven: "The Algorithm Design Manual"
