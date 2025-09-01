# 📝 Best Time to Buy and Sell Stock with Cooldown

## **Problem Statement**

* You are given an array `prices` where `prices[i]` is the price of a given stock on the ith day.
* Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:
  * After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
  * Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
* Example 1:
  * Input: prices = [1,2,3,0,2]
  * Output: 3
  * Explanation: transactions = [buy, sell, cooldown, buy, sell]
* Example 2:
  * Input: prices = [1]
  * Output: 0
* Constraints:
  * 1 <= prices.length <= 5000
  * 0 <= prices[i] <= 1000

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the maximum profit by buying and selling stocks with a cooldown restriction
* At each day, we have three possible actions: buy, sell, or do nothing
* We can only buy if we don't currently hold a stock
* We can only sell if we currently hold a stock
* After selling, we must cooldown for one day (cannot buy on the next day)
* One approach would be to use recursion to try all possible combinations of actions and find the one that yields the maximum profit

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes the current day, whether we hold a stock, and whether we're in cooldown
2. Base case: If we've processed all days, return 0 (no more profit can be made)
3. If we're in cooldown, we can only do nothing on this day
4. If we don't hold a stock and are not in cooldown:
   1. Option 1: Buy the stock on this day
   2. Option 2: Do nothing on this day
   3. Take the maximum of these two options
5. If we hold a stock:
   1. Option 1: Sell the stock on this day
   2. Option 2: Do nothing on this day
   3. Take the maximum of these two options
6. Return the maximum profit

---

## **Step 3: Brute Force Implementation (Code)**

```python
def maxProfit(prices):
    def dfs(day, holding, cooldown):
        # Base case: reached the end of the array
        if day == len(prices):
            return 0
        
        # If in cooldown, can only do nothing
        if cooldown:
            return dfs(day + 1, holding, False)
        
        # Initialize profit for doing nothing
        do_nothing = dfs(day + 1, holding, False)
        
        if holding:
            # Option: Sell the stock
            sell = prices[day] + dfs(day + 1, False, True)
            return max(do_nothing, sell)
        else:
            # Option: Buy the stock
            buy = -prices[day] + dfs(day + 1, True, False)
            return max(do_nothing, buy)
    
    return dfs(0, False, False)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(3^n) where n is the number of days. At each day, we have at most 3 different states (holding, not holding, cooldown), and we explore all possible combinations.
* Space Complexity: O(n) for the recursion stack.
* This approach is inefficient for a large number of days due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For prices = [1, 2, 3, 0, 2]:
  * dfs(0, False, False):
    * do_nothing = dfs(1, False, False):
      * ... (many recursive calls)
    * buy = -1 + dfs(1, True, False):
      * ... (many recursive calls)
  * ... (many more recursive calls)
* There's redundant computation in the recursive calls, especially for overlapping subproblems like dfs(2, True, False)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the maximum profit at each day depends only on the maximum profit at future days and the current action.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently calculate the maximum profit for each state (day, holding, cooldown).

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming with memoization to avoid redundant computations
* Let's define a memoization table memo[day][holding][cooldown] to store the maximum profit for each state
* Alternatively, we can use a bottom-up approach with three state variables:
  * buy[i]: Maximum profit on day i if we have a stock in our hand
  * sell[i]: Maximum profit on day i if we don't have a stock and are not in cooldown
  * cooldown[i]: Maximum profit on day i if we don't have a stock and are in cooldown
* The transitions are:
  * buy[i] = max(buy[i-1], sell[i-1] - prices[i])
  * sell[i] = max(sell[i-1], cooldown[i-1])
  * cooldown[i] = buy[i-1] + prices[i]
* The answer will be max(sell[n-1], cooldown[n-1])

### **Step 2: Flow Steps (Optimized - Memoization)**

1. Create a recursive function with memoization that takes the current day, whether we hold a stock, and whether we're in cooldown
2. Use a dictionary to store computed results for each state
3. Implement the same logic as the brute force approach, but check the memoization table before computing
4. Start the recursion with day 0, not holding a stock, and not in cooldown

### **Step 3: Implementation (Optimized - Memoization)**

```python
def maxProfit(prices):
    memo = {}
    
    def dfs(day, holding, cooldown):
        # Base case: reached the end of the array
        if day == len(prices):
            return 0
        
        # Check if we've already computed this state
        if (day, holding, cooldown) in memo:
            return memo[(day, holding, cooldown)]
        
        # If in cooldown, can only do nothing
        if cooldown:
            memo[(day, holding, cooldown)] = dfs(day + 1, holding, False)
            return memo[(day, holding, cooldown)]
        
        # Initialize profit for doing nothing
        do_nothing = dfs(day + 1, holding, False)
        
        if holding:
            # Option: Sell the stock
            sell = prices[day] + dfs(day + 1, False, True)
            memo[(day, holding, cooldown)] = max(do_nothing, sell)
        else:
            # Option: Buy the stock
            buy = -prices[day] + dfs(day + 1, True, False)
            memo[(day, holding, cooldown)] = max(do_nothing, buy)
        
        return memo[(day, holding, cooldown)]
    
    return dfs(0, False, False)
```

### **Bottom-Up Dynamic Programming**

```python
def maxProfit(prices):
    n = len(prices)
    if n <= 1:
        return 0
    
    # Initialize state arrays
    buy = [0] * n
    sell = [0] * n
    cooldown = [0] * n
    
    # Base cases
    buy[0] = -prices[0]  # Buy on day 0
    sell[0] = 0          # Do nothing on day 0
    cooldown[0] = 0      # No cooldown on day 0
    
    for i in range(1, n):
        # Maximum profit if we have a stock in hand on day i
        buy[i] = max(buy[i-1], sell[i-1] - prices[i])
        
        # Maximum profit if we don't have a stock and are not in cooldown on day i
        sell[i] = max(sell[i-1], cooldown[i-1])
        
        # Maximum profit if we don't have a stock and are in cooldown on day i
        cooldown[i] = buy[i-1] + prices[i]
    
    # The answer is the maximum profit on the last day without holding a stock
    return max(sell[n-1], cooldown[n-1])
```

### **Space-Optimized Bottom-Up Dynamic Programming**

```python
def maxProfit(prices):
    n = len(prices)
    if n <= 1:
        return 0
    
    # Initialize state variables
    buy = -prices[0]
    sell = 0
    cooldown = 0
    
    for i in range(1, n):
        prev_buy = buy
        prev_sell = sell
        prev_cooldown = cooldown
        
        # Update state variables
        buy = max(prev_buy, prev_sell - prices[i])
        sell = max(prev_sell, prev_cooldown)
        cooldown = prev_buy + prices[i]
    
    # The answer is the maximum profit on the last day without holding a stock
    return max(sell, cooldown)
```

### **Step 4: Complexity Analysis (Optimized - Memoization)**

* Time Complexity: O(n) where n is the number of days. We have 3 * n different states, and each state is computed once.
* Space Complexity: O(n) for the memoization table.
* This approach is much more efficient than the brute force approach for a large number of days.

### **Complexity Analysis (Bottom-Up Dynamic Programming)**

* Time Complexity: O(n) where n is the number of days. We process each day once.
* Space Complexity: O(n) for the state arrays.
* This approach is also efficient and avoids the overhead of recursion.

### **Complexity Analysis (Space-Optimized Bottom-Up Dynamic Programming)**

* Time Complexity: O(n) where n is the number of days. We process each day once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is the most space-efficient while maintaining the same time complexity.

### **Step 5: Visualizing Computation (Optimized - Bottom-Up Dynamic Programming)**

* For prices = [1, 2, 3, 0, 2]:
  * Initialize:
    * buy[0] = -1, sell[0] = 0, cooldown[0] = 0
  * i = 1 (price = 2):
    * buy[1] = max(buy[0], sell[0] - prices[1]) = max(-1, 0 - 2) = -1
    * sell[1] = max(sell[0], cooldown[0]) = max(0, 0) = 0
    * cooldown[1] = buy[0] + prices[1] = -1 + 2 = 1
  * i = 2 (price = 3):
    * buy[2] = max(buy[1], sell[1] - prices[2]) = max(-1, 0 - 3) = -1
    * sell[2] = max(sell[1], cooldown[1]) = max(0, 1) = 1
    * cooldown[2] = buy[1] + prices[2] = -1 + 3 = 2
  * i = 3 (price = 0):
    * buy[3] = max(buy[2], sell[2] - prices[3]) = max(-1, 1 - 0) = 1
    * sell[3] = max(sell[2], cooldown[2]) = max(1, 2) = 2
    * cooldown[3] = buy[2] + prices[3] = -1 + 0 = -1
  * i = 4 (price = 2):
    * buy[4] = max(buy[3], sell[3] - prices[4]) = max(1, 2 - 2) = 1
    * sell[4] = max(sell[3], cooldown[3]) = max(2, -1) = 2
    * cooldown[4] = buy[3] + prices[4] = 1 + 2 = 3
  * max(sell[4], cooldown[4]) = max(2, 3) = 3, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(3^n) | O(n) | Inefficient for a large number of days |
| Dynamic Programming (Memoization) | O(n) | O(n) | Efficient with memoization |
| Dynamic Programming (Bottom-Up) | O(n) | O(n) | Efficient with state arrays |
| Dynamic Programming (Space-Optimized) | O(n) | O(1) | Most space-efficient approach |

---

## **Final Takeaways**

* The key to efficiently solving the Best Time to Buy and Sell Stock with Cooldown problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using either a top-down approach with memoization or a bottom-up approach with state arrays.
* The space optimization from O(n) to O(1) is possible because we only need the results from the previous day to compute the current day.
* This problem demonstrates the importance of carefully defining the state transitions in dynamic programming problems with complex constraints.
* The cooldown constraint adds an extra dimension to the state space compared to the basic stock trading problem, making it more challenging but still solvable with dynamic programming.

---

## **Real-life Analogy**

* Imagine you're a trader who wants to maximize profits but must follow a rule: after selling a stock, you must take a day off before buying again. The brute force approach would be to try all possible combinations of buy/sell/rest decisions, which is inefficient. The dynamic programming approach would be to keep track of the maximum profit for each state (holding a stock, not holding a stock, in cooldown) and build the solution incrementally, which is much more efficient.

---

## **Summary**

* We solved the Best Time to Buy and Sell Stock with Cooldown problem by first considering a brute force recursive approach that tries all possible combinations of actions, resulting in O(3^n) time complexity. We then optimized using dynamic programming with memoization to avoid redundant computations, reducing the time complexity to O(n). We also explored a bottom-up dynamic programming approach and a space-optimized version that reduces the space complexity from O(n) to O(1). The key insight is to recognize that the maximum profit at each day depends only on the maximum profit at future days and the current action, which allows us to build the solution incrementally. 