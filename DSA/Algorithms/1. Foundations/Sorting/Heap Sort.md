# Heap Sort

## Name
- Category: Sorting
- Summary: Comparison-based sorting algorithm that uses a binary heap data structure to build a max-heap and repeatedly extract the maximum element.

## Preconditions / Assumptions
- Random access to elements (array or similar data structure)
- Comparison operation defined for elements
- In-place sorting preferred
- Stable sorting not required

## Properties
- Deterministic
- Not stable (doesn't preserve order of equal elements)
- In-place (no extra memory beyond the input array)
- Based on binary heap data structure
- Comparison-based sorting algorithm
- Guaranteed O(n log n) worst-case performance

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(n log n) | O(n log n) | O(n log n) | O(1) | In-place implementation |
| Bottom-up | O(n) | O(n log n) | O(n log n) | O(1) | Faster heap construction |
| With Floyd's | O(n) | O(n log n) | O(n log n) | O(1) | Optimized sift-down |
| External | O(n log n) | O(n log n) | O(n log n) | O(1) | For large datasets |

## When to Use / When to Avoid
- Use if:
  - Worst-case O(n log n) performance is critical
  - In-place sorting required
  - Stable sorting not needed
  - Memory usage is constrained
  - Guaranteed performance more important than average case
- Avoid if:
  - Stable sorting required
  - Cache efficiency critical (poor locality)
  - Average-case performance is more important (quicksort often better)
  - Input is nearly sorted (insertion sort may be better)

## Correctness (Proof Sketch)
- Heap property: In a max-heap, for any node i, the value at i is greater than or equal to its children
- Heap construction: Bottom-up approach ensures heap property for all subtrees
- Extraction: Maximum element is always at the root of the heap
- Invariant: After i iterations, the i largest elements are in their final sorted positions
- Termination: After n-1 extractions, all elements are in sorted order
- Complexity: Heap construction is O(n), each extraction is O(log n), for total O(n log n)

## Pseudocode (canonical)
```pseudo
function heapSort(arr):
    n = arr.length
    
    # Build max heap
    for i = n/2 - 1 down to 0:
        heapify(arr, n, i)
    
    # Extract elements one by one
    for i = n - 1 down to 0:
        # Move current root to end
        swap(arr[0], arr[i])
        
        # Call heapify on reduced heap
        heapify(arr, i, 0)

# Heapify a subtree rooted at node i
function heapify(arr, n, i):
    largest = i
    left = 2*i + 1
    right = 2*i + 2
    
    # Check if left child exists and is greater than root
    if left < n and arr[left] > arr[largest]:
        largest = left
    
    # Check if right child exists and is greater than largest so far
    if right < n and arr[right] > arr[largest]:
        largest = right
    
    # If largest is not root
    if largest != i:
        swap(arr[i], arr[largest])
        
        # Recursively heapify the affected subtree
        heapify(arr, n, largest)
```

## Implementation Notes
- Can be implemented iteratively to avoid recursion stack
- Bottom-up heap construction is more efficient than repeated insertions
- Floyd's method for building heaps is more efficient in practice
- Watch for off-by-one errors in array indexing (0-based vs 1-based)
- Can be adapted for priority queue implementation
- For large arrays, consider iterative heapify to avoid stack overflow
- Can be modified for ascending order by using min-heap instead of max-heap

## Edge-case Checklist
- Empty array: Return as is
- Single element: Already sorted
- Two elements: Simple comparison and swap if needed
- Duplicate elements: Handled correctly, but order not preserved (not stable)
- Already sorted array: Still O(n log n), no optimization
- Reverse sorted: Still O(n log n), no worse than random data
- All elements equal: Still performs all comparisons

## Examples
- Minimal example:
  - Input: [4, 10, 3, 5, 1]
  - After build_heap: [10, 5, 3, 4, 1] (max-heap)
  - Iterations:
    1. Swap 10 & 1: [1, 5, 3, 4, 10], heapify: [5, 4, 3, 1, 10]
    2. Swap 5 & 1: [1, 4, 3, 5, 10], heapify: [4, 1, 3, 5, 10]
    3. Swap 4 & 3: [3, 1, 4, 5, 10], heapify: [3, 1, 4, 5, 10]
    4. Swap 3 & 1: [1, 3, 4, 5, 10] (sorted)
  
- Heap visualization:
  ```
      10
     /  \
    5    3
   / \
  4   1
  ```

## Variants / Alternatives
- Min-heap variant: For descending order
- Bottom-up heap construction: More efficient building of initial heap
- Floyd's heap construction: Optimized approach for building heaps
- Smoothsort: Variant with O(n) best case for nearly sorted data
- External heapsort: For sorting data that doesn't fit in memory
- Alternatives:
  - Quicksort: Often faster in practice, but O(n²) worst case
  - Mergesort: Stable, guaranteed O(n log n), but not in-place
  - Introsort: Hybrid of quicksort, heapsort, and insertion sort

## References
- Williams, J.W.J. "Algorithm 232: Heapsort" (1964)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 6
- Knuth, Donald E.: "The Art of Computer Programming, Volume 3: Sorting and Searching"
- Sedgewick, Robert: "Algorithms in C++, Parts 1-4", Chapter 9
