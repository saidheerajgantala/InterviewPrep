# 📝 Letter Combinations of a Phone Number

## **Problem Statement**

* Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.
* A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.
  * 2: "abc"
  * 3: "def"
  * 4: "ghi"
  * 5: "jkl"
  * 6: "mno"
  * 7: "pqrs"
  * 8: "tuv"
  * 9: "wxyz"
* Example 1:
  * Input: digits = "23"
  * Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
* Example 2:
  * Input: digits = ""
  * Output: []
* Example 3:
  * Input: digits = "2"
  * Output: ["a","b","c"]
* Constraints:
  * 0 <= digits.length <= 4
  * digits[i] is a digit in the range ['2', '9'].

---

## **Step 1: Thinking Process (Brute Force)**

* We need to generate all possible letter combinations for the given digits
* Each digit maps to a set of letters, and we need to consider all combinations
* One approach would be to use backtracking to generate all combinations
* For each digit, we try all possible letters and recursively generate combinations for the remaining digits

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a mapping from digits to letters
2. Initialize an empty result list
3. Define a backtracking function that takes the current combination and the remaining digits:
   1. If there are no remaining digits, add the current combination to the result list
   2. Otherwise, for each letter mapped to the first digit of the remaining digits:
      1. Add the letter to the current combination
      2. Recursively call the backtracking function with the updated combination and the remaining digits
      3. Remove the letter from the current combination (backtrack)
4. Call the backtracking function with an empty combination and all digits
5. Return the result list

---

## **Step 3: Brute Force Implementation (Code)**

```python
def letterCombinations(digits):
    if not digits:
        return []
    
    # Mapping from digits to letters
    mapping = {
        '2': 'abc',
        '3': 'def',
        '4': 'ghi',
        '5': 'jkl',
        '6': 'mno',
        '7': 'pqrs',
        '8': 'tuv',
        '9': 'wxyz'
    }
    
    result = []
    
    def backtrack(index, current):
        if index == len(digits):
            result.append(''.join(current))
            return
        
        for letter in mapping[digits[index]]:
            current.append(letter)
            backtrack(index + 1, current)
            current.pop()
    
    backtrack(0, [])
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(4^n) where n is the number of digits. In the worst case, each digit maps to 4 letters (e.g., digit 7 maps to "pqrs"), and we need to generate all combinations.
* Space Complexity: O(n) for the recursion stack and to store each combination.
* This approach is actually quite efficient for this problem, as we're using backtracking to generate all combinations without redundant computation.

---

## **Step 5: Visualizing Redundant Computation**

* For the example digits = "23":
  * Start with an empty combination: ""
  * For digit "2", try "a": "a"
    * For digit "3", try "d": "ad"
      * No more digits, add to result: "ad"
    * For digit "3", try "e": "ae"
      * No more digits, add to result: "ae"
    * For digit "3", try "f": "af"
      * No more digits, add to result: "af"
  * For digit "2", try "b": "b"
    * For digit "3", try "d": "bd"
      * No more digits, add to result: "bd"
    * For digit "3", try "e": "be"
      * No more digits, add to result: "be"
    * For digit "3", try "f": "bf"
      * No more digits, add to result: "bf"
  * For digit "2", try "c": "c"
    * For digit "3", try "d": "cd"
      * No more digits, add to result: "cd"
    * For digit "3", try "e": "ce"
      * No more digits, add to result: "ce"
    * For digit "3", try "f": "cf"
      * No more digits, add to result: "cf"
* Result: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
* There's no redundant computation in this approach, as we generate each combination exactly once

---

## **Why are we using this algorithm?**

* For this problem, the backtracking approach is already quite efficient.
* The property that matches this problem is that we need to generate all possible combinations of letters for the given digits.
* By using backtracking, we can efficiently generate all combinations without redundant computation.
* Alternatives would be equally efficient or more complex.
* Precondition: We need to have a mapping from digits to letters.

---

## **Algo optimization**

While the backtracking approach is already efficient, we can consider an iterative approach using a queue:

### **Step 1: Thinking Process (Optimized - Iterative)**

* We can use an iterative approach with a queue to generate all combinations
* Start with an empty string in the queue
* For each digit, dequeue all current combinations, append each possible letter for the current digit, and enqueue the new combinations
* After processing all digits, the queue will contain all possible letter combinations

### **Step 2: Flow Steps (Optimized - Iterative)**

1. Create a mapping from digits to letters
2. Initialize a queue with an empty string
3. For each digit:
   1. Get the letters mapped to the current digit
   2. Dequeue all current combinations
   3. For each combination and each letter, create a new combination and enqueue it
4. Return the queue as the result list

### **Step 3: Implementation (Optimized - Iterative)**

```python
from collections import deque

def letterCombinations(digits):
    if not digits:
        return []
    
    # Mapping from digits to letters
    mapping = {
        '2': 'abc',
        '3': 'def',
        '4': 'ghi',
        '5': 'jkl',
        '6': 'mno',
        '7': 'pqrs',
        '8': 'tuv',
        '9': 'wxyz'
    }
    
    queue = deque([""])
    
    for digit in digits:
        letters = mapping[digit]
        size = len(queue)
        
        for _ in range(size):
            current = queue.popleft()
            for letter in letters:
                queue.append(current + letter)
    
    return list(queue)
```

### **Step 4: Complexity Analysis (Optimized - Iterative)**

* Time Complexity: O(4^n) where n is the number of digits. In the worst case, each digit maps to 4 letters, and we need to generate all combinations.
* Space Complexity: O(4^n) for storing all combinations in the queue.
* This approach has the same time complexity as the backtracking approach, but it's iterative rather than recursive.

### **Step 5: Visualizing Computation (Optimized - Iterative)**

* For the example digits = "23":
  * Initialize queue: [""]
  * Process digit "2":
    * Dequeue "": ""
    * Enqueue "a", "b", "c": ["a", "b", "c"]
  * Process digit "3":
    * Dequeue "a": "a"
    * Enqueue "ad", "ae", "af": ["b", "c", "ad", "ae", "af"]
    * Dequeue "b": "b"
    * Enqueue "bd", "be", "bf": ["c", "ad", "ae", "af", "bd", "be", "bf"]
    * Dequeue "c": "c"
    * Enqueue "cd", "ce", "cf": ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
  * Result: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
* The iterative approach efficiently generates all combinations by processing one digit at a time

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Backtracking) | O(4^n) | O(n) | Efficient with recursion |
| Optimized (Iterative) | O(4^n) | O(4^n) | Efficient with iteration |

---

## **Final Takeaways**

* The key to efficiently solving the Letter Combinations of a Phone Number problem is to generate all possible combinations of letters for the given digits.
* Both the backtracking and iterative approaches have the same time complexity, but the backtracking approach uses less space.
* The choice between the two approaches depends on personal preference and the specific requirements of the problem.
* This problem demonstrates the power of backtracking and iterative approaches for generating all possible combinations.

---

## **Real-life Analogy**

* Imagine you're trying to guess a phone password that consists of letters instead of numbers. You know which numbers were pressed, but each number can represent multiple letters. The backtracking approach would be like systematically trying all possible letter combinations for the given numbers, one digit at a time. The iterative approach would be like maintaining a list of all possible combinations so far, and for each new digit, generating all new combinations by appending each possible letter to each existing combination.

---

## **Summary**

* We solved the Letter Combinations of a Phone Number problem by first considering a backtracking approach that recursively generates all combinations, resulting in O(4^n) time complexity. We then explored an iterative approach using a queue, which has the same time complexity but is iterative rather than recursive. Both approaches efficiently generate all possible letter combinations for the given digits, and the choice between them depends on personal preference and the specific requirements of the problem. 