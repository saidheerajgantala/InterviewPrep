# 📝 Jump Game II

## **Problem Statement**

* Given an array of non-negative integers `nums`, you are initially positioned at the first index of the array.
* Each element in the array represents your maximum jump length at that position.
* Your goal is to reach the last index in the minimum number of jumps.
* You can assume that you can always reach the last index.
* Example:
  * Input: nums = [2,3,1,1,4]
  * Output: 2 (Jump 1 step from index 0 to 1, then 3 steps to the last index)
* Constraints:
  * 1 <= nums.length <= 10^4
  * 0 <= nums[i] <= 1000

---

## **Step 1: Thinking Process (Brute Force)**

* Similar to Jump Game I, we can use a recursive approach
* For each position, we try all possible jumps and find the minimum number of jumps needed
* We need to explore all possible paths to find the one with the minimum number of jumps
* This is essentially a breadth-first search of all possible paths

---

## **Step 2: Flow Steps (Brute Force)**

1. Define a recursive function `minJumps(position, jumps)` that returns the minimum jumps needed to reach the end from the given position
2. Base cases:
   1. If position >= last index, return jumps (we've reached or passed the end)
   2. If nums[position] == 0, return infinity (we can't jump further)
3. Initialize minJumpsNeeded to infinity
4. For each possible jump length from 1 to nums[position]:
   1. Calculate jumpsNeeded = minJumps(position + jump, jumps + 1)
   2. Update minJumpsNeeded if jumpsNeeded is smaller
5. Return minJumpsNeeded

---

## **Step 3: Brute Force Implementation (Code)**

```python
def jump(nums):
    def min_jumps(position, jumps):
        # Base cases
        if position >= len(nums) - 1:
            return jumps
        if nums[position] == 0:
            return float('inf')
        
        min_jumps_needed = float('inf')
        max_jump = nums[position]
        
        # Try all possible jumps
        for j in range(1, max_jump + 1):
            jumps_needed = min_jumps(position + j, jumps + 1)
            min_jumps_needed = min(min_jumps_needed, jumps_needed)
        
        return min_jumps_needed
    
    return min_jumps(0, 0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n^n) in the worst case, where n is the length of the array. This is because at each position, we might need to try n different jumps, and the recursion depth could be n.
* Space Complexity: O(n) for the recursion stack.
* This solution will time out for large inputs due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For array [2,3,1,1,4]:
  * From index 0, we try jumps of 1 and 2
  * From index 1, we try jumps of 1, 2, and 3
  * From index 2, we try a jump of 1
  * From index 3, we try a jump of 1
  * We're repeatedly calculating the minimum jumps for the same positions.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a greedy approach with the BFS concept.
* The key insight is to always make the jump that gets us the furthest in the next step.
* This is known as the "greedy" approach because we're always choosing the locally optimal move.
* We can solve this in linear time by keeping track of the current range we can jump to.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can think of this problem as finding the minimum number of "levels" needed to reach the end
* Each level represents the range of indices we can reach with a certain number of jumps
* For each level, we find the furthest position we can reach in the next jump
* This is similar to a BFS approach, where each level represents a certain number of jumps

### **Step 2: Flow Steps (Optimized)**

1. Initialize variables:
   - jumps = 0 (number of jumps taken)
   - current_max_reach = 0 (furthest position reachable in the current jump)
   - next_max_reach = 0 (furthest position reachable in the next jump)
2. Iterate through the array (except the last element, as we don't need to jump from it):
   1. Update next_max_reach = max(next_max_reach, i + nums[i])
   2. If we've reached the end of the current jump range (i == current_max_reach):
      1. Increment jumps
      2. Update current_max_reach = next_max_reach
      3. If current_max_reach >= last index, return jumps
3. Return jumps

### **Step 3: Implementation (Optimized)**

```python
def jump(nums):
    n = len(nums)
    if n == 1:
        return 0  # Already at the end
    
    jumps = 0
    current_max_reach = 0
    next_max_reach = 0
    
    for i in range(n - 1):  # Don't need to process the last element
        # Update the furthest position we can reach after the next jump
        next_max_reach = max(next_max_reach, i + nums[i])
        
        # If we've reached the end of the current jump range
        if i == current_max_reach:
            # Take a jump
            jumps += 1
            # Update the range for the next jump
            current_max_reach = next_max_reach
            
            # If we can already reach the end, return the jumps
            if current_max_reach >= n - 1:
                return jumps
    
    return jumps
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. We only need to iterate through the array once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* Much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For array [2,3,1,1,4]:
  * Start: jumps = 0, current_max_reach = 0, next_max_reach = 0
  * Process index 0: next_max_reach = max(0, 0+2) = 2
    * i == current_max_reach, so jumps = 1, current_max_reach = 2
  * Process index 1: next_max_reach = max(2, 1+3) = 4
    * i != current_max_reach, continue
  * Process index 2: next_max_reach = max(4, 2+1) = 4
    * i == current_max_reach, so jumps = 2, current_max_reach = 4
    * current_max_reach >= n-1, so return jumps = 2
  * Final result: 2 jumps

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n^n) | O(n) | Tries all possible jump combinations recursively |
| Optimized (Algorithm: Greedy BFS) | O(n) | O(1) | Uses a level-by-level approach similar to BFS |

---

## **Final Takeaways**

* Jump Game II builds on Jump Game I but asks for the minimum number of jumps.
* The greedy approach works because we always want to maximize the range we can reach in the next jump.
* This problem demonstrates how a BFS-like approach can be implemented iteratively without using a queue.

---

## **Real-life Analogy**

* Imagine you're planning a road trip and want to minimize the number of times you need to refuel. At each gas station, you would choose to fill up enough gas to reach the furthest possible gas station in your range, thus minimizing the total number of stops.

---

## **Summary**

* We tackled the "Jump Game II" problem first with a recursive approach that explores all possible jumps, resulting in exponential time complexity. We then optimized using a greedy BFS-like approach that processes the array level by level, reducing the time complexity to O(n). This demonstrates how thinking about a problem in terms of "levels" or "ranges" can lead to an efficient solution. 