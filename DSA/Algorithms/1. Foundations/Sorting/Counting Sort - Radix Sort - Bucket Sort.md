# Linear-Time Sorting Algorithms: Counting Sort, Radix Sort, and Bucket Sort

## Name
- Category: Sorting (Non-comparison based)
- Summary: Family of sorting algorithms that achieve O(n) time complexity by not relying on comparisons between elements, instead using the values themselves to determine position.

## Preconditions / Assumptions
### Counting Sort
- Elements are integers in a known small range [0, k]
- k is not significantly larger than n (ideally k = O(n))
- Stable sorting may be required

### Radix Sort
- Elements can be represented with fixed-width keys (integers, strings)
- Each digit/character has a limited range
- Can use a stable sort (like counting sort) as a subroutine

### Bucket Sort
- Input is uniformly distributed over a range
- Elements can be mapped to a finite number of buckets
- Each bucket can be sorted efficiently (often with insertion sort)

## Properties
### Counting Sort
- Deterministic
- Stable (if implemented correctly)
- Not in-place (requires O(n + k) extra memory)
- Linear time complexity when k = O(n)

### Radix Sort
- Deterministic
- Stable (if using stable sort for digits)
- Not in-place (requires O(n + k) extra memory)
- Linear time complexity for fixed-width keys

### Bucket Sort
- Deterministic
- Stable (if using stable sort within buckets)
- Not in-place (requires O(n) extra memory)
- Linear average time with uniform distribution

## Complexity (per variant)
| Algorithm | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Counting Sort | O(n + k) | O(n + k) | O(n + k) | O(n + k) | k is the range of input |
| Radix Sort (LSD) | O(d(n + k)) | O(d(n + k)) | O(d(n + k)) | O(n + k) | d digits, k is radix |
| Radix Sort (MSD) | O(d(n + k)) | O(d(n + k)) | O(d(n + k)) | O(n + k) | Can be faster with early termination |
| Bucket Sort | O(n) | O(n) | O(n²) | O(n) | Worst case when all items in one bucket |

## When to Use / When to Avoid
### Counting Sort
- Use if:
  - Elements are integers in a small range
  - Stability is required
  - Extra memory usage is acceptable
- Avoid if:
  - Range k is very large compared to n
  - Elements are not integers or cannot be easily mapped to integers
  - Memory is severely constrained

### Radix Sort
- Use if:
  - Elements have fixed-width keys
  - Keys can be processed digit by digit
  - Stability is required
- Avoid if:
  - Keys have widely varying lengths
  - Comparison-based sorting is more natural for the data
  - Memory is severely constrained

### Bucket Sort
- Use if:
  - Input is likely uniformly distributed
  - Elements can be easily mapped to buckets
  - Average-case performance is critical
- Avoid if:
  - Distribution is highly skewed
  - Mapping to buckets is complex
  - Worst-case performance must be guaranteed

## Correctness (Proof Sketch)
### Counting Sort
- Count occurrences of each value in the input
- Compute cumulative counts to determine positions
- Place each element in its correct position based on counts
- Stable because elements with equal values maintain their relative order
- Termination: Each element is processed exactly once

### Radix Sort
- Sort elements digit by digit, starting from least significant (LSD) or most significant (MSD)
- Each digit sort must be stable to preserve previous sorting work
- After sorting all digits, the array is fully sorted
- Termination: Fixed number of digit sorts (d)

### Bucket Sort
- Distribute elements into buckets based on their value
- Sort each bucket individually (often with insertion sort)
- Concatenate all buckets in order
- With uniform distribution, each bucket has approximately n/k elements
- Termination: All elements are processed and all buckets are sorted

## Pseudocode (canonical)
```pseudo
# Counting Sort
function countingSort(arr, max_val):
    n = arr.length
    # Create counting array and output array
    count = new array of size (max_val + 1), initialized with 0
    output = new array of size n
    
    # Count occurrences of each element
    for i = 0 to n-1:
        count[arr[i]]++
    
    # Compute cumulative counts
    for i = 1 to max_val:
        count[i] += count[i-1]
    
    # Build output array (in reverse order for stability)
    for i = n-1 downto 0:
        output[count[arr[i]]-1] = arr[i]
        count[arr[i]]--
    
    # Copy output array to original array
    for i = 0 to n-1:
        arr[i] = output[i]
    
    return arr

# Radix Sort (LSD)
function radixSort(arr, d):  # d is the number of digits
    n = arr.length
    
    # Process each digit, starting from least significant
    for digit = 0 to d-1:
        # Use counting sort for each digit
        countingSortByDigit(arr, digit)
    
    return arr

function countingSortByDigit(arr, digit):
    n = arr.length
    output = new array of size n
    count = new array of size 10, initialized with 0  # For decimal digits
    
    # Count occurrences of each digit value
    for i = 0 to n-1:
        digit_value = getDigit(arr[i], digit)
        count[digit_value]++
    
    # Compute cumulative counts
    for i = 1 to 9:
        count[i] += count[i-1]
    
    # Build output array (in reverse order for stability)
    for i = n-1 downto 0:
        digit_value = getDigit(arr[i], digit)
        output[count[digit_value]-1] = arr[i]
        count[digit_value]--
    
    # Copy output array to original array
    for i = 0 to n-1:
        arr[i] = output[i]

# Bucket Sort
function bucketSort(arr, num_buckets):
    n = arr.length
    
    # Find min and max values
    min_val = minimum value in arr
    max_val = maximum value in arr
    range = max_val - min_val + 1
    
    # Create buckets
    buckets = array of num_buckets empty lists
    
    # Distribute elements into buckets
    for i = 0 to n-1:
        bucket_idx = floor((arr[i] - min_val) * num_buckets / range)
        buckets[bucket_idx].append(arr[i])
    
    # Sort individual buckets (often with insertion sort)
    for i = 0 to num_buckets-1:
        sort(buckets[i])  # Any stable sorting algorithm
    
    # Concatenate buckets back into array
    index = 0
    for i = 0 to num_buckets-1:
        for each element in buckets[i]:
            arr[index++] = element
    
    return arr
```

## Implementation Notes
### Counting Sort
- Ensure the count array is large enough for the maximum value
- For objects with integer keys, modify to sort based on the key
- Can be optimized when the range starts from a non-zero minimum value
- Be careful with indexing when building the output array
- Consider using a direct addressing approach for small ranges

### Radix Sort
- Can use any stable sort for digit sorting (counting sort is common)
- LSD (Least Significant Digit) processes from right to left
- MSD (Most Significant Digit) processes from left to right and can use recursion
- Choose an appropriate radix (base) - larger radix means fewer passes but more space
- For strings, can sort character by character

### Bucket Sort
- Choose an appropriate number of buckets (often n or sqrt(n))
- Bucket size affects performance significantly
- Mapping function should distribute elements uniformly
- Can be combined with other sorting algorithms for hybrid approaches
- Consider using linked lists for buckets to avoid resizing

## Edge-case Checklist
### Counting Sort
- Empty array: Return as is
- Single element: Already sorted
- All elements equal: All placed in same count bucket
- Maximum value much larger than n: Inefficient space usage
- Negative numbers: Shift values to non-negative range

### Radix Sort
- Empty array: Return as is
- Single element: Already sorted
- Different length keys: Pad shorter keys or use special handling
- Leading zeros: Handle appropriately in digit extraction
- Negative numbers: Use special handling (e.g., separate pass or bit manipulation)

### Bucket Sort
- Empty array: Return as is
- Single element: Already sorted
- All elements equal: All placed in same bucket (worst case)
- Non-uniform distribution: Some buckets may be much larger than others
- Floating-point values: Requires appropriate mapping function

## Examples
### Counting Sort
- Input: [4, 2, 2, 8, 3, 3, 1]
- Count array: [0, 1, 2, 2, 1, 0, 0, 0, 1]
- Cumulative counts: [0, 1, 3, 5, 6, 6, 6, 6, 7]
- Output: [1, 2, 2, 3, 3, 4, 8]

### Radix Sort
- Input: [170, 45, 75, 90, 802, 24, 2, 66]
- After sorting by 1s digit: [170, 90, 2, 802, 24, 45, 75, 66]
- After sorting by 10s digit: [2, 802, 24, 45, 66, 170, 75, 90]
- After sorting by 100s digit: [2, 24, 45, 66, 75, 90, 170, 802]

### Bucket Sort
- Input: [0.42, 0.32, 0.33, 0.52, 0.37, 0.47, 0.51]
- Bucket distribution:
  - Bucket 0 [0.0-0.2): []
  - Bucket 1 [0.2-0.4): [0.32, 0.33, 0.37]
  - Bucket 2 [0.4-0.6): [0.42, 0.47, 0.52, 0.51]
  - Bucket 3 [0.6-0.8): []
  - Bucket 4 [0.8-1.0): []
- After sorting buckets: [0.32, 0.33, 0.37, 0.42, 0.47, 0.51, 0.52]

## Variants / Alternatives
### Counting Sort
- In-place Counting Sort: Uses the input array for counting (with limitations)
- Range Counting Sort: Optimized for a known range of values
- Parallel Counting Sort: Distributes counting across multiple processors

### Radix Sort
- MSD Radix Sort: Starts from most significant digit (can use recursion)
- Hybrid Radix Sort: Switches to insertion sort for small subarrays
- American Flag Sort: In-place variant of MSD radix sort
- String Radix Sort: Specialized for string keys

### Bucket Sort
- Histogram Sort: Uses a histogram to determine bucket sizes
- Proxmap Sort: Specialized variant for floating-point values
- Count Distribution Sort: Parallel variant of bucket sort

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapters 8.2-8.4
- Knuth, Donald E. "The Art of Computer Programming, Volume 3: Sorting and Searching"
- Sedgewick, Robert: "Algorithms, 4th Edition", Sections 5.1, 5.2
- Andersson, Arne: "General Balanced Trees" (1999) - for radix tree connections
