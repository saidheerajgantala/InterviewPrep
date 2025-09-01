# 📝 Find the Duplicate Number

## **Problem Statement**

* Given an array of integers `nums` containing n + 1 integers where each integer is in the range [1, n] inclusive.
* There is only one repeated number in `nums`, return this repeated number.
* You must solve the problem without modifying the array `nums` and uses only constant extra space.
* Example:
  * Input: nums = [1,3,4,2,2]
  * Output: 2
* Constraints:
  * 1 <= n <= 10^5
  * nums.length == n + 1
  * 1 <= nums[i] <= n
  * All the integers in nums appear only once except for precisely one integer which appears two or more times.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the duplicate number in an array
* One straightforward approach would be to check each number against all other numbers
* For each number, we iterate through the array and count its occurrences
* If the count is greater than 1, we've found the duplicate
* However, this approach has a time complexity of O(n²)

---

## **Step 2: Flow Steps (Brute Force)**

1. For each number in the array:
   1. Count its occurrences in the array
   2. If the count is greater than 1, return the number

---

## **Step 3: Brute Force Implementation (Code)**

```python
def findDuplicate(nums):
    n = len(nums)
    
    for i in range(n):
        count = 0
        for j in range(n):
            if nums[j] == nums[i]:
                count += 1
        if count > 1:
            return nums[i]
    
    return -1  # No duplicate found (should not happen given the constraints)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the length of the array. For each number, we iterate through the entire array.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient for large arrays and doesn't meet the requirement of using only constant extra space.

---

## **Step 5: Visualizing Redundant Computation**

* For nums = [1,3,4,2,2]:
  * Check nums[0] = 1: count = 1, not a duplicate
  * Check nums[1] = 3: count = 1, not a duplicate
  * Check nums[2] = 4: count = 1, not a duplicate
  * Check nums[3] = 2: count = 2, found a duplicate, return 2
* We're checking each number against all other numbers, which is redundant
* We can optimize this by using more efficient approaches

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Cycle Detection algorithm (Floyd's Tortoise and Hare).
* The property that matches this problem is the ability to find a duplicate number in an array without modifying the array and using only constant extra space.
* By treating the array as a linked list where the value at each index points to the next index, we can use cycle detection to find the duplicate.
* Alternatives like sorting or using a hash set would either modify the array or use extra space, which doesn't meet the requirements.
* Precondition: There is exactly one duplicate number in the array.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can use Floyd's Tortoise and Hare algorithm (cycle detection)
* The key insight is to treat the array as a linked list where the value at each index points to the next index
* Since there is a duplicate number, there must be a cycle in this linked list
* We can use the cycle detection algorithm to find the entrance to the cycle, which is the duplicate number

### **Step 2: Flow Steps (Optimized)**

1. Initialize two pointers, slow and fast, to the first element of the array
2. Move slow one step at a time and fast two steps at a time until they meet
3. Reset one pointer to the start and keep the other at the meeting point
4. Move both pointers one step at a time until they meet again
5. The meeting point is the duplicate number

### **Step 3: Implementation (Optimized)**

```python
def findDuplicate(nums):
    # Phase 1: Find the intersection point of the two pointers
    slow = nums[0]
    fast = nums[0]
    
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    
    # Phase 2: Find the entrance to the cycle
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    
    return slow
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. We traverse the array at most twice.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is optimal in terms of both time and space complexity, and it meets the requirements of not modifying the array and using only constant extra space.

### **Step 5: Visualizing Computation (Optimized)**

* For nums = [1,3,4,2,2]:
  * Initialize slow = nums[0] = 1, fast = nums[0] = 1
  * Iteration 1: slow = nums[1] = 3, fast = nums[nums[1]] = nums[3] = 2
  * Iteration 2: slow = nums[3] = 2, fast = nums[nums[2]] = nums[4] = 2
  * slow == fast, so we break out of the loop
  * Reset slow = nums[0] = 1, keep fast = 2
  * Iteration 1: slow = nums[1] = 3, fast = nums[2] = 4
  * Iteration 2: slow = nums[3] = 2, fast = nums[4] = 2
  * slow == fast, so we return slow = 2
* We're efficiently finding the duplicate number without modifying the array and using only constant extra space.

### **Alternative Approach: Binary Search**

Another approach is to use binary search on the range [1, n]:

```python
def findDuplicate(nums):
    left, right = 1, len(nums) - 1
    
    while left < right:
        mid = left + (right - left) // 2
        count = sum(1 for num in nums if num <= mid)
        
        if count > mid:
            right = mid
        else:
            left = mid + 1
    
    return left
```

This approach also has O(n log n) time complexity and O(1) space complexity, but it's a bit more complex to understand.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Checks each number against all other numbers |
| Optimized (Algorithm: Cycle Detection) | O(n) | O(1) | Uses Floyd's Tortoise and Hare algorithm |
| Alternative (Algorithm: Binary Search) | O(n log n) | O(1) | Uses binary search on the range [1, n] |

---

## **Final Takeaways**

* The key to finding the duplicate number without modifying the array and using only constant extra space is to treat the array as a linked list and use cycle detection.
* By using Floyd's Tortoise and Hare algorithm, we can efficiently find the entrance to the cycle, which is the duplicate number.
* This problem demonstrates how creative problem-solving can lead to efficient algorithms that meet strict constraints.

---

## **Real-life Analogy**

* Imagine a circular race track with multiple entrances, but only one entrance has two paths leading to it (the duplicate number). If two runners start at the beginning and one runs twice as fast as the other, they will eventually meet somewhere on the track. By then resetting one runner to the beginning and having both run at the same speed, they will meet again at the entrance with two paths leading to it, which is the duplicate number.

---

## **Summary**

* We solved the "Find the Duplicate Number" problem by first considering a brute force approach that checks each number against all other numbers, resulting in O(n²) time complexity. We then optimized using Floyd's Tortoise and Hare algorithm (cycle detection) by treating the array as a linked list where the value at each index points to the next index, reducing the time complexity to O(n) while maintaining O(1) space complexity. The key insight is to find the entrance to the cycle in this linked list, which is the duplicate number. 