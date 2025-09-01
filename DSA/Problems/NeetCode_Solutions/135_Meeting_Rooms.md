# 📝 Meeting Rooms

## **Problem Statement**

* Given an array of meeting time intervals where `intervals[i] = [starti, endi]`, determine if a person could attend all meetings.
* Example:
  * Input: intervals = [[0,30],[5,10],[15,20]]
  * Output: false (The person cannot attend all meetings, as [0,30] and [5,10] overlap)
  * Input: intervals = [[7,10],[2,4]]
  * Output: true (The person can attend all meetings, as they don't overlap)
* Constraints:
  * 0 <= intervals.length <= 10^4
  * intervals[i].length == 2
  * 0 <= starti < endi <= 10^6

---

## **Step 1: Thinking Process (Brute Force)**

* To determine if a person can attend all meetings, we need to check if any two meetings overlap.
* A naive approach would be to compare each meeting with every other meeting and check for overlap.
* This would be inefficient with O(n²) comparisons.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. For each meeting i:
   1. For each meeting j (where j != i):
      1. Check if meetings i and j overlap.
      2. If they overlap, return false.
2. Return true if no overlaps are found.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def canAttendMeetings(intervals):
    for i in range(len(intervals)):
        for j in range(i + 1, len(intervals)):
            if (intervals[i][0] >= intervals[j][0] and intervals[i][0] < intervals[j][1]) or \
               (intervals[j][0] >= intervals[i][0] and intervals[j][0] < intervals[i][1]):
                return False
    return True
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the number of meetings. We compare each meeting with every other meeting.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient for large inputs.

---

## **Step 5: Visualizing Redundant Computation**

* For intervals = [[0,30],[5,10],[15,20]]:
  * Compare [0,30] with [5,10]: 5 >= 0 and 5 < 30, so they overlap -> return false
  * We don't need to check further.

---

## **Why are we using this algorithm?**

* For optimization, we'll sort the meetings by their start time.
* After sorting, we only need to check if any meeting's start time is earlier than the previous meeting's end time.
* This reduces the time complexity from O(n²) to O(n log n).

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* First, we sort the meetings by their start time.
* Then, we iterate through the sorted meetings and check if the current meeting's start time is earlier than the previous meeting's end time.
* If it is, the meetings overlap and the person cannot attend all meetings.

### **Step 2: Flow Steps (Optimized)**

1. Sort the meetings by their start time.
2. Iterate through the sorted meetings starting from the second meeting:
   1. If the current meeting's start time is earlier than the previous meeting's end time, return false.
3. Return true if no overlaps are found.

### **Step 3: Implementation (Optimized)**

```python
def canAttendMeetings(intervals):
    if not intervals:
        return True
    
    # Sort meetings by start time
    intervals.sort(key=lambda x: x[0])
    
    for i in range(1, len(intervals)):
        # If the current meeting starts before the previous meeting ends
        if intervals[i][0] < intervals[i-1][1]:
            return False
    
    return True
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n log n) where n is the number of meetings. Sorting the meetings takes O(n log n) time, and checking for overlaps takes O(n) time.
* Space Complexity: O(1) if we ignore the space used for sorting.
* This approach is much more efficient than the brute force approach.

### **Step 5: Visualizing Computation (Optimized)**

* For intervals = [[0,30],[5,10],[15,20]]:
  * Sort by start time -> [[0,30],[5,10],[15,20]] (already sorted)
  * Process [5,10]:
    * 5 < 30, so they overlap -> return false
  * We don't need to check further.

* For intervals = [[7,10],[2,4]]:
  * Sort by start time -> [[2,4],[7,10]]
  * Process [7,10]:
    * 7 >= 4, so they don't overlap
  * All meetings checked, no overlaps found -> return true

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Compares each meeting with every other meeting |
| Optimized | O(n log n) | O(1) | Sorts meetings by start time and checks adjacent meetings for overlap |

---

## **Final Takeaways**

* The Meeting Rooms problem is a simple application of interval overlap checking.
* By sorting the meetings by their start time, we can efficiently check for overlaps in a single pass.
* This problem demonstrates how sorting can simplify a problem and reduce its time complexity.

---

## **Real-life Analogy**

* Imagine you're checking if a person can attend all scheduled meetings in a day. If you sort the meetings by their start time, you can easily check if any meeting starts before the previous one ends. This is similar to how we check for overlaps in our optimized approach.

---

## **Summary**

* We tackled the "Meeting Rooms" problem by first sorting the meetings by their start time. Then, we iterated through the sorted meetings and checked if the current meeting's start time is earlier than the previous meeting's end time. If it is, the meetings overlap and the person cannot attend all meetings. This approach has a time complexity of O(n log n) where n is the number of meetings, which is more efficient than the brute force approach of comparing each meeting with every other meeting. 