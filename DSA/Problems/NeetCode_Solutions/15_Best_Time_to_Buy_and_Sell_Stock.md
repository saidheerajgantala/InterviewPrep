# 📝 Best Time to Buy and Sell Stock

## **Problem Statement**

* You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day.
* You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.
* Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.
* Example:
  * Input: prices = [7,1,5,3,6,4]
  * Output: 5
  * Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
  * Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
* Constraints:
  * 1 <= prices.length <= 10^5
  * 0 <= prices[i] <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the maximum profit by buying and selling the stock once
* The profit is determined by the difference between the selling price and the buying price
* The most straightforward approach is to check all possible buy and sell combinations
* For each possible buying day, we check all possible selling days after it
* We keep track of the maximum profit we can achieve

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize `max_profit = 0`
2. For each index i from 0 to n-2 (buying day):
   1. For each index j from i+1 to n-1 (selling day):
      1. Calculate profit = prices[j] - prices[i]
      2. Update `max_profit = max(max_profit, profit)`
3. Return `max_profit`

---

## **Step 3: Brute Force Implementation (Code)**

```python
def maxProfit(prices):
    n = len(prices)
    max_profit = 0
    
    for i in range(n - 1):
        for j in range(i + 1, n):
            profit = prices[j] - prices[i]
            max_profit = max(max_profit, profit)
    
    return max_profit
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the length of the array. We have two nested loops.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient for large arrays and will likely result in a time limit exceeded error.

---

## **Step 5: Visualizing Redundant Computation**

* For array [7,1,5,3,6,4]:
  * For buying day i=0 (price=7), we check selling days 1 through 5
  * For buying day i=1 (price=1), we check selling days 2 through 5
  * For buying day i=2 (price=5), we check selling days 3 through 5
  * ...and so on
* We're repeatedly checking many combinations that clearly won't give the maximum profit
* For example, if we find a lower buying price later, we don't need to consider the higher buying prices we've seen before

---

## **Why are we using this algorithm?**

* For optimization, we'll use a One-Pass algorithm.
* The property that matches this problem is the ability to track the minimum price seen so far and the maximum profit.
* By keeping track of these values as we iterate through the array, we can find the maximum profit in a single pass.
* Alternatives like dynamic programming would be more complex and less efficient.
* Precondition: We need to buy before we sell.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of checking all possible buy and sell combinations, we can use a greedy approach
* As we iterate through the array, we keep track of:
  * The minimum price we've seen so far (potential buying price)
  * The maximum profit we can achieve by selling at the current price
* For each day, we update the minimum price if the current price is lower
* We also calculate the profit if we sell at the current price (using the minimum price seen so far) and update the maximum profit if it's higher

### **Step 2: Flow Steps (Optimized)**

1. Initialize `min_price = infinity` and `max_profit = 0`
2. For each price in the array:
   1. Update `min_price = min(min_price, price)`
   2. Calculate potential profit = price - min_price
   3. Update `max_profit = max(max_profit, potential profit)`
3. Return `max_profit`

### **Step 3: Implementation (Optimized)**

```python
def maxProfit(prices):
    n = len(prices)
    if n <= 1:
        return 0
    
    min_price = float('inf')
    max_profit = 0
    
    for price in prices:
        # Update minimum price seen so far
        min_price = min(min_price, price)
        
        # Calculate potential profit and update maximum profit
        potential_profit = price - min_price
        max_profit = max(max_profit, potential_profit)
    
    return max_profit
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. We make a single pass through the array.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This is much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For array [7,1,5,3,6,4]:
  * Initialize min_price = infinity, max_profit = 0
  * For price=7: min_price = 7, potential_profit = 0, max_profit = 0
  * For price=1: min_price = 1, potential_profit = 0, max_profit = 0
  * For price=5: min_price = 1, potential_profit = 4, max_profit = 4
  * For price=3: min_price = 1, potential_profit = 2, max_profit = 4
  * For price=6: min_price = 1, potential_profit = 5, max_profit = 5
  * For price=4: min_price = 1, potential_profit = 3, max_profit = 5
  * Return max_profit = 5
* We're efficiently finding the maximum profit by keeping track of the minimum price seen so far.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Checks all possible buy and sell combinations |
| Optimized (Algorithm: One-Pass) | O(n) | O(1) | Tracks minimum price and maximum profit |

---

## **Final Takeaways**

* Using a greedy approach allows us to efficiently find the maximum profit in a single pass.
* The key insight is that we only need to keep track of the minimum price seen so far and the maximum profit.
* This problem demonstrates how understanding the structure of the problem can lead to simple and efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're a trader trying to find the best day to buy and sell a stock. The brute force approach would be like looking at every possible combination of buying and selling days and calculating the profit for each. The optimized approach is like keeping a mental note of the lowest price you've seen so far and continuously calculating what your profit would be if you sold today, updating your maximum potential profit as you go along.

---

## **Summary**

* We solved the "Best Time to Buy and Sell Stock" problem by first considering a brute force approach that checks all possible buy and sell combinations, resulting in O(n²) time complexity. We then optimized using a one-pass approach that tracks the minimum price seen so far and the maximum profit, reducing the time complexity to O(n) while maintaining O(1) space complexity. This demonstrates how a simple greedy approach can lead to significant efficiency improvements. 