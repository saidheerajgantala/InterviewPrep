# 📝 Plus One

## **Problem Statement**

* You are given a large integer represented as an integer array `digits`, where each `digits[i]` is the ith digit of the integer.
* The digits are ordered from most significant to least significant in left-to-right order.
* The large integer does not contain any leading 0's.
* Increment the large integer by one and return the resulting array of digits.
* Example:
  * Input: digits = [1,2,3]
  * Output: [1,2,4] (123 + 1 = 124)
  * Input: digits = [4,3,2,1]
  * Output: [4,3,2,2] (4321 + 1 = 4322)
  * Input: digits = [9]
  * Output: [1,0] (9 + 1 = 10)
* Constraints:
  * 1 <= digits.length <= 100
  * 0 <= digits[i] <= 9
  * digits does not contain any leading 0's.

---

## **Step 1: Thinking Process (Brute Force)**

* To increment a large integer represented as an array of digits, we need to add 1 to the least significant digit (the rightmost digit) and handle any carry that might occur.
* If the least significant digit is less than 9, we can simply increment it and return the array.
* If the least significant digit is 9, it becomes 0 after adding 1, and we need to carry the 1 to the next digit.
* This process continues until we either find a digit that doesn't result in a carry or we've carried through all digits.
* If we've carried through all digits, we need to add a new most significant digit of 1.

---

## **Step 2: Flow Steps (Brute Force)**

1. Start from the least significant digit (the rightmost digit).
2. Add 1 to the current digit.
3. If the result is less than 10, we're done. Return the array.
4. Otherwise, set the current digit to 0 and carry the 1 to the next digit.
5. If we've carried through all digits, add a new most significant digit of 1.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def plusOne(digits):
    n = len(digits)
    
    # Start from the least significant digit
    for i in range(n - 1, -1, -1):
        # Add 1 to the current digit
        digits[i] += 1
        
        # If the result is less than 10, we're done
        if digits[i] < 10:
            return digits
        
        # Otherwise, set the current digit to 0 and carry the 1
        digits[i] = 0
    
    # If we've carried through all digits, add a new most significant digit of 1
    return [1] + digits
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of digits. In the worst case, we need to iterate through all digits.
* Space Complexity: O(1) if we ignore the space used for the output. If we consider the space used for the output, it's O(n) or O(n+1) if we need to add a new most significant digit.
* This approach is actually optimal for this problem.

---

## **Step 5: Visualizing Computation**

* For digits = [1,2,3]:
  * Start from the least significant digit: 3
  * Add 1: 3 + 1 = 4
  * 4 < 10, so we're done
  * Return [1,2,4]

* For digits = [4,3,2,1]:
  * Start from the least significant digit: 1
  * Add 1: 1 + 1 = 2
  * 2 < 10, so we're done
  * Return [4,3,2,2]

* For digits = [9]:
  * Start from the least significant digit: 9
  * Add 1: 9 + 1 = 10
  * 10 >= 10, so set the current digit to 0 and carry the 1
  * We've carried through all digits, so add a new most significant digit of 1
  * Return [1,0]

* For digits = [9,9,9]:
  * Start from the least significant digit: 9
  * Add 1: 9 + 1 = 10
  * 10 >= 10, so set the current digit to 0 and carry the 1
  * Next digit: 9
  * Add 1: 9 + 1 = 10
  * 10 >= 10, so set the current digit to 0 and carry the 1
  * Next digit: 9
  * Add 1: 9 + 1 = 10
  * 10 >= 10, so set the current digit to 0 and carry the 1
  * We've carried through all digits, so add a new most significant digit of 1
  * Return [1,0,0,0]

---

## **Why are we using this algorithm?**

* The approach of iterating from the least significant digit and handling carries is intuitive and efficient.
* There's no need for optimization as this approach is already optimal.
* However, we can make the code more concise by using a different approach.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of explicitly handling carries, we can convert the array of digits to an integer, add 1, and then convert back to an array of digits.
* This approach is more concise but has limitations due to integer overflow for very large numbers.
* In languages with arbitrary precision integers (like Python), this approach works for all valid inputs.

### **Step 2: Flow Steps (Optimized)**

1. Convert the array of digits to an integer.
2. Add 1 to the integer.
3. Convert the result back to an array of digits.

### **Step 3: Implementation (Optimized)**

```python
def plusOne(digits):
    # Convert the array of digits to an integer
    num = 0
    for digit in digits:
        num = num * 10 + digit
    
    # Add 1 to the integer
    num += 1
    
    # Convert the result back to an array of digits
    result = []
    while num > 0:
        result.insert(0, num % 10)
        num //= 10
    
    return result if result else [0]
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the number of digits. Converting the array to an integer and back takes O(n) time.
* Space Complexity: O(n) for storing the result array.
* This approach is more concise but has limitations due to integer overflow for very large numbers. In languages with arbitrary precision integers (like Python), this approach works for all valid inputs.

### **Step 5: Visualizing Computation (Optimized)**

* For digits = [1,2,3]:
  * Convert to integer: 1 * 100 + 2 * 10 + 3 = 123
  * Add 1: 123 + 1 = 124
  * Convert back to array: [1,2,4]

* For digits = [9,9,9]:
  * Convert to integer: 9 * 100 + 9 * 10 + 9 = 999
  * Add 1: 999 + 1 = 1000
  * Convert back to array: [1,0,0,0]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Iterative | O(n) | O(1) or O(n) | Handles carries explicitly |
| Conversion | O(n) | O(n) | Converts to integer and back |

---

## **Final Takeaways**

* The Plus One problem is a simple arithmetic operation that requires handling carries.
* Both approaches (iterative and conversion) have the same time complexity, but the iterative approach is more space-efficient and doesn't have limitations due to integer overflow.
* This problem demonstrates how understanding the properties of a problem can lead to a straightforward solution.

---

## **Real-life Analogy**

* Imagine you're counting items one by one. When you reach 9 items and add one more, you need to carry the 1 to the tens place and start counting from 0 again in the ones place. This is similar to how we handle carries in the Plus One problem.

---

## **Summary**

* We tackled the "Plus One" problem by iterating from the least significant digit and handling carries. We also explored an alternative approach of converting the array to an integer, adding 1, and converting back to an array. Both approaches have a time complexity of O(n) where n is the number of digits, but the iterative approach is more space-efficient and doesn't have limitations due to integer overflow. This problem demonstrates how understanding the properties of a problem can lead to a straightforward solution. 