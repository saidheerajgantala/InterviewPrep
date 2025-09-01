# Rat in a Maze

## Name
- Category: Backtracking
- Summary: Algorithm to find a path for a rat from a source to a destination in a maze, represented as a binary matrix where 1 indicates open paths and 0 indicates blocked paths.

## Preconditions / Assumptions
- Maze is represented as a binary matrix (0 for blocked, 1 for open)
- Rat starts at the top-left corner (0,0)
- Destination is the bottom-right corner (n-1, n-1)
- Rat can move in four directions: up, down, left, right
- A valid path consists of open cells only
- Goal is to find any valid path (or all paths)

## Properties
- Deterministic
- Complete (finds a solution if one exists)
- Uses backtracking to explore all possible paths
- May find multiple solutions
- Exponential time complexity in worst case
- Can be adapted to find shortest path

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(4^(n²)) | O(n²) | Worst case: all cells open |
| Finding all paths | O(4^(n²)) | O(n²) | Explores all possible paths |
| Finding shortest path | O(n²) | O(n²) | Using BFS instead of backtracking |
| With pruning | O(4^(n²)) | O(n²) | Better average case |

## When to Use / When to Avoid
- Use if:
  - Need to find any path in a maze
  - Need to find all possible paths
  - Maze is relatively small
  - Backtracking approach is suitable
  - Complete exploration is required
- Avoid if:
  - Maze is very large (exponential complexity)
  - Only shortest path is needed (use BFS instead)
  - Memory is severely constrained
  - Real-time solution is required

## Correctness (Proof Sketch)
- Backtracking explores all possible paths from source to destination
- At each step, the algorithm tries all valid moves (up, down, left, right)
- If a move leads to a dead end, it backtracks and tries a different path
- Solution is found when the destination is reached
- If no solution exists, all possible paths will be explored and rejected
- Marking visited cells prevents cycles
- Completeness follows from exhaustive exploration of all possible paths

## Pseudocode (canonical)
```pseudo
function solveMaze(maze):
    n = maze.length
    
    # Solution matrix to store the path
    solution = new n×n matrix, initialized with 0
    
    # Mark start position
    solution[0][0] = 1
    
    if solveMazeUtil(maze, 0, 0, solution, n):
        return solution
    else:
        return "No solution exists"

function solveMazeUtil(maze, x, y, solution, n):
    # Check if (x, y) is the destination
    if x == n-1 and y == n-1:
        return true
    
    # Define possible moves: right, down, left, up
    dx = [0, 1, 0, -1]
    dy = [1, 0, -1, 0]
    
    for i = 0 to 3:
        next_x = x + dx[i]
        next_y = y + dy[i]
        
        # Check if the move is valid
        if isSafe(maze, next_x, next_y, n, solution):
            # Mark the cell as part of the solution
            solution[next_x][next_y] = 1
            
            # Recur for the next move
            if solveMazeUtil(maze, next_x, next_y, solution, n):
                return true
            
            # If the move doesn't lead to a solution, backtrack
            solution[next_x][next_y] = 0
    
    return false

function isSafe(maze, x, y, n, solution):
    # Check if (x, y) is a valid and unvisited cell
    return (x >= 0 and x < n and 
            y >= 0 and y < n and 
            maze[x][y] == 1 and 
            solution[x][y] == 0)

# Finding all paths
function printAllPaths(maze):
    n = maze.length
    
    # Solution matrix to store the current path
    solution = new n×n matrix, initialized with 0
    
    # Mark start position
    solution[0][0] = 1
    
    # List to store all solutions
    all_paths = []
    
    findAllPaths(maze, 0, 0, solution, n, all_paths, "")
    
    return all_paths

function findAllPaths(maze, x, y, solution, n, all_paths, current_path):
    # Check if (x, y) is the destination
    if x == n-1 and y == n-1:
        all_paths.add(current_path)
        return
    
    # Define possible moves: right, down, left, up
    dx = [0, 1, 0, -1]
    dy = [1, 0, -1, 0]
    move_chars = ['R', 'D', 'L', 'U']
    
    for i = 0 to 3:
        next_x = x + dx[i]
        next_y = y + dy[i]
        
        # Check if the move is valid
        if isSafe(maze, next_x, next_y, n, solution):
            # Mark the cell as part of the solution
            solution[next_x][next_y] = 1
            
            # Recur for the next move
            findAllPaths(maze, next_x, next_y, solution, n, all_paths, 
                         current_path + move_chars[i])
            
            # Backtrack
            solution[next_x][next_y] = 0
```

## Implementation Notes
- Use a solution matrix to track the path
- For finding all paths, store each path as a string of moves (e.g., "RDLDR")
- Consider using a queue for BFS implementation to find shortest path
- For large mazes, use iterative implementation to avoid stack overflow
- Optimize by pruning paths that cannot lead to a solution
- Consider using A* search for finding optimal paths
- Handle edge cases like empty maze or single cell maze
- For visualization, use different values in solution matrix for different paths

## Edge-case Checklist
- Empty maze: Handle appropriately
- Single cell maze: Trivial solution
- No valid path exists: Return appropriate indicator
- Multiple valid paths: Standard implementation finds one, can be modified to find all
- Start or destination blocked: No solution exists
- Maze with only one valid path: Solution is unique
- Maze with many valid paths: Consider performance implications

## Examples
- Minimal example:
  - Maze:
    ```
    [1, 0, 0, 0]
    [1, 1, 0, 1]
    [0, 1, 0, 0]
    [1, 1, 1, 1]
    ```
  - Solution path:
    ```
    [1, 0, 0, 0]
    [1, 1, 0, 0]
    [0, 1, 0, 0]
    [0, 1, 1, 1]
    ```
  - Path representation: "DRRD"
  
- No solution example:
  - Maze:
    ```
    [1, 0, 0, 0]
    [0, 0, 0, 1]
    [0, 1, 0, 0]
    [0, 1, 1, 1]
    ```
  - No path exists from (0,0) to (3,3)

## Variants / Alternatives
- Finding all possible paths: Explore all paths without stopping at first solution
- Finding shortest path: Use BFS instead of backtracking
- Weighted maze: Cells have different costs, find minimum cost path
- Multiple destinations: Find paths to multiple targets
- Alternatives:
  - BFS: For shortest path in unweighted maze
  - Dijkstra's algorithm: For weighted maze
  - A* search: For optimal path with heuristic
  - Dynamic programming: For specific maze structures

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Sedgewick, Wayne: "Algorithms, 4th Edition"
- Skiena, Steven: "The Algorithm Design Manual"
- Backtracking algorithms in competitive programming resources
- GeeksforGeeks: "Rat in a Maze" problem
