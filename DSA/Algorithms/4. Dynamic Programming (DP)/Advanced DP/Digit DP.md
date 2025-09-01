# Digit DP

## Name
- Category: Dynamic Programming, Combinatorics
- Summary: Specialized dynamic programming technique for counting numbers with specific digit-related properties within a given range [L, R].

## Preconditions / Assumptions
- Working with a range of integers [L, R]
- Need to count numbers with specific digit properties
- Constraints typically allow for digit-by-digit processing
- Range can be very large (up to 10^18 or more)

## Properties
- Deterministic
- Complete (finds exact count)
- Processes digits from most significant to least significant
- Uses states to track constraints and properties
- Avoids enumerating all numbers in the range
- Typically has logarithmic complexity in terms of range size

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(log(R) * 10 * states) | O(log(R) * states) | states depends on problem |
| Memoized | O(log(R) * 10 * states) | O(log(R) * states) | Top-down approach |
| Bottom-up | O(log(R) * 10 * states) | O(log(R) * states) | Iterative approach |
| Multi-constraint | O(log(R) * 10^k * states) | O(log(R) * states) | k constraints |

## When to Use / When to Avoid
- Use if:
  - Counting numbers with digit constraints in large ranges
  - Problem involves digit properties (sum, product, specific digits)
  - Direct enumeration would be too slow
  - Range boundaries are important
- Avoid if:
  - Problem doesn't involve digit properties
  - Range is small enough for direct enumeration
  - More efficient mathematical formula exists
  - Memory constraints are severe

## Correctness (Proof Sketch)
- Break down the problem into digit-by-digit subproblems
- Use states to track necessary information (position, tight bound, etc.)
- Base case: When all digits are processed, check if constraints are satisfied
- Recurrence relation: For each possible digit at current position, count valid numbers
- Handle tight bounds correctly (numbers must be ≤ R and ≥ L)
- Termination: After processing all digits, we have the total count

## Pseudocode (canonical)
```pseudo
# Top-down memoized approach for counting numbers in range [L, R] with digit sum = target
function digitDP(R, target):
    digits = convert R to array of digits
    memo = new table initialized with -1
    
    # Count numbers from 0 to R with digit sum = target
    count_R = solve(0, 0, true, digits, target, memo)
    
    # For range [L, R], compute count for [0, L-1] and subtract
    L = L - 1  # Adjust to get [0, L-1]
    digits_L = convert L to array of digits
    memo = new table initialized with -1
    count_L = solve(0, 0, true, digits_L, target, memo)
    
    return count_R - count_L

# Core recursive function
# pos: current position, sum: running digit sum
# tight: whether current number is constrained by upper bound
# digits: digits of the upper bound
function solve(pos, sum, tight, digits, target, memo):
    # Base case: all digits processed
    if pos == digits.length:
        return sum == target ? 1 : 0
    
    # Check if already computed
    if memo[pos][sum][tight] != -1:
        return memo[pos][sum][tight]
    
    result = 0
    
    # Determine the limit for current digit
    limit = tight ? digits[pos] : 9
    
    # Try all possible digits for current position
    for digit = 0 to limit:
        # Calculate new sum and tight flag for next position
        new_sum = sum + digit
        new_tight = tight && (digit == digits[pos])
        
        # Recursive call for next position
        result += solve(pos + 1, new_sum, new_tight, digits, target, memo)
    
    # Memoize and return
    memo[pos][sum][tight] = result
    return result

# Handling range [L, R] directly
function countInRange(L, R, target):
    # For simplicity, assume L ≤ R
    digits_R = convert R to array of digits
    digits_L = convert L to array of digits
    
    memo_R = new table initialized with -1
    memo_L = new table initialized with -1
    
    # Count numbers from 0 to R with digit sum = target
    count_R = solve(0, 0, true, digits_R, target, memo_R)
    
    # Count numbers from 0 to L-1 with digit sum = target
    L = L - 1  # Adjust to get [0, L-1]
    count_L = 0
    if L >= 0:  # Check if L-1 is valid
        digits_L = convert L to array of digits
        count_L = solve(0, 0, true, digits_L, target, memo_L)
    
    return count_R - count_L
```

## Implementation Notes
- Use memoization to avoid recomputing overlapping subproblems
- Carefully handle the tight bound constraints
- For range [L, R], compute count for [0, R] and [0, L-1] separately
- Leading zeros may need special handling depending on the problem
- State representation depends on the specific constraints
- Consider using bottom-up DP for better cache locality
- For multiple constraints, combine them in the state representation
- Watch for integer overflow in calculations

## Edge-case Checklist
- Empty range (L > R): Return 0
- Single number range (L = R): Direct check
- Leading zeros: May need special handling
- Negative numbers: Convert to positive or handle separately
- Very large ranges: Watch for integer overflow
- No valid numbers in range: Return 0
- All numbers in range valid: Use mathematical shortcut

## Examples
- Minimal example:
  - Problem: Count numbers in range [1, 100] with digit sum = 5
  - Approach:
    - For R = 100: digits = [1, 0, 0]
    - For L-1 = 0: digits = [0]
    - Count for [0, 100] = 19 (numbers: 5, 14, 23, 32, 41, 50, etc.)
    - Count for [0, 0] = 0 (no numbers with digit sum 5)
    - Result = 19 - 0 = 19
  
- Detailed example:
  - Problem: Count numbers in range [1, 50] with even digit sum
  - Process for R = 50:
    - pos=0, tight=true, limit=5: try digits 0-5
    - For digit=0: solve(1, 0, false, ...)
    - For digit=1: solve(1, 1, false, ...)
    - ...
    - For digit=5: solve(1, 5, true, ...)
    - At pos=1, similar process for each branch
    - Count numbers with even digit sum in [0, 50]
  - Similar process for L-1 = 0
  - Subtract to get result for [1, 50]

## Variants / Alternatives
- Multi-constraint Digit DP: Tracking multiple properties simultaneously
- Digit DP with state compression: Reducing state space
- Digit DP for non-decimal bases: Binary, hexadecimal, etc.
- Digit DP with additional operations: Products, GCDs, etc.
- Alternatives:
  - Mathematical formulas for specific constraints
  - Generating functions for certain digit properties
  - Direct enumeration for small ranges
  - Pattern recognition for special cases

## References
- Competitive Programming resources (e.g., CP-Algorithms)
- "Dynamic Programming for Coding Interviews" by Meenakshi and Kamal Rawat
- CSES Problem Set: "Digit Queries" and similar problems
- LeetCode Problem #233: "Number of Digit One"
- LeetCode Problem #902: "Numbers At Most N Given Digit Set"
