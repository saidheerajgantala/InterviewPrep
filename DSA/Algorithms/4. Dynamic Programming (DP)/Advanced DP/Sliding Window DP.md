# Sliding Window DP

## Name
- Category: Dynamic Programming Optimization
- Summary: Technique that optimizes space complexity in dynamic programming by maintaining only the necessary states from previous iterations, typically when the current state depends only on a fixed window of previous states.

## Preconditions / Assumptions
- DP recurrence depends only on a fixed window of previous states
- Window size is typically much smaller than the total number of states
- Original DP solution requires excessive memory
- State transitions follow a regular pattern

## Properties
- Deterministic
- Complete (finds optimal solution)
- Reduces space complexity from O(n) or O(n²) to O(k) or O(n)
- Preserves time complexity of the original DP solution
- Particularly useful for problems with large input sizes
- Can be combined with other DP optimization techniques

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| 1D DP | O(n) | O(k) | k is window size |
| 2D DP (row) | O(n²) | O(k·n) | k rows needed |
| 2D DP (both) | O(n²) | O(k²) | k rows and columns |
| Circular buffer | O(n) | O(k) | Constant window size |

## When to Use / When to Avoid
- Use if:
  - Memory is a constraint
  - DP state depends only on a fixed window of previous states
  - Problem size is large
  - Original DP solution exceeds memory limits
- Avoid if:
  - Random access to all previous states is required
  - Window size is comparable to the total number of states
  - Memory is not a constraint
  - Implementation complexity outweighs the memory benefit

## Correctness (Proof Sketch)
- Original DP recurrence only depends on a fixed window of previous states
- Sliding window maintains exactly those states needed for future computations
- State transitions are preserved exactly as in the original DP
- Final answer is computed using the same logic as the original DP
- No information is lost by discarding states outside the window
- Correctness follows from the correctness of the original DP solution

## Pseudocode (canonical)
```pseudo
# 1D DP with Sliding Window (e.g., Fibonacci with constant space)
function fibonacciSlidingWindow(n):
    if n <= 1:
        return n
    
    a = 0  # fib(0)
    b = 1  # fib(1)
    
    for i = 2 to n:
        c = a + b  # fib(i) = fib(i-2) + fib(i-1)
        a = b      # Slide window: a becomes fib(i-1)
        b = c      # b becomes fib(i)
    
    return b

# 2D DP with Sliding Window (e.g., Edit Distance with O(min(m,n)) space)
function editDistanceSlidingWindow(s1, s2):
    m = s1.length
    n = s2.length
    
    # Ensure s1 is the shorter string to minimize space
    if m > n:
        swap(s1, s2)
        swap(m, n)
    
    # Use two rows instead of full matrix
    prev = new array of size m+1
    curr = new array of size m+1
    
    # Initialize first row (base case)
    for i = 0 to m:
        prev[i] = i
    
    # Fill the DP table row by row
    for j = 1 to n:
        curr[0] = j  # First column of current row
        
        for i = 1 to m:
            if s1[i-1] == s2[j-1]:
                curr[i] = prev[i-1]  # No operation needed
            else:
                curr[i] = 1 + min(prev[i-1],  # Substitution
                                  prev[i],    # Deletion
                                  curr[i-1])  # Insertion
        
        # Slide window: current row becomes previous row
        swap(prev, curr)
    
    return prev[m]

# Circular buffer implementation for sliding window
function slidingWindowWithCircularBuffer(n, k):
    # k is the window size
    buffer = new array of size k
    
    # Initialize buffer with base cases
    for i = 0 to min(k-1, n):
        buffer[i] = initialValue(i)
    
    for i = k to n:
        # Compute next state using values in the buffer
        next_value = computeNextState(buffer)
        
        # Update buffer (circular indexing)
        buffer[i % k] = next_value
    
    # Return final answer (may need to extract from buffer)
    return buffer[n % k]
```

## Implementation Notes
- Use modulo arithmetic for circular buffer implementation
- Be careful with index calculations when sliding the window
- For 2D DP, consider which dimension to optimize (usually the smaller one)
- When multiple previous states are needed, maintain exactly the necessary window
- Pay attention to initialization of the window
- For problems with complex state transitions, carefully map old indices to new ones
- Consider using double buffering (two arrays) for clearer code
- For some problems, a deque can be used instead of a fixed-size array

## Edge-case Checklist
- Empty input: Handle appropriately (often trivial solution)
- Input size smaller than window size: May need special handling
- Window size of 1: Simplifies to constant space
- Boundary conditions: Pay special attention to the first and last elements
- Initialization: Ensure base cases are correctly set
- Index calculations: Be careful with off-by-one errors
- Circular buffer wrap-around: Ensure proper modulo arithmetic

## Examples
### Fibonacci with Sliding Window
- Original DP: dp[i] = dp[i-1] + dp[i-2]
- Sliding Window:
  - Only need last two values
  - Space complexity reduced from O(n) to O(1)
  - Implementation:
    ```
    a = 0, b = 1
    for i = 2 to n:
        c = a + b
        a = b
        b = c
    return b
    ```

### Edit Distance with Sliding Window
- Original DP: dp[i][j] depends on dp[i-1][j], dp[i][j-1], and dp[i-1][j-1]
- Sliding Window:
  - Only need current and previous row
  - Space complexity reduced from O(m·n) to O(min(m,n))
  - Implementation: See pseudocode above

## Variants / Alternatives
- Circular buffer: Uses modulo arithmetic to reuse array space
- Double buffering: Maintains two arrays for current and previous states
- Rolling hash: Similar concept applied to string algorithms
- Sliding window with deque: For problems requiring min/max in a window
- Alternatives:
  - State compression: Different approach to reduce memory usage
  - Divide and conquer DP: For certain types of problems
  - Iterative DP: When full recursion tree is not needed
  - Memory-efficient backtracking: For reconstruction of solutions

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- "Dynamic Programming Optimizations" by Competitive Programming resources
- "Algorithm Design Manual" by Steven Skiena
- LeetCode Problem #72: "Edit Distance" (classic sliding window DP example)
- LeetCode Problem #198: "House Robber" (simple sliding window DP)
- "Competitive Programmer's Handbook" by Antti Laaksonen 