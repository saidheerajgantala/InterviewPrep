# 📝 Palindrome Partitioning

## **Problem Statement**

* Given a string `s`, partition `s` such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of `s`.
* A palindrome string is a string that reads the same backward as forward.
* Example 1:
  * Input: s = "aab"
  * Output: [["a","a","b"],["aa","b"]]
* Example 2:
  * Input: s = "a"
  * Output: [["a"]]
* Constraints:
  * 1 <= s.length <= 16
  * `s` contains only lowercase English letters.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find all possible ways to partition the string such that each substring is a palindrome
* One approach would be to generate all possible partitions of the string and check if each substring is a palindrome
* This is a classic backtracking problem where we try different partitioning points

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty result list to store all valid partitions
2. Define a backtracking function that takes the current partition and the remaining string:
   1. If there's no remaining string, add the current partition to the result list
   2. For each possible prefix of the remaining string:
      1. If the prefix is a palindrome, add it to the current partition
      2. Recursively call the backtracking function with the updated partition and the remaining string
      3. Remove the prefix from the current partition (backtrack)
3. Call the backtracking function with an empty partition and the entire string
4. Return the result list

---

## **Step 3: Brute Force Implementation (Code)**

```python
def partition(s):
    result = []
    
    def is_palindrome(string):
        return string == string[::-1]
    
    def backtrack(start, path):
        if start == len(s):
            result.append(path.copy())
            return
        
        for end in range(start + 1, len(s) + 1):
            prefix = s[start:end]
            if is_palindrome(prefix):
                path.append(prefix)
                backtrack(end, path)
                path.pop()
    
    backtrack(0, [])
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n * 2^n) where n is the length of the string. In the worst case, we have 2^(n-1) possible partitions, and for each partition, we need O(n) time to check if each substring is a palindrome.
* Space Complexity: O(n) for the recursion stack and to store each partition.
* This approach is inefficient for large strings due to the exponential growth of the number of partitions.

---

## **Step 5: Visualizing Redundant Computation**

* For the example s = "aab":
  * Start with an empty partition: []
  * Try "a": ["a"]
    * Try "a": ["a", "a"]
      * Try "b": ["a", "a", "b"]
        * No remaining string, add to result: ["a", "a", "b"]
    * Try "ab": Not a palindrome, skip
  * Try "aa": ["aa"]
    * Try "b": ["aa", "b"]
      * No remaining string, add to result: ["aa", "b"]
  * Try "aab": Not a palindrome, skip
* Result: [["a", "a", "b"], ["aa", "b"]]
* There's redundant computation in checking if a substring is a palindrome, as we might check the same substring multiple times

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to precompute whether each substring is a palindrome.
* The property that matches this problem is that we can determine if a substring is a palindrome based on its inner substring.
* By precomputing palindrome information, we can avoid redundant checks during the backtracking process.
* Alternatives like the brute force approach would be less efficient due to redundant palindrome checks.
* Precondition: We need to be able to precompute palindrome information efficiently.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - DP + Backtracking)**

* We can use dynamic programming to precompute whether each substring is a palindrome
* Let dp[i][j] be true if the substring s[i...j] is a palindrome, false otherwise
* Then, dp[i][j] = true if s[i] == s[j] and (j - i <= 2 or dp[i+1][j-1] is true)
* This way, we can avoid redundant palindrome checks during the backtracking process

### **Step 2: Flow Steps (Optimized - DP + Backtracking)**

1. Precompute palindrome information using dynamic programming
2. Initialize an empty result list to store all valid partitions
3. Define a backtracking function that takes the current partition and the current index:
   1. If the current index is equal to the length of the string, add the current partition to the result list
   2. For each possible ending index from the current index to the end of the string:
      1. If the substring from the current index to the ending index is a palindrome (using the precomputed information), add it to the current partition
      2. Recursively call the backtracking function with the updated partition and the next index
      3. Remove the substring from the current partition (backtrack)
4. Call the backtracking function with an empty partition and index 0
5. Return the result list

### **Step 3: Implementation (Optimized - DP + Backtracking)**

```python
def partition(s):
    n = len(s)
    result = []
    
    # Precompute palindrome information
    dp = [[False] * n for _ in range(n)]
    for i in range(n):
        dp[i][i] = True  # Single characters are palindromes
    for i in range(n - 1):
        dp[i][i + 1] = (s[i] == s[i + 1])  # Check adjacent characters
    for length in range(3, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            dp[i][j] = (s[i] == s[j]) and dp[i + 1][j - 1]
    
    def backtrack(start, path):
        if start == n:
            result.append(path.copy())
            return
        
        for end in range(start, n):
            if dp[start][end]:
                path.append(s[start:end + 1])
                backtrack(end + 1, path)
                path.pop()
    
    backtrack(0, [])
    return result
```

### **Alternative Approach: Optimized DP Calculation**

We can optimize the DP calculation by using a simpler approach:

```python
def partition(s):
    n = len(s)
    result = []
    
    # Precompute palindrome information
    dp = [[False] * n for _ in range(n)]
    
    def backtrack(start, path):
        if start == n:
            result.append(path.copy())
            return
        
        for end in range(start, n):
            if s[start] == s[end] and (end - start <= 2 or dp[start + 1][end - 1]):
                dp[start][end] = True
                path.append(s[start:end + 1])
                backtrack(end + 1, path)
                path.pop()
    
    backtrack(0, [])
    return result
```

### **Step 4: Complexity Analysis (Optimized - DP + Backtracking)**

* Time Complexity: O(n * 2^n) where n is the length of the string. The precomputation of palindrome information takes O(n^2) time, and the backtracking process still has O(n * 2^n) time complexity in the worst case. However, the constant factor is much smaller due to the efficient palindrome checks.
* Space Complexity: O(n^2) for the DP table, plus O(n) for the recursion stack and to store each partition.
* This approach is more efficient than the brute force approach due to the precomputation of palindrome information.

### **Step 5: Visualizing Computation (Optimized - DP + Backtracking)**

* For the example s = "aab":
  * Precompute palindrome information:
    * dp[0][0] = true ("a" is a palindrome)
    * dp[1][1] = true ("a" is a palindrome)
    * dp[2][2] = true ("b" is a palindrome)
    * dp[0][1] = true ("aa" is a palindrome)
    * dp[1][2] = false ("ab" is not a palindrome)
    * dp[0][2] = false ("aab" is not a palindrome)
  * Start with an empty partition: []
  * Try "a" (dp[0][0] is true): ["a"]
    * Try "a" (dp[1][1] is true): ["a", "a"]
      * Try "b" (dp[2][2] is true): ["a", "a", "b"]
        * No remaining string, add to result: ["a", "a", "b"]
    * Try "ab" (dp[1][2] is false): Skip
  * Try "aa" (dp[0][1] is true): ["aa"]
    * Try "b" (dp[2][2] is true): ["aa", "b"]
      * No remaining string, add to result: ["aa", "b"]
  * Try "aab" (dp[0][2] is false): Skip
* Result: [["a", "a", "b"], ["aa", "b"]]
* The DP approach efficiently avoids redundant palindrome checks

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n * 2^n) | O(n) | Inefficient due to redundant palindrome checks |
| Optimized (DP + Backtracking) | O(n * 2^n) | O(n^2) | More efficient with precomputed palindrome information |

---

## **Final Takeaways**

* The key to efficiently solving the Palindrome Partitioning problem is to use dynamic programming to precompute palindrome information.
* By precomputing whether each substring is a palindrome, we can avoid redundant checks during the backtracking process.
* This problem demonstrates the power of combining dynamic programming with backtracking for problems that involve partitioning and checking properties of substrings.

---

## **Real-life Analogy**

* Imagine you're trying to divide a long text into paragraphs, where each paragraph must be a palindrome. The brute force approach would be to try all possible ways to divide the text and check if each paragraph is a palindrome. The optimized approach would be to first mark all possible palindromes in the text, and then try different ways to divide the text using only the marked palindromes. This is similar to how we precompute palindrome information and then use backtracking to find all valid partitions.

---

## **Summary**

* We solved the Palindrome Partitioning problem by first considering a brute force approach that generates all possible partitions and checks if each substring is a palindrome, resulting in O(n * 2^n) time complexity. We then optimized using dynamic programming to precompute palindrome information, which has the same asymptotic time complexity but is more efficient in practice due to avoiding redundant palindrome checks. The key insight is to use dynamic programming to determine if a substring is a palindrome based on its inner substring, and then use backtracking to find all valid partitions. 