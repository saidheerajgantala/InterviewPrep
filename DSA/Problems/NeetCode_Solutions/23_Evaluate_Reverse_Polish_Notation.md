# 📝 Evaluate Reverse Polish Notation

## **Problem Statement**

* You are given an array of strings `tokens` that represents an arithmetic expression in a Reverse Polish Notation (RPN).
* Evaluate the expression. Return an integer that represents the value of the expression.
* Note that:
  * The valid operators are '+', '-', '*', and '/'.
  * Each operand may be an integer or another expression.
  * The division between two integers always truncates toward zero.
  * There will not be any division by zero.
  * The input represents a valid arithmetic expression in a reverse polish notation.
  * The answer and all the intermediate calculations can be represented in a 32-bit integer.
* Example:
  * Input: tokens = ["2","1","+","3","*"]
  * Output: 9
  * Explanation: ((2 + 1) * 3) = 9
* Constraints:
  * 1 <= tokens.length <= 10^4
  * tokens[i] is either an operator: "+", "-", "*", or "/", or an integer in the range [-200, 200].

---

## **Step 1: Thinking Process (Brute Force)**

* Reverse Polish Notation (RPN) is a mathematical notation where every operator follows all of its operands
* To evaluate an RPN expression, we need to process the tokens from left to right:
  * If the token is an operand (number), push it onto a stack
  * If the token is an operator, pop the required number of operands from the stack, perform the operation, and push the result back onto the stack
* After processing all tokens, the stack should contain only one element, which is the final result
* This approach is actually optimal for evaluating RPN expressions

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty stack
2. For each token in the input array:
   1. If the token is a number, convert it to an integer and push it onto the stack
   2. If the token is an operator:
      1. Pop the top two elements from the stack (the second operand first, then the first operand)
      2. Perform the operation
      3. Push the result back onto the stack
3. Return the top element of the stack as the final result

---

## **Step 3: Brute Force Implementation (Code)**

```python
def evalRPN(tokens):
    stack = []
    
    for token in tokens:
        if token in ["+", "-", "*", "/"]:
            # Pop the top two elements
            b = stack.pop()
            a = stack.pop()
            
            # Perform the operation
            if token == "+":
                stack.append(a + b)
            elif token == "-":
                stack.append(a - b)
            elif token == "*":
                stack.append(a * b)
            elif token == "/":
                # Integer division that truncates toward zero
                stack.append(int(a / b))
        else:
            # Convert the token to an integer and push onto the stack
            stack.append(int(token))
    
    # The final result should be the only element in the stack
    return stack[0]
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n), where n is the number of tokens. We process each token exactly once.
* Space Complexity: O(n) in the worst case, where the stack can grow up to the size of the input.
* This approach is actually optimal for evaluating RPN expressions, as we need to process each token exactly once and maintain a stack to keep track of operands.

---

## **Step 5: Visualizing Redundant Computation**

* For tokens = ["2","1","+","3","*"]:
  * Process "2": stack = [2]
  * Process "1": stack = [2, 1]
  * Process "+": pop 1 and 2, push (2+1) = 3, stack = [3]
  * Process "3": stack = [3, 3]
  * Process "*": pop 3 and 3, push (3*3) = 9, stack = [9]
  * Return 9
* There's no redundant computation in this approach. Each token is processed exactly once, and the stack operations are all O(1).

---

## **Why are we using this algorithm?**

* We're using a Stack data structure for this problem.
* The property that matches this problem is the Last-In-First-Out (LIFO) behavior needed for operator evaluation.
* A stack is perfect for keeping track of operands and applying operators in the correct order.
* Alternatives like using recursion would be more complex and less efficient.
* Precondition: The input represents a valid RPN expression.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* The brute force approach is already optimal in terms of time and space complexity
* However, we can make some minor optimizations:
  * Use a dictionary to map operators to their corresponding operations
  * Handle edge cases more robustly

### **Step 2: Flow Steps (Optimized)**

1. Initialize an empty stack and a dictionary mapping operators to their operations
2. For each token in the input array:
   1. If the token is an operator:
      1. Pop the top two elements from the stack
      2. Perform the operation using the dictionary mapping
      3. Push the result back onto the stack
   2. Otherwise, convert the token to an integer and push it onto the stack
3. Return the top element of the stack as the final result

### **Step 3: Implementation (Optimized)**

```python
def evalRPN(tokens):
    stack = []
    operations = {
        "+": lambda a, b: a + b,
        "-": lambda a, b: a - b,
        "*": lambda a, b: a * b,
        "/": lambda a, b: int(a / b)  # Integer division that truncates toward zero
    }
    
    for token in tokens:
        if token in operations:
            b = stack.pop()
            a = stack.pop()
            stack.append(operations[token](a, b))
        else:
            stack.append(int(token))
    
    return stack[0]
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n), where n is the number of tokens. We process each token exactly once.
* Space Complexity: O(n) in the worst case, where the stack can grow up to the size of the input.
* The optimized approach has the same time and space complexity as the brute force approach, but the code is more concise and easier to maintain.

### **Step 5: Visualizing Computation (Optimized)**

* For tokens = ["2","1","+","3","*"]:
  * Process "2": stack = [2]
  * Process "1": stack = [2, 1]
  * Process "+": pop 1 and 2, push operations["+"](2, 1) = 3, stack = [3]
  * Process "3": stack = [3, 3]
  * Process "*": pop 3 and 3, push operations["*"](3, 3) = 9, stack = [9]
  * Return 9
* The computation is the same as before, but the code is more concise and easier to extend.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n) | O(n) | Uses a stack to evaluate RPN expression |
| Optimized | O(n) | O(n) | Uses a stack and a dictionary for operations |

---

## **Final Takeaways**

* Using a stack is the perfect approach for evaluating expressions in Reverse Polish Notation.
* The key insight is that RPN expressions are designed to be evaluated using a stack-based approach.
* This problem demonstrates how understanding the structure of the problem can lead to a simple and efficient solution.

---

## **Real-life Analogy**

* Imagine you're following a recipe that lists all the ingredients first, followed by the steps to combine them. The RPN evaluation is like having a row of ingredients (operands) and instructions (operators). You pick up ingredients from left to right and place them on your counter (stack). When you encounter an instruction, you take the most recently placed ingredients, combine them according to the instruction, and place the result back on the counter. At the end, you're left with the final dish (result).

---

## **Summary**

* We solved the "Evaluate Reverse Polish Notation" problem using a stack to process tokens from left to right. For operands, we push them onto the stack; for operators, we pop the required operands, perform the operation, and push the result back. This approach has O(n) time complexity and O(n) space complexity, which is optimal for evaluating RPN expressions. We optimized the solution slightly by using a dictionary to map operators to their corresponding operations, making the code more concise and easier to maintain. The key insight is that RPN expressions are designed to be evaluated using a stack-based approach, which aligns perfectly with the Last-In-First-Out (LIFO) behavior of a stack. 