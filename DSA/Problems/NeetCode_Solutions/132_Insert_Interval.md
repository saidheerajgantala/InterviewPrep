# 📝 Insert Interval

## **Problem Statement**

* You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the ith interval and `intervals` is sorted in ascending order by `starti`.
* You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.
* Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).
* Return `intervals` after the insertion.
* Example:
  * Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
  * Output: [[1,5],[6,9]] (The new interval [2,5] overlaps with [1,3], so they are merged)
  * Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
  * Output: [[1,2],[3,10],[12,16]] (The new interval [4,8] overlaps with [3,5], [6,7], [8,10], so they are merged)
* Constraints:
  * 0 <= intervals.length <= 10^4
  * intervals[i].length == 2
  * 0 <= starti <= endi <= 10^5
  * intervals is sorted by starti in ascending order.
  * newInterval.length == 2
  * 0 <= start <= end <= 10^5

---

## **Step 1: Thinking Process (Brute Force)**

* A naive approach would be to insert the new interval into the array, then sort the array, and finally merge overlapping intervals.
* This would work, but it's not the most efficient approach.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. Insert the new interval into the array.
2. Sort the array by the start time of each interval.
3. Merge overlapping intervals:
   1. Initialize a result array with the first interval.
   2. For each interval in the sorted array:
      1. If the current interval overlaps with the last interval in the result, merge them.
      2. Otherwise, add the current interval to the result.
4. Return the result array.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def insert(intervals, newInterval):
    # Insert the new interval
    intervals.append(newInterval)
    
    # Sort the intervals by start time
    intervals.sort(key=lambda x: x[0])
    
    # Merge overlapping intervals
    result = []
    for interval in intervals:
        # If the result is empty or there's no overlap, add the interval
        if not result or result[-1][1] < interval[0]:
            result.append(interval)
        # Otherwise, merge the intervals
        else:
            result[-1][1] = max(result[-1][1], interval[1])
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n log n) where n is the number of intervals. Sorting the intervals takes O(n log n) time.
* Space Complexity: O(n) for storing the result array.
* This approach is reasonably efficient but can be optimized further.

---

## **Step 5: Visualizing Redundant Computation**

* For intervals = [[1,3],[6,9]], newInterval = [2,5]:
  * Insert [2,5] -> [[1,3],[6,9],[2,5]]
  * Sort -> [[1,3],[2,5],[6,9]]
  * Merge:
    * Add [1,3] to result -> [[1,3]]
    * [2,5] overlaps with [1,3], merge -> [[1,5]]
    * [6,9] doesn't overlap, add -> [[1,5],[6,9]]
  * Final result: [[1,5],[6,9]]

---

## **Why are we using this algorithm?**

* For optimization, we'll take advantage of the fact that the intervals are already sorted.
* We can avoid the sorting step by directly inserting the new interval in the correct position and merging overlapping intervals in a single pass.
* This will reduce the time complexity from O(n log n) to O(n).

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Since the intervals are already sorted, we can iterate through them and handle three cases:
  1. Intervals that come before the new interval (no overlap).
  2. Intervals that overlap with the new interval.
  3. Intervals that come after the new interval (no overlap).
* We'll add intervals from case 1 directly to the result.
* We'll merge intervals from case 2 with the new interval.
* We'll add intervals from case 3 directly to the result.

### **Step 2: Flow Steps (Optimized)**

1. Initialize a result array.
2. Iterate through the intervals:
   1. If the current interval ends before the new interval starts, add it to the result (case 1).
   2. If the current interval starts after the new interval ends, we've processed all overlapping intervals. Add the new interval to the result and add all remaining intervals (case 3).
   3. If there's an overlap, update the new interval to the merged interval (case 2).
3. If we've gone through all intervals and haven't added the new interval yet, add it to the result.
4. Return the result array.

### **Step 3: Implementation (Optimized)**

```python
def insert(intervals, newInterval):
    result = []
    i = 0
    n = len(intervals)
    
    # Add all intervals that come before the new interval
    while i < n and intervals[i][1] < newInterval[0]:
        result.append(intervals[i])
        i += 1
    
    # Merge overlapping intervals
    while i < n and intervals[i][0] <= newInterval[1]:
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
        i += 1
    
    # Add the merged interval
    result.append(newInterval)
    
    # Add all intervals that come after the new interval
    while i < n:
        result.append(intervals[i])
        i += 1
    
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the number of intervals. We only need to iterate through the intervals once.
* Space Complexity: O(n) for storing the result array.
* This approach is more efficient than the brute force approach.

### **Step 5: Visualizing Computation (Optimized)**

* For intervals = [[1,3],[6,9]], newInterval = [2,5]:
  * Add intervals that come before [2,5]:
    * [1,3] overlaps with [2,5], so we don't add it yet.
  * Merge overlapping intervals:
    * Merge [1,3] with [2,5] -> [1,5]
  * Add the merged interval:
    * Add [1,5] to result -> [[1,5]]
  * Add intervals that come after [1,5]:
    * Add [6,9] to result -> [[1,5],[6,9]]
  * Final result: [[1,5],[6,9]]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n log n) | O(n) | Sorts the intervals after insertion |
| Optimized | O(n) | O(n) | Takes advantage of the fact that intervals are already sorted |

---

## **Final Takeaways**

* The Insert Interval problem is a good example of how understanding the properties of the input can lead to a more efficient algorithm.
* By taking advantage of the fact that the intervals are already sorted, we can avoid the sorting step and solve the problem in linear time.
* This problem demonstrates how a careful analysis of the problem can lead to a more efficient algorithm.

---

## **Real-life Analogy**

* Imagine you're scheduling a new meeting in your already sorted calendar. You would first skip all meetings that end before your new meeting starts. Then, you would merge your new meeting with any overlapping meetings. Finally, you would add all meetings that start after your new meeting ends. This is similar to how we handle the three cases in our optimized approach.

---

## **Summary**

* We tackled the "Insert Interval" problem by taking advantage of the fact that the intervals are already sorted. We iterated through the intervals and handled three cases: intervals that come before the new interval, intervals that overlap with the new interval, and intervals that come after the new interval. This approach has a time complexity of O(n) where n is the number of intervals, which is more efficient than the brute force approach of sorting the intervals after insertion. 