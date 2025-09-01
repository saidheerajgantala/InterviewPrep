# 📝 Coin Change

## **Problem Statement**

* You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.
* Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.
* You may assume that you have an infinite number of each kind of coin.
* Example 1:
  * Input: coins = [1,2,5], amount = 11
  * Output: 3
  * Explanation: 11 = 5 + 5 + 1
* Example 2:
  * Input: coins = [2], amount = 3
  * Output: -1
* Example 3:
  * Input: coins = [1], amount = 0
  * Output: 0
* Constraints:
  * 1 <= coins.length <= 12
  * 1 <= coins[i] <= 2^31 - 1
  * 0 <= amount <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the minimum number of coins needed to make up a given amount
* For each amount from 1 to the target amount, we can try using each coin and see which gives us the minimum number of coins
* This suggests a recursive approach where we try each coin at each step and take the minimum
* For each amount, we have choices: use coin 1, use coin 2, ..., use coin n
* We need to handle the case where the amount cannot be made up by any combination of coins

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes the remaining amount to make up
2. Base cases:
   1. If the remaining amount is 0, we've successfully made up the amount, so return 0 (no more coins needed)
   2. If the remaining amount is negative, this combination is not valid, so return -1
3. Initialize minCoins to a large value (e.g., amount + 1 or infinity)
4. For each coin in the coins array:
   1. Recursively call the function with the remaining amount minus the coin value
   2. If the result is not -1 (i.e., a valid combination), update minCoins if the result plus 1 (for the current coin) is smaller
5. Return minCoins if it's still a valid value, otherwise return -1

---

## **Step 3: Brute Force Implementation (Code)**

```python
def coinChange(coins, amount):
    def min_coins(remaining):
        # Base cases
        if remaining == 0:
            return 0
        if remaining < 0:
            return -1
        
        # Try each coin and take the minimum
        min_count = float('inf')
        for coin in coins:
            result = min_coins(remaining - coin)
            if result != -1:
                min_count = min(min_count, result + 1)
        
        return min_count if min_count != float('inf') else -1
    
    return min_coins(amount)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(amount^n) where n is the number of coins. In the worst case, we make n recursive calls at each step, and the depth of the recursion tree is amount.
* Space Complexity: O(amount) for the recursion stack.
* This approach is inefficient for large amounts due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For coins = [1, 2, 5] and amount = 11:
  * min_coins(11):
    * Try coin 1: min_coins(10)
      * Try coin 1: min_coins(9)
        * ... (many recursive calls)
      * Try coin 2: min_coins(8)
        * ... (many recursive calls)
      * Try coin 5: min_coins(5)
        * ... (many recursive calls)
    * Try coin 2: min_coins(9)
      * Try coin 1: min_coins(8)
        * ... (many recursive calls)
      * Try coin 2: min_coins(7)
        * ... (many recursive calls)
      * Try coin 5: min_coins(4)
        * ... (many recursive calls)
    * Try coin 5: min_coins(6)
      * Try coin 1: min_coins(5)
        * ... (many recursive calls)
      * Try coin 2: min_coins(4)
        * ... (many recursive calls)
      * Try coin 5: min_coins(1)
        * Try coin 1: min_coins(0) -> 0
        * Try coin 2: min_coins(-1) -> -1
        * Try coin 5: min_coins(-4) -> -1
        * Return 1
      * Return 1 + 1 = 2
    * Return min(many results) = 3
* There's significant redundant computation in the recursive calls, especially for overlapping subproblems like min_coins(5)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the minimum number of coins for an amount depends only on the minimum number of coins for smaller amounts.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently determine the minimum number of coins for smaller amounts.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let dp[i] represent the minimum number of coins needed to make up amount i
* Base case: dp[0] = 0 (no coins needed to make up amount 0)
* For each amount i from 1 to the target amount:
  * Initialize dp[i] to a large value (e.g., amount + 1 or infinity)
  * For each coin in the coins array:
    * If i - coin >= 0 (i.e., we can use this coin), update dp[i] if dp[i - coin] + 1 is smaller
* The answer will be dp[amount] if it's a valid value, otherwise -1

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Create an array dp of size amount + 1, initialized with a large value (e.g., amount + 1)
2. Set dp[0] = 0 (base case)
3. For each amount i from 1 to the target amount:
   1. For each coin in the coins array:
      1. If i - coin >= 0, update dp[i] = min(dp[i], dp[i - coin] + 1)
4. Return dp[amount] if it's less than the initial large value, otherwise return -1

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def coinChange(coins, amount):
    # Initialize dp array with a value larger than the maximum possible number of coins
    dp = [amount + 1] * (amount + 1)
    dp[0] = 0  # Base case: no coins needed to make up amount 0
    
    for i in range(1, amount + 1):
        for coin in coins:
            if i - coin >= 0:
                dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] <= amount else -1
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(amount * n) where n is the number of coins. We process each amount once, and for each amount, we consider all n coins.
* Space Complexity: O(amount) for the dp array.
* This approach is much more efficient than the brute force approach for large amounts.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For coins = [1, 2, 5] and amount = 11:
  * Initialize dp = [0, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12] (where 12 is amount + 1)
  * i = 1:
    * coin = 1: dp[1] = min(12, dp[0] + 1) = min(12, 0 + 1) = 1
    * coin = 2: i - coin < 0, skip
    * coin = 5: i - coin < 0, skip
    * dp[1] = 1
  * i = 2:
    * coin = 1: dp[2] = min(12, dp[1] + 1) = min(12, 1 + 1) = 2
    * coin = 2: dp[2] = min(2, dp[0] + 1) = min(2, 0 + 1) = 1
    * coin = 5: i - coin < 0, skip
    * dp[2] = 1
  * i = 3:
    * coin = 1: dp[3] = min(12, dp[2] + 1) = min(12, 1 + 1) = 2
    * coin = 2: dp[3] = min(2, dp[1] + 1) = min(2, 1 + 1) = 2
    * coin = 5: i - coin < 0, skip
    * dp[3] = 2
  * ... (similar calculations for i = 4 to i = 10)
  * i = 11:
    * coin = 1: dp[11] = min(12, dp[10] + 1) = min(12, 3 + 1) = 4
    * coin = 2: dp[11] = min(4, dp[9] + 1) = min(4, 3 + 1) = 4
    * coin = 5: dp[11] = min(4, dp[6] + 1) = min(4, 2 + 1) = 3
    * dp[11] = 3
* dp[11] = 3, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(amount^n) | O(amount) | Inefficient for large amounts |
| Dynamic Programming (Tabulation) | O(amount * n) | O(amount) | Efficient for large amounts |

---

## **Final Takeaways**

* The key to efficiently solving the Coin Change problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using a bottom-up approach by building the solution from smaller subproblems.
* Special attention should be paid to the initialization of the dp array and the base case.
* This problem is a classic example of how dynamic programming can transform an exponential-time algorithm into a polynomial-time one.
* The Coin Change problem is a variation of the unbounded knapsack problem, where we can use each item (coin) multiple times.

---

## **Real-life Analogy**

* Imagine you're trying to pay for an item with the fewest number of coins possible. You have different denominations available, and you want to minimize the number of coins you use. The brute force approach would be to try all possible combinations of coins, which is inefficient. The dynamic programming approach would be to calculate the minimum number of coins for each smaller amount first, then use those results to calculate the minimum for the target amount, which is much more efficient.

---

## **Summary**

* We solved the Coin Change problem by first considering a brute force recursive approach that tries all possible combinations of coins, resulting in O(amount^n) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(amount * n). The key insight is to recognize that the minimum number of coins for an amount depends only on the minimum number of coins for smaller amounts, which allows us to build the solution incrementally from smaller amounts to the target amount. 