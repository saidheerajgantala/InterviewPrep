# 📝 Longest Increasing Subsequence

## **Problem Statement**

* Given an integer array `nums`, return the length of the longest strictly increasing subsequence.
* A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements.
* Example 1:
  * Input: nums = [10,9,2,5,3,7,101,18]
  * Output: 4
  * Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
* Example 2:
  * Input: nums = [0,1,0,3,2,3]
  * Output: 4
  * Explanation: The longest increasing subsequence is [0,1,2,3], therefore the length is 4.
* Example 3:
  * Input: nums = [7,7,7,7,7,7,7]
  * Output: 1
  * Explanation: The longest increasing subsequence is [7], therefore the length is 1.
* Constraints:
  * 1 <= nums.length <= 2500
  * -10^4 <= nums[i] <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the length of the longest strictly increasing subsequence
* A subsequence doesn't need to be contiguous, but the order of elements must be preserved
* One approach would be to generate all possible subsequences and find the longest one that is strictly increasing
* This would involve trying all combinations of elements, which is a lot of combinations
* For each element, we have two choices: include it in the subsequence or exclude it
* This suggests a recursive approach

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes the current index and the previous element's value
2. Base case: If we've reached the end of the array, return 0 (no more elements to include)
3. For the current element, we have two choices:
   1. Skip the element: Recursively call the function with the next index and the same previous value
   2. Include the element (if it's greater than the previous element): Recursively call the function with the next index and the current element as the new previous value, and add 1 to the result
4. Return the maximum of the two choices

---

## **Step 3: Brute Force Implementation (Code)**

```python
def lengthOfLIS(nums):
    def find_lis(index, prev_val):
        # Base case: reached the end of the array
        if index == len(nums):
            return 0
        
        # Skip the current element
        skip = find_lis(index + 1, prev_val)
        
        # Include the current element if it's greater than the previous one
        include = 0
        if nums[index] > prev_val:
            include = 1 + find_lis(index + 1, nums[index])
        
        # Return the maximum of the two choices
        return max(skip, include)
    
    # Start with index 0 and a very small previous value to include any first element
    return find_lis(0, float('-inf'))
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n) where n is the length of the array. In the worst case, we make two recursive calls at each step, leading to an exponential number of function calls.
* Space Complexity: O(n) for the recursion stack.
* This approach is inefficient for large arrays due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For nums = [10, 9, 2, 5, 3, 7, 101, 18]:
  * find_lis(0, -inf):
    * Skip: find_lis(1, -inf)
      * ... (many recursive calls)
    * Include: 1 + find_lis(1, 10)
      * Skip: find_lis(2, 10)
        * ... (many recursive calls)
      * Include: Not possible (9 < 10)
  * ... (many more recursive calls)
* There's significant redundant computation in the recursive calls, especially for overlapping subproblems like find_lis(2, 10)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the length of the longest increasing subsequence ending at each position depends only on the lengths of the longest increasing subsequences ending at previous positions.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently determine the longest increasing subsequence ending at each position.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let dp[i] represent the length of the longest increasing subsequence ending at index i
* Base case: dp[i] = 1 for all i (each element by itself is an increasing subsequence of length 1)
* For each position i from 1 to n-1:
  * For each position j from 0 to i-1:
    * If nums[i] > nums[j], we can extend the subsequence ending at j with the element at i
    * Update dp[i] = max(dp[i], dp[j] + 1)
* The answer will be the maximum value in the dp array

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Create an array dp of size n, initialized with 1 (each element by itself is an increasing subsequence of length 1)
2. For each position i from 1 to n-1:
   1. For each position j from 0 to i-1:
      1. If nums[i] > nums[j], update dp[i] = max(dp[i], dp[j] + 1)
3. Return the maximum value in the dp array

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def lengthOfLIS(nums):
    if not nums:
        return 0
    
    n = len(nums)
    dp = [1] * n  # Each element by itself is an increasing subsequence of length 1
    
    for i in range(1, n):
        for j in range(i):
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    return max(dp)
```

### **Further Optimization - Binary Search**

We can optimize further using a binary search approach:

```python
def lengthOfLIS(nums):
    if not nums:
        return 0
    
    # tails[i] stores the smallest ending value of all increasing subsequences of length i+1
    tails = []
    
    for num in nums:
        # Binary search to find the position to insert the current element
        left, right = 0, len(tails)
        while left < right:
            mid = (left + right) // 2
            if tails[mid] < num:
                left = mid + 1
            else:
                right = mid
        
        # If we're at the end, append the element
        if left == len(tails):
            tails.append(num)
        # Otherwise, replace the element at the found position
        else:
            tails[left] = num
    
    return len(tails)
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(n^2) where n is the length of the array. For each position i, we check up to i previous positions.
* Space Complexity: O(n) for the dp array.
* This approach is much more efficient than the brute force approach for large arrays.

### **Complexity Analysis (Binary Search Optimization)**

* Time Complexity: O(n log n) where n is the length of the array. For each of the n elements, we perform a binary search which takes O(log n) time.
* Space Complexity: O(n) for the tails array.
* This approach is even more efficient than the standard dynamic programming approach for large arrays.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For nums = [10, 9, 2, 5, 3, 7, 101, 18]:
  * Initialize dp = [1, 1, 1, 1, 1, 1, 1, 1]
  * i = 1 (nums[1] = 9):
    * j = 0: nums[1] < nums[0], skip
    * dp[1] = 1
  * i = 2 (nums[2] = 2):
    * j = 0: nums[2] < nums[0], skip
    * j = 1: nums[2] < nums[1], skip
    * dp[2] = 1
  * i = 3 (nums[3] = 5):
    * j = 0: nums[3] < nums[0], skip
    * j = 1: nums[3] < nums[1], skip
    * j = 2: nums[3] > nums[2], dp[3] = max(1, 1 + 1) = 2
    * dp[3] = 2
  * i = 4 (nums[4] = 3):
    * j = 0: nums[4] < nums[0], skip
    * j = 1: nums[4] < nums[1], skip
    * j = 2: nums[4] > nums[2], dp[4] = max(1, 1 + 1) = 2
    * j = 3: nums[4] < nums[3], skip
    * dp[4] = 2
  * i = 5 (nums[5] = 7):
    * j = 0: nums[5] < nums[0], skip
    * j = 1: nums[5] < nums[1], skip
    * j = 2: nums[5] > nums[2], dp[5] = max(1, 1 + 1) = 2
    * j = 3: nums[5] > nums[3], dp[5] = max(2, 2 + 1) = 3
    * j = 4: nums[5] > nums[4], dp[5] = max(3, 2 + 1) = 3
    * dp[5] = 3
  * i = 6 (nums[6] = 101):
    * j = 0: nums[6] > nums[0], dp[6] = max(1, 1 + 1) = 2
    * j = 1: nums[6] > nums[1], dp[6] = max(2, 1 + 1) = 2
    * j = 2: nums[6] > nums[2], dp[6] = max(2, 1 + 1) = 2
    * j = 3: nums[6] > nums[3], dp[6] = max(2, 2 + 1) = 3
    * j = 4: nums[6] > nums[4], dp[6] = max(3, 2 + 1) = 3
    * j = 5: nums[6] > nums[5], dp[6] = max(3, 3 + 1) = 4
    * dp[6] = 4
  * i = 7 (nums[7] = 18):
    * j = 0: nums[7] > nums[0], dp[7] = max(1, 1 + 1) = 2
    * j = 1: nums[7] > nums[1], dp[7] = max(2, 1 + 1) = 2
    * j = 2: nums[7] > nums[2], dp[7] = max(2, 1 + 1) = 2
    * j = 3: nums[7] > nums[3], dp[7] = max(2, 2 + 1) = 3
    * j = 4: nums[7] > nums[4], dp[7] = max(3, 2 + 1) = 3
    * j = 5: nums[7] > nums[5], dp[7] = max(3, 3 + 1) = 4
    * j = 6: nums[7] < nums[6], skip
    * dp[7] = 4
  * dp = [1, 1, 1, 2, 2, 3, 4, 4]
  * max(dp) = 4, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Visualizing Computation (Binary Search Optimization)**

* For nums = [10, 9, 2, 5, 3, 7, 101, 18]:
  * tails = []
  * num = 10: Insert at position 0, tails = [10]
  * num = 9: Replace at position 0, tails = [9]
  * num = 2: Replace at position 0, tails = [2]
  * num = 5: Insert at position 1, tails = [2, 5]
  * num = 3: Replace at position 1, tails = [2, 3]
  * num = 7: Insert at position 2, tails = [2, 3, 7]
  * num = 101: Insert at position 3, tails = [2, 3, 7, 101]
  * num = 18: Replace at position 3, tails = [2, 3, 7, 18]
  * len(tails) = 4, which is the answer
* The binary search approach efficiently computes the result with fewer operations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(2^n) | O(n) | Inefficient for large arrays |
| Dynamic Programming | O(n^2) | O(n) | Efficient for medium-sized arrays |
| Binary Search | O(n log n) | O(n) | Efficient for large arrays |

---

## **Final Takeaways**

* The key to efficiently solving the Longest Increasing Subsequence problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The standard dynamic programming approach has O(n^2) time complexity, which is sufficient for most inputs.
* The binary search optimization reduces the time complexity to O(n log n), making it more efficient for large arrays.
* It's important to note that the binary search approach doesn't actually construct the longest increasing subsequence, but it correctly calculates its length.
* This problem demonstrates how different optimization techniques can be applied to the same problem to improve efficiency.

---

## **Real-life Analogy**

* Imagine you're analyzing a stock's price over time and want to find the longest sequence of days where the price strictly increases (not necessarily on consecutive days). The brute force approach would be to check all possible combinations of days, which is inefficient. The dynamic programming approach would be to keep track of the longest increasing sequence ending at each day, which is much more efficient.

---

## **Summary**

* We solved the Longest Increasing Subsequence problem by first considering a brute force recursive approach that tries all possible subsequences, resulting in O(2^n) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(n^2). Finally, we explored a binary search optimization that further reduces the time complexity to O(n log n). The key insight is to recognize that the length of the longest increasing subsequence ending at each position depends only on the lengths of the longest increasing subsequences ending at previous positions, which allows us to build the solution incrementally. 