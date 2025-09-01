# State Compression DP

## Name
- Category: Dynamic Programming, Bitmasks
- Summary: Optimization technique that uses bitwise operations to efficiently represent and manipulate sets of states in dynamic programming problems, typically reducing space complexity and enabling solutions for problems with exponential state spaces.

## Preconditions / Assumptions
- Problem has a large but manageable state space
- States can be represented as bits in a bitmask
- Number of elements in the state is small enough for bitmask representation (typically n ≤ 20-30)
- Transitions between states follow a clear pattern
- Memory optimization is important

## Properties
- Deterministic
- Complete (finds optimal solution)
- Uses bitwise operations for efficiency
- Reduces space complexity by compact state representation
- Often used for NP-hard problems with small input size
- Can handle exponential state spaces more efficiently

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(n·2ⁿ) | O(2ⁿ) | n is number of elements |
| With additional state | O(k·n·2ⁿ) | O(k·2ⁿ) | k is size of additional state |
| Profile DP | O(n·2ⁿ) | O(2ⁿ) | For problems with adjacent row dependencies |
| Broken Profile DP | O(n·3ⁿ) | O(2ⁿ) | For certain tiling problems |

## When to Use / When to Avoid
- Use if:
  - Problem has exponential state space but small n
  - States can be naturally represented as presence/absence of elements
  - Memory optimization is critical
  - Problem involves subsets, permutations, or combinations
- Avoid if:
  - Number of elements is too large (n > 30)
  - States cannot be efficiently encoded as bits
  - Problem has more efficient specialized algorithms
  - Time complexity is more critical than space complexity

## Correctness (Proof Sketch)
- Bitmask representation preserves all necessary information about states
- Bitwise operations correctly implement state transitions
- Base cases are correctly handled
- All possible states are explored
- Optimal substructure property ensures correct solutions
- Memoization prevents redundant computation
- Final answer corresponds to the desired state in the DP table

## Pseudocode (canonical)
```pseudo
# Traveling Salesman Problem using State Compression DP
function tsp(dist):
    n = number of cities
    # dp[mask][i] = min cost to visit cities in mask and end at city i
    dp = new table[0...2^n-1][0...n-1], initialized with infinity
    
    # Base case: start at city 0
    dp[1][0] = 0  # Mask 1 represents only city 0 visited
    
    # Iterate through all possible subsets of cities
    for mask = 1 to 2^n - 1:
        for end = 0 to n-1:
            # Skip if end city is not in the mask
            if (mask & (1 << end)) == 0:
                continue
            
            # Try all possible previous cities
            prev_mask = mask ^ (1 << end)  # Remove end from mask
            if prev_mask == 0:  # No previous cities
                continue
                
            for prev = 0 to n-1:
                if (prev_mask & (1 << prev)) != 0:  # prev city is in prev_mask
                    dp[mask][end] = min(dp[mask][end], 
                                       dp[prev_mask][prev] + dist[prev][end])
    
    # Find minimum cost to return to city 0
    result = infinity
    for end = 1 to n-1:
        result = min(result, dp[(1 << n) - 1][end] + dist[end][0])
    
    return result

# Subset Sum using State Compression
function subsetSum(nums, target):
    n = nums.length
    # dp[mask] = true if subset represented by mask sums to target
    dp = new array of size 2^n, initialized with false
    
    # Base case: empty subset sums to 0
    dp[0] = (target == 0)
    
    # Compute subset sums
    subset_sum = new array of size 2^n, initialized with 0
    for mask = 1 to 2^n - 1:
        # Get the last set bit
        last_bit = mask & -mask
        last_index = log2(last_bit)
        
        # Remove last bit and add its value
        prev_mask = mask ^ last_bit
        subset_sum[mask] = subset_sum[prev_mask] + nums[last_index]
        
        # Check if this subset sums to target
        dp[mask] = (subset_sum[mask] == target)
    
    # Find any subset that sums to target
    for mask = 1 to 2^n - 1:
        if dp[mask]:
            return true
    
    return false

# Minimum Vertex Cover using State Compression
function minVertexCover(graph):
    n = number of vertices
    # dp[mask] = min size of vertex cover for induced subgraph
    dp = new array of size 2^n, initialized with infinity
    
    # Base case: empty graph needs 0 vertices
    dp[0] = 0
    
    # Process all possible subgraph states
    for mask = 1 to 2^n - 1:
        # Check if current set of vertices forms a valid vertex cover
        valid = true
        for u = 0 to n-1:
            for v = 0 to n-1:
                if graph[u][v] == 1:  # Edge exists
                    # If neither u nor v is in the mask, not a valid cover
                    if ((mask & (1 << u)) == 0) and ((mask & (1 << v)) == 0):
                        valid = false
                        break
            if not valid:
                break
        
        if valid:
            # Count number of vertices in the cover
            count = 0
            for i = 0 to n-1:
                if (mask & (1 << i)) != 0:
                    count++
            dp[mask] = count
    
    return dp[(1 << n) - 1]  # Return result for full graph
```

## Implementation Notes
- Use bit manipulation operations for efficient state transitions
- Consider using lookup tables for common bit operations
- Memoize results to avoid redundant computation
- Be careful with bit indexing (0-indexed vs. 1-indexed)
- Use iterative DP when possible to avoid stack overflow
- For large states, consider using hash maps instead of arrays
- Watch for integer overflow in bitmask operations
- For problems with many states, consider pruning impossible states
- Use __builtin_popcount (C++) or Integer.bitCount (Java) for counting set bits

## Edge-case Checklist
- Empty input: Handle appropriately (often trivial solution)
- Single element: Often has simple direct solution
- All elements must be included/excluded: Check if simplified solution exists
- Maximum number of elements (n ≈ 30): Watch for integer overflow
- Unreachable states: May need special handling
- Initialization of base cases: Critical for correctness
- Boundary conditions in state transitions

## Examples
### Traveling Salesman Problem
- Input: 4 cities with distance matrix:
  ```
  [0, 10, 15, 20]
  [10, 0, 35, 25]
  [15, 35, 0, 30]
  [20, 25, 30, 0]
  ```
- States: dp[mask][end] = minimum cost to visit cities in mask and end at city 'end'
- Computation:
  - dp[0001][0] = 0 (start at city 0)
  - dp[0011][1] = dp[0001][0] + dist[0][1] = 0 + 10 = 10
  - dp[0101][2] = dp[0001][0] + dist[0][2] = 0 + 15 = 15
  - dp[1001][3] = dp[0001][0] + dist[0][3] = 0 + 20 = 20
  - ...
  - dp[1111][1] = min(dp[1101][0] + dist[0][1], dp[1011][2] + dist[2][1], dp[0111][3] + dist[3][1]) = min(...)
  - Final result: min(dp[1111][1] + dist[1][0], dp[1111][2] + dist[2][0], dp[1111][3] + dist[3][0])

### Subset Sum
- Input: nums = [3, 34, 4, 12, 5, 2], target = 9
- States: dp[mask] = true if subset represented by mask sums to target
- Computation:
  - dp[000000] = (target == 0) = false
  - dp[000001] = (subset_sum[000001] == target) = (3 == 9) = false
  - dp[000010] = (subset_sum[000010] == target) = (34 == 9) = false
  - ...
  - dp[001001] = (subset_sum[001001] == target) = (3 + 4 + 2 == 9) = true
  - Result: true (subset {3, 4, 2} sums to 9)

## Variants / Alternatives
- Profile DP: For problems with adjacent row dependencies
- Broken Profile DP: For tiling problems
- Multi-dimensional State Compression: Using multiple bitmasks
- SOS (Sum over Subsets) DP: For computing functions over all subsets
- Alternatives:
  - Meet in the middle: For larger state spaces
  - Branch and bound: For pruning impossible states
  - Integer linear programming: For certain optimization problems
  - Specialized algorithms for specific problems

## References
- Competitive Programming resources (e.g., CP-Algorithms)
- "Competitive Programmer's Handbook" by Antti Laaksonen
- "Algorithms" by Robert Sedgewick and Kevin Wayne
- "Dynamic Programming for Coding Interviews" by Meenakshi and Kamal Rawat
- CSES Problem Set: "Dynamic Programming" section
- LeetCode Problem #1349: "Maximum Students Taking Exam" (state compression example) 