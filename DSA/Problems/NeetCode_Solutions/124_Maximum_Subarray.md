# 📝 Maximum Subarray

## **Problem Statement**

* Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
* Example:
  * Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
  * Output: 6 (subarray [4,-1,2,1] has the largest sum = 6)
* Constraints:
  * 1 <= nums.length <= 10^5
  * -10^4 <= nums[i] <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* The most straightforward approach is to consider all possible contiguous subarrays
* For each possible start index, we'll try all possible end indices
* For each subarray, we'll calculate the sum and keep track of the maximum sum found
* This gives us all possible contiguous subarrays and their sums

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize maxSum to the smallest possible value
2. For each start index i from 0 to n-1:
   1. Initialize currentSum to 0
   2. For each end index j from i to n-1:
      1. Add nums[j] to currentSum
      2. Update maxSum if currentSum is greater
3. Return maxSum

---

## **Step 3: Brute Force Implementation (Code)**

```python
def maxSubArray(nums):
    n = len(nums)
    max_sum = float('-inf')
    
    for i in range(n):
        current_sum = 0
        for j in range(i, n):
            current_sum += nums[j]
            max_sum = max(max_sum, current_sum)
    
    return max_sum
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the length of the array. We have two nested loops.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This is inefficient for large arrays due to the quadratic time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For array [-2,1,-3,4,-1,2,1,-5,4]:
  * We calculate sum of [-2], then [-2,1], then [-2,1,-3], etc.
  * Then we calculate sum of [1], then [1,-3], then [1,-3,4], etc.
  * There's a lot of redundant addition happening.

---

## **Why are we using this algorithm?**

* For optimization, we'll use Kadane's Algorithm.
* Kadane's Algorithm is specifically designed for the maximum subarray problem.
* It uses dynamic programming to efficiently find the maximum sum subarray in linear time.
* The key insight is that if a subarray's sum becomes negative, it's better to start fresh.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of calculating all possible subarrays, we can use Kadane's Algorithm
* The key insight: at each position, we have two choices:
  1. Continue the current subarray (add the current element to the running sum)
  2. Start a new subarray from the current element (if the running sum becomes negative)
* We always choose the option that gives us the maximum sum

### **Step 2: Flow Steps (Optimized)**

1. Initialize maxSum and currentSum to the first element of the array
2. For each element from the second element to the end:
   1. currentSum = max(nums[i], currentSum + nums[i])
   2. maxSum = max(maxSum, currentSum)
3. Return maxSum

### **Step 3: Implementation (Optimized)**

```python
def maxSubArray(nums):
    if not nums:
        return 0
        
    max_sum = current_sum = nums[0]
    
    for i in range(1, len(nums)):
        # Either extend the previous subarray or start a new one
        current_sum = max(nums[i], current_sum + nums[i])
        # Update the maximum sum if the current sum is greater
        max_sum = max(max_sum, current_sum)
    
    return max_sum
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. We only need to iterate through the array once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* Much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For array [-2,1,-3,4,-1,2,1,-5,4]:
  * Start: max_sum = current_sum = -2
  * Process 1: current_sum = max(1, -2+1) = 1, max_sum = 1
  * Process -3: current_sum = max(-3, 1+(-3)) = -2, max_sum = 1
  * Process 4: current_sum = max(4, -2+4) = 4, max_sum = 4
  * Process -1: current_sum = max(-1, 4+(-1)) = 3, max_sum = 4
  * Process 2: current_sum = max(2, 3+2) = 5, max_sum = 5
  * Process 1: current_sum = max(1, 5+1) = 6, max_sum = 6
  * Process -5: current_sum = max(-5, 6+(-5)) = 1, max_sum = 6
  * Process 4: current_sum = max(4, 1+4) = 5, max_sum = 6
  * Final max_sum = 6

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Calculates sum for all possible subarrays |
| Optimized (Algorithm: Kadane's) | O(n) | O(1) | Makes optimal local choices at each step |

---

## **Final Takeaways**

* Kadane's Algorithm is a classic example of dynamic programming where we build the solution incrementally.
* The key insight is recognizing when to "reset" our current sum if it becomes negative.
* This problem demonstrates how a greedy approach (making locally optimal choices) can lead to a globally optimal solution.

---

## **Real-life Analogy**

* Imagine you're hiking on a trail with ups and downs in elevation. Your goal is to find the section of the trail with the highest net elevation gain. Kadane's algorithm is like continuously evaluating as you walk: "Should I count the elevation changes from where I started, or should I forget the past and start measuring from this point forward?" You always choose the option that gives you the highest net gain.

---

## **Summary**

* We tackled the "Maximum Subarray" problem first with a brute force approach that examines all possible subarrays, resulting in O(n²) time complexity. We then optimized using Kadane's Algorithm, which makes optimal local decisions at each step to find the global maximum in O(n) time. This demonstrates the power of dynamic programming in solving optimization problems efficiently. 