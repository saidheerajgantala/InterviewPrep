# 📝 Valid Sudoku

## **Problem Statement**

* Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:
  1. Each row must contain the digits 1-9 without repetition.
  2. Each column must contain the digits 1-9 without repetition.
  3. Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.
* Note:
  * A Sudoku board (partially filled) could be valid but is not necessarily solvable.
  * Only the filled cells need to be validated according to the mentioned rules.
* Example:
  * Input: board = 
    ```
    [["5","3",".",".","7",".",".",".","."]
    ,["6",".",".","1","9","5",".",".","."]
    ,[".","9","8",".",".",".",".","6","."]
    ,["8",".",".",".","6",".",".",".","3"]
    ,["4",".",".","8",".","3",".",".","1"]
    ,["7",".",".",".","2",".",".",".","6"]
    ,[".","6",".",".",".",".","2","8","."]
    ,[".",".",".","4","1","9",".",".","5"]
    ,[".",".",".",".","8",".",".","7","9"]]
    ```
  * Output: true
* Constraints:
  * board.length == 9
  * board[i].length == 9
  * board[i][j] is a digit 1-9 or '.'

---

## **Step 1: Thinking Process (Brute Force)**

* We need to check if the Sudoku board is valid according to the three rules
* For each rule, we need to check if there are any duplicate digits in the respective group (row, column, or 3x3 sub-box)
* The most straightforward approach is to iterate through each row, each column, and each 3x3 sub-box separately
* For each group, we can use a set to keep track of the digits we've seen so far

---

## **Step 2: Flow Steps (Brute Force)**

1. Check all rows:
   1. For each row:
      1. Create an empty set
      2. For each cell in the row:
         1. If the cell is not empty and the digit is already in the set, return false
         2. Otherwise, add the digit to the set
2. Check all columns:
   1. For each column:
      1. Create an empty set
      2. For each cell in the column:
         1. If the cell is not empty and the digit is already in the set, return false
         2. Otherwise, add the digit to the set
3. Check all 3x3 sub-boxes:
   1. For each sub-box:
      1. Create an empty set
      2. For each cell in the sub-box:
         1. If the cell is not empty and the digit is already in the set, return false
         2. Otherwise, add the digit to the set
4. If all checks pass, return true

---

## **Step 3: Brute Force Implementation (Code)**

```python
def isValidSudoku(board):
    # Check rows
    for row in range(9):
        seen = set()
        for col in range(9):
            if board[row][col] != '.':
                if board[row][col] in seen:
                    return False
                seen.add(board[row][col])
    
    # Check columns
    for col in range(9):
        seen = set()
        for row in range(9):
            if board[row][col] != '.':
                if board[row][col] in seen:
                    return False
                seen.add(board[row][col])
    
    # Check 3x3 sub-boxes
    for box_row in range(0, 9, 3):
        for box_col in range(0, 9, 3):
            seen = set()
            for row in range(box_row, box_row + 3):
                for col in range(box_col, box_col + 3):
                    if board[row][col] != '.':
                        if board[row][col] in seen:
                            return False
                        seen.add(board[row][col])
    
    return True
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(1) since the board size is fixed at 9x9. But if we consider the board size as n, it would be O(n²).
* Space Complexity: O(1) since we're using sets of at most 9 elements.
* While the time complexity is already optimal for this specific problem (with a fixed board size), we're iterating through the board multiple times, which is inefficient.

---

## **Step 5: Visualizing Redundant Computation**

* We're iterating through the entire board three times:
  * Once to check rows
  * Once to check columns
  * Once to check 3x3 sub-boxes
* For each cell, we're checking it three times (once in its row, once in its column, and once in its 3x3 sub-box).
* This is redundant and we can optimize by checking all three conditions in a single pass.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Single-Pass Hash Set algorithm.
* The property that matches this problem is the ability to check for duplicates efficiently using hash sets.
* By using separate hash sets for rows, columns, and sub-boxes, we can check all conditions in a single pass.
* Alternatives like sorting would be less efficient and more complex.
* Precondition: The board is a 9x9 grid with digits 1-9 or empty cells.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of making three passes through the board, we can check all three conditions in a single pass
* We'll use three sets of hash sets:
  * One set of 9 hash sets for rows
  * One set of 9 hash sets for columns
  * One set of 9 hash sets for 3x3 sub-boxes
* As we iterate through each cell, we'll check if the digit violates any of the three conditions

### **Step 2: Flow Steps (Optimized)**

1. Initialize three sets of hash sets:
   1. rows: 9 hash sets, one for each row
   2. cols: 9 hash sets, one for each column
   3. boxes: 9 hash sets, one for each 3x3 sub-box
2. Iterate through each cell (row, col) in the board:
   1. If the cell is not empty:
      1. Calculate the box index: (row // 3) * 3 + col // 3
      2. If the digit is already in rows[row] or cols[col] or boxes[box_idx], return false
      3. Otherwise, add the digit to all three sets
3. If all cells pass the check, return true

### **Step 3: Implementation (Optimized)**

```python
def isValidSudoku(board):
    # Initialize hash sets
    rows = [set() for _ in range(9)]
    cols = [set() for _ in range(9)]
    boxes = [set() for _ in range(9)]
    
    # Check all cells in one pass
    for row in range(9):
        for col in range(9):
            if board[row][col] != '.':
                digit = board[row][col]
                box_idx = (row // 3) * 3 + col // 3
                
                # Check if the digit is already in any of the sets
                if (digit in rows[row] or 
                    digit in cols[col] or 
                    digit in boxes[box_idx]):
                    return False
                
                # Add the digit to all three sets
                rows[row].add(digit)
                cols[col].add(digit)
                boxes[box_idx].add(digit)
    
    return True
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(1) since the board size is fixed at 9x9. But if we consider the board size as n, it would be O(n²).
* Space Complexity: O(1) since we're using a fixed number of sets with at most 9 elements each.
* While the asymptotic complexity remains the same, we're now only iterating through the board once instead of three times.

### **Step 5: Visualizing Computation (Optimized)**

* For each cell, we're checking three conditions simultaneously:
  * Is the digit already in the row?
  * Is the digit already in the column?
  * Is the digit already in the 3x3 sub-box?
* We're using hash sets for O(1) lookups, making the checks efficient.
* By processing each cell only once, we've eliminated the redundant iterations.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Iterates through the board three times |
| Optimized (Algorithm: Single-Pass Hash Set) | O(n²) | O(1) | Iterates through the board once |

---

## **Final Takeaways**

* Using multiple data structures to track different conditions can allow us to check all conditions in a single pass.
* The key insight is recognizing that we can check all three Sudoku rules simultaneously as we iterate through the board.
* This problem demonstrates how careful organization of data can eliminate redundant iterations.

---

## **Real-life Analogy**

* Imagine you're a teacher checking if students have been assigned to the right classrooms. The brute force approach would be like walking through the school three times: once to check each classroom, once to check each subject group, and once to check each grade level. The optimized approach is like walking through once with three different lists, checking all three conditions for each student as you go.

---

## **Summary**

* We solved the "Valid Sudoku" problem by first using a brute force approach that checks rows, columns, and sub-boxes separately, resulting in three passes through the board. We then optimized by using three sets of hash sets to check all conditions in a single pass, reducing the number of iterations while maintaining the same time complexity. This demonstrates how proper data organization can eliminate redundant work even when the asymptotic complexity remains the same. 