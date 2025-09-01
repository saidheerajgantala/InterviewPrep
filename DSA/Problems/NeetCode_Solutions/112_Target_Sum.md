# 📝 Target Sum

## **Problem Statement**

* You are given an integer array `nums` and an integer `target`.
* You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers.
* For example, if nums = [2, 1], you can add a '+' before 2 and a '-' before 1 and concatenate them to build the expression "+2-1".
* Return the number of different expressions that you can build, which evaluates to target.
* Example 1:
  * Input: nums = [1,1,1,1,1], target = 3
  * Output: 5
  * Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
    * -1 + 1 + 1 + 1 + 1 = 3
    * +1 - 1 + 1 + 1 + 1 = 3
    * +1 + 1 - 1 + 1 + 1 = 3
    * +1 + 1 + 1 - 1 + 1 = 3
    * +1 + 1 + 1 + 1 - 1 = 3
* Example 2:
  * Input: nums = [1], target = 1
  * Output: 1
* Constraints:
  * 1 <= nums.length <= 20
  * 0 <= nums[i] <= 1000
  * 0 <= sum(nums[i]) <= 1000
  * -1000 <= target <= 1000

---

## **Step 1: Thinking Process (Brute Force)**

* We need to count the number of ways to assign '+' or '-' to each number to reach the target sum
* For each number in the array, we have two choices: add it or subtract it
* This suggests a recursive approach where we try both options for each number and count the valid combinations
* We'll build the expression step by step, keeping track of the running sum

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes the current index and the current sum
2. Base case: If we've processed all numbers (reached the end of the array), check if the current sum equals the target
   1. If yes, return 1 (found a valid combination)
   2. If no, return 0 (not a valid combination)
3. Recursive case: Try both adding and subtracting the current number
   1. Add the current number: recursively call the function with the next index and current sum + nums[index]
   2. Subtract the current number: recursively call the function with the next index and current sum - nums[index]
   3. Return the sum of the results from both choices
4. Start the recursion with index 0 and sum 0

---

## **Step 3: Brute Force Implementation (Code)**

```python
def findTargetSumWays(nums, target):
    def backtrack(index, current_sum):
        # Base case: reached the end of the array
        if index == len(nums):
            return 1 if current_sum == target else 0
        
        # Try adding the current number
        add = backtrack(index + 1, current_sum + nums[index])
        
        # Try subtracting the current number
        subtract = backtrack(index + 1, current_sum - nums[index])
        
        # Return the total number of ways
        return add + subtract
    
    return backtrack(0, 0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n) where n is the length of the array. In the worst case, we explore all possible combinations of '+' and '-' for each number.
* Space Complexity: O(n) for the recursion stack.
* This approach is inefficient for large arrays due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For nums = [1, 1, 1, 1, 1], target = 3:
  * backtrack(0, 0):
    * Add: backtrack(1, 1)
      * Add: backtrack(2, 2)
        * Add: backtrack(3, 3)
          * Add: backtrack(4, 4)
            * Add: backtrack(5, 5) -> 0
            * Subtract: backtrack(5, 3) -> 1
          * Subtract: backtrack(4, 2)
            * Add: backtrack(5, 3) -> 1
            * Subtract: backtrack(5, 1) -> 0
        * Subtract: backtrack(3, 1)
          * Add: backtrack(4, 2)
            * Add: backtrack(5, 3) -> 1
            * Subtract: backtrack(5, 1) -> 0
          * Subtract: backtrack(4, 0)
            * Add: backtrack(5, 1) -> 0
            * Subtract: backtrack(5, -1) -> 0
      * Subtract: backtrack(2, 0)
        * ... (similar recursive calls)
    * Subtract: backtrack(1, -1)
      * ... (similar recursive calls)
* There's redundant computation in the recursive calls, especially for overlapping subproblems like backtrack(5, 3)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the number of ways to reach a specific sum using a subset of the array depends only on the number of ways to reach smaller sums using fewer elements.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently count the number of ways to reach a specific sum using a subset of the array.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming with memoization to avoid redundant computations
* Let's define a memoization table memo[i][j] to store the number of ways to reach sum j using the first i elements
* Since the sum can be negative, we need to handle negative indices in our memoization table
* Alternatively, we can transform the problem into a subset sum problem:
  * Let P be the subset of nums with positive signs and N be the subset with negative signs
  * Then P + N = sum(nums) and P - N = target
  * Solving these equations: P = (sum(nums) + target) / 2
  * So the problem becomes: find the number of subsets of nums that sum to (sum(nums) + target) / 2

### **Step 2: Flow Steps (Optimized - Memoization)**

1. Create a recursive function with memoization that takes the current index and the current sum
2. Use a dictionary to store computed results for each (index, sum) pair
3. Base case: If we've processed all numbers, check if the current sum equals the target
4. Recursive case: Try both adding and subtracting the current number, using memoization to avoid redundant computations
5. Start the recursion with index 0 and sum 0

### **Step 3: Implementation (Optimized - Memoization)**

```python
def findTargetSumWays(nums, target):
    # Dictionary to store computed results
    memo = {}
    
    def backtrack(index, current_sum):
        # Check if we've already computed this result
        if (index, current_sum) in memo:
            return memo[(index, current_sum)]
        
        # Base case: reached the end of the array
        if index == len(nums):
            return 1 if current_sum == target else 0
        
        # Try adding the current number
        add = backtrack(index + 1, current_sum + nums[index])
        
        # Try subtracting the current number
        subtract = backtrack(index + 1, current_sum - nums[index])
        
        # Store the result in the memoization table
        memo[(index, current_sum)] = add + subtract
        
        return memo[(index, current_sum)]
    
    return backtrack(0, 0)
```

### **Alternative Approach: Subset Sum Transformation**

```python
def findTargetSumWays(nums, target):
    total_sum = sum(nums)
    
    # If the target is out of range or (total_sum + target) is odd, return 0
    if abs(target) > total_sum or (total_sum + target) % 2 != 0:
        return 0
    
    # Calculate the sum we need to find
    subset_sum = (total_sum + target) // 2
    
    # Initialize dp array
    dp = [0] * (subset_sum + 1)
    dp[0] = 1  # There's one way to get sum 0 (by not selecting any element)
    
    for num in nums:
        for j in range(subset_sum, num - 1, -1):
            dp[j] += dp[j - num]
    
    return dp[subset_sum]
```

### **Step 4: Complexity Analysis (Optimized - Memoization)**

* Time Complexity: O(n * sum) where n is the length of the array and sum is the range of possible sums (which is at most 2 * sum(nums) + 1).
* Space Complexity: O(n * sum) for the memoization table.
* This approach is much more efficient than the brute force approach for large arrays.

### **Complexity Analysis (Subset Sum Transformation)**

* Time Complexity: O(n * subset_sum) where n is the length of the array and subset_sum is (sum(nums) + target) / 2.
* Space Complexity: O(subset_sum) for the dp array.
* This approach is even more space-efficient while maintaining a similar time complexity.

### **Step 5: Visualizing Computation (Optimized - Memoization)**

* For nums = [1, 1, 1, 1, 1], target = 3:
  * We compute backtrack(0, 0) and store the result in memo
  * When we encounter the same subproblem again, we retrieve the result from memo instead of recomputing
  * For example, backtrack(3, 1) is computed once and reused multiple times
  * This significantly reduces the number of recursive calls

### **Visualizing Computation (Subset Sum Transformation)**

* For nums = [1, 1, 1, 1, 1], target = 3:
  * total_sum = 5, subset_sum = (5 + 3) / 2 = 4
  * Initialize dp = [1, 0, 0, 0, 0]
  * Process num = 1:
    * dp[4] += dp[3] = 0
    * dp[3] += dp[2] = 0
    * dp[2] += dp[1] = 0
    * dp[1] += dp[0] = 1
    * dp = [1, 1, 0, 0, 0]
  * Process num = 1:
    * dp[4] += dp[3] = 0
    * dp[3] += dp[2] = 0
    * dp[2] += dp[1] = 1
    * dp[1] += dp[0] = 1
    * dp = [1, 2, 1, 0, 0]
  * Process num = 1:
    * dp[4] += dp[3] = 0
    * dp[3] += dp[2] = 1
    * dp[2] += dp[1] = 2
    * dp[1] += dp[0] = 1
    * dp = [1, 3, 3, 1, 0]
  * Process num = 1:
    * dp[4] += dp[3] = 1
    * dp[3] += dp[2] = 3
    * dp[2] += dp[1] = 3
    * dp[1] += dp[0] = 1
    * dp = [1, 4, 6, 4, 1]
  * Process num = 1:
    * dp[4] += dp[3] = 4
    * dp[3] += dp[2] = 6
    * dp[2] += dp[1] = 4
    * dp[1] += dp[0] = 1
    * dp = [1, 5, 10, 10, 5]
  * dp[4] = 5, which is the answer
* The subset sum transformation efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(2^n) | O(n) | Inefficient for large arrays |
| Dynamic Programming (Memoization) | O(n * sum) | O(n * sum) | Efficient for medium-sized arrays |
| Subset Sum Transformation | O(n * subset_sum) | O(subset_sum) | Space-optimized version |

---

## **Final Takeaways**

* The key to efficiently solving the Target Sum problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The memoization approach directly models the problem but requires handling negative sums in the memoization table.
* The subset sum transformation is an elegant way to convert the problem into a standard subset sum problem, which can be solved using dynamic programming.
* When using the subset sum approach, it's important to check if the target is within the range of possible sums and if (total_sum + target) is even.
* This problem demonstrates how transforming a problem can sometimes lead to a more efficient solution.

---

## **Real-life Analogy**

* Imagine you're a financial planner trying to determine how many different ways you can invest a set of money to reach a target return. Each investment can either yield a positive return ('+') or a negative return ('-'). The brute force approach would be to try all possible combinations of investments, which is inefficient. The dynamic programming approach would be to keep track of the number of ways to reach each possible return using a subset of the investments, which is much more efficient.

---

## **Summary**

* We solved the Target Sum problem by first considering a brute force recursive approach that tries all possible combinations of '+' and '-' for each number, resulting in O(2^n) time complexity. We then optimized using dynamic programming with memoization to avoid redundant computations, reducing the time complexity to O(n * sum). Finally, we explored a transformation that converts the problem into a subset sum problem, which can be solved using dynamic programming with O(n * subset_sum) time complexity and O(subset_sum) space complexity. The key insight is to recognize that the number of ways to reach a specific sum using a subset of the array depends only on the number of ways to reach smaller sums using fewer elements, which allows us to build the solution incrementally. 