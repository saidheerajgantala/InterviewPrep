# 📝 Word Search

## **Problem Statement**

* Given an m x n grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.
* The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.
* Example 1:
  * Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
  * Output: true
  * Explanation: The word "ABCCED" can be constructed by starting at the top-left 'A', moving right to 'B', right to 'C', down to 'C', left to 'E', and down to 'D'.
* Example 2:
  * Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
  * Output: true
  * Explanation: The word "SEE" can be constructed by starting at the middle 'S', moving down to 'E', and right to 'E'.
* Example 3:
  * Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
  * Output: false
  * Explanation: The word "ABCB" cannot be constructed because the letter 'B' would be used more than once.
* Constraints:
  * m == board.length
  * n == board[i].length
  * 1 <= m, n <= 6
  * 1 <= word.length <= 15
  * `board` and `word` consists of only lowercase and uppercase English letters.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find if the given word exists in the grid
* One approach would be to try starting from each cell in the grid and check if the word can be formed by exploring adjacent cells
* We need to make sure we don't use the same cell more than once in a single path
* This is a classic backtracking problem

---

## **Step 2: Flow Steps (Brute Force)**

1. Iterate through each cell in the grid
2. For each cell, if it matches the first character of the word, start a depth-first search (DFS)
3. In the DFS, mark the current cell as visited, then recursively check adjacent cells for the next character
4. If we can form the entire word, return true
5. If we've checked all cells and couldn't form the word, return false

---

## **Step 3: Brute Force Implementation (Code)**

```python
def exist(board, word):
    if not board or not word:
        return False
    
    m, n = len(board), len(board[0])
    
    def dfs(i, j, k):
        # If we've matched all characters in the word
        if k == len(word):
            return True
        
        # Check if the current position is valid
        if i < 0 or i >= m or j < 0 or j >= n or board[i][j] != word[k]:
            return False
        
        # Mark the current cell as visited
        temp = board[i][j]
        board[i][j] = '#'
        
        # Check adjacent cells
        found = (dfs(i+1, j, k+1) or
                 dfs(i-1, j, k+1) or
                 dfs(i, j+1, k+1) or
                 dfs(i, j-1, k+1))
        
        # Restore the cell
        board[i][j] = temp
        
        return found
    
    # Try starting from each cell
    for i in range(m):
        for j in range(n):
            if dfs(i, j, 0):
                return True
    
    return False
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(m * n * 4^L) where m and n are the dimensions of the board and L is the length of the word. For each cell, we might need to explore all four directions for each character in the word.
* Space Complexity: O(L) for the recursion stack, where L is the length of the word.
* This approach is actually quite efficient for this problem, as we're using backtracking to prune the search space.

---

## **Step 5: Visualizing Redundant Computation**

* For the example board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED":
  * Start at (0,0) with 'A': matches the first character
    * Check (1,0) with 'S': doesn't match the second character 'B'
    * Check (0,1) with 'B': matches the second character
      * Check (1,1) with 'F': doesn't match the third character 'C'
      * Check (0,2) with 'C': matches the third character
        * Check (1,2) with 'C': matches the fourth character
          * Check (2,2) with 'E': doesn't match the fifth character 'E'
          * Check (1,3) with 'S': doesn't match the fifth character 'E'
          * Check (1,1) with 'F': already visited
          * Check (0,3) with 'E': matches the fifth character
            * Check (1,3) with 'S': doesn't match the sixth character 'D'
            * Check (0,2) with 'C': already visited
            * No valid path found
        * No valid path found
      * No valid path found
    * No valid path found
  * ...and so on
* There's no redundant computation in this approach, as we mark cells as visited to avoid revisiting them in the same path

---

## **Why are we using this algorithm?**

* For this problem, the backtracking approach is already quite efficient.
* The property that matches this problem is that we need to explore all possible paths to find if the word exists.
* By using backtracking, we can prune the search space by marking cells as visited and avoiding revisiting them in the same path.
* Alternatives would be less efficient or more complex.
* Precondition: We need to be able to modify the board to mark cells as visited.

---

## **Algo optimization**

While the backtracking approach is already efficient, we can make a few optimizations:

### **Step 1: Thinking Process (Optimized - Early Pruning)**

* We can add some early pruning to avoid unnecessary DFS calls
* Before starting the search, we can check if the board contains all the characters in the word
* We can also check if the first character of the word exists in the board
* These simple checks can save a lot of computation for cases where the word cannot possibly exist in the board

### **Step 2: Flow Steps (Optimized - Early Pruning)**

1. Check if the board contains all the characters in the word
2. Iterate through each cell in the grid
3. For each cell, if it matches the first character of the word, start a DFS
4. In the DFS, mark the current cell as visited, then recursively check adjacent cells for the next character
5. If we can form the entire word, return true
6. If we've checked all cells and couldn't form the word, return false

### **Step 3: Implementation (Optimized - Early Pruning)**

```python
def exist(board, word):
    if not board or not word:
        return False
    
    m, n = len(board), len(board[0])
    
    # Check if the board contains all characters in the word
    board_chars = {}
    for i in range(m):
        for j in range(n):
            board_chars[board[i][j]] = board_chars.get(board[i][j], 0) + 1
    
    word_chars = {}
    for char in word:
        word_chars[char] = word_chars.get(char, 0) + 1
    
    for char, count in word_chars.items():
        if char not in board_chars or board_chars[char] < count:
            return False
    
    def dfs(i, j, k):
        # If we've matched all characters in the word
        if k == len(word):
            return True
        
        # Check if the current position is valid
        if i < 0 or i >= m or j < 0 or j >= n or board[i][j] != word[k]:
            return False
        
        # Mark the current cell as visited
        temp = board[i][j]
        board[i][j] = '#'
        
        # Check adjacent cells
        found = (dfs(i+1, j, k+1) or
                 dfs(i-1, j, k+1) or
                 dfs(i, j+1, k+1) or
                 dfs(i, j-1, k+1))
        
        # Restore the cell
        board[i][j] = temp
        
        return found
    
    # Try starting from each cell
    for i in range(m):
        for j in range(n):
            if board[i][j] == word[0] and dfs(i, j, 0):
                return True
    
    return False
```

### **Another Optimization: Direction Array**

We can use a direction array to simplify the DFS code:

```python
def exist(board, word):
    if not board or not word:
        return False
    
    m, n = len(board), len(board[0])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # right, down, left, up
    
    def dfs(i, j, k):
        if k == len(word):
            return True
        
        if i < 0 or i >= m or j < 0 or j >= n or board[i][j] != word[k]:
            return False
        
        temp = board[i][j]
        board[i][j] = '#'
        
        for di, dj in directions:
            if dfs(i + di, j + dj, k + 1):
                board[i][j] = temp
                return True
        
        board[i][j] = temp
        return False
    
    for i in range(m):
        for j in range(n):
            if board[i][j] == word[0] and dfs(i, j, 0):
                return True
    
    return False
```

### **Step 4: Complexity Analysis (Optimized - Early Pruning)**

* Time Complexity: O(m * n * 4^L) in the worst case, but the early pruning can significantly reduce the number of DFS calls for cases where the word cannot possibly exist in the board.
* Space Complexity: O(L) for the recursion stack, plus O(m * n) for the character frequency maps.
* This approach is more efficient than the brute force approach for cases where the word cannot possibly exist in the board.

### **Step 5: Visualizing Computation (Optimized - Early Pruning)**

* For the example board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB":
  * Check if the board contains all characters in the word:
    * board_chars = {'A': 2, 'B': 1, 'C': 2, 'E': 3, 'S': 2, 'F': 1, 'D': 1}
    * word_chars = {'A': 1, 'B': 2, 'C': 1}
    * 'B' appears only once in the board but twice in the word, so return false immediately
* This early pruning avoids the need to perform any DFS calls for this example

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Backtracking) | O(m * n * 4^L) | O(L) | Efficient due to backtracking |
| Optimized (Early Pruning) | O(m * n * 4^L) | O(L + m * n) | More efficient with early pruning |

---

## **Final Takeaways**

* The key to efficiently solving the Word Search problem is to use backtracking with early pruning.
* By marking cells as visited during the DFS, we avoid revisiting them in the same path.
* By checking if the board contains all the characters in the word before starting the search, we can avoid unnecessary DFS calls.
* This problem demonstrates the power of backtracking for exploring all possible paths in a grid.

---

## **Real-life Analogy**

* Imagine you're trying to find a specific path through a maze. You start at a certain point and explore all possible directions. If you reach a dead end, you backtrack to the last intersection and try a different direction. This is similar to how the backtracking algorithm works in this problem, exploring all possible paths to find if the word exists in the grid.

---

## **Summary**

* We solved the Word Search problem by using a backtracking approach with early pruning, resulting in O(m * n * 4^L) time complexity in the worst case. The key insights are to mark cells as visited during the DFS to avoid revisiting them in the same path, and to check if the board contains all the characters in the word before starting the search. This approach efficiently explores all possible paths to find if the word exists in the grid. 