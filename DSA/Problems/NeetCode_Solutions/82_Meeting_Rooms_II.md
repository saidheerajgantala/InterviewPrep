# 📝 Meeting Rooms II

## **Problem Statement**

* Given an array of meeting time intervals `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of conference rooms required.
* Example 1:
  * Input: intervals = [[0,30],[5,10],[15,20]]
  * Output: 2
  * Explanation: We need two conference rooms.
    * Room 1: (0,30)
    * Room 2: (5,10), (15,20)
* Example 2:
  * Input: intervals = [[7,10],[2,4]]
  * Output: 1
  * Explanation: We need only one conference room. After the meeting [2,4] ends, the meeting [7,10] can use the same room.
* Constraints:
  * 1 <= intervals.length <= 10^4
  * 0 <= starti < endi <= 10^6

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the minimum number of conference rooms required to accommodate all meetings
* One approach would be to simulate the booking process:
  * Sort the intervals by start time
  * For each interval, check if any existing room is available (i.e., the previous meeting in that room ends before the current meeting starts)
  * If a room is available, book the current meeting in that room
  * If no room is available, create a new room for the current meeting
* This approach is intuitive but might be inefficient for large inputs

---

## **Step 2: Flow Steps (Brute Force)**

1. Sort the intervals by start time
2. Initialize an empty list of rooms, where each room stores the end time of the last meeting in that room
3. For each interval:
   1. Check if any existing room is available (i.e., the end time of the last meeting in that room is less than or equal to the start time of the current meeting)
   2. If a room is available, update the end time of that room with the end time of the current meeting
   3. If no room is available, create a new room with the end time of the current meeting
4. Return the number of rooms

---

## **Step 3: Brute Force Implementation (Code)**

```python
def minMeetingRooms(intervals):
    if not intervals:
        return 0
    
    # Sort intervals by start time
    intervals.sort(key=lambda x: x[0])
    
    # Initialize a list to store the end time of meetings in each room
    rooms = []
    
    for interval in intervals:
        start, end = interval
        
        # Check if any existing room is available
        room_found = False
        for i in range(len(rooms)):
            if rooms[i] <= start:
                # Update the end time of this room
                rooms[i] = end
                room_found = True
                break
        
        # If no room is available, create a new room
        if not room_found:
            rooms.append(end)
    
    # Return the number of rooms
    return len(rooms)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n^2) where n is the number of intervals. Sorting takes O(n log n) time, and for each interval, we might need to check all existing rooms, which takes O(n) time in the worst case.
* Space Complexity: O(n) for storing the rooms.
* This approach is inefficient for large inputs due to the nested loop for checking available rooms.

---

## **Step 5: Visualizing Redundant Computation**

* For the example intervals = [[0,30],[5,10],[15,20]]:
  * Sort intervals: [[0,30],[5,10],[15,20]]
  * Process [0,30]: Create room 1 with end time 30, rooms = [30]
  * Process [5,10]: Check room 1, not available (5 < 30), create room 2 with end time 10, rooms = [30, 10]
  * Process [15,20]: Check room 1, not available (15 < 30), check room 2, available (15 > 10), update room 2 with end time 20, rooms = [30, 20]
  * Return 2 rooms
* For the example intervals = [[7,10],[2,4]]:
  * Sort intervals: [[2,4],[7,10]]
  * Process [2,4]: Create room 1 with end time 4, rooms = [4]
  * Process [7,10]: Check room 1, available (7 > 4), update room 1 with end time 10, rooms = [10]
  * Return 1 room
* The redundant computation is in checking all rooms for each interval, which is inefficient

---

## **Why are we using this algorithm?**

* For optimization, we'll use a min-heap to efficiently track the end times of meetings.
* The property that matches this problem is that we only need to know the earliest ending meeting to determine if a new room is needed.
* By using a min-heap, we can efficiently find and update the earliest ending meeting.
* Alternatives like the brute force approach would be less efficient for large inputs.
* Precondition: We need to be able to compare meeting times.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Min-Heap)**

* We can use a min-heap to efficiently track the end times of meetings in each room
* The key insight is that we only need to check if the earliest ending meeting has ended before the current meeting starts
* If it has, we can reuse that room; otherwise, we need a new room
* This approach avoids checking all rooms for each meeting

### **Step 2: Flow Steps (Optimized - Min-Heap)**

1. Sort the intervals by start time
2. Initialize a min-heap to store the end times of meetings
3. For each interval:
   1. If the heap is not empty and the earliest ending meeting (the top of the heap) has ended before or at the start of the current meeting, remove it from the heap
   2. Add the end time of the current meeting to the heap
4. Return the size of the heap, which represents the number of rooms needed

### **Step 3: Implementation (Optimized - Min-Heap)**

```python
import heapq

def minMeetingRooms(intervals):
    if not intervals:
        return 0
    
    # Sort intervals by start time
    intervals.sort(key=lambda x: x[0])
    
    # Initialize a min-heap to store the end times of meetings
    rooms = []
    
    for interval in intervals:
        start, end = interval
        
        # If the earliest ending meeting has ended, remove it
        if rooms and rooms[0] <= start:
            heapq.heappop(rooms)
        
        # Add the end time of the current meeting
        heapq.heappush(rooms, end)
    
    # Return the number of rooms
    return len(rooms)
```

### **Alternative Approach: Chronological Ordering**

Another efficient approach is to use chronological ordering of all start and end times:

```python
def minMeetingRooms(intervals):
    if not intervals:
        return 0
    
    # Separate start and end times
    start_times = sorted([interval[0] for interval in intervals])
    end_times = sorted([interval[1] for interval in intervals])
    
    # Initialize variables
    rooms = 0
    end_idx = 0
    
    # Process all meetings in chronological order
    for start in start_times:
        # If the earliest ending meeting has ended, free up a room
        if start >= end_times[end_idx]:
            end_idx += 1
        else:
            # Otherwise, we need a new room
            rooms += 1
    
    return rooms
```

### **Step 4: Complexity Analysis (Optimized - Min-Heap)**

* Time Complexity: O(n log n) where n is the number of intervals. Sorting takes O(n log n) time, and each heap operation takes O(log n) time, which we do at most 2n times (n pushes and at most n pops).
* Space Complexity: O(n) for storing the heap.
* This approach is much more efficient than the brute force approach, especially for large inputs.

### **Complexity Analysis (Chronological Ordering)**

* Time Complexity: O(n log n) where n is the number of intervals. Sorting the start and end times takes O(n log n) time, and the rest of the algorithm is O(n).
* Space Complexity: O(n) for storing the start and end times.
* This approach is also efficient and has the same time complexity as the min-heap approach.

### **Step 5: Visualizing Computation (Optimized - Min-Heap)**

* For the example intervals = [[0,30],[5,10],[15,20]]:
  * Sort intervals: [[0,30],[5,10],[15,20]]
  * Process [0,30]: Add 30 to the heap, heap = [30]
  * Process [5,10]: 5 < 30, so add 10 to the heap, heap = [10, 30]
  * Process [15,20]: 15 > 10, so remove 10 from the heap, add 20 to the heap, heap = [20, 30]
  * Return 2 rooms
* For the example intervals = [[7,10],[2,4]]:
  * Sort intervals: [[2,4],[7,10]]
  * Process [2,4]: Add 4 to the heap, heap = [4]
  * Process [7,10]: 7 > 4, so remove 4 from the heap, add 10 to the heap, heap = [10]
  * Return 1 room
* The min-heap approach efficiently tracks the end times of meetings and avoids redundant computation

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n^2) | O(n) | Inefficient for large inputs |
| Optimized (Min-Heap) | O(n log n) | O(n) | Efficient for all inputs |
| Chronological Ordering | O(n log n) | O(n) | Also efficient for all inputs |

---

## **Final Takeaways**

* The key to efficiently solving the Meeting Rooms II problem is to track the end times of meetings and determine if a new room is needed based on the earliest ending meeting.
* By using a min-heap, we can efficiently find and update the earliest ending meeting.
* The chronological ordering approach provides an alternative perspective by treating start and end times as separate events.
* This problem demonstrates how choosing the right data structure can significantly improve the efficiency of an algorithm.

---

## **Real-life Analogy**

* Imagine you're a hotel manager trying to assign rooms to guests based on their check-in and check-out times. The brute force approach would be like checking each room one by one to see if it's available for a new guest. The min-heap approach would be like keeping track of the earliest check-out time among all occupied rooms, and immediately assigning that room to a new guest if their check-in time is after the check-out time. The chronological ordering approach would be like maintaining a timeline of all check-ins and check-outs, and determining the maximum number of rooms needed at any point in time.

---

## **Summary**

* We solved the Meeting Rooms II problem by first considering a brute force approach that checks all rooms for each meeting, resulting in O(n^2) time complexity. We then optimized using a min-heap to efficiently track the end times of meetings, reducing the time complexity to O(n log n). We also explored a chronological ordering approach that treats start and end times as separate events, which also has O(n log n) time complexity. The key insight is to track the end times of meetings and determine if a new room is needed based on the earliest ending meeting. 