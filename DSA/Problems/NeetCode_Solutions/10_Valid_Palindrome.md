# 📝 Valid Palindrome

## **Problem Statement**

* A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward.
* Alphanumeric characters include letters and numbers.
* Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.
* Example:
  * Input: s = "A man, a plan, a canal: Panama"
  * Output: true
  * Explanation: "amanaplanacanalpanama" is a palindrome.
* Constraints:
  * 1 <= s.length <= 2 * 10^5
  * s consists only of printable ASCII characters

---

## **Step 1: Thinking Process (Brute Force)**

* We need to check if a string is a palindrome after cleaning it up
* First, we need to convert all uppercase letters to lowercase
* Then, we need to remove all non-alphanumeric characters
* Finally, we need to check if the resulting string reads the same forward and backward
* A simple approach would be to create a new string with only the valid characters, then check if it's a palindrome

---

## **Step 2: Flow Steps (Brute Force)**

1. Create an empty string `cleaned`
2. Iterate through each character in the input string:
   1. If the character is alphanumeric, convert it to lowercase and add it to `cleaned`
3. Create a reversed version of `cleaned`
4. Compare `cleaned` with its reverse
5. Return true if they are the same, false otherwise

---

## **Step 3: Brute Force Implementation (Code)**

```python
def isPalindrome(s):
    # Clean the string
    cleaned = ""
    for char in s:
        if char.isalnum():
            cleaned += char.lower()
    
    # Check if it's a palindrome
    return cleaned == cleaned[::-1]
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the length of the string.
  * Cleaning the string takes O(n) time
  * Creating the reverse takes O(n) time
  * Comparison takes O(n) time
* Space Complexity: O(n) for storing the cleaned string and its reverse.
* While the time complexity is already optimal, we can improve the space complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For input "A man, a plan, a canal: Panama":
  * Cleaned string: "amanaplanacanalpanama"
  * Reverse: "amanaplanacanalpanama"
  * Compare: They are the same, so it's a palindrome
* We're creating two new strings (cleaned and its reverse), which is unnecessary
* We can check if a string is a palindrome without creating a full reversed copy

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Two-Pointer algorithm.
* The property that matches this problem is the ability to check if a string is a palindrome by comparing characters from both ends.
* By using two pointers moving from opposite ends, we can check if a string is a palindrome in-place.
* Alternatives like creating a reversed copy would use more space.
* Precondition: We need to handle non-alphanumeric characters and case sensitivity.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of creating a new cleaned string, we can use two pointers
* One pointer starts from the beginning, the other from the end
* We skip non-alphanumeric characters
* We compare the characters (ignoring case) at both pointers
* If at any point the characters don't match, it's not a palindrome
* If the pointers meet or cross, we've checked all characters and it's a palindrome

### **Step 2: Flow Steps (Optimized)**

1. Initialize two pointers: `left = 0` and `right = len(s) - 1`
2. While `left < right`:
   1. If `s[left]` is not alphanumeric, increment `left` and continue
   2. If `s[right]` is not alphanumeric, decrement `right` and continue
   3. If the lowercase versions of `s[left]` and `s[right]` don't match, return false
   4. Otherwise, increment `left` and decrement `right`
3. Return true (if we've checked all characters and they match)

### **Step 3: Implementation (Optimized)**

```python
def isPalindrome(s):
    left, right = 0, len(s) - 1
    
    while left < right:
        # Skip non-alphanumeric characters from the left
        if not s[left].isalnum():
            left += 1
            continue
        
        # Skip non-alphanumeric characters from the right
        if not s[right].isalnum():
            right -= 1
            continue
        
        # Compare characters (ignoring case)
        if s[left].lower() != s[right].lower():
            return False
        
        # Move pointers
        left += 1
        right -= 1
    
    return True
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the string.
  * We still need to check each character at most once
* Space Complexity: O(1) as we're using only a constant amount of extra space.
* This approach is more space-efficient than the brute force method.

### **Step 5: Visualizing Computation (Optimized)**

* For input "A man, a plan, a canal: Panama":
  * left=0 ('A'), right=29 ('a') → match (both 'a' in lowercase)
  * left=1 (' '), skip non-alphanumeric
  * left=2 ('m'), right=28 ('m') → match
  * left=3 ('a'), right=27 ('a') → match
  * left=4 ('n'), right=26 ('n') → match
  * ...and so on
  * All characters match, so it's a palindrome
* We're checking if the string is a palindrome without creating any new strings.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n) | O(n) | Creates new strings |
| Optimized (Algorithm: Two-Pointer) | O(n) | O(1) | In-place comparison |

---

## **Final Takeaways**

* Using a two-pointer approach allows us to check if a string is a palindrome without creating new strings.
* The key insight is that we can compare characters from both ends of the string and skip non-alphanumeric characters.
* This problem demonstrates how we can improve space efficiency by avoiding unnecessary string operations.

---

## **Real-life Analogy**

* Imagine you have a long line of people, and you want to check if they're arranged symmetrically. The brute force approach would be like taking a photo, removing anyone not in uniform, and then checking if the photo looks the same forward and backward. The optimized approach is like having two people start from opposite ends of the line, walking toward each other and checking if each pair of people they encounter are identical (ignoring anyone not in uniform).

---

## **Summary**

* We solved the "Valid Palindrome" problem by first using a brute force approach that creates a cleaned string and checks if it's equal to its reverse, resulting in O(n) time and space complexity. We then optimized using a two-pointer approach that compares characters from both ends of the string in-place, reducing the space complexity to O(1) while maintaining the O(n) time complexity. This demonstrates how we can improve space efficiency by avoiding unnecessary string operations. 