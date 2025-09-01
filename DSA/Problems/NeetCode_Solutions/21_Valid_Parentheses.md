# 📝 Valid Parentheses

## **Problem Statement**

* Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.
* An input string is valid if:
  1. Open brackets must be closed by the same type of brackets.
  2. Open brackets must be closed in the correct order.
  3. Every close bracket has a corresponding open bracket of the same type.
* Example:
  * Input: s = "()[]{}"
  * Output: true
  * Explanation: The brackets are properly matched and closed.
* Constraints:
  * 1 <= s.length <= 10^4
  * s consists of parentheses only '()[]{}'.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to check if the parentheses in the string are valid
* For a string to be valid, each closing bracket must match the most recently opened bracket of the same type
* This suggests a Last-In-First-Out (LIFO) behavior, which is characteristic of a stack
* We can iterate through the string, and for each character:
  * If it's an opening bracket, push it onto the stack
  * If it's a closing bracket, check if it matches the top of the stack
* If at the end the stack is empty, the string is valid

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty stack
2. For each character in the string:
   1. If it's an opening bracket ('(', '{', '['), push it onto the stack
   2. If it's a closing bracket (')', '}', ']'):
      1. If the stack is empty, return false (no matching opening bracket)
      2. Pop the top element from the stack
      3. If the popped element doesn't match the current closing bracket, return false
3. After processing all characters, if the stack is empty, return true; otherwise, return false

---

## **Step 3: Brute Force Implementation (Code)**

```python
def isValid(s):
    stack = []
    
    for char in s:
        if char in '({[':
            stack.append(char)
        else:  # char is ')', '}', or ']'
            if not stack:
                return False
            
            top = stack.pop()
            
            if (char == ')' and top != '(') or \
               (char == '}' and top != '{') or \
               (char == ']' and top != '['):
                return False
    
    return len(stack) == 0
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n), where n is the length of the string. We iterate through the string once.
* Space Complexity: O(n) in the worst case (e.g., for a string with all opening brackets).
* This approach is actually optimal for this problem, as we need to check each character exactly once.

---

## **Step 5: Visualizing Redundant Computation**

* For string "()[]{}":
  * Process '(': stack = ['(']
  * Process ')': pop '(', stack = []
  * Process '[': stack = ['[']
  * Process ']': pop '[', stack = []
  * Process '{': stack = ['{']
  * Process '}': pop '{', stack = []
  * Stack is empty, so return true
* There's no redundant computation in this approach. Each character is processed exactly once, and the stack operations are all O(1).

---

## **Why are we using this algorithm?**

* We're using a Stack data structure for this problem.
* The property that matches this problem is the Last-In-First-Out (LIFO) behavior of brackets.
* A stack is perfect for tracking the most recently opened bracket that needs to be closed next.
* Alternatives like using counters would work for simple cases but fail for nested brackets.
* Precondition: We need to match brackets in the reverse order they were opened.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* The brute force approach is already optimal in terms of time complexity
* However, we can make a small optimization by using a hash map to store the mapping between opening and closing brackets
* This makes the code more concise and easier to extend if we need to support more types of brackets

### **Step 2: Flow Steps (Optimized)**

1. Initialize a hash map that maps closing brackets to their corresponding opening brackets
2. Initialize an empty stack
3. For each character in the string:
   1. If it's an opening bracket, push it onto the stack
   2. If it's a closing bracket:
      1. If the stack is empty, return false
      2. Pop the top element from the stack
      3. If the popped element doesn't match the expected opening bracket for the current closing bracket, return false
4. After processing all characters, if the stack is empty, return true; otherwise, return false

### **Step 3: Implementation (Optimized)**

```python
def isValid(s):
    # Map closing brackets to their corresponding opening brackets
    bracket_map = {')': '(', '}': '{', ']': '['}
    
    stack = []
    
    for char in s:
        if char not in bracket_map:  # It's an opening bracket
            stack.append(char)
        else:  # It's a closing bracket
            if not stack or stack.pop() != bracket_map[char]:
                return False
    
    return len(stack) == 0
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n), where n is the length of the string. We iterate through the string once.
* Space Complexity: O(n) in the worst case (e.g., for a string with all opening brackets).
* The time and space complexity remain the same as the brute force approach, but the code is more concise and easier to maintain.

### **Step 5: Visualizing Computation (Optimized)**

* For string "()[]{}":
  * bracket_map = {')': '(', '}': '{', ']': '['}
  * Process '(': It's an opening bracket, stack = ['(']
  * Process ')': It's a closing bracket, pop '(', check bracket_map[')'] = '(', match, stack = []
  * Process '[': It's an opening bracket, stack = ['[']
  * Process ']': It's a closing bracket, pop '[', check bracket_map[']'] = '[', match, stack = []
  * Process '{': It's an opening bracket, stack = ['{']
  * Process '}': It's a closing bracket, pop '{', check bracket_map['}'] = '{', match, stack = []
  * Stack is empty, so return true
* The computation is the same as before, but the code is more concise and easier to extend.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n) | O(n) | Uses a stack to track opening brackets |
| Optimized | O(n) | O(n) | Uses a stack and a hash map for bracket matching |

---

## **Final Takeaways**

* Using a stack is the perfect approach for problems involving matching pairs in a Last-In-First-Out (LIFO) order.
* The key insight is that the most recently opened bracket must be closed first.
* This problem demonstrates how understanding the structure of the problem can lead to a simple and efficient solution.

---

## **Real-life Analogy**

* Imagine you're packing nested boxes. When you open a new box, you place it on top of a stack. When you're ready to close boxes, you must close the most recently opened box first (the one on top of the stack). If you try to close a box that doesn't match the one on top of the stack, or if you have boxes left open at the end, something has gone wrong with your packing.

---

## **Summary**

* We solved the "Valid Parentheses" problem using a stack to track opening brackets and ensure they are properly matched with closing brackets. The time complexity is O(n) and the space complexity is O(n), which is optimal for this problem. We optimized the solution slightly by using a hash map to store the mapping between opening and closing brackets, making the code more concise and easier to extend. The key insight is that brackets must be closed in the reverse order they were opened, which aligns perfectly with the Last-In-First-Out (LIFO) behavior of a stack. 