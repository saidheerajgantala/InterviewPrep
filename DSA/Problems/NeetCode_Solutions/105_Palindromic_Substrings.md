# 📝 Palindromic Substrings

## **Problem Statement**

* Given a string `s`, return the number of palindromic substrings in it.
* A string is a palindrome when it reads the same backward as forward.
* A substring is a contiguous sequence of characters within the string.
* Example 1:
  * Input: s = "abc"
  * Output: 3
  * Explanation: Three palindromic strings: "a", "b", "c"
* Example 2:
  * Input: s = "aaa"
  * Output: 6
  * Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa"
* Constraints:
  * 1 <= s.length <= 1000
  * s consists of lowercase English letters.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to count all palindromic substrings in the given string
* A straightforward approach would be to generate all possible substrings and check if each one is a palindrome
* This approach is similar to the "Longest Palindromic Substring" problem but instead of finding the longest one, we count all of them
* For each substring, we'll check if it's a palindrome by comparing it with its reverse

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a counter for palindromic substrings
2. Generate all possible substrings:
   1. For each starting index i from 0 to n-1:
      1. For each ending index j from i to n-1:
         1. Extract the substring from index i to j
         2. Check if the substring is a palindrome
         3. If it is a palindrome, increment the counter
3. Return the counter

---

## **Step 3: Brute Force Implementation (Code)**

```python
def countSubstrings(s):
    n = len(s)
    count = 0
    
    for i in range(n):
        for j in range(i, n):
            substring = s[i:j+1]
            if substring == substring[::-1]:
                count += 1
    
    return count
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n^3) where n is the length of the string. We generate O(n^2) substrings, and for each substring, we check if it's a palindrome in O(n) time.
* Space Complexity: O(n) for storing the substring during the palindrome check.
* This approach is inefficient for long strings due to the cubic time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For s = "aaa":
  * Substrings starting at index 0:
    * "a": Is a palindrome, count = 1
    * "aa": Is a palindrome, count = 2
    * "aaa": Is a palindrome, count = 3
  * Substrings starting at index 1:
    * "a": Is a palindrome, count = 4
    * "aa": Is a palindrome, count = 5
  * Substrings starting at index 2:
    * "a": Is a palindrome, count = 6
* Total count = 6
* There's redundant computation in checking if a substring is a palindrome, especially for overlapping substrings

---

## **Why are we using this algorithm?**

* For optimization, we'll use the expand around center approach, similar to the "Longest Palindromic Substring" problem.
* The property that matches this problem is that a palindrome is symmetric around its center.
* By expanding around each possible center, we can find all palindromic substrings more efficiently.
* Alternatives like the brute force approach would be inefficient due to redundant palindrome checks.
* Precondition: We need to be able to efficiently check if a substring is a palindrome.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Expand Around Center)**

* Instead of generating all substrings and checking each one, we can expand around each possible center
* A palindrome can have either an odd or even length:
  * For odd-length palindromes, the center is a single character
  * For even-length palindromes, the center is between two characters
* For a string of length n, there are 2n-1 possible centers:
  * n centers for odd-length palindromes (each character)
  * n-1 centers for even-length palindromes (between each pair of adjacent characters)
* For each center, we expand outward as long as the characters on both sides match, and count each valid palindrome
* This way, we can find all palindromic substrings in O(n^2) time

### **Step 2: Flow Steps (Optimized - Expand Around Center)**

1. Initialize a counter for palindromic substrings
2. For each possible center (2n-1 centers in total):
   1. Expand around the center as long as the characters on both sides match
   2. For each valid palindrome found during expansion, increment the counter
3. Return the counter

### **Step 3: Implementation (Optimized - Expand Around Center)**

```python
def countSubstrings(s):
    n = len(s)
    count = 0
    
    def expand_around_center(left, right):
        nonlocal count
        while left >= 0 and right < n and s[left] == s[right]:
            count += 1
            left -= 1
            right += 1
    
    for i in range(n):
        # Expand around center for odd-length palindromes
        expand_around_center(i, i)
        # Expand around center for even-length palindromes
        expand_around_center(i, i + 1)
    
    return count
```

### **Alternative Approach: Dynamic Programming**

Another efficient approach is to use dynamic programming:

```python
def countSubstrings(s):
    n = len(s)
    dp = [[False] * n for _ in range(n)]
    count = 0
    
    # All substrings of length 1 are palindromes
    for i in range(n):
        dp[i][i] = True
        count += 1
    
    # Check for substrings of length 2
    for i in range(n - 1):
        if s[i] == s[i + 1]:
            dp[i][i + 1] = True
            count += 1
    
    # Check for substrings of length 3 or more
    for length in range(3, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j] and dp[i + 1][j - 1]:
                dp[i][j] = True
                count += 1
    
    return count
```

### **Step 4: Complexity Analysis (Optimized - Expand Around Center)**

* Time Complexity: O(n^2) where n is the length of the string. We have 2n-1 centers, and for each center, we expand outward, which takes O(n) time in the worst case.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is much more efficient than the brute force approach for long strings.

### **Complexity Analysis (Dynamic Programming)**

* Time Complexity: O(n^2) where n is the length of the string. We fill a 2D table of size n x n.
* Space Complexity: O(n^2) for the dp table.
* This approach is also efficient for long strings, but uses more space than the expand around center approach.

### **Step 5: Visualizing Computation (Optimized - Expand Around Center)**

* For s = "aaa":
  * Center at index 0 (character 'a'):
    * Expand to "a", count = 1
  * Center between indices 0 and 1:
    * Characters 'a' and 'a' match, expand to "aa", count = 2
  * Center at index 1 (character 'a'):
    * Expand to "a", count = 3
    * Expand to "aaa", count = 4
  * Center between indices 1 and 2:
    * Characters 'a' and 'a' match, expand to "aa", count = 5
  * Center at index 2 (character 'a'):
    * Expand to "a", count = 6
* Total count = 6
* The expand around center approach efficiently finds all palindromic substrings without redundant checks

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n^3) | O(n) | Inefficient for long strings |
| Expand Around Center | O(n^2) | O(1) | Efficient with minimal space usage |
| Dynamic Programming | O(n^2) | O(n^2) | Efficient but uses more space |

---

## **Final Takeaways**

* The key to efficiently solving the Palindromic Substrings problem is to avoid redundant palindrome checks.
* The expand around center approach is particularly elegant and efficient, as it leverages the symmetric nature of palindromes.
* Dynamic programming is also a viable approach, especially when we need to store and reuse palindrome information for other purposes.
* This problem is closely related to the "Longest Palindromic Substring" problem, and the same techniques can be applied.
* Understanding the property that a palindrome is symmetric around its center is crucial for developing efficient algorithms for palindrome-related problems.

---

## **Real-life Analogy**

* Imagine you're looking for all words in a book that read the same forward and backward. The brute force approach would be to check every possible word, which is time-consuming. The expand around center approach would be like starting from each letter (or between letters) and expanding outward as long as the letters on both sides match, which is much more efficient.

---

## **Summary**

* We solved the Palindromic Substrings problem by first considering a brute force approach that generates all substrings and checks each one, resulting in O(n^3) time complexity. We then optimized using the expand around center approach, which leverages the symmetric nature of palindromes to reduce the time complexity to O(n^2) with O(1) space complexity. We also explored a dynamic programming approach, which has the same time complexity but uses O(n^2) space. The key insight is to recognize that a palindrome is symmetric around its center, which allows us to efficiently find all palindromic substrings by expanding around each possible center. 