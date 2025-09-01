# 📝 Kth Largest Element in an Array

## **Problem Statement**

* Given an integer array `nums` and an integer `k`, return the `k`th largest element in the array.
* Note that it is the `k`th largest element in the sorted order, not the `k`th distinct element.
* Example 1:
  * Input: nums = [3,2,1,5,6,4], k = 2
  * Output: 5
  * Explanation: The 2nd largest element in the array is 5.
* Example 2:
  * Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
  * Output: 4
  * Explanation: The 4th largest element in the array is 4.
* Constraints:
  * 1 <= k <= nums.length <= 10^5
  * -10^4 <= nums[i] <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the kth largest element in the array
* One straightforward approach would be to sort the array in descending order and return the element at index k-1
* This approach is simple and works for small arrays

---

## **Step 2: Flow Steps (Brute Force)**

1. Sort the array in descending order
2. Return the element at index k-1

---

## **Step 3: Brute Force Implementation (Code)**

```python
def findKthLargest(nums, k):
    nums.sort(reverse=True)
    return nums[k-1]
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n log n) where n is the length of the array. This is due to the sorting operation.
* Space Complexity: O(1) if we sort the array in-place, or O(n) if the sorting algorithm creates a new array.
* This approach is simple but might not be the most efficient for large arrays or when k is small compared to n.

---

## **Step 5: Visualizing Redundant Computation**

* For the example nums = [3,2,1,5,6,4], k = 2:
  * Sort the array in descending order: [6,5,4,3,2,1]
  * Return the element at index k-1 = 1: 5
* For the example nums = [3,2,3,1,2,4,5,5,6], k = 4:
  * Sort the array in descending order: [6,5,5,4,3,3,2,2,1]
  * Return the element at index k-1 = 3: 4
* We're sorting the entire array, which is unnecessary if we only need the kth largest element

---

## **Why are we using this algorithm?**

* For optimization, we'll consider two approaches: using a min-heap and using QuickSelect.
* The property that matches this problem is that we only need to find the kth largest element, not sort the entire array.
* By using a min-heap of size k, we can efficiently keep track of the k largest elements.
* Alternatively, QuickSelect can find the kth largest element in expected O(n) time.
* Alternatives like the brute force approach would be less efficient for large arrays or when k is small compared to n.
* Precondition: We need to be able to compare elements in the array.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Min-Heap)**

* We can use a min-heap to keep track of the k largest elements
* We'll iterate through the array and add each element to the heap
* If the heap size exceeds k, we'll remove the smallest element
* After processing all elements, the smallest element in the heap will be the kth largest element in the array

### **Step 2: Flow Steps (Optimized - Min-Heap)**

1. Initialize a min-heap
2. Iterate through the array:
   1. Add the current element to the heap
   2. If the heap size exceeds k, remove the smallest element
3. Return the smallest element in the heap (the root)

### **Step 3: Implementation (Optimized - Min-Heap)**

```python
import heapq

def findKthLargest(nums, k):
    heap = []
    for num in nums:
        heapq.heappush(heap, num)
        if len(heap) > k:
            heapq.heappop(heap)
    return heap[0]
```

### **Alternative Approach: QuickSelect**

Another efficient approach is to use the QuickSelect algorithm, which is based on the QuickSort algorithm but only explores one partition:

```python
def findKthLargest(nums, k):
    def quickselect(nums, k):
        pivot = nums[len(nums) // 2]
        left = [x for x in nums if x > pivot]
        mid = [x for x in nums if x == pivot]
        right = [x for x in nums if x < pivot]
        
        if k <= len(left):
            return quickselect(left, k)
        elif k <= len(left) + len(mid):
            return pivot
        else:
            return quickselect(right, k - len(left) - len(mid))
    
    return quickselect(nums, k)
```

### **Step 4: Complexity Analysis (Optimized - Min-Heap)**

* Time Complexity: O(n log k) where n is the length of the array and k is the given parameter. We perform n heap operations, each taking O(log k) time.
* Space Complexity: O(k) for storing the heap.
* This approach is more efficient than the brute force approach when k is small compared to n.

### **Complexity Analysis (QuickSelect)**

* Time Complexity: 
  * Average case: O(n) where n is the length of the array.
  * Worst case: O(n²) if the pivot is always the smallest or largest element.
* Space Complexity: O(n) for the recursive calls and the partitioning.
* This approach is very efficient on average but can be slow in the worst case.

### **Step 5: Visualizing Computation (Optimized - Min-Heap)**

* For the example nums = [3,2,1,5,6,4], k = 2:
  * Initialize an empty min-heap
  * Process 3: heap = [3]
  * Process 2: heap = [2,3]
  * Process 1: heap = [1,3,2], remove the smallest element, heap = [2,3]
  * Process 5: heap = [2,3,5], remove the smallest element, heap = [3,5]
  * Process 6: heap = [3,5,6], remove the smallest element, heap = [5,6]
  * Process 4: heap = [4,6,5], remove the smallest element, heap = [5,6]
  * Return the smallest element in the heap: 5
* For the example nums = [3,2,3,1,2,4,5,5,6], k = 4:
  * Initialize an empty min-heap
  * Process elements and maintain a heap of size k
  * Final heap = [4,5,5,6]
  * Return the smallest element in the heap: 4
* The min-heap approach efficiently keeps track of the k largest elements

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Sort) | O(n log n) | O(1) or O(n) | Simple but inefficient for large arrays |
| Optimized (Min-Heap) | O(n log k) | O(k) | Efficient when k is small compared to n |
| QuickSelect | O(n) average, O(n²) worst | O(n) | Very efficient on average |

---

## **Final Takeaways**

* The key to efficiently finding the kth largest element in an array is to avoid sorting the entire array.
* Using a min-heap of size k allows us to efficiently keep track of the k largest elements.
* QuickSelect is another efficient algorithm that can find the kth largest element in expected O(n) time.
* The choice between these approaches depends on the specific requirements and constraints of the problem.

---

## **Real-life Analogy**

* Imagine you're a teacher trying to find the student with the kth highest score in a class. The brute force approach would be like sorting all the scores and then picking the kth one. The min-heap approach would be like maintaining a list of the k highest scores you've seen so far, and updating it as you go through each score. The QuickSelect approach would be like dividing the class into groups based on a pivot score, and then focusing only on the group that contains the kth highest score.

---

## **Summary**

* We solved the "Kth Largest Element in an Array" problem by first considering a brute force approach that sorts the array, resulting in O(n log n) time complexity. We then optimized using a min-heap to keep track of the k largest elements, reducing the time complexity to O(n log k). We also explored the QuickSelect algorithm, which has an expected time complexity of O(n). The key insight is to avoid sorting the entire array and focus only on finding the kth largest element. 