# 📝 Product of Array Except Self

## **Problem Statement**

* Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.
* The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.
* You must write an algorithm running in O(n) time and without using the division operation.
* Example:
  * Input: nums = [1,2,3,4]
  * Output: [24,12,8,6]
  * Explanation: 
    * answer[0] = 2*3*4 = 24
    * answer[1] = 1*3*4 = 12
    * answer[2] = 1*2*4 = 8
    * answer[3] = 1*2*3 = 6
* Constraints:
  * 2 <= nums.length <= 10^5
  * -30 <= nums[i] <= 30
  * The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer

---

## **Step 1: Thinking Process (Brute Force)**

* For each index i, we need to calculate the product of all elements except nums[i]
* The most straightforward approach is to iterate through the array for each element
* For each element, we compute the product of all other elements
* This will involve nested loops - one to iterate through each position, and another to calculate the product

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty result array of the same length as nums
2. For each index i from 0 to n-1:
   1. Initialize product = 1
   2. For each index j from 0 to n-1:
      1. If i != j, multiply product by nums[j]
   3. Set result[i] = product
3. Return the result array

---

## **Step 3: Brute Force Implementation (Code)**

```python
def productExceptSelf(nums):
    n = len(nums)
    result = [0] * n
    
    for i in range(n):
        product = 1
        for j in range(n):
            if i != j:
                product *= nums[j]
        result[i] = product
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the length of the array. We have two nested loops, each iterating through the array.
* Space Complexity: O(1) extra space (not counting the output array).
* This is inefficient because we're recalculating many of the same products repeatedly.

---

## **Step 5: Visualizing Redundant Computation**

* For array [1,2,3,4]:
  * For i=0: Calculate 2*3*4
  * For i=1: Calculate 1*3*4
  * For i=2: Calculate 1*2*4
  * For i=3: Calculate 1*2*3
* Notice that we're doing many redundant multiplications. For example, when calculating the product for i=0 and i=1, we're multiplying 3*4 in both cases.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Prefix and Suffix Products algorithm.
* The property that matches this problem is the ability to precompute products and reuse them.
* By calculating prefix and suffix products, we can compute the result for each position in O(1) time after preprocessing.
* Alternatives like using division would be simpler but are explicitly disallowed by the problem.
* Precondition: The problem guarantees that all products fit within a 32-bit integer.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of recalculating products for each element, we can precompute them
* For each position i, the result is the product of all elements to the left of i multiplied by the product of all elements to the right of i
* We can calculate prefix products (product of all elements to the left) and suffix products (product of all elements to the right) in two passes
* Then combine them to get the final result

### **Step 2: Flow Steps (Optimized)**

1. Initialize an array `result` of the same length as `nums` with all 1s
2. Calculate prefix products:
   1. Initialize `prefix = 1`
   2. For each index i from 0 to n-1:
      1. Set `result[i] *= prefix`
      2. Update `prefix *= nums[i]`
3. Calculate suffix products and multiply with prefix products:
   1. Initialize `suffix = 1`
   2. For each index i from n-1 down to 0:
      1. Set `result[i] *= suffix`
      2. Update `suffix *= nums[i]`
4. Return the result array

### **Step 3: Implementation (Optimized)**

```python
def productExceptSelf(nums):
    n = len(nums)
    result = [1] * n
    
    # Calculate prefix products
    prefix = 1
    for i in range(n):
        result[i] = prefix
        prefix *= nums[i]
    
    # Calculate suffix products and multiply with prefix products
    suffix = 1
    for i in range(n-1, -1, -1):
        result[i] *= suffix
        suffix *= nums[i]
    
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. We make two passes through the array.
* Space Complexity: O(1) extra space (not counting the output array).
* This is much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For array [1,2,3,4]:
  * First pass (prefix products):
    * result[0] = 1 (no elements to the left)
    * result[1] = 1 (product of elements to the left)
    * result[2] = 1*2 = 2 (product of elements to the left)
    * result[3] = 1*2*3 = 6 (product of elements to the left)
  * Second pass (suffix products):
    * result[3] = 6*1 = 6 (no elements to the right)
    * result[2] = 2*4 = 8 (product of elements to the right)
    * result[1] = 1*4*3 = 12 (product of elements to the right)
    * result[0] = 1*4*3*2 = 24 (product of elements to the right)
  * Final result: [24,12,8,6]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Recalculates products for each element |
| Optimized (Algorithm: Prefix/Suffix Products) | O(n) | O(1) | Uses precomputed products |

---

## **Final Takeaways**

* Precomputing values and reusing them can significantly reduce time complexity.
* The key insight is recognizing that for each position, we need the product of all elements to the left and all elements to the right.
* This problem demonstrates how breaking down a calculation into separate components (prefix and suffix) can lead to more efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're calculating the total product of a factory line for each day a specific machine is under maintenance. The brute force approach would be to recalculate the entire product for each day by multiplying all machines except the one under maintenance. The optimized approach is like keeping a running total of productivity before and after each machine - then for any specific machine, you just multiply the "before" and "after" values to get the total without that machine.

---

## **Summary**

* We solved the "Product of Array Except Self" problem by first using a brute force approach that recalculates products for each element, resulting in O(n²) time complexity. We then optimized using a prefix and suffix products approach, which precomputes products in two passes through the array, reducing the time complexity to O(n). This demonstrates the power of preprocessing and reusing calculations to avoid redundant work. 