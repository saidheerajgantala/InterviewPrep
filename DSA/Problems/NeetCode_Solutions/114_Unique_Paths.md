# 📝 Unique Paths

## **Problem Statement**

* There is a robot on an m x n grid. The robot is initially positioned at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m-1][n-1]).
* The robot can only move either down or right at any point in time.
* Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.
* The test cases are generated so that the answer will be less than or equal to 2 * 10^9.
* Example 1:
  * Input: m = 3, n = 7
  * Output: 28
* Example 2:
  * Input: m = 3, n = 2
  * Output: 3
  * Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
    1. Right -> Down -> Down
    2. Down -> Right -> Down
    3. Down -> Down -> Right
* Constraints:
  * 1 <= m, n <= 100

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the number of unique paths from the top-left to the bottom-right of a grid
* The robot can only move right or down at each step
* One approach would be to use recursion to explore all possible paths
* At each cell, we have two choices: move right or move down
* We need to count the total number of paths that lead to the destination

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes the current position (row, col)
2. Base cases:
   1. If we reach the destination (m-1, n-1), return 1 (found a valid path)
   2. If we go out of bounds (row >= m or col >= n), return 0 (invalid path)
3. Recursive case: Try moving right and down
   1. Move right: recursively call the function with (row, col+1)
   2. Move down: recursively call the function with (row+1, col)
   3. Return the sum of the results from both moves
4. Start the recursion with position (0, 0)

---

## **Step 3: Brute Force Implementation (Code)**

```python
def uniquePaths(m, n):
    def dfs(row, col):
        # Base case: reached the destination
        if row == m - 1 and col == n - 1:
            return 1
        
        # Base case: out of bounds
        if row >= m or col >= n:
            return 0
        
        # Move right and down
        right = dfs(row, col + 1)
        down = dfs(row + 1, col)
        
        # Return the total number of paths
        return right + down
    
    return dfs(0, 0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^(m+n)) where m is the number of rows and n is the number of columns. In the worst case, we explore all possible paths, which is exponential.
* Space Complexity: O(m+n) for the recursion stack.
* This approach is inefficient for large grids due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For m = 3, n = 3:
  * dfs(0, 0):
    * right = dfs(0, 1):
      * right = dfs(0, 2):
        * right = dfs(0, 3): Out of bounds, return 0
        * down = dfs(1, 2):
          * right = dfs(1, 3): Out of bounds, return 0
          * down = dfs(2, 2):
            * right = dfs(2, 3): Out of bounds, return 0
            * down = dfs(3, 2): Out of bounds, return 0
            * return 0
          * return 0
        * return 0
      * down = dfs(1, 1):
        * right = dfs(1, 2): (computed above)
        * down = dfs(2, 1):
          * right = dfs(2, 2): (computed above)
          * down = dfs(3, 1): Out of bounds, return 0
          * return 0
        * return 0
      * return 0
    * down = dfs(1, 0):
      * right = dfs(1, 1): (computed above)
      * down = dfs(2, 0):
        * right = dfs(2, 1): (computed above)
        * down = dfs(3, 0): Out of bounds, return 0
        * return 0
      * return 0
    * return 0
* There's redundant computation in the recursive calls, especially for overlapping subproblems like dfs(1, 1)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the number of ways to reach a cell depends only on the number of ways to reach the cells above and to the left of it.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently calculate the number of ways to reach each cell in the grid.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let dp[i][j] represent the number of unique paths to reach the cell at position (i, j)
* Base cases:
  * dp[0][0] = 1 (there's only one way to reach the starting position)
  * dp[0][j] = 1 for all j (there's only one way to reach any cell in the first row: by moving right)
  * dp[i][0] = 1 for all i (there's only one way to reach any cell in the first column: by moving down)
* For each cell (i, j) where i > 0 and j > 0:
  * dp[i][j] = dp[i-1][j] + dp[i][j-1] (the number of ways to reach the cell from above and from the left)
* The answer will be dp[m-1][n-1]

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Create a 2D array dp of size m x n, initialized with 0
2. Initialize the first row and first column with 1
3. For each cell (i, j) where i > 0 and j > 0:
   1. dp[i][j] = dp[i-1][j] + dp[i][j-1]
4. Return dp[m-1][n-1]

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def uniquePaths(m, n):
    # Create a 2D array filled with 0
    dp = [[0] * n for _ in range(m)]
    
    # Initialize the first row and first column with 1
    for i in range(m):
        dp[i][0] = 1
    for j in range(n):
        dp[0][j] = 1
    
    # Fill the dp table
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
    
    return dp[m-1][n-1]
```

### **Space Optimization**

We can optimize the space complexity to O(n) by using a 1D array:

```python
def uniquePaths(m, n):
    # Create a 1D array filled with 1
    dp = [1] * n
    
    # Fill the dp table
    for i in range(1, m):
        for j in range(1, n):
            dp[j] += dp[j-1]
    
    return dp[n-1]
```

### **Mathematical Solution**

There's also a mathematical solution using combinatorics. To reach the bottom-right corner, the robot needs to make exactly m-1 moves down and n-1 moves right, for a total of (m-1) + (n-1) = m+n-2 moves. The number of unique paths is the number of ways to choose which of these moves are down (or right), which is C(m+n-2, m-1) or C(m+n-2, n-1).

```python
def uniquePaths(m, n):
    from math import comb
    return comb(m + n - 2, m - 1)
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(m * n) where m is the number of rows and n is the number of columns. We fill a 2D table of size m x n.
* Space Complexity: O(m * n) for the dp array.
* This approach is much more efficient than the brute force approach for large grids.

### **Complexity Analysis (Space-Optimized Version)**

* Time Complexity: O(m * n) where m is the number of rows and n is the number of columns.
* Space Complexity: O(n) for the 1D dp array.
* This approach is even more space-efficient while maintaining the same time complexity.

### **Complexity Analysis (Mathematical Solution)**

* Time Complexity: O(min(m, n)) for calculating the combination.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is the most efficient in terms of both time and space complexity.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For m = 3, n = 3:
  * Initialize dp:
    ```
    1 1 1
    1 0 0
    1 0 0
    ```
  * Process dp[1][1]: dp[1][1] = dp[0][1] + dp[1][0] = 1 + 1 = 2
    ```
    1 1 1
    1 2 0
    1 0 0
    ```
  * Process dp[1][2]: dp[1][2] = dp[0][2] + dp[1][1] = 1 + 2 = 3
    ```
    1 1 1
    1 2 3
    1 0 0
    ```
  * Process dp[2][1]: dp[2][1] = dp[1][1] + dp[2][0] = 2 + 1 = 3
    ```
    1 1 1
    1 2 3
    1 3 0
    ```
  * Process dp[2][2]: dp[2][2] = dp[1][2] + dp[2][1] = 3 + 3 = 6
    ```
    1 1 1
    1 2 3
    1 3 6
    ```
  * dp[2][2] = 6, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(2^(m+n)) | O(m+n) | Inefficient for large grids |
| Dynamic Programming (2D) | O(m * n) | O(m * n) | Efficient for medium-sized grids |
| Dynamic Programming (1D) | O(m * n) | O(n) | Space-optimized version |
| Mathematical Solution | O(min(m, n)) | O(1) | Most efficient solution |

---

## **Final Takeaways**

* The key to efficiently solving the Unique Paths problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using a bottom-up approach by building the solution from smaller subproblems.
* The space optimization from a 2D array to a 1D array is possible because we only need the results from the previous row to compute the current row.
* The mathematical solution using combinatorics is the most efficient approach, but it requires understanding the problem from a different perspective.
* This problem demonstrates how different approaches can be applied to the same problem, each with its own trade-offs in terms of time and space complexity.

---

## **Real-life Analogy**

* Imagine you're trying to count the number of different routes from your home to your office, but you can only move east or south at each intersection. The brute force approach would be to try all possible routes, which is inefficient. The dynamic programming approach would be to count the number of ways to reach each intersection, which is much more efficient. The mathematical approach would be to recognize that you need to make a specific number of east and south moves, and calculate the number of ways to arrange these moves.

---

## **Summary**

* We solved the Unique Paths problem by first considering a brute force recursive approach that tries all possible paths, resulting in O(2^(m+n)) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(m * n). We explored a space optimization that reduces the space complexity from O(m * n) to O(n). Finally, we presented a mathematical solution using combinatorics that further reduces the time complexity to O(min(m, n)) and the space complexity to O(1). The key insight is to recognize that the number of ways to reach a cell depends only on the number of ways to reach the cells above and to the left of it, which allows us to build the solution incrementally. 