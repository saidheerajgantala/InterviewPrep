# 📝 Climbing Stairs

## **Problem Statement**

* You are climbing a staircase. It takes n steps to reach the top.
* Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?
* Example 1:
  * Input: n = 2
  * Output: 2
  * Explanation: There are two ways to climb to the top.
    * 1 step + 1 step
    * 2 steps
* Example 2:
  * Input: n = 3
  * Output: 3
  * Explanation: There are three ways to climb to the top.
    * 1 step + 1 step + 1 step
    * 1 step + 2 steps
    * 2 steps + 1 step
* Constraints:
  * 1 <= n <= 45

---

## **Step 1: Thinking Process (Brute Force)**

* We need to count the number of distinct ways to climb to the top of the staircase
* At each step, we can either take 1 step or 2 steps
* One approach would be to use recursion to explore all possible combinations of steps
* For each position, we can either take 1 step or 2 steps, and then recursively solve the problem for the remaining steps

---

## **Step 2: Flow Steps (Brute Force)**

1. Define a recursive function `climb(i)` that returns the number of ways to climb from step `i` to the top
2. Base cases:
   1. If `i > n`, return 0 (we've gone beyond the top)
   2. If `i == n`, return 1 (we've reached the top)
3. Recursive case:
   1. Return `climb(i + 1) + climb(i + 2)` (take 1 step or 2 steps)
4. Call `climb(0)` to start from the bottom

---

## **Step 3: Brute Force Implementation (Code)**

```python
def climbStairs(n):
    def climb(i):
        if i > n:
            return 0
        if i == n:
            return 1
        return climb(i + 1) + climb(i + 2)
    
    return climb(0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n) because for each step, we have two choices (1 step or 2 steps), and we explore all possible combinations.
* Space Complexity: O(n) for the recursion stack.
* This approach is inefficient for large values of n due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For n = 5:
  * climb(0) = climb(1) + climb(2)
    * climb(1) = climb(2) + climb(3)
      * climb(2) = climb(3) + climb(4)
        * climb(3) = climb(4) + climb(5)
          * climb(4) = climb(5) + climb(6)
            * climb(5) = 1
            * climb(6) = 0
          * climb(5) = 1
        * climb(4) = 1
      * climb(3) = 2
    * climb(2) = 3
  * climb(0) = 5
* Notice that we're calculating climb(2), climb(3), climb(4), and climb(5) multiple times

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant calculations.
* The property that matches this problem is that the number of ways to climb to step n depends only on the number of ways to climb to steps n-1 and n-2.
* By using memoization or tabulation, we can avoid recalculating the same values.
* Alternatives like the brute force approach would be inefficient due to redundant calculations.
* Precondition: We need to be able to store and reuse previously calculated results.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant calculations
* We'll use either memoization (top-down) or tabulation (bottom-up) approach
* For memoization, we'll use a dictionary or array to store the results of subproblems
* For tabulation, we'll build up the solution from the base cases

### **Step 2: Flow Steps (Optimized - Memoization)**

1. Define a recursive function `climb(i, memo)` that returns the number of ways to climb from step `i` to the top
2. Before calculating, check if the result for `i` is already in the memo
3. If not, calculate it and store it in the memo
4. Base cases and recursive case remain the same as the brute force approach
5. Call `climb(0, {})` to start from the bottom with an empty memo

### **Step 3: Implementation (Optimized - Memoization)**

```python
def climbStairs(n):
    def climb(i, memo):
        if i in memo:
            return memo[i]
        if i > n:
            return 0
        if i == n:
            return 1
        memo[i] = climb(i + 1, memo) + climb(i + 2, memo)
        return memo[i]
    
    return climb(0, {})
```

### **Step 3: Implementation (Optimized - Tabulation)**

```python
def climbStairs(n):
    if n <= 2:
        return n
    
    dp = [0] * (n + 1)
    dp[1] = 1
    dp[2] = 2
    
    for i in range(3, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    
    return dp[n]
```

### **Step 3: Implementation (Optimized - Space-Efficient Tabulation)**

```python
def climbStairs(n):
    if n <= 2:
        return n
    
    a, b = 1, 2
    for i in range(3, n + 1):
        a, b = b, a + b
    
    return b
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(n) because we calculate the result for each step exactly once.
* Space Complexity:
  * Memoization: O(n) for the memo dictionary and recursion stack.
  * Tabulation: O(n) for the dp array.
  * Space-Efficient Tabulation: O(1) because we only use two variables.
* This approach is much more efficient than the brute force approach for large values of n.

### **Step 5: Visualizing Computation (Optimized - Tabulation)**

* For n = 5:
  * dp[1] = 1
  * dp[2] = 2
  * dp[3] = dp[2] + dp[1] = 2 + 1 = 3
  * dp[4] = dp[3] + dp[2] = 3 + 2 = 5
  * dp[5] = dp[4] + dp[3] = 5 + 3 = 8
  * Return dp[5] = 8
* The tabulation approach efficiently builds up the solution from the base cases

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(2^n) | O(n) | Inefficient due to redundant calculations |
| Memoization | O(n) | O(n) | Efficient with top-down approach |
| Tabulation | O(n) | O(n) | Efficient with bottom-up approach |
| Space-Efficient Tabulation | O(n) | O(1) | Most efficient in terms of space |

---

## **Final Takeaways**

* The key to efficiently solving the Climbing Stairs problem is to recognize that it follows the Fibonacci sequence.
* Dynamic programming is a powerful technique for problems with overlapping subproblems.
* Both memoization and tabulation approaches can be used, with tabulation often being more efficient in terms of space.
* This problem demonstrates the importance of recognizing patterns and avoiding redundant calculations.

---

## **Real-life Analogy**

* Imagine you're planning a hiking trip and want to know how many different routes you can take to reach the summit. At each junction, you can either take a short path (1 step) or a long path (2 steps). The number of ways to reach the summit from your current position depends on the number of ways to reach the summit from the next one or two junctions ahead.

---

## **Summary**

* We solved the Climbing Stairs problem by first considering a brute force approach that uses recursion to explore all possible combinations of steps, resulting in O(2^n) time complexity. We then optimized using dynamic programming, either with memoization (top-down) or tabulation (bottom-up), reducing the time complexity to O(n). We further optimized the space complexity to O(1) by using only two variables to keep track of the previous two steps. The key insight is to recognize that the number of ways to climb to step n is equal to the number of ways to climb to step n-1 plus the number of ways to climb to step n-2, which follows the Fibonacci sequence. 