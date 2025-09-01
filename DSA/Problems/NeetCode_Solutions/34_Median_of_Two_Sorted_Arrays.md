# 📝 Median of Two Sorted Arrays

## **Problem Statement**

* Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return the median of the two sorted arrays.
* The overall run time complexity should be O(log (m+n)).
* Example:
  * Input: nums1 = [1,3], nums2 = [2]
  * Output: 2.00000
  * Explanation: merged array = [1,2,3] and median is 2.
* Constraints:
  * nums1.length == m
  * nums2.length == n
  * 0 <= m <= 1000
  * 0 <= n <= 1000
  * 1 <= m + n <= 2000
  * -10^6 <= nums1[i], nums2[i] <= 10^6

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the median of two sorted arrays
* The median is the middle element of a sorted array if the array has an odd length, or the average of the two middle elements if the array has an even length
* A straightforward approach would be to merge the two sorted arrays and then find the median
* We can use the merge procedure from merge sort to combine the two sorted arrays
* Then, we can find the median by accessing the middle element(s)

---

## **Step 2: Flow Steps (Brute Force)**

1. Merge the two sorted arrays into a single sorted array
2. Calculate the median:
   1. If the merged array has an odd length, return the middle element
   2. If the merged array has an even length, return the average of the two middle elements

---

## **Step 3: Brute Force Implementation (Code)**

```python
def findMedianSortedArrays(nums1, nums2):
    # Merge the two sorted arrays
    merged = []
    i, j = 0, 0
    
    while i < len(nums1) and j < len(nums2):
        if nums1[i] <= nums2[j]:
            merged.append(nums1[i])
            i += 1
        else:
            merged.append(nums2[j])
            j += 1
    
    # Add any remaining elements
    while i < len(nums1):
        merged.append(nums1[i])
        i += 1
    
    while j < len(nums2):
        merged.append(nums2[j])
        j += 1
    
    # Calculate the median
    n = len(merged)
    if n % 2 == 1:
        return merged[n // 2]
    else:
        return (merged[n // 2 - 1] + merged[n // 2]) / 2
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(m + n) where m and n are the lengths of the two arrays. Merging the arrays takes O(m + n) time.
* Space Complexity: O(m + n) for storing the merged array.
* This approach doesn't meet the requirement of O(log (m+n)) time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For nums1 = [1,3] and nums2 = [2]:
  * Merge the arrays: [1,2,3]
  * Calculate the median: 2
* We're merging the entire arrays, which is inefficient if we only need the median
* We don't need to merge the entire arrays; we just need to find the middle element(s)
* We can use a binary search approach to find the median in O(log (m+n)) time

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Binary Search algorithm.
* The property that matches this problem is the ability to efficiently find the kth element in two sorted arrays.
* By using binary search on the smaller array, we can narrow down the search space and find the median in O(log (min(m,n))) time.
* Alternatives like merging the arrays would be O(m + n), which doesn't meet the requirement.
* Precondition: Both arrays are sorted.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can use binary search to find the median without merging the arrays
* The key insight is to partition the arrays in such a way that:
  * The left half of both arrays combined has the same number of elements as the right half
  * Every element in the left half is smaller than or equal to every element in the right half
* Once we have such a partition, the median is either the maximum of the left half (for odd total length) or the average of the maximum of the left half and the minimum of the right half (for even total length)
* We can binary search on the smaller array to find the correct partition

### **Step 2: Flow Steps (Optimized)**

1. Ensure nums1 is the smaller array (if not, swap nums1 and nums2)
2. Define left = 0 and right = m (where m is the length of nums1)
3. While left <= right:
   1. Calculate partition1 = (left + right) // 2
   2. Calculate partition2 = (m + n + 1) // 2 - partition1
   3. Calculate maxLeft1, minRight1, maxLeft2, minRight2
   4. If maxLeft1 <= minRight2 and maxLeft2 <= minRight1:
      1. We have found the correct partition
      2. Calculate the median based on the total length (odd or even)
   5. If maxLeft1 > minRight2:
      1. Move the partition1 to the left: right = partition1 - 1
   6. If maxLeft2 > minRight1:
      1. Move the partition1 to the right: left = partition1 + 1

### **Step 3: Implementation (Optimized)**

```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    m, n = len(nums1), len(nums2)
    left, right = 0, m
    
    while left <= right:
        partition1 = (left + right) // 2
        partition2 = (m + n + 1) // 2 - partition1
        
        # Edge cases
        maxLeft1 = float('-inf') if partition1 == 0 else nums1[partition1 - 1]
        minRight1 = float('inf') if partition1 == m else nums1[partition1]
        maxLeft2 = float('-inf') if partition2 == 0 else nums2[partition2 - 1]
        minRight2 = float('inf') if partition2 == n else nums2[partition2]
        
        if maxLeft1 <= minRight2 and maxLeft2 <= minRight1:
            # We have found the correct partition
            if (m + n) % 2 == 0:
                return (max(maxLeft1, maxLeft2) + min(minRight1, minRight2)) / 2
            else:
                return max(maxLeft1, maxLeft2)
        elif maxLeft1 > minRight2:
            # Move partition1 to the left
            right = partition1 - 1
        else:
            # Move partition1 to the right
            left = partition1 + 1
    
    # This should never happen if the input arrays are sorted
    return 0
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(log (min(m,n))) where m and n are the lengths of the two arrays. We perform binary search on the smaller array.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach meets the requirement of O(log (m+n)) time complexity, as log(min(m,n)) <= log(m+n).

### **Step 5: Visualizing Computation (Optimized)**

* For nums1 = [1,3] and nums2 = [2]:
  * m = 2, n = 1
  * left = 0, right = 2
  * partition1 = 1, partition2 = 1
  * maxLeft1 = 1, minRight1 = 3, maxLeft2 = 2, minRight2 = infinity
  * maxLeft1 <= minRight2 and maxLeft2 <= minRight1, so we have found the correct partition
  * (m + n) % 2 == 0 is false, so the median is max(maxLeft1, maxLeft2) = max(1, 2) = 2
* We're efficiently finding the median without merging the arrays.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Merge) | O(m + n) | O(m + n) | Merges the arrays and then finds the median |
| Optimized (Algorithm: Binary Search) | O(log (min(m,n))) | O(1) | Uses binary search to find the correct partition |

---

## **Final Takeaways**

* Binary search can be used to efficiently find the median of two sorted arrays without merging them.
* The key insight is to partition the arrays in such a way that the left half of both arrays combined has the same number of elements as the right half, and every element in the left half is smaller than or equal to every element in the right half.
* This problem demonstrates how binary search can be applied to problems beyond just searching in a sorted array.

---

## **Real-life Analogy**

* Imagine you have two sorted stacks of cards, and you want to find the middle card if you were to merge them. The brute force approach would be like merging the two stacks and then picking the middle card. The optimized approach is like intelligently partitioning the stacks without actually merging them - by using binary search to find the right partition points, you can determine the middle card(s) without having to go through the entire merging process.

---

## **Summary**

* We solved the "Median of Two Sorted Arrays" problem by first considering a brute force approach that merges the arrays and then finds the median, resulting in O(m + n) time complexity. We then optimized using a binary search approach that partitions the arrays without merging them, reducing the time complexity to O(log (min(m,n))) while maintaining O(1) space complexity. The key insight is to find the correct partition points such that the left half of both arrays combined has the same number of elements as the right half, and every element in the left half is smaller than or equal to every element in the right half. 