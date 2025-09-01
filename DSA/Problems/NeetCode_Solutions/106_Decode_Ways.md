# 📝 Decode Ways

## **Problem Statement**

* A message containing letters from A-Z can be encoded into numbers using the following mapping: 'A' -> "1", 'B' -> "2", ..., 'Z' -> "26".
* To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways).
* Given a string `s` containing only digits, return the number of ways to decode it.
* The test cases are generated so that the answer fits in a 32-bit integer.
* Example 1:
  * Input: s = "12"
  * Output: 2
  * Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).
* Example 2:
  * Input: s = "226"
  * Output: 3
  * Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
* Example 3:
  * Input: s = "06"
  * Output: 0
  * Explanation: "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").
* Constraints:
  * 1 <= s.length <= 100
  * s contains only digits and may contain leading zero(s).

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the number of ways to decode a string of digits
* For each digit, we have two choices:
  * Decode it as a single digit (if it's not '0')
  * Decode it along with the next digit as a two-digit number (if the two-digit number is between 10 and 26)
* This suggests a recursive approach where we try both options at each step and count the total number of valid decodings
* We need to handle edge cases like leading zeros and invalid two-digit numbers

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes the current index in the string
2. Base case: If we've reached the end of the string, we've found a valid decoding, so return 1
3. If the current digit is '0', return 0 (as we can't decode a single '0')
4. Initialize count to 0
5. Try decoding the current digit as a single digit:
   1. Recursively call the function with the next index
   2. Add the result to count
6. If there's at least one more digit and the two-digit number is between 10 and 26:
   1. Recursively call the function with the index after the next
   2. Add the result to count
7. Return count

---

## **Step 3: Brute Force Implementation (Code)**

```python
def numDecodings(s):
    def decode(index):
        # Base case: we've successfully reached the end of the string
        if index == len(s):
            return 1
        
        # If the current digit is '0', it can't be decoded on its own
        if s[index] == '0':
            return 0
        
        # Try decoding the current digit as a single character
        count = decode(index + 1)
        
        # Try decoding the current and next digit as a two-digit number
        if index + 1 < len(s) and int(s[index:index+2]) <= 26:
            count += decode(index + 2)
        
        return count
    
    return decode(0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n) where n is the length of the string. In the worst case, we make two recursive calls at each step, leading to an exponential number of function calls.
* Space Complexity: O(n) for the recursion stack.
* This approach is inefficient for long strings due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For s = "226":
  * decode(0):
    * Try single digit '2': decode(1)
      * Try single digit '2': decode(2)
        * Try single digit '6': decode(3)
          * Reached end, return 1
        * Try two digits '26': Not possible (out of bounds)
        * Return 1
      * Try two digits '22': decode(3)
        * Reached end, return 1
      * Return 1 + 1 = 2
    * Try two digits '22': decode(2)
      * Try single digit '6': decode(3)
        * Reached end, return 1
      * Try two digits '26': Not possible (out of bounds)
      * Return 1
    * Return 2 + 1 = 3
* Total ways = 3
* There's redundant computation in the recursive calls, especially for overlapping subproblems like decode(2)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the number of ways to decode a string depends only on the number of ways to decode its suffixes.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently determine if a one or two-digit number is a valid encoding.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let dp[i] represent the number of ways to decode the substring s[i:] (from index i to the end)
* Base case: dp[n] = 1 (empty string has one way to decode, which is to decode nothing)
* For each index i from n-1 down to 0:
  * If the current digit is '0', dp[i] = 0 (can't decode a single '0')
  * Otherwise, dp[i] = dp[i+1] (ways to decode by taking the current digit as a single character)
  * If the current and next digit form a valid two-digit number (10 to 26), add dp[i+2] to dp[i]
* The answer will be dp[0] (number of ways to decode the entire string)

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Create an array dp of size n+1, where dp[i] represents the number of ways to decode the substring s[i:]
2. Initialize dp[n] = 1 (base case: empty string)
3. For each index i from n-1 down to 0:
   1. If the current digit is not '0', set dp[i] = dp[i+1]
   2. If the current and next digit form a valid two-digit number (10 to 26), add dp[i+2] to dp[i]
4. Return dp[0]

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def numDecodings(s):
    n = len(s)
    # dp[i] represents the number of ways to decode the substring s[i:]
    dp = [0] * (n + 1)
    dp[n] = 1  # Empty string has one way to decode
    
    for i in range(n - 1, -1, -1):
        # If the current digit is not '0', we can decode it as a single digit
        if s[i] != '0':
            dp[i] += dp[i + 1]
        
        # If the current and next digit form a valid two-digit number
        if i + 1 < n and (s[i] == '1' or (s[i] == '2' and s[i + 1] <= '6')):
            dp[i] += dp[i + 2]
    
    return dp[0]
```

### **Space-Optimized Version**

Since we only need the values of dp[i+1] and dp[i+2] to calculate dp[i], we can optimize the space complexity to O(1):

```python
def numDecodings(s):
    n = len(s)
    # Initialize variables to store dp[i+1] and dp[i+2]
    dp_i_plus_1 = 1  # dp[n] = 1
    dp_i_plus_2 = 0  # This will be updated in the first iteration
    
    for i in range(n - 1, -1, -1):
        # Initialize current dp[i]
        dp_i = 0
        
        # If the current digit is not '0', we can decode it as a single digit
        if s[i] != '0':
            dp_i += dp_i_plus_1
        
        # If the current and next digit form a valid two-digit number
        if i + 1 < n and (s[i] == '1' or (s[i] == '2' and s[i + 1] <= '6')):
            dp_i += dp_i_plus_2
        
        # Update dp_i_plus_2 and dp_i_plus_1 for the next iteration
        dp_i_plus_2 = dp_i_plus_1
        dp_i_plus_1 = dp_i
    
    return dp_i_plus_1
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(n) where n is the length of the string. We process each digit once.
* Space Complexity: O(n) for the dp array in the first implementation, and O(1) for the space-optimized version.
* This approach is much more efficient than the brute force approach for long strings.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For s = "226":
  * Initialize dp[3] = 1 (empty string)
  * i = 2 (digit '6'):
    * '6' is not '0', so dp[2] += dp[3] = 1
    * No valid two-digit number starting at index 2, so dp[2] = 1
  * i = 1 (digit '2'):
    * '2' is not '0', so dp[1] += dp[2] = 1
    * '26' is a valid two-digit number, so dp[1] += dp[3] = 1 + 1 = 2
  * i = 0 (digit '2'):
    * '2' is not '0', so dp[0] += dp[1] = 2
    * '22' is a valid two-digit number, so dp[0] += dp[2] = 2 + 1 = 3
* dp[0] = 3, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(2^n) | O(n) | Inefficient for long strings |
| Dynamic Programming (Tabulation) | O(n) | O(n) | Efficient with linear space |
| Space-Optimized DP | O(n) | O(1) | Efficient with constant space |

---

## **Final Takeaways**

* The key to efficiently solving the Decode Ways problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using a bottom-up approach by building the solution from smaller subproblems.
* Special attention should be paid to edge cases like leading zeros and invalid two-digit numbers.
* The space-optimized version demonstrates how we can reduce space complexity by only keeping track of the necessary previous results.
* This problem is a classic example of how dynamic programming can transform an exponential-time algorithm into a linear-time one.

---

## **Real-life Analogy**

* Imagine you're trying to count the number of ways to climb a staircase where you can take either 1 or 2 steps at a time, but some steps are broken and can't be stepped on (like the '0's in our problem). The brute force approach would be to try all possible combinations of steps, which is inefficient. The dynamic programming approach would be to calculate the number of ways to reach each step based on the number of ways to reach the previous steps, which is much more efficient.

---

## **Summary**

* We solved the Decode Ways problem by first considering a brute force recursive approach that tries all possible decodings, resulting in O(2^n) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(n) and the space complexity to O(n) or even O(1) with further optimization. The key insight is to recognize that the number of ways to decode a string depends only on the number of ways to decode its suffixes, which allows us to build the solution incrementally from the end of the string to the beginning. 