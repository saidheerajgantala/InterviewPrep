# 📝 N-Queens

## **Problem Statement**

* The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.
* Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.
* Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.
* Example 1:
  * Input: n = 4
  * Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
  * Explanation: There are two distinct solutions to the 4-queens puzzle as shown above
* Example 2:
  * Input: n = 1
  * Output: [["Q"]]
* Constraints:
  * 1 <= n <= 9

---

## **Step 1: Thinking Process (Brute Force)**

* We need to place n queens on an n x n chessboard such that no two queens attack each other
* Queens can attack in horizontal, vertical, and diagonal directions
* One approach would be to try all possible placements of queens and check if they are valid
* This is a classic backtracking problem

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty result list to store all valid solutions
2. Define a backtracking function that takes the current board state and the current row:
   1. If we've placed queens in all rows, add the current board state to the result list
   2. For each column in the current row:
      1. If it's safe to place a queen at (row, col), place the queen
      2. Recursively call the backtracking function for the next row
      3. Remove the queen (backtrack)
3. Call the backtracking function with an empty board and row 0
4. Return the result list

---

## **Step 3: Brute Force Implementation (Code)**

```python
def solveNQueens(n):
    result = []
    
    # Initialize the board with empty spaces
    board = [['.' for _ in range(n)] for _ in range(n)]
    
    def is_safe(row, col):
        # Check column
        for i in range(row):
            if board[i][col] == 'Q':
                return False
        
        # Check upper-left diagonal
        i, j = row - 1, col - 1
        while i >= 0 and j >= 0:
            if board[i][j] == 'Q':
                return False
            i -= 1
            j -= 1
        
        # Check upper-right diagonal
        i, j = row - 1, col + 1
        while i >= 0 and j < n:
            if board[i][j] == 'Q':
                return False
            i -= 1
            j += 1
        
        return True
    
    def backtrack(row):
        if row == n:
            # Convert the board to the required format
            result.append([''.join(row) for row in board])
            return
        
        for col in range(n):
            if is_safe(row, col):
                board[row][col] = 'Q'
                backtrack(row + 1)
                board[row][col] = '.'
    
    backtrack(0)
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n!) where n is the size of the board. In the worst case, we need to try all possible placements of queens.
* Space Complexity: O(n^2) for the board and O(n) for the recursion stack.
* This approach is inefficient for large values of n due to the factorial growth of the number of possible placements.

---

## **Step 5: Visualizing Redundant Computation**

* For the example n = 4:
  * Start with an empty board
  * Try placing a queen at (0, 0): board[0][0] = 'Q'
    * Try placing a queen at (1, 0): Not safe, skip
    * Try placing a queen at (1, 1): Not safe, skip
    * Try placing a queen at (1, 2): board[1][2] = 'Q'
      * Try placing a queen at (2, 0): board[2][0] = 'Q'
        * Try placing a queen at (3, 0): Not safe, skip
        * Try placing a queen at (3, 1): Not safe, skip
        * Try placing a queen at (3, 2): Not safe, skip
        * Try placing a queen at (3, 3): Not safe, skip
        * No valid placement for row 3, backtrack
      * Remove queen at (2, 0): board[2][0] = '.'
      * Try placing a queen at (2, 1): Not safe, skip
      * Try placing a queen at (2, 2): Not safe, skip
      * Try placing a queen at (2, 3): board[2][3] = 'Q'
        * Try placing a queen at (3, 0): Not safe, skip
        * Try placing a queen at (3, 1): board[3][1] = 'Q'
          * All queens placed, add to result: [".Q..","...Q","Q...","..Q."]
        * Remove queen at (3, 1): board[3][1] = '.'
        * Try placing a queen at (3, 2): Not safe, skip
        * Try placing a queen at (3, 3): Not safe, skip
      * Remove queen at (2, 3): board[2][3] = '.'
    * Remove queen at (1, 2): board[1][2] = '.'
    * Try placing a queen at (1, 3): board[1][3] = 'Q'
      * ...and so on
* There's redundant computation in checking if a placement is safe, as we repeatedly check the same columns and diagonals

---

## **Why are we using this algorithm?**

* For optimization, we'll use a more efficient way to check if a placement is safe.
* The property that matches this problem is that we can keep track of occupied columns and diagonals to quickly check if a placement is safe.
* By using sets to keep track of occupied columns and diagonals, we can avoid redundant checks.
* Alternatives like the brute force approach would be less efficient due to redundant safety checks.
* Precondition: We need to be able to uniquely identify columns and diagonals.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Sets for Tracking)**

* We can use sets to keep track of occupied columns and diagonals
* For columns, we simply store the column index
* For diagonals, we can use row + col for one direction and row - col for the other
* This way, we can check if a placement is safe in O(1) time

### **Step 2: Flow Steps (Optimized - Sets for Tracking)**

1. Initialize sets to keep track of occupied columns and diagonals
2. Initialize an empty result list to store all valid solutions
3. Define a backtracking function that takes the current board state and the current row:
   1. If we've placed queens in all rows, add the current board state to the result list
   2. For each column in the current row:
      1. If it's safe to place a queen at (row, col) (by checking the sets), place the queen and update the sets
      2. Recursively call the backtracking function for the next row
      3. Remove the queen and update the sets (backtrack)
4. Call the backtracking function with an empty board and row 0
5. Return the result list

### **Step 3: Implementation (Optimized - Sets for Tracking)**

```python
def solveNQueens(n):
    result = []
    
    # Initialize the board with empty spaces
    board = [['.' for _ in range(n)] for _ in range(n)]
    
    # Sets to keep track of occupied columns and diagonals
    cols = set()
    pos_diag = set()  # row + col
    neg_diag = set()  # row - col
    
    def backtrack(row):
        if row == n:
            # Convert the board to the required format
            result.append([''.join(row) for row in board])
            return
        
        for col in range(n):
            if col in cols or row + col in pos_diag or row - col in neg_diag:
                continue
            
            # Place the queen
            board[row][col] = 'Q'
            cols.add(col)
            pos_diag.add(row + col)
            neg_diag.add(row - col)
            
            backtrack(row + 1)
            
            # Remove the queen (backtrack)
            board[row][col] = '.'
            cols.remove(col)
            pos_diag.remove(row + col)
            neg_diag.remove(row - col)
    
    backtrack(0)
    return result
```

### **Step 4: Complexity Analysis (Optimized - Sets for Tracking)**

* Time Complexity: O(n!) where n is the size of the board. In the worst case, we still need to try all possible placements of queens. However, the constant factor is much smaller due to the efficient safety checks.
* Space Complexity: O(n^2) for the board, O(n) for the recursion stack, and O(n) for the sets.
* This approach is more efficient than the brute force approach due to the O(1) safety checks.

### **Step 5: Visualizing Computation (Optimized - Sets for Tracking)**

* For the example n = 4:
  * Initialize sets: cols = {}, pos_diag = {}, neg_diag = {}
  * Try placing a queen at (0, 0): board[0][0] = 'Q', cols = {0}, pos_diag = {0}, neg_diag = {0}
    * Try placing a queen at (1, 0): Not safe (col 0 is occupied), skip
    * Try placing a queen at (1, 1): Not safe (pos_diag 2 is occupied), skip
    * Try placing a queen at (1, 2): board[1][2] = 'Q', cols = {0, 2}, pos_diag = {0, 3}, neg_diag = {0, -1}
      * Try placing a queen at (2, 0): Not safe (col 0 is occupied), skip
      * Try placing a queen at (2, 1): board[2][1] = 'Q', cols = {0, 1, 2}, pos_diag = {0, 3, 3}, neg_diag = {0, -1, 1}
        * Not safe (pos_diag 3 is already occupied), backtrack
      * Try placing a queen at (2, 2): Not safe (col 2 is occupied), skip
      * Try placing a queen at (2, 3): board[2][3] = 'Q', cols = {0, 2, 3}, pos_diag = {0, 3, 5}, neg_diag = {0, -1, -1}
        * Not safe (neg_diag -1 is already occupied), backtrack
      * No valid placement for row 2, backtrack
    * Remove queen at (1, 2): board[1][2] = '.', cols = {0}, pos_diag = {0}, neg_diag = {0}
    * Try placing a queen at (1, 3): board[1][3] = 'Q', cols = {0, 3}, pos_diag = {0, 4}, neg_diag = {0, -2}
      * ...and so on
* The sets approach efficiently avoids redundant safety checks

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n!) | O(n^2) | Inefficient due to redundant safety checks |
| Optimized (Sets for Tracking) | O(n!) | O(n^2) | Efficient with O(1) safety checks |

---

## **Final Takeaways**

* The key to efficiently solving the N-Queens problem is to use sets to keep track of occupied columns and diagonals.
* By using sets, we can check if a placement is safe in O(1) time, avoiding redundant safety checks.
* This problem demonstrates the power of backtracking combined with efficient data structures for constraint checking.

---

## **Real-life Analogy**

* Imagine you're a wedding planner trying to seat guests at tables such that no two guests who dislike each other are seated at the same table. The brute force approach would be to try all possible seating arrangements and check if each one is valid. The optimized approach would be to keep track of which tables are already occupied by guests who dislike each other, making it easier to find valid seating arrangements.

---

## **Summary**

* We solved the N-Queens problem by first considering a brute force approach that tries all possible placements and checks if they are valid, resulting in O(n!) time complexity. We then optimized using sets to keep track of occupied columns and diagonals, which has the same asymptotic time complexity but is more efficient in practice due to O(1) safety checks. The key insight is to use sets to quickly determine if a placement is safe, avoiding redundant checks during the backtracking process. 