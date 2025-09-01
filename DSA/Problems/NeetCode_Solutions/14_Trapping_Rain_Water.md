# 📝 Trapping Rain Water

## **Problem Statement**

* Given `n` non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.
* Example:
  * Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
  * Output: 6
  * Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
* Constraints:
  * n == height.length
  * 1 <= n <= 2 * 10^4
  * 0 <= height[i] <= 10^5

---

## **Step 1: Thinking Process (Brute Force)**

* Water can be trapped at a position if there are higher bars on both sides
* The amount of water trapped at a position is determined by the minimum of the maximum heights on the left and right sides, minus the height at the current position
* For each position, we need to find:
  * The maximum height to its left
  * The maximum height to its right
* Then, the water trapped at that position is min(left_max, right_max) - height[i]
* We sum up the water trapped at all positions to get the total

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize `total_water = 0`
2. For each position i from 1 to n-2:
   1. Find `left_max` = maximum height from 0 to i-1
   2. Find `right_max` = maximum height from i+1 to n-1
   3. Calculate water at position i = max(0, min(left_max, right_max) - height[i])
   4. Add this water to `total_water`
3. Return `total_water`

---

## **Step 3: Brute Force Implementation (Code)**

```python
def trap(height):
    n = len(height)
    total_water = 0
    
    for i in range(1, n - 1):
        # Find maximum height to the left
        left_max = max(height[:i])
        
        # Find maximum height to the right
        right_max = max(height[i+1:])
        
        # Calculate water trapped at this position
        water = max(0, min(left_max, right_max) - height[i])
        total_water += water
    
    return total_water
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the length of the array.
  * For each position, we're finding the maximum height to its left and right, which takes O(n) time
  * We do this for each of the n positions
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient for large arrays and will likely result in a time limit exceeded error.

---

## **Step 5: Visualizing Redundant Computation**

* For array [0,1,0,2,1,0,1,3,2,1,2,1]:
  * For position i=1 (height=1), we find left_max=0, right_max=3
  * For position i=2 (height=0), we find left_max=1, right_max=3
  * For position i=3 (height=2), we find left_max=1, right_max=3
  * ...and so on
* We're repeatedly calculating the maximum heights to the left and right for each position
* This is redundant because the maximum heights can be precomputed once and reused

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Dynamic Programming approach.
* The property that matches this problem is the ability to precompute and store results for reuse.
* By precomputing the maximum heights to the left and right for each position, we can avoid redundant calculations.
* Alternatives like the two-pointer approach would also work but might be less intuitive initially.
* Precondition: We need to efficiently find the maximum heights to the left and right for each position.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of recalculating the maximum heights for each position, we can precompute them
* We can use two arrays:
  * `left_max[i]` = maximum height from 0 to i
  * `right_max[i]` = maximum height from i to n-1
* Then, for each position, the water trapped is min(left_max[i], right_max[i]) - height[i]
* This way, we avoid redundant calculations and improve efficiency

### **Step 2: Flow Steps (Optimized)**

1. Initialize arrays `left_max` and `right_max` of size n
2. Compute `left_max`:
   1. left_max[0] = height[0]
   2. For each position i from 1 to n-1:
      1. left_max[i] = max(left_max[i-1], height[i])
3. Compute `right_max`:
   1. right_max[n-1] = height[n-1]
   2. For each position i from n-2 down to 0:
      1. right_max[i] = max(right_max[i+1], height[i])
4. Initialize `total_water = 0`
5. For each position i from 0 to n-1:
   1. Calculate water at position i = max(0, min(left_max[i], right_max[i]) - height[i])
   2. Add this water to `total_water`
6. Return `total_water`

### **Step 3: Implementation (Optimized)**

```python
def trap(height):
    n = len(height)
    if n <= 2:
        return 0
    
    # Precompute maximum heights to the left
    left_max = [0] * n
    left_max[0] = height[0]
    for i in range(1, n):
        left_max[i] = max(left_max[i-1], height[i])
    
    # Precompute maximum heights to the right
    right_max = [0] * n
    right_max[n-1] = height[n-1]
    for i in range(n-2, -1, -1):
        right_max[i] = max(right_max[i+1], height[i])
    
    # Calculate water trapped at each position
    total_water = 0
    for i in range(n):
        water = min(left_max[i], right_max[i]) - height[i]
        total_water += water
    
    return total_water
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array.
  * We make three passes through the array: one to compute left_max, one to compute right_max, and one to calculate the water trapped
* Space Complexity: O(n) for the left_max and right_max arrays.
* This is much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For array [0,1,0,2,1,0,1,3,2,1,2,1]:
  * left_max = [0,1,1,2,2,2,2,3,3,3,3,3]
  * right_max = [3,3,3,3,3,3,3,3,2,2,2,1]
  * For position i=0 (height=0), water = min(0,3) - 0 = 0
  * For position i=1 (height=1), water = min(1,3) - 1 = 0
  * For position i=2 (height=0), water = min(1,3) - 0 = 1
  * ...and so on
  * Total water = 6
* We're efficiently calculating the water trapped at each position using precomputed values.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Recalculates maximum heights for each position |
| Optimized (Algorithm: Dynamic Programming) | O(n) | O(n) | Precomputes maximum heights |

---

## **Final Takeaways**

* Precomputing values and storing them for reuse is a common dynamic programming technique.
* The key insight is recognizing that we can avoid redundant calculations by computing the maximum heights once.
* This problem demonstrates how understanding the structure of the computation can lead to significant efficiency improvements.

---

## **Real-life Analogy**

* Imagine you're calculating how much rainwater would collect in a series of valleys between mountains. The brute force approach would be like standing at each point and looking both left and right to find the highest peaks, then calculating how much water would collect at that point. The optimized approach is like having two teams: one starts from the left and marks the highest peak seen so far at each point, and the other does the same from the right. Then, you can quickly determine the water level at each point by comparing these marks.

---

## **Summary**

* We solved the "Trapping Rain Water" problem by first considering a brute force approach that recalculates the maximum heights for each position, resulting in O(n²) time complexity. We then optimized using a dynamic programming approach that precomputes the maximum heights to the left and right for each position, reducing the time complexity to O(n) at the cost of O(n) space. This demonstrates the power of precomputation and reuse in algorithm design. 