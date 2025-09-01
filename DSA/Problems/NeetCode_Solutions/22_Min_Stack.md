# 📝 Min Stack

## **Problem Statement**

* Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
* Implement the `MinStack` class:
  * `MinStack()` initializes the stack object.
  * `void push(int val)` pushes the element val onto the stack.
  * `void pop()` removes the element on the top of the stack.
  * `int top()` gets the top element of the stack.
  * `int getMin()` retrieves the minimum element in the stack.
* You must implement a solution with O(1) time complexity for each function.
* Example:
  * Input: ["MinStack","push","push","push","getMin","pop","top","getMin"]
  * [[],[-2],[0],[-3],[],[],[],[]]
  * Output: [null,null,null,null,-3,null,0,-2]
  * Explanation:
    * MinStack minStack = new MinStack();
    * minStack.push(-2);
    * minStack.push(0);
    * minStack.push(-3);
    * minStack.getMin(); // return -3
    * minStack.pop();
    * minStack.top();    // return 0
    * minStack.getMin(); // return -2
* Constraints:
  * -2^31 <= val <= 2^31 - 1
  * Methods pop, top and getMin operations will always be called on non-empty stacks.
  * At most 3 * 10^4 calls will be made to push, pop, top, and getMin.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to design a stack that can retrieve the minimum element in constant time
* A regular stack supports push, pop, and top operations in O(1) time
* The challenge is to support getMin() in O(1) time as well
* A naive approach would be to scan the entire stack to find the minimum element each time getMin() is called
* However, this would be O(n) time, not O(1)

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty array to store the stack elements
2. For push(val):
   1. Append val to the end of the array
3. For pop():
   1. Remove and return the last element of the array
4. For top():
   1. Return the last element of the array
5. For getMin():
   1. Iterate through the entire array to find the minimum element
   2. Return the minimum element

---

## **Step 3: Brute Force Implementation (Code)**

```python
class MinStack:
    def __init__(self):
        self.stack = []
    
    def push(self, val):
        self.stack.append(val)
    
    def pop(self):
        if self.stack:
            self.stack.pop()
    
    def top(self):
        if self.stack:
            return self.stack[-1]
        return None
    
    def getMin(self):
        if self.stack:
            return min(self.stack)
        return None
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity:
  * push(): O(1)
  * pop(): O(1)
  * top(): O(1)
  * getMin(): O(n), where n is the number of elements in the stack
* Space Complexity: O(n) to store the stack elements
* This approach doesn't meet the requirement of O(1) time complexity for getMin()

---

## **Step 5: Visualizing Redundant Computation**

* Consider the sequence of operations in the example:
  * push(-2): stack = [-2]
  * push(0): stack = [-2, 0]
  * push(-3): stack = [-2, 0, -3]
  * getMin(): scan the entire stack to find min = -3
  * pop(): stack = [-2, 0]
  * top(): return 0
  * getMin(): scan the entire stack again to find min = -2
* We're repeatedly scanning the entire stack to find the minimum element
* This is redundant because the minimum element only changes when we push a new minimum or pop the current minimum

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Two-Stack approach or a Stack with Pairs.
* The property that matches this problem is the ability to track the minimum element at each state of the stack.
* By storing additional information with each element, we can retrieve the minimum in O(1) time.
* Alternatives like using a heap would be less efficient for this specific problem.
* Precondition: We need to efficiently track the minimum element as the stack changes.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of scanning the entire stack to find the minimum, we can keep track of the minimum element at each state of the stack
* One approach is to use an auxiliary stack (min_stack) that keeps track of the minimum element at each state
* When we push a new element, we also push the current minimum to the min_stack
* When we pop an element, we also pop from the min_stack
* This way, the top of the min_stack always gives us the current minimum in O(1) time

### **Step 2: Flow Steps (Optimized)**

1. Initialize two empty arrays: stack and min_stack
2. For push(val):
   1. Append val to stack
   2. If min_stack is empty or val <= min_stack[-1], append val to min_stack
   3. Otherwise, append min_stack[-1] to min_stack (duplicate the current minimum)
3. For pop():
   1. Remove the last element from both stack and min_stack
4. For top():
   1. Return the last element of stack
5. For getMin():
   1. Return the last element of min_stack

### **Step 3: Implementation (Optimized)**

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []
    
    def push(self, val):
        self.stack.append(val)
        
        # Update min_stack
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
        else:
            self.min_stack.append(self.min_stack[-1])
    
    def pop(self):
        if self.stack:
            self.stack.pop()
            self.min_stack.pop()
    
    def top(self):
        if self.stack:
            return self.stack[-1]
        return None
    
    def getMin(self):
        if self.min_stack:
            return self.min_stack[-1]
        return None
```

### **Alternative Implementation (Stack with Pairs)**

Another approach is to store pairs of (value, current_min) in a single stack:

```python
class MinStack:
    def __init__(self):
        self.stack = []
    
    def push(self, val):
        # If the stack is empty, the min is the current value
        # Otherwise, it's the minimum of the current value and the previous min
        current_min = val if not self.stack else min(val, self.stack[-1][1])
        self.stack.append((val, current_min))
    
    def pop(self):
        if self.stack:
            self.stack.pop()
    
    def top(self):
        if self.stack:
            return self.stack[-1][0]
        return None
    
    def getMin(self):
        if self.stack:
            return self.stack[-1][1]
        return None
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity:
  * push(): O(1)
  * pop(): O(1)
  * top(): O(1)
  * getMin(): O(1)
* Space Complexity: O(n) for both approaches
  * Two-Stack: We use two stacks, each storing n elements in the worst case
  * Stack with Pairs: We use one stack storing pairs, which is still O(n)
* Both optimized approaches meet the requirement of O(1) time complexity for all operations

### **Step 5: Visualizing Computation (Optimized)**

* Using the Two-Stack approach for the example:
  * push(-2): stack = [-2], min_stack = [-2]
  * push(0): stack = [-2, 0], min_stack = [-2, -2]
  * push(-3): stack = [-2, 0, -3], min_stack = [-2, -2, -3]
  * getMin(): return min_stack[-1] = -3
  * pop(): stack = [-2, 0], min_stack = [-2, -2]
  * top(): return stack[-1] = 0
  * getMin(): return min_stack[-1] = -2
* We're efficiently tracking the minimum element at each state of the stack without redundant computation

### **Complexity Summary**

| Approach | Time (push, pop, top, getMin) | Space | Notes |
|---|---|---|---|
| Brute Force | O(1), O(1), O(1), O(n) | O(n) | Scans the entire stack for getMin() |
| Two-Stack | O(1), O(1), O(1), O(1) | O(n) | Uses an auxiliary stack to track minimums |
| Stack with Pairs | O(1), O(1), O(1), O(1) | O(n) | Stores (value, min) pairs in a single stack |

---

## **Final Takeaways**

* Tracking additional information (the current minimum) with each stack element allows us to retrieve the minimum in O(1) time.
* The key insight is that the minimum element only changes when we push a new minimum or pop the current minimum.
* This problem demonstrates how using auxiliary data structures or storing additional information can lead to more efficient solutions.

---

## **Real-life Analogy**

* Imagine you're stacking books on a shelf, and you want to quickly know the thinnest book in the stack at any time. The brute force approach would be like measuring every book each time you need to find the thinnest one. The optimized approach is like keeping a separate note for each position in the stack, recording the thinnest book up to that position. When you add a new book, you simply compare it with the previous thinnest book and update your note. When you remove a book, you also remove the corresponding note. This way, you always know the thinnest book in the current stack without having to measure all books again.

---

## **Summary**

* We designed a "Min Stack" that supports push, pop, top, and getMin operations, all in O(1) time. We first considered a brute force approach that scans the entire stack to find the minimum, resulting in O(n) time for getMin(). We then optimized using either a two-stack approach or a stack with pairs, both of which track the minimum element at each state of the stack, reducing the time complexity of getMin() to O(1). The key insight is to store additional information with each element to efficiently track the minimum as the stack changes. 