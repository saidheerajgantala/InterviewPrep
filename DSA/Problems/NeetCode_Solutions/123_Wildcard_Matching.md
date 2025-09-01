# 📝 Wildcard Matching

## **Problem Statement**

* Given an input string `s` and a pattern `p`, implement wildcard pattern matching with support for '?' and '*' where:
  * '?' Matches any single character.
  * '*' Matches any sequence of characters (including the empty sequence).
* The matching should cover the entire input string (not partial).
* Example 1:
  * Input: s = "aa", p = "a"
  * Output: false
  * Explanation: "a" does not match the entire string "aa".
* Example 2:
  * Input: s = "aa", p = "*"
  * Output: true
  * Explanation: '*' matches any sequence of characters.
* Example 3:
  * Input: s = "cb", p = "?a"
  * Output: false
  * Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
* Constraints:
  * 0 <= s.length, p.length <= 2000
  * s contains only lowercase English letters.
  * p contains only lowercase English letters, '?' or '*'.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine if a string matches a given pattern with special characters '?' and '*'
* The pattern '?' matches any single character
* The pattern '*' matches any sequence of characters (including the empty sequence)
* One approach would be to use recursion to try all possible matches
* For each position in the string and pattern, we need to decide how to match the current characters
* This suggests a recursive approach where we try different matching options

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes two indices i and j, representing the current positions in the string s and pattern p
2. Base cases:
   1. If we've reached the end of both s and p, return true (match complete)
   2. If we've reached the end of s but not p, check if the remaining pattern consists only of '*' characters
   3. If we've reached the end of p but not s, return false (pattern exhausted but string remains)
3. Check if the current characters match:
   1. If p[j] is '?', it matches any character in s, so recursively call with (i+1, j+1)
   2. If p[j] is '*', we have two choices:
      1. Skip the '*' (match empty sequence): recursively call with (i, j+1)
      2. Match one character and keep the '*': recursively call with (i+1, j)
   3. If p[j] is a regular character, check if it matches s[i] and recursively call with (i+1, j+1)
4. Return the result of the recursive calls

---

## **Step 3: Brute Force Implementation (Code)**

```python
def isMatch(s, p):
    def match(i, j):
        # Base cases
        if j == len(p):
            return i == len(s)
        if i == len(s):
            return all(p[k] == '*' for k in range(j, len(p)))
        
        # Check if the current characters match
        if p[j] == '?' or p[j] == s[i]:
            return match(i + 1, j + 1)
        
        # If the current pattern character is '*'
        if p[j] == '*':
            # Either skip the '*' or match one character and keep the '*'
            return match(i, j + 1) or match(i + 1, j)
        
        return False
    
    return match(0, 0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^(m+n)) where m is the length of the string and n is the length of the pattern. In the worst case, we explore all possible combinations of matching, which is exponential.
* Space Complexity: O(m+n) for the recursion stack.
* This approach is inefficient for long strings and patterns due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For s = "aa", p = "*":
  * match(0, 0): p[0] is '*'
    * Skip the '*': match(0, 1) - This is out of bounds for p, so we check if i == len(s), which is false
    * Match one character and keep the '*': match(1, 0)
      * Skip the '*': match(1, 1) - This is out of bounds for p, so we check if i == len(s), which is false
      * Match one character and keep the '*': match(2, 0)
        * Skip the '*': match(2, 1) - This is out of bounds for p, so we check if i == len(s), which is true
        * Return true
      * Return true
    * Return true
  * Return true
* There's redundant computation in the recursive calls, especially for overlapping subproblems like match(1, 0)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the matching result for a prefix of the string and a prefix of the pattern depends only on the matching results for smaller prefixes.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently determine if a prefix of the string matches a prefix of the pattern.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let dp[i][j] represent whether the first i characters of s match the first j characters of p
* Base cases:
  * dp[0][0] = true (empty string matches empty pattern)
  * dp[0][j] depends on whether the pattern can match an empty string (e.g., "*" can match an empty string)
* For each position (i, j):
  * If p[j-1] is '?', dp[i][j] = dp[i-1][j-1] (matches any single character)
  * If p[j-1] is '*', dp[i][j] = dp[i][j-1] (skip '*') or dp[i-1][j] (match one character and keep '*')
  * If p[j-1] is a regular character, dp[i][j] = dp[i-1][j-1] and s[i-1] == p[j-1]
* The answer will be dp[m][n] where m is the length of s and n is the length of p

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Create a 2D array dp of size (m+1) x (n+1), where dp[i][j] represents whether the first i characters of s match the first j characters of p
2. Initialize dp[0][0] = true (base case: empty string matches empty pattern)
3. Initialize the first row (dp[0][j]) based on whether the pattern can match an empty string
4. For each position (i, j) from 1 to m, 1 to n:
   1. If p[j-1] is '?', dp[i][j] = dp[i-1][j-1]
   2. If p[j-1] is '*', dp[i][j] = dp[i][j-1] or dp[i-1][j]
   3. If p[j-1] is a regular character, dp[i][j] = dp[i-1][j-1] and s[i-1] == p[j-1]
5. Return dp[m][n]

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def isMatch(s, p):
    m, n = len(s), len(p)
    
    # Create a 2D array filled with False
    dp = [[False] * (n + 1) for _ in range(m + 1)]
    
    # Base case: empty string matches empty pattern
    dp[0][0] = True
    
    # Initialize the first row (dp[0][j])
    for j in range(1, n + 1):
        if p[j - 1] == '*':
            dp[0][j] = dp[0][j - 1]
    
    # Fill the dp table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if p[j - 1] == '?':
                dp[i][j] = dp[i - 1][j - 1]
            elif p[j - 1] == '*':
                dp[i][j] = dp[i][j - 1] or dp[i - 1][j]
            else:
                dp[i][j] = dp[i - 1][j - 1] and s[i - 1] == p[j - 1]
    
    return dp[m][n]
```

### **Space Optimization**

We can optimize the space complexity to O(n) by using two 1D arrays:

```python
def isMatch(s, p):
    m, n = len(s), len(p)
    
    # Create two 1D arrays for current and previous row
    prev = [False] * (n + 1)
    curr = [False] * (n + 1)
    
    # Base case: empty string matches empty pattern
    prev[0] = True
    
    # Initialize the first row (prev[j])
    for j in range(1, n + 1):
        if p[j - 1] == '*':
            prev[j] = prev[j - 1]
    
    # Fill the dp table
    for i in range(1, m + 1):
        curr[0] = False  # Reset current row
        for j in range(1, n + 1):
            if p[j - 1] == '?':
                curr[j] = prev[j - 1]
            elif p[j - 1] == '*':
                curr[j] = curr[j - 1] or prev[j]
            else:
                curr[j] = prev[j - 1] and s[i - 1] == p[j - 1]
        
        # Swap current and previous rows
        prev, curr = curr, prev
    
    return prev[n]
```

### **Greedy Approach**

There's also a greedy approach that can solve this problem in O(m * n) time but with better average-case performance:

```python
def isMatch(s, p):
    s_idx = p_idx = 0
    star_idx = s_backup = -1
    
    while s_idx < len(s):
        # If the current characters match or pattern has '?'
        if p_idx < len(p) and (p[p_idx] == '?' or p[p_idx] == s[s_idx]):
            s_idx += 1
            p_idx += 1
        # If pattern has '*'
        elif p_idx < len(p) and p[p_idx] == '*':
            # Save the position of '*' and the current position in s
            star_idx = p_idx
            s_backup = s_idx
            p_idx += 1
        # If we have seen a '*' before
        elif star_idx != -1:
            # Backtrack: use the '*' to match one more character in s
            p_idx = star_idx + 1
            s_backup += 1
            s_idx = s_backup
        else:
            return False
    
    # Skip remaining '*' in pattern
    while p_idx < len(p) and p[p_idx] == '*':
        p_idx += 1
    
    # If we've reached the end of the pattern, we have a match
    return p_idx == len(p)
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(m * n) where m is the length of the string and n is the length of the pattern. We fill a 2D table of size (m+1) x (n+1).
* Space Complexity: O(m * n) for the dp array.
* This approach is much more efficient than the brute force approach for long strings and patterns.

### **Complexity Analysis (Space-Optimized Version)**

* Time Complexity: O(m * n) where m is the length of the string and n is the length of the pattern.
* Space Complexity: O(n) for the 1D dp arrays.
* This approach is even more space-efficient while maintaining the same time complexity.

### **Complexity Analysis (Greedy Approach)**

* Time Complexity: O(m * n) in the worst case, but often better in practice.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is the most space-efficient and can be faster in practice.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For s = "aa", p = "*":
  * Initialize dp:
    ```
      |   | "" | *
    --|---|----|-
    "" | T | F
    a  | F | F
    a  | F | F
    ```
  * Initialize the first row:
    * dp[0][0] = true (empty string matches empty pattern)
    * dp[0][1] = dp[0][0] = true (since p[0] is '*', it can match an empty string)
    ```
      |   | "" | *
    --|---|----|-
    "" | T | T
    a  | F | F
    a  | F | F
    ```
  * Process dp[1][1]: p[0] is '*', so dp[1][1] = dp[1][0] or dp[0][1] = false or true = true
    ```
      |   | "" | *
    --|---|----|-
    "" | T | T
    a  | F | T
    a  | F | F
    ```
  * Process dp[2][1]: p[0] is '*', so dp[2][1] = dp[2][0] or dp[1][1] = false or true = true
    ```
      |   | "" | *
    --|---|----|-
    "" | T | T
    a  | F | T
    a  | F | T
    ```
  * dp[2][1] = true, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(2^(m+n)) | O(m+n) | Inefficient for long strings and patterns |
| Dynamic Programming | O(m * n) | O(m * n) | Efficient for medium-sized strings and patterns |
| Dynamic Programming (Space-Optimized) | O(m * n) | O(n) | More space-efficient approach |
| Greedy Approach | O(m * n) | O(1) | Most space-efficient, often faster in practice |

---

## **Final Takeaways**

* The key to efficiently solving the Wildcard Matching problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using either a bottom-up dynamic programming approach or a greedy approach.
* The space optimization from a 2D array to a 1D array is possible because we only need the results from the previous row to compute the current row.
* The greedy approach can be more efficient in practice, especially for patterns with few wildcards.
* This problem is similar to the Regular Expression Matching problem, but with simpler wildcard rules.

---

## **Real-life Analogy**

* Imagine you're a file system trying to find all files that match a pattern like "*.txt" or "doc?.pdf". The brute force approach would be to try all possible ways to match the pattern with each file name, which is inefficient. The dynamic programming approach would be to build up the solution by finding if smaller parts of the pattern match smaller parts of the file name, which is much more efficient.

---

## **Summary**

* We solved the Wildcard Matching problem by first considering a brute force recursive approach that tries all possible matching options, resulting in O(2^(m+n)) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(m * n). We also explored a space-optimized version that reduces the space complexity from O(m * n) to O(n), and a greedy approach that uses only O(1) extra space. The key insight is to recognize that the matching result for a prefix of the string and a prefix of the pattern depends only on the matching results for smaller prefixes, which allows us to build the solution incrementally. 