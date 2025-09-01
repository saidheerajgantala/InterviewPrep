# 📝 Longest Common Subsequence

## **Problem Statement**

* Given two strings `text1` and `text2`, return the length of their longest common subsequence. If there is no common subsequence, return 0.
* A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.
* For example, "ace" is a subsequence of "abcde".
* A common subsequence of two strings is a subsequence that is common to both strings.
* Example 1:
  * Input: text1 = "abcde", text2 = "ace"
  * Output: 3
  * Explanation: The longest common subsequence is "ace" and its length is 3.
* Example 2:
  * Input: text1 = "abc", text2 = "abc"
  * Output: 3
  * Explanation: The longest common subsequence is "abc" and its length is 3.
* Example 3:
  * Input: text1 = "abc", text2 = "def"
  * Output: 0
  * Explanation: There is no such common subsequence, so the result is 0.
* Constraints:
  * 1 <= text1.length, text2.length <= 1000
  * text1 and text2 consist of only lowercase English characters.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the length of the longest common subsequence (LCS) between two strings
* A subsequence is formed by removing some characters from the original string without changing the order
* One approach would be to generate all subsequences of both strings and find the longest common one
* However, this would be inefficient as there are 2^n possible subsequences for a string of length n
* Instead, we can use recursion to solve this problem by comparing characters from both strings

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes two indices i and j, representing the current positions in text1 and text2
2. Base case: If either index reaches the end of its string, return 0 (no more characters to match)
3. If the characters at the current positions match:
   1. Include this character in the LCS and recursively solve for the rest of the strings (i+1, j+1)
   2. Add 1 to the result (for the matching character)
4. If the characters don't match:
   1. Try skipping a character in text1 and recursively solve (i+1, j)
   2. Try skipping a character in text2 and recursively solve (i, j+1)
   3. Take the maximum of these two results
5. Return the result

---

## **Step 3: Brute Force Implementation (Code)**

```python
def longestCommonSubsequence(text1, text2):
    def lcs(i, j):
        # Base case: if either string is empty
        if i == len(text1) or j == len(text2):
            return 0
        
        # If characters match, include them in the LCS
        if text1[i] == text2[j]:
            return 1 + lcs(i + 1, j + 1)
        
        # If characters don't match, try skipping one character from either string
        return max(lcs(i + 1, j), lcs(i, j + 1))
    
    return lcs(0, 0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^(m+n)) where m is the length of text1 and n is the length of text2. In the worst case, we explore all possible combinations of characters.
* Space Complexity: O(m+n) for the recursion stack.
* This approach is inefficient for long strings due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For text1 = "abcde", text2 = "ace":
  * lcs(0, 0):
    * text1[0] == text2[0], so 1 + lcs(1, 1)
      * lcs(1, 1):
        * text1[1] != text2[1], so max(lcs(2, 1), lcs(1, 2))
          * lcs(2, 1):
            * text1[2] == text2[1], so 1 + lcs(3, 2)
              * ... (many recursive calls)
          * lcs(1, 2):
            * text1[1] != text2[2], so max(lcs(2, 2), lcs(1, 3))
              * ... (many recursive calls)
* There's redundant computation in the recursive calls, especially for overlapping subproblems like lcs(2, 2)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the longest common subsequence of two strings depends only on the longest common subsequences of their prefixes.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently calculate the longest common subsequence of prefixes of the two strings.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let dp[i][j] represent the length of the longest common subsequence of text1[0...i-1] and text2[0...j-1]
* Base case: dp[0][j] = dp[i][0] = 0 (the LCS of an empty string with any string is 0)
* For each i from 1 to m and j from 1 to n:
  * If text1[i-1] == text2[j-1], dp[i][j] = 1 + dp[i-1][j-1] (include the matching character)
  * Otherwise, dp[i][j] = max(dp[i-1][j], dp[i][j-1]) (skip a character from either string)
* The answer will be dp[m][n]

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Create a 2D array dp of size (m+1) x (n+1), initialized with 0
2. For each i from 1 to m:
   1. For each j from 1 to n:
      1. If text1[i-1] == text2[j-1], dp[i][j] = 1 + dp[i-1][j-1]
      2. Otherwise, dp[i][j] = max(dp[i-1][j], dp[i][j-1])
3. Return dp[m][n]

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def longestCommonSubsequence(text1, text2):
    m, n = len(text1), len(text2)
    
    # Create a 2D array filled with 0
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Fill the dp table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i - 1] == text2[j - 1]:
                dp[i][j] = 1 + dp[i - 1][j - 1]
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    
    return dp[m][n]
```

### **Space Optimization**

We can optimize the space complexity to O(min(m, n)) by using a 1D array:

```python
def longestCommonSubsequence(text1, text2):
    # Ensure text1 is the shorter string for space optimization
    if len(text1) > len(text2):
        text1, text2 = text2, text1
    
    m, n = len(text1), len(text2)
    
    # Create two 1D arrays for current and previous row
    prev = [0] * (m + 1)
    curr = [0] * (m + 1)
    
    # Fill the dp table
    for j in range(1, n + 1):
        for i in range(1, m + 1):
            if text1[i - 1] == text2[j - 1]:
                curr[i] = 1 + prev[i - 1]
            else:
                curr[i] = max(prev[i], curr[i - 1])
        
        # Update previous row
        prev, curr = curr, prev
    
    return prev[m]
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(m * n) where m is the length of text1 and n is the length of text2. We fill a 2D table of size (m+1) x (n+1).
* Space Complexity: O(m * n) for the dp array.
* This approach is much more efficient than the brute force approach for long strings.

### **Complexity Analysis (Space-Optimized Version)**

* Time Complexity: O(m * n) where m is the length of text1 and n is the length of text2.
* Space Complexity: O(min(m, n)) for the 1D dp arrays.
* This approach is even more space-efficient while maintaining the same time complexity.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For text1 = "abcde", text2 = "ace":
  * Initialize dp:
    ```
      | "" | a | c | e
    --|---|---|---|---
    "" | 0 | 0 | 0 | 0
    a  | 0 | 0 | 0 | 0
    b  | 0 | 0 | 0 | 0
    c  | 0 | 0 | 0 | 0
    d  | 0 | 0 | 0 | 0
    e  | 0 | 0 | 0 | 0
    ```
  * Process dp[1][1]: text1[0] == text2[0], so dp[1][1] = 1 + dp[0][0] = 1
    ```
      | "" | a | c | e
    --|---|---|---|---
    "" | 0 | 0 | 0 | 0
    a  | 0 | 1 | 0 | 0
    b  | 0 | 0 | 0 | 0
    c  | 0 | 0 | 0 | 0
    d  | 0 | 0 | 0 | 0
    e  | 0 | 0 | 0 | 0
    ```
  * Process dp[1][2]: text1[0] != text2[1], so dp[1][2] = max(dp[0][2], dp[1][1]) = max(0, 1) = 1
    ```
      | "" | a | c | e
    --|---|---|---|---
    "" | 0 | 0 | 0 | 0
    a  | 0 | 1 | 1 | 0
    b  | 0 | 0 | 0 | 0
    c  | 0 | 0 | 0 | 0
    d  | 0 | 0 | 0 | 0
    e  | 0 | 0 | 0 | 0
    ```
  * ... (continue filling the table)
  * Final dp table:
    ```
      | "" | a | c | e
    --|---|---|---|---
    "" | 0 | 0 | 0 | 0
    a  | 0 | 1 | 1 | 1
    b  | 0 | 1 | 1 | 1
    c  | 0 | 1 | 2 | 2
    d  | 0 | 1 | 2 | 2
    e  | 0 | 1 | 2 | 3
    ```
  * dp[5][3] = 3, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(2^(m+n)) | O(m+n) | Inefficient for long strings |
| Dynamic Programming (2D) | O(m * n) | O(m * n) | Efficient for medium-sized strings |
| Dynamic Programming (1D) | O(m * n) | O(min(m, n)) | Space-optimized version |

---

## **Final Takeaways**

* The key to efficiently solving the Longest Common Subsequence problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using a bottom-up approach by building the solution from smaller subproblems.
* The space optimization from a 2D array to a 1D array is possible because we only need the results from the previous row to compute the current row.
* This problem is a classic example of dynamic programming and is often used as a building block for more complex string algorithms.
* The LCS problem has applications in various fields, such as bioinformatics (DNA sequence alignment), version control systems (diff tools), and natural language processing.

---

## **Real-life Analogy**

* Imagine you're comparing two versions of a document to find the common parts that haven't changed. The brute force approach would be to try all possible combinations of paragraphs from both documents, which is inefficient. The dynamic programming approach would be to compare the documents paragraph by paragraph, keeping track of the longest common sequence found so far, which is much more efficient. This is similar to how "diff" tools work in version control systems.

---

## **Summary**

* We solved the Longest Common Subsequence problem by first considering a brute force recursive approach that tries all possible combinations of characters, resulting in O(2^(m+n)) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(m * n). Finally, we explored a space optimization that reduces the space complexity from O(m * n) to O(min(m, n)). The key insight is to recognize that the longest common subsequence of two strings depends only on the longest common subsequences of their prefixes, which allows us to build the solution incrementally. 