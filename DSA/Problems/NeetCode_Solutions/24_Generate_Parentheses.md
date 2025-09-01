# 📝 Generate Parentheses

## **Problem Statement**

* Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
* Example:
  * Input: n = 3
  * Output: ["((()))","(()())","(())()","()(())","()()()"]
* Constraints:
  * 1 <= n <= 8

---

## **Step 1: Thinking Process (Brute Force)**

* We need to generate all valid combinations of n pairs of parentheses
* A valid combination means that every opening parenthesis '(' has a matching closing parenthesis ')', and they are properly nested
* One approach is to generate all possible combinations of '(' and ')' of length 2n, and then filter out the invalid ones
* However, this would be very inefficient as there are 2^(2n) possible combinations, and most of them would be invalid

---

## **Step 2: Flow Steps (Brute Force)**

1. Generate all possible combinations of '(' and ')' of length 2n
2. For each combination, check if it's a valid parentheses string
3. If valid, add it to the result list
4. Return the result list

---

## **Step 3: Brute Force Implementation (Code)**

```python
def generateParenthesis(n):
    def is_valid(s):
        # Check if the parentheses string is valid
        count = 0
        for char in s:
            if char == '(':
                count += 1
            else:  # char == ')'
                count -= 1
                if count < 0:
                    return False
        return count == 0
    
    def generate_all(current, length):
        # Generate all possible combinations of '(' and ')'
        if length == 0:
            if is_valid(current):
                result.append(current)
            return
        
        generate_all(current + '(', length - 1)
        generate_all(current + ')', length - 1)
    
    result = []
    generate_all('', 2 * n)
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^(2n) * n), where n is the input parameter.
  * There are 2^(2n) possible combinations of '(' and ')'
  * For each combination, we spend O(n) time checking if it's valid
* Space Complexity: O(n * 2^(2n)) for storing all valid combinations
* This approach is extremely inefficient for large values of n

---

## **Step 5: Visualizing Redundant Computation**

* For n = 2, there are 2^4 = 16 possible combinations of '(' and ')':
  * "((((", "((())", "(()(", "(()*", "(())", "(()*(", "()((", "()())", "())(", "())*", ")(()", ")()(", "))()", "))((", ")))(", "))))"
  * Only 2 of these are valid: "(())" and "()()"
* We're generating and checking many invalid combinations
* We can be smarter about our generation process to avoid invalid combinations altogether

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Backtracking algorithm.
* The property that matches this problem is the ability to build valid parentheses strings incrementally.
* By keeping track of the number of opening and closing parentheses used so far, we can ensure that we only generate valid combinations.
* Alternatives like dynamic programming would be more complex for this specific problem.
* Precondition: We need to ensure that at each step, the number of closing parentheses does not exceed the number of opening parentheses.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of generating all possible combinations and filtering out the invalid ones, we can generate only valid combinations
* We can use backtracking to build the combinations incrementally
* At each step, we have two choices:
  * Add an opening parenthesis '(' if we haven't used all n opening parentheses
  * Add a closing parenthesis ')' if the number of closing parentheses used so far is less than the number of opening parentheses
* This way, we ensure that we only generate valid combinations

### **Step 2: Flow Steps (Optimized)**

1. Initialize an empty result list
2. Define a recursive backtracking function that takes:
   1. The current string being built
   2. The number of opening parentheses used so far
   3. The number of closing parentheses used so far
3. In the backtracking function:
   1. If the current string has length 2n, add it to the result list and return
   2. If we can add an opening parenthesis (open < n), recursively call the function with an added '('
   3. If we can add a closing parenthesis (close < open), recursively call the function with an added ')'
4. Call the backtracking function with an empty string and both counts at 0
5. Return the result list

### **Step 3: Implementation (Optimized)**

```python
def generateParenthesis(n):
    result = []
    
    def backtrack(current, open_count, close_count):
        # If the current string has length 2n, it's complete
        if len(current) == 2 * n:
            result.append(current)
            return
        
        # Add an opening parenthesis if we haven't used all n
        if open_count < n:
            backtrack(current + '(', open_count + 1, close_count)
        
        # Add a closing parenthesis if it's valid (more open than close)
        if close_count < open_count:
            backtrack(current + ')', open_count, close_count + 1)
    
    # Start the backtracking with an empty string
    backtrack('', 0, 0)
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(4^n / sqrt(n)), which is the nth Catalan number (the number of valid parentheses combinations).
* Space Complexity: O(n * 4^n / sqrt(n)) for storing all valid combinations and the recursion stack.
* This is much more efficient than the brute force approach, as we only generate valid combinations.

### **Step 5: Visualizing Computation (Optimized)**

* For n = 2:
  * Start with current = '', open = 0, close = 0
  * Add '(': current = '(', open = 1, close = 0
    * Add '(': current = '((', open = 2, close = 0
      * Add ')': current = '(()', open = 2, close = 1
        * Add ')': current = '(())', open = 2, close = 2 (complete, add to result)
    * Add ')': current = '()', open = 1, close = 1
      * Add '(': current = '()(', open = 2, close = 1
        * Add ')': current = '()()', open = 2, close = 2 (complete, add to result)
  * Result: ["(())", "()()"]
* We only generate valid combinations, avoiding the exponential waste of the brute force approach.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(2^(2n) * n) | O(n * 2^(2n)) | Generates all combinations and filters |
| Optimized (Algorithm: Backtracking) | O(4^n / sqrt(n)) | O(n * 4^n / sqrt(n)) | Generates only valid combinations |

---

## **Final Takeaways**

* Using backtracking allows us to efficiently generate only valid combinations.
* The key insight is to maintain the invariant that the number of closing parentheses never exceeds the number of opening parentheses.
* This problem demonstrates how understanding the constraints of the problem can lead to significant efficiency improvements.

---

## **Real-life Analogy**

* Imagine you're building a tower with blocks, where each level must have a solid foundation below it. The brute force approach would be like trying all possible arrangements of blocks and then checking if the tower stands. The optimized approach is like building the tower level by level, ensuring at each step that the structure remains stable - you only place a block if it has proper support, avoiding unstable configurations altogether.

---

## **Summary**

* We solved the "Generate Parentheses" problem by first considering a brute force approach that generates all possible combinations and filters out invalid ones, resulting in O(2^(2n) * n) time complexity. We then optimized using a backtracking approach that generates only valid combinations by maintaining the invariant that the number of closing parentheses never exceeds the number of opening parentheses, reducing the time complexity to O(4^n / sqrt(n)). This demonstrates how understanding the constraints of the problem can lead to significant efficiency improvements. 