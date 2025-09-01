# Fibonacci Sequence (DP)

## Name
- Category: Dynamic Programming (1D)
- Summary: Efficiently compute the nth Fibonacci number by storing previously calculated values to avoid redundant computation.

## Preconditions / Assumptions
- Input is a non-negative integer n
- Output fits within numeric type limits (watch for overflow)
- F(0) = 0, F(1) = 1, F(n) = F(n-1) + F(n-2) for n > 1
- Zero-indexed (F(0)=0, F(1)=1, F(2)=1, F(3)=2, etc.)

## Properties
- Deterministic
- Complete (always finds the correct answer)
- Bottom-up or top-down approaches
- Linear time and space complexity (compared to exponential for naive recursion)

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Naive Recursion | O(2ⁿ) | O(2ⁿ) | O(2ⁿ) | O(n) | Exponential due to redundant calculations |
| Top-down (Memoization) | O(n) | O(n) | O(n) | O(n) | Stack space + memoization table |
| Bottom-up (Tabulation) | O(n) | O(n) | O(n) | O(n) | Only table space |
| Space-optimized | O(n) | O(n) | O(n) | O(1) | Only stores last two values |
| Matrix Exponentiation | O(log n) | O(log n) | O(log n) | O(1) | Advanced technique |

## When to Use / When to Avoid
- Use if:
  - Learning DP (classic introductory problem)
  - Need to compute Fibonacci numbers up to moderate n
  - Demonstrating memoization vs tabulation
- Avoid if:
  - Very large n (use matrix exponentiation or Binet's formula)
  - Need mathematical properties rather than specific values
  - Extremely constrained memory (use constant space version)

## Correctness (Proof Sketch)
- Base cases: F(0)=0, F(1)=1 are correctly defined
- Recurrence relation: F(n) = F(n-1) + F(n-2) directly implemented
- Memoization ensures each subproblem solved exactly once
- Bottom-up builds solutions from smaller to larger inputs
- Optimal substructure: Fibonacci value depends only on two previous values

## Pseudocode (canonical)
```pseudo
# Top-down (Memoization)
function fibonacci_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
        
    memo[n] = fibonacci_memo(n-1, memo) + fibonacci_memo(n-2, memo)
    return memo[n]

# Bottom-up (Tabulation)
function fibonacci_tab(n):
    if n <= 1:
        return n
        
    dp = new array of size (n+1)
    dp[0] = 0
    dp[1] = 1
    
    for i = 2 to n:
        dp[i] = dp[i-1] + dp[i-2]
        
    return dp[n]

# Space-optimized
function fibonacci_optimized(n):
    if n <= 1:
        return n
        
    a = 0
    b = 1
    
    for i = 2 to n:
        c = a + b
        a = b
        b = c
        
    return b
```

## Implementation Notes
- Watch for integer overflow with large n (use BigInteger or similar)
- Memoization is naturally recursive, may hit stack limits for large n
- Tabulation avoids recursion stack issues
- Space-optimized version only needs two variables (three during calculation)
- For extremely large n, use matrix exponentiation or Binet's formula

## Edge-case Checklist
- n = 0: Should return 0
- n = 1: Should return 1
- Large n: Watch for integer overflow
- Negative n: Not defined in standard Fibonacci (handle as error)

## Examples
- Minimal example:
  - Input: n = 6
  - Output: 8 (sequence: 0,1,1,2,3,5,8)
  
- Large example:
  - Input: n = 50
  - Output: 12586269025 (requires big integer in many languages)

## Variants / Alternatives
- Matrix Exponentiation: O(log n) time using matrix multiplication
- Binet's Formula: Direct calculation using golden ratio (floating point precision issues)
- Extended Fibonacci: F(-n) = (-1)^(n+1) * F(n)
- Tribonacci: F(n) = F(n-1) + F(n-2) + F(n-3)
- Fibonacci with different initial values
- Alternatives:
  - Closed-form solution (Binet's formula)
  - Fast doubling method

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter on Dynamic Programming
- Sedgewick, Wayne: "Algorithms, 4th Edition"
- Knuth, Donald: "The Art of Computer Programming, Volume 1", Section 1.2.8
