# 📝 Search a 2D Matrix

## **Problem Statement**

* You are given an m x n integer matrix `matrix` with the following two properties:
  * Each row is sorted in non-decreasing order.
  * The first integer of each row is greater than the last integer of the previous row.
* Given an integer `target`, return `true` if `target` is in `matrix` or `false` otherwise.
* You must write a solution in O(log(m * n)) time complexity.
* Example:
  * Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
  * Output: true
* Constraints:
  * m == matrix.length
  * n == matrix[i].length
  * 1 <= m, n <= 100
  * -10^4 <= matrix[i][j], target <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine if the target value exists in the matrix
* The most straightforward approach would be to iterate through each element in the matrix
* For each element, we check if it equals the target
* If we find the target, we return true
* If we reach the end of the matrix without finding the target, we return false

---

## **Step 2: Flow Steps (Brute Force)**

1. Iterate through each row i from 0 to m-1:
   1. Iterate through each column j from 0 to n-1:
      1. If matrix[i][j] equals target, return true
2. If the target is not found, return false

---

## **Step 3: Brute Force Implementation (Code)**

```python
def searchMatrix(matrix, target):
    m, n = len(matrix), len(matrix[0])
    
    for i in range(m):
        for j in range(n):
            if matrix[i][j] == target:
                return True
    
    return False
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(m * n) where m is the number of rows and n is the number of columns. In the worst case, we might need to check all elements.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach doesn't meet the requirement of O(log(m * n)) time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]] and target = 3:
  * Check matrix[0][0] = 1, not equal to target
  * Check matrix[0][1] = 3, equal to target, return true
* We're checking each element one by one, which is inefficient for large matrices
* Since the matrix has special properties (each row is sorted and the first element of each row is greater than the last element of the previous row), we can use these properties to narrow down the search space

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Binary Search algorithm.
* The property that matches this problem is the ability to efficiently search in a sorted array.
* By treating the 2D matrix as a 1D sorted array, we can apply binary search to find the target in O(log(m * n)) time.
* Alternatives like linear search would be O(m * n), which doesn't meet the requirement.
* Precondition: The matrix must satisfy the given properties (each row is sorted and the first element of each row is greater than the last element of the previous row).

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* The key insight is that the matrix can be treated as a 1D sorted array
* We can map a 1D index to a 2D position using division and modulo operations
* For a 1D index i in a matrix with n columns:
  * The row index is i / n (integer division)
  * The column index is i % n (modulo)
* With this mapping, we can apply binary search on the 1D index

### **Step 2: Flow Steps (Optimized)**

1. Initialize left = 0 and right = m * n - 1
2. While left <= right:
   1. Calculate mid = left + (right - left) // 2
   2. Calculate the 2D position (row, col) from mid:
      1. row = mid // n
      2. col = mid % n
   3. If matrix[row][col] equals target, return true
   4. If matrix[row][col] > target, set right = mid - 1
   5. If matrix[row][col] < target, set left = mid + 1
3. If the target is not found, return false

### **Step 3: Implementation (Optimized)**

```python
def searchMatrix(matrix, target):
    if not matrix or not matrix[0]:
        return False
    
    m, n = len(matrix), len(matrix[0])
    left, right = 0, m * n - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        # Convert 1D index to 2D position
        row, col = mid // n, mid % n
        
        if matrix[row][col] == target:
            return True
        elif matrix[row][col] > target:
            right = mid - 1
        else:
            left = mid + 1
    
    return False
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(log(m * n)) where m is the number of rows and n is the number of columns. We're applying binary search on a virtual array of size m * n.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach meets the requirement of O(log(m * n)) time complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]] and target = 3:
  * m = 3, n = 4
  * Initialize left = 0, right = 11
  * mid = 5, row = 5 // 4 = 1, col = 5 % 4 = 1, matrix[1][1] = 11 > target, so right = mid - 1 = 4
  * left = 0, right = 4, mid = 2, row = 2 // 4 = 0, col = 2 % 4 = 2, matrix[0][2] = 5 > target, so right = mid - 1 = 1
  * left = 0, right = 1, mid = 0, row = 0 // 4 = 0, col = 0 % 4 = 0, matrix[0][0] = 1 < target, so left = mid + 1 = 1
  * left = 1, right = 1, mid = 1, row = 1 // 4 = 0, col = 1 % 4 = 1, matrix[0][1] = 3 == target, return true
* We're efficiently narrowing down the search space by treating the 2D matrix as a 1D sorted array.

### **Alternative Approach: Two Binary Searches**

Another approach is to use two binary searches:
1. First, find the row that may contain the target
2. Then, search for the target within that row

```python
def searchMatrix(matrix, target):
    if not matrix or not matrix[0]:
        return False
    
    m, n = len(matrix), len(matrix[0])
    
    # Binary search to find the row
    top, bottom = 0, m - 1
    while top <= bottom:
        mid_row = top + (bottom - top) // 2
        if matrix[mid_row][0] <= target <= matrix[mid_row][n-1]:
            # Target might be in this row, search within the row
            left, right = 0, n - 1
            while left <= right:
                mid_col = left + (right - left) // 2
                if matrix[mid_row][mid_col] == target:
                    return True
                elif matrix[mid_row][mid_col] < target:
                    left = mid_col + 1
                else:
                    right = mid_col - 1
            return False
        elif matrix[mid_row][0] > target:
            bottom = mid_row - 1
        else:
            top = mid_row + 1
    
    return False
```

This approach also has a time complexity of O(log(m) + log(n)) = O(log(m * n)), but it might be more intuitive for some.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(m * n) | O(1) | Checks each element one by one |
| Optimized (Algorithm: Single Binary Search) | O(log(m * n)) | O(1) | Treats the 2D matrix as a 1D sorted array |
| Alternative (Algorithm: Two Binary Searches) | O(log(m * n)) | O(1) | First finds the row, then searches within the row |

---

## **Final Takeaways**

* Binary search is a powerful algorithm for searching in sorted arrays with O(log n) time complexity.
* The key insight is to treat the 2D matrix as a 1D sorted array using a mapping between 1D indices and 2D positions.
* This problem demonstrates how understanding the properties of the input data (sorted rows and columns with a specific relationship) can lead to efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're looking for a book in a library where books are arranged in multiple shelves, with each shelf containing books sorted by their publication date. Additionally, the first book on each shelf is newer than the last book on the previous shelf. The brute force approach would be like checking each book one by one. The optimized approach is like treating all shelves as one long shelf and using a binary search to efficiently find the book, leveraging the fact that the entire collection is effectively sorted.

---

## **Summary**

* We solved the "Search a 2D Matrix" problem by first considering a linear search approach that checks each element one by one, resulting in O(m * n) time complexity. We then optimized using a binary search approach that treats the 2D matrix as a 1D sorted array, reducing the time complexity to O(log(m * n)) while maintaining O(1) space complexity. The key insight is to leverage the sorted property of the matrix and use a mapping between 1D indices and 2D positions to efficiently narrow down the search space. 