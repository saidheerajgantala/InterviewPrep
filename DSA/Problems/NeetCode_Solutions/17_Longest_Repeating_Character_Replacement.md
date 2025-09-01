# 📝 Longest Repeating Character Replacement

## **Problem Statement**

* You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.
* Return the length of the longest substring containing the same letter you can get after performing the above operations.
* Example:
  * Input: s = "ABAB", k = 2
  * Output: 4
  * Explanation: Replace the two 'A's with two 'B's or vice versa.
* Constraints:
  * 1 <= s.length <= 10^5
  * s consists of only uppercase English letters.
  * 0 <= k <= s.length

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the longest substring where, after changing at most k characters, all characters are the same
* The most straightforward approach would be to check all possible substrings
* For each substring, we count the occurrences of each character
* The character with the maximum count is the one we want to keep
* The number of characters we need to change is the length of the substring minus the maximum count
* If this number is less than or equal to k, the substring is valid

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize `max_length = 0`
2. For each starting index i from 0 to n-1:
   1. For each ending index j from i to n-1:
      1. Count the occurrences of each character in the substring s[i:j+1]
      2. Find the character with the maximum count
      3. Calculate the number of characters to change: (j-i+1) - max_count
      4. If the number of characters to change is less than or equal to k, update `max_length = max(max_length, j-i+1)`
3. Return `max_length`

---

## **Step 3: Brute Force Implementation (Code)**

```python
def characterReplacement(s, k):
    n = len(s)
    max_length = 0
    
    for i in range(n):
        for j in range(i, n):
            # Count occurrences of each character in the substring
            char_count = {}
            for idx in range(i, j + 1):
                if s[idx] in char_count:
                    char_count[s[idx]] += 1
                else:
                    char_count[s[idx]] = 1
            
            # Find the character with the maximum count
            max_count = max(char_count.values()) if char_count else 0
            
            # Calculate the number of characters to change
            chars_to_change = (j - i + 1) - max_count
            
            # If the number of characters to change is less than or equal to k, update max_length
            if chars_to_change <= k:
                max_length = max(max_length, j - i + 1)
    
    return max_length
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n³) where n is the length of the string.
  * We have two nested loops, which is O(n²)
  * For each substring, we count the occurrences of each character, which takes O(n) time
* Space Complexity: O(1) as we only need to store the count of at most 26 uppercase English letters.
* This approach is very inefficient for long strings and will likely result in a time limit exceeded error.

---

## **Step 5: Visualizing Redundant Computation**

* For string "ABAB" with k = 2:
  * We check all possible substrings (10 combinations for n=4)
  * For each substring, we count the occurrences of each character
  * We're repeatedly counting characters in overlapping substrings
  * We're also checking many substrings that clearly won't be the longest

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Sliding Window algorithm with character frequency tracking.
* The property that matches this problem is the ability to efficiently track character frequencies in a window.
* By using a sliding window approach, we can avoid checking all possible substrings.
* Alternatives like brute force would be much less efficient.
* Precondition: We need to efficiently track the maximum frequency of any character in the current window.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of checking all possible substrings, we can use a sliding window approach
* We maintain a window and track the frequency of each character in the window
* The key insight is that the window is valid if: (window_length - max_frequency) <= k
* This means we can replace at most k characters to make all characters the same
* We try to expand the window as much as possible
* If the window becomes invalid, we shrink it from the left
* We keep track of the maximum valid window size we've seen

### **Step 2: Flow Steps (Optimized)**

1. Initialize `max_length = 0`, `left = 0`, and a frequency map for characters
2. Initialize `max_frequency = 0` to track the maximum frequency of any character in the current window
3. For each index right from 0 to n-1:
   1. Increment the frequency of s[right] in the frequency map
   2. Update `max_frequency = max(max_frequency, frequency[s[right]])`
   3. If (right - left + 1 - max_frequency) > k:
      1. Decrement the frequency of s[left] in the frequency map
      2. Increment left
   4. Update `max_length = max(max_length, right - left + 1)`
4. Return `max_length`

### **Step 3: Implementation (Optimized)**

```python
def characterReplacement(s, k):
    n = len(s)
    freq = {}
    max_frequency = 0
    max_length = 0
    left = 0
    
    for right in range(n):
        # Increment frequency of the current character
        freq[s[right]] = freq.get(s[right], 0) + 1
        
        # Update the maximum frequency
        max_frequency = max(max_frequency, freq[s[right]])
        
        # If the window is invalid, shrink it from the left
        if (right - left + 1) - max_frequency > k:
            freq[s[left]] -= 1
            left += 1
        
        # Update the maximum length
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the string. We make a single pass through the string.
* Space Complexity: O(1) as we only need to store the count of at most 26 uppercase English letters.
* This is much more efficient than the brute force approach, especially for long strings.

### **Step 5: Visualizing Computation (Optimized)**

* For string "ABAB" with k = 2:
  * Initialize freq = {}, max_frequency = 0, max_length = 0, left = 0
  * For right=0 (A): freq = {A:1}, max_frequency = 1, window is valid, max_length = 1
  * For right=1 (B): freq = {A:1, B:1}, max_frequency = 1, window is valid, max_length = 2
  * For right=2 (A): freq = {A:2, B:1}, max_frequency = 2, window is valid, max_length = 3
  * For right=3 (B): freq = {A:2, B:2}, max_frequency = 2, window is valid (we can change 2 characters), max_length = 4
  * Return max_length = 4
* We're efficiently finding the longest valid substring using a sliding window.

### **Important Note on max_frequency**

In the optimized implementation, we don't decrement `max_frequency` when we shrink the window. This is because:

1. If the current maximum frequency character is not the one being removed, `max_frequency` remains unchanged.
2. If it is the one being removed, we should technically recalculate `max_frequency`, but this would be expensive.
3. Since we're only interested in the maximum valid window length, not decrementing `max_frequency` might make some windows seem invalid when they're actually valid, but this won't affect our final answer because we've already considered those windows when they were valid.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n³) | O(1) | Checks all possible substrings |
| Optimized (Algorithm: Sliding Window) | O(n) | O(1) | Uses a sliding window with character frequency tracking |

---

## **Final Takeaways**

* Using a sliding window approach allows us to efficiently find the longest valid substring.
* The key insight is that a window is valid if (window_length - max_frequency) <= k.
* This problem demonstrates how understanding the structure of the problem can lead to efficient algorithms without checking all possibilities.

---

## **Real-life Analogy**

* Imagine you're a teacher trying to form the longest row of students wearing the same color shirt. You're allowed to give at most k students a new shirt to match the majority. The brute force approach would be like checking every possible group of students and counting how many shirt changes you'd need for each group. The optimized approach is like starting with a small group and gradually adding students from the right, keeping track of the most common shirt color. If you'd need more than k shirt changes, you remove students from the left until you're back within your budget of k changes.

---

## **Summary**

* We solved the "Longest Repeating Character Replacement" problem by first considering a brute force approach that checks all possible substrings, resulting in O(n³) time complexity. We then optimized using a sliding window approach with character frequency tracking, reducing the time complexity to O(n) while maintaining O(1) space complexity. The key insight is that a window is valid if the number of characters that need to be changed (window_length - max_frequency) is less than or equal to k. This demonstrates how understanding the problem structure can lead to efficient algorithms. 