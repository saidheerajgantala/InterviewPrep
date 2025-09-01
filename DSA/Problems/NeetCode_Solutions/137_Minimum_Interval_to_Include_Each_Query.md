# 📝 Minimum Interval to Include Each Query

## **Problem Statement**

* You are given a 2D integer array `intervals`, where `intervals[i] = [lefti, righti]` describes the ith interval starting at `lefti` and ending at `righti` (inclusive).
* The size of an interval is defined as the number of integers it contains, or more formally `righti - lefti + 1`.
* You are also given an integer array `queries`. The answer to the jth query is the size of the smallest interval i such that `lefti <= queries[j] <= righti`. If no such interval exists, the answer is -1.
* Return an array containing the answers to the queries.
* Example:
  * Input: intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]
  * Output: [3,3,1,4] (The smallest interval containing query 2 is [1,4] with size 4-1+1=4, but [2,4] is smaller with size 3. The smallest interval containing query 3 is [3,6] with size 4. The smallest interval containing query 4 is [4,4] with size 1. The smallest interval containing query 5 is [3,6] with size 4.)
  * Input: intervals = [[2,3],[2,5],[1,8],[20,25]], queries = [2,19,5,22]
  * Output: [2,-1,4,6] (The smallest interval containing query 2 is [2,3] with size 2. No interval contains query 19. The smallest interval containing query 5 is [2,5] with size 4. The smallest interval containing query 22 is [20,25] with size 6.)
* Constraints:
  * 1 <= intervals.length <= 10^5
  * 1 <= queries.length <= 10^5
  * intervals[i].length == 2
  * 1 <= lefti <= righti <= 10^7
  * 1 <= queries[j] <= 10^7

---

## **Step 1: Thinking Process (Brute Force)**

* For each query, we need to find the smallest interval that contains it.
* A naive approach would be to check each interval for each query and keep track of the smallest valid interval.
* This would be inefficient with O(n * q) comparisons, where n is the number of intervals and q is the number of queries.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an array `result` of length `queries.length` with all values set to -1.
2. For each query:
   1. Initialize `min_size` to infinity.
   2. For each interval:
      1. If the interval contains the query (left <= query <= right):
         1. Calculate the size of the interval (right - left + 1).
         2. If the size is smaller than `min_size`, update `min_size` and the corresponding entry in `result`.
3. Return the `result` array.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def minInterval(intervals, queries):
    result = [-1] * len(queries)
    
    for i, query in enumerate(queries):
        min_size = float('inf')
        
        for left, right in intervals:
            if left <= query <= right:
                size = right - left + 1
                if size < min_size:
                    min_size = size
                    result[i] = size
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n * q) where n is the number of intervals and q is the number of queries. For each query, we check all intervals.
* Space Complexity: O(q) for storing the result array.
* This approach is inefficient for large inputs.

---

## **Step 5: Visualizing Redundant Computation**

* For intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]:
  * For query 2:
    * Check [1,4]: 1 <= 2 <= 4, size = 4
    * Check [2,4]: 2 <= 2 <= 4, size = 3 (smaller, update)
    * Check [3,6]: 3 <= 2 <= 6, false
    * Check [4,4]: 4 <= 2 <= 4, false
    * Result for query 2: 3
  * For query 3:
    * Check [1,4]: 1 <= 3 <= 4, size = 4
    * Check [2,4]: 2 <= 3 <= 4, size = 3 (smaller, update)
    * Check [3,6]: 3 <= 3 <= 6, size = 4 (not smaller)
    * Check [4,4]: 4 <= 3 <= 4, false
    * Result for query 3: 3
  * ... (continue for the rest of the queries)

---

## **Why are we using this algorithm?**

* For optimization, we'll use a combination of sorting and a min-heap.
* The key insight is to process the intervals in order of their size and only consider intervals that contain the query.
* We can use a min-heap to efficiently find the smallest interval that contains each query.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* First, we sort the queries and remember their original indices.
* Then, we sort the intervals by their left endpoint.
* For each query, we consider all intervals that start before or at the query.
* Among these intervals, we use a min-heap to keep track of the intervals that contain the query, sorted by their size.
* The smallest interval in the heap that contains the query is our answer.

### **Step 2: Flow Steps (Optimized)**

1. Sort the queries and remember their original indices.
2. Sort the intervals by their left endpoint.
3. Initialize a min-heap to keep track of intervals sorted by their size.
4. Initialize an array `result` of length `queries.length`.
5. Initialize a pointer `i` to 0 (for iterating through the sorted intervals).
6. For each sorted query:
   1. While `i` is within bounds and the left endpoint of the current interval is less than or equal to the query:
      1. Add the interval to the heap as a tuple (size, right endpoint).
      2. Increment `i`.
   2. While the heap is not empty and the right endpoint of the smallest interval is less than the query:
      1. Remove the interval from the heap (it doesn't contain the query).
   3. If the heap is not empty, the smallest interval in the heap contains the query. Set the corresponding entry in `result` to the size of this interval.
   4. Otherwise, set the corresponding entry in `result` to -1.
7. Return the `result` array (reordered according to the original query indices).

### **Step 3: Implementation (Optimized)**

```python
import heapq

def minInterval(intervals, queries):
    # Sort intervals by left endpoint
    intervals.sort()
    
    # Sort queries and remember original indices
    sorted_queries = sorted((q, i) for i, q in enumerate(queries))
    
    result = [-1] * len(queries)
    heap = []  # Min-heap to keep track of intervals sorted by size
    i = 0  # Pointer for iterating through sorted intervals
    
    for query, original_idx in sorted_queries:
        # Add all intervals that start before or at the query
        while i < len(intervals) and intervals[i][0] <= query:
            left, right = intervals[i]
            if right >= query:  # Only add intervals that might contain the query
                heapq.heappush(heap, (right - left + 1, right))
            i += 1
        
        # Remove intervals that don't contain the query
        while heap and heap[0][1] < query:
            heapq.heappop(heap)
        
        # If there's a valid interval, update the result
        if heap:
            result[original_idx] = heap[0][0]
    
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O((n + q) * log(n + q)) where n is the number of intervals and q is the number of queries. Sorting the intervals and queries takes O(n log n) and O(q log q) time, respectively. Each interval and query is processed once, and each heap operation takes O(log n) time.
* Space Complexity: O(n + q) for storing the sorted queries and the heap.
* This approach is much more efficient than the brute force approach.

### **Step 5: Visualizing Computation (Optimized)**

* For intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]:
  * Sort intervals by left endpoint -> [[1,4],[2,4],[3,6],[4,4]] (already sorted)
  * Sort queries and remember original indices -> [(2,0),(3,1),(4,2),(5,3)]
  * Process query 2:
    * Add intervals starting before or at 2: [1,4] (size 4), [2,4] (size 3)
    * Heap: [(3, 4), (4, 4)]
    * Result for query 2: 3
  * Process query 3:
    * Add intervals starting before or at 3: [3,6] (size 4)
    * Heap: [(3, 4), (4, 4), (4, 6)]
    * Result for query 3: 3
  * Process query 4:
    * Add intervals starting before or at 4: [4,4] (size 1)
    * Heap: [(1, 4), (3, 4), (4, 4), (4, 6)]
    * Result for query 4: 1
  * Process query 5:
    * No new intervals to add
    * Remove intervals that don't contain 5: [1,4], [2,4], [4,4]
    * Heap: [(4, 6)]
    * Result for query 5: 4
  * Final result: [3,3,1,4]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n * q) | O(q) | Checks each interval for each query |
| Optimized | O((n + q) * log(n + q)) | O(n + q) | Uses sorting and a min-heap to efficiently find the smallest interval for each query |

---

## **Final Takeaways**

* The Minimum Interval to Include Each Query problem is a good example of how sorting and using a min-heap can optimize a problem.
* By processing the queries in order and only considering relevant intervals, we avoid redundant computations.
* This problem demonstrates how data structures like heaps can be used to efficiently solve complex problems.

---

## **Real-life Analogy**

* Imagine you're planning a trip and want to find the shortest time window that includes specific events. Instead of checking all possible time windows for each event, you would first sort the events by their start time and then use a priority queue to keep track of the shortest time windows that include each event. This is similar to how we use sorting and a min-heap in our optimized approach.

---

## **Summary**

* We tackled the "Minimum Interval to Include Each Query" problem by first sorting the queries and intervals. Then, for each query, we considered all intervals that start before or at the query and used a min-heap to keep track of the intervals that contain the query, sorted by their size. The smallest interval in the heap that contains the query is our answer. This approach has a time complexity of O((n + q) * log(n + q)) where n is the number of intervals and q is the number of queries, which is much more efficient than the brute force approach of checking each interval for each query. 