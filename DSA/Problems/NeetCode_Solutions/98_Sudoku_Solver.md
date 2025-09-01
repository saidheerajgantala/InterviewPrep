# 📝 Sudoku Solver

## **Problem Statement**

* Write a program to solve a Sudoku puzzle by filling the empty cells.
* A sudoku solution must satisfy all of the following rules:
  * Each of the digits 1-9 must occur exactly once in each row.
  * Each of the digits 1-9 must occur exactly once in each column.
  * Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.
* The '.' character indicates empty cells.
* Example 1:
  * Input: board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
  * Output: [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
  * Explanation: The input board is shown above and the only valid solution is shown below:
    ```
    5 3 4 | 6 7 8 | 9 1 2
    6 7 2 | 1 9 5 | 3 4 8
    1 9 8 | 3 4 2 | 5 6 7
    ------+-------+------
    8 5 9 | 7 6 1 | 4 2 3
    4 2 6 | 8 5 3 | 7 9 1
    7 1 3 | 9 2 4 | 8 5 6
    ------+-------+------
    9 6 1 | 5 3 7 | 2 8 4
    2 8 7 | 4 1 9 | 6 3 5
    3 4 5 | 2 8 6 | 1 7 9
    ```
* Constraints:
  * board.length == 9
  * board[i].length == 9
  * board[i][j] is a digit or '.'.
  * It is guaranteed that the input board has only one solution.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to fill the empty cells in the Sudoku puzzle such that all rules are satisfied
* One approach would be to try all possible numbers for each empty cell and backtrack if we reach an invalid state
* This is a classic backtracking problem where we try different values for each empty cell

---

## **Step 2: Flow Steps (Brute Force)**

1. Find an empty cell in the board
2. If there are no empty cells, the Sudoku is solved
3. For each number from 1 to 9:
   1. If the number is valid for the current cell (i.e., it doesn't violate any Sudoku rule), place it in the cell
   2. Recursively try to solve the rest of the Sudoku
   3. If the recursive call returns true, the Sudoku is solved
   4. If the recursive call returns false, backtrack by removing the number from the cell and try the next number
4. If no number works for the current cell, return false

---

## **Step 3: Brute Force Implementation (Code)**

```python
def solveSudoku(board):
    # Find an empty cell
    def find_empty():
        for i in range(9):
            for j in range(9):
                if board[i][j] == '.':
                    return i, j
        return None
    
    # Check if a number is valid in a given position
    def is_valid(num, pos):
        row, col = pos
        
        # Check row
        for j in range(9):
            if board[row][j] == num:
                return False
        
        # Check column
        for i in range(9):
            if board[i][col] == num:
                return False
        
        # Check 3x3 box
        box_row, box_col = 3 * (row // 3), 3 * (col // 3)
        for i in range(box_row, box_row + 3):
            for j in range(box_col, box_col + 3):
                if board[i][j] == num:
                    return False
        
        return True
    
    def solve():
        empty = find_empty()
        if not empty:
            return True
        
        row, col = empty
        for num in map(str, range(1, 10)):
            if is_valid(num, (row, col)):
                board[row][col] = num
                
                if solve():
                    return True
                
                board[row][col] = '.'  # Backtrack
        
        return False
    
    solve()
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(9^m) where m is the number of empty cells. In the worst case, we need to try all 9 numbers for each empty cell.
* Space Complexity: O(m) for the recursion stack, where m is the number of empty cells.
* This approach is efficient for small Sudoku puzzles but can be slow for puzzles with many empty cells.

---

## **Step 5: Visualizing Redundant Computation**

* For each empty cell, we check if a number is valid by scanning the entire row, column, and 3x3 box
* This involves redundant checks, especially when we're trying multiple numbers for the same cell
* We also repeatedly search for empty cells, which is inefficient

---

## **Why are we using this algorithm?**

* For optimization, we'll use a more efficient way to check if a number is valid and to find empty cells.
* The property that matches this problem is that we can keep track of which numbers are already used in each row, column, and box.
* By using sets to keep track of used numbers, we can check if a number is valid in O(1) time.
* Alternatives like the brute force approach would be less efficient due to redundant checks.
* Precondition: We need to be able to efficiently track used numbers in rows, columns, and boxes.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Sets for Tracking)**

* We can use sets to keep track of which numbers are already used in each row, column, and 3x3 box
* This way, we can check if a number is valid in O(1) time
* We can also precompute the list of empty cells to avoid repeatedly searching for them

### **Step 2: Flow Steps (Optimized - Sets for Tracking)**

1. Initialize sets to keep track of used numbers in each row, column, and box
2. Precompute the list of empty cells
3. For each empty cell:
   1. For each number from 1 to 9:
      1. If the number is not used in the current row, column, and box, place it in the cell and update the sets
      2. Recursively try to solve the rest of the Sudoku
      3. If the recursive call returns true, the Sudoku is solved
      4. If the recursive call returns false, backtrack by removing the number from the cell and the sets, and try the next number
4. If no number works for the current cell, return false

### **Step 3: Implementation (Optimized - Sets for Tracking)**

```python
def solveSudoku(board):
    # Initialize sets to keep track of used numbers
    rows = [set() for _ in range(9)]
    cols = [set() for _ in range(9)]
    boxes = [set() for _ in range(9)]
    
    # Precompute used numbers and empty cells
    empty_cells = []
    for i in range(9):
        for j in range(9):
            if board[i][j] != '.':
                num = board[i][j]
                rows[i].add(num)
                cols[j].add(num)
                box_idx = (i // 3) * 3 + j // 3
                boxes[box_idx].add(num)
            else:
                empty_cells.append((i, j))
    
    def backtrack(idx):
        if idx == len(empty_cells):
            return True
        
        row, col = empty_cells[idx]
        box_idx = (row // 3) * 3 + col // 3
        
        for num in map(str, range(1, 10)):
            if num not in rows[row] and num not in cols[col] and num not in boxes[box_idx]:
                # Place the number
                board[row][col] = num
                rows[row].add(num)
                cols[col].add(num)
                boxes[box_idx].add(num)
                
                if backtrack(idx + 1):
                    return True
                
                # Backtrack
                board[row][col] = '.'
                rows[row].remove(num)
                cols[col].remove(num)
                boxes[box_idx].remove(num)
        
        return False
    
    backtrack(0)
```

### **Step 4: Complexity Analysis (Optimized - Sets for Tracking)**

* Time Complexity: O(9^m) where m is the number of empty cells. In the worst case, we still need to try all 9 numbers for each empty cell. However, the constant factor is much smaller due to the efficient validity checks.
* Space Complexity: O(9 * 3) = O(1) for the sets and O(m) for the empty cells list and recursion stack.
* This approach is more efficient than the brute force approach due to the O(1) validity checks and precomputation of empty cells.

### **Step 5: Visualizing Computation (Optimized - Sets for Tracking)**

* For the example Sudoku puzzle:
  * Initialize sets for rows, columns, and boxes
  * Precompute used numbers and empty cells
  * For each empty cell:
    * Try each number from 1 to 9
    * If the number is valid (not in the current row, column, and box), place it and update the sets
    * Recursively try to solve the rest of the Sudoku
    * If the recursive call returns true, the Sudoku is solved
    * If the recursive call returns false, backtrack and try the next number
* The sets approach efficiently avoids redundant validity checks

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(9^m) | O(m) | Inefficient due to redundant validity checks |
| Optimized (Sets for Tracking) | O(9^m) | O(1) + O(m) | Efficient with O(1) validity checks |

---

## **Final Takeaways**

* The key to efficiently solving the Sudoku Solver problem is to use sets to keep track of used numbers in each row, column, and box.
* By using sets, we can check if a number is valid in O(1) time, avoiding redundant checks.
* Precomputing the list of empty cells also helps avoid repeatedly searching for them.
* This problem demonstrates the power of backtracking combined with efficient data structures for constraint checking.

---

## **Real-life Analogy**

* Imagine you're filling out a complex form where each field has multiple constraints based on other fields. The brute force approach would be to try different values for each field and check all constraints for each attempt. The optimized approach would be to keep track of which values are already used or invalid for each field, making it easier to find valid values.

---

## **Summary**

* We solved the Sudoku Solver problem by first considering a brute force approach that tries all possible numbers for each empty cell, resulting in O(9^m) time complexity. We then optimized using sets to keep track of used numbers in each row, column, and box, which has the same asymptotic time complexity but is more efficient in practice due to O(1) validity checks. The key insight is to use sets to quickly determine if a number is valid for a cell, avoiding redundant checks during the backtracking process. 