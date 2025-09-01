# 📝 Distinct Subsequences

## **Problem Statement**

* Given two strings `s` and `t`, return the number of distinct subsequences of `s` which equals `t`.
* A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).
* The test cases are generated so that the answer fits on a 32-bit signed integer.
* Example 1:
  * Input: s = "rabbbit", t = "rabbit"
  * Output: 3
  * Explanation: There are 3 ways to get "rabbit" from "rabbbit":
    * "r**abbb**it" -> "rabbit"
    * "ra**bbb**it" -> "rabbit"
    * "rab**bb**it" -> "rabbit"
* Example 2:
  * Input: s = "babgbag", t = "bag"
  * Output: 5
  * Explanation: There are 5 ways to get "bag" from "babgbag":
    * "**ba**b**g**bag" -> "bag"
    * "**ba**bgba**g**" -> "bag"
    * "**b**abgb**ag**" -> "bag"
    * "ba**b**gb**ag**" -> "bag"
    * "babg**bag**" -> "bag"
* Constraints:
  * 1 <= s.length, t.length <= 1000
  * s and t consist of lowercase English letters.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to count the number of distinct subsequences of s that equal t
* A subsequence is formed by deleting some characters without changing the order
* One approach would be to generate all subsequences of s and count how many equal t
* However, this would be inefficient as there are 2^n possible subsequences for a string of length n
* Instead, we can use recursion to solve this problem by considering each character of s and deciding whether to include it in the subsequence or not

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes two indices i and j, representing the current positions in s and t
2. Base cases:
   1. If j reaches the end of t, we've found a valid subsequence, so return 1
   2. If i reaches the end of s but j hasn't reached the end of t, we can't form t, so return 0
3. If the current characters match (s[i] == t[j]):
   1. We have two choices: include s[i] in the subsequence or skip it
   2. If we include it, we move to the next character in both s and t
   3. If we skip it, we move to the next character in s but stay at the same position in t
   4. Return the sum of these two choices
4. If the current characters don't match (s[i] != t[j]):
   1. We can only skip s[i] and move to the next character in s
5. Start the recursion with i = 0 and j = 0

---

## **Step 3: Brute Force Implementation (Code)**

```python
def numDistinct(s, t):
    def count_subsequences(i, j):
        # Base cases
        if j == len(t):
            return 1
        if i == len(s):
            return 0
        
        # If the current characters match
        if s[i] == t[j]:
            # Either include s[i] or skip it
            return count_subsequences(i + 1, j + 1) + count_subsequences(i + 1, j)
        else:
            # Skip s[i]
            return count_subsequences(i + 1, j)
    
    return count_subsequences(0, 0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n) where n is the length of s. In the worst case, we explore all possible subsequences of s.
* Space Complexity: O(m + n) for the recursion stack, where m is the length of t.
* This approach is inefficient for long strings due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For s = "rabbbit", t = "rabbit":
  * count_subsequences(0, 0): s[0] == t[0] ('r' == 'r')
    * Include: count_subsequences(1, 1)
      * s[1] == t[1] ('a' == 'a')
        * Include: count_subsequences(2, 2)
          * s[2] == t[2] ('b' == 'b')
            * Include: count_subsequences(3, 3)
              * s[3] == t[3] ('b' == 'b')
                * Include: count_subsequences(4, 4)
                  * s[4] != t[4] ('b' != 'i')
                  * Skip: count_subsequences(5, 4)
                    * ... (many recursive calls)
                * Skip: count_subsequences(4, 3)
                  * ... (many recursive calls)
              * Skip: count_subsequences(3, 2)
                * ... (many recursive calls)
            * Skip: count_subsequences(2, 1)
              * ... (many recursive calls)
        * Skip: count_subsequences(1, 0)
          * ... (many recursive calls)
    * Skip: count_subsequences(0, 0) (this is a redundant computation)
* There's redundant computation in the recursive calls, especially for overlapping subproblems like count_subsequences(3, 2)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming with memoization to avoid redundant computations.
* The property that matches this problem is that the number of distinct subsequences depends only on the number of distinct subsequences for smaller prefixes of s and t.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently calculate the number of distinct subsequences for each prefix of s and t.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming with Memoization)**

* We can use dynamic programming with memoization to avoid redundant computations
* Let memo[i][j] represent the number of distinct subsequences of s[i:] that equal t[j:]
* We'll use the same recursive approach as before, but store the results in the memo table
* For each state (i, j), we'll compute the result once and reuse it when needed

### **Step 2: Flow Steps (Optimized - Dynamic Programming with Memoization)**

1. Create a 2D array memo of size (m+1) x (n+1), initialized with -1
2. Implement the recursive function with memoization:
   1. If the result for (i, j) is already in memo, return it
   2. Otherwise, compute the result as in the brute force approach
   3. Store the result in memo[i][j] and return it
3. Start the recursion with i = 0 and j = 0

### **Step 3: Implementation (Optimized - Dynamic Programming with Memoization)**

```python
def numDistinct(s, t):
    m, n = len(s), len(t)
    memo = [[-1] * (n + 1) for _ in range(m + 1)]
    
    def count_subsequences(i, j):
        # Base cases
        if j == n:
            return 1
        if i == m:
            return 0
        
        # If we've already computed the result, return it
        if memo[i][j] != -1:
            return memo[i][j]
        
        # If the current characters match
        if s[i] == t[j]:
            # Either include s[i] or skip it
            memo[i][j] = count_subsequences(i + 1, j + 1) + count_subsequences(i + 1, j)
        else:
            # Skip s[i]
            memo[i][j] = count_subsequences(i + 1, j)
        
        return memo[i][j]
    
    return count_subsequences(0, 0)
```

### **Bottom-Up Dynamic Programming**

```python
def numDistinct(s, t):
    m, n = len(s), len(t)
    
    # Create a 2D array filled with 0
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Base case: empty string t is a subsequence of any string s
    for i in range(m + 1):
        dp[i][0] = 1
    
    # Fill the dp table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s[i - 1] == t[j - 1]:
                # Either include s[i-1] or skip it
                dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
            else:
                # Skip s[i-1]
                dp[i][j] = dp[i - 1][j]
    
    return dp[m][n]
```

### **Space-Optimized Bottom-Up Dynamic Programming**

```python
def numDistinct(s, t):
    m, n = len(s), len(t)
    
    # Create a 1D array filled with 0
    dp = [0] * (n + 1)
    dp[0] = 1  # Base case: empty string t is a subsequence of any string s
    
    # Fill the dp table
    for i in range(1, m + 1):
        for j in range(n, 0, -1):
            if s[i - 1] == t[j - 1]:
                dp[j] += dp[j - 1]
    
    return dp[n]
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming with Memoization)**

* Time Complexity: O(m * n) where m is the length of s and n is the length of t. We compute the result for each (i, j) pair exactly once.
* Space Complexity: O(m * n) for the memo array and the recursion stack.
* This approach is much more efficient than the brute force approach for long strings.

### **Complexity Analysis (Bottom-Up Dynamic Programming)**

* Time Complexity: O(m * n) where m is the length of s and n is the length of t. We fill a 2D table of size (m+1) x (n+1).
* Space Complexity: O(m * n) for the dp array.
* This approach is also efficient and avoids the overhead of recursion.

### **Complexity Analysis (Space-Optimized Bottom-Up Dynamic Programming)**

* Time Complexity: O(m * n) where m is the length of s and n is the length of t.
* Space Complexity: O(n) for the 1D dp array.
* This approach is the most space-efficient while maintaining the same time complexity.

### **Step 5: Visualizing Computation (Optimized - Bottom-Up Dynamic Programming)**

* For s = "rabbbit", t = "rabbit":
  * Initialize dp:
    ```
      |   | "" | r | a | b | b | i | t
    --|---|----|----|---|---|---|---|--
    "" | 1 | 0 | 0 | 0 | 0 | 0 | 0
    r  | 1 | ? | ? | ? | ? | ? | ?
    a  | 1 | ? | ? | ? | ? | ? | ?
    b  | 1 | ? | ? | ? | ? | ? | ?
    b  | 1 | ? | ? | ? | ? | ? | ?
    b  | 1 | ? | ? | ? | ? | ? | ?
    i  | 1 | ? | ? | ? | ? | ? | ?
    t  | 1 | ? | ? | ? | ? | ? | ?
    ```
  * Process dp[1][1]: s[0] == t[0] ('r' == 'r'), so dp[1][1] = dp[0][0] + dp[0][1] = 1 + 0 = 1
    ```
      |   | "" | r | a | b | b | i | t
    --|---|----|----|---|---|---|---|--
    "" | 1 | 0 | 0 | 0 | 0 | 0 | 0
    r  | 1 | 1 | ? | ? | ? | ? | ?
    a  | 1 | ? | ? | ? | ? | ? | ?
    b  | 1 | ? | ? | ? | ? | ? | ?
    b  | 1 | ? | ? | ? | ? | ? | ?
    b  | 1 | ? | ? | ? | ? | ? | ?
    i  | 1 | ? | ? | ? | ? | ? | ?
    t  | 1 | ? | ? | ? | ? | ? | ?
    ```
  * Process dp[1][2]: s[0] != t[1] ('r' != 'a'), so dp[1][2] = dp[0][2] = 0
    ```
      |   | "" | r | a | b | b | i | t
    --|---|----|----|---|---|---|---|--
    "" | 1 | 0 | 0 | 0 | 0 | 0 | 0
    r  | 1 | 1 | 0 | ? | ? | ? | ?
    a  | 1 | ? | ? | ? | ? | ? | ?
    b  | 1 | ? | ? | ? | ? | ? | ?
    b  | 1 | ? | ? | ? | ? | ? | ?
    b  | 1 | ? | ? | ? | ? | ? | ?
    i  | 1 | ? | ? | ? | ? | ? | ?
    t  | 1 | ? | ? | ? | ? | ? | ?
    ```
  * ... (continue filling the table)
  * Final dp table:
    ```
      |   | "" | r | a | b | b | i | t
    --|---|----|----|---|---|---|---|--
    "" | 1 | 0 | 0 | 0 | 0 | 0 | 0
    r  | 1 | 1 | 0 | 0 | 0 | 0 | 0
    a  | 1 | 1 | 1 | 0 | 0 | 0 | 0
    b  | 1 | 1 | 1 | 1 | 0 | 0 | 0
    b  | 1 | 1 | 1 | 2 | 1 | 0 | 0
    b  | 1 | 1 | 1 | 3 | 3 | 0 | 0
    i  | 1 | 1 | 1 | 3 | 3 | 3 | 0
    t  | 1 | 1 | 1 | 3 | 3 | 3 | 3
    ```
  * dp[7][6] = 3, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(2^m) | O(m + n) | Inefficient for long strings |
| Dynamic Programming (Memoization) | O(m * n) | O(m * n) | Efficient for medium-sized strings |
| Dynamic Programming (Bottom-Up) | O(m * n) | O(m * n) | Efficient and avoids recursion overhead |
| Dynamic Programming (Space-Optimized) | O(m * n) | O(n) | Most space-efficient approach |

---

## **Final Takeaways**

* The key to efficiently solving the Distinct Subsequences problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using either a top-down approach with memoization or a bottom-up approach with a 2D array.
* The space optimization from a 2D array to a 1D array is possible because we only need the results from the previous row to compute the current row.
* Special attention should be paid to the direction of iteration in the space-optimized version to avoid overwriting values before they are used.
* This problem is a classic example of string matching with dynamic programming, which has applications in bioinformatics, text processing, and pattern recognition.

---

## **Real-life Analogy**

* Imagine you're a detective trying to find all the ways a suspect's DNA sequence (t) could be hidden within a larger DNA sample (s). The brute force approach would be to try all possible combinations of nucleotides from the sample, which is inefficient. The dynamic programming approach would be to build up the solution by finding how many ways smaller parts of the suspect's DNA can be hidden in smaller parts of the sample, which is much more efficient.

---

## **Summary**

* We solved the Distinct Subsequences problem by first considering a brute force recursive approach that tries all possible subsequences, resulting in O(2^m) time complexity. We then optimized using dynamic programming with memoization to avoid redundant computations, reducing the time complexity to O(m * n). We also explored a bottom-up dynamic programming approach and a space-optimized version that reduces the space complexity from O(m * n) to O(n). The key insight is to recognize that the number of distinct subsequences depends only on the number of distinct subsequences for smaller prefixes of s and t, which allows us to build the solution incrementally. 