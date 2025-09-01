# 📝 Partition Equal Subset Sum

## **Problem Statement**

* Given an integer array `nums`, return `true` if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or `false` otherwise.
* Example 1:
  * Input: nums = [1,5,11,5]
  * Output: true
  * Explanation: The array can be partitioned as [1, 5, 5] and [11].
* Example 2:
  * Input: nums = [1,2,3,5]
  * Output: false
  * Explanation: The array cannot be partitioned into equal sum subsets.
* Constraints:
  * 1 <= nums.length <= 200
  * 1 <= nums[i] <= 100

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine if the array can be partitioned into two subsets with equal sum
* First, we need to check if the total sum of the array is even. If it's odd, it's impossible to partition the array into two equal subsets
* If the total sum is even, we need to find a subset with sum equal to half of the total sum
* This is essentially a subset sum problem: can we find a subset of the array that sums to a specific target (half of the total sum)?
* One approach would be to try all possible subsets of the array and check if any of them sum to the target

---

## **Step 2: Flow Steps (Brute Force)**

1. Calculate the total sum of the array
2. If the total sum is odd, return false (impossible to partition into equal subsets)
3. Calculate the target sum: target = total_sum / 2
4. Create a recursive function that tries to find a subset with sum equal to the target:
   1. Base case: If the target is 0, return true (we've found a subset with the desired sum)
   2. Base case: If we've reached the end of the array or the target is negative, return false
   3. For the current element, we have two choices:
      1. Include it: Recursively call the function with the target reduced by the current element
      2. Exclude it: Recursively call the function with the same target but moving to the next element
   4. Return true if either choice leads to a valid solution
5. Return the result of the recursive function

---

## **Step 3: Brute Force Implementation (Code)**

```python
def canPartition(nums):
    total_sum = sum(nums)
    
    # If the total sum is odd, we cannot partition the array into equal subsets
    if total_sum % 2 != 0:
        return False
    
    target = total_sum // 2
    
    def can_find_subset(index, target):
        # Base cases
        if target == 0:
            return True
        if index == len(nums) or target < 0:
            return False
        
        # Include the current element
        if can_find_subset(index + 1, target - nums[index]):
            return True
        
        # Exclude the current element
        if can_find_subset(index + 1, target):
            return True
        
        return False
    
    return can_find_subset(0, target)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n) where n is the length of the array. In the worst case, we explore all possible subsets of the array.
* Space Complexity: O(n) for the recursion stack.
* This approach is inefficient for large arrays due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For nums = [1, 5, 11, 5]:
  * total_sum = 22, target = 11
  * can_find_subset(0, 11):
    * Include nums[0] = 1: can_find_subset(1, 10)
      * Include nums[1] = 5: can_find_subset(2, 5)
        * Include nums[2] = 11: can_find_subset(3, -6) -> False
        * Exclude nums[2] = 11: can_find_subset(3, 5)
          * Include nums[3] = 5: can_find_subset(4, 0) -> True
          * Return True
        * Return True
      * Return True
    * Return True
  * Return True
* There's redundant computation in the recursive calls, especially for overlapping subproblems like can_find_subset(2, 5)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the ability to form a subset with a specific sum depends only on the ability to form subsets with smaller sums using fewer elements.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently determine if a subset of the array can sum to a specific target.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let dp[i][j] represent whether a subset of the first i elements can sum to j
* Base case: dp[0][0] = true (an empty subset can sum to 0)
* For each element nums[i-1] and each possible sum j from 0 to target:
  * If we can form sum j without using the current element: dp[i][j] = dp[i-1][j]
  * If we can form sum j using the current element: dp[i][j] = dp[i-1][j - nums[i-1]] (if j >= nums[i-1])
* The answer will be dp[n][target]

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Calculate the total sum of the array
2. If the total sum is odd, return false (impossible to partition into equal subsets)
3. Calculate the target sum: target = total_sum / 2
4. Create a 2D boolean array dp of size (n+1) x (target+1), where dp[i][j] represents whether a subset of the first i elements can sum to j
5. Initialize dp[0][0] = true (an empty subset can sum to 0)
6. For each element nums[i-1] from 1 to n:
   1. For each possible sum j from 0 to target:
      1. If we can form sum j without using the current element: dp[i][j] = dp[i-1][j]
      2. If j >= nums[i-1] and we can form sum j - nums[i-1] using the first i-1 elements: dp[i][j] = dp[i-1][j - nums[i-1]]
7. Return dp[n][target]

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def canPartition(nums):
    total_sum = sum(nums)
    
    # If the total sum is odd, we cannot partition the array into equal subsets
    if total_sum % 2 != 0:
        return False
    
    target = total_sum // 2
    n = len(nums)
    
    # dp[i][j] represents whether a subset of the first i elements can sum to j
    dp = [[False] * (target + 1) for _ in range(n + 1)]
    
    # Base case: an empty subset can sum to 0
    for i in range(n + 1):
        dp[i][0] = True
    
    for i in range(1, n + 1):
        for j in range(1, target + 1):
            # If we can form sum j without using the current element
            dp[i][j] = dp[i-1][j]
            
            # If we can form sum j using the current element
            if j >= nums[i-1]:
                dp[i][j] = dp[i][j] or dp[i-1][j - nums[i-1]]
    
    return dp[n][target]
```

### **Space Optimization**

We can optimize the space complexity to O(target) by using a 1D array:

```python
def canPartition(nums):
    total_sum = sum(nums)
    
    # If the total sum is odd, we cannot partition the array into equal subsets
    if total_sum % 2 != 0:
        return False
    
    target = total_sum // 2
    
    # dp[j] represents whether a subset can sum to j
    dp = [False] * (target + 1)
    dp[0] = True  # Base case: an empty subset can sum to 0
    
    for num in nums:
        # We need to iterate backwards to avoid using the same element multiple times
        for j in range(target, num - 1, -1):
            dp[j] = dp[j] or dp[j - num]
    
    return dp[target]
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(n * target) where n is the length of the array and target is half of the total sum. We fill a 2D table of size (n+1) x (target+1).
* Space Complexity: O(n * target) for the dp array.
* This approach is much more efficient than the brute force approach for large arrays.

### **Complexity Analysis (Space-Optimized Version)**

* Time Complexity: O(n * target) where n is the length of the array and target is half of the total sum.
* Space Complexity: O(target) for the 1D dp array.
* This approach is even more space-efficient while maintaining the same time complexity.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For nums = [1, 5, 11, 5]:
  * total_sum = 22, target = 11
  * Initialize dp[0][0] = true, all other dp[i][j] = false
  * i = 1 (nums[0] = 1):
    * j = 1: dp[1][1] = dp[0][1] or dp[0][0] = false or true = true
    * j = 2 to j = 11: dp[1][j] = dp[0][j] = false
  * i = 2 (nums[1] = 5):
    * j = 1: dp[2][1] = dp[1][1] = true
    * j = 2: dp[2][2] = dp[1][2] = false
    * j = 3: dp[2][3] = dp[1][3] = false
    * j = 4: dp[2][4] = dp[1][4] = false
    * j = 5: dp[2][5] = dp[1][5] or dp[1][0] = false or true = true
    * j = 6: dp[2][6] = dp[1][6] or dp[1][1] = false or true = true
    * j = 7 to j = 11: dp[2][j] = dp[1][j] = false
  * i = 3 (nums[2] = 11):
    * j = 1: dp[3][1] = dp[2][1] = true
    * j = 2: dp[3][2] = dp[2][2] = false
    * j = 3: dp[3][3] = dp[2][3] = false
    * j = 4: dp[3][4] = dp[2][4] = false
    * j = 5: dp[3][5] = dp[2][5] = true
    * j = 6: dp[3][6] = dp[2][6] = true
    * j = 7 to j = 10: dp[3][j] = dp[2][j] = false
    * j = 11: dp[3][11] = dp[2][11] or dp[2][0] = false or true = true
  * i = 4 (nums[3] = 5):
    * j = 1: dp[4][1] = dp[3][1] = true
    * j = 2: dp[4][2] = dp[3][2] = false
    * j = 3: dp[4][3] = dp[3][3] = false
    * j = 4: dp[4][4] = dp[3][4] = false
    * j = 5: dp[4][5] = dp[3][5] = true
    * j = 6: dp[4][6] = dp[3][6] = true
    * j = 7 to j = 10: dp[4][j] = dp[3][j] = false
    * j = 11: dp[4][11] = dp[3][11] = true
  * dp[4][11] = true, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(2^n) | O(n) | Inefficient for large arrays |
| Dynamic Programming (2D) | O(n * target) | O(n * target) | Efficient for medium-sized arrays |
| Dynamic Programming (1D) | O(n * target) | O(target) | Space-optimized version |

---

## **Final Takeaways**

* The key to efficiently solving the Partition Equal Subset Sum problem is to recognize it as a variation of the subset sum problem and use dynamic programming to avoid redundant computations.
* The problem can be solved using a bottom-up approach by building the solution from smaller subproblems.
* The space optimization from a 2D array to a 1D array is possible because we only need the results from the previous row to compute the current row.
* When using the 1D array, it's important to iterate backwards to avoid using the same element multiple times.
* This problem demonstrates how dynamic programming can transform an exponential-time algorithm into a polynomial-time one.

---

## **Real-life Analogy**

* Imagine you have a set of weights and you want to divide them into two groups with equal total weight. The brute force approach would be to try all possible ways to divide the weights, which is inefficient. The dynamic programming approach would be to check if you can form a subset with weight equal to half the total weight, which is much more efficient.

---

## **Summary**

* We solved the Partition Equal Subset Sum problem by first considering a brute force recursive approach that tries all possible subsets, resulting in O(2^n) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(n * target). Finally, we explored a space optimization that reduces the space complexity from O(n * target) to O(target). The key insight is to recognize that the ability to form a subset with a specific sum depends only on the ability to form subsets with smaller sums using fewer elements, which allows us to build the solution incrementally. 