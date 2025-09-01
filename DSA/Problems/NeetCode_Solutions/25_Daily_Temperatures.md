# 📝 Daily Temperatures

## **Problem Statement**

* Given an array of integers `temperatures` representing daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the ith day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0`.
* Example:
  * Input: temperatures = [73,74,75,71,69,72,76,73]
  * Output: [1,1,4,2,1,1,0,0]
  * Explanation: 
    * For temperatures[0] = 73, the next warmer temperature is temperatures[1] = 74, so answer[0] = 1.
    * For temperatures[1] = 74, the next warmer temperature is temperatures[2] = 75, so answer[1] = 1.
    * For temperatures[2] = 75, the next warmer temperature is temperatures[6] = 76, so answer[2] = 4.
    * And so on...
* Constraints:
  * 1 <= temperatures.length <= 10^5
  * 30 <= temperatures[i] <= 100

---

## **Step 1: Thinking Process (Brute Force)**

* For each day, we need to find the next day with a higher temperature
* The most straightforward approach is to use a nested loop
* For each day i, we iterate through the future days j > i until we find a day with a higher temperature
* If we find such a day, we set answer[i] = j - i (the number of days to wait)
* If we don't find a warmer day, we set answer[i] = 0

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an answer array of the same length as temperatures, filled with 0s
2. For each index i from 0 to n-1:
   1. For each index j from i+1 to n-1:
      1. If temperatures[j] > temperatures[i]:
         1. Set answer[i] = j - i
         2. Break the inner loop
3. Return the answer array

---

## **Step 3: Brute Force Implementation (Code)**

```python
def dailyTemperatures(temperatures):
    n = len(temperatures)
    answer = [0] * n
    
    for i in range(n):
        for j in range(i + 1, n):
            if temperatures[j] > temperatures[i]:
                answer[i] = j - i
                break
    
    return answer
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the length of the array. In the worst case, we might need to check all future days for each day.
* Space Complexity: O(1) extra space (not counting the output array).
* This approach is inefficient for large arrays and will likely result in a time limit exceeded error.

---

## **Step 5: Visualizing Redundant Computation**

* For temperatures = [73,74,75,71,69,72,76,73]:
  * For i=0 (73), we check j=1 (74) and find a warmer day immediately
  * For i=1 (74), we check j=2 (75) and find a warmer day immediately
  * For i=2 (75), we check j=3 (71), j=4 (69), j=5 (72), j=6 (76) and find a warmer day
  * ...and so on
* We're repeatedly checking the same temperatures multiple times
* For example, when processing i=2, we check temperatures[3], temperatures[4], etc.
* Then when processing i=3, we check temperatures[4], temperatures[5], etc. again

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Stack data structure.
* The property that matches this problem is the ability to efficiently find the next greater element.
* By using a monotonic decreasing stack, we can process the array in reverse order and efficiently find the next warmer day.
* Alternatives like using a heap would be less efficient for this specific problem.
* Precondition: We need to efficiently track days with temperatures in decreasing order.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of using nested loops, we can use a stack to keep track of days with temperatures in decreasing order
* We process the array from right to left (or from left to right with a different approach)
* For each day, we pop days from the stack until we find a day with a higher temperature
* This way, we only process each day once, leading to a linear time complexity

### **Step 2: Flow Steps (Optimized)**

1. Initialize an answer array of the same length as temperatures, filled with 0s
2. Initialize an empty stack to store indices of days
3. For each index i from 0 to n-1:
   1. While the stack is not empty and the current temperature is greater than the temperature at the index at the top of the stack:
      1. Pop an index j from the stack
      2. Set answer[j] = i - j (the number of days to wait)
   2. Push the current index i onto the stack
4. Return the answer array

### **Step 3: Implementation (Optimized)**

```python
def dailyTemperatures(temperatures):
    n = len(temperatures)
    answer = [0] * n
    stack = []  # Stack to store indices of days
    
    for i in range(n):
        # Pop indices from the stack while the current temperature is greater
        while stack and temperatures[i] > temperatures[stack[-1]]:
            j = stack.pop()
            answer[j] = i - j
        
        # Push the current index onto the stack
        stack.append(i)
    
    return answer
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. Each index is pushed and popped from the stack at most once.
* Space Complexity: O(n) in the worst case, where the stack can grow up to the size of the input.
* This is much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For temperatures = [73,74,75,71,69,72,76,73]:
  * i=0 (73): stack = [0]
  * i=1 (74): 74 > 73, pop 0, answer[0] = 1-0 = 1, stack = [1]
  * i=2 (75): 75 > 74, pop 1, answer[1] = 2-1 = 1, stack = [2]
  * i=3 (71): 71 < 75, stack = [2,3]
  * i=4 (69): 69 < 71, stack = [2,3,4]
  * i=5 (72): 72 > 69, pop 4, answer[4] = 5-4 = 1, 72 > 71, pop 3, answer[3] = 5-3 = 2, 72 < 75, stack = [2,5]
  * i=6 (76): 76 > 72, pop 5, answer[5] = 6-5 = 1, 76 > 75, pop 2, answer[2] = 6-2 = 4, stack = [6]
  * i=7 (73): 73 < 76, stack = [6,7]
  * Final answer = [1,1,4,2,1,1,0,0]
* We're efficiently finding the next warmer day for each day in a single pass through the array.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Checks all future days for each day |
| Optimized (Algorithm: Stack) | O(n) | O(n) | Uses a stack to track days with temperatures in decreasing order |

---

## **Final Takeaways**

* Using a stack allows us to efficiently find the next greater element in an array.
* The key insight is to maintain a monotonic decreasing stack of indices, which allows us to process each element only once.
* This problem demonstrates how using the right data structure can lead to significant efficiency improvements.

---

## **Real-life Analogy**

* Imagine you're standing in a line of people arranged by height, and you want to find the next taller person for each person. The brute force approach would be like having each person look at everyone in front of them until they find someone taller. The optimized approach is like having people stand on each other's shoulders in decreasing height order - when a new person arrives, anyone shorter than them gets to see the new person as their next taller person, and then the new person joins the stack.

---

## **Summary**

* We solved the "Daily Temperatures" problem by first considering a brute force approach that checks all future days for each day, resulting in O(n²) time complexity. We then optimized using a stack to keep track of days with temperatures in decreasing order, reducing the time complexity to O(n) at the cost of O(n) space complexity. The key insight is to maintain a monotonic decreasing stack of indices, which allows us to process each element only once and efficiently find the next warmer day for each day. 