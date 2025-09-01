# 📝 Dynamic Programming

## **Concept Overview**

* Dynamic Programming (DP) is a method for solving complex problems by breaking them down into simpler subproblems.
* It is applicable when the problem has overlapping subproblems and optimal substructure.
* Overlapping subproblems means the same subproblems are solved multiple times.
* Optimal substructure means the optimal solution to the problem can be constructed from optimal solutions of its subproblems.
* DP uses memoization (top-down) or tabulation (bottom-up) to avoid redundant calculations.

---

## **Key Principles**

1. **Break Down the Problem**: Divide the problem into smaller subproblems.
2. **Solve Subproblems**: Solve each subproblem once and store the result.
3. **Reuse Solutions**: When the same subproblem occurs again, look up the previously computed solution.
4. **Build Up the Solution**: Combine the solutions of subproblems to solve the original problem.

---

## **Approaches to Dynamic Programming**

### **Top-Down Approach (Memoization)**

* Start with the original problem and recursively break it down into smaller subproblems.
* Use a cache (usually a dictionary or array) to store the results of subproblems.
* Before solving a subproblem, check if its result is already in the cache.

```python
def fibonacci(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 2:
        return 1
    memo[n] = fibonacci(n-1, memo) + fibonacci(n-2, memo)
    return memo[n]
```

### **Bottom-Up Approach (Tabulation)**

* Start with the smallest subproblems and build up to the original problem.
* Use an array or table to store the results of subproblems.
* Fill the table iteratively, using previously computed values.

```python
def fibonacci(n):
    if n <= 2:
        return 1
    dp = [0] * (n + 1)
    dp[1] = dp[2] = 1
    for i in range(3, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

---

## **Common Dynamic Programming Patterns**

### **1. Linear DP**

* Solve problems in a linear fashion, where the state depends on previous states.
* Examples: Fibonacci, Climbing Stairs, House Robber.

```python
def climb_stairs(n):
    if n <= 2:
        return n
    dp = [0] * (n + 1)
    dp[1] = 1
    dp[2] = 2
    for i in range(3, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

### **2. Two-Dimensional DP**

* Use a 2D table to store results, where the state depends on multiple dimensions.
* Examples: Longest Common Subsequence, Edit Distance, Minimum Path Sum.

```python
def longest_common_subsequence(text1, text2):
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]
```

### **3. State Compression**

* Reduce the space complexity by using only the necessary states.
* Examples: Knapsack Problem with space optimization.

```python
def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [0] * (capacity + 1)
    
    for i in range(n):
        for w in range(capacity, weights[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    
    return dp[capacity]
```

### **4. Interval DP**

* Solve problems involving intervals or subarrays.
* Examples: Matrix Chain Multiplication, Burst Balloons.

```python
def matrix_chain_multiplication(p):
    n = len(p) - 1
    dp = [[0] * n for _ in range(n)]
    
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            dp[i][j] = float('inf')
            for k in range(i, j):
                dp[i][j] = min(dp[i][j], dp[i][k] + dp[k+1][j] + p[i] * p[k+1] * p[j+1])
    
    return dp[0][n-1]
```

### **5. Decision Making**

* Make decisions at each step to maximize or minimize a value.
* Examples: House Robber, Stock Trading.

```python
def house_robber(nums):
    if not nums:
        return 0
    if len(nums) == 1:
        return nums[0]
    
    dp = [0] * len(nums)
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])
    
    for i in range(2, len(nums)):
        dp[i] = max(dp[i-1], dp[i-2] + nums[i])
    
    return dp[-1]
```

---

## **Steps to Solve DP Problems**

1. **Identify if it's a DP Problem**:
   * Check if the problem has overlapping subproblems and optimal substructure.
   * Look for keywords like "maximum/minimum," "count ways," "optimize," etc.

2. **Define the State**:
   * Determine what information you need to represent a subproblem.
   * For example, in the Fibonacci sequence, the state is the position `n`.

3. **Formulate the Recurrence Relation**:
   * Express the solution to the current state in terms of solutions to smaller states.
   * For example, in Fibonacci, `F(n) = F(n-1) + F(n-2)`.

4. **Identify the Base Cases**:
   * Determine the simplest subproblems that can be solved directly.
   * For example, in Fibonacci, `F(1) = F(2) = 1`.

5. **Decide the Approach**:
   * Choose between top-down (memoization) and bottom-up (tabulation) based on the problem.

6. **Implement the Solution**:
   * Write the code, ensuring that you handle all edge cases.

7. **Optimize if Necessary**:
   * Look for ways to reduce space complexity or improve time complexity.

---

## **Common DP Problems and Their Solutions**

### **Fibonacci Sequence**

* Problem: Find the nth Fibonacci number.
* State: `dp[i]` = the ith Fibonacci number.
* Recurrence Relation: `dp[i] = dp[i-1] + dp[i-2]`.
* Base Cases: `dp[1] = dp[2] = 1`.

### **Climbing Stairs**

* Problem: Count the number of ways to climb n stairs, taking 1 or 2 steps at a time.
* State: `dp[i]` = number of ways to climb i stairs.
* Recurrence Relation: `dp[i] = dp[i-1] + dp[i-2]`.
* Base Cases: `dp[1] = 1, dp[2] = 2`.

### **Longest Common Subsequence**

* Problem: Find the length of the longest subsequence common to two sequences.
* State: `dp[i][j]` = length of LCS of text1[0...i-1] and text2[0...j-1].
* Recurrence Relation:
  * If text1[i-1] == text2[j-1], then `dp[i][j] = dp[i-1][j-1] + 1`.
  * Otherwise, `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.
* Base Cases: `dp[0][j] = dp[i][0] = 0`.

### **Knapsack Problem**

* Problem: Maximize value while keeping weight under capacity.
* State: `dp[i][w]` = maximum value with first i items and capacity w.
* Recurrence Relation:
  * If weight[i-1] <= w, then `dp[i][w] = max(dp[i-1][w], dp[i-1][w-weight[i-1]] + value[i-1])`.
  * Otherwise, `dp[i][w] = dp[i-1][w]`.
* Base Cases: `dp[0][w] = 0, dp[i][0] = 0`.

---

## **Dynamic Programming vs. Other Techniques**

### **DP vs. Greedy Algorithms**

* Greedy algorithms make locally optimal choices at each step.
* DP considers all possible choices and selects the best overall solution.
* Greedy is faster but doesn't always give the optimal solution.
* DP is slower but guarantees the optimal solution for problems with optimal substructure.

### **DP vs. Divide and Conquer**

* Both break down problems into smaller subproblems.
* Divide and conquer subproblems are independent and don't overlap.
* DP subproblems often overlap, and solutions are cached for reuse.

### **DP vs. Backtracking**

* Backtracking explores all possible solutions and backtracks when a solution is invalid.
* DP builds solutions incrementally and avoids redundant calculations.
* Backtracking is used for problems where we need to find all solutions.
* DP is used for optimization problems where we need to find the best solution.

---

## **Real-life Analogy**

* Imagine you're planning a road trip and want to find the shortest path from your home to a destination. You could calculate the distance for every possible route (brute force), or you could use a map that already has distances between cities (DP). By using the map, you avoid recalculating distances for common segments of different routes.

---

## **Implementation Considerations**

* **Memory Usage**: DP can use a lot of memory, especially for problems with large inputs or multiple dimensions.
* **Time Complexity**: DP trades time for space, reducing time complexity by using extra space.
* **Recursion Limit**: Top-down DP may hit recursion limits for large inputs.
* **Initialization**: Properly initialize the DP table to avoid undefined behavior.
* **State Transition**: Ensure the state transition is correct and covers all cases.

---

## **Summary**

* Dynamic Programming is a powerful technique for solving optimization problems by breaking them down into simpler subproblems.
* It is applicable when the problem has overlapping subproblems and optimal substructure.
* DP uses memoization (top-down) or tabulation (bottom-up) to avoid redundant calculations.
* Common DP patterns include linear DP, two-dimensional DP, state compression, interval DP, and decision making.
* To solve a DP problem, identify if it's a DP problem, define the state, formulate the recurrence relation, identify the base cases, decide the approach, implement the solution, and optimize if necessary.
* DP is more efficient than brute force for problems with overlapping subproblems, but it may use more memory. 