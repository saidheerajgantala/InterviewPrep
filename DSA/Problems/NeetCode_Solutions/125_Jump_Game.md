# 📝 Jump Game

## **Problem Statement**

* Given an array of non-negative integers `nums`, you are initially positioned at the first index of the array.
* Each element in the array represents your maximum jump length at that position.
* Determine if you are able to reach the last index.
* Example:
  * Input: nums = [2,3,1,1,4]
  * Output: true (Jump 1 step from index 0 to 1, then 3 steps to the last index)
  * Input: nums = [3,2,1,0,4]
  * Output: false (You will always arrive at index 3 with value 0. No way to reach the last index)
* Constraints:
  * 1 <= nums.length <= 10^4
  * 0 <= nums[i] <= 10^5

---

## **Step 1: Thinking Process (Brute Force)**

* We can use a recursive approach to explore all possible jumps from each position
* From each position, we can jump any distance from 1 up to the value at that position
* We need to try all possible jumps and see if any of them can reach the last index
* This is essentially a depth-first search of all possible paths

---

## **Step 2: Flow Steps (Brute Force)**

1. Define a recursive function `canReach(position)` that returns true if we can reach the end from the given position
2. Base cases:
   1. If position >= last index, return true (we've reached or passed the end)
   2. If nums[position] == 0, return false (we can't jump further)
3. For each possible jump length from 1 to nums[position]:
   1. If canReach(position + jump) returns true, return true
4. Return false if no jump leads to the end

---

## **Step 3: Brute Force Implementation (Code)**

```python
def canJump(nums):
    def can_reach(position):
        # Base cases
        if position >= len(nums) - 1:
            return True
        if nums[position] == 0:
            return False
        
        # Try all possible jumps
        max_jump = nums[position]
        for jump in range(1, max_jump + 1):
            if can_reach(position + jump):
                return True
        
        return False
    
    return can_reach(0)
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
  * We're repeatedly checking the same positions multiple times.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a greedy approach.
* The key insight is that we only need to track the furthest position we can reach.
* If at any point the furthest reachable position is less than our current position, we can't proceed.
* This allows us to solve the problem in a single pass through the array.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of trying all possible jumps, we can keep track of the furthest position we can reach
* As we iterate through the array, we update this furthest position
* If at any point our current position is beyond the furthest we can reach, we can't proceed
* If the furthest position we can reach is at least the last index, we can reach the end

### **Step 2: Flow Steps (Optimized)**

1. Initialize `max_reach = 0` (the furthest index we can reach)
2. Iterate through the array:
   1. If the current index is greater than max_reach, return false (we can't reach this position)
   2. Update max_reach = max(max_reach, i + nums[i])
   3. If max_reach >= last index, return true
3. Return true if we've gone through the entire array

### **Step 3: Implementation (Optimized)**

```python
def canJump(nums):
    max_reach = 0
    
    for i in range(len(nums)):
        # If we can't reach the current position
        if i > max_reach:
            return False
        
        # Update the furthest position we can reach
        max_reach = max(max_reach, i + nums[i])
        
        # If we can reach the last index, return true
        if max_reach >= len(nums) - 1:
            return True
    
    return True  # If we've gone through the entire array
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. We only need to iterate through the array once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* Much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For array [2,3,1,1,4]:
  * Start: max_reach = 0
  * Process index 0: max_reach = max(0, 0+2) = 2
  * Process index 1: max_reach = max(2, 1+3) = 4
  * Process index 2: max_reach = max(4, 2+1) = 4
  * Process index 3: max_reach = max(4, 3+1) = 4
  * Process index 4: max_reach = max(4, 4+4) = 8
  * max_reach >= 4, so we return true

* For array [3,2,1,0,4]:
  * Start: max_reach = 0
  * Process index 0: max_reach = max(0, 0+3) = 3
  * Process index 1: max_reach = max(3, 1+2) = 3
  * Process index 2: max_reach = max(3, 2+1) = 3
  * Process index 3: max_reach = max(3, 3+0) = 3
  * Process index 4: i > max_reach, so we return false

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n^n) | O(n) | Tries all possible jump combinations recursively |
| Optimized (Algorithm: Greedy) | O(n) | O(1) | Tracks the furthest reachable position |

---

## **Final Takeaways**

* The Jump Game is a classic example where a greedy approach works better than exhaustive search.
* The key insight is focusing on the furthest position we can reach rather than trying all possible jumps.
* This problem demonstrates how identifying the right metric to track can drastically simplify a problem.

---

## **Real-life Analogy**

* Imagine planning a road trip with limited fuel. Instead of calculating every possible combination of gas stations to stop at, you simply need to know if your current fuel level can get you to the next station. As long as you can always reach the next station, you'll eventually reach your destination.

---

## **Summary**

* We tackled the "Jump Game" problem first with a recursive approach that explores all possible jumps, resulting in exponential time complexity. We then optimized using a greedy approach that tracks the furthest reachable position, reducing the time complexity to O(n). This demonstrates how a greedy strategy can be much more efficient than exhaustive search for certain problems. 