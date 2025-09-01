# 📝 Merge Intervals

## **Problem Statement**

* Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.
* Example:
  * Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
  * Output: [[1,6],[8,10],[15,18]] (Since intervals [1,3] and [2,6] overlap, merge them into [1,6])
  * Input: intervals = [[1,4],[4,5]]
  * Output: [[1,5]] (Intervals [1,4] and [4,5] are considered overlapping)
* Constraints:
  * 1 <= intervals.length <= 10^4
  * intervals[i].length == 2
  * 0 <= starti <= endi <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* To merge overlapping intervals, we first need to identify which intervals overlap.
* A naive approach would be to compare each interval with every other interval and merge them if they overlap.
* This would be inefficient with O(n²) comparisons.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a result array with the first interval.
2. For each interval in the input:
   1. Check if it overlaps with any interval in the result array.
   2. If it overlaps, merge it with the overlapping interval.
   3. If it doesn't overlap with any interval, add it to the result array.
3. Return the result array.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def merge(intervals):
    if not intervals:
        return []
    
    result = [intervals[0]]
    
    for i in range(1, len(intervals)):
        current = intervals[i]
        merged = False
        
        for j in range(len(result)):
            if (current[0] <= result[j][1] and current[1] >= result[j][0]) or \
               (result[j][0] <= current[1] and result[j][1] >= current[0]):
                # Merge overlapping intervals
                result[j][0] = min(result[j][0], current[0])
                result[j][1] = max(result[j][1], current[1])
                merged = True
                break
        
        if not merged:
            result.append(current)
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the number of intervals. For each interval, we check if it overlaps with any interval in the result array.
* Space Complexity: O(n) for storing the result array.
* This approach is inefficient for large inputs.

---

## **Step 5: Visualizing Redundant Computation**

* For intervals = [[1,3],[2,6],[8,10],[15,18]]:
  * Initialize result = [[1,3]]
  * Process [2,6]:
    * Check if it overlaps with [1,3] -> Yes, merge -> result = [[1,6]]
  * Process [8,10]:
    * Check if it overlaps with [1,6] -> No, add -> result = [[1,6],[8,10]]
  * Process [15,18]:
    * Check if it overlaps with [1,6] -> No
    * Check if it overlaps with [8,10] -> No, add -> result = [[1,6],[8,10],[15,18]]
  * Final result: [[1,6],[8,10],[15,18]]

---

## **Why are we using this algorithm?**

* For optimization, we'll sort the intervals by their start time.
* This way, we only need to compare each interval with the last interval in the result array.
* If the current interval's start time is less than or equal to the last interval's end time, they overlap.
* This reduces the time complexity from O(n²) to O(n log n).

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* First, we sort the intervals by their start time.
* Then, we iterate through the sorted intervals and merge overlapping intervals.
* Since the intervals are sorted, we only need to compare each interval with the last interval in the result array.

### **Step 2: Flow Steps (Optimized)**

1. Sort the intervals by their start time.
2. Initialize a result array with the first interval.
3. For each interval in the sorted array:
   1. If the current interval's start time is less than or equal to the last interval's end time in the result array, they overlap. Merge them by updating the end time of the last interval in the result array.
   2. Otherwise, add the current interval to the result array.
4. Return the result array.

### **Step 3: Implementation (Optimized)**

```python
def merge(intervals):
    if not intervals:
        return []
    
    # Sort intervals by start time
    intervals.sort(key=lambda x: x[0])
    
    result = [intervals[0]]
    
    for i in range(1, len(intervals)):
        current = intervals[i]
        last = result[-1]
        
        # If the current interval overlaps with the last interval in the result
        if current[0] <= last[1]:
            # Merge them
            last[1] = max(last[1], current[1])
        else:
            # Add the current interval to the result
            result.append(current)
    
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n log n) where n is the number of intervals. Sorting the intervals takes O(n log n) time, and merging takes O(n) time.
* Space Complexity: O(n) for storing the result array.
* This approach is much more efficient than the brute force approach.

### **Step 5: Visualizing Computation (Optimized)**

* For intervals = [[1,3],[2,6],[8,10],[15,18]]:
  * Sort -> [[1,3],[2,6],[8,10],[15,18]] (already sorted)
  * Initialize result = [[1,3]]
  * Process [2,6]:
    * 2 <= 3, so they overlap, merge -> result = [[1,6]]
  * Process [8,10]:
    * 8 > 6, so they don't overlap, add -> result = [[1,6],[8,10]]
  * Process [15,18]:
    * 15 > 10, so they don't overlap, add -> result = [[1,6],[8,10],[15,18]]
  * Final result: [[1,6],[8,10],[15,18]]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(n) | Compares each interval with every other interval |
| Optimized | O(n log n) | O(n) | Sorts the intervals and compares each interval with the last merged interval |

---

## **Final Takeaways**

* The Merge Intervals problem is a classic example of how sorting can simplify a problem.
* By sorting the intervals by their start time, we ensure that overlapping intervals are adjacent in the sorted array.
* This allows us to merge intervals in a single pass through the sorted array.

---

## **Real-life Analogy**

* Imagine you're a scheduler trying to merge overlapping meetings. If you first sort the meetings by their start time, you can easily identify overlapping meetings by checking if the next meeting starts before the current meeting ends. This is similar to how we merge intervals in our optimized approach.

---

## **Summary**

* We tackled the "Merge Intervals" problem by first sorting the intervals by their start time. Then, we iterated through the sorted intervals and merged overlapping intervals by comparing each interval with the last interval in the result array. This approach has a time complexity of O(n log n) where n is the number of intervals, which is more efficient than the brute force approach of comparing each interval with every other interval. 