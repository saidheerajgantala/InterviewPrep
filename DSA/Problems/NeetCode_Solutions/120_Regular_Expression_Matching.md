# 📝 Regular Expression Matching

## **Problem Statement**

* Given an input string `s` and a pattern `p`, implement regular expression matching with support for '.' and '*' where:
  * '.' Matches any single character.
  * '*' Matches zero or more of the preceding element.
* The matching should cover the entire input string (not partial).
* Example 1:
  * Input: s = "aa", p = "a"
  * Output: false
  * Explanation: "a" does not match the entire string "aa".
* Example 2:
  * Input: s = "aa", p = "a*"
  * Output: true
  * Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
* Example 3:
  * Input: s = "ab", p = ".*"
  * Output: true
  * Explanation: ".*" means "zero or more (*) of any character (.)".
* Constraints:
  * 1 <= s.length <= 20
  * 1 <= p.length <= 30
  * s contains only lowercase English letters.
  * p contains only lowercase English letters, '.', and '*'.
  * It is guaranteed for each appearance of the character '*', there will be a previous valid character to match.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine if a string matches a given pattern with special characters '.' and '*'
* The pattern '.' matches any single character
* The pattern '*' matches zero or more of the preceding element
* One approach would be to use recursion to try all possible matches
* For each position in the string and pattern, we need to decide how to match the current characters
* This suggests a recursive approach where we try different matching options

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes two indices i and j, representing the current positions in the string s and pattern p
2. Base cases:
   1. If we've reached the end of both s and p, return true (match complete)
   2. If we've reached the end of s but not p, check if the remaining pattern can match an empty string (e.g., "a*b*c*")
   3. If we've reached the end of p but not s, return false (pattern exhausted but string remains)
3. Check if the current characters match:
   1. If p[j+1] is '*', we have two choices:
      1. Skip the pattern (zero occurrences): recursively call with (i, j+2)
      2. Match the current character and try again: recursively call with (i+1, j) if s[i] matches p[j]
   2. If p[j+1] is not '*', we need to match the current characters:
      1. If s[i] matches p[j] (either they're the same or p[j] is '.'), recursively call with (i+1, j+1)
      2. Otherwise, return false
4. Return the result of the recursive calls

---

## **Step 3: Brute Force Implementation (Code)**

```python
def isMatch(s, p):
    def match(i, j):
        # Base cases
        if j == len(p):
            return i == len(s)
        
        # Check if the current characters match
        first_match = i < len(s) and (p[j] == s[i] or p[j] == '.')
        
        # If the next character is '*'
        if j + 1 < len(p) and p[j + 1] == '*':
            # Skip the pattern (zero occurrences) or match the current character
            return match(i, j + 2) or (first_match and match(i + 1, j))
        
        # If the next character is not '*', we need to match the current characters
        return first_match and match(i + 1, j + 1)
    
    return match(0, 0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^(m+n)) where m is the length of the string and n is the length of the pattern. In the worst case, we explore all possible combinations of matching, which is exponential.
* Space Complexity: O(m+n) for the recursion stack.
* This approach is inefficient for long strings and patterns due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For s = "aa", p = "a*":
  * match(0, 0):
    * first_match = true (s[0] == p[0])
    * p[1] is '*', so we have two choices:
      * Skip the pattern: match(0, 2) - This is out of bounds for p, so we check if i == len(s), which is false
      * Match the current character: match(1, 0) (since first_match is true)
        * first_match = true (s[1] == p[0])
        * p[1] is '*', so we have two choices:
          * Skip the pattern: match(1, 2) - This is out of bounds for p, so we check if i == len(s), which is true
          * Match the current character: match(2, 0) - This is out of bounds for s, so we return false
        * Return true (from match(1, 2))
      * Return true (from match(1, 0))
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
  * dp[0][j] depends on whether the pattern can match an empty string (e.g., "a*b*c*")
* For each position (i, j):
  * If p[j-1] is '*':
    * dp[i][j] = dp[i][j-2] (zero occurrences) or (dp[i-1][j] and s[i-1] matches p[j-2]) (one or more occurrences)
  * Otherwise:
    * dp[i][j] = dp[i-1][j-1] and s[i-1] matches p[j-1]
* The answer will be dp[m][n] where m is the length of s and n is the length of p

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Create a 2D array dp of size (m+1) x (n+1), where dp[i][j] represents whether the first i characters of s match the first j characters of p
2. Initialize dp[0][0] = true (base case: empty string matches empty pattern)
3. Initialize the first row (dp[0][j]) based on whether the pattern can match an empty string
4. For each position (i, j) from 1 to m, 1 to n:
   1. If p[j-1] is '*':
      1. dp[i][j] = dp[i][j-2] (zero occurrences)
      2. If s[i-1] matches p[j-2], dp[i][j] = dp[i][j] or dp[i-1][j] (one or more occurrences)
   2. Otherwise:
      1. If s[i-1] matches p[j-1], dp[i][j] = dp[i-1][j-1]
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
            dp[0][j] = dp[0][j - 2]
    
    # Fill the dp table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if p[j - 1] == '*':
                # Zero occurrences
                dp[i][j] = dp[i][j - 2]
                
                # One or more occurrences
                if p[j - 2] == '.' or p[j - 2] == s[i - 1]:
                    dp[i][j] = dp[i][j] or dp[i - 1][j]
            else:
                # Current characters match
                if p[j - 1] == '.' or p[j - 1] == s[i - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
    
    return dp[m][n]
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(m * n) where m is the length of the string and n is the length of the pattern. We fill a 2D table of size (m+1) x (n+1).
* Space Complexity: O(m * n) for the dp array.
* This approach is much more efficient than the brute force approach for long strings and patterns.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For s = "aa", p = "a*":
  * Initialize dp:
    ```
      |   | "" | a | *
    --|---|----|----|--
    "" | T | F | F
    a  | F | F | F
    a  | F | F | F
    ```
  * Initialize the first row:
    * dp[0][0] = true (empty string matches empty pattern)
    * dp[0][1] = false (empty string doesn't match "a")
    * dp[0][2] = dp[0][0] = true (since p[1] is '*', we can have zero occurrences of 'a')
    ```
      |   | "" | a | *
    --|---|----|----|--
    "" | T | F | T
    a  | F | F | F
    a  | F | F | F
    ```
  * Process dp[1][1]: s[0] matches p[0], so dp[1][1] = dp[0][0] = true
    ```
      |   | "" | a | *
    --|---|----|----|--
    "" | T | F | T
    a  | F | T | F
    a  | F | F | F
    ```
  * Process dp[1][2]: p[1] is '*', so dp[1][2] = dp[1][0] or (dp[0][2] and s[0] matches p[0]) = false or (true and true) = true
    ```
      |   | "" | a | *
    --|---|----|----|--
    "" | T | F | T
    a  | F | T | T
    a  | F | F | F
    ```
  * Process dp[2][1]: s[1] matches p[0], so dp[2][1] = dp[1][0] = false
    ```
      |   | "" | a | *
    --|---|----|----|--
    "" | T | F | T
    a  | F | T | T
    a  | F | F | F
    ```
  * Process dp[2][2]: p[1] is '*', so dp[2][2] = dp[2][0] or (dp[1][2] and s[1] matches p[0]) = false or (true and true) = true
    ```
      |   | "" | a | *
    --|---|----|----|--
    "" | T | F | T
    a  | F | T | T
    a  | F | F | T
    ```
  * dp[2][2] = true, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(2^(m+n)) | O(m+n) | Inefficient for long strings and patterns |
| Dynamic Programming | O(m * n) | O(m * n) | Efficient for medium-sized strings and patterns |

---

## **Final Takeaways**

* The key to efficiently solving the Regular Expression Matching problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using a bottom-up approach by building the solution from smaller subproblems.
* Special attention should be paid to the handling of the '*' character, which can match zero or more occurrences of the preceding element.
* The base case initialization is crucial, especially for patterns that can match an empty string.
* This problem is a classic example of string matching with dynamic programming, which has applications in text processing, compiler design, and bioinformatics.

---

## **Real-life Analogy**

* Imagine you're a text editor trying to find if a search pattern matches a document. The brute force approach would be to try all possible ways to match the pattern with the document, which is inefficient. The dynamic programming approach would be to build up the solution by finding if smaller parts of the pattern match smaller parts of the document, which is much more efficient.

---

## **Summary**

* We solved the Regular Expression Matching problem by first considering a brute force recursive approach that tries all possible matching options, resulting in O(2^(m+n)) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(m * n). The key insight is to recognize that the matching result for a prefix of the string and a prefix of the pattern depends only on the matching results for smaller prefixes, which allows us to build the solution incrementally. 