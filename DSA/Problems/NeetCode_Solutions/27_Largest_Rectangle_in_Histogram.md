# 📝 Largest Rectangle in Histogram

## **Problem Statement**

* Given an array of integers `heights` representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.
* Example:
  * Input: heights = [2,1,5,6,2,3]
  * Output: 10
  * Explanation: The above is a histogram where width of each bar is 1. The largest rectangle is shown in the red area, which has an area = 10 units.
* Constraints:
  * 1 <= heights.length <= 10^5
  * 0 <= heights[i] <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the largest rectangle that can be formed in the histogram
* A rectangle in a histogram extends horizontally and has a height equal to the minimum bar height in that range
* One approach is to consider every possible pair of bars (left and right boundaries) and calculate the area of the rectangle
* The area would be (right - left + 1) * min(heights[left:right+1])
* We need to find the maximum of all these areas

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize `max_area = 0`
2. For each left boundary from 0 to n-1:
   1. For each right boundary from left to n-1:
      1. Find the minimum height in the range [left, right]
      2. Calculate the area = (right - left + 1) * min_height
      3. Update `max_area = max(max_area, area)`
3. Return `max_area`

---

## **Step 3: Brute Force Implementation (Code)**

```python
def largestRectangleArea(heights):
    n = len(heights)
    max_area = 0
    
    for left in range(n):
        min_height = float('inf')
        for right in range(left, n):
            # Find the minimum height in the range [left, right]
            min_height = min(min_height, heights[right])
            
            # Calculate the area
            area = (right - left + 1) * min_height
            
            # Update the maximum area
            max_area = max(max_area, area)
    
    return max_area
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the length of the array. We have two nested loops, and for each pair of boundaries, we're finding the minimum height in O(1) time (by keeping track of the minimum as we go).
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient for large arrays and will likely result in a time limit exceeded error.

---

## **Step 5: Visualizing Redundant Computation**

* For heights = [2,1,5,6,2,3]:
  * For left=0, right=0: min_height = 2, area = 2
  * For left=0, right=1: min_height = 1, area = 2
  * For left=0, right=2: min_height = 1, area = 3
  * ...and so on
* We're repeatedly finding the minimum height for overlapping ranges
* For example, when we move from (left=0, right=2) to (left=0, right=3), we're recalculating the minimum for the range [0,2] and then including heights[3]

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Stack-based approach.
* The property that matches this problem is the ability to efficiently find the largest rectangle that includes each bar.
* By using a monotonic increasing stack, we can find the largest rectangle that each bar can form.
* Alternatives like dynamic programming would be more complex and less efficient.
* Precondition: We need to efficiently find the left and right boundaries for each bar where it is the minimum height.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of considering all possible pairs of bars, we can focus on finding the largest rectangle that each bar can form
* For each bar, we need to find the left and right boundaries where this bar is the minimum height
* We can use a stack to keep track of the indices of bars in increasing order of height
* When we encounter a bar shorter than the top of the stack, we know we've found the right boundary for the bars in the stack
* We can then calculate the area for each bar and keep track of the maximum

### **Step 2: Flow Steps (Optimized)**

1. Initialize an empty stack and `max_area = 0`
2. Iterate through each bar at index i:
   1. While the stack is not empty and the current bar is shorter than the bar at the top of the stack:
      1. Pop the top of the stack (call it j)
      2. Calculate the area = heights[j] * (i - stack[-1] - 1 if stack else i)
      3. Update `max_area = max(max_area, area)`
   2. Push the current index i onto the stack
3. After the iteration, process any remaining bars in the stack:
   1. Pop the top of the stack (call it j)
   2. Calculate the area = heights[j] * (n - stack[-1] - 1 if stack else n)
   3. Update `max_area = max(max_area, area)`
4. Return `max_area`

### **Step 3: Implementation (Optimized)**

```python
def largestRectangleArea(heights):
    n = len(heights)
    stack = []  # Stack to store indices of bars
    max_area = 0
    
    for i in range(n):
        # While the current bar is shorter than the bar at the top of the stack
        while stack and heights[i] < heights[stack[-1]]:
            # Pop the top of the stack
            height = heights[stack.pop()]
            
            # Calculate the width
            width = i - stack[-1] - 1 if stack else i
            
            # Calculate the area and update max_area
            max_area = max(max_area, height * width)
        
        # Push the current index onto the stack
        stack.append(i)
    
    # Process any remaining bars in the stack
    while stack:
        height = heights[stack.pop()]
        width = n - stack[-1] - 1 if stack else n
        max_area = max(max_area, height * width)
    
    return max_area
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. Each bar is pushed and popped from the stack at most once.
* Space Complexity: O(n) in the worst case, where the stack can grow up to the size of the input.
* This is much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For heights = [2,1,5,6,2,3]:
  * i=0 (height=2): stack = [0]
  * i=1 (height=1): 1 < 2, pop 0, height = 2, width = 1, area = 2, stack = [], push 1, stack = [1]
  * i=2 (height=5): 5 > 1, push 2, stack = [1,2]
  * i=3 (height=6): 6 > 5, push 3, stack = [1,2,3]
  * i=4 (height=2): 2 < 6, pop 3, height = 6, width = 1, area = 6, stack = [1,2]
    * 2 < 5, pop 2, height = 5, width = 2, area = 10, stack = [1]
    * 2 > 1, push 4, stack = [1,4]
  * i=5 (height=3): 3 > 2, push 5, stack = [1,4,5]
  * After iteration, process remaining stack:
    * Pop 5, height = 3, width = 1, area = 3
    * Pop 4, height = 2, width = 3, area = 6
    * Pop 1, height = 1, width = 6, area = 6
  * max_area = 10
* We're efficiently finding the largest rectangle that each bar can form by using a stack to track the left and right boundaries.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Considers all possible pairs of bars |
| Optimized (Algorithm: Stack) | O(n) | O(n) | Uses a stack to find the largest rectangle for each bar |

---

## **Final Takeaways**

* Using a stack allows us to efficiently find the largest rectangle that each bar can form.
* The key insight is to maintain a monotonic increasing stack of bar indices, which helps us find the left and right boundaries for each bar.
* This problem demonstrates how using the right data structure can lead to significant efficiency improvements.

---

## **Real-life Analogy**

* Imagine you're trying to find the largest rectangular table that can fit under an irregularly shaped ceiling. The brute force approach would be like measuring the area of every possible rectangular region under the ceiling. The optimized approach is like standing at each point and looking left and right to find how far you can extend a table of that height - by processing the ceiling heights in a specific order, you can efficiently find the maximum possible table size without checking every possible configuration.

---

## **Summary**

* We solved the "Largest Rectangle in Histogram" problem by first considering a brute force approach that checks all possible pairs of bars, resulting in O(n²) time complexity. We then optimized using a stack-based approach that finds the largest rectangle for each bar by maintaining a monotonic increasing stack of bar indices, reducing the time complexity to O(n) at the cost of O(n) space complexity. The key insight is to find the left and right boundaries for each bar where it is the minimum height, which allows us to calculate the largest rectangle that includes that bar. 