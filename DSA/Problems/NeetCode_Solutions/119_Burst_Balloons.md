# 📝 Burst Balloons

## **Problem Statement**

* You are given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by an array `nums`.
* You are asked to burst all the balloons. If you burst the ith balloon, you will get nums[i-1] * nums[i] * nums[i+1] coins. If i-1 or i+1 goes out of bounds of the array, then treat it as if there is a balloon with a 1 painted on it.
* Return the maximum coins you can collect by bursting the balloons wisely.
* Example 1:
  * Input: nums = [3,1,5,8]
  * Output: 167
  * Explanation:
    * nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
    * coins = 3*1*5 + 3*5*8 + 1*3*8 + 1*8*1 = 15 + 120 + 24 + 8 = 167
* Example 2:
  * Input: nums = [1,5]
  * Output: 10
* Constraints:
  * n == nums.length
  * 1 <= n <= 500
  * 0 <= nums[i] <= 100

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the maximum coins we can collect by bursting all balloons
* The key challenge is that the value of coins we get for bursting a balloon depends on its neighbors, which change as balloons are burst
* One approach would be to try all possible orders of bursting the balloons and find the maximum coins
* For each remaining balloon, we can try bursting it and recursively solve the problem for the remaining balloons
* This suggests a recursive approach where we try all possible combinations

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes the current array of balloons
2. Base case: If there are no balloons left, return 0 (no more coins can be collected)
3. For each balloon in the array:
   1. Calculate the coins we get for bursting this balloon
   2. Remove the balloon from the array
   3. Recursively call the function with the updated array
   4. Add the coins to the result of the recursive call
   5. Put the balloon back (backtrack)
4. Return the maximum coins among all possibilities

---

## **Step 3: Brute Force Implementation (Code)**

```python
def maxCoins(nums):
    def burst(balloons):
        if not balloons:
            return 0
        
        max_coins = 0
        for i in range(len(balloons)):
            # Calculate coins for bursting the current balloon
            left = 1 if i == 0 else balloons[i-1]
            right = 1 if i == len(balloons) - 1 else balloons[i+1]
            coins = left * balloons[i] * right
            
            # Remove the balloon and recursively solve the problem
            remaining = balloons[:i] + balloons[i+1:]
            max_coins = max(max_coins, coins + burst(remaining))
        
        return max_coins
    
    return burst(nums)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n!) where n is the number of balloons. For each recursive call, we try all remaining balloons, leading to a factorial number of function calls.
* Space Complexity: O(n^2) for the recursion stack and the copies of the array.
* This approach is extremely inefficient for a large number of balloons due to the factorial time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For nums = [3, 1, 5]:
  * burst([3, 1, 5]):
    * Try bursting balloon 0 (value 3):
      * coins = 1 * 3 * 1 = 3
      * burst([1, 5]):
        * Try bursting balloon 0 (value 1):
          * coins = 1 * 1 * 5 = 5
          * burst([5]):
            * coins = 1 * 5 * 1 = 5
            * burst([]) = 0
            * return 5
          * return 5 + 5 = 10
        * Try bursting balloon 1 (value 5):
          * coins = 1 * 5 * 1 = 5
          * burst([1]):
            * coins = 1 * 1 * 1 = 1
            * burst([]) = 0
            * return 1
          * return 5 + 1 = 6
        * return max(10, 6) = 10
      * return 3 + 10 = 13
    * ... (similar recursive calls for other balloons)
* There's redundant computation in the recursive calls, especially for overlapping subproblems

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming with a different approach.
* The key insight is to think about the problem in reverse: instead of considering which balloon to burst first, we consider which balloon to burst last.
* By considering the last balloon to burst, we can divide the problem into independent subproblems, which is suitable for dynamic programming.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently calculate the maximum coins for each subproblem.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* Instead of thinking about which balloon to burst first, we'll think about which balloon to burst last
* If we know the last balloon to burst is at index k, then the problem is divided into two independent subproblems:
  * The maximum coins from bursting all balloons in the range [left, k-1]
  * The maximum coins from bursting all balloons in the range [k+1, right]
* After solving these two subproblems, we add the coins from bursting the last balloon at index k
* Let dp[left][right] represent the maximum coins from bursting all balloons in the range [left, right]
* Base case: dp[i][i] = nums[i-1] * nums[i] * nums[i+1] (bursting a single balloon)
* For each range [left, right]:
  * For each k in the range [left, right]:
    * dp[left][right] = max(dp[left][right], dp[left][k-1] + dp[k+1][right] + nums[left-1] * nums[k] * nums[right+1])
* To handle the boundary cases, we can add 1s at the beginning and end of the array

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Add 1s at the beginning and end of the nums array: [1] + nums + [1]
2. Create a 2D array dp of size (n+2) x (n+2), initialized with 0
3. For each length from 1 to n:
   1. For each left from 1 to n-length+1:
      1. Calculate right = left + length - 1
      2. For each k from left to right:
         1. dp[left][right] = max(dp[left][right], dp[left][k-1] + dp[k+1][right] + nums[left-1] * nums[k] * nums[right+1])
4. Return dp[1][n]

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def maxCoins(nums):
    # Add 1s at the beginning and end
    nums = [1] + nums + [1]
    n = len(nums) - 2  # Number of original balloons
    
    # Create a 2D array filled with 0
    dp = [[0] * (n + 2) for _ in range(n + 2)]
    
    # Fill the dp table
    for length in range(1, n + 1):
        for left in range(1, n - length + 2):
            right = left + length - 1
            for k in range(left, right + 1):
                dp[left][right] = max(dp[left][right],
                                     dp[left][k-1] + dp[k+1][right] + nums[left-1] * nums[k] * nums[right+1])
    
    return dp[1][n]
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(n^3) where n is the number of balloons. We have O(n^2) subproblems, and for each subproblem, we consider O(n) possible values of k.
* Space Complexity: O(n^2) for the dp array.
* This approach is much more efficient than the brute force approach for a large number of balloons.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For nums = [3, 1, 5]:
  * Add 1s: nums = [1, 3, 1, 5, 1]
  * Initialize dp:
    ```
      | 0 | 1 | 2 | 3 | 4
    --|---|---|---|---|---
    0 | 0 | 0 | 0 | 0 | 0
    1 | 0 | 0 | 0 | 0 | 0
    2 | 0 | 0 | 0 | 0 | 0
    3 | 0 | 0 | 0 | 0 | 0
    4 | 0 | 0 | 0 | 0 | 0
    ```
  * length = 1:
    * left = 1, right = 1:
      * k = 1: dp[1][1] = dp[1][0] + dp[2][1] + nums[0] * nums[1] * nums[2] = 0 + 0 + 1 * 3 * 1 = 3
    * left = 2, right = 2:
      * k = 2: dp[2][2] = dp[2][1] + dp[3][2] + nums[1] * nums[2] * nums[3] = 0 + 0 + 3 * 1 * 5 = 15
    * left = 3, right = 3:
      * k = 3: dp[3][3] = dp[3][2] + dp[4][3] + nums[2] * nums[3] * nums[4] = 0 + 0 + 1 * 5 * 1 = 5
    ```
      | 0 | 1 | 2 | 3 | 4
    --|---|---|---|---|---
    0 | 0 | 0 | 0 | 0 | 0
    1 | 0 | 3 | 0 | 0 | 0
    2 | 0 | 0 | 15| 0 | 0
    3 | 0 | 0 | 0 | 5 | 0
    4 | 0 | 0 | 0 | 0 | 0
    ```
  * length = 2:
    * left = 1, right = 2:
      * k = 1: dp[1][2] = dp[1][0] + dp[2][2] + nums[0] * nums[1] * nums[3] = 0 + 15 + 1 * 3 * 5 = 30
      * k = 2: dp[1][2] = max(30, dp[1][1] + dp[3][2] + nums[0] * nums[2] * nums[3]) = max(30, 3 + 0 + 1 * 1 * 5) = max(30, 8) = 30
    * left = 2, right = 3:
      * k = 2: dp[2][3] = dp[2][1] + dp[3][3] + nums[1] * nums[2] * nums[4] = 0 + 5 + 3 * 1 * 1 = 8
      * k = 3: dp[2][3] = max(8, dp[2][2] + dp[4][3] + nums[1] * nums[3] * nums[4]) = max(8, 15 + 0 + 3 * 5 * 1) = max(8, 30) = 30
    ```
      | 0 | 1 | 2 | 3 | 4
    --|---|---|---|---|---
    0 | 0 | 0 | 0 | 0 | 0
    1 | 0 | 3 | 30| 0 | 0
    2 | 0 | 0 | 15| 30| 0
    3 | 0 | 0 | 0 | 5 | 0
    4 | 0 | 0 | 0 | 0 | 0
    ```
  * length = 3:
    * left = 1, right = 3:
      * k = 1: dp[1][3] = dp[1][0] + dp[2][3] + nums[0] * nums[1] * nums[4] = 0 + 30 + 1 * 3 * 1 = 33
      * k = 2: dp[1][3] = max(33, dp[1][1] + dp[3][3] + nums[0] * nums[2] * nums[4]) = max(33, 3 + 5 + 1 * 1 * 1) = max(33, 9) = 33
      * k = 3: dp[1][3] = max(33, dp[1][2] + dp[4][3] + nums[0] * nums[3] * nums[4]) = max(33, 30 + 0 + 1 * 5 * 1) = max(33, 35) = 35
    ```
      | 0 | 1 | 2 | 3 | 4
    --|---|---|---|---|---
    0 | 0 | 0 | 0 | 0 | 0
    1 | 0 | 3 | 30| 35| 0
    2 | 0 | 0 | 15| 30| 0
    3 | 0 | 0 | 0 | 5 | 0
    4 | 0 | 0 | 0 | 0 | 0
    ```
  * dp[1][3] = 35, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n!) | O(n^2) | Extremely inefficient for large n |
| Dynamic Programming | O(n^3) | O(n^2) | Efficient for medium-sized n |

---

## **Final Takeaways**

* The key to efficiently solving the Burst Balloons problem is to think about the problem in reverse: which balloon to burst last instead of first.
* This approach allows us to divide the problem into independent subproblems, which is suitable for dynamic programming.
* The problem demonstrates the importance of choosing the right perspective when formulating a dynamic programming solution.
* The O(n^3) time complexity is a significant improvement over the O(n!) brute force approach, making it feasible to solve for medium-sized inputs.
* This problem is a classic example of the "divide and conquer" strategy in dynamic programming, where we divide the problem into independent subproblems.

---

## **Real-life Analogy**

* Imagine you're planning a demolition sequence for a row of buildings, where the value of demolishing a building depends on which buildings are still standing next to it. The brute force approach would be to try all possible demolition sequences, which is inefficient. The dynamic programming approach would be to think about which building to demolish last, divide the problem into independent sections, and build the solution incrementally, which is much more efficient.

---

## **Summary**

* We solved the Burst Balloons problem by first considering a brute force recursive approach that tries all possible orders of bursting balloons, resulting in O(n!) time complexity. We then optimized using dynamic programming with a different perspective: considering which balloon to burst last instead of first. This allowed us to divide the problem into independent subproblems and reduce the time complexity to O(n^3). The key insight is to recognize that by thinking about the last balloon to burst, we can formulate a dynamic programming solution that avoids the combinatorial explosion of the brute force approach. 