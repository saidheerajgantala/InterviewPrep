# Manacher's Algorithm (Longest Palindrome Substring)

## Name
- Category: String Algorithms
- Summary: Linear-time algorithm for finding the longest palindromic substring in a string, utilizing previously computed palindrome information to avoid redundant comparisons.

## Preconditions / Assumptions
- Input is a string or sequence of characters
- Characters can be compared for equality in constant time
- The string fits in memory
- Empty string is considered a valid input

## Properties
- Deterministic
- Complete (finds all palindromes)
- Linear time complexity O(n)
- Linear space complexity O(n)
- Finds both odd and even length palindromes
- Can be extended to find all palindromic substrings
- Utilizes dynamic programming principles

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(n) | O(n) | n = string length |
| Naive approach | O(n²) | O(1) | For comparison |
| With preprocessing | O(n) | O(n) | Handles both odd and even palindromes |
| All palindromes | O(n) | O(n) | Returns all palindromic substrings |

## When to Use / When to Avoid
- Use if:
  - Linear time complexity is required
  - Need to find longest palindromic substring
  - String has many palindromes
  - Efficiency is critical
  - Need to find all palindromic substrings
- Avoid if:
  - Simplicity is more important than efficiency (naive approach is simpler)
  - Memory is severely constrained
  - Only need to check if a string is a palindrome
  - String is very short (naive approach may be faster in practice)

## Correctness (Proof Sketch)
- The algorithm transforms the string by inserting special characters to handle both odd and even length palindromes
- For each position i, it computes P[i], the radius of the palindrome centered at i
- When expanding a palindrome centered at position i, it reuses information from previously computed palindromes
- If position i is within the boundary of a previously computed palindrome centered at position c:
  - P[i] is at least min(P[2*c-i], R-i) where R is the right boundary of the palindrome centered at c
  - This avoids redundant comparisons
- The algorithm then attempts to expand the palindrome centered at i further
- The longest palindrome is the one with the largest radius
- Time complexity is O(n) because each character is compared at most twice

## Pseudocode (canonical)
```pseudo
function manacher(s):
    # Transform string to handle both odd and even length palindromes
    # Insert special characters between each character and at boundaries
    t = "#"
    for char in s:
        t += char + "#"
    
    n = t.length
    
    # P[i] = radius of palindrome centered at i
    P = new array of size n, initialized with 0
    
    # Variables to track center and right boundary of current palindrome
    C = 0  # center of current palindrome
    R = 0  # right boundary of current palindrome
    
    for i = 1 to n-1:
        # Mirror of i with respect to C
        mirror = 2 * C - i
        
        # If i is within the right boundary, use previously computed values
        if i < R:
            P[i] = min(R - i, P[mirror])
        
        # Attempt to expand palindrome centered at i
        # Check characters on both sides as long as they match
        while i + P[i] + 1 < n and i - P[i] - 1 >= 0 and 
              t[i + P[i] + 1] == t[i - P[i] - 1]:
            P[i]++
        
        # If palindrome centered at i extends beyond R,
        # adjust center and right boundary
        if i + P[i] > R:
            C = i
            R = i + P[i]
    
    # Find the maximum element in P
    maxLen = 0
    centerIndex = 0
    
    for i = 1 to n-1:
        if P[i] > maxLen:
            maxLen = P[i]
            centerIndex = i
    
    # Extract the longest palindromic substring
    start = (centerIndex - maxLen) / 2  # Integer division
    end = start + maxLen
    
    return s.substring(start, end)

# Variant to find all palindromic substrings
function findAllPalindromes(s):
    # Transform the string
    t = "#"
    for char in s:
        t += char + "#"
    
    n = t.length
    P = new array of size n, initialized with 0
    C = 0
    R = 0
    
    for i = 1 to n-1:
        mirror = 2 * C - i
        
        if i < R:
            P[i] = min(R - i, P[mirror])
        
        while i + P[i] + 1 < n and i - P[i] - 1 >= 0 and 
              t[i + P[i] + 1] == t[i - P[i] - 1]:
            P[i]++
        
        if i + P[i] > R:
            C = i
            R = i + P[i]
    
    # Collect all palindromic substrings
    palindromes = []
    
    for i = 1 to n-1:
        center = i / 2  # Integer division
        for length = 1 to P[i]:
            if i % 2 == 1:  # Odd position in transformed string
                # Odd length palindrome in original string
                start = center - length / 2
                end = center + length / 2
                palindromes.add(s.substring(start, end + 1))
            else:  # Even position in transformed string
                # Even length palindrome in original string
                start = center - length / 2 + 1
                end = center + length / 2
                palindromes.add(s.substring(start, end))
    
    return palindromes
```

## Implementation Notes
- Use special characters (like '#') that don't appear in the input string for transformation
- For better performance, precompute the transformed string
- Be careful with index calculations when converting between original and transformed string
- Consider using a more efficient data structure for collecting palindromes
- Handle edge cases like empty string or single character string
- For very long strings, consider chunking the text
- The algorithm can be modified to find the number of palindromic substrings
- Be careful with integer division when extracting palindromes
- Consider using a custom class to represent palindromes with their start and end positions

## Edge-case Checklist
- Empty string: Return empty string
- Single character string: Return the character itself
- String with all identical characters: Entire string is a palindrome
- No palindromes longer than 1: Return any character
- Multiple palindromes of same length: Return the first one
- Palindrome at the beginning or end of string: Should be detected
- Odd and even length palindromes: Both should be handled correctly
- Special characters in the string: Should not interfere with the algorithm

## Examples
- Minimal example:
  - String: "babad"
  - Transformed: "#b#a#b#a#d#"
  - P array: [0,1,0,3,0,1,0,1,0,1,0]
  - Explanation:
    - P[3] = 3 means palindrome centered at position 3 has radius 3
    - This corresponds to "bab" in the original string
  - Result: "bab" (or "aba", both are valid)
  
- Complex example:
  - String: "cbbd"
  - Transformed: "#c#b#b#d#"
  - P array: [0,1,0,1,2,1,0,1,0]
  - Explanation:
    - P[4] = 2 means palindrome centered at position 4 has radius 2
    - This corresponds to "bb" in the original string
  - Result: "bb"

## Variants / Alternatives
- Finding all palindromic substrings: Return all palindromes instead of just the longest
- Counting palindromic substrings: Count the number of distinct palindromes
- Palindrome pairs: Find all pairs of strings that form a palindrome when concatenated
- Longest palindromic subsequence: Different problem, typically solved with dynamic programming
- Alternatives:
  - Naive approach: O(n²) time, check all possible centers
  - Dynamic programming: O(n²) time, O(n²) space
  - Suffix trees: O(n) time but more complex implementation
  - Suffix arrays: O(n log n) time

## References
- Manacher, Glenn: "A New Linear-Time 'On-Line' Algorithm for Finding the Smallest Initial Palindrome of a String" (1975)
- Gusfield, Dan: "Algorithms on Strings, Trees, and Sequences"
- Crochemore, Maxime and Rytter, Wojciech: "Jewels of Stringology"
- CP-Algorithms: "Manacher's Algorithm - Finding all sub-palindromes in O(N)"
- LeetCode Problem #5: "Longest Palindromic Substring" 