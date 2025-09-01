# 📝 Valid Anagram

## **Problem Statement**

* Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.
* An anagram is a word formed by rearranging the letters of another word, using all the original letters exactly once.
* Example:
  * Input: s = "anagram", t = "nagaram"
  * Output: true
* Constraints:
  * 1 <= s.length, t.length <= 5 * 10^4
  * s and t consist of lowercase English letters

---

## **Step 1: Thinking Process (Brute Force)**

* Two strings are anagrams if they contain exactly the same characters with the same frequencies
* One approach is to sort both strings and then compare them
* If they are anagrams, the sorted strings should be identical
* This is straightforward and easy to implement

---

## **Step 2: Flow Steps (Brute Force)**

1. If the lengths of s and t are different, return false (they can't be anagrams)
2. Sort string s
3. Sort string t
4. Compare the sorted strings
5. Return true if they're equal, false otherwise

---

## **Step 3: Brute Force Implementation (Code)**

```python
def isAnagram(s, t):
    if len(s) != len(t):
        return False
    
    sorted_s = sorted(s)
    sorted_t = sorted(t)
    
    return sorted_s == sorted_t
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n log n) where n is the length of the strings. Sorting is the dominant operation.
* Space Complexity: O(n) to store the sorted strings.
* While not terribly inefficient, sorting is more expensive than necessary for this problem.

---

## **Step 5: Visualizing Redundant Computation**

* For strings s = "anagram", t = "nagaram":
  * We sort s → "aaagmnr"
  * We sort t → "aaagmnr"
  * Then compare them
* The sorting operation is overkill - we don't need the letters in order, we just need to count them.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Hash Table (Counter) algorithm.
* The property that matches this problem is frequency counting.
* A hash table allows us to count character frequencies in linear time.
* Alternatives like sorting work but are less efficient at O(n log n).
* Precondition: We're working with a finite character set (lowercase English letters), which makes the space complexity effectively constant.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of sorting, we can count the frequency of each character in both strings
* For anagrams, the character counts must be identical
* We can use a hash table (dictionary) to store character frequencies
* If the frequency tables match, the strings are anagrams

### **Step 2: Flow Steps (Optimized)**

1. If the lengths of s and t are different, return false
2. Create a hash table (dictionary) to store character frequencies
3. Iterate through s, incrementing the count for each character
4. Iterate through t, decrementing the count for each character
5. If any count becomes negative, return false
6. Return true if all counts are zero

### **Step 3: Implementation (Optimized)**

```python
def isAnagram(s, t):
    if len(s) != len(t):
        return False
    
    char_count = {}
    
    # Count characters in s
    for char in s:
        if char in char_count:
            char_count[char] += 1
        else:
            char_count[char] = 1
    
    # Decrement counts for characters in t
    for char in t:
        if char not in char_count or char_count[char] == 0:
            return False
        char_count[char] -= 1
    
    return True
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the strings. We only need to iterate through each string once.
* Space Complexity: O(k) where k is the size of the character set (26 for lowercase English letters), which is effectively O(1).
* This is more efficient than the sorting approach, especially for large strings.

### **Step 5: Visualizing Computation (Optimized)**

* For strings s = "anagram", t = "nagaram":
  * Count s: {'a': 3, 'n': 1, 'g': 1, 'r': 1, 'm': 1}
  * Process t: decrement counts for each character
  * Final counts: all zeros, so they're anagrams

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Sorting) | O(n log n) | O(n) | Sorts both strings and compares |
| Optimized (Algorithm: Hash Table) | O(n) | O(1) | Counts character frequencies |

---

## **Final Takeaways**

* Frequency counting with hash tables is a common pattern for problems involving character/element distributions.
* When comparing compositions (rather than order), consider counting instead of sorting.
* This problem demonstrates how understanding the exact requirements can lead to more efficient algorithms.

---

## **Real-life Analogy**

* Imagine comparing two bags of Scrabble tiles to see if they contain the same letters. The brute force approach would be to arrange both sets of tiles in alphabetical order and compare them side by side. The optimized approach is like counting how many of each letter you have in each bag - much faster than sorting them first.

---

## **Summary**

* We solved the "Valid Anagram" problem by first using a sorting approach with O(n log n) time complexity. We then recognized that sorting was unnecessary and optimized to a linear-time solution using a hash table to count character frequencies. This reduced our time complexity to O(n) with effectively constant space complexity, showing how understanding the problem's essence (frequency matching rather than order) leads to more efficient solutions. 