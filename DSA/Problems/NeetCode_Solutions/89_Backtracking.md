# 📝 Backtracking

## **Concept Overview**

* Backtracking is an algorithmic technique for solving problems recursively by trying to build a solution incrementally, one piece at a time.
* When the algorithm reaches a point where it cannot continue with the current solution (i.e., the solution becomes invalid), it "backtracks" by removing the last element and trying a different approach.
* It's essentially a depth-first search of the solution space, exploring all possible solutions and abandoning those that cannot lead to a valid solution.
* Backtracking is particularly useful for constraint satisfaction problems, such as puzzles, permutations, combinations, and subset problems.

---

## **Key Principles**

1. **Choice**: At each step, we make a choice from a set of available options.
2. **Constraint**: We check if the choice satisfies the problem constraints.
3. **Goal**: We check if we have reached a valid solution.
4. **Backtrack**: If the choice leads to an invalid solution, we undo the choice and try another option.

---

## **General Template**

```python
def backtrack(current_solution, remaining_choices):
    if is_solution_complete(current_solution):
        # Add the solution to the result
        result.append(current_solution.copy())
        return
    
    for choice in get_valid_choices(current_solution, remaining_choices):
        # Make a choice
        apply_choice(current_solution, choice)
        
        # Recursively solve the subproblem
        backtrack(current_solution, get_remaining_choices(remaining_choices, choice))
        
        # Undo the choice (backtrack)
        undo_choice(current_solution, choice)
```

---

## **Common Backtracking Problems**

1. **Permutations**: Generate all permutations of a set of elements.
2. **Combinations**: Generate all combinations of k elements from a set of n elements.
3. **Subsets**: Generate all subsets of a set of elements.
4. **N-Queens**: Place N queens on an N×N chessboard such that no two queens threaten each other.
5. **Sudoku**: Fill a 9×9 grid with digits so that each column, each row, and each of the nine 3×3 subgrids contains all of the digits from 1 to 9.
6. **Word Search**: Find if a word exists in a grid of characters.
7. **Palindrome Partitioning**: Partition a string such that every substring of the partition is a palindrome.
8. **Combination Sum**: Find all unique combinations of candidates where the chosen numbers sum to a target.

---

## **Example: Generating Permutations**

```python
def permute(nums):
    result = []
    backtrack([], nums, result)
    return result

def backtrack(current, remaining, result):
    if not remaining:
        result.append(current.copy())
        return
    
    for i in range(len(remaining)):
        # Make a choice
        current.append(remaining[i])
        
        # Recursively solve the subproblem
        backtrack(current, remaining[:i] + remaining[i+1:], result)
        
        # Undo the choice (backtrack)
        current.pop()
```

---

## **Example: Generating Subsets**

```python
def subsets(nums):
    result = []
    backtrack([], 0, nums, result)
    return result

def backtrack(current, start, nums, result):
    # Add the current subset to the result
    result.append(current.copy())
    
    for i in range(start, len(nums)):
        # Make a choice
        current.append(nums[i])
        
        # Recursively solve the subproblem
        backtrack(current, i + 1, nums, result)
        
        # Undo the choice (backtrack)
        current.pop()
```

---

## **Example: N-Queens**

```python
def solveNQueens(n):
    result = []
    board = [['.' for _ in range(n)] for _ in range(n)]
    backtrack(0, board, result, n)
    return result

def backtrack(row, board, result, n):
    if row == n:
        # Add the solution to the result
        result.append([''.join(row) for row in board])
        return
    
    for col in range(n):
        if is_valid(board, row, col, n):
            # Make a choice
            board[row][col] = 'Q'
            
            # Recursively solve the subproblem
            backtrack(row + 1, board, result, n)
            
            # Undo the choice (backtrack)
            board[row][col] = '.'

def is_valid(board, row, col, n):
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
```

---

## **Optimization Techniques**

1. **Pruning**: Stop exploring branches that cannot lead to a valid solution.
2. **Ordering**: Try the most promising choices first.
3. **Constraint Propagation**: Use constraints to reduce the search space.
4. **Symmetry Breaking**: Avoid exploring symmetric solutions.
5. **Memoization**: Cache results to avoid redundant computations.

---

## **Backtracking vs. Dynamic Programming**

| Aspect | Backtracking | Dynamic Programming |
|---|---|---|
| Problem Type | Decision problems, enumeration problems | Optimization problems |
| Approach | Explore all possible solutions | Build optimal solution from optimal subproblems |
| Time Complexity | Often exponential | Often polynomial |
| Space Complexity | Often linear (due to recursion stack) | Often polynomial |
| Typical Problems | Permutations, combinations, puzzles | Shortest path, knapsack, edit distance |

---

## **Backtracking vs. Brute Force**

* Backtracking is a refined version of brute force, where we prune branches that cannot lead to a valid solution.
* Brute force explores all possible solutions, even those that are clearly invalid.
* Backtracking is more efficient than brute force because it avoids exploring invalid solutions early on.

---

## **Real-life Analogy**

* Imagine you're trying to find your way through a maze. You start walking down a path, and if you reach a dead end, you backtrack to the last intersection and try a different path. This process continues until you either find the exit or explore all possible paths.
* Similarly, in backtracking, we explore a potential solution, and if it leads to an invalid state, we backtrack and try a different approach.

---

## **Implementation Considerations**

* **State Representation**: Choose an appropriate way to represent the current state of the solution.
* **Constraint Checking**: Implement efficient functions to check if a choice satisfies the constraints.
* **Termination Condition**: Define clear conditions for when a solution is complete or invalid.
* **Recursion Depth**: Be mindful of the recursion depth, as backtracking can lead to stack overflow for large problem sizes.

---

## **Summary**

* Backtracking is a powerful algorithmic technique for solving problems recursively by trying to build a solution incrementally.
* It's particularly useful for constraint satisfaction problems, such as puzzles, permutations, combinations, and subset problems.
* The key principles are choice, constraint, goal, and backtrack.
* Optimization techniques like pruning, ordering, and constraint propagation can significantly improve the efficiency of backtracking algorithms.
* While backtracking can be exponential in the worst case, it's often much more efficient than brute force due to early pruning of invalid solutions. 