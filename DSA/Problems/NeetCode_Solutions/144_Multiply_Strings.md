# 📝 Multiply Strings

## **Problem Statement**

* Given two non-negative integers represented as strings `num1` and `num2`, return their product as a string.
* You must not use any built-in BigInteger library or convert the inputs to integers directly.
* Example:
  * Input: num1 = "2", num2 = "3"
  * Output: "6"
  * Input: num1 = "123", num2 = "456"
  * Output: "56088"
* Constraints:
  * 1 <= num1.length, num2.length <= 200
  * num1 and num2 consist of digits only.
  * Both num1 and num2 do not contain any leading zero, except the number 0 itself.

---

## **Step 1: Thinking Process (Brute Force)**

* To multiply two numbers represented as strings, we need to implement the standard multiplication algorithm that we learn in school.
* We multiply each digit of the second number with the first number, and then add up the results.
* For each multiplication of a digit with a number, we need to handle carries.
* For the addition of the results, we also need to handle carries.
* Let's think more carefully about the implementation.

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a result array of size `len(num1) + len(num2)` filled with zeros.
2. Iterate through `num1` from right to left (i from `len(num1) - 1` to 0):
   1. Iterate through `num2` from right to left (j from `len(num2) - 1` to 0):
      1. Multiply the digits: `digit1 = int(num1[i])`, `digit2 = int(num2[j])`, `product = digit1 * digit2`.
      2. Add the product to the appropriate position in the result array: `result[i + j + 1] += product`.
      3. Handle carry: `result[i + j] += result[i + j + 1] // 10`, `result[i + j + 1] %= 10`.
3. Convert the result array to a string, removing any leading zeros.
4. Return the result string.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def multiply(num1, num2):
    if num1 == "0" or num2 == "0":
        return "0"
    
    # Initialize result array with zeros
    m, n = len(num1), len(num2)
    result = [0] * (m + n)
    
    # Multiply each digit and add to result
    for i in range(m - 1, -1, -1):
        for j in range(n - 1, -1, -1):
            product = int(num1[i]) * int(num2[j])
            p1, p2 = i + j, i + j + 1  # Positions in result array
            total = product + result[p2]
            
            result[p1] += total // 10
            result[p2] = total % 10
    
    # Convert result to string and remove leading zeros
    result_str = ''.join(map(str, result))
    return result_str.lstrip('0') or "0"
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(m * n) where m and n are the lengths of the input strings. We perform m * n single-digit multiplications.
* Space Complexity: O(m + n) for storing the result array.
* This approach is actually optimal for this problem, as we need to perform at least m * n operations to multiply two numbers with m and n digits.

---

## **Step 5: Visualizing Computation**

* For num1 = "123", num2 = "456":
  * Initialize result = [0, 0, 0, 0, 0, 0]
  * Multiply 3 (num1[2]) with 6 (num2[2]): 3 * 6 = 18
    * p1 = 2 + 2 = 4, p2 = 2 + 2 + 1 = 5
    * total = 18 + result[5] = 18 + 0 = 18
    * result[4] += 18 // 10 = 1, result[5] = 18 % 10 = 8
    * result = [0, 0, 0, 0, 1, 8]
  * Multiply 3 (num1[2]) with 5 (num2[1]): 3 * 5 = 15
    * p1 = 2 + 1 = 3, p2 = 2 + 1 + 1 = 4
    * total = 15 + result[4] = 15 + 1 = 16
    * result[3] += 16 // 10 = 1, result[4] = 16 % 10 = 6
    * result = [0, 0, 0, 1, 6, 8]
  * ... (continue for all digit pairs)
  * Final result = [0, 0, 5, 6, 0, 8, 8]
  * After removing leading zeros: "56088"

---

## **Why are we using this algorithm?**

* The approach of simulating the standard multiplication algorithm is intuitive and efficient.
* There's no need for optimization as this approach is already optimal.
* However, we can make the code more concise by handling carries differently.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of handling carries immediately after each multiplication, we can accumulate all products in the result array and then handle all carries at the end.
* This makes the code more concise and easier to understand.

### **Step 2: Flow Steps (Optimized)**

1. Initialize a result array of size `len(num1) + len(num2)` filled with zeros.
2. Iterate through `num1` from right to left (i from `len(num1) - 1` to 0):
   1. Iterate through `num2` from right to left (j from `len(num2) - 1` to 0):
      1. Multiply the digits: `digit1 = int(num1[i])`, `digit2 = int(num2[j])`, `product = digit1 * digit2`.
      2. Add the product to the appropriate position in the result array: `result[i + j + 1] += product`.
3. Handle all carries:
   1. Iterate through the result array from right to left:
      1. Add the carry from the previous position: `result[i] += carry`.
      2. Calculate the new carry: `carry = result[i] // 10`.
      3. Update the current position: `result[i] %= 10`.
4. Convert the result array to a string, removing any leading zeros.
5. Return the result string.

### **Step 3: Implementation (Optimized)**

```python
def multiply(num1, num2):
    if num1 == "0" or num2 == "0":
        return "0"
    
    # Initialize result array with zeros
    m, n = len(num1), len(num2)
    result = [0] * (m + n)
    
    # Multiply each digit and add to result
    for i in range(m - 1, -1, -1):
        for j in range(n - 1, -1, -1):
            product = int(num1[i]) * int(num2[j])
            result[i + j + 1] += product
    
    # Handle carries
    for i in range(len(result) - 1, 0, -1):
        result[i - 1] += result[i] // 10
        result[i] %= 10
    
    # Convert result to string and remove leading zeros
    result_str = ''.join(map(str, result))
    return result_str.lstrip('0') or "0"
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(m * n) where m and n are the lengths of the input strings. We perform m * n single-digit multiplications.
* Space Complexity: O(m + n) for storing the result array.
* This approach has the same time and space complexity as the brute force approach, but the code is more concise and easier to understand.

### **Step 5: Visualizing Computation (Optimized)**

* For num1 = "123", num2 = "456":
  * Initialize result = [0, 0, 0, 0, 0, 0]
  * Multiply all digit pairs and accumulate in result:
    * 3 * 6 = 18, result[5] += 18, result = [0, 0, 0, 0, 0, 18]
    * 3 * 5 = 15, result[4] += 15, result = [0, 0, 0, 0, 15, 18]
    * 3 * 4 = 12, result[3] += 12, result = [0, 0, 0, 12, 15, 18]
    * 2 * 6 = 12, result[4] += 12, result = [0, 0, 0, 12, 27, 18]
    * 2 * 5 = 10, result[3] += 10, result = [0, 0, 0, 22, 27, 18]
    * 2 * 4 = 8, result[2] += 8, result = [0, 0, 8, 22, 27, 18]
    * 1 * 6 = 6, result[3] += 6, result = [0, 0, 8, 28, 27, 18]
    * 1 * 5 = 5, result[2] += 5, result = [0, 0, 13, 28, 27, 18]
    * 1 * 4 = 4, result[1] += 4, result = [0, 4, 13, 28, 27, 18]
  * Handle carries:
    * result[5] = 18, carry = 1, result[5] = 8, result[4] += 1, result = [0, 4, 13, 28, 28, 8]
    * result[4] = 28, carry = 2, result[4] = 8, result[3] += 2, result = [0, 4, 13, 30, 8, 8]
    * result[3] = 30, carry = 3, result[3] = 0, result[2] += 3, result = [0, 4, 16, 0, 8, 8]
    * result[2] = 16, carry = 1, result[2] = 6, result[1] += 1, result = [0, 5, 6, 0, 8, 8]
    * result[1] = 5, carry = 0, result[1] = 5, result[0] += 0, result = [0, 5, 6, 0, 8, 8]
  * Final result = [0, 5, 6, 0, 8, 8]
  * After removing leading zeros: "56088"

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(m * n) | O(m + n) | Handles carries immediately after each multiplication |
| Optimized | O(m * n) | O(m + n) | Accumulates products first, then handles all carries at the end |

---

## **Final Takeaways**

* The Multiply Strings problem is a classic example of implementing a standard arithmetic operation without using built-in libraries.
* Both approaches (handling carries immediately vs. handling them at the end) have the same time and space complexity, but the latter is more concise and easier to understand.
* This problem demonstrates how understanding the properties of a problem can lead to a clean and efficient solution.

---

## **Real-life Analogy**

* Imagine you're a cashier calculating the total cost of multiple items. You can either calculate the running total after each item (handling carries immediately) or add up all the costs first and then calculate the final total (handling carries at the end). Both approaches give the same result, but the latter might be more efficient if you're good at mental math.

---

## **Summary**

* We tackled the "Multiply Strings" problem by implementing the standard multiplication algorithm. We explored two approaches: handling carries immediately after each multiplication vs. accumulating products first and then handling all carries at the end. Both approaches have a time complexity of O(m * n) where m and n are the lengths of the input strings, and a space complexity of O(m + n) for storing the result array. The latter approach is more concise and easier to understand, demonstrating how understanding the properties of a problem can lead to a clean and efficient solution. 