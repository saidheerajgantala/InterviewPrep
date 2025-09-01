# 📝 Find Median from Data Stream

## **Problem Statement**

* The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.
* For example, for arr = [2,3,4], the median is 3.
* For example, for arr = [2,3], the median is (2 + 3) / 2 = 2.5.
* Implement the MedianFinder class:
  * `MedianFinder()` initializes the MedianFinder object.
  * `void addNum(int num)` adds the integer num from the data stream to the data structure.
  * `double findMedian()` returns the median of all elements so far.
* Example:
  * Input:
    * ["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
    * [[], [1], [2], [], [3], []]
  * Output:
    * [null, null, null, 1.5, null, 2.0]
  * Explanation:
    * MedianFinder medianFinder = new MedianFinder();
    * medianFinder.addNum(1);    // arr = [1]
    * medianFinder.addNum(2);    // arr = [1, 2]
    * medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
    * medianFinder.addNum(3);    // arr = [1, 2, 3]
    * medianFinder.findMedian(); // return 2.0
* Constraints:
  * -10^5 <= num <= 10^5
  * There will be at least one element in the data structure before calling findMedian.
  * At most 5 * 10^4 calls will be made to addNum and findMedian.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to design a data structure that can efficiently find the median of a stream of numbers
* The most straightforward approach would be to maintain a sorted list of numbers
* When a new number is added, we insert it into the correct position to maintain the sorted order
* To find the median, we access the middle element(s) of the sorted list
* This approach is simple but not the most efficient for large data streams

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty list to store the numbers
2. For `addNum(num)`:
   1. Insert num into the list at the correct position to maintain the sorted order
3. For `findMedian()`:
   1. If the list has an odd number of elements, return the middle element
   2. If the list has an even number of elements, return the average of the two middle elements

---

## **Step 3: Brute Force Implementation (Code)**

```python
class MedianFinder:
    def __init__(self):
        self.nums = []
    
    def addNum(self, num):
        # Insert num into the correct position
        if not self.nums:
            self.nums.append(num)
        else:
            # Binary search to find the insertion position
            left, right = 0, len(self.nums) - 1
            while left <= right:
                mid = left + (right - left) // 2
                if self.nums[mid] < num:
                    left = mid + 1
                else:
                    right = mid - 1
            
            # Insert num at the correct position
            self.nums.insert(left, num)
    
    def findMedian(self):
        n = len(self.nums)
        if n % 2 == 1:
            return self.nums[n // 2]
        else:
            return (self.nums[n // 2 - 1] + self.nums[n // 2]) / 2
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity:
  * `addNum`: O(n) where n is the number of elements in the list. Although we use binary search to find the insertion position in O(log n) time, the insertion itself takes O(n) time due to the need to shift elements.
  * `findMedian`: O(1) as we directly access the middle element(s).
* Space Complexity: O(n) for storing all the numbers.
* This approach is inefficient for large data streams due to the O(n) time complexity of `addNum`.

---

## **Step 5: Visualizing Redundant Computation**

* For the example:
  * Initialize nums = []
  * addNum(1): nums = [1]
  * addNum(2): nums = [1, 2]
  * findMedian(): (1 + 2) / 2 = 1.5
  * addNum(3): nums = [1, 2, 3]
  * findMedian(): 2
* The insertion operation is inefficient, especially for large lists, as we need to shift elements to maintain the sorted order
* We can optimize this by using a more efficient data structure

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Two Heap approach.
* The property that matches this problem is the ability to efficiently find the median of a stream of numbers.
* By using two heaps (a max heap for the lower half and a min heap for the upper half), we can maintain the median in O(log n) time.
* Alternatives like using a sorted list would be less efficient for large data streams.
* Precondition: We need to efficiently add numbers and find the median.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can use two heaps to maintain the numbers:
  * A max heap for the lower half of the numbers
  * A min heap for the upper half of the numbers
* We maintain the heaps such that:
  * All numbers in the max heap are less than or equal to all numbers in the min heap
  * The sizes of the two heaps differ by at most 1
* With this setup:
  * If the total number of elements is odd, the median is the top element of the heap with more elements
  * If the total number of elements is even, the median is the average of the top elements of both heaps

### **Step 2: Flow Steps (Optimized)**

1. Initialize a max heap for the lower half and a min heap for the upper half
2. For `addNum(num)`:
   1. If the max heap is empty or num <= the top of the max heap, push num to the max heap
   2. Otherwise, push num to the min heap
   3. Balance the heaps such that their sizes differ by at most 1
3. For `findMedian()`:
   1. If the sizes of the heaps are equal, return the average of the top elements of both heaps
   2. Otherwise, return the top element of the heap with more elements

### **Step 3: Implementation (Optimized)**

```python
import heapq

class MedianFinder:
    def __init__(self):
        self.max_heap = []  # Max heap for the lower half
        self.min_heap = []  # Min heap for the upper half
    
    def addNum(self, num):
        # Python's heapq is a min heap, so we negate values for the max heap
        if not self.max_heap or -self.max_heap[0] >= num:
            heapq.heappush(self.max_heap, -num)
        else:
            heapq.heappush(self.min_heap, num)
        
        # Balance the heaps
        if len(self.max_heap) > len(self.min_heap) + 1:
            heapq.heappush(self.min_heap, -heapq.heappop(self.max_heap))
        elif len(self.min_heap) > len(self.max_heap):
            heapq.heappush(self.max_heap, -heapq.heappop(self.min_heap))
    
    def findMedian(self):
        if len(self.max_heap) > len(self.min_heap):
            return -self.max_heap[0]
        else:
            return (-self.max_heap[0] + self.min_heap[0]) / 2
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity:
  * `addNum`: O(log n) where n is the total number of elements. The heap operations (push and pop) take O(log n) time.
  * `findMedian`: O(1) as we directly access the top elements of the heaps.
* Space Complexity: O(n) for storing all the numbers in the heaps.
* This approach is much more efficient than the brute force approach for large data streams.

### **Step 5: Visualizing Computation (Optimized)**

* For the example:
  * Initialize max_heap = [], min_heap = []
  * addNum(1): max_heap = [-1], min_heap = []
  * addNum(2): max_heap = [-1], min_heap = [2]
  * findMedian(): (-(-1) + 2) / 2 = 1.5
  * addNum(3): max_heap = [-1], min_heap = [2, 3]
    * Unbalanced, so move 2 to max_heap: max_heap = [-1, -2], min_heap = [3]
  * findMedian(): (-(-1) + 3) / 2 = 2
* We're efficiently maintaining the median using two heaps.

### **Complexity Summary**

| Approach | Time (addNum, findMedian) | Space | Notes |
|---|---|---|---|
| Brute Force (Sorted List) | O(n), O(1) | O(n) | Inefficient for large data streams |
| Optimized (Algorithm: Two Heaps) | O(log n), O(1) | O(n) | Efficiently maintains the median |

---

## **Final Takeaways**

* The key to efficiently finding the median from a data stream is to use two heaps: a max heap for the lower half and a min heap for the upper half.
* By maintaining the heaps such that all numbers in the max heap are less than or equal to all numbers in the min heap, and their sizes differ by at most 1, we can efficiently find the median in O(1) time.
* This problem demonstrates how using the right data structures can lead to efficient algorithms for specific tasks.

---

## **Real-life Analogy**

* Imagine you're a teacher keeping track of the median score in a class as students submit their tests one by one. The brute force approach would be like sorting all the scores after each submission, which becomes increasingly time-consuming as more students submit their tests. The optimized approach is like dividing the scores into two groups: below-median and above-median. After each submission, you place the new score in the appropriate group and adjust the groups if necessary to maintain balance. This way, you can quickly find the median by looking at the highest score in the below-median group and/or the lowest score in the above-median group.

---

## **Summary**

* We designed a "Find Median from Data Stream" data structure by first considering a brute force approach using a sorted list, resulting in O(n) time complexity for adding numbers. We then optimized using a two-heap approach (a max heap for the lower half and a min heap for the upper half), reducing the time complexity to O(log n) for adding numbers while maintaining O(1) time complexity for finding the median. The key insight is to maintain the heaps such that all numbers in the max heap are less than or equal to all numbers in the min heap, and their sizes differ by at most 1, which allows us to efficiently find the median in O(1) time. 