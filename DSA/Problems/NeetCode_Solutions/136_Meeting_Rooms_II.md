# 📝 Meeting Rooms II

## **Problem Statement**

* Given an array of meeting time intervals where `intervals[i] = [starti, endi]`, find the minimum number of conference rooms required.
* Example:
  * Input: intervals = [[0,30],[5,10],[15,20]]
  * Output: 2 (We need two rooms: one for [0,30] and another for [5,10] and [15,20])
  * Input: intervals = [[7,10],[2,4]]
  * Output: 1 (We only need one room as the meetings don't overlap)
* Constraints:
  * 1 <= intervals.length <= 10^4
  * 0 <= starti < endi <= 10^6

---

## **Step 1: Thinking Process (Brute Force)**

* To find the minimum number of conference rooms required, we need to track the number of overlapping meetings at any given time.
* A naive approach would be to simulate the meetings minute by minute and count the number of active meetings.
* This would be inefficient if the meeting times span a large range.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. Find the earliest start time and the latest end time among all meetings.
2. For each minute from the earliest start time to the latest end time:
   1. Count the number of meetings that are active at that minute.
   2. Update the maximum number of active meetings if necessary.
3. Return the maximum number of active meetings.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def minMeetingRooms(intervals):
    if not intervals:
        return 0
    
    # Find the earliest start time and the latest end time
    earliest_start = min(interval[0] for interval in intervals)
    latest_end = max(interval[1] for interval in intervals)
    
    max_rooms = 0
    
    # Simulate each minute
    for time in range(earliest_start, latest_end + 1):
        # Count the number of active meetings at this time
        active_meetings = sum(1 for start, end in intervals if start <= time < end)
        max_rooms = max(max_rooms, active_meetings)
    
    return max_rooms
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n * m) where n is the number of meetings and m is the range of meeting times (latest_end - earliest_start).
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient if the meeting times span a large range.

---

## **Step 5: Visualizing Redundant Computation**

* For intervals = [[0,30],[5,10],[15,20]]:
  * earliest_start = 0, latest_end = 30
  * For each minute from 0 to 30, we count the number of active meetings.
  * This is inefficient because the state only changes at the start or end of a meeting.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a chronological ordering approach.
* The key insight is that we only need to track the state changes (when meetings start or end).
* We can use a min-heap to keep track of the end times of active meetings.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* First, we sort the meetings by their start time.
* We use a min-heap to keep track of the end times of active meetings.
* As we process each meeting in order of start time:
  1. We remove all meetings that have ended before the current meeting starts.
  2. We add the current meeting to the heap.
  3. The size of the heap represents the number of rooms needed at that point.
* The maximum size of the heap during this process is the minimum number of rooms required.

### **Step 2: Flow Steps (Optimized)**

1. Sort the meetings by their start time.
2. Initialize a min-heap to keep track of the end times of active meetings.
3. Initialize max_rooms = 0.
4. For each meeting in the sorted array:
   1. Remove all meetings from the heap that have ended before the current meeting starts.
   2. Add the current meeting's end time to the heap.
   3. Update max_rooms if the current size of the heap is larger.
5. Return max_rooms.

### **Step 3: Implementation (Optimized)**

```python
import heapq

def minMeetingRooms(intervals):
    if not intervals:
        return 0
    
    # Sort meetings by start time
    intervals.sort(key=lambda x: x[0])
    
    # Min-heap to keep track of the end times of active meetings
    heap = []
    max_rooms = 0
    
    for start, end in intervals:
        # Remove all meetings that have ended
        while heap and heap[0] <= start:
            heapq.heappop(heap)
        
        # Add the current meeting
        heapq.heappush(heap, end)
        
        # Update the maximum number of rooms
        max_rooms = max(max_rooms, len(heap))
    
    return max_rooms
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n log n) where n is the number of meetings. Sorting the meetings takes O(n log n) time, and each heap operation takes O(log n) time.
* Space Complexity: O(n) for storing the heap.
* This approach is much more efficient than the brute force approach, especially if the meeting times span a large range.

### **Step 5: Visualizing Computation (Optimized)**

* For intervals = [[0,30],[5,10],[15,20]]:
  * Sort by start time -> [[0,30],[5,10],[15,20]] (already sorted)
  * Process [0,30]:
    * Remove ended meetings: None
    * Add [0,30] to heap -> [30]
    * max_rooms = max(0, 1) = 1
  * Process [5,10]:
    * Remove ended meetings: None (30 > 5)
    * Add [5,10] to heap -> [10, 30]
    * max_rooms = max(1, 2) = 2
  * Process [15,20]:
    * Remove ended meetings: [10] (10 <= 15)
    * Add [15,20] to heap -> [20, 30]
    * max_rooms = max(2, 2) = 2
  * Final max_rooms = 2

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n * m) | O(1) | Simulates each minute and counts active meetings |
| Optimized (Algorithm: Min-Heap) | O(n log n) | O(n) | Uses a min-heap to track active meetings |

---

## **Final Takeaways**

* The Meeting Rooms II problem is a classic example of the interval scheduling problem.
* The key insight is to track the state changes (when meetings start or end) rather than simulating each minute.
* Using a min-heap to keep track of the end times of active meetings allows us to efficiently determine the minimum number of rooms required.

---

## **Real-life Analogy**

* Imagine you're a hotel manager trying to determine how many conference rooms you need. You would look at the schedule of meetings and count how many are happening simultaneously at peak times. This is similar to how we track the maximum number of active meetings in our optimized approach.

---

## **Summary**

* We tackled the "Meeting Rooms II" problem by first sorting the meetings by their start time. Then, we used a min-heap to keep track of the end times of active meetings. As we processed each meeting, we removed all meetings that had ended, added the current meeting to the heap, and updated the maximum number of rooms if necessary. This approach has a time complexity of O(n log n) where n is the number of meetings, which is much more efficient than the brute force approach of simulating each minute. 