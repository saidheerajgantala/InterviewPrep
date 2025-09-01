# Merge Sort

## Name
- Category: Sorting
- Summary: Divide-and-conquer algorithm that recursively divides the array into halves, sorts them, and merges the sorted halves.

## Preconditions / Assumptions
- Random access to elements (array or similar data structure)
- Comparison operation defined for elements
- Sufficient memory for auxiliary arrays during merging
- Stable sort required (maintains relative order of equal elements)

## Properties
- Deterministic
- Stable (preserves order of equal elements)
- Not in-place (requires O(n) auxiliary space)
- Divide-and-conquer paradigm
- External sorting friendly (works well with disk-based data)

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(n log n) | O(n log n) | O(n log n) | O(n) | Auxiliary space for merging |
| Bottom-up | O(n log n) | O(n log n) | O(n log n) | O(n) | Iterative, no recursion |
| In-place | O(n log n) | O(n log n) | O(n log n) | O(1) | Complex, rarely used |
| Timsort | O(n) | O(n log n) | O(n log n) | O(n) | Adaptive, exploits runs |

## When to Use / When to Avoid
- Use if:
  - Stable sorting is required
  - Worst-case O(n log n) performance is critical
  - External sorting with limited memory
  - Linked list sorting (natural fit)
- Avoid if:
  - Memory is severely constrained
  - In-place sorting is required
  - Data is nearly sorted (consider insertion sort)
  - Small arrays (overhead of recursion)

## Correctness (Proof Sketch)
- Base case: A single element is trivially sorted
- Inductive step: If we can sort two subarrays, we can merge them in sorted order
- Merging preserves the sorted property of subarrays
- Recursion terminates when subarrays reach size 1
- Every element is compared at most log n times

## Pseudocode (canonical)
```pseudo
function mergeSort(arr, left, right):
    if left < right:
        mid = left + (right - left) / 2
        
        # Recursively sort both halves
        mergeSort(arr, left, mid)
        mergeSort(arr, mid + 1, right)
        
        # Merge the sorted halves
        merge(arr, left, mid, right)

function merge(arr, left, mid, right):
    # Create temporary arrays
    n1 = mid - left + 1
    n2 = right - mid
    L = new array of size n1
    R = new array of size n2
    
    # Copy data to temporary arrays
    for i = 0 to n1-1:
        L[i] = arr[left + i]
    for j = 0 to n2-1:
        R[j] = arr[mid + 1 + j]
    
    # Merge the arrays back
    i = 0, j = 0, k = left
    while i < n1 and j < n2:
        if L[i] <= R[j]:  # <= ensures stability
            arr[k] = L[i]
            i++
        else:
            arr[k] = R[j]
            j++
        k++
    
    # Copy remaining elements
    while i < n1:
        arr[k] = L[i]
        i++
        k++
    while j < n2:
        arr[k] = R[j]
        j++
        k++
```

## Implementation Notes
- Use insertion sort for small subarrays (typically < 10-20 elements)
- Avoid creating temporary arrays repeatedly by reusing allocated space
- For linked lists, merging doesn't require extra space
- Bottom-up implementation avoids recursion overhead
- Parallelizable: different subarrays can be sorted independently

## Edge-case Checklist
- Empty array: Return as is
- Single element: Already sorted
- Two elements: Simple comparison and swap if needed
- Duplicate elements: Preserved in original order (stability)
- Already sorted array: Still O(n log n), no optimization in basic version
- Reverse sorted: No worse than random data

## Examples
- Minimal example:
  - Input: [38, 27, 43, 3, 9, 82, 10]
  - Output: [3, 9, 10, 27, 38, 43, 82]
  
- Stability example:
  - Input: [(A,3), (B,1), (C,3), (D,2)]
  - Output: [(B,1), (D,2), (A,3), (C,3)]
  - Note: (A,3) comes before (C,3) in output, preserving original order

## Variants / Alternatives
- Bottom-up Merge Sort: Iterative implementation
- Natural Merge Sort: Exploits existing runs in the data
- Timsort: Hybrid of merge sort and insertion sort (used in Python, Java)
- Parallel Merge Sort: Distributes sorting across multiple processors
- External Merge Sort: For data that doesn't fit in memory
- Alternatives:
  - Quicksort: Usually faster in practice, but O(n²) worst case
  - Heapsort: In-place O(n log n), but not stable

## References
- Knuth, Donald E. "The Art of Computer Programming, Volume 3: Sorting and Searching"
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 2.3
- Sedgewick, Robert: "Algorithms in C++, Parts 1-4", Chapter 8
