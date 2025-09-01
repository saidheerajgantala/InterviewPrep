# Bubble Sort

## Name
- Category: Sorting
- Summary: Simple comparison-based sorting algorithm that repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order.

## Preconditions / Assumptions
- Random access to elements (array or similar data structure)
- Comparison operation defined for elements
- In-place sorting preferred
- Stability may be required

## Properties
- Deterministic
- Stable (preserves order of equal elements)
- In-place (requires O(1) extra memory)
- Simple implementation
- Adaptive (can detect if list is already sorted)
- Inefficient for large lists

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(n) | O(n²) | O(n²) | O(1) | Best case when already sorted |
| Optimized | O(n) | O(n²) | O(n²) | O(1) | Early termination if no swaps |
| Bidirectional | O(n) | O(n²) | O(n²) | O(1) | Cocktail sort variant |
| Recursive | O(n) | O(n²) | O(n²) | O(n) | Due to recursion stack |

## When to Use / When to Avoid
- Use if:
  - Simplicity is more important than efficiency
  - List is very small (n ≤ 10)
  - List is nearly sorted
  - Memory usage is severely constrained
  - Educational purposes
- Avoid if:
  - List is large
  - Performance is critical
  - Better alternatives are available (almost always)
  - Complex data structures with expensive comparison operations

## Correctness (Proof Sketch)
- Invariant: After i passes, the i largest elements are in their correct positions
- Each pass moves the largest unsorted element to its correct position
- After n-1 passes, all elements must be in their correct positions
- Stable because only adjacent elements are swapped when strictly out of order
- Termination: Fixed number of passes (n-1) in worst case

## Pseudocode (canonical)
```pseudo
function bubbleSort(arr):
    n = arr.length
    
    for i = 0 to n-2:
        for j = 0 to n-2-i:
            if arr[j] > arr[j+1]:
                swap(arr[j], arr[j+1])
    
    return arr

# Optimized version with early termination
function optimizedBubbleSort(arr):
    n = arr.length
    
    for i = 0 to n-2:
        swapped = false
        
        for j = 0 to n-2-i:
            if arr[j] > arr[j+1]:
                swap(arr[j], arr[j+1])
                swapped = true
        
        # If no swapping occurred in this pass, array is sorted
        if not swapped:
            break
    
    return arr
```

## Implementation Notes
- Use optimized version with early termination flag
- Each pass places the largest unsorted element at the end
- Inner loop can be shortened by i each time (elements at the end are already sorted)
- Can be modified to sort in descending order by changing comparison
- For stability, ensure equal elements are not swapped
- Consider bidirectional bubble sort (cocktail sort) for better performance
- Can be parallelized with odd-even transposition sort variant

## Edge-case Checklist
- Empty array: Return as is
- Single element: Already sorted
- Two elements: Simple comparison and swap if needed
- Duplicate elements: Stable sorting preserves original order
- Already sorted array: Best case O(n) with optimized version
- Reverse sorted array: Worst case O(n²)
- All elements equal: No swaps needed, early termination

## Examples
- Minimal example:
  - Input: [5, 1, 4, 2, 8]
  - First pass: [1, 4, 2, 5, 8] (swaps: 5-1, 5-4, 5-2)
  - Second pass: [1, 2, 4, 5, 8] (swaps: 4-2)
  - Third pass: No swaps, array is sorted
  - Output: [1, 2, 4, 5, 8]
  
- Stability example:
  - Input: [(A,2), (B,1), (C,2)]
  - Output: [(B,1), (A,2), (C,2)] (maintains order of equal keys)

## Variants / Alternatives
- Cocktail Sort (Bidirectional Bubble Sort): Alternates between forward and backward passes
- Odd-Even Sort: Parallel version of bubble sort
- Comb Sort: Improves bubble sort by comparing elements with gap larger than 1
- Gnome Sort: Similar to insertion sort, moves elements backward until in position
- Alternatives:
  - Insertion Sort: Usually faster in practice
  - Selection Sort: Fewer swaps but same time complexity
  - Quicksort/Mergesort/Heapsort: Much faster for large arrays

## References
- Knuth, Donald E. "The Art of Computer Programming, Volume 3: Sorting and Searching"
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Sedgewick, Robert: "Algorithms in C++, Parts 1-4", Chapter 6
- Astrachan, Owen: "Bubble Sort: An Archaeological Algorithmic Analysis" (SIGCSE 2003)
