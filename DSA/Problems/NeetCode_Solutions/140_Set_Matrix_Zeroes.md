# 📝 Set Matrix Zeroes

## **Problem Statement**

* Given an m x n integer matrix `matrix`, if an element is 0, set its entire row and column to 0's.
* You must do it in place.
* Example:
  * Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
  * Output: [[1,0,1],[0,0,0],[1,0,1]] (The 0 is at position (1,1), so its entire row and column are set to 0)
  * Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
  * Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]] (The 0s are at positions (0,0) and (0,3), so their entire rows and columns are set to 0)
* Constraints:
  * m == matrix.length
  * n == matrix[i].length
  * 1 <= m, n <= 200
  * -2^31 <= matrix[i][j] <= 2^31 - 1

---

## **Step 1: Thinking Process (Brute Force)**

* For each cell with a value of 0, we need to set its entire row and column to 0.
* A naive approach would be to iterate through the matrix, and for each 0, set its row and column to 0.
* However, this would cause issues if we set a cell to 0 and then later encounter it as part of another row or column.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a copy of the original matrix.
2. Iterate through the original matrix:
   1. If a cell is 0, set its entire row and column to 0 in the copy.
3. Return the copy.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def setZeroes(matrix):
    if not matrix:
        return []
    
    m, n = len(matrix), len(matrix[0])
    copy = [row[:] for row in matrix]
    
    for i in range(m):
        for j in range(n):
            if matrix[i][j] == 0:
                # Set entire row to 0
                for k in range(n):
                    copy[i][k] = 0
                # Set entire column to 0
                for k in range(m):
                    copy[k][j] = 0
    
    # Copy back to the original matrix
    for i in range(m):
        for j in range(n):
            matrix[i][j] = copy[i][j]
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(m * n * (m + n)) where m is the number of rows and n is the number of columns. For each cell, we might need to set an entire row and column to 0.
* Space Complexity: O(m * n) for storing the copy of the matrix.
* This approach is inefficient in terms of both time and space complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For matrix = [[1,1,1],[1,0,1],[1,1,1]]:
  * Create a copy: [[1,1,1],[1,0,1],[1,1,1]]
  * Find 0 at (1,1):
    * Set row 1 to 0: copy = [[1,1,1],[0,0,0],[1,1,1]]
    * Set column 1 to 0: copy = [[1,0,1],[0,0,0],[1,0,1]]
  * Copy back to the original matrix: matrix = [[1,0,1],[0,0,0],[1,0,1]]

---

## **Why are we using this algorithm?**

* For optimization, we can use the first row and first column of the matrix itself as markers.
* If a cell in the original matrix is 0, we'll mark the corresponding row and column in the first row and first column.
* Then, we'll iterate through the matrix again and set cells to 0 based on these markers.
* This approach reduces the space complexity to O(1).

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We'll use the first row and first column of the matrix as markers.
* If a cell in the original matrix is 0, we'll mark the corresponding row and column in the first row and first column.
* We need to handle the first row and first column separately, as they're used as markers.
* We'll use two boolean variables to remember if the first row and first column should be set to 0.

### **Step 2: Flow Steps (Optimized)**

1. Initialize two boolean variables: `first_row_has_zero` and `first_col_has_zero`.
2. Check if the first row and first column have any 0s, and set the boolean variables accordingly.
3. Use the first row and first column as markers:
   1. For each cell (i, j) where i > 0 and j > 0:
      1. If the cell is 0, set matrix[i][0] and matrix[0][j] to 0.
4. Set rows and columns to 0 based on the markers:
   1. For each cell (i, j) where i > 0 and j > 0:
      1. If matrix[i][0] == 0 or matrix[0][j] == 0, set matrix[i][j] to 0.
5. Set the first row and first column to 0 if needed:
   1. If `first_row_has_zero`, set the first row to 0.
   2. If `first_col_has_zero`, set the first column to 0.

### **Step 3: Implementation (Optimized)**

```python
def setZeroes(matrix):
    if not matrix:
        return []
    
    m, n = len(matrix), len(matrix[0])
    first_row_has_zero = any(matrix[0][j] == 0 for j in range(n))
    first_col_has_zero = any(matrix[i][0] == 0 for i in range(m))
    
    # Use the first row and first column as markers
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][j] == 0:
                matrix[i][0] = 0
                matrix[0][j] = 0
    
    # Set rows and columns to 0 based on the markers
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][0] == 0 or matrix[0][j] == 0:
                matrix[i][j] = 0
    
    # Set the first row to 0 if needed
    if first_row_has_zero:
        for j in range(n):
            matrix[0][j] = 0
    
    # Set the first column to 0 if needed
    if first_col_has_zero:
        for i in range(m):
            matrix[i][0] = 0
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(m * n) where m is the number of rows and n is the number of columns. We iterate through the matrix a constant number of times.
* Space Complexity: O(1) as we use the matrix itself as markers.
* This approach is much more efficient than the brute force approach in terms of both time and space complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For matrix = [[1,1,1],[1,0,1],[1,1,1]]:
  * first_row_has_zero = false, first_col_has_zero = false
  * Find 0 at (1,1):
    * Set matrix[1][0] = 0 and matrix[0][1] = 0
  * Matrix after marking: [[1,0,1],[0,0,1],[1,1,1]]
  * Set rows and columns to 0 based on the markers:
    * matrix[1][1] is already 0
    * matrix[1][2] = 0 (because matrix[1][0] = 0)
  * Matrix after setting: [[1,0,1],[0,0,0],[1,0,1]]
  * first_row_has_zero = false, so the first row remains unchanged
  * first_col_has_zero = false, so the first column remains unchanged
  * Final matrix: [[1,0,1],[0,0,0],[1,0,1]]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(m * n * (m + n)) | O(m * n) | Creates a copy of the matrix |
| Optimized | O(m * n) | O(1) | Uses the first row and first column as markers |

---

## **Final Takeaways**

* The Set Matrix Zeroes problem is a classic example of in-place matrix manipulation.
* The key insight is to use the first row and first column of the matrix as markers.
* This problem demonstrates how understanding the constraints of a problem can lead to an efficient in-place solution.

---

## **Real-life Analogy**

* Imagine you're organizing a seating arrangement for an event. If a seat is marked as unavailable, you need to mark the entire row and column as unavailable. Instead of creating a new seating chart, you can use the first row and first column of the existing chart as markers, saving space and time.

---

## **Summary**

* We tackled the "Set Matrix Zeroes" problem by using the first row and first column of the matrix as markers. If a cell in the original matrix is 0, we mark the corresponding row and column in the first row and first column. Then, we iterate through the matrix again and set cells to 0 based on these markers. This approach has a time complexity of O(m * n) where m is the number of rows and n is the number of columns, and a space complexity of O(1) as we use the matrix itself as markers. This is much more efficient than the brute force approach of creating a copy of the matrix. 