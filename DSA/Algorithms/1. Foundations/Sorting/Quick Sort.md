# Quick Sort

## Name
- Category: Sorting
- Summary: Divide-and-conquer algorithm that recursively partitions the array around a pivot element, placing smaller elements before and larger elements after.

## Preconditions / Assumptions
- Random access to elements (array or similar data structure)
- Comparison operation defined for elements
- In-place sorting preferred
- Stable sorting not required

## Properties
- Divide-and-conquer paradigm
- Not stable (doesn't preserve order of equal elements)
- In-place (requires O(log n) stack space)
- Adaptive to input patterns with good pivot selection
- Randomized variant is probabilistically optimal

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Basic | O(n log n) | O(n log n) | O(n²) | O(log n) | Worst case with bad pivots |
| Randomized | O(n log n) | O(n log n) | O(n²) | O(log n) | Worst case extremely unlikely |
| Median-of-3 | O(n log n) | O(n log n) | O(n²) | O(log n) | Better pivot selection |
| Dual-Pivot | O(n log n) | O(n log n) | O(n²) | O(log n) | Uses two pivots, faster in practice |
| 3-Way | O(n) | O(n log n) | O(n²) | O(log n) | O(n) for many equal elements |

## When to Use / When to Avoid
- Use if:
  - Average-case performance is critical
  - In-place sorting required
  - Cache efficiency important
  - Stable sorting not needed
  - Random access available
- Avoid if:
  - Worst-case O(n²) unacceptable
  - Stable sorting required
  - Input likely to be nearly sorted or reversed
  - Working with linked lists

## Correctness (Proof Sketch)
- Partitioning invariant: After partition, all elements < pivot are before it, all elements > pivot are after it
- Recursive structure: After partitioning, both subarrays are sorted independently
- Base case: Arrays of size 0 or 1 are already sorted
- Termination: Each recursive call operates on strictly smaller arrays
- Randomization: Ensures expected O(n log n) time regardless of input order

## Pseudocode (canonical)
```pseudo
function quickSort(arr, low, high):
    if low < high:
        # Partition the array and get pivot position
        pivot_idx = partition(arr, low, high)
        
        # Recursively sort elements before and after pivot
        quickSort(arr, low, pivot_idx - 1)
        quickSort(arr, pivot_idx + 1, high)

function partition(arr, low, high):
    # Choose rightmost element as pivot
    pivot = arr[high]
    
    # Index of smaller element
    i = low - 1
    
    for j = low to high - 1:
        # If current element is smaller than pivot
        if arr[j] <= pivot:
            i++
            swap(arr[i], arr[j])
    
    # Place pivot in its correct position
    swap(arr[i + 1], arr[high])
    return i + 1

# Randomized variant
function randomizedPartition(arr, low, high):
    # Choose random pivot
    random_idx = random(low, high)
    swap(arr[random_idx], arr[high])
    return partition(arr, low, high)
```

## Implementation Notes
- Use insertion sort for small subarrays (typically < 10-20 elements)
- Consider median-of-three pivot selection (first, middle, last)
- Watch for stack overflow with deep recursion (use tail recursion)
- Implement iterative version for very large arrays
- Handle duplicate elements with three-way partitioning
- Consider introsort (switch to heapsort if recursion too deep)
- Be careful with partitioning to avoid off-by-one errors

## Edge-case Checklist
- Empty array: Return as is
- Single element: Already sorted
- All elements equal: Three-way partitioning helps
- Already sorted array: Bad for basic quicksort (use randomized)
- Reverse sorted: Also bad for basic quicksort
- Few unique values: Three-way partitioning beneficial
- Large arrays: Watch for stack overflow in recursive implementation

## Examples
- Minimal example:
  - Input: [38, 27, 43, 3, 9, 82, 10]
  - First partition (pivot=10): [3, 9, 10, 43, 27, 82, 38]
  - Recursive calls on [3, 9] and [43, 27, 82, 38]
  - Final output: [3, 9, 10, 27, 38, 43, 82]
  
- Worst-case example:
  - Input (already sorted): [1, 2, 3, 4, 5]
  - Basic quicksort makes O(n²) comparisons
  - Randomized or median-of-3 avoids this

## Variants / Alternatives
- Randomized Quicksort: Random pivot selection
- Median-of-three: Better pivot selection
- Three-way Quicksort: Handles duplicates efficiently
- Dual-pivot Quicksort: Uses two pivots (Java's Arrays.sort)
- Introsort: Hybrid of quicksort and heapsort
- External Quicksort: For data that doesn't fit in memory
- Alternatives:
  - Mergesort: Stable, guaranteed O(n log n), but not in-place
  - Heapsort: In-place, guaranteed O(n log n), but slower in practice
  - Timsort: Hybrid sort, better for partially sorted data

## References
- Hoare, C.A.R. "Quicksort", Computer Journal, 1962
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 7
- Sedgewick, Robert: "Algorithms in C++, Parts 1-4", Chapter 7
- Bentley, Jon L. "Programming Pearls", Column 11
