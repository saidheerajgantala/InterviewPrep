# Sudoku Solver

## Name
- Category: Backtracking
- Summary: Algorithm to solve a Sudoku puzzle by systematically trying digits and backtracking when constraints are violated.

## Preconditions / Assumptions
- Standard 9×9 Sudoku grid (can be generalized to n²×n²)
- Input grid contains valid initial state (no constraint violations)
- Empty cells represented by a specific value (e.g., 0 or '.')
- Solution must satisfy Sudoku constraints: digits 1-9 in each row, column, and 3×3 box
- At least one valid solution exists

## Properties
- Deterministic
- Complete (finds solution if one exists)
- Exponential worst-case time complexity
- Backtracking approach (try, check, undo if invalid)
- Can be optimized with constraint propagation techniques

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Basic Backtracking | O(9^m) | O(9^m) | O(9^m) | O(n²) | m = number of empty cells |
| With Constraint Propagation | O(9^m) | O(9^m) | O(9^m) | O(n²) | Much faster in practice |
| Dancing Links (DLX) | O(9^m) | O(9^m) | O(9^m) | O(n⁴) | Efficient for exact cover |
| SAT Solver | O(9^m) | O(9^m) | O(9^m) | O(n⁴) | Converts to boolean satisfiability |

## When to Use / When to Avoid
- Use if:
  - Need to solve standard Sudoku puzzles
  - Complete solution required
  - Puzzle has reasonable number of givens (>17)
- Avoid if:
  - Need to generate Sudoku puzzles (different approach)
  - Extremely large Sudoku variants (e.g., 16×16, 25×25)
  - Need to find all possible solutions (modified approach)
  - Real-time constraints (may be too slow for very difficult puzzles)

## Correctness (Proof Sketch)
- Invariant: Partial solution maintains Sudoku constraints at each step
- Systematic exploration: Try all valid digits for each empty cell
- Backtracking: If no valid digit exists, undo previous choice and try next option
- Termination: Either find a valid solution or exhaust all possibilities
- Completeness: If a solution exists, it will be found by trying all valid combinations

## Pseudocode (canonical)
```pseudo
function solveSudoku(board):
    # Find an empty cell
    row, col = findEmptyCell(board)
    
    # If no empty cell, puzzle is solved
    if no empty cell found:
        return true
    
    # Try digits 1-9 for the empty cell
    for digit from 1 to 9:
        if isValid(board, row, col, digit):
            # Place the digit
            board[row][col] = digit
            
            # Recursively solve rest of the puzzle
            if solveSudoku(board):
                return true
            
            # If placing digit didn't lead to a solution, backtrack
            board[row][col] = EMPTY
    
    # No digit worked, trigger backtracking
    return false

function isValid(board, row, col, digit):
    # Check row
    for c = 0 to 8:
        if board[row][c] == digit:
            return false
    
    # Check column
    for r = 0 to 8:
        if board[r][col] == digit:
            return false
    
    # Check 3x3 box
    boxRow = 3 * (row / 3)  # Integer division
    boxCol = 3 * (col / 3)
    for r = boxRow to boxRow + 2:
        for c = boxCol to boxCol + 2:
            if board[r][c] == digit:
                return false
    
    return true
```

## Implementation Notes
- Optimize empty cell selection (e.g., choose cell with fewest valid options)
- Use bit manipulation for faster constraint checking
- Consider using constraint propagation techniques (e.g., naked/hidden singles)
- Pre-compute valid digits for each cell to reduce redundant checks
- For multiple solutions, modify to continue searching after finding one
- Consider iterative implementation to avoid stack overflow for large puzzles
- Use data structures to track valid digits per row, column, and box

## Edge-case Checklist
- Empty puzzle: Trivial solution (fill with valid pattern)
- Already solved puzzle: Return as is
- Invalid initial state: No solution possible
- Multiple solutions: Standard algorithm finds only one
- Very difficult puzzles: May take significant time
- Minimum givens (17 for standard Sudoku): Harder to solve
- Uniqueness: Not guaranteed by the algorithm (depends on input)

## Examples
- Minimal example:
  ```
  5 3 . | . 7 . | . . .
  6 . . | 1 9 5 | . . .
  . 9 8 | . . . | . 6 .
  ------+-------+------
  8 . . | . 6 . | . . 3
  4 . . | 8 . 3 | . . 1
  7 . . | . 2 . | . . 6
  ------+-------+------
  . 6 . | . . . | 2 8 .
  . . . | 4 1 9 | . . 5
  . . . | . 8 . | . 7 9
  ```
  
- Solving process:
  - Find empty cell (0,2)
  - Try digits 1-9, find 1 and 2 invalid
  - Place 4, continue to next empty cell
  - Eventually encounter invalid state, backtrack
  - Continue until solution found

## Variants / Alternatives
- Constraint Propagation: Naked singles, hidden singles, etc.
- Dancing Links (DLX): Efficient exact cover algorithm
- SAT Solver: Convert to boolean satisfiability problem
- Stochastic approaches: Simulated annealing, genetic algorithms
- Human-like solving techniques: Apply logical rules
- Alternatives:
  - Integer Programming formulation
  - Graph coloring approach
  - Neural network solvers (less reliable)

## References
- Knuth, Donald: "Dancing Links" paper for exact cover problems
- Norvig, Peter: "Solving Every Sudoku Puzzle" essay
- Russell, Stuart and Norvig, Peter: "Artificial Intelligence: A Modern Approach"
- Crook, J.F.: "A Pencil-and-Paper Algorithm for Solving Sudoku Puzzles"
