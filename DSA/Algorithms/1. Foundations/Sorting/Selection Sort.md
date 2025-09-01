# Selection Sort

## Name
- Category: Sorting
- Summary: Simple comparison-based sorting algorithm that divides the input into a sorted and unsorted region, repeatedly selecting the smallest element from the unsorted region and moving it to the end of the sorted region.

## Preconditions / Assumptions
- Random access to elements (array or similar data structure)
- Comparison operation defined for elements
- In-place sorting preferred
- Stability not required (though stable variants exist)

## Properties
- Deterministic
- Not stable in standard implementation (though stable variants exist)
- In-place (requires O(1) extra memory)
- Simple implementation
- Performs same number of comparisons regardless of input order
- Minimal number of swaps (O(n))

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(n²) | O(n²) | O(n²) | O(1) | Always makes n(n-1)/2 comparisons |
| Stable | O(n²) | O(n²) | O(n²) | O(n) | Requires extra space for stability |
| Bidirectional | O(n²) | O(n²) | O(n²) | O(1) | Cocktail selection sort |
| Heap-based | O(n log n) | O(n log n) | O(n log n) | O(1) | Becomes heapsort |

## When to Use / When to Avoid
- Use if:
  - Simplicity is more important than efficiency
  - Memory writes are expensive (makes only O(n) swaps)
  - List is small (n ≤ 20)
  - Auxiliary memory is limited
  - Cost of swapping elements is high
- Avoid if:
  - List is large
  - Performance is critical
  - Stability is required (without modification)
  - Better alternatives are available

## Correctness (Proof Sketch)
- Invariant: At the start of each iteration, elements A[0..i-1] are in their final sorted positions
- Each iteration selects the smallest element from A[i..n-1] and places it at position i
- After n-1 iterations, all elements must be in their correct positions
- Not stable because elements can jump over equal elements during swaps
- Termination: Fixed number of iterations (n-1)

## Pseudocode (canonical)
```pseudo
function selectionSort(arr):
    n = arr.length
    
    for i = 0 to n-2:
        # Find index of minimum element in unsorted portion
        min_idx = i
        for j = i+1 to n-1:
            if arr[j] < arr[min_idx]:
                min_idx = j
        
        # Swap minimum element with first element of unsorted portion
        if min_idx != i:
            swap(arr[i], arr[min_idx])
    
    return arr

# Stable selection sort variant
function stableSelectionSort(arr):
    n = arr.length
    
    for i = 0 to n-2:
        min_idx = i
        
        # Find minimum element
        for j = i+1 to n-1:
            if arr[j] < arr[min_idx]:
                min_idx = j
        
        # Move minimum element to position i without swapping
        # (preserves order of equal elements)
        min_val = arr[min_idx]
        
        # Shift elements between i and min_idx
        for j = min_idx downto i+1:
            arr[j] = arr[j-1]
        
        arr[i] = min_val
    
    return arr
```

## Implementation Notes
- Always performs n(n-1)/2 comparisons regardless of input order
- Makes at most n-1 swaps (much fewer than bubble sort)
- Can be modified to sort in descending order by finding maximum instead of minimum
- Stable version requires shifting elements rather than swapping
- Can be optimized to find both minimum and maximum in each pass (bidirectional)
- Good choice when write operations are expensive compared to read operations
- Consider using binary heap for better performance (becomes heapsort)

## Edge-case Checklist
- Empty array: Return as is
- Single element: Already sorted
- Two elements: Simple comparison and swap if needed
- Duplicate elements: Standard version may change relative order
- Already sorted array: Still O(n²), no early termination possible
- Reverse sorted array: Still O(n²), same as any other order
- All elements equal: Still performs all comparisons, but no swaps

## Examples
- Minimal example:
  - Input: [64, 25, 12, 22, 11]
  - First pass: Find min (11) and swap with first element: [11, 25, 12, 22, 64]
  - Second pass: Find min in remaining (12) and swap with second element: [11, 12, 25, 22, 64]
  - Third pass: Find min in remaining (22) and swap with third element: [11, 12, 22, 25, 64]
  - Fourth pass: Find min in remaining (25) and swap with fourth element: [11, 12, 22, 25, 64]
  - Output: [11, 12, 22, 25, 64]
  
- Stability example (standard version):
  - Input: [(A,2), (B,1), (C,2)]
  - Output: [(B,1), (C,2), (A,2)] (order of equal keys changed)
  - With stable version: [(B,1), (A,2), (C,2)] (order preserved)

## Variants / Alternatives
- Stable Selection Sort: Preserves order of equal elements using shifts instead of swaps
- Bidirectional Selection Sort: Finds both minimum and maximum in each pass
- Heapsort: Uses binary heap data structure for O(n log n) performance
- Alternatives:
  - Insertion Sort: Usually faster in practice, especially for small or nearly sorted arrays
  - Bubble Sort: Similar complexity but more swaps
  - Quicksort/Mergesort: Much faster for large arrays

## References
- Knuth, Donald E. "The Art of Computer Programming, Volume 3: Sorting and Searching"
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 2.2
- Sedgewick, Robert: "Algorithms, 4th Edition", Section 2.1
- Mahmoud, Hosam M. "Sorting: A Distribution Theory" (2000)
