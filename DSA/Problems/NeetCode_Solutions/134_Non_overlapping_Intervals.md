# 📝 Non-overlapping Intervals

## **Problem Statement**

* Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.
* Example:
  * Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
  * Output: 1 (Remove [1,3] and the rest of the intervals are non-overlapping)
  * Input: intervals = [[1,2],[1,2],[1,2]]
  * Output: 2 (You need to remove two [1,2] intervals to make the rest of the intervals non-overlapping)
  * Input: intervals = [[1,2],[2,3]]
  * Output: 0 (You don't need to remove any of the intervals since they're already non-overlapping)
* Constraints:
  * 1 <= intervals.length <= 10^5
  * intervals[i].length == 2
  * -5 * 10^4 <= starti < endi <= 5 * 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* To make intervals non-overlapping, we need to remove some intervals.
* A naive approach would be to try all possible combinations of intervals and find the maximum number of non-overlapping intervals.
* This would be extremely inefficient with 2^n combinations for n intervals.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. Generate all possible subsets of intervals.
2. For each subset, check if the intervals are non-overlapping.
3. Among the valid subsets, find the one with the maximum number of intervals.
4. Return the total number of intervals minus the maximum number of non-overlapping intervals.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def eraseOverlapIntervals(intervals):
    def is_non_overlapping(subset):
        subset.sort(key=lambda x: x[0])
        for i in range(1, len(subset)):
            if subset[i][0] < subset[i-1][1]:
                return False
        return True
    
    def generate_subsets(index, current_subset):
        if index == len(intervals):
            if is_non_overlapping(current_subset):
                return len(current_subset)
            return 0
        
        # Include the current interval
        include = generate_subsets(index + 1, current_subset + [intervals[index]])
        
        # Exclude the current interval
        exclude = generate_subsets(index + 1, current_subset)
        
        return max(include, exclude)
    
    max_non_overlapping = generate_subsets(0, [])
    return len(intervals) - max_non_overlapping
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n * n log n) where n is the number of intervals. We generate 2^n subsets, and for each subset, we spend O(n log n) time checking if it's non-overlapping.
* Space Complexity: O(n) for storing the current subset and the recursion stack.
* This approach is extremely inefficient and will time out for large inputs.

---

## **Step 5: Visualizing Redundant Computation**

* For intervals = [[1,2],[2,3],[3,4],[1,3]]:
  * We would check all 2^4 = 16 possible subsets.
  * Many of these subsets would be invalid because they contain overlapping intervals.
  * The maximum number of non-overlapping intervals is 3 (by removing [1,3]).

---

## **Why are we using this algorithm?**

* For optimization, we'll use a greedy approach.
* The key insight is to sort the intervals by their end time and greedily select intervals that don't overlap.
* This approach works because we want to minimize the number of intervals to remove, which is equivalent to maximizing the number of intervals to keep.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* First, we sort the intervals by their end time.
* Then, we greedily select intervals that don't overlap with the previously selected interval.
* The number of intervals to remove is the total number of intervals minus the number of selected intervals.

### **Step 2: Flow Steps (Optimized)**

1. Sort the intervals by their end time.
2. Initialize variables: count = 1 (number of selected intervals), end = intervals[0][1] (end time of the last selected interval).
3. Iterate through the sorted intervals starting from the second interval:
   1. If the current interval's start time is greater than or equal to the end time of the last selected interval, they don't overlap. Select the current interval, increment count, and update end.
4. Return the total number of intervals minus count.

### **Step 3: Implementation (Optimized)**

```python
def eraseOverlapIntervals(intervals):
    if not intervals:
        return 0
    
    # Sort intervals by end time
    intervals.sort(key=lambda x: x[1])
    
    count = 1  # Number of non-overlapping intervals
    end = intervals[0][1]  # End time of the last selected interval
    
    for i in range(1, len(intervals)):
        # If the current interval doesn't overlap with the last selected interval
        if intervals[i][0] >= end:
            count += 1
            end = intervals[i][1]
    
    return len(intervals) - count
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n log n) where n is the number of intervals. Sorting the intervals takes O(n log n) time, and the greedy selection takes O(n) time.
* Space Complexity: O(1) if we ignore the space used for sorting.
* This approach is much more efficient than the brute force approach.

### **Step 5: Visualizing Computation (Optimized)**

* For intervals = [[1,2],[2,3],[3,4],[1,3]]:
  * Sort by end time -> [[1,2],[2,3],[1,3],[3,4]]
  * Initialize count = 1, end = 2
  * Process [2,3]:
    * 2 >= 2, so they don't overlap, select it -> count = 2, end = 3
  * Process [1,3]:
    * 1 < 3, so they overlap, skip it
  * Process [3,4]:
    * 3 >= 3, so they don't overlap, select it -> count = 3, end = 4
  * Return 4 - 3 = 1 (number of intervals to remove)

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(2^n * n log n) | O(n) | Tries all possible subsets of intervals |
| Optimized (Algorithm: Greedy) | O(n log n) | O(1) | Sorts intervals by end time and greedily selects non-overlapping intervals |

---

## **Final Takeaways**

* The Non-overlapping Intervals problem is a classic example of the activity selection problem.
* The greedy approach of sorting intervals by their end time and selecting non-overlapping intervals is optimal.
* This problem demonstrates how a greedy strategy can be much more efficient than exhaustive search for certain problems.

---

## **Real-life Analogy**

* Imagine you're scheduling meetings in a conference room. To maximize the number of meetings, you would prioritize meetings that end earlier, as this leaves more time for other meetings. This is similar to how we sort intervals by their end time in our greedy approach.

---

## **Summary**

* We tackled the "Non-overlapping Intervals" problem by first sorting the intervals by their end time. Then, we greedily selected intervals that don't overlap with the previously selected interval. The number of intervals to remove is the total number of intervals minus the number of selected intervals. This greedy approach has a time complexity of O(n log n) where n is the number of intervals, which is much more efficient than the brute force approach of trying all possible subsets of intervals. 