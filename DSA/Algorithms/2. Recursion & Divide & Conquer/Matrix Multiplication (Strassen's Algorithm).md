# Strassen's Matrix Multiplication Algorithm

## Name
- Category: Divide and Conquer
- Summary: Recursive algorithm for matrix multiplication that reduces the number of recursive multiplications from 8 to 7, achieving better asymptotic complexity than the naive approach.

## Preconditions / Assumptions
- Input matrices A and B are square with dimensions n×n
- n is a power of 2 (for simplicity; can be padded otherwise)
- Matrix operations (addition, subtraction) are defined
- Sufficient memory for recursive calls and temporary matrices

## Properties
- Deterministic
- Complete (always computes the correct result)
- Divide-and-conquer approach
- More efficient than naive multiplication for large matrices
- Numerically less stable than standard multiplication
- Higher constant factors and overhead than naive approach

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Naive | O(n³) | O(n²) | Standard matrix multiplication |
| Strassen's | O(n^log₂7) ≈ O(n^2.81) | O(n²) | 7 recursive multiplications |
| Optimized Strassen's | O(n^log₂7) | O(n²) | With base case optimization |
| Coppersmith-Winograd | O(n^2.376) | O(n²) | Theoretical improvement, impractical |
| Hybrid | O(n^log₂7) to O(n³) | O(n²) | Switches to naive for small matrices |

## When to Use / When to Avoid
- Use if:
  - Multiplying very large matrices
  - Theoretical efficiency is more important than numerical stability
  - Matrix dimensions are powers of 2 or padding overhead is acceptable
  - Memory usage is not a critical constraint
- Avoid if:
  - Matrices are small (n < 32-64 typically)
  - Numerical stability is critical
  - Memory is severely constrained
  - Optimized BLAS libraries are available

## Correctness (Proof Sketch)
- Divide each n×n matrix into four n/2 × n/2 submatrices
- Express result submatrices in terms of products and sums of input submatrices
- Rearrange to compute using only 7 recursive multiplications instead of 8
- Base case: Direct multiplication for small matrices
- Invariant: Each recursive call correctly computes the product for its submatrices
- Termination: Recursion depth is logarithmic in matrix size

## Pseudocode (canonical)
```pseudo
function strassenMultiply(A, B):
    n = A.rows  # Assume square matrix where n is a power of 2
    
    # Base case: small matrix
    if n <= CUTOFF:
        return naiveMultiply(A, B)
    
    # Divide matrices into quadrants
    n2 = n / 2
    A11, A12, A21, A22 = divideMatrix(A)
    B11, B12, B21, B22 = divideMatrix(B)
    
    # Compute 7 products with recursive calls
    P1 = strassenMultiply(A11 + A22, B11 + B22)
    P2 = strassenMultiply(A21 + A22, B11)
    P3 = strassenMultiply(A11, B12 - B22)
    P4 = strassenMultiply(A22, B21 - B11)
    P5 = strassenMultiply(A11 + A12, B22)
    P6 = strassenMultiply(A21 - A11, B11 + B12)
    P7 = strassenMultiply(A12 - A22, B21 + B22)
    
    # Compute result quadrants
    C11 = P1 + P4 - P5 + P7
    C12 = P3 + P5
    C21 = P2 + P4
    C22 = P1 - P2 + P3 + P6
    
    # Combine quadrants into result matrix
    return combineMatrix(C11, C12, C21, C22)

function divideMatrix(M):
    n = M.rows
    n2 = n / 2
    
    M11 = submatrix(M, 0, 0, n2, n2)
    M12 = submatrix(M, 0, n2, n2, n2)
    M21 = submatrix(M, n2, 0, n2, n2)
    M22 = submatrix(M, n2, n2, n2, n2)
    
    return M11, M12, M21, M22

function combineMatrix(M11, M12, M21, M22):
    n2 = M11.rows
    n = 2 * n2
    
    result = new matrix of size n×n
    
    for i = 0 to n2-1:
        for j = 0 to n2-1:
            result[i][j] = M11[i][j]
            result[i][j+n2] = M12[i][j]
            result[i+n2][j] = M21[i][j]
            result[i+n2][j+n2] = M22[i][j]
    
    return result
```

## Implementation Notes
- Use a hybrid approach: switch to naive multiplication for small matrices (typically n < 32-64)
- For non-power-of-2 dimensions, pad matrices with zeros
- Consider in-place operations to reduce memory usage
- Matrix addition and subtraction are O(n²) operations
- Optimize memory allocation by reusing temporary matrices
- For parallel implementation, each recursive call can be executed in parallel
- Consider using optimized BLAS libraries for base case operations
- Watch for numerical instability due to additional additions/subtractions

## Edge-case Checklist
- Empty matrices: Return empty result
- 1×1 matrices: Direct multiplication
- Non-power-of-2 dimensions: Pad with zeros
- Very large matrices: Watch for memory constraints
- Small matrices: Use naive multiplication
- Sparse matrices: Consider specialized sparse matrix algorithms instead

## Examples
- Minimal example:
  - A = [[1, 2], [3, 4]], B = [[5, 6], [7, 8]]
  - Naive result: C = [[19, 22], [43, 50]]
  - Strassen's approach:
    - P1 = (1+4)(5+8) = 5×13 = 65
    - P2 = (3+4)×5 = 7×5 = 35
    - P3 = 1×(6-8) = 1×(-2) = -2
    - P4 = 4×(7-5) = 4×2 = 8
    - P5 = (1+2)×8 = 3×8 = 24
    - P6 = (3-1)×(5+6) = 2×11 = 22
    - P7 = (2-4)×(7+8) = (-2)×15 = -30
    - C11 = P1 + P4 - P5 + P7 = 65 + 8 - 24 + (-30) = 19
    - C12 = P3 + P5 = -2 + 24 = 22
    - C21 = P2 + P4 = 35 + 8 = 43
    - C22 = P1 - P2 + P3 + P6 = 65 - 35 + (-2) + 22 = 50
    - C = [[19, 22], [43, 50]]

## Variants / Alternatives
- Winograd's variant: Reduces the number of additions/subtractions
- Coppersmith-Winograd algorithm: Theoretical improvement to O(n^2.376)
- Williams' algorithm: Further theoretical improvement to O(n^2.373)
- Hybrid Strassen-Winograd: Combines benefits of both approaches
- Alternatives:
  - Naive matrix multiplication: O(n³), more stable, simpler
  - Block matrix multiplication: Better cache performance
  - Cannon's algorithm: For distributed memory systems
  - BLAS libraries: Highly optimized implementations

## References
- Strassen, Volker. "Gaussian Elimination is not Optimal" (1969)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 4
- Golub, Gene H., and Charles F. Van Loan: "Matrix Computations"
- D'Alberto, Paolo, and Alexandru Nicolau: "Adaptive Strassen's Matrix Multiplication" (2007) 