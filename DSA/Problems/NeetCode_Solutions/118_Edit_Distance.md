# 📝 Edit Distance

## **Problem Statement**

* Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` to `word2`.
* You have the following three operations permitted on a word:
  * Insert a character
  * Delete a character
  * Replace a character
* Example 1:
  * Input: word1 = "horse", word2 = "ros"
  * Output: 3
  * Explanation: 
    * horse -> rorse (replace 'h' with 'r')
    * rorse -> rose (remove 'r')
    * rose -> ros (remove 'e')
* Example 2:
  * Input: word1 = "intention", word2 = "execution"
  * Output: 5
  * Explanation: 
    * intention -> inention (remove 't')
    * inention -> enention (replace 'i' with 'e')
    * enention -> exention (replace 'n' with 'x')
    * exention -> exection (replace 'n' with 'c')
    * exection -> execution (insert 'u')
* Constraints:
  * 0 <= word1.length, word2.length <= 500
  * word1 and word2 consist of lowercase English letters.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the minimum number of operations to convert word1 to word2
* At each step, we have three choices: insert, delete, or replace a character
* One approach would be to use recursion to try all possible combinations of operations
* For each position in both strings, we need to decide which operation to perform
* This suggests a recursive approach where we try all possible operations at each step

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes two indices i and j, representing the current positions in word1 and word2
2. Base cases:
   1. If i reaches the end of word1, we need to insert all remaining characters from word2
   2. If j reaches the end of word2, we need to delete all remaining characters from word1
3. If the characters at the current positions match, no operation is needed, so recursively solve for (i+1, j+1)
4. Otherwise, try all three operations and take the minimum:
   1. Insert: recursively solve for (i, j+1) and add 1
   2. Delete: recursively solve for (i+1, j) and add 1
   3. Replace: recursively solve for (i+1, j+1) and add 1
5. Return the minimum of these three options

---

## **Step 3: Brute Force Implementation (Code)**

```python
def minDistance(word1, word2):
    def edit_distance(i, j):
        # Base cases
        if i == len(word1):
            return len(word2) - j
        if j == len(word2):
            return len(word1) - i
        
        # If characters match, no operation needed
        if word1[i] == word2[j]:
            return edit_distance(i + 1, j + 1)
        
        # Try all three operations and take the minimum
        insert = 1 + edit_distance(i, j + 1)
        delete = 1 + edit_distance(i + 1, j)
        replace = 1 + edit_distance(i + 1, j + 1)
        
        return min(insert, delete, replace)
    
    return edit_distance(0, 0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(3^(m+n)) where m is the length of word1 and n is the length of word2. In the worst case, we explore all possible combinations of operations, which is exponential.
* Space Complexity: O(m+n) for the recursion stack.
* This approach is inefficient for long strings due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For word1 = "horse", word2 = "ros":
  * edit_distance(0, 0):
    * word1[0] != word2[0], so try all operations:
      * insert = 1 + edit_distance(0, 1):
        * ... (many recursive calls)
      * delete = 1 + edit_distance(1, 0):
        * ... (many recursive calls)
      * replace = 1 + edit_distance(1, 1):
        * word1[1] == word2[1], so edit_distance(2, 2):
          * ... (many recursive calls)
    * return min(insert, delete, replace)
  * ... (many more recursive calls)
* There's redundant computation in the recursive calls, especially for overlapping subproblems like edit_distance(2, 2)

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the minimum edit distance between prefixes of the two strings depends only on the minimum edit distance between smaller prefixes.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently calculate the minimum edit distance between prefixes of the two strings.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let dp[i][j] represent the minimum number of operations to convert word1[0...i-1] to word2[0...j-1]
* Base cases:
  * dp[i][0] = i (delete i characters from word1)
  * dp[0][j] = j (insert j characters from word2)
* For each position (i, j):
  * If word1[i-1] == word2[j-1], dp[i][j] = dp[i-1][j-1] (no operation needed)
  * Otherwise, dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) (minimum of insert, delete, replace)
* The answer will be dp[m][n] where m is the length of word1 and n is the length of word2

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Create a 2D array dp of size (m+1) x (n+1), where dp[i][j] represents the minimum number of operations to convert word1[0...i-1] to word2[0...j-1]
2. Initialize the base cases:
   1. dp[i][0] = i for all i from 0 to m
   2. dp[0][j] = j for all j from 0 to n
3. For each position (i, j) from 1 to m, 1 to n:
   1. If word1[i-1] == word2[j-1], dp[i][j] = dp[i-1][j-1]
   2. Otherwise, dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
4. Return dp[m][n]

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def minDistance(word1, word2):
    m, n = len(word1), len(word2)
    
    # Create a 2D array filled with 0
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Base cases
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
    
    # Fill the dp table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(dp[i - 1][j],      # Delete
                                   dp[i][j - 1],      # Insert
                                   dp[i - 1][j - 1])  # Replace
    
    return dp[m][n]
```

### **Space Optimization**

We can optimize the space complexity to O(min(m, n)) by using two 1D arrays:

```python
def minDistance(word1, word2):
    m, n = len(word1), len(word2)
    
    # Ensure word1 is the shorter string for space optimization
    if m > n:
        word1, word2 = word2, word1
        m, n = n, m
    
    # Create two 1D arrays for current and previous row
    prev = list(range(n + 1))
    curr = [0] * (n + 1)
    
    # Fill the dp table
    for i in range(1, m + 1):
        curr[0] = i
        for j in range(1, n + 1):
            if word1[i - 1] == word2[j - 1]:
                curr[j] = prev[j - 1]
            else:
                curr[j] = 1 + min(prev[j],      # Delete
                                  curr[j - 1],  # Insert
                                  prev[j - 1])  # Replace
        
        # Update previous row
        prev, curr = curr, prev
    
    return prev[n]
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(m * n) where m is the length of word1 and n is the length of word2. We fill a 2D table of size (m+1) x (n+1).
* Space Complexity: O(m * n) for the dp array.
* This approach is much more efficient than the brute force approach for long strings.

### **Complexity Analysis (Space-Optimized Version)**

* Time Complexity: O(m * n) where m is the length of word1 and n is the length of word2.
* Space Complexity: O(min(m, n)) for the 1D dp arrays.
* This approach is even more space-efficient while maintaining the same time complexity.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For word1 = "horse", word2 = "ros":
  * Initialize dp:
    ```
      |   | "" | r  | o  | s
    --|---|----|----|----|-
    "" | 0 | 1  | 2  | 3
    h  | 1 | ?  | ?  | ?
    o  | 2 | ?  | ?  | ?
    r  | 3 | ?  | ?  | ?
    s  | 4 | ?  | ?  | ?
    e  | 5 | ?  | ?  | ?
    ```
  * Process dp[1][1]: word1[0] != word2[0], so dp[1][1] = 1 + min(dp[0][1], dp[1][0], dp[0][0]) = 1 + min(1, 1, 0) = 1
    ```
      |   | "" | r  | o  | s
    --|---|----|----|----|-
    "" | 0 | 1  | 2  | 3
    h  | 1 | 1  | ?  | ?
    o  | 2 | ?  | ?  | ?
    r  | 3 | ?  | ?  | ?
    s  | 4 | ?  | ?  | ?
    e  | 5 | ?  | ?  | ?
    ```
  * Process dp[1][2]: word1[0] != word2[1], so dp[1][2] = 1 + min(dp[0][2], dp[1][1], dp[0][1]) = 1 + min(2, 1, 1) = 2
    ```
      |   | "" | r  | o  | s
    --|---|----|----|----|-
    "" | 0 | 1  | 2  | 3
    h  | 1 | 1  | 2  | ?
    o  | 2 | ?  | ?  | ?
    r  | 3 | ?  | ?  | ?
    s  | 4 | ?  | ?  | ?
    e  | 5 | ?  | ?  | ?
    ```
  * ... (continue filling the table)
  * Final dp table:
    ```
      |   | "" | r  | o  | s
    --|---|----|----|----|-
    "" | 0 | 1  | 2  | 3
    h  | 1 | 1  | 2  | 3
    o  | 2 | 2  | 1  | 2
    r  | 3 | 2  | 2  | 2
    s  | 4 | 3  | 3  | 2
    e  | 5 | 4  | 4  | 3
    ```
  * dp[5][3] = 3, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(3^(m+n)) | O(m+n) | Inefficient for long strings |
| Dynamic Programming (2D) | O(m * n) | O(m * n) | Efficient for medium-sized strings |
| Dynamic Programming (1D) | O(m * n) | O(min(m, n)) | Space-optimized version |

---

## **Final Takeaways**

* The key to efficiently solving the Edit Distance problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using a bottom-up approach by building the solution from smaller subproblems.
* The space optimization from a 2D array to a 1D array is possible because we only need the results from the previous row to compute the current row.
* This problem is a classic example of the Levenshtein distance algorithm, which has applications in spell checking, DNA sequence alignment, and plagiarism detection.
* The three operations (insert, delete, replace) correspond to specific transitions in the dynamic programming table, making it a versatile algorithm for string transformation problems.

---

## **Real-life Analogy**

* Imagine you're a text editor trying to find the minimum number of keystrokes to transform one document into another. The brute force approach would be to try all possible combinations of insert, delete, and replace operations, which is inefficient. The dynamic programming approach would be to build up the solution by finding the minimum number of operations for smaller prefixes of the documents first, which is much more efficient.

---

## **Summary**

* We solved the Edit Distance problem by first considering a brute force recursive approach that tries all possible combinations of operations, resulting in O(3^(m+n)) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(m * n). Finally, we explored a space optimization that reduces the space complexity from O(m * n) to O(min(m, n)). The key insight is to recognize that the minimum edit distance between prefixes of the two strings depends only on the minimum edit distance between smaller prefixes, which allows us to build the solution incrementally. 