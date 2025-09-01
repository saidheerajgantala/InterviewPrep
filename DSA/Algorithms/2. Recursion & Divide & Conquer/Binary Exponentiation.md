# Binary Exponentiation

## Name
- Category: Math, Divide and Conquer
- Summary: Efficient algorithm to compute x^n using the binary representation of the exponent, reducing the number of multiplications from O(n) to O(log n).

## Preconditions / Assumptions
- Base x can be any value that supports multiplication
- Exponent n is a non-negative integer
- Multiplication operation is defined for the type of x
- Result fits within the numeric type limits

## Properties
- Deterministic
- Complete (always computes the correct result)
- Logarithmic time complexity
- Works with any algebraic structure with associative multiplication
- Can be extended to modular exponentiation

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Naive | O(n) | O(1) | n multiplications |
| Binary (Recursive) | O(log n) | O(log n) | Stack space |
| Binary (Iterative) | O(log n) | O(1) | No recursion overhead |
| Montgomery | O(log n) | O(1) | Optimized for modular exponentiation |
| Exponentiation by squaring | O(log n) | O(1) | Same as binary exponentiation |

## When to Use / When to Avoid
- Use if:
  - Need to compute large powers efficiently
  - Exponent is large
  - Working with modular arithmetic
  - Computing matrix exponentiation
- Avoid if:
  - Exponent is very small (naive approach may be simpler)
  - Special properties allow for more efficient computation
  - Numerical stability is a concern (consider logarithms)

## Correctness (Proof Sketch)
- Based on the binary representation of exponent n
- For each bit in n, we either multiply by the current power or skip
- Invariant: At each step, we maintain the correct partial result
- For recursive approach: x^n = (x^(n/2))² if n is even, x * (x^(n/2))² if n is odd
- For iterative approach: We accumulate result while processing bits of n
- Termination: We process all bits of n (log n bits)

## Pseudocode (canonical)
```pseudo
# Recursive binary exponentiation
function binpow_recursive(x, n):
    if n == 0:
        return 1
    
    half = binpow_recursive(x, n / 2)  # Integer division
    
    if n % 2 == 0:
        return half * half
    else:
        return half * half * x

# Iterative binary exponentiation
function binpow_iterative(x, n):
    result = 1
    
    while n > 0:
        # If current bit is 1, multiply result by current power of x
        if n % 2 == 1:
            result = result * x
        
        # Square the current power of x
        x = x * x
        
        # Move to next bit
        n = n / 2  # Integer division
    
    return result

# Modular binary exponentiation
function modpow(x, n, m):
    result = 1
    x = x % m
    
    while n > 0:
        if n % 2 == 1:
            result = (result * x) % m
        
        x = (x * x) % m
        n = n / 2  # Integer division
    
    return result
```

## Implementation Notes
- Use the iterative version to avoid stack overflow for large exponents
- Be careful with integer overflow in intermediate calculations
- For modular exponentiation, apply modulo after each multiplication
- For matrix exponentiation, replace scalar multiplication with matrix multiplication
- Can be optimized by using bitwise operations (n & 1 for n % 2, n >> 1 for n / 2)
- Consider using Montgomery multiplication for frequent modular operations
- For floating-point bases with very large exponents, consider using logarithms

## Edge-case Checklist
- x = 0, n = 0: Mathematically undefined, typically return 1 by convention
- x = 0, n > 0: Return 0
- x = 1, any n: Return 1
- n = 0, x ≠ 0: Return 1
- n = 1, any x: Return x
- Negative exponents: Not directly handled, requires reciprocal
- Very large exponents: Watch for overflow in result
- Modular exponentiation: Ensure intermediate results stay within range

## Examples
- Minimal example:
  - Compute 3^13
  - Binary representation of 13: 1101
  - Trace:
    - Initialize result = 1, x = 3
    - Bit 1: result = 1 * 3 = 3, x = 3 * 3 = 9
    - Bit 0: result = 3, x = 9 * 9 = 81
    - Bit 1: result = 3 * 81 = 243, x = 81 * 81 = 6561
    - Bit 1: result = 243 * 6561 = 1,594,323
  - Result: 3^13 = 1,594,323
  
- Modular example:
  - Compute 7^23 mod 10
  - Each step applies modulo 10
  - Result: 7^23 mod 10 = 7 (last digit cycles in pattern)

## Variants / Alternatives
- Right-to-left binary exponentiation: Process bits from LSB to MSB
- Left-to-right binary exponentiation: Process bits from MSB to LSB
- Montgomery modular multiplication: More efficient for repeated modular operations
- Sliding window exponentiation: Processes multiple bits at once
- Alternatives:
  - Exponentiation by squaring: Another name for the same algorithm
  - Addition chain exponentiation: Finds optimal sequence of multiplications
  - Fast Fourier Transform: For specific exponentiation patterns

## References
- Knuth, Donald E. "The Art of Computer Programming, Volume 2: Seminumerical Algorithms"
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Handbook of Applied Cryptography, Chapter 14 (Efficient Implementation)
- CP-Algorithms: "Binary Exponentiation" (https://cp-algorithms.com/algebra/binary-exp.html)
