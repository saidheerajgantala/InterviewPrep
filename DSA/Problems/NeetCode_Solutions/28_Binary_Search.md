# 📝 Binary Search

## **Problem Statement**

* Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.
* You must write an algorithm with O(log n) runtime complexity.
* Example:
  * Input: nums = [-1,0,3,5,9,12], target = 9
  * Output: 4
  * Explanation: 9 exists in nums and its index is 4
* Constraints:
  * 1 <= nums.length <= 10^4
  * -10^4 <= nums[i], target <= 10^4
  * All the integers in nums are unique.
  * nums is sorted in ascending order.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the index of the target element in a sorted array
* The most straightforward approach would be to iterate through the array and check each element
* If we find the target, we return its index
* If we reach the end of the array without finding the target, we return -1
* This approach has a time complexity of O(n), which doesn't meet the requirement of O(log n)

---

## **Step 2: Flow Steps (Brute Force)**

1. Iterate through each index i from 0 to n-1:
   1. If nums[i] equals target, return i
2. If the target is not found, return -1

---

## **Step 3: Brute Force Implementation (Code)**

```python
def search(nums, target):
    for i in range(len(nums)):
        if nums[i] == target:
            return i
    return -1
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the length of the array. In the worst case, we might need to check all elements.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach doesn't meet the requirement of O(log n) runtime complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For nums = [-1,0,3,5,9,12] and target = 9:
  * Check nums[0] = -1, not equal to target
  * Check nums[1] = 0, not equal to target
  * Check nums[2] = 3, not equal to target
  * Check nums[3] = 5, not equal to target
  * Check nums[4] = 9, equal to target, return 4
* We're checking each element one by one, which is inefficient for large arrays
* Since the array is sorted, we can use this property to narrow down the search space

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Binary Search algorithm.
* The property that matches this problem is the ability to efficiently search in a sorted array.
* By repeatedly dividing the search space in half, we can find the target in O(log n) time.
* Alternatives like linear search would be O(n), which doesn't meet the requirement.
* Precondition: The array must be sorted.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Since the array is sorted, we can use binary search to efficiently find the target
* We maintain two pointers, left and right, that define the current search space
* In each step, we calculate the middle index and compare the middle element with the target
* If the middle element equals the target, we return its index
* If the middle element is greater than the target, we search in the left half
* If the middle element is less than the target, we search in the right half
* We continue this process until we find the target or the search space is empty

### **Step 2: Flow Steps (Optimized)**

1. Initialize left = 0 and right = n - 1
2. While left <= right:
   1. Calculate mid = left + (right - left) // 2
   2. If nums[mid] equals target, return mid
   3. If nums[mid] > target, set right = mid - 1
   4. If nums[mid] < target, set left = mid + 1
3. If the target is not found, return -1

### **Step 3: Implementation (Optimized)**

```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if nums[mid] == target:
            return mid
        elif nums[mid] > target:
            right = mid - 1
        else:  # nums[mid] < target
            left = mid + 1
    
    return -1
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(log n) where n is the length of the array. In each step, we halve the search space.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach meets the requirement of O(log n) runtime complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For nums = [-1,0,3,5,9,12] and target = 9:
  * Initialize left = 0, right = 5
  * mid = 2, nums[mid] = 3 < target, so left = mid + 1 = 3
  * left = 3, right = 5, mid = 4, nums[mid] = 9 == target, return 4
* We're efficiently narrowing down the search space by half in each step.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Linear Search) | O(n) | O(1) | Checks each element one by one |
| Optimized (Algorithm: Binary Search) | O(log n) | O(1) | Divides the search space in half in each step |

---

## **Final Takeaways**

* Binary search is a powerful algorithm for searching in sorted arrays with O(log n) time complexity.
* The key insight is to repeatedly divide the search space in half based on the comparison with the middle element.
* This problem demonstrates the importance of leveraging the properties of the input data (sorted in this case) to design efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're looking for a name in a physical phone book. The brute force approach would be like starting from the first page and checking each name until you find the one you're looking for. The binary search approach is like opening the phone book in the middle, checking if the name you're looking for comes before or after that page, and then repeating the process with the relevant half of the book. This is much more efficient, especially for large phone books.

---

## **Summary**

* We solved the "Binary Search" problem by first considering a linear search approach that checks each element one by one, resulting in O(n) time complexity. We then optimized using a binary search approach that repeatedly divides the search space in half, reducing the time complexity to O(log n) while maintaining O(1) space complexity. The key insight is to leverage the sorted property of the array to efficiently narrow down the search space in each step.