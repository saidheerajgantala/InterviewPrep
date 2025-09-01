# 📝 Valid Parenthesis String

## **Problem Statement**

* Given a string `s` containing only three types of characters: '(', ')' and '*', return whether the input string is valid.
* An input string is valid if:
  * Open brackets must be closed by the corresponding closing brackets.
  * Open brackets must be closed in the correct order.
  * '*' could be treated as a single right parenthesis ')' or a single left parenthesis '(' or an empty string "".
* Example:
  * Input: s = "()"
  * Output: true
  * Input: s = "(*)"
  * Output: true
  * Input: s = "(*))"
  * Output: true
* Constraints:
  * 1 <= s.length <= 100
  * s[i] is '(', ')' or '*'.

---

## **Step 1: Thinking Process (Brute Force)**

* The '*' character introduces complexity because it can be treated as '(', ')' or an empty string.
* A naive approach would be to try all possible combinations of treating '*' as one of its three options.
* This would result in 3^k combinations, where k is the number of '*' characters.
* For each combination, we would check if the resulting string is valid.

---

## **Step 2: Flow Steps (Brute Force)**

1. Generate all possible strings by replacing each '*' with '(', ')' or an empty string.
2. For each generated string, check if it's a valid parenthesis string.
3. Return true if any of the generated strings is valid, false otherwise.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def checkValidString(s):
    def is_valid(s):
        balance = 0
        for char in s:
            if char == '(':
                balance += 1
            elif char == ')':
                balance -= 1
            if balance < 0:
                return False
        return balance == 0
    
    def generate_strings(index, current):
        if index == len(s):
            return is_valid(current)
        
        if s[index] == '*':
            # Try '*' as '('
            if generate_strings(index + 1, current + '('):
                return True
            # Try '*' as ')'
            if generate_strings(index + 1, current + ')'):
                return True
            # Try '*' as empty string
            if generate_strings(index + 1, current):
                return True
            return False
        else:
            return generate_strings(index + 1, current + s[index])
    
    return generate_strings(0, "")
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(3^k * n) where k is the number of '*' characters and n is the length of the string. We generate 3^k strings, and for each string, we spend O(n) time checking if it's valid.
* Space Complexity: O(n) for storing the current string and the recursion stack.
* This approach is extremely inefficient and will time out for large inputs.

---

## **Step 5: Visualizing Redundant Computation**

* For s = "(*))":
  * We would generate all possible strings:
    * "(())" - Valid
    * "())" - Invalid
    * "()" - Invalid
  * There are 3^1 = 3 possible strings to check.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a greedy approach with two passes.
* The key insight is to keep track of the range of possible values for the balance of open and close parentheses.
* We can do this by tracking the minimum and maximum possible balance as we scan through the string.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We'll keep track of the minimum and maximum possible balance of open parentheses.
* For '(', both the minimum and maximum balance increase by 1.
* For ')', both the minimum and maximum balance decrease by 1.
* For '*', the minimum balance decreases by 1 (treating '*' as ')'), and the maximum balance increases by 1 (treating '*' as '(').
* If at any point the maximum balance becomes negative, it means we have too many closing parentheses, and the string is invalid.
* After processing the entire string, if the minimum balance is 0, it means we can have a valid string by treating some '*' as '(' and others as ')' or empty strings.

### **Step 2: Flow Steps (Optimized)**

1. Initialize min_balance = 0, max_balance = 0.
2. Iterate through the string:
   1. If s[i] is '(', increment both min_balance and max_balance.
   2. If s[i] is ')', decrement both min_balance and max_balance.
   3. If s[i] is '*', decrement min_balance and increment max_balance.
   4. If max_balance becomes negative, return false.
   5. If min_balance becomes negative, reset it to 0 (we can treat some '*' as '(' to avoid negative balance).
3. Return true if min_balance is 0, false otherwise.

### **Step 3: Implementation (Optimized)**

```python
def checkValidString(s):
    min_balance = 0
    max_balance = 0
    
    for char in s:
        if char == '(':
            min_balance += 1
            max_balance += 1
        elif char == ')':
            min_balance -= 1
            max_balance -= 1
        else:  # char == '*'
            min_balance -= 1  # Treat '*' as ')'
            max_balance += 1  # Treat '*' as '('
        
        # If max_balance becomes negative, it means we have too many closing parentheses
        if max_balance < 0:
            return False
        
        # We can never have a negative balance, so reset min_balance to 0
        min_balance = max(0, min_balance)
    
    # If min_balance is 0, it means we can have a valid string
    return min_balance == 0
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the string. We only need to iterate through the string once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is much more efficient than the brute force approach.

### **Step 5: Visualizing Computation (Optimized)**

* For s = "(*))":
  * Start: min_balance = 0, max_balance = 0
  * Process '(': min_balance = 1, max_balance = 1
  * Process '*': min_balance = 1-1 = 0, max_balance = 1+1 = 2
  * Process ')': min_balance = 0-1 = -1 (reset to 0), max_balance = 2-1 = 1
  * Process ')': min_balance = 0-1 = -1 (reset to 0), max_balance = 1-1 = 0
  * Final: min_balance = 0, max_balance = 0, return true

* For s = "((":
  * Start: min_balance = 0, max_balance = 0
  * Process '(': min_balance = 1, max_balance = 1
  * Process '(': min_balance = 2, max_balance = 2
  * Final: min_balance = 2, max_balance = 2, return false

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(3^k * n) | O(n) | Tries all possible combinations of '*' |
| Optimized (Algorithm: Greedy) | O(n) | O(1) | Tracks the range of possible balances |

---

## **Final Takeaways**

* The Valid Parenthesis String problem is a good example of how a greedy approach can be much more efficient than brute force.
* The key insight is to keep track of the range of possible values for the balance of open and close parentheses.
* This problem demonstrates how understanding the constraints of a problem can lead to a more efficient algorithm.

---

## **Real-life Analogy**

* Imagine you're managing a warehouse where items can be either added or removed. Some operations are flexible (like '*' in our problem) and can either add an item, remove an item, or do nothing. To ensure the warehouse never has a negative number of items, you need to track the minimum and maximum possible number of items at each step.

---

## **Summary**

* We tackled the "Valid Parenthesis String" problem by tracking the minimum and maximum possible balance of open parentheses as we scan through the string. For '(', both the minimum and maximum balance increase by 1. For ')', both decrease by 1. For '*', the minimum balance decreases by 1 (treating '*' as ')'), and the maximum balance increases by 1 (treating '*' as '('). If at any point the maximum balance becomes negative, the string is invalid. After processing the entire string, if the minimum balance is 0, the string is valid. This greedy approach has a time complexity of O(n) where n is the length of the string, which is much more efficient than the brute force approach of trying all possible combinations of '*'. 