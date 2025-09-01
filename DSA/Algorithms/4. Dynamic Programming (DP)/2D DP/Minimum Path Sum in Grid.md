# Minimum Path Sum in Grid

## Name
- Category: Dynamic Programming (2D)
- Summary: Algorithm to find the path from top-left to bottom-right of a grid with minimum sum of values along the path, moving only right or down.

## Preconditions / Assumptions
- 2D grid of non-negative integers
- Can only move right or down at each step
- Goal is to minimize the sum of values along the path
- Must start at top-left (0,0) and end at bottom-right (m-1,n-1)

## Properties
- Deterministic
- Complete (finds optimal solution)
- Optimal substructure (solution built from solutions to overlapping subproblems)
- 2D dynamic programming approach
- Greedy approach doesn't work (local minimum doesn't guarantee global minimum)

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard DP | O(m*n) | O(m*n) | m, n are grid dimensions |
| Space-optimized | O(m*n) | O(min(m,n)) | Only need one row/column |
| With path reconstruction | O(m*n) | O(m*n) | For tracking the actual path |
| Recursive + Memo | O(m*n) | O(m*n) | Plus recursion stack |

## When to Use / When to Avoid
- Use if:
  - Need to find minimum cost path in grid
  - Movement is restricted to right and down
  - Grid values represent costs or weights
  - Need guaranteed optimal solution
- Avoid if:
  - Movement in all directions is allowed (use Dijkstra's algorithm)
  - Grid is very large (consider A* or other heuristic approaches)
  - Negative weights exist (may need Bellman-Ford)
  - Only need to find if a path exists (use BFS/DFS)

## Correctness (Proof Sketch)
- Base cases:
  - dp[0][0] = grid[0][0]
  - First row: can only come from left
  - First column: can only come from above
- Recurrence relation: dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])
  - At each cell, choose the minimum of coming from above or from left
- Optimal substructure: Minimum path to (i,j) built from minimum paths to (i-1,j) and (i,j-1)
- Termination: After filling the entire dp table, dp[m-1][n-1] contains the optimal solution
- Proof by induction: If dp[i'][j'] is correct for all (i',j') lexicographically smaller than (i,j), then dp[i][j] is correct

## Pseudocode (canonical)
```pseudo
function minPathSum(grid):
    m = grid.rows
    n = grid.columns
    
    # Create DP table
    dp = new table[0...m-1][0...n-1]
    
    # Base case: starting point
    dp[0][0] = grid[0][0]
    
    # Fill first row (can only come from left)
    for j = 1 to n-1:
        dp[0][j] = dp[0][j-1] + grid[0][j]
    
    # Fill first column (can only come from above)
    for i = 1 to m-1:
        dp[i][0] = dp[i-1][0] + grid[i][0]
    
    # Fill the rest of the DP table
    for i = 1 to m-1:
        for j = 1 to n-1:
            dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])
    
    return dp[m-1][n-1]

# Space-optimized version
function minPathSumOptimized(grid):
    m = grid.rows
    n = grid.columns
    
    # Use a single row for DP
    dp = new array[0...n-1]
    
    # Initialize with first row
    dp[0] = grid[0][0]
    for j = 1 to n-1:
        dp[j] = dp[j-1] + grid[0][j]
    
    # Process remaining rows
    for i = 1 to m-1:
        dp[0] += grid[i][0]  # First column special case
        
        for j = 1 to n-1:
            dp[j] = grid[i][j] + min(dp[j], dp[j-1])
    
    return dp[n-1]

# With path reconstruction
function minPathSumWithPath(grid):
    m = grid.rows
    n = grid.columns
    
    # Create DP table and path tracker
    dp = new table[0...m-1][0...n-1]
    path = new table[0...m-1][0...n-1]  # 0 for left, 1 for up
    
    # Base case
    dp[0][0] = grid[0][0]
    
    # Fill first row
    for j = 1 to n-1:
        dp[0][j] = dp[0][j-1] + grid[0][j]
        path[0][j] = 0  # came from left
    
    # Fill first column
    for i = 1 to m-1:
        dp[i][0] = dp[i-1][0] + grid[i][0]
        path[i][0] = 1  # came from up
    
    # Fill the rest
    for i = 1 to m-1:
        for j = 1 to n-1:
            if dp[i-1][j] <= dp[i][j-1]:
                dp[i][j] = grid[i][j] + dp[i-1][j]
                path[i][j] = 1  # came from up
            else:
                dp[i][j] = grid[i][j] + dp[i][j-1]
                path[i][j] = 0  # came from left
    
    # Reconstruct the path
    i = m-1
    j = n-1
    pathCoordinates = []
    pathCoordinates.add((i, j))
    
    while i > 0 or j > 0:
        if path[i][j] == 1:  # came from up
            i--
        else:  # came from left
            j--
        pathCoordinates.add((i, j))
    
    return dp[m-1][n-1], reverse(pathCoordinates)
```

## Implementation Notes
- Space optimization: Only need to store one row of the DP table at a time
- For path reconstruction, store decisions made at each cell
- Consider using memoization for recursive approach
- For very large grids, consider using A* search with Manhattan distance heuristic
- Can be extended to allow diagonal movement
- Handle edge cases like empty grid or single-cell grid
- Can be generalized to find maximum path sum by negating values and finding minimum

## Edge-case Checklist
- Empty grid: Return 0 or appropriate error
- Single-cell grid: Return the value of that cell
- First row or column with very large values: May lead to overflow
- All cells with equal values: Any path has the same sum
- Very large grid: Watch for time/space constraints
- Negative values: Algorithm still works, but problem definition may change

## Examples
- Minimal example:
  - Grid:
    ```
    [1, 3, 1]
    [1, 5, 1]
    [4, 2, 1]
    ```
  - DP table:
    ```
    [1,  4,  5]
    [2,  7,  6]
    [6,  8,  7]
    ```
  - Minimum path sum: 7
  - Path: (0,0) → (1,0) → (2,0) → (2,1) → (2,2)
  
- Edge case example:
  - 1x1 grid: [5]
  - Minimum path sum: 5

## Variants / Alternatives
- Maximum Path Sum: Find path with maximum sum
- Path with constraints: Must pass through certain cells
- Diagonal movement: Allow moving diagonally as well
- Variable start/end: Start and end positions not fixed
- Alternatives:
  - A* search: For very large grids with good heuristics
  - Dijkstra's algorithm: For more general graph problems
  - Bellman-Ford: If negative weights are allowed

## References
- LeetCode Problem #64: "Minimum Path Sum"
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Skiena, Steven: "The Algorithm Design Manual"
- Sedgewick, Wayne: "Algorithms, 4th Edition"
