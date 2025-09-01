# 📝 Coin Change II

## **Problem Statement**

* You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.
* Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.
* You may assume that you have an infinite number of each kind of coin.
* The answer is guaranteed to fit into a signed 32-bit integer.
* Example 1:
  * Input: amount = 5, coins = [1,2,5]
  * Output: 4
  * Explanation: There are four ways to make up the amount:
    * 5=5
    * 5=2+2+1
    * 5=2+1+1+1
    * 5=1+1+1+1+1
* Example 2:
  * Input: amount = 3, coins = [2]
  * Output: 0
  * Explanation: The amount of 3 cannot be made up just with coins of 2.
* Example 3:
  * Input: amount = 10, coins = [10]
  * Output: 1
* Constraints:
  * 1 <= coins.length <= 300
  * 1 <= coins[i] <= 5000
  * All the values of coins are unique.
  * 0 <= amount <= 5000

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the number of ways to make up a given amount using the given coins
* We can use each coin an infinite number of times
* One approach would be to use recursion to try all possible combinations of coins
* For each coin, we can decide how many of that coin to use (from 0 to as many as possible)
* This suggests a recursive approach where we try different combinations of coins

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes the remaining amount and the index of the current coin
2. Base cases:
   1. If the remaining amount is 0, return 1 (found a valid combination)
   2. If the remaining amount is negative, return 0 (invalid combination)
   3. If we've used all coins and the amount is still positive, return 0 (can't make up the amount)
3. For the current coin, try using it 0, 1, 2, ... times as long as the amount remains non-negative
4. Sum up the number of ways for each choice and return the result

---

## **Step 3: Brute Force Implementation (Code)**

```python
def change(amount, coins):
    def count_ways(remaining, coin_index):
        # Base cases
        if remaining == 0:
            return 1
        if remaining < 0 or coin_index == len(coins):
            return 0
        
        # Try using the current coin 0, 1, 2, ... times
        ways = 0
        for i in range(0, remaining // coins[coin_index] + 1):
            ways += count_ways(remaining - i * coins[coin_index], coin_index + 1)
        
        return ways
    
    return count_ways(amount, 0)
```

An alternative brute force implementation using a simpler recursive structure:

```python
def change(amount, coins):
    def count_ways(remaining, coin_index):
        # Base cases
        if remaining == 0:
            return 1
        if remaining < 0 or coin_index == len(coins):
            return 0
        
        # Option 1: Skip the current coin
        skip = count_ways(remaining, coin_index + 1)
        
        # Option 2: Use the current coin
        use = count_ways(remaining - coins[coin_index], coin_index)
        
        return skip + use
    
    return count_ways(amount, 0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(amount^n) where n is the number of coins. In the worst case, we try all possible combinations of coins, which is exponential.
* Space Complexity: O(amount * n) for the recursion stack.
* This approach is inefficient for large amounts or a large number of coins due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For amount = 5, coins = [1, 2, 5]:
  * count_ways(5, 0):
    * skip = count_ways(5, 1):
      * ... (many recursive calls)
    * use = count_ways(4, 0):
      * skip = count_ways(4, 1):
        * ... (many recursive calls)
      * use = count_ways(3, 0):
        * ... (many recursive calls)
    * return skip + use
  * ... (many more recursive calls)
* There's redundant computation in the recursive calls, especially for overlapping subproblems like count_ways(3, 1)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the number of ways to make up an amount using a subset of coins depends only on the number of ways to make up smaller amounts using the same or fewer coins.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently calculate the number of ways to make up each amount using each subset of coins.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let dp[i][j] represent the number of ways to make up amount j using the first i coins
* Base case: dp[0][0] = 1 (there's one way to make up amount 0 using 0 coins: by not using any coin)
* For each coin and each amount:
  * If we don't use the current coin: dp[i][j] = dp[i-1][j]
  * If we use the current coin: dp[i][j] += dp[i][j - coins[i-1]] (if j >= coins[i-1])
* The answer will be dp[n][amount] where n is the number of coins

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Create a 2D array dp of size (n+1) x (amount+1), initialized with 0
2. Set dp[0][0] = 1 (base case)
3. For each coin i from 1 to n:
   1. For each amount j from 0 to amount:
      1. Don't use the current coin: dp[i][j] = dp[i-1][j]
      2. Use the current coin (if possible): dp[i][j] += dp[i][j - coins[i-1]] (if j >= coins[i-1])
4. Return dp[n][amount]

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def change(amount, coins):
    n = len(coins)
    
    # Create a 2D array filled with 0
    dp = [[0] * (amount + 1) for _ in range(n + 1)]
    
    # Base case: there's one way to make amount 0 (by not using any coin)
    dp[0][0] = 1
    
    # Fill the dp table
    for i in range(1, n + 1):
        dp[i][0] = 1  # One way to make amount 0 with any number of coins
        for j in range(1, amount + 1):
            # Don't use the current coin
            dp[i][j] = dp[i-1][j]
            
            # Use the current coin if possible
            if j >= coins[i-1]:
                dp[i][j] += dp[i][j - coins[i-1]]
    
    return dp[n][amount]
```

### **Space Optimization**

We can optimize the space complexity to O(amount) by using a 1D array:

```python
def change(amount, coins):
    # Create a 1D array filled with 0
    dp = [0] * (amount + 1)
    
    # Base case: there's one way to make amount 0 (by not using any coin)
    dp[0] = 1
    
    # For each coin, update the dp array
    for coin in coins:
        for j in range(coin, amount + 1):
            dp[j] += dp[j - coin]
    
    return dp[amount]
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(n * amount) where n is the number of coins. We fill a 2D table of size (n+1) x (amount+1).
* Space Complexity: O(n * amount) for the dp array.
* This approach is much more efficient than the brute force approach for large amounts or a large number of coins.

### **Complexity Analysis (Space-Optimized Version)**

* Time Complexity: O(n * amount) where n is the number of coins.
* Space Complexity: O(amount) for the 1D dp array.
* This approach is even more space-efficient while maintaining the same time complexity.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For amount = 5, coins = [1, 2, 5]:
  * Initialize dp:
    ```
      | 0 | 1 | 2 | 3 | 4 | 5
    --|---|---|---|---|---|---
    0 | 1 | 0 | 0 | 0 | 0 | 0
    1 | 0 | 0 | 0 | 0 | 0 | 0
    2 | 0 | 0 | 0 | 0 | 0 | 0
    3 | 0 | 0 | 0 | 0 | 0 | 0
    ```
  * Process coin 1:
    * dp[1][0] = 1
    * dp[1][1] = dp[0][1] + dp[1][1-1] = 0 + 1 = 1
    * dp[1][2] = dp[0][2] + dp[1][2-1] = 0 + 1 = 1
    * dp[1][3] = dp[0][3] + dp[1][3-1] = 0 + 1 = 1
    * dp[1][4] = dp[0][4] + dp[1][4-1] = 0 + 1 = 1
    * dp[1][5] = dp[0][5] + dp[1][5-1] = 0 + 1 = 1
    ```
      | 0 | 1 | 2 | 3 | 4 | 5
    --|---|---|---|---|---|---
    0 | 1 | 0 | 0 | 0 | 0 | 0
    1 | 1 | 1 | 1 | 1 | 1 | 1
    2 | 0 | 0 | 0 | 0 | 0 | 0
    3 | 0 | 0 | 0 | 0 | 0 | 0
    ```
  * Process coin 2:
    * dp[2][0] = 1
    * dp[2][1] = dp[1][1] = 1
    * dp[2][2] = dp[1][2] + dp[2][2-2] = 1 + 1 = 2
    * dp[2][3] = dp[1][3] + dp[2][3-2] = 1 + 1 = 2
    * dp[2][4] = dp[1][4] + dp[2][4-2] = 1 + 2 = 3
    * dp[2][5] = dp[1][5] + dp[2][5-2] = 1 + 2 = 3
    ```
      | 0 | 1 | 2 | 3 | 4 | 5
    --|---|---|---|---|---|---
    0 | 1 | 0 | 0 | 0 | 0 | 0
    1 | 1 | 1 | 1 | 1 | 1 | 1
    2 | 1 | 1 | 2 | 2 | 3 | 3
    3 | 0 | 0 | 0 | 0 | 0 | 0
    ```
  * Process coin 5:
    * dp[3][0] = 1
    * dp[3][1] = dp[2][1] = 1
    * dp[3][2] = dp[2][2] = 2
    * dp[3][3] = dp[2][3] = 2
    * dp[3][4] = dp[2][4] = 3
    * dp[3][5] = dp[2][5] + dp[3][5-5] = 3 + 1 = 4
    ```
      | 0 | 1 | 2 | 3 | 4 | 5
    --|---|---|---|---|---|---
    0 | 1 | 0 | 0 | 0 | 0 | 0
    1 | 1 | 1 | 1 | 1 | 1 | 1
    2 | 1 | 1 | 2 | 2 | 3 | 3
    3 | 1 | 1 | 2 | 2 | 3 | 4
    ```
  * dp[3][5] = 4, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Visualizing Computation (Space-Optimized Version)**

* For amount = 5, coins = [1, 2, 5]:
  * Initialize dp = [1, 0, 0, 0, 0, 0]
  * Process coin 1:
    * dp[1] += dp[1-1] = 0 + 1 = 1
    * dp[2] += dp[2-1] = 0 + 1 = 1
    * dp[3] += dp[3-1] = 0 + 1 = 1
    * dp[4] += dp[4-1] = 0 + 1 = 1
    * dp[5] += dp[5-1] = 0 + 1 = 1
    * dp = [1, 1, 1, 1, 1, 1]
  * Process coin 2:
    * dp[2] += dp[2-2] = 1 + 1 = 2
    * dp[3] += dp[3-2] = 1 + 1 = 2
    * dp[4] += dp[4-2] = 1 + 2 = 3
    * dp[5] += dp[5-2] = 1 + 2 = 3
    * dp = [1, 1, 2, 2, 3, 3]
  * Process coin 5:
    * dp[5] += dp[5-5] = 3 + 1 = 4
    * dp = [1, 1, 2, 2, 3, 4]
  * dp[5] = 4, which is the answer
* The space-optimized approach efficiently computes the result with minimal space usage

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(amount^n) | O(amount * n) | Inefficient for large amounts or many coins |
| Dynamic Programming (2D) | O(n * amount) | O(n * amount) | Efficient for medium-sized problems |
| Dynamic Programming (1D) | O(n * amount) | O(amount) | Space-optimized version |

---

## **Final Takeaways**

* The key to efficiently solving the Coin Change II problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using a bottom-up approach by building the solution from smaller subproblems.
* The space optimization from a 2D array to a 1D array is possible because we only need the results from the previous row to compute the current row.
* The order of the loops matters: to ensure we count each combination only once, we iterate over coins in the outer loop and amounts in the inner loop.
* This problem is a classic example of the "unbounded knapsack" problem, where we can use each item (coin) multiple times.

---

## **Real-life Analogy**

* Imagine you're a cashier trying to count the number of ways to give change for a purchase. The brute force approach would be to try all possible combinations of coins, which is inefficient. The dynamic programming approach would be to build up the solution by counting the number of ways to give change for smaller amounts first, which is much more efficient.

---

## **Summary**

* We solved the Coin Change II problem by first considering a brute force recursive approach that tries all possible combinations of coins, resulting in O(amount^n) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(n * amount). Finally, we explored a space optimization that reduces the space complexity from O(n * amount) to O(amount). The key insight is to recognize that the number of ways to make up an amount using a subset of coins depends only on the number of ways to make up smaller amounts using the same or fewer coins, which allows us to build the solution incrementally. 