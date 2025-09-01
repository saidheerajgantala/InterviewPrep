# Matrix Chain Multiplication

## Name
- Category: Dynamic Programming, Optimization
- Summary: Algorithm to determine the most efficient way to multiply a sequence of matrices, minimizing the total number of scalar multiplications by finding the optimal parenthesization.

## Preconditions / Assumptions
- Input is an array of matrix dimensions where matrices are A₁, A₂, ..., Aₙ
- Matrix Aᵢ has dimensions p[i-1] × p[i]
- Matrix multiplication is associative: (AB)C = A(BC)
- The goal is to minimize the number of scalar multiplications
- The order of matrices cannot be changed (only the parenthesization)
- All matrices can be multiplied (dimensions are compatible)

## Properties
- Deterministic
- Complete (finds the optimal solution)
- Time complexity: O(n³)
- Space complexity: O(n²)
- Bottom-up dynamic programming approach
- Solves the optimal parenthesization problem
- Can be extended to reconstruct the optimal parenthesization
- Applicable to other associative operations with varying costs

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard DP | O(n³) | O(n²) | n = number of matrices |
| Recursive with memoization | O(n³) | O(n²) | Top-down approach |
| Optimized DP | O(n³) | O(n²) | With various optimizations |
| Hu-Shing Algorithm | O(n log n) | O(n²) | Advanced, complex implementation |

## When to Use / When to Avoid
- Use if:
  - Need to find optimal order for matrix multiplication
  - Number of matrices is moderate (up to a few hundred)
  - Matrices have significantly different dimensions
  - Exact optimal solution is required
  - Similar optimization problems with associative operations
- Avoid if:
  - All matrices have similar dimensions (less optimization potential)
  - Number of matrices is very large (consider heuristics)
  - Approximation is acceptable (greedy approaches may be faster)
  - Memory is severely constrained
  - Real-time constraints require faster solutions

## Correctness (Proof Sketch)
- Define dp[i][j] as the minimum cost of multiplying matrices Aᵢ through Aⱼ
- Base case: dp[i][i] = 0 (single matrix requires no multiplications)
- For a chain Aᵢ to Aⱼ, try all possible split points k where i ≤ k < j
- Recurrence relation: dp[i][j] = min(dp[i][k] + dp[k+1][j] + p[i-1]*p[k]*p[j])
- The optimal solution builds upon optimal solutions to subproblems
- The algorithm considers all possible ways to parenthesize the chain
- Final answer is dp[1][n], representing the optimal cost for the entire chain
- Time complexity is O(n³) due to three nested loops (i, j, and k)

## Pseudocode (canonical)
```pseudo
function matrixChainMultiplication(p):
    # p is an array of matrix dimensions
    # Matrix i has dimensions p[i-1] x p[i]
    n = p.length - 1  # Number of matrices
    
    # Initialize DP table
    dp = new table of size n×n, initialized with infinity
    
    # Base case: cost of multiplying a single matrix is 0
    for i = 1 to n:
        dp[i][i] = 0
    
    # l is the chain length
    for l = 2 to n:
        # i is the start of the chain
        for i = 1 to n-l+1:
            # j is the end of the chain
            j = i + l - 1
            
            # Try all possible split points
            for k = i to j-1:
                # Calculate cost for this split
                cost = dp[i][k] + dp[k+1][j] + p[i-1] * p[k] * p[j]
                
                # Update if this is better than current best
                if cost < dp[i][j]:
                    dp[i][j] = cost
    
    return dp[1][n]  # Minimum cost for the entire chain

# With parenthesization reconstruction
function matrixChainWithParenthesis(p):
    n = p.length - 1
    
    # Initialize DP table and split points
    dp = new table of size n×n, initialized with infinity
    split = new table of size n×n, initialized with 0
    
    # Base case
    for i = 1 to n:
        dp[i][i] = 0
    
    for l = 2 to n:
        for i = 1 to n-l+1:
            j = i + l - 1
            
            for k = i to j-1:
                cost = dp[i][k] + dp[k+1][j] + p[i-1] * p[k] * p[j]
                
                if cost < dp[i][j]:
                    dp[i][j] = cost
                    split[i][j] = k  # Save the split point
    
    # Reconstruct the optimal parenthesization
    parenthesization = printOptimalParenthesis(split, 1, n)
    
    return dp[1][n], parenthesization

function printOptimalParenthesis(split, i, j):
    if i == j:
        return "A" + i
    else:
        k = split[i][j]
        left = printOptimalParenthesis(split, i, k)
        right = printOptimalParenthesis(split, k+1, j)
        return "(" + left + " × " + right + ")"
```

## Implementation Notes
- Use 1-indexed matrices for cleaner code (A₁ to Aₙ)
- Consider using a bottom-up approach to avoid stack overflow
- For large number of matrices, consider using a sparse table representation
- Track split points to reconstruct the optimal parenthesization
- Use appropriate data types to avoid overflow for large matrices
- Consider using logarithms for very large cost values
- Implement memoization for recursive approaches
- For special cases (e.g., all matrices same size), use simplified algorithms
- Be careful with indexing in the dimension array
- Consider parallel implementations for very large instances

## Edge-case Checklist
- Single matrix: Cost is 0 (no multiplications needed)
- Two matrices: Only one way to multiply
- Chain of identical matrices: Still need to compute optimal order
- Very large dimensions: Watch for integer overflow
- All matrices have the same dimensions: Still compute optimal order
- Dimensions with extreme differences: Handle numerical stability
- Empty chain: Return 0 or appropriate error
- Maximum chain length: Consider memory limitations

## Examples
- Minimal example:
  - Matrices: A₁(2×3), A₂(3×4), A₃(4×2)
  - Dimensions array p = [2, 3, 4, 2]
  - DP table:
    ```
      | A₁  A₂  A₃
    --+------------
    A₁ | 0   24  16
    A₂ | -   0   24
    A₃ | -   -   0
    ```
  - Optimal cost: 16 (multiplying (A₁×A₂)×A₃)
  - Parenthesization: ((A₁×A₂)×A₃)
  
- Complex example:
  - Matrices: A₁(5×4), A₂(4×6), A₃(6×2), A₄(2×7)
  - Dimensions array p = [5, 4, 6, 2, 7]
  - Optimal cost: 158
  - Parenthesization: (A₁×(A₂×(A₃×A₄)))

## Variants / Alternatives
- Recursive with memoization: Top-down approach
- Hu-Shing algorithm: More efficient but complex
- Parallel matrix chain multiplication: For distributed computing
- Approximate algorithms: For very large chains
- Chain multiplication with additional constraints
- Alternatives:
  - Greedy approaches: Not optimal but faster
  - Genetic algorithms: For very large instances
  - Strassen's algorithm: For the actual matrix multiplication
  - Convex optimization: For special cases

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Bellman, Richard: "Dynamic Programming" (1957)
- Hu, T.C. and Shing, M.T.: "Computation of Matrix Chain Products" (1982)
- Sedgewick, Robert and Wayne, Kevin: "Algorithms, 4th Edition"
- Aho, Alfred V., Hopcroft, John E., and Ullman, Jeffrey D.: "Data Structures and Algorithms"
