# 📝 Spiral Matrix

## **Problem Statement**

* Given an m x n `matrix`, return all elements of the `matrix` in spiral order.
* Example:
  * Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
  * Output: [1,2,3,6,9,8,7,4,5] (Spiral order: right, down, left, up, and repeat)
  * Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
  * Output: [1,2,3,4,8,12,11,10,9,5,6,7] (Spiral order: right, down, left, up, and repeat)
* Constraints:
  * m == matrix.length
  * n == matrix[i].length
  * 1 <= m, n <= 10
  * -100 <= matrix[i][j] <= 100

---

## **Step 1: Thinking Process (Brute Force)**

* To traverse the matrix in spiral order, we need to move in a specific pattern: right, down, left, up, and repeat.
* We can define boundaries for the matrix and shrink them as we traverse the matrix.
* We need to keep track of the direction we're moving in and switch directions when we hit a boundary.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize four boundaries: top = 0, right = n-1, bottom = m-1, left = 0.
2. Initialize a result array to store the elements in spiral order.
3. While top <= bottom and left <= right:
   1. Traverse from left to right along the top boundary, then increment top.
   2. Traverse from top to bottom along the right boundary, then decrement right.
   3. If top <= bottom, traverse from right to left along the bottom boundary, then decrement bottom.
   4. If left <= right, traverse from bottom to top along the left boundary, then increment left.
4. Return the result array.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def spiralOrder(matrix):
    if not matrix:
        return []
    
    m, n = len(matrix), len(matrix[0])
    result = []
    
    top, right, bottom, left = 0, n - 1, m - 1, 0
    
    while top <= bottom and left <= right:
        # Traverse from left to right along the top boundary
        for j in range(left, right + 1):
            result.append(matrix[top][j])
        top += 1
        
        # Traverse from top to bottom along the right boundary
        for i in range(top, bottom + 1):
            result.append(matrix[i][right])
        right -= 1
        
        # Traverse from right to left along the bottom boundary
        if top <= bottom:
            for j in range(right, left - 1, -1):
                result.append(matrix[bottom][j])
            bottom -= 1
        
        # Traverse from bottom to top along the left boundary
        if left <= right:
            for i in range(bottom, top - 1, -1):
                result.append(matrix[i][left])
            left += 1
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(m * n) where m is the number of rows and n is the number of columns. We visit each cell exactly once.
* Space Complexity: O(m * n) for storing the result array.
* This approach is actually optimal in terms of time complexity, as we need to visit each cell exactly once.

---

## **Step 5: Visualizing Computation**

* For matrix = [[1,2,3],[4,5,6],[7,8,9]]:
  * Initialize top = 0, right = 2, bottom = 2, left = 0, result = []
  * First iteration:
    * Traverse top row: result = [1,2,3], top = 1
    * Traverse right column: result = [1,2,3,6,9], right = 1
    * Traverse bottom row: result = [1,2,3,6,9,8,7], bottom = 1
    * Traverse left column: result = [1,2,3,6,9,8,7,4], left = 1
  * Second iteration:
    * Traverse top row: result = [1,2,3,6,9,8,7,4,5], top = 2
    * top > bottom, so we're done
  * Final result: [1,2,3,6,9,8,7,4,5]

---

## **Why are we using this algorithm?**

* The approach of defining boundaries and shrinking them as we traverse the matrix is intuitive and efficient.
* We visit each cell exactly once, which is optimal.
* However, there's another approach using a direction array and a visited matrix that might be more concise.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can use a direction array to represent the four directions: right, down, left, up.
* We'll also use a visited matrix to keep track of the cells we've already visited.
* We'll start from the top-left corner and move in the current direction until we hit a boundary or a visited cell.
* When we hit a boundary or a visited cell, we'll change direction and continue.

### **Step 2: Flow Steps (Optimized)**

1. Initialize a result array to store the elements in spiral order.
2. Initialize a visited matrix of the same size as the input matrix, with all values set to false.
3. Initialize a direction array for the four directions: right, down, left, up.
4. Initialize the current position as (0, 0) and the current direction as 0 (right).
5. While we haven't visited all cells:
   1. Add the current cell's value to the result array and mark it as visited.
   2. Calculate the next position in the current direction.
   3. If the next position is out of bounds or already visited, change direction and recalculate the next position.
   4. Move to the next position.
6. Return the result array.

### **Step 3: Implementation (Optimized)**

```python
def spiralOrder(matrix):
    if not matrix:
        return []
    
    m, n = len(matrix), len(matrix[0])
    result = []
    visited = [[False] * n for _ in range(m)]
    
    # Direction vectors for right, down, left, up
    dr = [0, 1, 0, -1]
    dc = [1, 0, -1, 0]
    
    r, c, di = 0, 0, 0  # Start from the top-left corner, moving right
    
    for _ in range(m * n):
        result.append(matrix[r][c])
        visited[r][c] = True
        
        # Calculate the next position
        nr, nc = r + dr[di], c + dc[di]
        
        # Change direction if the next position is out of bounds or already visited
        if nr < 0 or nr >= m or nc < 0 or nc >= n or visited[nr][nc]:
            di = (di + 1) % 4  # Change direction (right -> down -> left -> up -> right)
            nr, nc = r + dr[di], c + dc[di]
        
        r, c = nr, nc  # Move to the next position
    
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(m * n) where m is the number of rows and n is the number of columns. We visit each cell exactly once.
* Space Complexity: O(m * n) for storing the result array and the visited matrix.
* This approach is similar to the brute force approach in terms of time complexity, but it uses a different strategy.

### **Step 5: Visualizing Computation (Optimized)**

* For matrix = [[1,2,3],[4,5,6],[7,8,9]]:
  * Initialize result = [], visited = [[False, False, False], [False, False, False], [False, False, False]], r = 0, c = 0, di = 0
  * Visit (0, 0): result = [1], visited[0][0] = True
  * Next position: (0, 1), continue in the same direction
  * Visit (0, 1): result = [1, 2], visited[0][1] = True
  * ... and so on
  * Final result: [1, 2, 3, 6, 9, 8, 7, 4, 5]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Boundary Shrinking | O(m * n) | O(m * n) | Shrinks boundaries as we traverse |
| Direction Array | O(m * n) | O(m * n) | Uses a direction array and a visited matrix |

---

## **Final Takeaways**

* The Spiral Matrix problem is a classic example of matrix traversal.
* Both approaches (boundary shrinking and direction array) have the same time complexity, but they use different strategies.
* The boundary shrinking approach is more intuitive and easier to understand.
* The direction array approach is more concise and can be extended to other matrix traversal problems.

---

## **Real-life Analogy**

* Imagine you're exploring a maze in a spiral pattern. You start from the entrance and keep following the right wall (or left wall) until you reach the center. This is similar to how we traverse the matrix in a spiral order.

---

## **Summary**

* We tackled the "Spiral Matrix" problem using two approaches: boundary shrinking and direction array. Both approaches have a time complexity of O(m * n) where m is the number of rows and n is the number of columns. The boundary shrinking approach is more intuitive and easier to understand, while the direction array approach is more concise and can be extended to other matrix traversal problems. 