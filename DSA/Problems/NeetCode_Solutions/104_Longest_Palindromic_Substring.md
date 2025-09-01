# 📝 Longest Palindromic Substring

## **Problem Statement**

* Given a string `s`, return the longest palindromic substring in `s`.
* A string is called a palindrome when it reads the same backward as forward.
* Example 1:
  * Input: s = "babad"
  * Output: "bab"
  * Explanation: "aba" is also a valid answer.
* Example 2:
  * Input: s = "cbbd"
  * Output: "bb"
* Constraints:
  * 1 <= s.length <= 1000
  * s consists of only digits and English letters.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the longest substring that is a palindrome
* One approach would be to generate all possible substrings and check if each one is a palindrome
* Then return the longest palindromic substring found
* This approach is straightforward but might be inefficient for long strings

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize variables to keep track of the longest palindromic substring found so far
2. Generate all possible substrings:
   1. For each starting index i from 0 to n-1:
      1. For each ending index j from i to n-1:
         1. Extract the substring from index i to j
         2. Check if the substring is a palindrome
         3. If it is a palindrome and longer than the longest found so far, update the longest palindromic substring
3. Return the longest palindromic substring found

---

## **Step 3: Brute Force Implementation (Code)**

```python
def longestPalindrome(s):
    n = len(s)
    longest = ""
    max_length = 0
    
    for i in range(n):
        for j in range(i, n):
            substring = s[i:j+1]
            if substring == substring[::-1] and len(substring) > max_length:
                longest = substring
                max_length = len(substring)
    
    return longest
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n^3) where n is the length of the string. We generate O(n^2) substrings, and for each substring, we check if it's a palindrome in O(n) time.
* Space Complexity: O(n) for storing the longest palindromic substring.
* This approach is inefficient for long strings due to the cubic time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For s = "babad":
  * Substrings starting at index 0:
    * "b": Is a palindrome, length = 1
    * "ba": Not a palindrome
    * "bab": Is a palindrome, length = 3
    * "baba": Not a palindrome
    * "babad": Not a palindrome
  * Substrings starting at index 1:
    * "a": Is a palindrome, length = 1
    * "ab": Not a palindrome
    * "aba": Is a palindrome, length = 3
    * "abad": Not a palindrome
  * Substrings starting at index 2:
    * "b": Is a palindrome, length = 1
    * "ba": Not a palindrome
    * "bad": Not a palindrome
  * Substrings starting at index 3:
    * "a": Is a palindrome, length = 1
    * "ad": Not a palindrome
  * Substrings starting at index 4:
    * "d": Is a palindrome, length = 1
* The longest palindromic substrings are "bab" and "aba", both of length 3
* There's redundant computation in checking if a substring is a palindrome, especially for overlapping substrings

---

## **Why are we using this algorithm?**

* For optimization, we'll use the expand around center approach or dynamic programming.
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
* For each center, we expand outward as long as the characters on both sides match
* This way, we can find all palindromic substrings in O(n^2) time

### **Step 2: Flow Steps (Optimized - Expand Around Center)**

1. Initialize variables to keep track of the longest palindromic substring found so far
2. For each possible center (2n-1 centers in total):
   1. Expand around the center as long as the characters on both sides match
   2. Keep track of the longest palindrome found during the expansion
3. Return the longest palindromic substring found

### **Step 3: Implementation (Optimized - Expand Around Center)**

```python
def longestPalindrome(s):
    if not s:
        return ""
    
    start = 0
    max_length = 1
    
    def expand_around_center(left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return left + 1, right - 1
    
    for i in range(len(s)):
        # Expand around center for odd-length palindromes
        left1, right1 = expand_around_center(i, i)
        # Expand around center for even-length palindromes
        left2, right2 = expand_around_center(i, i + 1)
        
        # Update longest palindrome if needed
        if right1 - left1 + 1 > max_length:
            start = left1
            max_length = right1 - left1 + 1
        
        if right2 - left2 + 1 > max_length:
            start = left2
            max_length = right2 - left2 + 1
    
    return s[start:start + max_length]
```

### **Alternative Approach: Dynamic Programming**

Another efficient approach is to use dynamic programming:

```python
def longestPalindrome(s):
    n = len(s)
    if n <= 1:
        return s
    
    # Initialize a table to store whether s[i:j+1] is a palindrome
    dp = [[False] * n for _ in range(n)]
    
    # All substrings of length 1 are palindromes
    for i in range(n):
        dp[i][i] = True
    
    start = 0
    max_length = 1
    
    # Check for substrings of length 2
    for i in range(n - 1):
        if s[i] == s[i + 1]:
            dp[i][i + 1] = True
            start = i
            max_length = 2
    
    # Check for substrings of length 3 or more
    for length in range(3, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j] and dp[i + 1][j - 1]:
                dp[i][j] = True
                if length > max_length:
                    start = i
                    max_length = length
    
    return s[start:start + max_length]
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

* For s = "babad":
  * Center at index 0 (character 'b'):
    * Expand to "b", length = 1
  * Center between indices 0 and 1:
    * Characters 'b' and 'a' don't match, so no palindrome
  * Center at index 1 (character 'a'):
    * Expand to "a", length = 1
    * Expand to "bab", length = 3
  * Center between indices 1 and 2:
    * Characters 'a' and 'b' don't match, so no palindrome
  * Center at index 2 (character 'b'):
    * Expand to "b", length = 1
    * Expand to "aba", length = 3
  * Center between indices 2 and 3:
    * Characters 'b' and 'a' don't match, so no palindrome
  * Center at index 3 (character 'a'):
    * Expand to "a", length = 1
  * Center between indices 3 and 4:
    * Characters 'a' and 'd' don't match, so no palindrome
  * Center at index 4 (character 'd'):
    * Expand to "d", length = 1
* The longest palindromic substrings are "bab" and "aba", both of length 3
* The expand around center approach efficiently finds all palindromic substrings

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n^3) | O(n) | Inefficient for long strings |
| Expand Around Center | O(n^2) | O(1) | Efficient with minimal space usage |
| Dynamic Programming | O(n^2) | O(n^2) | Efficient but uses more space |

---

## **Final Takeaways**

* The key to efficiently solving the Longest Palindromic Substring problem is to avoid redundant palindrome checks.
* The expand around center approach is particularly elegant and efficient, as it leverages the symmetric nature of palindromes.
* Dynamic programming is also a viable approach, especially when we need to store and reuse palindrome information for other purposes.
* This problem demonstrates the importance of recognizing patterns and properties in the problem domain to develop efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're looking for the longest word in a book that reads the same forward and backward. The brute force approach would be to check every possible word, which is time-consuming. The expand around center approach would be like starting from each letter (or between letters) and expanding outward as long as the letters on both sides match, which is much more efficient.

---

## **Summary**

* We solved the Longest Palindromic Substring problem by first considering a brute force approach that generates all substrings and checks each one, resulting in O(n^3) time complexity. We then optimized using the expand around center approach, which leverages the symmetric nature of palindromes to reduce the time complexity to O(n^2) with O(1) space complexity. We also explored a dynamic programming approach, which has the same time complexity but uses O(n^2) space. The key insight is to recognize that a palindrome is symmetric around its center, which allows us to efficiently find all palindromic substrings by expanding around each possible center. 