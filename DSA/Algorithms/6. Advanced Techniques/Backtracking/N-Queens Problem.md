# N-Queens Problem

## Name
- Category: Backtracking
- Summary: Place N queens on an N×N chessboard such that no two queens threaten each other (no shared row, column, or diagonal).

## Preconditions / Assumptions
- Board is N×N
- N queens must be placed
- Standard chess queen movement rules apply
- Solutions exist for all N ≥ 4 (and N=1)
- N=2 and N=3 have no solutions

## Properties
- Deterministic
- Complete (finds all solutions if they exist)
- Exponential time complexity (NP-hard problem)
- Symmetry in solutions (rotations and reflections)

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Naive Backtracking | O(N!) | O(N!) | O(N!) | O(N) | Checking all queen positions |
| Optimized Backtracking | O(N!) | O(N!) | O(N!) | O(N) | With conflict sets |
| Bitwise Solution | O(N!) | O(N!) | O(N!) | O(N) | Faster conflict checking |
| Branch and Bound | O(N!) | O(N!) | O(N!) | O(N) | Pruning with heuristics |

## When to Use / When to Avoid
- Use if:
  - Need all solutions to the N-Queens problem
  - Teaching backtracking concepts
  - N is reasonably small (typically N ≤ 15)
- Avoid if:
  - N is very large (exponential growth becomes prohibitive)
  - Only need to know if a solution exists (specialized algorithms exist)
  - Need only one solution (simpler algorithms available)

## Correctness (Proof Sketch)
- Backtracking ensures systematic exploration of solution space
- Row and column constraints enforced by placing one queen per row/column
- Diagonal constraints checked explicitly
- Completeness: All valid configurations are eventually explored
- Termination: Finite search space with clear backtracking conditions

## Pseudocode (canonical)
```pseudo
function solveNQueens(n):
    solutions = []
    board = new array of size n×n filled with '.'
    
    function backtrack(row):
        if row == n:
            # Found a solution
            solutions.add(copy of board)
            return
        
        for col = 0 to n-1:
            if isValid(row, col):
                # Place queen
                board[row][col] = 'Q'
                
                # Recurse to next row
                backtrack(row + 1)
                
                # Remove queen (backtrack)
                board[row][col] = '.'
    
    function isValid(row, col):
        # Check column
        for i = 0 to row-1:
            if board[i][col] == 'Q':
                return false
        
        # Check upper-left diagonal
        for i, j = row-1, col-1; i >= 0 and j >= 0; i--, j--:
            if board[i][j] == 'Q':
                return false
        
        # Check upper-right diagonal
        for i, j = row-1, col+1; i >= 0 and j < n; i--, j++:
            if board[i][j] == 'Q':
                return false
        
        return true
    
    backtrack(0)
    return solutions
```

## Implementation Notes
- Optimize conflict checking with sets or bit manipulation
- Place one queen per row to reduce search space
- Use symmetry to reduce computation (for certain values of N)
- Consider iterative implementation for very large N to avoid stack overflow
- Pre-compute diagonal indices for faster checking
- Can use integers to represent board state instead of 2D array

## Edge-case Checklist
- N = 1: Trivial solution (single queen)
- N = 2, N = 3: No solutions exist
- N = 4: First non-trivial case, has 2 solutions
- Large N: Watch for stack overflow in recursive implementation
- Memory constraints: Consider compact board representation

## Examples
- Minimal example:
  - Input: N = 4
  - Output: Two solutions
    1. [".Q..", "...Q", "Q...", "..Q."]
    2. ["..Q.", "Q...", "...Q", ".Q.."]
  
- Larger example:
  - N = 8: 92 distinct solutions (12 if accounting for symmetry)

## Variants / Alternatives
- Count-only variant: Just count solutions without storing them
- First-solution variant: Stop after finding one solution
- K-Queens: Place K queens on an N×N board (K ≤ N)
- N-Queens Completion: Some queens already placed
- N-Rooks: Simpler variant (only row/column constraints)
- N-Bishops, N-Knights: Different movement rules
- Alternatives:
  - Constraint satisfaction solvers
  - Genetic algorithms for approximate solutions
  - Las Vegas algorithm for random solution

## References
- Backtracking Algorithms in MCPL by Wirth
- "Artificial Intelligence: A Modern Approach" by Russell and Norvig
- "The Art of Computer Programming, Volume 4" by Donald Knuth
- OEIS A000170 for number of solutions by board size
