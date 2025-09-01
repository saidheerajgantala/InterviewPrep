# 📝 House Robber

## **Problem Statement**

* You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
* Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.
* Example 1:
  * Input: nums = [1,2,3,1]
  * Output: 4
  * Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3). Total amount you can rob = 1 + 3 = 4.
* Example 2:
  * Input: nums = [2,7,9,3,1]
  * Output: 12
  * Explanation: Rob house 1 (money = 2), rob house 3 (money = 9), and rob house 5 (money = 1). Total amount you can rob = 2 + 9 + 1 = 12.
* Constraints:
  * 1 <= nums.length <= 100
  * 0 <= nums[i] <= 400

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the maximum amount of money that can be robbed without robbing adjacent houses
* At each house, we have two choices: rob it or skip it
* If we rob a house, we cannot rob the adjacent houses
* One approach would be to use recursion to explore all possible combinations of houses to rob
* For each house, we can either rob it and skip the next house, or skip it and consider the next house

---

## **Step 2: Flow Steps (Brute Force)**

1. Define a recursive function `rob(i)` that returns the maximum amount of money that can be robbed from houses `i` to the end
2. Base cases:
   1. If `i >= len(nums)`, return 0 (no more houses to rob)
3. Recursive case:
   1. Return `max(nums[i] + rob(i + 2), rob(i + 1))` (rob current house and skip next, or skip current house)
4. Call `rob(0)` to start from the first house

---

## **Step 3: Brute Force Implementation (Code)**

```python
def rob(nums):
    def rob_recursive(i):
        if i >= len(nums):
            return 0
        return max(nums[i] + rob_recursive(i + 2), rob_recursive(i + 1))
    
    return rob_recursive(0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n) because for each house, we have two choices (rob it or skip it), and we explore all possible combinations.
* Space Complexity: O(n) for the recursion stack.
* This approach is inefficient for large values of n due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For nums = [1, 2, 3, 1]:
  * rob(0) = max(1 + rob(2), rob(1))
    * rob(2) = max(3 + rob(4), rob(3))
      * rob(4) = 0
      * rob(3) = max(1 + rob(5), rob(4))
        * rob(5) = 0
        * rob(4) = 0
    * rob(1) = max(2 + rob(3), rob(2))
      * rob(3) = max(1 + rob(5), rob(4))
        * rob(5) = 0
        * rob(4) = 0
      * rob(2) = max(3 + rob(4), rob(3))
        * rob(4) = 0
        * rob(3) = max(1 + rob(5), rob(4))
          * rob(5) = 0
          * rob(4) = 0
* Notice that we're calculating rob(2), rob(3), and rob(4) multiple times

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant calculations.
* The property that matches this problem is that the maximum amount of money that can be robbed from houses i to the end depends only on the maximum amount of money that can be robbed from houses i+1 and i+2 to the end.
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

1. Define a recursive function `rob(i, memo)` that returns the maximum amount of money that can be robbed from houses `i` to the end
2. Before calculating, check if the result for `i` is already in the memo
3. If not, calculate it and store it in the memo
4. Base cases and recursive case remain the same as the brute force approach
5. Call `rob(0, {})` to start from the first house with an empty memo

### **Step 3: Implementation (Optimized - Memoization)**

```python
def rob(nums):
    def rob_recursive(i, memo):
        if i in memo:
            return memo[i]
        if i >= len(nums):
            return 0
        memo[i] = max(nums[i] + rob_recursive(i + 2, memo), rob_recursive(i + 1, memo))
        return memo[i]
    
    return rob_recursive(0, {})
```

### **Step 3: Implementation (Optimized - Tabulation)**

```python
def rob(nums):
    if not nums:
        return 0
    if len(nums) == 1:
        return nums[0]
    
    dp = [0] * len(nums)
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])
    
    for i in range(2, len(nums)):
        dp[i] = max(dp[i - 1], dp[i - 2] + nums[i])
    
    return dp[-1]
```

### **Step 3: Implementation (Optimized - Space-Efficient Tabulation)**

```python
def rob(nums):
    if not nums:
        return 0
    if len(nums) == 1:
        return nums[0]
    
    prev2 = nums[0]
    prev1 = max(nums[0], nums[1])
    
    for i in range(2, len(nums)):
        curr = max(prev1, prev2 + nums[i])
        prev2, prev1 = prev1, curr
    
    return prev1
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(n) because we calculate the result for each house exactly once.
* Space Complexity:
  * Memoization: O(n) for the memo dictionary and recursion stack.
  * Tabulation: O(n) for the dp array.
  * Space-Efficient Tabulation: O(1) because we only use two variables.
* This approach is much more efficient than the brute force approach for large values of n.

### **Step 5: Visualizing Computation (Optimized - Tabulation)**

* For nums = [1, 2, 3, 1]:
  * dp[0] = 1
  * dp[1] = max(1, 2) = 2
  * dp[2] = max(dp[1], dp[0] + nums[2]) = max(2, 1 + 3) = 4
  * dp[3] = max(dp[2], dp[1] + nums[3]) = max(4, 2 + 1) = 4
  * Return dp[3] = 4
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

* The key to efficiently solving the House Robber problem is to use dynamic programming to avoid redundant calculations.
* The problem has a clear recurrence relation: the maximum amount of money that can be robbed up to house i is the maximum of:
  * The maximum amount of money that can be robbed up to house i-1 (skip house i)
  * The maximum amount of money that can be robbed up to house i-2 plus the money in house i (rob house i)
* Both memoization and tabulation approaches can be used, with tabulation often being more efficient in terms of space.
* This problem demonstrates the importance of recognizing overlapping subproblems and optimal substructure, which are key characteristics of problems that can be solved efficiently using dynamic programming.

---

## **Real-life Analogy**

* Imagine you're planning a vacation and can only visit one city per day. Some cities have festivals on specific days that you don't want to miss, but you can't visit adjacent cities on consecutive days due to travel constraints. You want to maximize the number of festivals you can attend. The maximum number of festivals you can attend up to a certain day depends on the maximum number of festivals you can attend up to the previous day or the day before that.

---

## **Summary**

* We solved the House Robber problem by first considering a brute force approach that uses recursion to explore all possible combinations of houses to rob, resulting in O(2^n) time complexity. We then optimized using dynamic programming, either with memoization (top-down) or tabulation (bottom-up), reducing the time complexity to O(n). We further optimized the space complexity to O(1) by using only two variables to keep track of the previous two states. The key insight is to recognize that the maximum amount of money that can be robbed up to house i is the maximum of the maximum amount of money that can be robbed up to house i-1 (skip house i) and the maximum amount of money that can be robbed up to house i-2 plus the money in house i (rob house i). 