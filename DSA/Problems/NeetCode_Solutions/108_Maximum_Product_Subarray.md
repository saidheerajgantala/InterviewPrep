# 📝 Maximum Product Subarray

## **Problem Statement**

* Given an integer array `nums`, find a contiguous non-empty subarray within the array that has the largest product, and return the product.
* The test cases are generated so that the answer will fit in a 32-bit integer.
* A subarray is a contiguous subsequence of the array.
* Example 1:
  * Input: nums = [2,3,-2,4]
  * Output: 6
  * Explanation: [2,3] has the largest product 6.
* Example 2:
  * Input: nums = [-2,0,-1]
  * Output: 0
  * Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
* Constraints:
  * 1 <= nums.length <= 2 * 10^4
  * -10 <= nums[i] <= 10
  * The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the contiguous subarray with the largest product
* A straightforward approach would be to generate all possible subarrays and calculate their products
* Then return the maximum product found
* This approach is simple but might be inefficient for large arrays

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a variable max_product to store the maximum product found so far
2. Generate all possible subarrays:
   1. For each starting index i from 0 to n-1:
      1. Initialize a variable product to 1
      2. For each ending index j from i to n-1:
         1. Multiply product by nums[j]
         2. Update max_product if product is larger
3. Return max_product

---

## **Step 3: Brute Force Implementation (Code)**

```python
def maxProduct(nums):
    if not nums:
        return 0
    
    n = len(nums)
    max_product = float('-inf')
    
    for i in range(n):
        product = 1
        for j in range(i, n):
            product *= nums[j]
            max_product = max(max_product, product)
    
    return max_product
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n^2) where n is the length of the array. We generate O(n^2) subarrays, and for each subarray, we calculate the product in O(1) time (since we're incrementally updating the product).
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient for large arrays due to the quadratic time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For nums = [2, 3, -2, 4]:
  * Subarrays starting at index 0:
    * [2]: Product = 2
    * [2, 3]: Product = 2 * 3 = 6
    * [2, 3, -2]: Product = 6 * (-2) = -12
    * [2, 3, -2, 4]: Product = -12 * 4 = -48
  * Subarrays starting at index 1:
    * [3]: Product = 3
    * [3, -2]: Product = 3 * (-2) = -6
    * [3, -2, 4]: Product = -6 * 4 = -24
  * Subarrays starting at index 2:
    * [-2]: Product = -2
    * [-2, 4]: Product = -2 * 4 = -8
  * Subarrays starting at index 3:
    * [4]: Product = 4
* The maximum product is 6 from the subarray [2, 3]
* There's redundant computation in calculating the products, especially for overlapping subarrays

---

## **Why are we using this algorithm?**

* For optimization, we'll use a dynamic programming approach that keeps track of both the maximum and minimum product ending at each position.
* The key insight is that a negative number can change the sign of the product, making the minimum product so far potentially become the maximum product if we encounter another negative number.
* By keeping track of both the maximum and minimum products ending at each position, we can handle negative numbers efficiently.
* Alternatives like the brute force approach would be inefficient due to redundant product calculations.
* Precondition: We need to be able to efficiently calculate the maximum product of a subarray ending at each position.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let's define two variables:
  * max_so_far: the maximum product subarray ending at the current position
  * min_so_far: the minimum product subarray ending at the current position
* For each element in the array:
  * If the element is positive:
    * max_so_far = max(nums[i], max_so_far * nums[i])
    * min_so_far = min(nums[i], min_so_far * nums[i])
  * If the element is negative:
    * A negative number will make the maximum product minimum and the minimum product maximum
    * We need to swap max_so_far and min_so_far before updating them
  * Update the global maximum product if max_so_far is larger
* This approach handles negative numbers efficiently by keeping track of both the maximum and minimum products

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Initialize variables max_product, max_so_far, and min_so_far
2. For each element nums[i] in the array:
   1. If nums[i] is negative, swap max_so_far and min_so_far
   2. Update max_so_far = max(nums[i], max_so_far * nums[i])
   3. Update min_so_far = min(nums[i], min_so_far * nums[i])
   4. Update max_product = max(max_product, max_so_far)
3. Return max_product

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def maxProduct(nums):
    if not nums:
        return 0
    
    max_product = nums[0]
    max_so_far = nums[0]
    min_so_far = nums[0]
    
    for i in range(1, len(nums)):
        # If the current element is negative, swap max_so_far and min_so_far
        if nums[i] < 0:
            max_so_far, min_so_far = min_so_far, max_so_far
        
        # Update max_so_far and min_so_far
        max_so_far = max(nums[i], max_so_far * nums[i])
        min_so_far = min(nums[i], min_so_far * nums[i])
        
        # Update the global maximum product
        max_product = max(max_product, max_so_far)
    
    return max_product
```

### **Alternative Implementation (Cleaner)**

A cleaner implementation that avoids the swap operation:

```python
def maxProduct(nums):
    if not nums:
        return 0
    
    max_product = nums[0]
    max_so_far = nums[0]
    min_so_far = nums[0]
    
    for i in range(1, len(nums)):
        # Calculate new max_so_far and min_so_far
        temp_max = max(nums[i], max(max_so_far * nums[i], min_so_far * nums[i]))
        min_so_far = min(nums[i], min(max_so_far * nums[i], min_so_far * nums[i]))
        max_so_far = temp_max
        
        # Update the global maximum product
        max_product = max(max_product, max_so_far)
    
    return max_product
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(n) where n is the length of the array. We process each element once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is much more efficient than the brute force approach for large arrays.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For nums = [2, 3, -2, 4]:
  * Initialize max_product = max_so_far = min_so_far = 2
  * i = 1 (nums[1] = 3):
    * nums[1] is positive, no swap needed
    * max_so_far = max(3, 2 * 3) = max(3, 6) = 6
    * min_so_far = min(3, 2 * 3) = min(3, 6) = 3
    * max_product = max(2, 6) = 6
  * i = 2 (nums[2] = -2):
    * nums[2] is negative, swap max_so_far and min_so_far: max_so_far = 3, min_so_far = 6
    * max_so_far = max(-2, 3 * (-2)) = max(-2, -6) = -2
    * min_so_far = min(-2, 6 * (-2)) = min(-2, -12) = -12
    * max_product = max(6, -2) = 6
  * i = 3 (nums[3] = 4):
    * nums[3] is positive, no swap needed
    * max_so_far = max(4, -2 * 4) = max(4, -8) = 4
    * min_so_far = min(4, -12 * 4) = min(4, -48) = -48
    * max_product = max(6, 4) = 6
* The maximum product is 6 from the subarray [2, 3]
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n^2) | O(1) | Inefficient for large arrays |
| Dynamic Programming | O(n) | O(1) | Efficient for large arrays |

---

## **Final Takeaways**

* The key to efficiently solving the Maximum Product Subarray problem is to handle negative numbers properly.
* By keeping track of both the maximum and minimum products ending at each position, we can efficiently handle negative numbers that might flip the sign of the product.
* Special attention should be paid to the case where the current element is negative, as it can make the minimum product so far become the maximum product.
* This problem demonstrates the importance of considering both maximum and minimum values in dynamic programming problems where the sign of the values can change.
* The approach is similar to Kadane's algorithm for the Maximum Subarray problem, but with the additional complexity of handling negative numbers.

---

## **Real-life Analogy**

* Imagine you're a stock trader tracking the daily percentage change of a stock. If the stock increases by 10% one day and decreases by 5% the next day, the overall change is not simply 10% - 5% = 5%, but rather (1 + 0.1) * (1 - 0.05) - 1 = 0.045 or 4.5%. The product of the daily factors (1 + percentage change) gives the overall change. The Maximum Product Subarray problem is like finding the period with the highest overall percentage increase.

---

## **Summary**

* We solved the Maximum Product Subarray problem by first considering a brute force approach that generates all possible subarrays and calculates their products, resulting in O(n^2) time complexity. We then optimized using a dynamic programming approach that keeps track of both the maximum and minimum products ending at each position, reducing the time complexity to O(n). The key insight is to handle negative numbers properly by considering both the maximum and minimum products, as a negative number can flip the sign and make the minimum product become the maximum product. 