# 📝 Rotate Image

## **Problem Statement**

* You are given an n x n 2D `matrix` representing an image, rotate the image by 90 degrees (clockwise).
* You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.
* Example:
  * Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
  * Output: [[7,4,1],[8,5,2],[9,6,3]]
  * Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
  * Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
* Constraints:
  * n == matrix.length == matrix[i].length
  * 1 <= n <= 20
  * -1000 <= matrix[i][j] <= 1000

---

## **Step 1: Thinking Process (Brute Force)**

* To rotate the image by 90 degrees clockwise, we need to transform the matrix such that the first row becomes the last column, the second row becomes the second-to-last column, and so on.
* A straightforward approach would be to create a new matrix and fill it with the rotated values.
* However, the problem requires an in-place rotation, so we need to modify the original matrix directly.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a new n x n matrix.
2. For each row i from 0 to n-1:
   1. For each column j from 0 to n-1:
      1. Set the new matrix at position [j][n-1-i] to the value at position [i][j] in the original matrix.
3. Copy the values from the new matrix back to the original matrix.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def rotate(matrix):
    n = len(matrix)
    # Create a new matrix
    new_matrix = [[0] * n for _ in range(n)]
    
    # Fill the new matrix with rotated values
    for i in range(n):
        for j in range(n):
            new_matrix[j][n-1-i] = matrix[i][j]
    
    # Copy the values back to the original matrix
    for i in range(n):
        for j in range(n):
            matrix[i][j] = new_matrix[i][j]
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the size of the matrix. We need to iterate through all n² elements twice.
* Space Complexity: O(n²) for storing the new matrix.
* This approach is inefficient in terms of space complexity, as the problem requires an in-place rotation.

---

## **Step 5: Visualizing Redundant Computation**

* For matrix = [[1,2,3],[4,5,6],[7,8,9]]:
  * Create a new matrix: [[0,0,0],[0,0,0],[0,0,0]]
  * Fill the new matrix:
    * new_matrix[0][2] = matrix[0][0] = 1
    * new_matrix[1][2] = matrix[0][1] = 2
    * ... and so on
  * Final new_matrix: [[7,4,1],[8,5,2],[9,6,3]]
  * Copy back to the original matrix.

---

## **Why are we using this algorithm?**

* For optimization, we'll use an in-place rotation approach.
* The key insight is that we can perform the rotation in layers, starting from the outermost layer and moving inward.
* For each layer, we rotate the four corners and then move inward along the edges.
* Another approach is to first transpose the matrix (swap rows with columns) and then reverse each row.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can perform the rotation in-place using two steps:
  1. Transpose the matrix (swap rows with columns).
  2. Reverse each row.
* This effectively rotates the matrix by 90 degrees clockwise.

### **Step 2: Flow Steps (Optimized)**

1. Transpose the matrix:
   1. For each row i from 0 to n-1:
      1. For each column j from i+1 to n-1:
         1. Swap matrix[i][j] with matrix[j][i].
2. Reverse each row:
   1. For each row i from 0 to n-1:
      1. Reverse the elements in the row.

### **Step 3: Implementation (Optimized)**

```python
def rotate(matrix):
    n = len(matrix)
    
    # Transpose the matrix
    for i in range(n):
        for j in range(i + 1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    # Reverse each row
    for i in range(n):
        matrix[i].reverse()
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n²) where n is the size of the matrix. We need to iterate through all n² elements.
* Space Complexity: O(1) as we perform the rotation in-place.
* This approach is more efficient than the brute force approach in terms of space complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For matrix = [[1,2,3],[4,5,6],[7,8,9]]:
  * Transpose the matrix:
    * Swap (0,1) with (1,0): [[1,4,3],[2,5,6],[7,8,9]]
    * Swap (0,2) with (2,0): [[1,4,7],[2,5,6],[3,8,9]]
    * Swap (1,2) with (2,1): [[1,4,7],[2,5,8],[3,6,9]]
  * Reverse each row:
    * Reverse row 0: [[7,4,1],[2,5,8],[3,6,9]]
    * Reverse row 1: [[7,4,1],[8,5,2],[3,6,9]]
    * Reverse row 2: [[7,4,1],[8,5,2],[9,6,3]]
  * Final matrix: [[7,4,1],[8,5,2],[9,6,3]]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(n²) | Creates a new matrix and copies values back |
| Optimized | O(n²) | O(1) | Transposes the matrix and reverses each row in-place |

---

## **Final Takeaways**

* The Rotate Image problem is a classic example of matrix manipulation.
* The key insight is that a 90-degree clockwise rotation can be achieved by first transposing the matrix and then reversing each row.
* This problem demonstrates how understanding the properties of a problem can lead to an efficient in-place solution.

---

## **Real-life Analogy**

* Imagine you have a grid of tiles on a table. To rotate the grid 90 degrees clockwise, you could first flip it along its diagonal (transpose), and then flip it horizontally (reverse each row). This is similar to how we rotate the matrix in our optimized approach.

---

## **Summary**

* We tackled the "Rotate Image" problem by first transposing the matrix (swapping rows with columns) and then reversing each row. This effectively rotates the matrix by 90 degrees clockwise. This approach has a time complexity of O(n²) where n is the size of the matrix, and a space complexity of O(1) as we perform the rotation in-place. This is more efficient than the brute force approach of creating a new matrix and copying values back. 