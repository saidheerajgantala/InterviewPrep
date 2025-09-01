# 📝 Interleaving String

## **Problem Statement**

* Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an interleaving of `s1` and `s2`.
* An interleaving of two strings `s` and `t` is a configuration where `s` and `t` are divided into `n` and `m` substrings respectively, such that:
  * s = s1 + s2 + ... + sn
  * t = t1 + t2 + ... + tm
  * |n - m| <= 1
  * The interleaving is s1 + t1 + s2 + t2 + s3 + t3 + ... or t1 + s1 + t2 + s2 + t3 + s3 + ...
* Note: `a + b` is the concatenation of strings `a` and `b`.
* Example 1:
  * Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
  * Output: true
  * Explanation: One way to obtain s3 is:
    * Split s1 into s1 = "aa" + "bc" + "c", and s2 into s2 = "dbbc" + "a".
    * Interleaving the two splits, we get "aa" + "dbbc" + "bc" + "a" + "c" = "aadbbcbcac".
    * Since s3 can be obtained by interleaving s1 and s2, we return true.
* Example 2:
  * Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
  * Output: false
  * Explanation: Notice how it is impossible to interleave s2 with any other string to obtain s3.
* Example 3:
  * Input: s1 = "", s2 = "", s3 = ""
  * Output: true
* Constraints:
  * 0 <= s1.length, s2.length <= 100
  * 0 <= s3.length <= 200
  * s1, s2, and s3 consist of lowercase English letters.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine if s3 can be formed by interleaving s1 and s2
* One approach would be to try all possible ways to interleave s1 and s2 and check if any of them match s3
* At each step, we have two choices: take the next character from s1 or take the next character from s2
* This suggests a recursive approach where we try both options at each step
* We need to keep track of our current positions in s1, s2, and s3

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes the current positions in s1, s2, and s3
2. Base case: If we've processed all characters in s3, check if we've also processed all characters in s1 and s2
3. If the current character in s1 matches the current character in s3, recursively call the function with the next positions in s1 and s3
4. If the current character in s2 matches the current character in s3, recursively call the function with the next positions in s2 and s3
5. Return true if either recursive call returns true, otherwise return false
6. Start the recursion with all positions at 0

---

## **Step 3: Brute Force Implementation (Code)**

```python
def isInterleave(s1, s2, s3):
    if len(s1) + len(s2) != len(s3):
        return False
    
    def dfs(i, j, k):
        # Base case: if we've processed all characters in s3
        if k == len(s3):
            return i == len(s1) and j == len(s2)
        
        # Try taking a character from s1
        if i < len(s1) and s1[i] == s3[k] and dfs(i + 1, j, k + 1):
            return True
        
        # Try taking a character from s2
        if j < len(s2) and s2[j] == s3[k] and dfs(i, j + 1, k + 1):
            return True
        
        return False
    
    return dfs(0, 0, 0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^(m+n)) where m is the length of s1 and n is the length of s2. In the worst case, we explore all possible ways to interleave the two strings.
* Space Complexity: O(m+n) for the recursion stack.
* This approach is inefficient for long strings due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For s1 = "aab", s2 = "aac", s3 = "aabcaa":
  * dfs(0, 0, 0):
    * s1[0] = 'a' matches s3[0], try dfs(1, 0, 1)
      * s1[1] = 'a' matches s3[1], try dfs(2, 0, 2)
        * s1[2] = 'b' matches s3[2], try dfs(3, 0, 3)
          * s2[0] = 'a' doesn't match s3[3] = 'c', skip
          * Return false
        * s2[0] = 'a' doesn't match s3[2] = 'b', skip
        * Return false
      * s2[0] = 'a' matches s3[1], try dfs(1, 1, 2)
        * ... (similar recursive calls)
    * s2[0] = 'a' matches s3[0], try dfs(0, 1, 1)
      * ... (similar recursive calls)
* There's redundant computation in the recursive calls, especially for overlapping subproblems like dfs(1, 1, 2)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the ability to interleave strings up to certain positions depends only on the ability to interleave the prefixes of those strings.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently determine if prefixes of s1 and s2 can be interleaved to form a prefix of s3.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let dp[i][j] represent whether the first i characters of s1 and the first j characters of s2 can be interleaved to form the first i+j characters of s3
* Base case: dp[0][0] = true (empty strings can be interleaved to form an empty string)
* For each i and j:
  * If the current character in s1 matches the current character in s3, dp[i][j] = dp[i-1][j]
  * If the current character in s2 matches the current character in s3, dp[i][j] = dp[i][j-1]
* The answer will be dp[m][n] where m is the length of s1 and n is the length of s2

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. If the length of s3 is not equal to the sum of the lengths of s1 and s2, return false
2. Create a 2D boolean array dp of size (m+1) x (n+1), where dp[i][j] represents whether the first i characters of s1 and the first j characters of s2 can be interleaved to form the first i+j characters of s3
3. Initialize dp[0][0] = true (base case: empty strings can be interleaved to form an empty string)
4. For each i from 0 to m:
   1. For each j from 0 to n:
      1. If i > 0 and s1[i-1] matches s3[i+j-1], dp[i][j] = dp[i][j] or dp[i-1][j]
      2. If j > 0 and s2[j-1] matches s3[i+j-1], dp[i][j] = dp[i][j] or dp[i][j-1]
5. Return dp[m][n]

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def isInterleave(s1, s2, s3):
    m, n, l = len(s1), len(s2), len(s3)
    
    # Check if the lengths match
    if m + n != l:
        return False
    
    # dp[i][j] represents whether the first i characters of s1 and the first j characters of s2
    # can be interleaved to form the first i+j characters of s3
    dp = [[False] * (n + 1) for _ in range(m + 1)]
    
    # Base case: empty strings can be interleaved to form an empty string
    dp[0][0] = True
    
    # Fill the first row: only characters from s2
    for j in range(1, n + 1):
        dp[0][j] = dp[0][j-1] and s2[j-1] == s3[j-1]
    
    # Fill the first column: only characters from s1
    for i in range(1, m + 1):
        dp[i][0] = dp[i-1][0] and s1[i-1] == s3[i-1]
    
    # Fill the rest of the dp table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            # Current character in s3 matches current character in s1
            if s1[i-1] == s3[i+j-1]:
                dp[i][j] = dp[i][j] or dp[i-1][j]
            
            # Current character in s3 matches current character in s2
            if s2[j-1] == s3[i+j-1]:
                dp[i][j] = dp[i][j] or dp[i][j-1]
    
    return dp[m][n]
```

### **Space Optimization**

We can optimize the space complexity to O(n) by using a 1D array:

```python
def isInterleave(s1, s2, s3):
    m, n, l = len(s1), len(s2), len(s3)
    
    # Check if the lengths match
    if m + n != l:
        return False
    
    # dp[j] represents whether the first i characters of s1 and the first j characters of s2
    # can be interleaved to form the first i+j characters of s3
    dp = [False] * (n + 1)
    
    # Base case: empty strings can be interleaved to form an empty string
    dp[0] = True
    
    # Fill the first row: only characters from s2
    for j in range(1, n + 1):
        dp[j] = dp[j-1] and s2[j-1] == s3[j-1]
    
    # Fill the rest of the dp table
    for i in range(1, m + 1):
        dp[0] = dp[0] and s1[i-1] == s3[i-1]
        for j in range(1, n + 1):
            dp[j] = (dp[j] and s1[i-1] == s3[i+j-1]) or (dp[j-1] and s2[j-1] == s3[i+j-1])
    
    return dp[n]
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(m * n) where m is the length of s1 and n is the length of s2. We fill a 2D table of size (m+1) x (n+1).
* Space Complexity: O(m * n) for the dp array.
* This approach is much more efficient than the brute force approach for long strings.

### **Complexity Analysis (Space-Optimized Version)**

* Time Complexity: O(m * n) where m is the length of s1 and n is the length of s2.
* Space Complexity: O(n) for the 1D dp array.
* This approach is even more space-efficient while maintaining the same time complexity.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For s1 = "aab", s2 = "aac", s3 = "aaabac":
  * Initialize dp[0][0] = true
  * Fill the first row:
    * dp[0][1] = dp[0][0] && s2[0] == s3[0] = true && 'a' == 'a' = true
    * dp[0][2] = dp[0][1] && s2[1] == s3[1] = true && 'a' == 'a' = true
    * dp[0][3] = dp[0][2] && s2[2] == s3[2] = true && 'c' == 'a' = false
  * Fill the first column:
    * dp[1][0] = dp[0][0] && s1[0] == s3[0] = true && 'a' == 'a' = true
    * dp[2][0] = dp[1][0] && s1[1] == s3[1] = true && 'a' == 'a' = true
    * dp[3][0] = dp[2][0] && s1[2] == s3[2] = true && 'b' == 'a' = false
  * Fill the rest of the dp table:
    * dp[1][1]: s1[0] == s3[1] = 'a' == 'a', dp[1][1] = dp[0][1] = true
                s2[0] == s3[1] = 'a' == 'a', dp[1][1] = dp[1][0] = true
    * dp[1][2]: s1[0] == s3[2] = 'a' == 'a', dp[1][2] = dp[0][2] = true
                s2[1] == s3[2] = 'a' == 'a', dp[1][2] = dp[1][1] = true
    * ... (continue filling the table)
  * dp[3][3] = true, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(2^(m+n)) | O(m+n) | Inefficient for long strings |
| Dynamic Programming (2D) | O(m * n) | O(m * n) | Efficient for medium-sized strings |
| Dynamic Programming (1D) | O(m * n) | O(n) | Space-optimized version |

---

## **Final Takeaways**

* The key to efficiently solving the Interleaving String problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using a bottom-up approach by building the solution from smaller subproblems.
* The space optimization from a 2D array to a 1D array is possible because we only need the results from the previous row to compute the current row.
* It's important to check if the lengths match before proceeding with the algorithm.
* This problem demonstrates how dynamic programming can transform an exponential-time algorithm into a polynomial-time one.

---

## **Real-life Analogy**

* Imagine you're trying to merge two lines of people into a single line while maintaining the relative order of people within each original line. The brute force approach would be to try all possible ways to merge the lines, which is inefficient. The dynamic programming approach would be to keep track of whether the first i people from line 1 and the first j people from line 2 can be merged to form the first i+j people in the merged line, which is much more efficient.

---

## **Summary**

* We solved the Interleaving String problem by first considering a brute force recursive approach that tries all possible ways to interleave the two strings, resulting in O(2^(m+n)) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(m * n). Finally, we explored a space optimization that reduces the space complexity from O(m * n) to O(n). The key insight is to recognize that the ability to interleave strings up to certain positions depends only on the ability to interleave the prefixes of those strings, which allows us to build the solution incrementally. 