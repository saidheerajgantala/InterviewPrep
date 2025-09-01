# 📝 Longest Increasing Path in a Matrix

## **Problem Statement**

* Given an m x n integers matrix, return the length of the longest increasing path in the matrix.
* From each cell, you can either move in four directions: left, right, up, or down. You may not move diagonally or move outside the boundary (i.e., wrap-around is not allowed).
* Example 1:
  * Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
  * Output: 4
  * Explanation: The longest increasing path is [1, 2, 6, 9].
* Example 2:
  * Input: matrix = [[3,4,5],[3,2,6],[2,2,1]]
  * Output: 4
  * Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
* Example 3:
  * Input: matrix = [[1]]
  * Output: 1
* Constraints:
  * m == matrix.length
  * n == matrix[i].length
  * 1 <= m, n <= 200
  * 0 <= matrix[i][j] <= 2^31 - 1

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the length of the longest increasing path in the matrix
* From each cell, we can move in four directions: left, right, up, or down
* One approach would be to use depth-first search (DFS) from each cell to find the longest path
* For each cell, we can try all four directions and continue the path if the next cell has a larger value
* We need to keep track of the maximum path length found so far

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a variable max_path to store the maximum path length found so far
2. For each cell (i, j) in the matrix:
   1. Perform DFS from this cell to find the longest increasing path:
      1. Initialize a path length of 1 (for the current cell)
      2. For each of the four directions (up, down, left, right):
         1. If the next cell is within bounds and has a larger value:
            1. Recursively perform DFS from the next cell
            2. Update the path length if a longer path is found
      3. Update max_path if the current path is longer
3. Return max_path

---

## **Step 3: Brute Force Implementation (Code)**

```python
def longestIncreasingPath(matrix):
    if not matrix:
        return 0
    
    m, n = len(matrix), len(matrix[0])
    max_path = 0
    
    def dfs(i, j):
        # Initialize path length for the current cell
        path_length = 1
        
        # Try all four directions
        for di, dj in [(0, 1), (1, 0), (0, -1), (-1, 0)]:
            ni, nj = i + di, j + dj
            
            # Check if the next cell is valid and has a larger value
            if 0 <= ni < m and 0 <= nj < n and matrix[ni][nj] > matrix[i][j]:
                path_length = max(path_length, 1 + dfs(ni, nj))
        
        return path_length
    
    # Try DFS from each cell
    for i in range(m):
        for j in range(n):
            max_path = max(max_path, dfs(i, j))
    
    return max_path
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^(m+n)) where m is the number of rows and n is the number of columns. In the worst case, we explore all possible paths, which is exponential.
* Space Complexity: O(m*n) for the recursion stack.
* This approach is inefficient for large matrices due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For matrix = [[9,9,4],[6,6,8],[2,1,1]]:
  * dfs(0, 0): matrix[0][0] = 9
    * No valid next cells (all neighbors are smaller or out of bounds)
    * Return 1
  * dfs(0, 1): matrix[0][1] = 9
    * No valid next cells
    * Return 1
  * dfs(0, 2): matrix[0][2] = 4
    * No valid next cells
    * Return 1
  * dfs(1, 0): matrix[1][0] = 6
    * Valid next cell: (0, 0) with value 9
      * dfs(0, 0) = 1 (already computed)
    * Return 1 + 1 = 2
  * ... (many more recursive calls)
  * dfs(2, 1): matrix[2][1] = 1
    * Valid next cell: (1, 1) with value 6
      * dfs(1, 1) = ... (many recursive calls)
    * Return 1 + ... = ?
* There's redundant computation in the recursive calls, especially for overlapping subproblems like dfs(0, 0)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming with memoization to avoid redundant computations.
* The property that matches this problem is that the longest increasing path from a cell depends only on the longest increasing paths from its valid neighbors.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently calculate the longest increasing path from each cell.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming with Memoization)**

* We can use dynamic programming with memoization to avoid redundant computations
* Let memo[i][j] represent the length of the longest increasing path starting from cell (i, j)
* We'll use DFS with memoization to compute memo[i][j] for each cell
* For each cell, we'll try all four directions and continue the path if the next cell has a larger value
* If we've already computed the longest path from a cell, we'll reuse that result instead of recomputing it

### **Step 2: Flow Steps (Optimized - Dynamic Programming with Memoization)**

1. Initialize a 2D array memo of size m x n, filled with 0
2. For each cell (i, j) in the matrix:
   1. If memo[i][j] is not 0, return memo[i][j]
   2. Otherwise, compute the longest increasing path from this cell:
      1. Initialize a path length of 1 (for the current cell)
      2. For each of the four directions (up, down, left, right):
         1. If the next cell is within bounds and has a larger value:
            1. Recursively compute the longest path from the next cell
            2. Update the path length if a longer path is found
      3. Store the result in memo[i][j] and return it
3. Return the maximum value in the memo array

### **Step 3: Implementation (Optimized - Dynamic Programming with Memoization)**

```python
def longestIncreasingPath(matrix):
    if not matrix:
        return 0
    
    m, n = len(matrix), len(matrix[0])
    memo = [[0] * n for _ in range(m)]
    
    def dfs(i, j):
        # If we've already computed the result, return it
        if memo[i][j] != 0:
            return memo[i][j]
        
        # Initialize path length for the current cell
        path_length = 1
        
        # Try all four directions
        for di, dj in [(0, 1), (1, 0), (0, -1), (-1, 0)]:
            ni, nj = i + di, j + dj
            
            # Check if the next cell is valid and has a larger value
            if 0 <= ni < m and 0 <= nj < n and matrix[ni][nj] > matrix[i][j]:
                path_length = max(path_length, 1 + dfs(ni, nj))
        
        # Store the result in the memo array
        memo[i][j] = path_length
        return path_length
    
    # Try DFS from each cell
    max_path = 0
    for i in range(m):
        for j in range(n):
            max_path = max(max_path, dfs(i, j))
    
    return max_path
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming with Memoization)**

* Time Complexity: O(m * n) where m is the number of rows and n is the number of columns. We compute the result for each cell exactly once.
* Space Complexity: O(m * n) for the memo array and the recursion stack.
* This approach is much more efficient than the brute force approach for large matrices.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming with Memoization)**

* For matrix = [[9,9,4],[6,6,8],[2,1,1]]:
  * Initialize memo = [[0,0,0],[0,0,0],[0,0,0]]
  * dfs(0, 0): matrix[0][0] = 9
    * No valid next cells
    * memo[0][0] = 1
  * dfs(0, 1): matrix[0][1] = 9
    * No valid next cells
    * memo[0][1] = 1
  * dfs(0, 2): matrix[0][2] = 4
    * No valid next cells
    * memo[0][2] = 1
  * dfs(1, 0): matrix[1][0] = 6
    * Valid next cell: (0, 0) with value 9
      * dfs(0, 0) = 1 (already computed)
    * memo[1][0] = 1 + 1 = 2
  * ... (compute for all cells)
  * Final memo array:
    ```
    [[1, 1, 1],
     [2, 2, 1],
     [3, 4, 2]]
    ```
  * max_path = 4, which is the answer
* The dynamic programming approach with memoization efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (DFS) | O(2^(m+n)) | O(m*n) | Inefficient for large matrices |
| Dynamic Programming (Memoization) | O(m*n) | O(m*n) | Efficient for all matrix sizes |

---

## **Final Takeaways**

* The key to efficiently solving the Longest Increasing Path in a Matrix problem is to recognize the overlapping subproblems and use dynamic programming with memoization to avoid redundant computations.
* The problem can be solved using a top-down approach with memoization, where we compute the longest path from each cell and store the results for reuse.
* The time complexity is reduced from exponential to linear in the size of the matrix, making it feasible to solve for large matrices.
* This problem is a classic example of dynamic programming applied to a graph-like structure (the matrix), where each cell represents a node and the edges are determined by the increasing path constraint.
* The approach can be extended to handle different constraints on the path, such as decreasing paths or paths with specific patterns.

---

## **Real-life Analogy**

* Imagine you're a hiker trying to find the longest uphill path in a mountainous region represented by a grid of elevations. The brute force approach would be to try all possible paths from each starting point, which is inefficient. The dynamic programming approach would be to remember the longest path from each point you've already visited, which is much more efficient.

---

## **Summary**

* We solved the Longest Increasing Path in a Matrix problem by first considering a brute force DFS approach that tries all possible paths from each cell, resulting in O(2^(m+n)) time complexity. We then optimized using dynamic programming with memoization to avoid redundant computations, reducing the time complexity to O(m * n). The key insight is to recognize that the longest increasing path from a cell depends only on the longest increasing paths from its valid neighbors, which allows us to build the solution incrementally by storing and reusing intermediate results. 