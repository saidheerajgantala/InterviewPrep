# 📝 Car Fleet

## **Problem Statement**

* There are n cars going to the same destination along a one-lane road. The destination is target miles away.
* You are given two integer arrays `position` and `speed`, both of length n, where `position[i]` is the position of the ith car and `speed[i]` is the speed of the ith car (in miles per hour).
* A car can never pass another car ahead of it, but it can catch up to it and drive bumper to bumper at the same speed. The faster car will slow down to match the slower car's speed. The distance between these two cars is ignored (i.e., they are assumed to have the same position).
* A car fleet is some non-empty set of cars driving at the same speed. Note that a single car is also a car fleet.
* If a car catches up to a car fleet right at the destination point, it will still be considered as one car fleet.
* Return the number of car fleets that will arrive at the destination.
* Example:
  * Input: target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]
  * Output: 3
  * Explanation:
    * The cars starting at 10 (speed 2) and 8 (speed 4) become a fleet, meeting at 12.
    * The car starting at 0 (speed 1) doesn't catch up to any other car, so it is a fleet by itself.
    * The cars starting at 5 (speed 1) and 3 (speed 3) become a fleet, meeting at 6. The fleet moves at speed 1 until it reaches the target.
    * Note that no other cars meet these fleets before the destination, so the answer is 3.
* Constraints:
  * n == position.length == speed.length
  * 1 <= n <= 10^5
  * 0 <= position[i] < target
  * All the values of position are unique.
  * 0 < speed[i] <= 10^6
  * 1 <= target <= 10^6

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine how many car fleets will arrive at the destination
* A car fleet forms when a faster car catches up to a slower car ahead of it
* The key insight is that once cars form a fleet, they move at the speed of the slowest car in the fleet
* We need to track which cars will catch up to others before reaching the destination
* One approach is to simulate the movement of all cars until they reach the destination

---

## **Step 2: Flow Steps (Brute Force)**

1. Pair each car's position with its speed
2. Sort the cars by their position in descending order (from closest to destination to furthest)
3. Initialize a list to track the time each car takes to reach the destination
4. For each car:
   1. Calculate the time it takes to reach the destination: (target - position) / speed
   2. Add this time to the list
5. Iterate through the list of times:
   1. If a car takes more time than the car ahead of it, it will catch up and form a fleet
   2. Otherwise, it will remain a separate fleet
6. Count the number of fleets and return the result

---

## **Step 3: Brute Force Implementation (Code)**

```python
def carFleet(target, position, speed):
    # Pair each car's position with its speed
    cars = list(zip(position, speed))
    
    # Sort cars by position in descending order
    cars.sort(reverse=True)
    
    # Calculate time to reach destination for each car
    times = [(target - pos) / spd for pos, spd in cars]
    
    # Count the number of fleets
    fleets = 0
    max_time = 0
    
    for time in times:
        if time > max_time:
            # This car cannot catch up to the fleet ahead
            fleets += 1
            max_time = time
    
    return fleets
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n log n) where n is the number of cars. The dominant operation is sorting the cars by position.
* Space Complexity: O(n) for storing the cars and times arrays.
* This approach is actually quite efficient, and it's not truly a "brute force" approach. However, we can optimize it slightly.

---

## **Step 5: Visualizing Redundant Computation**

* For target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]:
  * Cars sorted by position (descending): [(10,2), (8,4), (5,1), (3,3), (0,1)]
  * Times to reach destination: [1, 1, 7, 3, 12]
  * Car at position 10: time = 1, max_time = 0 -> max_time = 1, fleets = 1
  * Car at position 8: time = 1, max_time = 1 -> max_time = 1, fleets = 1 (forms a fleet with the car ahead)
  * Car at position 5: time = 7, max_time = 1 -> max_time = 7, fleets = 2
  * Car at position 3: time = 3, max_time = 7 -> max_time = 7, fleets = 2 (forms a fleet with the car ahead)
  * Car at position 0: time = 12, max_time = 7 -> max_time = 12, fleets = 3
* We're doing some redundant computation by calculating all times first and then iterating through them again.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Stack-based approach.
* The property that matches this problem is the ability to track the slowest car in each fleet.
* By processing cars from closest to destination to furthest, we can determine which cars will form fleets.
* Alternatives like simulation would be less efficient.
* Precondition: Cars cannot pass each other, and they form fleets when a faster car catches up to a slower one.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can combine the calculation of times and the counting of fleets into a single pass
* We'll use a stack to keep track of the times of the lead cars in each fleet
* If a car takes more time to reach the destination than the car ahead of it, it will form a new fleet
* Otherwise, it will catch up and join the fleet ahead

### **Step 2: Flow Steps (Optimized)**

1. Pair each car's position with its speed
2. Sort the cars by their position in descending order (from closest to destination to furthest)
3. Initialize an empty stack to track the times of lead cars in fleets
4. For each car:
   1. Calculate the time it takes to reach the destination: (target - position) / speed
   2. If the stack is empty or the current car takes more time than the car at the top of the stack, push its time onto the stack
5. Return the size of the stack, which represents the number of fleets

### **Step 3: Implementation (Optimized)**

```python
def carFleet(target, position, speed):
    # Pair each car's position with its speed
    cars = list(zip(position, speed))
    
    # Sort cars by position in descending order
    cars.sort(reverse=True)
    
    stack = []  # Stack to track times of lead cars in fleets
    
    for pos, spd in cars:
        # Calculate time to reach destination
        time = (target - pos) / spd
        
        # If this car is slower than the lead car of the last fleet, it forms a new fleet
        if not stack or time > stack[-1]:
            stack.append(time)
    
    return len(stack)
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n log n) where n is the number of cars. The dominant operation is still sorting the cars by position.
* Space Complexity: O(n) in the worst case, where each car forms its own fleet.
* The time complexity remains the same, but we've simplified the implementation and reduced the number of operations.

### **Step 5: Visualizing Computation (Optimized)**

* For target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]:
  * Cars sorted by position (descending): [(10,2), (8,4), (5,1), (3,3), (0,1)]
  * Car at position 10: time = 1, stack = [1]
  * Car at position 8: time = 1, 1 <= 1, so it joins the fleet ahead, stack = [1]
  * Car at position 5: time = 7, 7 > 1, so it forms a new fleet, stack = [1, 7]
  * Car at position 3: time = 3, 3 < 7, so it joins the fleet ahead, stack = [1, 7]
  * Car at position 0: time = 12, 12 > 7, so it forms a new fleet, stack = [1, 7, 12]
  * Number of fleets = len(stack) = 3
* We're efficiently determining which cars form fleets in a single pass through the sorted cars.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n log n) | O(n) | Calculates all times first, then counts fleets |
| Optimized (Algorithm: Stack) | O(n log n) | O(n) | Combines calculation and counting in a single pass |

---

## **Final Takeaways**

* Processing cars from closest to destination to furthest allows us to determine which cars will form fleets.
* The key insight is that if a car takes more time to reach the destination than the car ahead of it, it will form a new fleet.
* This problem demonstrates how understanding the physical constraints of the problem (cars cannot pass each other) can lead to an efficient algorithm.

---

## **Real-life Analogy**

* Imagine you're watching cars on a highway from above, all heading toward the same exit. The brute force approach would be like tracking each car's position over time until they all reach the exit. The optimized approach is like looking at a snapshot of the highway and calculating when each car would reach the exit if it maintained its current speed - if a car would reach the exit later than a car ahead of it, they'll form a fleet, otherwise they'll remain separate.

---

## **Summary**

* We solved the "Car Fleet" problem by first considering an approach that calculates the time each car takes to reach the destination and then counts the number of fleets. We then optimized by using a stack to track the times of lead cars in fleets, combining the calculation and counting into a single pass. Both approaches have O(n log n) time complexity due to the sorting operation, but the optimized approach is more concise and efficient in terms of the number of operations. The key insight is that if a car takes more time to reach the destination than the car ahead of it, it will form a new fleet. 