# 📝 House Robber II

## **Problem Statement**

* You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.
* Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.
* Example 1:
  * Input: nums = [2,3,2]
  * Output: 3
  * Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
* Example 2:
  * Input: nums = [1,2,3,1]
  * Output: 4
  * Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3). Total amount you can rob = 1 + 3 = 4.
* Example 3:
  * Input: nums = [1,2,3]
  * Output: 3
* Constraints:
  * 1 <= nums.length <= 100
  * 0 <= nums[i] <= 1000

---

## **Step 1: Thinking Process (Brute Force)**

* This problem is similar to the original House Robber problem, but with a circular arrangement of houses
* The key difference is that if we rob the first house, we cannot rob the last house, and vice versa
* One approach would be to consider two scenarios:
  1. Rob houses 1 to n-1 (excluding the last house)
  2. Rob houses 2 to n (excluding the first house)
* Then take the maximum of these two scenarios
* For each scenario, we can use the same approach as the original House Robber problem

---

## **Step 2: Flow Steps (Brute Force)**

1. If there's only one house, return its value
2. If there are only two houses, return the maximum of their values
3. For houses 1 to n-1 (excluding the last house):
   1. Define a recursive function `rob1(i)` that returns the maximum amount of money that can be robbed from houses `i` to `n-1`
   2. Base cases: If `i >= n-1`, return 0
   3. Recursive case: Return `max(nums[i] + rob1(i + 2), rob1(i + 1))`
4. For houses 2 to n (excluding the first house):
   1. Define a recursive function `rob2(i)` that returns the maximum amount of money that can be robbed from houses `i` to `n`
   2. Base cases: If `i >= n`, return 0
   3. Recursive case: Return `max(nums[i] + rob2(i + 2), rob2(i + 1))`
5. Return the maximum of `rob1(0)` and `rob2(1)`

---

## **Step 3: Brute Force Implementation (Code)**

```python
def rob(nums):
    n = len(nums)
    
    if n == 1:
        return nums[0]
    if n == 2:
        return max(nums[0], nums[1])
    
    def rob_recursive(start, end):
        def helper(i):
            if i >= end:
                return 0
            return max(nums[i] + helper(i + 2), helper(i + 1))
        
        return helper(start)
    
    # Rob houses 1 to n-1 (excluding the last house)
    max1 = rob_recursive(0, n - 1)
    
    # Rob houses 2 to n (excluding the first house)
    max2 = rob_recursive(1, n)
    
    return max(max1, max2)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n) because for each house, we have two choices (rob it or skip it), and we explore all possible combinations.
* Space Complexity: O(n) for the recursion stack.
* This approach is inefficient for large values of n due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For nums = [1, 2, 3, 1]:
  * rob_recursive(0, 3):
    * helper(0) = max(1 + helper(2), helper(1))
      * helper(2) = max(3 + helper(4), helper(3))
        * helper(4) = 0
        * helper(3) = 0
      * helper(1) = max(2 + helper(3), helper(2))
        * helper(3) = 0
        * helper(2) = max(3 + helper(4), helper(3))
          * helper(4) = 0
          * helper(3) = 0
  * rob_recursive(1, 4):
    * helper(1) = max(2 + helper(3), helper(2))
      * helper(3) = max(1 + helper(5), helper(4))
        * helper(5) = 0
        * helper(4) = 0
      * helper(2) = max(3 + helper(4), helper(3))
        * helper(4) = 0
        * helper(3) = max(1 + helper(5), helper(4))
          * helper(5) = 0
          * helper(4) = 0
* Notice that we're calculating helper(2), helper(3), and helper(4) multiple times in both scenarios

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant calculations.
* The property that matches this problem is that we can break it down into two subproblems:
  1. The original House Robber problem for houses 1 to n-1
  2. The original House Robber problem for houses 2 to n
* By using memoization or tabulation for each subproblem, we can avoid recalculating the same values.
* Alternatives like the brute force approach would be inefficient due to redundant calculations.
* Precondition: We need to be able to store and reuse previously calculated results.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to solve each subproblem efficiently
* We'll use the tabulation approach from the original House Robber problem
* We'll define a helper function to solve the original House Robber problem for a given range of houses
* Then we'll apply this helper function to the two scenarios:
  1. Rob houses 1 to n-1 (excluding the last house)
  2. Rob houses 2 to n (excluding the first house)

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. If there's only one house, return its value
2. Define a helper function `rob_range(start, end)` that solves the original House Robber problem for houses from index `start` to `end-1`
3. In the helper function, use the tabulation approach:
   1. Initialize dp array
   2. Set base cases
   3. Fill the dp array using the recurrence relation
   4. Return the last element of the dp array
4. Return the maximum of `rob_range(0, n-1)` and `rob_range(1, n)`

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def rob(nums):
    n = len(nums)
    
    if n == 1:
        return nums[0]
    if n == 2:
        return max(nums[0], nums[1])
    
    def rob_range(start, end):
        dp = [0] * (end - start + 1)
        dp[0] = nums[start]
        dp[1] = max(nums[start], nums[start + 1])
        
        for i in range(2, end - start + 1):
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[start + i])
        
        return dp[-1]
    
    # Rob houses 1 to n-1 (excluding the last house)
    max1 = rob_range(0, n - 2)
    
    # Rob houses 2 to n (excluding the first house)
    max2 = rob_range(1, n - 1)
    
    return max(max1, max2)
```

### **Step 3: Implementation (Optimized - Space-Efficient)**

```python
def rob(nums):
    n = len(nums)
    
    if n == 1:
        return nums[0]
    if n == 2:
        return max(nums[0], nums[1])
    
    def rob_range(start, end):
        prev2 = nums[start]
        prev1 = max(nums[start], nums[start + 1])
        
        for i in range(start + 2, end + 1):
            curr = max(prev1, prev2 + nums[i])
            prev2, prev1 = prev1, curr
        
        return prev1
    
    # Rob houses 1 to n-1 (excluding the last house)
    max1 = rob_range(0, n - 2)
    
    # Rob houses 2 to n (excluding the first house)
    max2 = rob_range(1, n - 1)
    
    return max(max1, max2)
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(n) because we solve two instances of the original House Robber problem, each taking O(n) time.
* Space Complexity:
  * Tabulation: O(n) for the dp array.
  * Space-Efficient: O(1) because we only use a few variables.
* This approach is much more efficient than the brute force approach for large values of n.

### **Step 5: Visualizing Computation (Optimized - Tabulation)**

* For nums = [1, 2, 3, 1]:
  * rob_range(0, 2):
    * dp[0] = 1
    * dp[1] = max(1, 2) = 2
    * dp[2] = max(dp[1], dp[0] + nums[2]) = max(2, 1 + 3) = 4
    * Return dp[2] = 4
  * rob_range(1, 3):
    * dp[0] = 2
    * dp[1] = max(2, 3) = 3
    * dp[2] = max(dp[1], dp[0] + nums[3]) = max(3, 2 + 1) = 3
    * Return dp[2] = 3
  * Return max(4, 3) = 4
* The tabulation approach efficiently builds up the solution from the base cases for each scenario

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(2^n) | O(n) | Inefficient due to redundant calculations |
| Tabulation | O(n) | O(n) | Efficient with bottom-up approach |
| Space-Efficient | O(n) | O(1) | Most efficient in terms of space |

---

## **Final Takeaways**

* The key to solving the House Robber II problem is to recognize that it can be broken down into two instances of the original House Robber problem:
  1. Rob houses 1 to n-1 (excluding the last house)
  2. Rob houses 2 to n (excluding the first house)
* This is because the houses are arranged in a circle, so if we rob the first house, we cannot rob the last house, and vice versa.
* By using dynamic programming to solve each subproblem efficiently, we can find the maximum amount of money that can be robbed.
* This problem demonstrates the importance of problem decomposition and reusing existing solutions.

---

## **Real-life Analogy**

* Imagine you're planning a vacation and can only visit one city per day. The cities are arranged in a circle, and you can't visit adjacent cities on consecutive days. You want to maximize the number of attractions you can visit. You can break this down into two scenarios: either skip the first city or skip the last city, then find the maximum number of attractions you can visit in each scenario.

---

## **Summary**

* We solved the House Robber II problem by breaking it down into two instances of the original House Robber problem: one excluding the first house and one excluding the last house. We then used dynamic programming to solve each subproblem efficiently, reducing the time complexity from O(2^n) to O(n). The key insight is to recognize that the circular arrangement of houses introduces a constraint that can be handled by considering two separate scenarios. 