# Binary Search on Answer

## Name
- Category: Divide and Conquer, Binary Search
- Summary: Technique that applies binary search to find the optimal value in a solution space, rather than searching for an element in an array, by leveraging monotonicity properties of the problem.

## Preconditions / Assumptions
- The solution space can be represented as a range of values
- There exists a boolean function that can determine if a value is a valid solution
- The boolean function exhibits monotonicity (all values above or below a certain threshold are valid/invalid)
- The range of possible solutions is bounded
- The optimal solution is within the defined range

## Properties
- Deterministic
- Complete (finds optimal solution if one exists)
- Logarithmic time complexity with respect to the solution range
- Requires a feasibility check function
- Can find maximum/minimum valid value
- Applicable to optimization problems with monotonic properties

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(log R * T) | O(1) | R = range size, T = time for feasibility check |
| Floating-point | O(log(R/ε) * T) | O(1) | ε = precision |
| Parallel | O(log R * T/P) | O(P) | P = number of processors |
| Multi-criteria | O(log R * T * k) | O(k) | k = number of criteria |

## When to Use / When to Avoid
- Use if:
  - Solution space is too large for linear search
  - Problem has monotonic property
  - Need to find optimal value in a range
  - Feasibility check is efficient
  - Problem can be reduced to "Can we achieve at least/at most X?"
- Avoid if:
  - No clear monotonicity in the solution space
  - Feasibility check is expensive
  - Solution space is small enough for linear search
  - More specialized algorithms exist for the problem
  - Precision requirements are very high for continuous domains

## Correctness (Proof Sketch)
- The solution space exhibits monotonicity: there exists a threshold T such that:
  - For minimization: all values ≥ T are invalid, all values < T are valid
  - For maximization: all values ≤ T are valid, all values > T are invalid
- Binary search maintains a range [low, high] that contains the threshold
- Each iteration halves the search space
- The feasibility check correctly classifies values as valid or invalid
- When low and high converge, we've found the threshold value
- Termination is guaranteed because the range size decreases by half each iteration

## Pseudocode (canonical)
```pseudo
# Finding maximum valid value
function binarySearchMaximum(low, high, isValid):
    # Initialize result to a value outside the range
    result = low - 1  # Or some appropriate invalid value
    
    while low <= high:
        mid = low + (high - low) / 2
        
        if isValid(mid):
            # mid is valid, so the result is at least mid
            result = mid
            low = mid + 1  # Look for larger valid values
        else:
            # mid is invalid, look for smaller values
            high = mid - 1
    
    return result

# Finding minimum valid value
function binarySearchMinimum(low, high, isValid):
    # Initialize result to a value outside the range
    result = high + 1  # Or some appropriate invalid value
    
    while low <= high:
        mid = low + (high - low) / 2
        
        if isValid(mid):
            # mid is valid, so the result is at most mid
            result = mid
            high = mid - 1  # Look for smaller valid values
        else:
            # mid is invalid, look for larger values
            low = mid + 1
    
    return result

# For floating-point values
function binarySearchFloat(low, high, isValid, epsilon):
    while high - low > epsilon:
        mid = low + (high - low) / 2
        
        if isValid(mid):
            low = mid  # For maximum valid value
            # Or: high = mid  # For minimum valid value
        else:
            high = mid  # For maximum valid value
            # Or: low = mid  # For minimum valid value
    
    return low  # Or high, depending on the problem
```

## Implementation Notes
- Carefully define the feasibility check function based on the problem
- For integer domains, use proper integer division and convergence checks
- For floating-point domains, define an appropriate precision (epsilon)
- Be careful with boundary conditions and edge cases
- Ensure monotonicity holds throughout the solution space
- Consider using parallel binary search for expensive feasibility checks
- For multi-criteria optimization, define a composite feasibility function
- Watch for overflow when computing mid = (low + high) / 2

## Edge-case Checklist
- Empty solution space: Return appropriate default value
- Single value in solution space: Check directly
- No valid solution exists: Return appropriate indicator
- All values are valid: Return boundary based on optimization direction
- Precision issues for floating-point: Use appropriate epsilon
- Integer overflow: Use low + (high - low) / 2 instead of (low + high) / 2
- Infinite loops: Ensure progress is made in each iteration

## Examples
### Capacity Allocation Problem
- Problem: Find the minimum capacity C such that all items can be allocated to at most K bins
- Solution space: [max item size, sum of all items]
- Feasibility check: Can all items fit into K bins of capacity C?
- Implementation:
  ```
  function canAllocate(items, bins, capacity):
      count = 1
      current_sum = 0
      
      for item in items:
          if item > capacity:
              return false
          
          if current_sum + item > capacity:
              count++
              current_sum = item
          else:
              current_sum += item
          
          if count > bins:
              return false
      
      return true
  
  result = binarySearchMinimum(max(items), sum(items), 
                              lambda c: canAllocate(items, K, c))
  ```

### Square Root Calculation
- Problem: Find the square root of N with precision ε
- Solution space: [0, N] (for N ≥ 1)
- Feasibility check: Is mid² ≤ N?
- Implementation:
  ```
  function sqrt(N, epsilon):
      if N < 0:
          return "Invalid input"
      if N == 0 or N == 1:
          return N
      
      low = 0
      high = N
      
      while high - low > epsilon:
          mid = low + (high - low) / 2
          
          if mid * mid <= N:
              low = mid
          else:
              high = mid
      
      return low
  ```

## Variants / Alternatives
- Ternary search: For unimodal functions (finding maximum/minimum)
- Parallel binary search: For expensive feasibility checks
- Exponential search: When bounds are not known
- Multi-criteria binary search: For problems with multiple constraints
- Alternatives:
  - Linear search: For small ranges
  - Gradient descent: For continuous optimization
  - Dynamic programming: For problems with optimal substructure
  - Specialized algorithms for specific problem domains

## References
- Bentley, Jon L.: "Programming Pearls", Column 4
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Competitive Programming resources (e.g., CP-Algorithms)
- "Handbook of Exact Algorithms for NP-Hard Problems"
- "Binary Search on Answer" technique in competitive programming contests
