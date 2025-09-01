# 📝 Find Minimum in Rotated Sorted Array

## **Problem Statement**

* Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:
  * `[4,5,6,7,0,1,2]` if it was rotated `4` times.
  * `[0,1,2,4,5,6,7]` if it was rotated `7` times.
* Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.
* Given the sorted rotated array `nums` of unique elements, return the minimum element of this array.
* You must write an algorithm that runs in O(log n) time.
* Example:
  * Input: nums = [3,4,5,1,2]
  * Output: 1
  * Explanation: The original array was [1,2,3,4,5] and it was rotated 3 times.
* Constraints:
  * n == nums.length
  * 1 <= n <= 5000
  * -5000 <= nums[i] <= 5000
  * All the integers of nums are unique.
  * nums is sorted and rotated between 1 and n times.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the minimum element in a rotated sorted array
* The most straightforward approach would be to iterate through the array and keep track of the minimum element
* This approach has a time complexity of O(n), which doesn't meet the requirement of O(log n)
* However, it's a good starting point to understand the problem

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize min_val = infinity
2. Iterate through each element in the array:
   1. Update min_val = min(min_val, current element)
3. Return min_val

---

## **Step 3: Brute Force Implementation (Code)**

```python
def findMin(nums):
    min_val = float('inf')
    
    for num in nums:
        min_val = min(min_val, num)
    
    return min_val
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the length of the array. We need to check all elements to find the minimum.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach doesn't meet the requirement of O(log n) runtime complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For nums = [3,4,5,1,2]:
  * Check nums[0] = 3, min_val = 3
  * Check nums[1] = 4, min_val = 3
  * Check nums[2] = 5, min_val = 3
  * Check nums[3] = 1, min_val = 1
  * Check nums[4] = 2, min_val = 1
  * Return min_val = 1
* We're checking each element one by one, which is inefficient for large arrays
* Since the array is sorted and then rotated, we can use this property to narrow down the search space

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Binary Search algorithm.
* The property that matches this problem is the ability to efficiently find the minimum element in a rotated sorted array.
* By comparing the middle element with the rightmost element, we can determine which half contains the minimum element.
* Alternatives like linear search would be O(n), which doesn't meet the requirement.
* Precondition: The array must be sorted and then possibly rotated.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can use binary search to efficiently find the minimum element
* In a rotated sorted array, the minimum element is the only one that is smaller than both its neighbors
* Alternatively, it's the only element that is smaller than the rightmost element of the array
* We can use this property to narrow down the search space
* If the middle element is greater than the rightmost element, the minimum must be in the right half
* If the middle element is less than the rightmost element, the minimum must be in the left half (including the middle element)

### **Step 2: Flow Steps (Optimized)**

1. Initialize left = 0 and right = n - 1
2. While left < right:
   1. Calculate mid = left + (right - left) // 2
   2. If nums[mid] > nums[right], the minimum is in the right half: left = mid + 1
   3. Otherwise, the minimum is in the left half (including the middle element): right = mid
3. Return nums[left] (or nums[right], they are the same at this point)

### **Step 3: Implementation (Optimized)**

```python
def findMin(nums):
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = left + (right - left) // 2
        
        if nums[mid] > nums[right]:
            # The minimum is in the right half
            left = mid + 1
        else:
            # The minimum is in the left half (including the middle element)
            right = mid
    
    return nums[left]
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(log n) where n is the length of the array. In each step, we halve the search space.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach meets the requirement of O(log n) runtime complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For nums = [3,4,5,1,2]:
  * Initialize left = 0, right = 4
  * mid = 2, nums[mid] = 5, nums[right] = 2
  * nums[mid] > nums[right], so the minimum is in the right half: left = mid + 1 = 3
  * left = 3, right = 4, mid = 3, nums[mid] = 1, nums[right] = 2
  * nums[mid] < nums[right], so the minimum is in the left half (including the middle element): right = mid = 3
  * left = 3, right = 3, left == right, so we exit the loop
  * Return nums[left] = nums[3] = 1
* We're efficiently narrowing down the search space by comparing the middle element with the rightmost element.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Linear Search) | O(n) | O(1) | Checks each element one by one |
| Optimized (Algorithm: Binary Search) | O(log n) | O(1) | Uses the property of rotated sorted arrays to narrow down the search space |

---

## **Final Takeaways**

* Binary search can be adapted to find the minimum element in a rotated sorted array by comparing the middle element with the rightmost element.
* The key insight is that in a rotated sorted array, the minimum element is the only one that is smaller than both its neighbors, or equivalently, it's the only element that is smaller than the rightmost element of the array.
* This problem demonstrates how understanding the properties of the input data can lead to efficient algorithms.

---

## **Real-life Analogy**

* Imagine you have a circular bookshelf where books are arranged in alphabetical order, but someone has rotated the shelf so that it no longer starts with 'A'. The brute force approach would be like checking each book one by one to find the one that comes first alphabetically. The optimized approach is like using the fact that the books are still in alphabetical order (just rotated) to efficiently find the first book - by comparing books at different positions and narrowing down the search space based on the alphabetical ordering.

---

## **Summary**

* We solved the "Find Minimum in Rotated Sorted Array" problem by first considering a linear search approach that checks each element one by one, resulting in O(n) time complexity. We then optimized using a binary search approach that compares the middle element with the rightmost element to determine which half contains the minimum element, reducing the time complexity to O(log n) while maintaining O(1) space complexity. The key insight is to leverage the property that in a rotated sorted array, the minimum element is the only one that is smaller than the rightmost element of the array. 