# House Robber

## Name
- Category: Dynamic Programming (1D)
- Summary: Algorithm to determine the maximum amount that can be robbed from a row of houses, where adjacent houses cannot be robbed due to security systems.

## Preconditions / Assumptions
- Array of non-negative integers representing money in each house
- Cannot rob adjacent houses (will trigger alarm)
- Goal is to maximize the total amount robbed
- No circular arrangement (houses in a straight line)

## Properties
- Deterministic
- Complete (finds optimal solution)
- Optimal substructure (optimal solution contains optimal solutions to subproblems)
- Overlapping subproblems (same subproblems solved multiple times)
- Linear time and space complexity

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Basic DP | O(n) | O(n) | n is number of houses |
| Space-optimized | O(n) | O(1) | Only need two previous states |
| Recursive + Memo | O(n) | O(n) | Plus recursion stack |
| Circular arrangement | O(n) | O(n) | House Robber II variant |

## When to Use / When to Avoid
- Use if:
  - Need to select non-adjacent elements to maximize sum
  - Problem can be modeled as a sequence of decisions
  - Optimal substructure exists
  - Simple, linear DP approach is sufficient
- Avoid if:
  - Additional constraints exist (beyond non-adjacency)
  - Houses are arranged in a cycle (need House Robber II variant)
  - Memory is extremely constrained (though O(1) space solution exists)
  - Problem requires path reconstruction (need additional tracking)

## Correctness (Proof Sketch)
- Base cases: 
  - With 0 houses: maximum profit is 0
  - With 1 house: maximum profit is the value of that house
- Recurrence relation: dp[i] = max(dp[i-1], dp[i-2] + nums[i])
  - Either skip current house and take previous best
  - Or rob current house and take best from two houses back
- Optimal substructure: Best solution for i houses uses best solutions for smaller subproblems
- Termination: After processing all houses, dp[n-1] contains the optimal solution
- Proof by induction: If dp[j] is correct for all j < i, then dp[i] is correct

## Pseudocode (canonical)
```pseudo
function rob(nums):
    n = nums.length
    
    # Handle edge cases
    if n == 0:
        return 0
    if n == 1:
        return nums[0]
    
    # Initialize DP array
    dp = new array of size n
    
    # Base cases
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])
    
    # Fill DP array
    for i = 2 to n-1:
        dp[i] = max(dp[i-1], dp[i-2] + nums[i])
    
    return dp[n-1]

# Space-optimized version
function robOptimized(nums):
    n = nums.length
    
    if n == 0:
        return 0
    if n == 1:
        return nums[0]
    
    # Only need two previous states
    prev2 = nums[0]
    prev1 = max(nums[0], nums[1])
    
    for i = 2 to n-1:
        current = max(prev1, prev2 + nums[i])
        prev2 = prev1
        prev1 = current
    
    return prev1
```

## Implementation Notes
- Use space-optimized version to reduce memory usage
- For very large inputs, watch for integer overflow
- Can be extended to handle circular arrangement (House Robber II)
- For path reconstruction, keep track of decisions made
- Recursive approach with memoization is an alternative
- Handle edge cases (empty array, single house) separately
- Can be generalized to allow robbing k houses apart instead of just non-adjacent

## Edge-case Checklist
- Empty array: Return 0
- Single house: Return its value
- Two houses: Return maximum of the two
- All houses with equal value: Take every other house
- Alternating high/low values: Take all high-value houses
- Decreasing/increasing sequence: Requires careful analysis

## Examples
- Minimal example:
  - Houses: [1, 2, 3, 1]
  - DP array: [1, 2, 4, 4]
  - Result: 4 (rob houses at positions 0 and 2)
  
- Step-by-step example:
  - Houses: [2, 7, 9, 3, 1]
  - dp[0] = 2
  - dp[1] = max(2, 7) = 7
  - dp[2] = max(7, 2+9) = max(7, 11) = 11
  - dp[3] = max(11, 7+3) = max(11, 10) = 11
  - dp[4] = max(11, 11+1) = max(11, 12) = 12
  - Result: 12 (rob houses at positions 2 and 4)

## Variants / Alternatives
- House Robber II: Houses arranged in a circle (first and last are adjacent)
- House Robber III: Houses arranged in a binary tree
- Maximum sum of non-adjacent elements: Same problem, different name
- Alternatives:
  - Recursive approach with memoization
  - Greedy approach (doesn't work for all cases)
  - State machine approach (two states: rob or skip)

## References
- LeetCode Problem #198: "House Robber"
- LeetCode Problem #213: "House Robber II" (circular arrangement)
- LeetCode Problem #337: "House Robber III" (tree arrangement)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter on Dynamic Programming
- Skiena, Steven: "The Algorithm Design Manual", Chapter on Dynamic Programming
