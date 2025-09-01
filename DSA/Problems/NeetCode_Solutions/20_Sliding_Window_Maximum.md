# 📝 Sliding Window Maximum

## **Problem Statement**

* You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.
* Return the max sliding window.
* Example:
  * Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
  * Output: [3,3,5,5,6,7]
  * Explanation: 
    * Window position                Max
    * ---------------               -----
    * [1  3  -1] -3  5  3  6  7       3
    *  1 [3  -1  -3] 5  3  6  7       3
    *  1  3 [-1  -3  5] 3  6  7       5
    *  1  3  -1 [-3  5  3] 6  7       5
    *  1  3  -1  -3 [5  3  6] 7       6
    *  1  3  -1  -3  5 [3  6  7]      7
* Constraints:
  * 1 <= nums.length <= 10^5
  * -10^4 <= nums[i] <= 10^4
  * 1 <= k <= nums.length

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the maximum element in each sliding window of size k
* The most straightforward approach would be to check all elements in each window
* For each window position, we find the maximum element in that window
* We add this maximum to our result array
* This approach is simple but inefficient for large arrays

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty result array
2. For each window position i from 0 to n-k:
   1. Find the maximum element in the window nums[i:i+k]
   2. Add this maximum to the result array
3. Return the result array

---

## **Step 3: Brute Force Implementation (Code)**

```python
def maxSlidingWindow(nums, k):
    n = len(nums)
    result = []
    
    for i in range(n - k + 1):
        # Find the maximum in the current window
        max_val = max(nums[i:i+k])
        result.append(max_val)
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n * k), where n is the length of the array.
  * We have n-k+1 windows
  * For each window, we spend O(k) time finding the maximum
* Space Complexity: O(n-k+1) for storing the result array.
* This approach is inefficient for large arrays and large window sizes.

---

## **Step 5: Visualizing Redundant Computation**

* For array [1,3,-1,-3,5,3,6,7] with k = 3:
  * Window 1: [1,3,-1], max = 3
  * Window 2: [3,-1,-3], max = 3
  * Window 3: [-1,-3,5], max = 5
  * Window 4: [-3,5,3], max = 5
  * Window 5: [5,3,6], max = 6
  * Window 6: [3,6,7], max = 7
* We're repeatedly finding the maximum in overlapping windows
* When we slide the window, we only remove one element and add one new element
* But we're recomputing the maximum from scratch for each window

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Deque (Double-ended Queue) data structure.
* The property that matches this problem is the ability to efficiently track the maximum element in a sliding window.
* By maintaining a deque of potential maximum elements, we can find the maximum in each window in O(1) time.
* Alternatives like using a heap would be less efficient for this specific problem.
* Precondition: We need to efficiently remove elements that are no longer in the window and maintain elements in decreasing order.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of recomputing the maximum for each window, we can use a deque to keep track of potential maximum elements
* The deque will store indices of elements in the array
* We maintain the deque such that:
  1. Elements in the deque are in decreasing order (from front to back)
  2. Elements outside the current window are removed
* The front of the deque will always contain the index of the maximum element in the current window
* This way, we can find the maximum in each window in O(1) time

### **Step 2: Flow Steps (Optimized)**

1. Initialize an empty result array and an empty deque
2. Process the first k elements:
   1. For each element, remove all smaller elements from the back of the deque
   2. Add the current element's index to the back of the deque
3. Add the maximum element (front of the deque) to the result array
4. For each remaining element at index i from k to n-1:
   1. Remove elements outside the window (indices <= i-k) from the front of the deque
   2. Remove all smaller elements from the back of the deque
   3. Add the current element's index to the back of the deque
   4. Add the maximum element (front of the deque) to the result array
5. Return the result array

### **Step 3: Implementation (Optimized)**

```python
from collections import deque

def maxSlidingWindow(nums, k):
    n = len(nums)
    if n == 0 or k == 0:
        return []
    
    if k == 1:
        return nums
    
    result = []
    dq = deque()  # Deque to store indices of elements
    
    # Process the first k elements
    for i in range(k):
        # Remove all smaller elements from the back of the deque
        while dq and nums[i] >= nums[dq[-1]]:
            dq.pop()
        
        # Add the current element's index to the back of the deque
        dq.append(i)
    
    # Add the maximum element to the result
    result.append(nums[dq[0]])
    
    # Process the remaining elements
    for i in range(k, n):
        # Remove elements outside the window
        if dq and dq[0] <= i - k:
            dq.popleft()
        
        # Remove all smaller elements from the back of the deque
        while dq and nums[i] >= nums[dq[-1]]:
            dq.pop()
        
        # Add the current element's index to the back of the deque
        dq.append(i)
        
        # Add the maximum element to the result
        result.append(nums[dq[0]])
    
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n), where n is the length of the array.
  * Each element is pushed and popped from the deque at most once
  * All other operations are O(1)
* Space Complexity: O(k) for the deque and O(n-k+1) for the result array.
* This is much more efficient than the brute force approach, especially for large arrays and large window sizes.

### **Step 5: Visualizing Computation (Optimized)**

* For array [1,3,-1,-3,5,3,6,7] with k = 3:
  * Process first k elements:
    * i=0, nums[0]=1: dq = [0]
    * i=1, nums[1]=3: 3 > 1, so remove 0, dq = [1]
    * i=2, nums[2]=-1: -1 < 3, so dq = [1,2]
    * Maximum is nums[dq[0]] = nums[1] = 3, result = [3]
  * Process remaining elements:
    * i=3, nums[3]=-3: Remove elements outside window (none), -3 < -1, so dq = [1,2,3]
    * Maximum is nums[dq[0]] = nums[1] = 3, result = [3,3]
    * i=4, nums[4]=5: Remove elements outside window (1), 5 > all, so remove all, dq = [4]
    * Maximum is nums[dq[0]] = nums[4] = 5, result = [3,3,5]
    * i=5, nums[5]=3: Remove elements outside window (none), 3 < 5, so dq = [4,5]
    * Maximum is nums[dq[0]] = nums[4] = 5, result = [3,3,5,5]
    * i=6, nums[6]=6: Remove elements outside window (4), 6 > all, so remove all, dq = [6]
    * Maximum is nums[dq[0]] = nums[6] = 6, result = [3,3,5,5,6]
    * i=7, nums[7]=7: Remove elements outside window (none), 7 > 6, so remove 6, dq = [7]
    * Maximum is nums[dq[0]] = nums[7] = 7, result = [3,3,5,5,6,7]
* We're efficiently finding the maximum in each window by maintaining a deque of potential maximum elements.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n * k) | O(n-k+1) | Recomputes maximum for each window |
| Optimized (Algorithm: Deque) | O(n) | O(k) + O(n-k+1) | Maintains a deque of potential maximum elements |

---

## **Final Takeaways**

* Using a deque allows us to efficiently track the maximum element in a sliding window.
* The key insight is to maintain the deque such that:
  1. Elements in the deque are in decreasing order (from front to back)
  2. Elements outside the current window are removed
* This problem demonstrates how using the right data structure can lead to significant efficiency improvements.

---

## **Real-life Analogy**

* Imagine you're a coach trying to find the tallest player in a team as players enter and leave the field in a first-in, first-out manner. The brute force approach would be like measuring every player on the field each time someone enters or leaves. The optimized approach is like keeping players lined up in descending order of height - when a new player enters, you remove all shorter players from the back of the line (as they can't be the tallest while the new player is on the field), and when a player leaves, you only need to check if they were at the front of the line. This way, the tallest player is always at the front of the line.

---

## **Summary**

* We solved the "Sliding Window Maximum" problem by first considering a brute force approach that recomputes the maximum for each window, resulting in O(n * k) time complexity. We then optimized using a deque to maintain potential maximum elements, reducing the time complexity to O(n) while maintaining O(k) space complexity (plus the result array). The key insight is to maintain the deque such that elements are in decreasing order and elements outside the current window are removed, allowing us to find the maximum in each window in O(1) time. 