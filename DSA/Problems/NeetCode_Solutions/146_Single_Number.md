# 📝 Single Number

## **Problem Statement**

* Given a non-empty array of integers `nums`, every element appears twice except for one. Find that single one.
* You must implement a solution with a linear runtime complexity and use only constant extra space.
* Example:
  * Input: nums = [2,2,1]
  * Output: 1
  * Input: nums = [4,1,2,1,2]
  * Output: 4
  * Input: nums = [1]
  * Output: 1
* Constraints:
  * 1 <= nums.length <= 3 * 10^4
  * -3 * 10^4 <= nums[i] <= 3 * 10^4
  * Each element in the array appears twice except for one element which appears only once.

---

## **Step 1: Thinking Process (Brute Force)**

* A straightforward approach would be to count the occurrences of each number and find the one that appears only once.
* We can use a hash map to store the count of each number.
* After counting, we iterate through the hash map to find the number with a count of 1.
* However, this approach uses O(n) extra space, which doesn't meet the requirement of using only constant extra space.

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a hash map to store the count of each number.
2. Iterate through the array:
   1. Increment the count of the current number in the hash map.
3. Iterate through the hash map:
   1. If the count of a number is 1, return that number.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def singleNumber(nums):
    count = {}
    
    # Count the occurrences of each number
    for num in nums:
        count[num] = count.get(num, 0) + 1
    
    # Find the number with a count of 1
    for num, freq in count.items():
        if freq == 1:
            return num
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the length of the array. We iterate through the array once to count the occurrences and then iterate through the hash map.
* Space Complexity: O(n) for storing the hash map.
* This approach doesn't meet the requirement of using only constant extra space.

---

## **Step 5: Visualizing Computation**

* For nums = [4,1,2,1,2]:
  * Initialize count = {}
  * Process 4: count = {4: 1}
  * Process 1: count = {4: 1, 1: 1}
  * Process 2: count = {4: 1, 1: 1, 2: 1}
  * Process 1: count = {4: 1, 1: 2, 2: 1}
  * Process 2: count = {4: 1, 1: 2, 2: 2}
  * Iterate through count:
    * 4 has a count of 1, return 4

---

## **Why are we using this algorithm?**

* The brute force approach uses O(n) extra space, which doesn't meet the requirement.
* To use only constant extra space, we can use the XOR (^) operation.
* XOR has the property that a ^ a = 0 and a ^ 0 = a.
* If we XOR all the numbers in the array, the numbers that appear twice will cancel out (become 0), and the single number will remain.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We'll use the XOR operation to find the single number.
* XOR has the property that a ^ a = 0 and a ^ 0 = a.
* If we XOR all the numbers in the array, the numbers that appear twice will cancel out (become 0), and the single number will remain.

### **Step 2: Flow Steps (Optimized)**

1. Initialize result = 0.
2. Iterate through the array:
   1. XOR the current number with result: result ^= num.
3. Return result.

### **Step 3: Implementation (Optimized)**

```python
def singleNumber(nums):
    result = 0
    
    for num in nums:
        result ^= num
    
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. We iterate through the array once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach meets the requirement of using only constant extra space.

### **Step 5: Visualizing Computation (Optimized)**

* For nums = [4,1,2,1,2]:
  * Initialize result = 0
  * Process 4: result = 0 ^ 4 = 4
  * Process 1: result = 4 ^ 1 = 5
  * Process 2: result = 5 ^ 2 = 7
  * Process 1: result = 7 ^ 1 = 6
  * Process 2: result = 6 ^ 2 = 4
  * Return result = 4

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Hash Map) | O(n) | O(n) | Uses a hash map to count occurrences |
| Optimized (XOR) | O(n) | O(1) | Uses the XOR operation to find the single number |

---

## **Final Takeaways**

* The Single Number problem is a classic example of how bit manipulation can be used to solve problems efficiently.
* The XOR operation is particularly useful for finding the odd one out in a collection of pairs.
* This problem demonstrates how understanding the properties of bitwise operations can lead to elegant and efficient solutions.

---

## **Real-life Analogy**

* Imagine you have a collection of pairs of identical items, plus one extra item. To find the extra item, you can pair up the identical items and the one that remains unpaired is your answer. XOR works in a similar way by "pairing up" identical numbers and leaving the single number unpaired.

---

## **Summary**

* We tackled the "Single Number" problem by using the XOR operation to find the number that appears only once in the array. XOR has the property that a ^ a = 0 and a ^ 0 = a, so if we XOR all the numbers in the array, the numbers that appear twice will cancel out (become 0), and the single number will remain. This approach has a time complexity of O(n) where n is the length of the array, and a space complexity of O(1), meeting the requirement of using only constant extra space. 