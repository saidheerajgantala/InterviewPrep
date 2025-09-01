# Word Search

## Name
- Category: Backtracking, Graph Search
- Summary: Algorithm to determine if a given word exists in a 2D grid of characters, where the word can be formed by connecting adjacent cells horizontally, vertically, or diagonally.

## Preconditions / Assumptions
- 2D grid of characters
- Word to search for is provided
- Valid path can move horizontally, vertically, or diagonally (depending on variant)
- Each cell can be used only once in a word
- Grid boundaries must be respected

## Properties
- Deterministic
- Complete (finds a solution if one exists)
- Uses backtracking to explore all possible paths
- May find multiple occurrences of the word
- Exponential time complexity in worst case
- Can be optimized with pruning techniques

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard (4-directional) | O(m*n*4^L) | O(L) | m×n grid, L = word length |
| 8-directional | O(m*n*8^L) | O(L) | Including diagonals |
| With pruning | O(m*n*4^L) | O(L) | Better average case |
| Trie-based | O(m*n*L) | O(T) | T = size of trie, for multiple words |

## When to Use / When to Avoid
- Use if:
  - Need to find a specific word in a character grid
  - Need to find multiple words (with trie optimization)
  - Grid is relatively small
  - Backtracking approach is suitable
  - Complete exploration is required
- Avoid if:
  - Grid is very large (exponential complexity)
  - Word is very long (exponential complexity)
  - Memory is severely constrained
  - Real-time solution is required for large inputs

## Correctness (Proof Sketch)
- Backtracking explores all possible paths from each starting cell
- At each step, the algorithm tries all valid moves (adjacent cells)
- If a move leads to a dead end, it backtracks and tries a different path
- Solution is found when the entire word is matched
- If no solution exists, all possible paths will be explored and rejected
- Marking visited cells prevents cycles
- Completeness follows from exhaustive exploration of all possible paths

## Pseudocode (canonical)
```pseudo
function exist(board, word):
    m = board.rows
    n = board.columns
    
    # Try each cell as a starting point
    for i = 0 to m-1:
        for j = 0 to n-1:
            if board[i][j] == word[0] and dfs(board, i, j, word, 0):
                return true
    
    return false

function dfs(board, i, j, word, index):
    # Base case: entire word has been matched
    if index == word.length:
        return true
    
    # Check boundaries and current character
    if i < 0 or i >= board.rows or j < 0 or j >= board.columns or 
       board[i][j] != word[index]:
        return false
    
    # Mark as visited by changing to a special character
    temp = board[i][j]
    board[i][j] = '#'
    
    # Try all four directions: up, right, down, left
    found = dfs(board, i-1, j, word, index+1) or
            dfs(board, i, j+1, word, index+1) or
            dfs(board, i+1, j, word, index+1) or
            dfs(board, i, j-1, word, index+1)
    
    # Restore the original character (backtrack)
    board[i][j] = temp
    
    return found

# Finding all occurrences
function findAllOccurrences(board, word):
    m = board.rows
    n = board.columns
    occurrences = []
    
    for i = 0 to m-1:
        for j = 0 to n-1:
            if board[i][j] == word[0]:
                path = []
                if dfsAllOccurrences(board, i, j, word, 0, path, occurrences):
                    # Found at least one occurrence starting at (i, j)
                    continue
    
    return occurrences

function dfsAllOccurrences(board, i, j, word, index, path, occurrences):
    # Base case: entire word has been matched
    if index == word.length:
        occurrences.add(copy of path)
        return true
    
    # Check boundaries and current character
    if i < 0 or i >= board.rows or j < 0 or j >= board.columns or 
       board[i][j] != word[index]:
        return false
    
    # Mark as visited
    temp = board[i][j]
    board[i][j] = '#'
    path.add((i, j))
    
    # Try all four directions
    found = false
    
    # Explore all directions even if one has already found a match
    found |= dfsAllOccurrences(board, i-1, j, word, index+1, path, occurrences)
    found |= dfsAllOccurrences(board, i, j+1, word, index+1, path, occurrences)
    found |= dfsAllOccurrences(board, i+1, j, word, index+1, path, occurrences)
    found |= dfsAllOccurrences(board, i, j-1, word, index+1, path, occurrences)
    
    # Backtrack
    board[i][j] = temp
    path.removeLast()
    
    return found
```

## Implementation Notes
- Use a temporary character (like '#') to mark visited cells
- For finding all occurrences, store each path separately
- Consider using a visited matrix instead of modifying the board
- For multiple words, use a trie data structure to share prefixes
- Optimize by checking if remaining letters exist in the board
- Prune paths that cannot lead to a solution
- Consider iterative implementation for very large grids
- Handle edge cases like empty grid or empty word
- For 8-directional search, add the four diagonal directions

## Edge-case Checklist
- Empty grid: Return false
- Empty word: Return true (by definition)
- Single character grid: Check if it matches the word
- Word longer than possible path in grid: Return false
- Word with characters not in grid: Return false early
- Multiple occurrences: Standard implementation finds one, can be modified to find all
- Case sensitivity: Define character matching appropriately

## Examples
- Minimal example:
  - Grid:
    ```
    [A, B, C, E]
    [S, F, C, S]
    [A, D, E, E]
    ```
  - Word: "ABCCED"
  - Path: (0,0) → (0,1) → (0,2) → (1,2) → (2,2) → (2,3)
  - Result: true
  
- No solution example:
  - Grid:
    ```
    [A, B, C, E]
    [S, F, C, S]
    [A, D, E, E]
    ```
  - Word: "ABCB"
  - Result: false (cannot reuse the same cell)

## Variants / Alternatives
- 8-directional search: Allow diagonal moves
- Finding all occurrences: Don't stop at first match
- Multiple words search: Use a trie to share prefixes
- Word break: Allow disconnected sequences
- Alternatives:
  - BFS: For shorter words or when memory is less constrained
  - Aho-Corasick algorithm: For multiple pattern matching
  - Dynamic programming: For specific grid structures
  - KMP algorithm: For linear pattern matching

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Sedgewick, Wayne: "Algorithms, 4th Edition"
- Skiena, Steven: "The Algorithm Design Manual"
- LeetCode Problem #79: "Word Search"
- LeetCode Problem #212: "Word Search II" (multiple words)
