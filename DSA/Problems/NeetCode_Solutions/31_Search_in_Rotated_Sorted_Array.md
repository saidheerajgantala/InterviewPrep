# 📝 Search in Rotated Sorted Array

## **Problem Statement**

* There is an integer array `nums` sorted in ascending order (with distinct values).
* Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index `k` (1 <= k < nums.length) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index 3 and become `[4,5,6,7,0,1,2]`.
* Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.
* You must write an algorithm with O(log n) runtime complexity.
* Example:
  * Input: nums = [4,5,6,7,0,1,2], target = 0
  * Output: 4
  * Explanation: Target 0 is found at index 4.
* Constraints:
  * 1 <= nums.length <= 5000
  * -10^4 <= nums[i] <= 10^4
  * All values of nums are unique.
  * nums is an ascending array that is possibly rotated.
  * -10^4 <= target <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the index of the target value in a rotated sorted array
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

* For nums = [4,5,6,7,0,1,2] and target = 0:
  * Check nums[0] = 4, not equal to target
  * Check nums[1] = 5, not equal to target
  * Check nums[2] = 6, not equal to target
  * Check nums[3] = 7, not equal to target
  * Check nums[4] = 0, equal to target, return 4
* We're checking each element one by one, which is inefficient for large arrays
* Since the array is sorted and then rotated, we can use this property to narrow down the search space

---

## **Why are we using this algorithm?**

* For optimization, we'll use a modified Binary Search algorithm.
* The property that matches this problem is the ability to efficiently search in a sorted array, even after rotation.
* By identifying which half of the array is sorted and whether the target lies in that half, we can narrow down the search space in each step.
* Alternatives like linear search would be O(n), which doesn't meet the requirement.
* Precondition: The array must be sorted and then possibly rotated.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can use a modified binary search to handle the rotation
* In a regular binary search, we compare the middle element with the target and decide which half to search next
* In a rotated array, we need to first determine which half is sorted
* Then, we check if the target lies in the sorted half
* If it does, we search in that half; otherwise, we search in the other half
* This way, we can still narrow down the search space by half in each step

### **Step 2: Flow Steps (Optimized)**

1. Initialize left = 0 and right = n - 1
2. While left <= right:
   1. Calculate mid = left + (right - left) // 2
   2. If nums[mid] equals target, return mid
   3. Determine which half is sorted:
      1. If nums[left] <= nums[mid], the left half is sorted
         1. If nums[left] <= target < nums[mid], search in the left half: right = mid - 1
         2. Otherwise, search in the right half: left = mid + 1
      2. Otherwise, the right half is sorted
         1. If nums[mid] < target <= nums[right], search in the right half: left = mid + 1
         2. Otherwise, search in the left half: right = mid - 1
3. If the target is not found, return -1

### **Step 3: Implementation (Optimized)**

```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if nums[mid] == target:
            return mid
        
        # Check if the left half is sorted
        if nums[left] <= nums[mid]:
            # Check if the target is in the left half
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:  # The right half is sorted
            # Check if the target is in the right half
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(log n) where n is the length of the array. In each step, we halve the search space.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach meets the requirement of O(log n) runtime complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For nums = [4,5,6,7,0,1,2] and target = 0:
  * Initialize left = 0, right = 6
  * mid = 3, nums[mid] = 7 != target
  * nums[left] = 4 <= nums[mid] = 7, so the left half is sorted
  * target = 0 is not in the range [4, 7), so search in the right half: left = mid + 1 = 4
  * left = 4, right = 6, mid = 5, nums[mid] = 1 != target
  * nums[left] = 0 <= nums[mid] = 1, so the left half is sorted
  * target = 0 is in the range [0, 1), so search in the left half: right = mid - 1 = 4
  * left = 4, right = 4, mid = 4, nums[mid] = 0 == target, return 4
* We're efficiently narrowing down the search space by identifying which half is sorted and whether the target lies in that half.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Linear Search) | O(n) | O(1) | Checks each element one by one |
| Optimized (Algorithm: Modified Binary Search) | O(log n) | O(1) | Identifies which half is sorted and whether the target lies in that half |

---

## **Final Takeaways**

* Binary search can be adapted to handle rotated sorted arrays by identifying which half is sorted and whether the target lies in that half.
* The key insight is that at least one half of the array must be sorted, even after rotation.
* This problem demonstrates how understanding the properties of the input data can lead to efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're looking for a specific book in a library where the shelves are arranged in alphabetical order, but someone has moved a section of books from the beginning to the end (a rotation). The brute force approach would be like checking each book one by one until you find the one you're looking for. The optimized approach is like first identifying which section of the library is still in alphabetical order, checking if your book would be in that section based on its title, and then focusing your search there - this way, you can quickly narrow down your search without checking every book.

---

## **Summary**

* We solved the "Search in Rotated Sorted Array" problem by first considering a linear search approach that checks each element one by one, resulting in O(n) time complexity. We then optimized using a modified binary search approach that identifies which half of the array is sorted and whether the target lies in that half, reducing the time complexity to O(log n) while maintaining O(1) space complexity. The key insight is that at least one half of the array must be sorted, even after rotation, which allows us to narrow down the search space efficiently. 