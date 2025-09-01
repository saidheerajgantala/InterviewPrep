# 📝 Happy Number

## **Problem Statement**

* Write an algorithm to determine if a number `n` is happy.
* A happy number is a number defined by the following process:
  * Starting with any positive integer, replace the number by the sum of the squares of its digits.
  * Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
  * Those numbers for which this process ends in 1 are happy.
* Return true if `n` is a happy number, and false if not.
* Example:
  * Input: n = 19
  * Output: true
  * Explanation:
    * 1^2 + 9^2 = 1 + 81 = 82
    * 8^2 + 2^2 = 64 + 4 = 68
    * 6^2 + 8^2 = 36 + 64 = 100
    * 1^2 + 0^2 + 0^2 = 1 + 0 + 0 = 1
  * Input: n = 2
  * Output: false
* Constraints:
  * 1 <= n <= 2^31 - 1

---

## **Step 1: Thinking Process (Brute Force)**

* To determine if a number is happy, we need to follow the process described in the problem statement.
* We repeatedly calculate the sum of the squares of the digits until we either reach 1 or detect a cycle.
* If we reach 1, the number is happy.
* If we detect a cycle, the number is not happy.
* Let's think about how to detect a cycle.

---

## **Step 2: Flow Steps (Brute Force)**

1. Define a function `get_next(n)` that calculates the sum of the squares of the digits of `n`.
2. Initialize a set `seen` to keep track of the numbers we've encountered.
3. While `n != 1` and `n` is not in `seen`:
   1. Add `n` to `seen`.
   2. Set `n` to `get_next(n)`.
4. Return `True` if `n == 1`, otherwise return `False`.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def isHappy(n):
    def get_next(n):
        total_sum = 0
        while n > 0:
            n, digit = divmod(n, 10)
            total_sum += digit ** 2
        return total_sum
    
    seen = set()
    while n != 1 and n not in seen:
        seen.add(n)
        n = get_next(n)
    
    return n == 1
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(log n) for the `get_next` function, as the number of digits in n is log n. The number of iterations in the while loop is bounded by the number of possible values for n, which is relatively small (all numbers eventually converge to a cycle or to 1).
* Space Complexity: O(log n) for the `seen` set, which stores at most log n numbers.
* This approach is actually quite efficient and is the standard solution for this problem.

---

## **Step 5: Visualizing Computation**

* For n = 19:
  * get_next(19) = 1^2 + 9^2 = 1 + 81 = 82
  * seen = {19}
  * n = 82
  * get_next(82) = 8^2 + 2^2 = 64 + 4 = 68
  * seen = {19, 82}
  * n = 68
  * get_next(68) = 6^2 + 8^2 = 36 + 64 = 100
  * seen = {19, 82, 68}
  * n = 100
  * get_next(100) = 1^2 + 0^2 + 0^2 = 1 + 0 + 0 = 1
  * seen = {19, 82, 68, 100}
  * n = 1
  * n == 1, so return True

* For n = 2:
  * get_next(2) = 2^2 = 4
  * seen = {2}
  * n = 4
  * get_next(4) = 4^2 = 16
  * seen = {2, 4}
  * n = 16
  * get_next(16) = 1^2 + 6^2 = 1 + 36 = 37
  * seen = {2, 4, 16}
  * n = 37
  * ... (continue until we detect a cycle)
  * Eventually, we'll encounter a number we've seen before, indicating a cycle that doesn't include 1.
  * Return False

---

## **Why are we using this algorithm?**

* The approach of using a set to detect cycles is intuitive and efficient.
* However, there's another approach using Floyd's Cycle-Finding Algorithm (also known as the "tortoise and hare" algorithm) that uses constant space.
* Let's explore this optimization.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can use Floyd's Cycle-Finding Algorithm to detect cycles with constant space.
* We'll have two pointers, slow and fast, both starting at n.
* The slow pointer moves one step at a time (calculates `get_next` once).
* The fast pointer moves two steps at a time (calculates `get_next` twice).
* If there's a cycle, the fast pointer will eventually catch up to the slow pointer.
* If the number is happy, both pointers will eventually reach 1.

### **Step 2: Flow Steps (Optimized)**

1. Define a function `get_next(n)` that calculates the sum of the squares of the digits of `n`.
2. Initialize two pointers, `slow` and `fast`, both set to `n`.
3. Do the following until either `slow == 1` or `slow == fast`:
   1. Move `slow` one step: `slow = get_next(slow)`.
   2. Move `fast` two steps: `fast = get_next(get_next(fast))`.
4. Return `True` if `slow == 1`, otherwise return `False`.

### **Step 3: Implementation (Optimized)**

```python
def isHappy(n):
    def get_next(n):
        total_sum = 0
        while n > 0:
            n, digit = divmod(n, 10)
            total_sum += digit ** 2
        return total_sum
    
    slow = n
    fast = n
    
    while True:
        slow = get_next(slow)
        fast = get_next(get_next(fast))
        
        if slow == 1 or slow == fast:
            break
    
    return slow == 1
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(log n) for the `get_next` function, as the number of digits in n is log n. The number of iterations in the while loop is bounded by the length of the cycle, which is relatively small.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is more space-efficient than the brute force approach.

### **Step 5: Visualizing Computation (Optimized)**

* For n = 19:
  * slow = 19, fast = 19
  * slow = get_next(19) = 82, fast = get_next(get_next(19)) = get_next(82) = 68
  * slow = get_next(82) = 68, fast = get_next(get_next(68)) = get_next(100) = 1
  * slow = get_next(68) = 100, fast = get_next(get_next(1)) = get_next(1) = 1
  * slow = get_next(100) = 1, fast = 1
  * slow == 1, so return True

* For n = 2:
  * slow = 2, fast = 2
  * slow = get_next(2) = 4, fast = get_next(get_next(2)) = get_next(4) = 16
  * slow = get_next(4) = 16, fast = get_next(get_next(16)) = get_next(37) = 58
  * ... (continue until slow == fast, indicating a cycle)
  * Eventually, slow and fast will meet at a number other than 1, indicating a cycle that doesn't include 1.
  * Return False

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Hash Set | O(log n) | O(log n) | Uses a set to detect cycles |
| Floyd's Cycle-Finding | O(log n) | O(1) | Uses two pointers to detect cycles |

---

## **Final Takeaways**

* The Happy Number problem is a classic example of cycle detection.
* Both approaches (using a set or Floyd's Cycle-Finding Algorithm) are efficient, but the latter uses constant space.
* This problem demonstrates how understanding the properties of a problem can lead to an efficient solution.

---

## **Real-life Analogy**

* Imagine you're walking in a maze with multiple paths. Some paths lead to the exit, while others loop endlessly. To determine if a path leads to the exit, you can either mark each intersection you've visited (hash set approach) or have two people walk the same path at different speeds (Floyd's Cycle-Finding approach). If the faster person catches up to the slower person, you know you're in a loop.

---

## **Summary**

* We tackled the "Happy Number" problem by detecting cycles in the sequence of numbers generated by the process. We explored two approaches: using a hash set to keep track of numbers we've seen, and using Floyd's Cycle-Finding Algorithm with two pointers moving at different speeds. Both approaches have a time complexity of O(log n), but the latter uses constant space, making it more efficient. This problem demonstrates how understanding the properties of a problem can lead to an elegant solution. 