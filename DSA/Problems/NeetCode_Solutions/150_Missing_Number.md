# 📝 Missing Number

## **Problem Statement**

* Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the only number in the range that is missing from the array.
* Example:
  * Input: nums = [3,0,1]
  * Output: 2 (n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums)
  * Input: nums = [0,1]
  * Output: 2 (n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums)
  * Input: nums = [9,6,4,2,3,5,7,0,1]
  * Output: 8 (n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums)
* Constraints:
  * n == nums.length
  * 1 <= n <= 10^4
  * 0 <= nums[i] <= n
  * All the numbers of nums are unique.
* Follow up: Could you implement a solution using only O(1) extra space complexity and O(n) runtime complexity?

---

## **Step 1: Thinking Process (Brute Force)**

* A straightforward approach would be to sort the array and then iterate through it to find the missing number.
* Another approach would be to use a hash set to store all the numbers in the array, and then check which number in the range [0, n] is missing.
* Let's implement these approaches and then optimize them.

---

## **Step 2: Flow Steps (Brute Force)**

1. Sort the array.
2. Iterate through the array:
   1. If nums[i] != i, return i.
3. If no missing number is found, return n.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def missingNumber(nums):
    nums.sort()
    
    for i in range(len(nums)):
        if nums[i] != i:
            return i
    
    return len(nums)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n log n) where n is the length of the array. Sorting the array takes O(n log n) time.
* Space Complexity: O(1) if we sort the array in place.
* This approach doesn't meet the follow-up requirement of O(n) runtime complexity.

---

## **Step 5: Visualizing Computation**

* For nums = [3,0,1]:
  * Sort: [0,1,3]
  * For i = 0: nums[0] = 0 = i, continue
  * For i = 1: nums[1] = 1 = i, continue
  * For i = 2: nums[2] = 3 != i, return 2
  * Return 2

---

## **Why are we using this algorithm?**

* The brute force approach has a time complexity of O(n log n), which doesn't meet the follow-up requirement of O(n) runtime complexity.
* We can optimize this by using mathematical properties or bit manipulation.
* Let's explore these optimizations.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Mathematical)**

* We can use the mathematical formula for the sum of the first n natural numbers: n * (n + 1) / 2.
* The sum of all numbers in the range [0, n] is n * (n + 1) / 2.
* The missing number is the difference between this sum and the sum of the numbers in the array.

### **Step 2: Flow Steps (Optimized - Mathematical)**

1. Calculate the expected sum of all numbers in the range [0, n]: n * (n + 1) / 2.
2. Calculate the actual sum of the numbers in the array.
3. Return the difference between the expected sum and the actual sum.

### **Step 3: Implementation (Optimized - Mathematical)**

```python
def missingNumber(nums):
    n = len(nums)
    expected_sum = n * (n + 1) // 2
    actual_sum = sum(nums)
    
    return expected_sum - actual_sum
```

### **Step 4: Complexity Analysis (Optimized - Mathematical)**

* Time Complexity: O(n) where n is the length of the array. We iterate through the array once to calculate the sum.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach meets the follow-up requirement of O(n) runtime complexity and O(1) extra space complexity.

### **Step 5: Visualizing Computation (Optimized - Mathematical)**

* For nums = [3,0,1]:
  * n = 3
  * expected_sum = 3 * (3 + 1) / 2 = 6
  * actual_sum = 3 + 0 + 1 = 4
  * Return expected_sum - actual_sum = 6 - 4 = 2

### **Step 1: Thinking Process (Optimized - Bit Manipulation)**

* We can also use the XOR operation to find the missing number.
* XOR has the property that a ^ a = 0 and a ^ 0 = a.
* If we XOR all the numbers in the range [0, n] with all the numbers in the array, the missing number will be the result.

### **Step 2: Flow Steps (Optimized - Bit Manipulation)**

1. Initialize result = n.
2. For each index i from 0 to n-1:
   1. XOR result with i and nums[i].
3. Return result.

### **Step 3: Implementation (Optimized - Bit Manipulation)**

```python
def missingNumber(nums):
    result = len(nums)
    
    for i in range(len(nums)):
        result ^= i ^ nums[i]
    
    return result
```

### **Step 4: Complexity Analysis (Optimized - Bit Manipulation)**

* Time Complexity: O(n) where n is the length of the array. We iterate through the array once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach also meets the follow-up requirement of O(n) runtime complexity and O(1) extra space complexity.

### **Step 5: Visualizing Computation (Optimized - Bit Manipulation)**

* For nums = [3,0,1]:
  * Initialize result = 3
  * For i = 0: result ^= 0 ^ 3 = 3 ^ 0 ^ 3 = 0
  * For i = 1: result ^= 1 ^ 0 = 0 ^ 1 ^ 0 = 1
  * For i = 2: result ^= 2 ^ 1 = 1 ^ 2 ^ 1 = 2
  * Return result = 2

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Sorting) | O(n log n) | O(1) | Sorts the array and checks for missing number |
| Optimized (Mathematical) | O(n) | O(1) | Uses the sum formula |
| Optimized (Bit Manipulation) | O(n) | O(1) | Uses XOR operation |

---

## **Final Takeaways**

* The Missing Number problem can be solved efficiently using mathematical properties or bit manipulation.
* Both optimized approaches have a time complexity of O(n) and a space complexity of O(1), meeting the follow-up requirements.
* This problem demonstrates how understanding mathematical properties and bit manipulation can lead to elegant and efficient solutions.

---

## **Real-life Analogy**

* Imagine you have a set of numbered cards from 0 to n, but one card is missing. The mathematical approach is like calculating the expected sum of all cards and then subtracting the actual sum to find the missing card. The bit manipulation approach is like pairing up each card with its corresponding number, and the unpaired card is the missing one.

---

## **Summary**

* We tackled the "Missing Number" problem using three approaches: sorting, mathematical, and bit manipulation. The sorting approach has a time complexity of O(n log n), while the mathematical and bit manipulation approaches have a time complexity of O(n). All three approaches have a space complexity of O(1). The mathematical approach uses the formula for the sum of the first n natural numbers, while the bit manipulation approach uses the XOR operation to find the missing number. Both optimized approaches meet the follow-up requirement of O(n) runtime complexity and O(1) extra space complexity. 