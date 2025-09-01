# 📝 Top K Frequent Elements

## **Problem Statement**

* Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.
* Example:
  * Input: nums = [1,1,1,2,2,3], k = 2
  * Output: [1,2] (because 1 appears 3 times and 2 appears 2 times, which are the most frequent)
* Constraints:
  * 1 <= nums.length <= 10^5
  * -10^4 <= nums[i] <= 10^4
  * k is in the range [1, the number of unique elements in the array]
  * It is guaranteed that the answer is unique

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the k elements that appear most frequently in the array
* First, we need to count the frequency of each element
* Then, we need to sort these elements by their frequencies
* Finally, we return the top k elements from this sorted list
* This approach is straightforward but may not be the most efficient

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a hash map to count the frequency of each element in the array
2. Convert the hash map to a list of (element, frequency) pairs
3. Sort this list by frequency in descending order
4. Return the first k elements from the sorted list

---

## **Step 3: Brute Force Implementation (Code)**

```python
def topKFrequent(nums, k):
    # Count the frequency of each element
    freq_map = {}
    for num in nums:
        if num in freq_map:
            freq_map[num] += 1
        else:
            freq_map[num] = 1
    
    # Convert to list of (element, frequency) pairs and sort by frequency
    freq_list = [(num, freq) for num, freq in freq_map.items()]
    freq_list.sort(key=lambda x: x[1], reverse=True)
    
    # Return the top k elements
    result = [num for num, freq in freq_list[:k]]
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n log n), where n is the length of the array.
  * O(n) to count frequencies
  * O(m log m) to sort the frequency list, where m is the number of unique elements (worst case m = n)
* Space Complexity: O(n) for the hash map and frequency list.
* This approach is inefficient because we're sorting the entire frequency list when we only need the top k elements.

---

## **Step 5: Visualizing Redundant Computation**

* For input [1,1,1,2,2,3] and k = 2:
  * Count frequencies: {1: 3, 2: 2, 3: 1}
  * Sort all frequencies: [(1, 3), (2, 2), (3, 1)]
  * Take top 2: [1, 2]
* We're sorting the entire frequency list, but we only need the top k elements. This is unnecessary work, especially if k is much smaller than the number of unique elements.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Heap (Priority Queue) algorithm.
* The property that matches this problem is finding the k largest/smallest elements efficiently.
* A min-heap of size k allows us to track the k largest elements without sorting the entire list.
* Alternatives like full sorting would work but would be O(n log n), which is less efficient when k << n.
* Precondition: We can build and maintain a heap of size k efficiently.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We still need to count the frequency of each element
* Instead of sorting all elements by frequency, we can use a min-heap of size k
* We'll iterate through the frequency map and maintain the k most frequent elements in the heap
* If the heap size exceeds k, we remove the element with the lowest frequency
* This way, we only need to "sort" k elements instead of all elements

### **Step 2: Flow Steps (Optimized)**

1. Create a hash map to count the frequency of each element
2. Create a min-heap to store the k most frequent elements
3. Iterate through the frequency map:
   1. Add the current (element, frequency) pair to the heap
   2. If the heap size exceeds k, remove the element with the lowest frequency
4. Extract all elements from the heap to get the result

### **Step 3: Implementation (Optimized)**

```python
import heapq

def topKFrequent(nums, k):
    # Count the frequency of each element
    freq_map = {}
    for num in nums:
        if num in freq_map:
            freq_map[num] += 1
        else:
            freq_map[num] = 1
    
    # Use a min-heap to keep track of the k most frequent elements
    heap = []
    for num, freq in freq_map.items():
        # Push the (frequency, element) pair to the heap
        # We use negative frequency to simulate a max-heap
        heapq.heappush(heap, (freq, num))
        if len(heap) > k:
            # Remove the element with the lowest frequency
            heapq.heappop(heap)
    
    # Extract the elements from the heap
    result = [num for freq, num in heap]
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n + m log k), where n is the length of the array and m is the number of unique elements.
  * O(n) to count frequencies
  * O(m log k) to process m elements with heap operations of O(log k)
* Space Complexity: O(n) for the hash map and O(k) for the heap.
* This is more efficient than the brute force approach, especially when k is much smaller than the number of unique elements.

### **Step 5: Visualizing Computation (Optimized)**

* For input [1,1,1,2,2,3] and k = 2:
  * Count frequencies: {1: 3, 2: 2, 3: 1}
  * Process frequencies with min-heap of size k=2:
    * Add (1, 3) to heap: [(1, 3)]
    * Add (2, 2) to heap: [(2, 2), (1, 3)]
    * Add (3, 1) to heap: [(3, 1), (1, 3)], then pop smallest: [(1, 3), (2, 2)]
  * Result: [1, 2]
* We only need to "sort" k elements at any time, not the entire frequency list.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Sorting) | O(n log n) | O(n) | Sorts all elements by frequency |
| Optimized (Algorithm: Heap) | O(n + m log k) | O(n + k) | Uses a min-heap of size k |

---

## **Final Takeaways**

* Using a heap of size k allows us to efficiently find the top k elements without sorting the entire list.
* This is a common pattern for "top k" problems - count frequencies, then use a heap to track the k largest/smallest.
* The key insight is recognizing that we only need to maintain k elements in sorted order, not all elements.

---

## **Real-life Analogy**

* Imagine you're a music streaming service trying to find the top 10 most played songs from millions of plays. The brute force approach would be like sorting your entire database by play count. The optimized approach is like maintaining a "top 10 chart" that updates whenever a song gets enough plays to enter the chart - you only need to compare new songs with the current top 10, not sort the entire catalog.

---

## **Summary**

* We solved the "Top K Frequent Elements" problem by first using a brute force approach that sorts all elements by frequency, resulting in O(n log n) time complexity. We then optimized using a min-heap of size k to track only the k most frequent elements, reducing the time complexity to O(n + m log k). This demonstrates the power of using appropriate data structures (heaps) for "top k" problems, allowing us to avoid unnecessary sorting of elements we don't need. 