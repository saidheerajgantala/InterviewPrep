# 📝 Permutation in String

## **Problem Statement**

* Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.
* In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.
* Example:
  * Input: s1 = "ab", s2 = "eidbaooo"
  * Output: true
  * Explanation: s2 contains one permutation of s1 ("ba").
* Constraints:
  * 1 <= s1.length, s2.length <= 10^4
  * s1 and s2 consist of lowercase English letters.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to check if s2 contains any permutation of s1 as a substring
* A permutation of s1 is any rearrangement of its characters
* The most straightforward approach would be to:
  1. Generate all permutations of s1
  2. Check if any of these permutations is a substring of s2
* However, generating all permutations would be very inefficient (O(n!))
* Instead, we can use the fact that two strings are permutations of each other if and only if they have the same character frequencies

---

## **Step 2: Flow Steps (Brute Force)**

1. Count the frequency of each character in s1
2. For each possible substring of s2 with length equal to s1:
   1. Count the frequency of each character in the substring
   2. Compare the frequency counts with s1
   3. If they match, return true
3. If no match is found, return false

---

## **Step 3: Brute Force Implementation (Code)**

```python
def checkInclusion(s1, s2):
    n1, n2 = len(s1), len(s2)
    
    # If s1 is longer than s2, it's impossible for s2 to contain a permutation of s1
    if n1 > n2:
        return False
    
    # Count the frequency of each character in s1
    s1_count = {}
    for char in s1:
        s1_count[char] = s1_count.get(char, 0) + 1
    
    # Check each possible substring of s2 with length equal to n1
    for i in range(n2 - n1 + 1):
        # Count the frequency of each character in the substring
        substring_count = {}
        for j in range(i, i + n1):
            substring_count[s2[j]] = substring_count.get(s2[j], 0) + 1
        
        # Compare the frequency counts
        if substring_count == s1_count:
            return True
    
    return False
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n1 + (n2 - n1 + 1) * n1) = O(n1 + n1 * n2 - n1² + n1) = O(n1 * n2), where n1 is the length of s1 and n2 is the length of s2.
  * O(n1) to count the frequency of characters in s1
  * O(n2 - n1 + 1) for the number of possible substrings of s2
  * O(n1) to count the frequency of characters in each substring
* Space Complexity: O(1) as we only need to store the count of at most 26 lowercase English letters.
* This approach is inefficient because we're recounting characters for each substring, which leads to redundant calculations.

---

## **Step 5: Visualizing Redundant Computation**

* For s1 = "ab" and s2 = "eidbaooo":
  * s1_count = {'a': 1, 'b': 1}
  * For substring "ei": {'e': 1, 'i': 1} != s1_count
  * For substring "id": {'i': 1, 'd': 1} != s1_count
  * For substring "db": {'d': 1, 'b': 1} != s1_count
  * For substring "ba": {'b': 1, 'a': 1} == s1_count, return true
* We're repeatedly counting characters in overlapping substrings
* For example, when we move from "ei" to "id", we're recounting 'i' which we've already counted

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Sliding Window algorithm with character frequency tracking.
* The property that matches this problem is the ability to efficiently track character frequencies in a fixed-size window.
* By using a sliding window approach, we can avoid recounting characters for each substring.
* Alternatives like generating all permutations would be much less efficient.
* Precondition: Two strings are permutations of each other if and only if they have the same character frequencies.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of recounting characters for each substring, we can use a sliding window approach
* We maintain a window of size equal to the length of s1
* As we slide the window through s2, we update the character frequencies
* We compare these frequencies with the frequencies in s1
* If they match at any point, we've found a permutation of s1 in s2

### **Step 2: Flow Steps (Optimized)**

1. Count the frequency of each character in s1
2. Initialize a window of size n1 at the beginning of s2 and count the frequency of each character in this window
3. Compare the frequency counts. If they match, return true
4. Slide the window through s2:
   1. Remove the leftmost character from the window and decrement its count
   2. Add the next character to the window and increment its count
   3. Compare the frequency counts. If they match, return true
5. If no match is found, return false

### **Step 3: Implementation (Optimized)**

```python
def checkInclusion(s1, s2):
    n1, n2 = len(s1), len(s2)
    
    # If s1 is longer than s2, it's impossible for s2 to contain a permutation of s1
    if n1 > n2:
        return False
    
    # Count the frequency of each character in s1 and the first window in s2
    s1_count = [0] * 26
    s2_count = [0] * 26
    
    for i in range(n1):
        s1_count[ord(s1[i]) - ord('a')] += 1
        s2_count[ord(s2[i]) - ord('a')] += 1
    
    # Check if the first window is a permutation of s1
    if s1_count == s2_count:
        return True
    
    # Slide the window through s2
    for i in range(n1, n2):
        # Add the next character to the window
        s2_count[ord(s2[i]) - ord('a')] += 1
        
        # Remove the leftmost character from the window
        s2_count[ord(s2[i - n1]) - ord('a')] -= 1
        
        # Check if the current window is a permutation of s1
        if s1_count == s2_count:
            return True
    
    return False
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n1 + n2), where n1 is the length of s1 and n2 is the length of s2.
  * O(n1) to count the frequency of characters in s1 and the first window in s2
  * O(n2 - n1) to slide the window through the rest of s2
  * O(1) for each comparison of frequency counts (since we're comparing arrays of fixed size 26)
* Space Complexity: O(1) as we only need to store the count of at most 26 lowercase English letters.
* This is much more efficient than the brute force approach, especially for long strings.

### **Step 5: Visualizing Computation (Optimized)**

* For s1 = "ab" and s2 = "eidbaooo":
  * s1_count = [1,1,0,0,...] (for 'a' and 'b')
  * Initialize window "ei": s2_count = [0,0,0,0,1,0,...,1,0,...] (for 'e' and 'i')
  * s1_count != s2_count
  * Slide window to "id": Add 'd', remove 'e' => s2_count = [0,0,0,1,0,0,...,1,0,...] (for 'd' and 'i')
  * s1_count != s2_count
  * Slide window to "db": Add 'b', remove 'i' => s2_count = [0,1,0,1,0,0,...] (for 'b' and 'd')
  * s1_count != s2_count
  * Slide window to "ba": Add 'a', remove 'd' => s2_count = [1,1,0,0,...] (for 'a' and 'b')
  * s1_count == s2_count, return true
* We're efficiently updating character frequencies as we slide the window, avoiding redundant calculations.

### **Further Optimization**

We can optimize the comparison of frequency counts by keeping track of how many characters have matching frequencies. This way, we don't need to compare the entire arrays each time.

```python
def checkInclusion(s1, s2):
    n1, n2 = len(s1), len(s2)
    
    if n1 > n2:
        return False
    
    s1_count = [0] * 26
    s2_count = [0] * 26
    
    # Count frequencies for s1 and the first window of s2
    for i in range(n1):
        s1_count[ord(s1[i]) - ord('a')] += 1
        s2_count[ord(s2[i]) - ord('a')] += 1
    
    # Count how many characters have matching frequencies
    matches = 0
    for i in range(26):
        if s1_count[i] == s2_count[i]:
            matches += 1
    
    # Slide the window
    for i in range(n1, n2):
        if matches == 26:
            return True
        
        # Add the next character
        right_char_idx = ord(s2[i]) - ord('a')
        s2_count[right_char_idx] += 1
        
        # Update matches for the added character
        if s2_count[right_char_idx] == s1_count[right_char_idx]:
            matches += 1
        elif s2_count[right_char_idx] == s1_count[right_char_idx] + 1:
            matches -= 1
        
        # Remove the leftmost character
        left_char_idx = ord(s2[i - n1]) - ord('a')
        s2_count[left_char_idx] -= 1
        
        # Update matches for the removed character
        if s2_count[left_char_idx] == s1_count[left_char_idx]:
            matches += 1
        elif s2_count[left_char_idx] == s1_count[left_char_idx] - 1:
            matches -= 1
    
    return matches == 26
```

This optimization reduces the comparison time from O(26) to O(1) for each window, though the overall time complexity remains O(n1 + n2).

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n1 * n2) | O(1) | Recounts characters for each substring |
| Optimized (Algorithm: Sliding Window) | O(n1 + n2) | O(1) | Uses a sliding window with character frequency tracking |
| Further Optimized | O(n1 + n2) | O(1) | Optimizes the comparison of frequency counts |

---

## **Final Takeaways**

* Using a sliding window approach allows us to efficiently check if s2 contains a permutation of s1.
* The key insight is that we can update character frequencies incrementally as we slide the window.
* This problem demonstrates how understanding the properties of permutations (same character frequencies) can lead to efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're a quality control inspector checking if a specific batch of ingredients (s1) was used in any part of a long production line (s2). The brute force approach would be like taking samples at each position and completely analyzing each sample from scratch. The optimized approach is like moving along the production line with a testing device that updates its readings incrementally - adding new ingredients as they enter the sample window and subtracting those that leave, quickly flagging when the sample matches your target batch.

---

## **Summary**

* We solved the "Permutation in String" problem by first considering a brute force approach that recounts characters for each substring, resulting in O(n1 * n2) time complexity. We then optimized using a sliding window approach with character frequency tracking, reducing the time complexity to O(n1 + n2) while maintaining O(1) space complexity. We further optimized by efficiently tracking matching character frequencies. The key insight is that two strings are permutations of each other if and only if they have the same character frequencies, which allows us to efficiently check for permutations without generating them explicitly. 