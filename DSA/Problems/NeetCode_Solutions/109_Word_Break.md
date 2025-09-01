# 📝 Word Break

## **Problem Statement**

* Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.
* Note that the same word in the dictionary may be reused multiple times in the segmentation.
* Example 1:
  * Input: s = "leetcode", wordDict = ["leet", "code"]
  * Output: true
  * Explanation: Return true because "leetcode" can be segmented as "leet code".
* Example 2:
  * Input: s = "applepenapple", wordDict = ["apple", "pen"]
  * Output: true
  * Explanation: Return true because "applepenapple" can be segmented as "apple pen apple". Note that you are allowed to reuse a dictionary word.
* Example 3:
  * Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
  * Output: false
* Constraints:
  * 1 <= s.length <= 300
  * 1 <= wordDict.length <= 1000
  * 1 <= wordDict[i].length <= 20
  * s and wordDict[i] consist of only lowercase English letters.
  * All the strings of wordDict are unique.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine if a string can be segmented into words from a dictionary
* One approach would be to try all possible segmentations of the string
* For each position in the string, we can check if the substring from the beginning to that position is a valid word
* If it is, we recursively check if the rest of the string can be segmented
* This is essentially a decision problem at each position: either the current substring is a word or it's not

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a recursive function that takes the current index in the string
2. Base case: If we've reached the end of the string, return true (empty string can be segmented)
3. For each possible ending index from the current index to the end of the string:
   1. Check if the substring from the current index to the ending index is a word in the dictionary
   2. If it is, recursively call the function with the ending index as the new starting index
   3. If any recursive call returns true, return true
4. If no segmentation is found, return false

---

## **Step 3: Brute Force Implementation (Code)**

```python
def wordBreak(s, wordDict):
    def can_segment(start):
        # Base case: if we've reached the end of the string
        if start == len(s):
            return True
        
        # Try all possible words starting from the current position
        for end in range(start + 1, len(s) + 1):
            if s[start:end] in wordDict and can_segment(end):
                return True
        
        return False
    
    return can_segment(0)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n) where n is the length of the string. In the worst case, we might need to check all possible substrings, which is exponential.
* Space Complexity: O(n) for the recursion stack.
* This approach is inefficient for long strings due to the exponential time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For s = "leetcode", wordDict = ["leet", "code"]:
  * can_segment(0):
    * Check s[0:1] = "l": Not in wordDict
    * Check s[0:2] = "le": Not in wordDict
    * Check s[0:3] = "lee": Not in wordDict
    * Check s[0:4] = "leet": In wordDict, call can_segment(4)
      * can_segment(4):
        * Check s[4:5] = "c": Not in wordDict
        * Check s[4:6] = "co": Not in wordDict
        * Check s[4:7] = "cod": Not in wordDict
        * Check s[4:8] = "code": In wordDict, call can_segment(8)
          * can_segment(8): Base case, return true
        * Return true
      * Return true
    * Return true
  * Return true
* There's redundant computation in the recursive calls, especially for overlapping subproblems

---

## **Why are we using this algorithm?**

* For optimization, we'll use dynamic programming to avoid redundant computations.
* The property that matches this problem is that the ability to segment a string depends only on the ability to segment its suffixes.
* By storing the results of already computed subproblems, we can avoid recalculating them.
* Alternatives like the brute force approach would be inefficient due to redundant recursive calls.
* Precondition: We need to be able to efficiently determine if a substring is a word in the dictionary.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Dynamic Programming)**

* We can use dynamic programming to avoid redundant computations
* Let dp[i] represent whether the substring s[0:i] can be segmented into words from the dictionary
* Base case: dp[0] = true (empty string can be segmented)
* For each position i from 1 to n:
  * For each position j from 0 to i-1:
    * If dp[j] is true (meaning s[0:j] can be segmented) and s[j:i] is a word in the dictionary, then dp[i] is true
* The answer will be dp[n]

### **Step 2: Flow Steps (Optimized - Dynamic Programming)**

1. Create a boolean array dp of size n+1, where dp[i] represents whether the substring s[0:i] can be segmented
2. Initialize dp[0] = true (base case: empty string can be segmented)
3. For each position i from 1 to n:
   1. For each position j from 0 to i-1:
      1. If dp[j] is true and s[j:i] is a word in the dictionary, set dp[i] = true and break
4. Return dp[n]

### **Step 3: Implementation (Optimized - Dynamic Programming)**

```python
def wordBreak(s, wordDict):
    n = len(s)
    # Convert wordDict to a set for O(1) lookups
    word_set = set(wordDict)
    
    # dp[i] represents whether s[0:i] can be segmented
    dp = [False] * (n + 1)
    dp[0] = True  # Empty string can be segmented
    
    for i in range(1, n + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break
    
    return dp[n]
```

### **Further Optimization - Early Pruning**

We can optimize further by checking if all characters in the string can be found in the dictionary words:

```python
def wordBreak(s, wordDict):
    # Early pruning: check if all characters in s are present in wordDict
    char_set = set(''.join(wordDict))
    if any(c not in char_set for c in s):
        return False
    
    n = len(s)
    word_set = set(wordDict)
    dp = [False] * (n + 1)
    dp[0] = True
    
    for i in range(1, n + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break
    
    return dp[n]
```

### **Step 4: Complexity Analysis (Optimized - Dynamic Programming)**

* Time Complexity: O(n^2 * m) where n is the length of the string and m is the average length of words in the dictionary. For each position i, we check up to i substrings, and each substring check takes O(m) time on average.
* Space Complexity: O(n) for the dp array.
* This approach is much more efficient than the brute force approach for long strings.

### **Step 5: Visualizing Computation (Optimized - Dynamic Programming)**

* For s = "leetcode", wordDict = ["leet", "code"]:
  * Initialize dp = [True, False, False, False, False, False, False, False, False]
  * i = 1:
    * j = 0: Check s[0:1] = "l": Not in wordDict, dp[1] = False
  * i = 2:
    * j = 0: Check s[0:2] = "le": Not in wordDict, dp[2] = False
    * j = 1: dp[1] is False, skip
  * i = 3:
    * j = 0: Check s[0:3] = "lee": Not in wordDict, dp[3] = False
    * j = 1, j = 2: dp[j] is False, skip
  * i = 4:
    * j = 0: Check s[0:4] = "leet": In wordDict, dp[4] = True
    * Break inner loop
  * i = 5:
    * j = 0: Check s[0:5] = "leetc": Not in wordDict
    * j = 1, j = 2, j = 3: dp[j] is False, skip
    * j = 4: dp[4] is True, check s[4:5] = "c": Not in wordDict, dp[5] = False
  * i = 6:
    * j = 0: Check s[0:6] = "leetco": Not in wordDict
    * j = 1, j = 2, j = 3: dp[j] is False, skip
    * j = 4: dp[4] is True, check s[4:6] = "co": Not in wordDict, dp[6] = False
    * j = 5: dp[5] is False, skip
  * i = 7:
    * j = 0: Check s[0:7] = "leetcod": Not in wordDict
    * j = 1, j = 2, j = 3: dp[j] is False, skip
    * j = 4: dp[4] is True, check s[4:7] = "cod": Not in wordDict, dp[7] = False
    * j = 5, j = 6: dp[j] is False, skip
  * i = 8:
    * j = 0: Check s[0:8] = "leetcode": Not in wordDict
    * j = 1, j = 2, j = 3: dp[j] is False, skip
    * j = 4: dp[4] is True, check s[4:8] = "code": In wordDict, dp[8] = True
    * Break inner loop
  * dp[8] = True, which is the answer
* The dynamic programming approach efficiently computes the result without redundant calculations

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(2^n) | O(n) | Inefficient for long strings |
| Dynamic Programming | O(n^2 * m) | O(n) | Efficient for long strings |
| DP with Early Pruning | O(n^2 * m) | O(n) | Additional optimization for certain cases |

---

## **Final Takeaways**

* The key to efficiently solving the Word Break problem is to recognize the overlapping subproblems and use dynamic programming to avoid redundant computations.
* The problem can be solved using a bottom-up approach by building the solution from smaller subproblems.
* Converting the wordDict to a set improves lookup time from O(k) to O(1) on average, where k is the number of words in the dictionary.
* Early pruning techniques can significantly improve performance for certain inputs.
* This problem demonstrates how dynamic programming can transform an exponential-time algorithm into a polynomial-time one.

---

## **Real-life Analogy**

* Imagine you're trying to parse a sentence without spaces (like "thisisasentence") into valid words. The brute force approach would be to try all possible ways to insert spaces, which is inefficient. The dynamic programming approach would be to start from the beginning and mark positions where valid words end, then use those positions to find the next valid words, which is much more efficient.

---

## **Summary**

* We solved the Word Break problem by first considering a brute force recursive approach that tries all possible segmentations, resulting in O(2^n) time complexity. We then optimized using dynamic programming to avoid redundant computations, reducing the time complexity to O(n^2 * m). The key insight is to recognize that the ability to segment a string depends only on the ability to segment its prefixes, which allows us to build the solution incrementally from smaller substrings to the entire string. Additionally, we implemented early pruning to further optimize the solution for certain inputs. 