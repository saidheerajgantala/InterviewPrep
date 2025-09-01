# 📝 Pow(x, n)

## **Problem Statement**

* Implement `pow(x, n)`, which calculates `x` raised to the power `n` (i.e., x^n).
* Example:
  * Input: x = 2.00000, n = 10
  * Output: 1024.00000 (2^10 = 1024)
  * Input: x = 2.10000, n = 3
  * Output: 9.26100 (2.1^3 = 9.261)
  * Input: x = 2.00000, n = -2
  * Output: 0.25000 (2^-2 = 1/2^2 = 1/4 = 0.25)
* Constraints:
  * -100.0 < x < 100.0
  * -2^31 <= n <= 2^31-1
  * -10^4 <= x^n <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* To calculate x^n, we can multiply x by itself n times.
* For negative n, we can calculate x^(-n) and then take the reciprocal.
* This approach is straightforward but inefficient for large values of n.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. If n is negative, set x = 1/x and n = -n.
2. Initialize result = 1.
3. For i from 1 to n:
   1. Multiply result by x.
4. Return result.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def myPow(x, n):
    # Handle negative exponent
    if n < 0:
        x = 1 / x
        n = -n
    
    result = 1
    for _ in range(n):
        result *= x
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the exponent. We perform n multiplications.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient for large values of n and will time out for the given constraints.

---

## **Step 5: Visualizing Redundant Computation**

* For x = 2, n = 10:
  * result = 1
  * result = 1 * 2 = 2
  * result = 2 * 2 = 4
  * result = 4 * 2 = 8
  * result = 8 * 2 = 16
  * result = 16 * 2 = 32
  * result = 32 * 2 = 64
  * result = 64 * 2 = 128
  * result = 128 * 2 = 256
  * result = 256 * 2 = 512
  * result = 512 * 2 = 1024
  * Return 1024

---

## **Why are we using this algorithm?**

* For optimization, we can use the binary exponentiation algorithm.
* The key insight is that x^n can be calculated more efficiently by using the property that x^n = (x^(n/2))^2 if n is even, and x^n = x * (x^(n/2))^2 if n is odd.
* This reduces the number of multiplications from O(n) to O(log n).

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We'll use the binary exponentiation algorithm, also known as "exponentiation by squaring".
* The key insight is that x^n can be calculated more efficiently by using the property that:
  * x^n = (x^(n/2))^2 if n is even
  * x^n = x * (x^(n/2))^2 if n is odd
* This reduces the number of multiplications from O(n) to O(log n).

### **Step 2: Flow Steps (Optimized)**

1. If n is negative, set x = 1/x and n = -n.
2. Initialize result = 1.
3. While n > 0:
   1. If n is odd, multiply result by x.
   2. Set x = x * x.
   3. Set n = n // 2.
4. Return result.

### **Step 3: Implementation (Optimized)**

```python
def myPow(x, n):
    # Handle negative exponent
    if n < 0:
        x = 1 / x
        n = -n
    
    result = 1
    while n > 0:
        # If n is odd, multiply result by x
        if n % 2 == 1:
            result *= x
        
        # Square x and halve n
        x *= x
        n //= 2
    
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(log n) where n is the exponent. We perform log n multiplications.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is much more efficient than the brute force approach and can handle large values of n.

### **Step 5: Visualizing Computation (Optimized)**

* For x = 2, n = 10:
  * result = 1, x = 2, n = 10
  * n is even, so x = 2 * 2 = 4, n = 5
  * n is odd, so result = 1 * 4 = 4, x = 4 * 4 = 16, n = 2
  * n is even, so x = 16 * 16 = 256, n = 1
  * n is odd, so result = 4 * 256 = 1024, x = 256 * 256 = 65536, n = 0
  * Return 1024

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n) | O(1) | Multiplies x by itself n times |
| Binary Exponentiation | O(log n) | O(1) | Uses the property that x^n = (x^(n/2))^2 if n is even |

---

## **Final Takeaways**

* The Pow(x, n) problem is a classic example of how mathematical properties can be used to optimize algorithms.
* The binary exponentiation algorithm reduces the time complexity from O(n) to O(log n), making it much more efficient for large values of n.
* This problem demonstrates how understanding the properties of a problem can lead to a more efficient solution.

---

## **Real-life Analogy**

* Imagine you're folding a piece of paper. Each fold doubles the number of layers. If you want to create 2^10 = 1024 layers, you only need to fold the paper 10 times, not 1024 times. This is similar to how binary exponentiation reduces the number of multiplications.

---

## **Summary**

* We tackled the "Pow(x, n)" problem by using the binary exponentiation algorithm, which reduces the time complexity from O(n) to O(log n). The key insight is that x^n can be calculated more efficiently by using the property that x^n = (x^(n/2))^2 if n is even, and x^n = x * (x^(n/2))^2 if n is odd. This approach is much more efficient than the brute force approach and can handle large values of n. 