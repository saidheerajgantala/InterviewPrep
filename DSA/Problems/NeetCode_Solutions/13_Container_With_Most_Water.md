# 📝 Container With Most Water

## **Problem Statement**

* You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `i`th line are `(i, 0)` and `(i, height[i])`.
* Find two lines that together with the x-axis form a container, such that the container contains the most water.
* Return the maximum amount of water a container can store.
* Notice that you may not slant the container.
* Example:
  * Input: height = [1,8,6,2,5,4,8,3,7]
  * Output: 49
  * Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
* Constraints:
  * n == height.length
  * 2 <= n <= 10^5
  * 0 <= height[i] <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find two lines that form a container with the maximum area
* The area of water between two lines is determined by:
  * The width (distance between the two lines)
  * The height (the minimum height of the two lines)
* Area = width * height = (j - i) * min(height[i], height[j])
* The most straightforward approach is to check all possible pairs of lines
* For each pair, calculate the area and keep track of the maximum

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize `max_area = 0`
2. For each index i from 0 to n-2:
   1. For each index j from i+1 to n-1:
      1. Calculate width = j - i
      2. Calculate height = min(height[i], height[j])
      3. Calculate area = width * height
      4. Update `max_area = max(max_area, area)`
3. Return `max_area`

---

## **Step 3: Brute Force Implementation (Code)**

```python
def maxArea(height):
    n = len(height)
    max_area = 0
    
    for i in range(n):
        for j in range(i + 1, n):
            width = j - i
            h = min(height[i], height[j])
            area = width * h
            max_area = max(max_area, area)
    
    return max_area
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the length of the array. We have two nested loops.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient for large arrays and will likely result in a time limit exceeded error.

---

## **Step 5: Visualizing Redundant Computation**

* For array [1,8,6,2,5,4,8,3,7]:
  * We check all possible pairs (36 combinations for n=9)
  * Many pairs are checked that clearly won't give the maximum area
  * For example, if we've found a container with a large area, we don't need to check containers with smaller width unless they have significantly taller heights

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Two-Pointer algorithm.
* The property that matches this problem is the ability to narrow down the search space based on the container properties.
* By using two pointers from opposite ends, we can efficiently find the container with the maximum area.
* Alternatives like dynamic programming would be more complex and less efficient.
* Precondition: The area is determined by the minimum height and the distance between lines.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of checking all pairs, we can use a two-pointer approach
* We start with the widest possible container (pointers at both ends)
* The area is limited by the shorter line
* To maximize area, we need to find taller lines
* So, we move the pointer pointing to the shorter line inward
* We continue this process until the pointers meet
* Along the way, we keep track of the maximum area

### **Step 2: Flow Steps (Optimized)**

1. Initialize two pointers: `left = 0` and `right = n - 1`
2. Initialize `max_area = 0`
3. While `left < right`:
   1. Calculate width = right - left
   2. Calculate height = min(height[left], height[right])
   3. Calculate area = width * height
   4. Update `max_area = max(max_area, area)`
   5. If height[left] < height[right], increment left
   6. Otherwise, decrement right
4. Return `max_area`

### **Step 3: Implementation (Optimized)**

```python
def maxArea(height):
    n = len(height)
    left, right = 0, n - 1
    max_area = 0
    
    while left < right:
        width = right - left
        h = min(height[left], height[right])
        area = width * h
        max_area = max(max_area, area)
        
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    
    return max_area
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. We scan the array once with the two pointers.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This is much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For array [1,8,6,2,5,4,8,3,7]:
  * left=0 (1), right=8 (7) → area = 8 * min(1, 7) = 8
  * height[left] < height[right], so left++
  * left=1 (8), right=8 (7) → area = 7 * min(8, 7) = 49
  * height[left] > height[right], so right--
  * left=1 (8), right=7 (3) → area = 6 * min(8, 3) = 18
  * height[left] > height[right], so right--
  * left=1 (8), right=6 (8) → area = 5 * min(8, 8) = 40
  * height[left] == height[right], so either pointer can move; let's move right--
  * left=1 (8), right=5 (4) → area = 4 * min(8, 4) = 16
  * ...and so on
  * Maximum area = 49
* We're efficiently narrowing down the search space by moving the pointer pointing to the shorter line.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Checks all possible pairs |
| Optimized (Algorithm: Two-Pointer) | O(n) | O(1) | Starts with the widest container and narrows down |

---

## **Final Takeaways**

* Using a two-pointer approach allows us to efficiently find the container with the maximum area.
* The key insight is that the area is limited by the shorter line, so we should always move the pointer pointing to the shorter line.
* This problem demonstrates how understanding the properties of a problem can lead to efficient algorithms without exhaustive searching.

---

## **Real-life Analogy**

* Imagine you're trying to find the largest rectangular swimming pool that can be built between two walls of varying heights. The brute force approach would be like measuring the area between every possible pair of walls. The optimized approach is like starting with the two farthest walls and then strategically moving inward - if the left wall is shorter than the right, you try a taller left wall closer in; if the right wall is shorter, you try a taller right wall closer in. This way, you efficiently find the largest possible pool without checking all combinations.

---

## **Summary**

* We solved the "Container With Most Water" problem by first considering a brute force approach that checks all possible pairs of lines, resulting in O(n²) time complexity. We then optimized using a two-pointer approach that starts with the widest container and strategically narrows down by moving the pointer pointing to the shorter line, reducing the time complexity to O(n). This demonstrates how understanding the properties of a problem can lead to efficient algorithms without exhaustive searching. 