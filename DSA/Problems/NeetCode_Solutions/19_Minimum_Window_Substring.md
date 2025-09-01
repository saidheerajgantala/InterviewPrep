# 📝 Minimum Window Substring

## **Problem Statement**

* Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string `""`.
* The testcases will be generated such that the answer is unique.
* Example:
  * Input: s = "ADOBECODEBANC", t = "ABC"
  * Output: "BANC"
  * Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
* Constraints:
  * m == s.length
  * n == t.length
  * 1 <= m, n <= 10^5
  * s and t consist of uppercase and lowercase English letters.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the minimum window substring of s that contains all characters of t
* The most straightforward approach would be to check all possible substrings of s
* For each substring, we check if it contains all characters of t (including duplicates)
* If it does, we update our minimum window if this substring is shorter
* This approach is very inefficient but gives us a starting point

---

## **Step 2: Flow Steps (Brute Force)**

1. Count the frequency of each character in t
2. Initialize `min_window = ""` and `min_length = infinity`
3. For each starting index i from 0 to m-1:
   1. For each ending index j from i to m-1:
      1. Count the frequency of each character in the substring s[i:j+1]
      2. Check if this substring contains all characters of t (including duplicates)
      3. If it does and its length is less than `min_length`, update `min_window` and `min_length`
4. Return `min_window`

---

## **Step 3: Brute Force Implementation (Code)**

```python
def minWindow(s, t):
    m, n = len(s), len(t)
    
    if n > m:
        return ""
    
    # Count the frequency of each character in t
    t_count = {}
    for char in t:
        t_count[char] = t_count.get(char, 0) + 1
    
    min_window = ""
    min_length = float('inf')
    
    # Check all possible substrings
    for i in range(m):
        for j in range(i, m):
            # Count the frequency of each character in the substring
            window_count = {}
            for k in range(i, j + 1):
                window_count[s[k]] = window_count.get(s[k], 0) + 1
            
            # Check if the substring contains all characters of t
            valid = True
            for char, count in t_count.items():
                if window_count.get(char, 0) < count:
                    valid = False
                    break
            
            # Update minimum window if valid
            if valid and (j - i + 1) < min_length:
                min_window = s[i:j+1]
                min_length = j - i + 1
    
    return min_window
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(m^3), where m is the length of string s.
  * O(m^2) for generating all possible substrings
  * O(m) for checking if each substring contains all characters of t
* Space Complexity: O(m + n) for storing the character frequency maps.
* This approach is extremely inefficient for long strings and will likely result in a time limit exceeded error.

---

## **Step 5: Visualizing Redundant Computation**

* For s = "ADOBECODEBANC" and t = "ABC":
  * We check all possible substrings (91 combinations for m=13)
  * For each substring, we count the frequency of each character
  * We're repeatedly counting characters in overlapping substrings
  * We're also checking many substrings that clearly won't be the minimum window
  * For example, once we find a valid window of length 4, we don't need to check longer windows

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Sliding Window algorithm with character frequency tracking.
* The property that matches this problem is the ability to efficiently track character frequencies in a variable-size window.
* By using a sliding window approach, we can avoid checking all possible substrings.
* Alternatives like brute force would be much less efficient.
* Precondition: We need to efficiently track whether the current window contains all characters of t.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of checking all possible substrings, we can use a sliding window approach
* We maintain a window and track the frequency of each character in the window
* We expand the window by moving the right pointer until we have a valid window (contains all characters of t)
* Once we have a valid window, we contract it from the left to minimize its size while keeping it valid
* We repeat this process until we've processed the entire string
* This way, we only need to scan the string once

### **Step 2: Flow Steps (Optimized)**

1. Count the frequency of each character in t
2. Initialize `left = 0`, `right = 0`, `min_window = ""`, `min_length = infinity`
3. Initialize a counter `required` to the number of unique characters in t
4. Initialize a counter `formed` to 0 (number of characters that have met their required frequency)
5. While `right < m`:
   1. Add s[right] to the window and update its frequency
   2. If s[right] is in t and its frequency in the window equals its frequency in t, increment `formed`
   3. While `formed == required` (we have a valid window):
      1. If the current window is smaller than `min_length`, update `min_window` and `min_length`
      2. Remove s[left] from the window and update its frequency
      3. If s[left] is in t and its frequency in the window becomes less than its frequency in t, decrement `formed`
      4. Increment `left` to contract the window
   4. Increment `right` to expand the window
6. Return `min_window`

### **Step 3: Implementation (Optimized)**

```python
def minWindow(s, t):
    m, n = len(s), len(t)
    
    if n > m or n == 0:
        return ""
    
    # Count the frequency of each character in t
    t_count = {}
    for char in t:
        t_count[char] = t_count.get(char, 0) + 1
    
    # Number of unique characters in t
    required = len(t_count)
    
    # Initialize window variables
    window_count = {}
    formed = 0
    
    # Initialize result variables
    min_window = ""
    min_length = float('inf')
    
    # Initialize pointers
    left = right = 0
    
    # Slide the window
    while right < m:
        # Add the current character to the window
        char = s[right]
        window_count[char] = window_count.get(char, 0) + 1
        
        # Check if we've met the required frequency for this character
        if char in t_count and window_count[char] == t_count[char]:
            formed += 1
        
        # Try to contract the window from the left
        while left <= right and formed == required:
            char = s[left]
            
            # Update the minimum window if smaller
            if right - left + 1 < min_length:
                min_window = s[left:right+1]
                min_length = right - left + 1
            
            # Remove the leftmost character from the window
            window_count[char] -= 1
            
            # Check if we've broken the required frequency for this character
            if char in t_count and window_count[char] < t_count[char]:
                formed -= 1
            
            # Contract the window
            left += 1
        
        # Expand the window
        right += 1
    
    return min_window
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(m + n), where m is the length of string s and n is the length of string t.
  * O(n) to count the frequency of characters in t
  * O(m) to scan through string s with the sliding window
* Space Complexity: O(m + n) for storing the character frequency maps.
* This is much more efficient than the brute force approach, especially for long strings.

### **Step 5: Visualizing Computation (Optimized)**

* For s = "ADOBECODEBANC" and t = "ABC":
  * t_count = {'A': 1, 'B': 1, 'C': 1}, required = 3
  * Initialize window_count = {}, formed = 0, min_window = "", min_length = infinity
  * right = 0, s[0] = 'A': window_count = {'A': 1}, formed = 1 (not valid yet)
  * right = 1, s[1] = 'D': window_count = {'A': 1, 'D': 1}, formed = 1 (not valid yet)
  * right = 2, s[2] = 'O': window_count = {'A': 1, 'D': 1, 'O': 1}, formed = 1 (not valid yet)
  * right = 3, s[3] = 'B': window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 1}, formed = 2 (not valid yet)
  * right = 4, s[4] = 'E': window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 1, 'E': 1}, formed = 2 (not valid yet)
  * right = 5, s[5] = 'C': window_count = {'A': 1, 'D': 1, 'O': 1, 'B': 1, 'E': 1, 'C': 1}, formed = 3 (valid)
    * Update min_window = "ADOBEC", min_length = 6
    * Contract window: left = 0, s[0] = 'A': window_count = {'A': 0, 'D': 1, 'O': 1, 'B': 1, 'E': 1, 'C': 1}, formed = 2 (not valid)
  * right = 6, s[6] = 'O': window_count = {'A': 0, 'D': 1, 'O': 2, 'B': 1, 'E': 1, 'C': 1}, formed = 2 (not valid yet)
  * ...and so on
  * Eventually, we find the minimum window "BANC" with length 4
* We're efficiently finding the minimum window substring using a sliding window approach.

### **Further Optimization**

We can optimize the solution further by only tracking characters that are in t, which can reduce the space complexity and the number of operations:

```python
def minWindow(s, t):
    m, n = len(s), len(t)
    
    if n > m or n == 0:
        return ""
    
    # Count the frequency of each character in t
    t_count = {}
    for char in t:
        t_count[char] = t_count.get(char, 0) + 1
    
    # Initialize window variables
    window_count = {}
    required = len(t_count)
    formed = 0
    
    # Initialize result variables
    min_window = ""
    min_length = float('inf')
    
    # Initialize pointers
    left = right = 0
    
    # Slide the window
    while right < m:
        # Add the current character to the window if it's in t
        char = s[right]
        if char in t_count:
            window_count[char] = window_count.get(char, 0) + 1
            if window_count[char] == t_count[char]:
                formed += 1
        
        # Try to contract the window from the left
        while left <= right and formed == required:
            char = s[left]
            
            # Update the minimum window if smaller
            if right - left + 1 < min_length:
                min_window = s[left:right+1]
                min_length = right - left + 1
            
            # Remove the leftmost character from the window if it's in t
            if char in t_count:
                window_count[char] -= 1
                if window_count[char] < t_count[char]:
                    formed -= 1
            
            # Contract the window
            left += 1
        
        # Expand the window
        right += 1
    
    return min_window
```

This optimization reduces the number of operations and the space used by only tracking characters that are in t.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(m³) | O(m + n) | Checks all possible substrings |
| Optimized (Algorithm: Sliding Window) | O(m + n) | O(m + n) | Uses a sliding window with character frequency tracking |
| Further Optimized | O(m + n) | O(k) | Only tracks characters in t, where k is the size of the character set |

---

## **Final Takeaways**

* Using a sliding window approach allows us to efficiently find the minimum window substring.
* The key insight is to expand the window until it's valid, then contract it to minimize its size while keeping it valid.
* This problem demonstrates how understanding the structure of the problem can lead to efficient algorithms without checking all possibilities.

---

## **Real-life Analogy**

* Imagine you're a tour guide planning a route through a city that must include specific landmarks (characters in t). The brute force approach would be like checking every possible route segment and seeing if it covers all landmarks. The optimized approach is like starting with a small route, extending it until it covers all landmarks, then trimming unnecessary detours from the beginning while ensuring all landmarks are still included. You continue this process of extending and trimming until you've explored the entire city, keeping track of the shortest valid route you've found.

---

## **Summary**

* We solved the "Minimum Window Substring" problem by first considering a brute force approach that checks all possible substrings, resulting in O(m³) time complexity. We then optimized using a sliding window approach with character frequency tracking, reducing the time complexity to O(m + n) while maintaining O(m + n) space complexity. We further optimized by only tracking characters that are in t. The key insight is to expand the window until it's valid, then contract it to minimize its size while keeping it valid, repeating this process until we've processed the entire string. 