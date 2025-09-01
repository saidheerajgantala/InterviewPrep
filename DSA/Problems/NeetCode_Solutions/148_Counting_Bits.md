# 📝 Counting Bits

## **Problem Statement**

* Given an integer `n`, return an array `ans` of length `n + 1` such that for each `i` (0 <= i <= n), `ans[i]` is the number of 1's in the binary representation of `i`.
* Example:
  * Input: n = 2
  * Output: [0,1,1] (0 -> 0, 1 -> 1, 2 -> 10)
  * Input: n = 5
  * Output: [0,1,1,2,1,2] (0 -> 0, 1 -> 1, 2 -> 10, 3 -> 11, 4 -> 100, 5 -> 101)
* Constraints:
  * 0 <= n <= 10^5
* Follow up: It is very easy to come up with a solution with a runtime of O(n log n). Can you do it in linear time O(n) and possibly in a single pass?

---

## **Step 1: Thinking Process (Brute Force)**

* A straightforward approach would be to count the number of 1's in the binary representation of each number from 0 to n.
* We can use the algorithm from the previous problem (Number of 1 Bits) to count the number of 1's in each number.
* This approach would have a time complexity of O(n log n) as we need to iterate through O(log i) bits for each number i.
* Let's implement this approach first and then optimize it.

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an array `ans` of length `n + 1` with all zeros.
2. For each number i from 0 to n:
   1. Count the number of 1's in the binary representation of i.
   2. Set `ans[i]` to this count.
3. Return the array `ans`.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def countBits(n):
    ans = [0] * (n + 1)
    
    for i in range(n + 1):
        # Count the number of 1's in the binary representation of i
        count = 0
        num = i
        while num:
            num &= (num - 1)
            count += 1
        ans[i] = count
    
    return ans
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n log n) where n is the input number. For each number i from 0 to n, we need to count the number of 1's in its binary representation, which takes O(log i) time.
* Space Complexity: O(n) for storing the result array.
* This approach is efficient but doesn't meet the follow-up requirement of linear time.

---

## **Step 5: Visualizing Computation**

* For n = 5:
  * Initialize ans = [0, 0, 0, 0, 0, 0]
  * For i = 0:
    * num = 0, count = 0
    * ans[0] = 0
  * For i = 1:
    * num = 1, num - 1 = 0, num & (num - 1) = 0, count = 1
    * ans[1] = 1
  * For i = 2:
    * num = 2, num - 1 = 1, num & (num - 1) = 0, count = 1
    * ans[2] = 1
  * For i = 3:
    * num = 3, num - 1 = 2, num & (num - 1) = 2, count = 1
    * num = 2, num - 1 = 1, num & (num - 1) = 0, count = 2
    * ans[3] = 2
  * For i = 4:
    * num = 4, num - 1 = 3, num & (num - 1) = 0, count = 1
    * ans[4] = 1
  * For i = 5:
    * num = 5, num - 1 = 4, num & (num - 1) = 4, count = 1
    * num = 4, num - 1 = 3, num & (num - 1) = 0, count = 2
    * ans[5] = 2
  * Return ans = [0, 1, 1, 2, 1, 2]

---

## **Why are we using this algorithm?**

* The brute force approach has a time complexity of O(n log n), which doesn't meet the follow-up requirement of linear time.
* We can optimize this by using dynamic programming to reuse previously computed results.
* There are several patterns we can exploit:
  1. For even numbers (i.e., numbers ending with 0 in binary), the number of 1's is the same as the number of 1's in i/2.
  2. For odd numbers (i.e., numbers ending with 1 in binary), the number of 1's is one more than the number of 1's in i/2.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We'll use dynamic programming to compute the number of 1's in each number.
* For even numbers (i.e., numbers ending with 0 in binary), the number of 1's is the same as the number of 1's in i/2.
* For odd numbers (i.e., numbers ending with 1 in binary), the number of 1's is one more than the number of 1's in i/2.
* This approach allows us to compute the result in linear time.

### **Step 2: Flow Steps (Optimized)**

1. Initialize an array `ans` of length `n + 1` with all zeros.
2. For each number i from 1 to n:
   1. If i is even, set `ans[i] = ans[i/2]`.
   2. If i is odd, set `ans[i] = ans[i/2] + 1`.
3. Return the array `ans`.

### **Step 3: Implementation (Optimized)**

```python
def countBits(n):
    ans = [0] * (n + 1)
    
    for i in range(1, n + 1):
        # If i is even, the number of 1's is the same as the number of 1's in i/2
        # If i is odd, the number of 1's is one more than the number of 1's in i/2
        ans[i] = ans[i >> 1] + (i & 1)
    
    return ans
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the input number. We compute the result for each number in constant time.
* Space Complexity: O(n) for storing the result array.
* This approach meets the follow-up requirement of linear time.

### **Step 5: Visualizing Computation (Optimized)**

* For n = 5:
  * Initialize ans = [0, 0, 0, 0, 0, 0]
  * For i = 1:
    * i >> 1 = 0, i & 1 = 1, ans[1] = ans[0] + 1 = 0 + 1 = 1
  * For i = 2:
    * i >> 1 = 1, i & 1 = 0, ans[2] = ans[1] + 0 = 1 + 0 = 1
  * For i = 3:
    * i >> 1 = 1, i & 1 = 1, ans[3] = ans[1] + 1 = 1 + 1 = 2
  * For i = 4:
    * i >> 1 = 2, i & 1 = 0, ans[4] = ans[2] + 0 = 1 + 0 = 1
  * For i = 5:
    * i >> 1 = 2, i & 1 = 1, ans[5] = ans[2] + 1 = 1 + 1 = 2
  * Return ans = [0, 1, 1, 2, 1, 2]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n log n) | O(n) | Counts the number of 1's in each number |
| Optimized (Dynamic Programming) | O(n) | O(n) | Uses previously computed results |

---

## **Final Takeaways**

* The Counting Bits problem is a good example of how dynamic programming can be used to optimize bit manipulation problems.
* By recognizing the pattern that the number of 1's in an even number is the same as the number of 1's in half that number, and the number of 1's in an odd number is one more than the number of 1's in half that number, we can compute the result in linear time.
* This problem demonstrates how understanding the properties of binary numbers can lead to elegant and efficient solutions.

---

## **Real-life Analogy**

* Imagine you're counting the number of lit bulbs in a series of binary displays. Instead of counting each bulb individually for each display, you can reuse the count from a similar display. For example, if you know that display #4 has 1 lit bulb, then display #8 (which is #4 with an extra 0 at the end) also has 1 lit bulb. And if display #5 (which is #2 with an extra 1 at the end) has 2 lit bulbs, then display #10 (which is #5 with an extra 0 at the end) also has 2 lit bulbs.

---

## **Summary**

* We tackled the "Counting Bits" problem by using dynamic programming to compute the number of 1's in each number from 0 to n. We recognized the pattern that the number of 1's in an even number is the same as the number of 1's in half that number, and the number of 1's in an odd number is one more than the number of 1's in half that number. This approach has a time complexity of O(n) where n is the input number, and a space complexity of O(n) for storing the result array. This is more efficient than the brute force approach of counting the number of 1's in each number, which has a time complexity of O(n log n). 