# Climbing Stairs

## Name
- Category: Dynamic Programming, Combinatorics
- Summary: Algorithm to count the number of distinct ways to climb a staircase with n steps, where you can climb 1 or 2 steps at a time, using dynamic programming to efficiently build the solution from smaller subproblems.

## Preconditions / Assumptions
- Input is a non-negative integer n representing the number of steps
- You can climb either 1 or 2 steps at a time
- The goal is to reach the top (nth step)
- Count the total number of distinct ways to reach the top
- The result fits within the range of a standard integer data type

## Properties
- Deterministic
- Complete (finds the exact number of ways)
- Time complexity: O(n)
- Space complexity: O(n), can be optimized to O(1)
- Bottom-up dynamic programming approach
- Equivalent to computing the (n+1)th Fibonacci number
- Can be extended to allow different step sizes
- Can be solved using matrix exponentiation in O(log n) time

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| DP (Bottom-up) | O(n) | O(n) | Standard tabulation |
| DP (Optimized) | O(n) | O(1) | Using only two variables |
| Recursive | O(2ⁿ) | O(n) | Without memoization (inefficient) |
| Memoized Recursive | O(n) | O(n) | Top-down with memoization |
| Matrix Exponentiation | O(log n) | O(1) | Advanced technique |
| Combinatorial | O(n) | O(1) | Using mathematical formula |

## When to Use / When to Avoid
- Use if:
  - Need to count ways to reach a target with specific increments
  - Problem can be broken down into overlapping subproblems
  - Input size is moderate (up to millions)
  - Problem follows the Fibonacci pattern
  - Need an exact count rather than just one solution
- Avoid if:
  - Need to enumerate all possible paths (not just count)
  - Input is extremely large (consider mathematical formula)
  - Problem has complex constraints beyond simple step choices
  - Memory is severely constrained and mathematical formula is applicable
  - Need to find the actual paths, not just the count

## Correctness (Proof Sketch)
- Define dp[i] as the number of ways to reach step i
- Base cases: dp[0] = 1 (one way to be at the starting position), dp[1] = 1 (one way to reach step 1)
- For each step i (2 ≤ i ≤ n), we can reach it by:
  - Taking a single step from step i-1, contributing dp[i-1] ways
  - Taking a double step from step i-2, contributing dp[i-2] ways
- Therefore, dp[i] = dp[i-1] + dp[i-2]
- This is exactly the Fibonacci recurrence relation
- The answer is dp[n], which represents all possible ways to reach the top
- Time complexity is O(n) as we compute each value once
- Space complexity is O(n) for the DP array, but can be optimized to O(1)

## Pseudocode (canonical)
```pseudo
# Bottom-up DP approach
function climbStairs(n):
    if n <= 1:
        return 1
    
    # Initialize DP array
    dp = new array of size n+1
    
    # Base cases
    dp[0] = 1
    dp[1] = 1
    
    # Fill the DP array
    for i = 2 to n:
        dp[i] = dp[i-1] + dp[i-2]
    
    return dp[n]

# Space-optimized approach
function climbStairsOptimized(n):
    if n <= 1:
        return 1
    
    # Initialize variables for the last two steps
    prev = 1  # Ways to reach step 0
    curr = 1  # Ways to reach step 1
    
    # Compute ways for each step
    for i = 2 to n:
        next = prev + curr
        prev = curr
        curr = next
    
    return curr

# Recursive approach with memoization
function climbStairsMemoized(n):
    memo = new array of size n+1, initialized with -1
    
    function climb(i):
        if i <= 1:
            return 1
        
        if memo[i] != -1:
            return memo[i]
        
        memo[i] = climb(i-1) + climb(i-2)
        return memo[i]
    
    return climb(n)

# Matrix exponentiation approach
function climbStairsMatrix(n):
    if n <= 1:
        return 1
    
    # Define the transformation matrix
    A = [[1, 1], [1, 0]]
    
    # Compute A^(n-1)
    result = matrixPower(A, n)
    
    # The answer is in the first row
    return result[0][0] + result[0][1]

function matrixPower(A, n):
    # Base case
    if n == 1:
        return A
    
    # Recursive case
    if n % 2 == 0:
        half = matrixPower(A, n/2)
        return matrixMultiply(half, half)
    else:
        half = matrixPower(A, (n-1)/2)
        return matrixMultiply(A, matrixMultiply(half, half))

function matrixMultiply(A, B):
    C = new 2×2 matrix
    C[0][0] = A[0][0]*B[0][0] + A[0][1]*B[1][0]
    C[0][1] = A[0][0]*B[0][1] + A[0][1]*B[1][1]
    C[1][0] = A[1][0]*B[0][0] + A[1][1]*B[1][0]
    C[1][1] = A[1][0]*B[0][1] + A[1][1]*B[1][1]
    return C
```

## Implementation Notes
- For small values of n, use the space-optimized approach
- For very large values of n, use matrix exponentiation or the mathematical formula
- Consider using modular arithmetic for large results
- Be careful with integer overflow for large n
- The problem can be generalized to allow k different step sizes
- For the generalized version, the recurrence becomes dp[i] = dp[i-1] + dp[i-2] + ... + dp[i-k]
- Consider using a sliding window approach for the generalized version
- For very large n, consider using Binet's formula (closed-form solution for Fibonacci numbers)
- The matrix exponentiation approach can be optimized with fast exponentiation

## Edge-case Checklist
- n = 0: Return 1 (one way to be at the starting position)
- n = 1: Return 1 (one way to climb 1 step)
- n = 2: Return 2 (two ways: 1+1 or 2)
- Large n: Watch for integer overflow
- Negative n: Not valid, handle appropriately (throw error or return 0)
- Decimal n: Not applicable to this problem (steps are discrete)

## Examples
- Minimal example:
  - n = 3
  - Ways:
    1. 1 step + 1 step + 1 step
    2. 1 step + 2 steps
    3. 2 steps + 1 step
  - DP array: [1, 1, 2, 3]
  - Result: 3
  
- Complex example:
  - n = 5
  - Ways:
    1. 1+1+1+1+1
    2. 1+1+1+2
    3. 1+1+2+1
    4. 1+2+1+1
    5. 2+1+1+1
    6. 1+2+2
    7. 2+1+2
    8. 2+2+1
  - DP array: [1, 1, 2, 3, 5, 8]
  - Result: 8

## Variants / Alternatives
- Climbing stairs with k different step sizes
- Minimum cost climbing stairs (each step has a cost)
- Distinct ways to reach a target sum
- Coin change problem (number of ways to make change)
- Paid staircase (different cost for different steps)
- Alternatives:
  - Combinatorial approach: Using binomial coefficients
  - Mathematical formula: Closed-form solution
  - State machine approach: Modeling transitions
  - Recursion with memoization: Top-down approach

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- LeetCode Problem #70: "Climbing Stairs"
- Fibonacci sequence in mathematics literature
- Sedgewick, Robert and Wayne, Kevin: "Algorithms, 4th Edition"
- Knuth, Donald: "The Art of Computer Programming, Volume 1: Fundamental Algorithms"
