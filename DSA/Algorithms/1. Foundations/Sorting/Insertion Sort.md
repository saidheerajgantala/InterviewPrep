# Insertion Sort

## Name
- Category: Sorting
- Summary: Simple, efficient sorting algorithm that builds the final sorted array one element at a time by inserting each element into its correct position among previously sorted elements.

## Preconditions / Assumptions
- Random access to elements (array or similar data structure)
- Comparison operation defined for elements
- In-place sorting preferred
- Stability may be required

## Properties
- Deterministic
- Stable (preserves order of equal elements)
- In-place (requires O(1) extra memory)
- Adaptive (efficient for nearly sorted data)
- Online (can sort as data arrives)
- Efficient for small data sets
- Simple implementation

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Standard | O(n) | O(n²) | O(n²) | O(1) | Best case when already sorted |
| Binary Insertion | O(n log n) | O(n²) | O(n²) | O(1) | Reduces comparisons, not swaps |
| Linked List | O(n) | O(n²) | O(n²) | O(1) | No shifting required |
| Shell Sort | O(n log n) | Depends on gap | O(n²) | O(1) | Generalization of insertion sort |

## When to Use / When to Avoid
- Use if:
  - List is small (n ≤ 20)
  - List is nearly sorted
  - Memory usage is constrained
  - Online sorting required (elements arrive one by one)
  - Simple implementation is preferred
- Avoid if:
  - List is large and unsorted
  - Performance is critical for large datasets
  - Data is in reverse or random order

## Correctness (Proof Sketch)
- Invariant: At the start of each iteration, elements A[0..i-1] are sorted
- Each step inserts A[i] into its correct position in the sorted subarray A[0..i-1]
- After n iterations, all elements are in their correct positions
- Stable because elements are only moved when strictly greater/smaller
- Termination: Fixed number of iterations (n-1)

## Pseudocode (canonical)
```pseudo
function insertionSort(arr):
    n = arr.length
    
    for i = 1 to n-1:
        # Store current element to be inserted
        key = arr[i]
        
        # Find position for key in the sorted portion
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j+1] = arr[j]  # Shift elements to make room
            j = j - 1
        
        # Insert key in the correct position
        arr[j+1] = key
    
    return arr

# Binary Insertion Sort variant (reduces comparisons)
function binaryInsertionSort(arr):
    n = arr.length
    
    for i = 1 to n-1:
        key = arr[i]
        
        # Use binary search to find insertion point
        left = 0
        right = i - 1
        
        while left <= right:
            mid = left + (right - left) / 2
            if arr[mid] > key:
                right = mid - 1
            else:
                left = mid + 1
        
        # Shift elements to make room
        for j = i - 1 downto left:
            arr[j+1] = arr[j]
        
        # Insert key at the correct position
        arr[left] = key
    
    return arr
```

## Implementation Notes
- Each iteration expands the sorted portion by one element
- For large arrays, binary search can reduce the number of comparisons
- Can be modified to sort in descending order by changing comparison
- For linked lists, no shifting is required (just pointer manipulation)
- Can be optimized by moving elements in blocks rather than one by one
- Performs well when array is already partially sorted
- Consider using Shell sort for better performance on larger arrays

## Edge-case Checklist
- Empty array: Return as is
- Single element: Already sorted
- Two elements: Simple comparison and swap if needed
- Duplicate elements: Stable sorting preserves original order
- Already sorted array: Best case O(n)
- Reverse sorted array: Worst case O(n²)
- All elements equal: Best case O(n) with optimized implementation

## Examples
- Minimal example:
  - Input: [5, 2, 4, 6, 1, 3]
  - After i=1: [2, 5, 4, 6, 1, 3] (insert 2 before 5)
  - After i=2: [2, 4, 5, 6, 1, 3] (insert 4 before 5)
  - After i=3: [2, 4, 5, 6, 1, 3] (6 is already in position)
  - After i=4: [1, 2, 4, 5, 6, 3] (insert 1 at beginning)
  - After i=5: [1, 2, 3, 4, 5, 6] (insert 3 after 2)
  - Output: [1, 2, 3, 4, 5, 6]
  
- Stability example:
  - Input: [(A,2), (B,1), (C,2)]
  - Output: [(B,1), (A,2), (C,2)] (maintains order of equal keys)

## Variants / Alternatives
- Binary Insertion Sort: Uses binary search to find insertion point
- Shell Sort: Generalization that sorts elements at decreasing intervals
- Library Sort (Gapped Insertion Sort): Leaves gaps for more efficient insertions
- Patience Sort: Builds sorted subsequences, then merges them
- Alternatives:
  - Selection Sort: Simpler but slower in practice
  - Bubble Sort: Similar complexity but usually slower
  - Merge Sort: More efficient for large arrays but not in-place
  - Quicksort: More efficient for large arrays but not stable

## References
- Knuth, Donald E. "The Art of Computer Programming, Volume 3: Sorting and Searching"
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 2.1
- Sedgewick, Robert: "Algorithms, 4th Edition", Section 2.1
- Bentley, Jon L. "Programming Pearls", Column 11
