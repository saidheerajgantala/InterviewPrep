# 📝 Min Cost Climbing Stairs

## **Problem Statement**

* You are given an integer array `cost` where `cost[i]` is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.
* You can either start from the step with index 0, or the step with index 1.
* Return the minimum cost to reach the top of the floor.
* Example 1:
  * Input: cost = [10,15,20]
  * Output: 15
  * Explanation: You will start at index 1.
    * Pay 15 and climb two steps to reach the top.
    * The total cost is 15.
* Example 2:
  * Input: cost = [1,100,1,1,1,100,1,1,100,1]
  * Output: 6
  * Explanation: You will start at index 0.
    * Pay 1 and climb two steps to reach index 2.
    * Pay 1 and climb two steps to reach index 4.
    * Pay 1 and climb two steps to reach index 6.
    * Pay 1 and climb one step to reach index 7.
    * Pay 1 and climb two steps to reach index 9.
    * Pay 1 and climb one step to reach the top.
    * The total cost is 6.
* Constraints:
  * 2 <= cost.length <= 1000
  * 0 <= cost[i] <= 999

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the minimum cost to reach the top of the staircase
* We can start from either step 0 or step 1
* At each step, we can either climb one or two steps
* One approach would be to use recursion to explore all possible paths and find the minimum cost
* For each position, we can either take one step or two steps, and then recursively solve the problem for the remaining steps

---

## **Step 2: Flow Steps (Brute Force)**

1. Define a recursive function `minCost(i)` that returns the minimum cost to reach the top from step `i`
2. Base cases:
   1. If `i >= len(cost)`, return 0 (we've reached the top)
3. Recursive case:
   1. Return `cost[i] + min(minCost(i + 1), minCost(i + 2))` (take 1 step or 2 steps)
4. Return `min(minCost(0), minCost(1))` to start from either step 0 or step 1

---

## **Step 3: Brute Force Implementation (Code)**

```python
def minCostClimbingStairs(cost):
    def minCost(i):
        if i >= len(cost):
            return 0
        return cost[i] + min(minCost(i + 1), minCost(i + 2))
    
    return min(minCost(0), minCost(1))
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n) because for each step, we have two choices (1 step or 2 steps), and we explore all possible combinations.
* Space Complexity: O(n) for the recursion stack.
* This approach is inefficient for large values of n due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For cost = [10, 15, 20]:
  * minCost(0) = 10 + min(minCost(1), minCost(2))
    * minCost(1) = 15 + min(minCost(2), minCost(3))
      * minCost(2) = 20 + min(minCost(3), minCost(4))
        * minCost(3) = 0
        * minCost(4) = 0
      * minCost(3) = 0
    * minCost(2) = 20 + min(minCost(3), minCost(4))
      * minCost(3) = 0
      * minCost(4) = 0
  * minCost(1) = 15 + min(minCost(2), minCost(3))
    * minCost(2) = 20 + min(minCost(3), minCost(4))
      * minCost(3) = 0
      * minCost(4) = 0
    * minCost(3) = 0
* Notice that we're calculating minCost(2), minCost(3), and minCost(4) multiple times

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant calculations.
* The property that matches this problem is that the minimum cost to reach the top from step i depends only on the minimum cost to reach the top from steps i+1 and i+2.
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

1. Define a recursive function `minCost(i, memo)` that returns the minimum cost to reach the top from step `i`
2. Before calculating, check if the result for `i` is already in the memo
3. If not, calculate it and store it in the memo
4. Base cases and recursive case remain the same as the brute force approach
5. Return `min(minCost(0, {}), minCost(1, {}))` to start from either step 0 or step 1

### **Step 3: Implementation (Optimized - Memoization)**

```python
def minCostClimbingStairs(cost):
    def minCost(i, memo):
        if i in memo:
            return memo[i]
        if i >= len(cost):
            return 0
        memo[i] = cost[i] + min(minCost(i + 1, memo), minCost(i + 2, memo))
        return memo[i]
    
    return min(minCost(0, {}), minCost(1, {}))
```

### **Step 3: Implementation (Optimized - Tabulation)**

```python
def minCostClimbingStairs(cost):
    n = len(cost)
    dp = [0] * (n + 1)
    
    for i in range(2, n + 1):
        dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2])
    
    return dp[n]
```

### **Step 3: Implementation (Optimized - Space-Efficient Tabulation)**

```python
def minCostClimbingStairs(cost):
    n = len(cost)
    prev1, prev2 = 0, 0
    
    for i in range(2, n + 1):
        curr = min(prev1 + cost[i - 1], prev2 + cost[i - 2])
        prev2, prev1 = prev1, curr
    
    return prev1
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(n) because we calculate the result for each step exactly once.
* Space Complexity:
  * Memoization: O(n) for the memo dictionary and recursion stack.
  * Tabulation: O(n) for the dp array.
  * Space-Efficient Tabulation: O(1) because we only use two variables.
* This approach is much more efficient than the brute force approach for large values of n.

### **Step 5: Visualizing Computation (Optimized - Tabulation)**

* For cost = [10, 15, 20]:
  * dp[0] = 0
  * dp[1] = 0
  * dp[2] = min(dp[1] + cost[1], dp[0] + cost[0]) = min(0 + 15, 0 + 10) = 10
  * dp[3] = min(dp[2] + cost[2], dp[1] + cost[1]) = min(10 + 20, 0 + 15) = 15
  * Return dp[3] = 15
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

* The key to efficiently solving the Min Cost Climbing Stairs problem is to use dynamic programming to avoid redundant calculations.
* Both memoization and tabulation approaches can be used, with tabulation often being more efficient in terms of space.
* This problem demonstrates the importance of recognizing overlapping subproblems and optimal substructure, which are key characteristics of problems that can be solved efficiently using dynamic programming.

---

## **Real-life Analogy**

* Imagine you're planning a hiking trip and want to minimize the energy expenditure to reach the summit. At each junction, you can either take a short path (1 step) or a long path (2 steps), each with its own energy cost. The minimum energy required to reach the summit from your current position depends on the minimum energy required to reach the summit from the next one or two junctions ahead.

---

## **Summary**

* We solved the Min Cost Climbing Stairs problem by first considering a brute force approach that uses recursion to explore all possible paths, resulting in O(2^n) time complexity. We then optimized using dynamic programming, either with memoization (top-down) or tabulation (bottom-up), reducing the time complexity to O(n). We further optimized the space complexity to O(1) by using only two variables to keep track of the previous two steps. The key insight is to recognize that the minimum cost to reach the top from step i is equal to the cost of step i plus the minimum of the minimum cost to reach the top from steps i+1 and i+2. 