# Factorial Algorithm

## Name
- Category: Recursion, Basic Math
- Summary: Algorithm to compute n! (the product of all positive integers less than or equal to n), demonstrating both recursive and iterative approaches.

## Preconditions / Assumptions
- Input n is a non-negative integer
- Result fits within the numeric type limits
- Multiplication operation is defined and efficient

## Properties
- Deterministic
- Complete (always computes the correct result)
- Linear time complexity
- Can be implemented recursively or iteratively
- Result grows very quickly (faster than exponential)

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Recursive | O(n) | O(n) | Stack space |
| Iterative | O(n) | O(1) | Constant extra space |
| Memoized | O(n) | O(n) | Caches previous results |
| Stirling's Approximation | O(1) | O(1) | Approximate for large n |

## When to Use / When to Avoid
- Use if:
  - Need exact factorial value for small n
  - Teaching recursion concepts
  - Building combinatorial algorithms
  - Computing permutations or combinations
- Avoid if:
  - n is very large (consider approximations)
  - Need high performance (consider lookup tables)
  - Limited stack space (avoid recursion)
  - Numerical precision is critical (consider logarithmic methods)

## Correctness (Proof Sketch)
- Definition: n! = n × (n-1) × (n-2) × ... × 2 × 1
- Base case: 0! = 1 (by convention)
- Recursive relation: n! = n × (n-1)!
- Invariant: At each step, we maintain the product of integers processed so far
- Termination: Recursion depth or iteration count is bounded by n

## Pseudocode (canonical)
```pseudo
# Recursive factorial
function factorial_recursive(n):
    if n == 0:
        return 1
    else:
        return n * factorial_recursive(n - 1)

# Iterative factorial
function factorial_iterative(n):
    result = 1
    for i = 2 to n:
        result = result * i
    return result

# Memoized factorial
function factorial_memoized(n, memo={}):
    if n in memo:
        return memo[n]
    if n == 0:
        return 1
    memo[n] = n * factorial_memoized(n - 1, memo)
    return memo[n]
```

## Implementation Notes
- Use iterative version to avoid stack overflow for large n
- Be careful with integer overflow; consider using arbitrary precision arithmetic
- For repeated calls, use memoization or a lookup table
- For very large n, consider using Stirling's approximation
- In many languages, factorial is available in standard libraries
- For numerical stability with large n, consider computing log(n!) and then exponentiating
- Watch for special cases like 0! = 1

## Edge-case Checklist
- n = 0: Return 1 (mathematical definition)
- n = 1: Return 1
- Negative n: Undefined (typically raise error)
- Large n: Watch for overflow
- Fractional n: Requires gamma function (not standard factorial)

## Examples
- Minimal example:
  - factorial(5) = 5 × 4 × 3 × 2 × 1 = 120
  
- Trace of recursive execution:
  - factorial(4) = 4 × factorial(3) = 4 × 6 = 24
  - factorial(3) = 3 × factorial(2) = 3 × 2 = 6
  - factorial(2) = 2 × factorial(1) = 2 × 1 = 2
  - factorial(1) = 1 × factorial(0) = 1 × 1 = 1
  - factorial(0) = 1 (base case)

## Variants / Alternatives
- Tail-recursive factorial: Optimized for languages with tail-call optimization
- Table lookup: Precompute factorials for small n
- Stirling's approximation: n! ≈ sqrt(2πn) × (n/e)^n for large n
- Gamma function: Extension to non-integer arguments
- Logarithmic factorial: Compute sum of logarithms to avoid overflow
- Alternatives:
  - Recursive vs. iterative implementations
  - Memoization for repeated calculations
  - Arbitrary precision arithmetic libraries

## References
- Knuth, Donald E. "The Art of Computer Programming, Volume 1: Fundamental Algorithms"
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Stirling, James: "Methodus Differentialis" (1730) for Stirling's approximation
- OEIS A000142: Factorial numbers sequence
