# 📝 Longest Substring Without Repeating Characters

## **Problem Statement**

* Given a string `s`, find the length of the longest substring without repeating characters.
* Example:
  * Input: s = "abcabcbb"
  * Output: 3
  * Explanation: The answer is "abc", with the length of 3.
* Constraints:
  * 0 <= s.length <= 5 * 10^4
  * s consists of English letters, digits, symbols and spaces.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the longest substring without repeating characters
* A substring is a contiguous sequence of characters within a string
* The most straightforward approach is to check all possible substrings
* For each substring, we check if it contains any repeating characters
* If it doesn't, we update our maximum length if this substring is longer

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize `max_length = 0`
2. For each starting index i from 0 to n-1:
   1. For each ending index j from i to n-1:
      1. Check if the substring s[i:j+1] has no repeating characters
      2. If it has no repeating characters, update `max_length = max(max_length, j-i+1)`
3. Return `max_length`

---

## **Step 3: Brute Force Implementation (Code)**

```python
def lengthOfLongestSubstring(s):
    n = len(s)
    max_length = 0
    
    for i in range(n):
        for j in range(i, n):
            # Check if the substring s[i:j+1] has no repeating characters
            if len(set(s[i:j+1])) == j - i + 1:
                max_length = max(max_length, j - i + 1)
    
    return max_length
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n³) where n is the length of the string.
  * We have two nested loops, which is O(n²)
  * For each substring, we check if it has repeating characters, which takes O(n) time
* Space Complexity: O(min(n, m)) where m is the size of the character set. In the worst case, we might need to store all characters in the set.
* This approach is very inefficient for long strings and will likely result in a time limit exceeded error.

---

## **Step 5: Visualizing Redundant Computation**

* For string "abcabcbb":
  * We check all possible substrings (36 combinations for n=8)
  * For each substring, we create a set and check if its size equals the length of the substring
  * We're repeatedly checking many substrings that clearly won't be the longest
  * For example, if we've found a substring of length 3 without repeating characters, we don't need to check substrings of length 1 or 2

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Sliding Window algorithm with a hash set.
* The property that matches this problem is the ability to efficiently track characters in the current window.
* By using a sliding window approach, we can avoid checking all possible substrings.
* Alternatives like brute force would be much less efficient.
* Precondition: We need to efficiently check if a character is already in the current substring.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of checking all possible substrings, we can use a sliding window approach
* We maintain a window of characters that has no repeating characters
* We use a hash set to keep track of characters in the current window
* As we iterate through the string, we expand the window by adding characters
* If we encounter a character that's already in the window, we shrink the window from the left until the duplicate is removed
* We keep track of the maximum window size we've seen

### **Step 2: Flow Steps (Optimized)**

1. Initialize `max_length = 0`, `left = 0`, and an empty hash set
2. For each index right from 0 to n-1:
   1. While s[right] is in the hash set:
      1. Remove s[left] from the hash set
      2. Increment left
   2. Add s[right] to the hash set
   3. Update `max_length = max(max_length, right - left + 1)`
3. Return `max_length`

### **Step 3: Implementation (Optimized)**

```python
def lengthOfLongestSubstring(s):
    n = len(s)
    char_set = set()
    max_length = 0
    left = 0
    
    for right in range(n):
        # If the character is already in the set, shrink the window from the left
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1
        
        # Add the current character to the set
        char_set.add(s[right])
        
        # Update the maximum length
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the string.
  * Although we have a nested while loop, each character is added and removed from the set at most once
* Space Complexity: O(min(n, m)) where m is the size of the character set. In the worst case, we might need to store all characters in the set.
* This is much more efficient than the brute force approach, especially for long strings.

### **Step 5: Visualizing Computation (Optimized)**

* For string "abcabcbb":
  * Initialize char_set = {}, max_length = 0, left = 0
  * For right=0 (a): char_set = {a}, max_length = 1
  * For right=1 (b): char_set = {a,b}, max_length = 2
  * For right=2 (c): char_set = {a,b,c}, max_length = 3
  * For right=3 (a): 'a' is already in char_set, so remove s[left] (a) and increment left to 1
    * Now char_set = {b,c,a}, max_length = 3
  * For right=4 (b): 'b' is already in char_set, so remove s[left] (b) and increment left to 2
    * Now char_set = {c,a,b}, max_length = 3
  * For right=5 (c): 'c' is already in char_set, so remove s[left] (c) and increment left to 3
    * Now char_set = {a,b,c}, max_length = 3
  * For right=6 (b): 'b' is already in char_set, so remove s[left] (a) and increment left to 4
    * Now char_set = {b,c,b}, which still has a duplicate 'b', so remove s[left] (b) and increment left to 5
    * Now char_set = {c,b}, max_length = 3
  * For right=7 (b): 'b' is already in char_set, so remove s[left] (c) and increment left to 6
    * Now char_set = {b}, which still has a duplicate 'b', so remove s[left] (b) and increment left to 7
    * Now char_set = {b}, max_length = 3
  * Return max_length = 3
* We're efficiently finding the longest substring without repeating characters using a sliding window.

### **Further Optimization**

We can optimize the algorithm further by using a hash map to store the index of each character. This allows us to jump directly to the position after the duplicate character instead of removing characters one by one.

```python
def lengthOfLongestSubstring(s):
    n = len(s)
    char_map = {}  # Maps character to its last seen index
    max_length = 0
    left = 0
    
    for right in range(n):
        # If the character is already in the window, update left pointer
        if s[right] in char_map and char_map[s[right]] >= left:
            left = char_map[s[right]] + 1
        else:
            # Update the maximum length
            max_length = max(max_length, right - left + 1)
        
        # Update the last seen index of the character
        char_map[s[right]] = right
    
    return max_length
```

This optimized version has the same time and space complexity but performs fewer operations in practice.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n³) | O(min(n, m)) | Checks all possible substrings |
| Optimized (Algorithm: Sliding Window) | O(n) | O(min(n, m)) | Uses a sliding window with a hash set |
| Further Optimized | O(n) | O(min(n, m)) | Uses a sliding window with a hash map |

---

## **Final Takeaways**

* Using a sliding window approach allows us to efficiently find the longest substring without repeating characters.
* The key insight is that we can maintain a window of unique characters and adjust it as we encounter duplicates.
* This problem demonstrates how using appropriate data structures (hash set/map) can lead to significant efficiency improvements.

---

## **Real-life Analogy**

* Imagine you're reading a book and trying to find the longest sequence of pages where no word is repeated. The brute force approach would be like checking every possible range of pages, counting unique words for each range. The optimized approach is like keeping a mental note of words you've seen as you read - when you encounter a word you've already seen, you "forget" all words from the beginning up to the first occurrence of that word, then continue reading, always tracking the longest sequence of pages with unique words.

---

## **Summary**

* We solved the "Longest Substring Without Repeating Characters" problem by first considering a brute force approach that checks all possible substrings, resulting in O(n³) time complexity. We then optimized using a sliding window approach with a hash set, reducing the time complexity to O(n). We further optimized by using a hash map to store character indices, allowing us to jump directly to the position after a duplicate character. This demonstrates how using appropriate algorithms and data structures can lead to significant efficiency improvements. 