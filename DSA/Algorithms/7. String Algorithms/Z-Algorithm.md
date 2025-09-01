# Z-Algorithm

## Name
- Category: String Matching, String Processing
- Summary: Linear-time algorithm that computes the Z-array, which gives the length of the longest substring starting at each position that is also a prefix of the string, with applications in pattern matching and string analysis.

## Preconditions / Assumptions
- Input is a string or sequence of characters
- Characters can be compared for equality in constant time
- The string fits in memory
- For pattern matching, pattern and text can be concatenated with a special character

## Properties
- Deterministic
- Complete (finds all matches)
- Linear time complexity
- Linear space complexity
- No preprocessing required beyond Z-array computation
- Can be used for multiple pattern matching problems
- Particularly efficient for repeated pattern detection

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(n) | O(n) | n = string length |
| Pattern matching | O(n+m) | O(n+m) | n = text length, m = pattern length |
| Extended Z-array | O(n) | O(n) | With additional information |
| Bidirectional | O(n) | O(n) | Computing Z-values from both ends |

## When to Use / When to Avoid
- Use if:
  - Linear time complexity is required
  - Need to find all matches of a pattern in text
  - Need to analyze string prefixes efficiently
  - String has repeated patterns or substrings
  - Memory usage is not a critical constraint
- Avoid if:
  - Pattern is very short (naive algorithm may be simpler)
  - Special characters cannot be used for concatenation
  - Memory is severely constrained
  - Problem requires approximate matching

## Correctness (Proof Sketch)
- The Z-array Z[i] represents the length of the longest substring starting at position i that is also a prefix of the string
- The algorithm maintains a "Z-box" [L,R] where R is the rightmost position reached so far
- For each position i:
  - If i is outside the current Z-box, compute Z[i] naively
  - If i is inside the Z-box, initialize Z[i] based on previously computed values
  - Extend Z[i] by character-by-character comparison if possible
  - Update the Z-box if necessary
- The amortized time complexity is O(n) because each character is compared at most twice
- For pattern matching, concatenate pattern and text with a special character and find Z-values matching pattern length

## Pseudocode (canonical)
```pseudo
function computeZArray(s):
    n = s.length
    z = new array of size n, initialized with 0
    
    # [L,R] make a window which matches with prefix of s
    L = 0
    R = 0
    
    for i = 1 to n-1:
        if i > R:
            # If i is outside the current Z-box, compute Z[i] naively
            L = i
            R = i
            
            while R < n and s[R-L] == s[R]:
                R++
            
            z[i] = R - L
            R--
        else:
            # i is inside the Z-box
            k = i - L
            
            # If Z[k] is less than remaining Z-box length, Z[i] = Z[k]
            if z[k] < R - i + 1:
                z[i] = z[k]
            else:
                # Otherwise, we need to extend the Z-box
                L = i
                
                while R < n and s[R-L] == s[R]:
                    R++
                
                z[i] = R - L
                R--
    
    return z

# Pattern matching using Z-algorithm
function zSearch(text, pattern):
    # Concatenate pattern and text with a special character
    concat = pattern + "$" + text
    n = concat.length
    
    # Compute Z-array
    z = computeZArray(concat)
    
    # Find all occurrences of pattern in text
    occurrences = []
    pattern_length = pattern.length
    
    for i = 0 to n-1:
        if z[i] == pattern_length:
            # Found pattern at position i - pattern_length - 1 in text
            occurrences.add(i - pattern_length - 1)
    
    return occurrences
```

## Implementation Notes
- Choose a special character for concatenation that doesn't appear in the pattern or text
- For multiple pattern matching, consider using Aho-Corasick algorithm instead
- Z-array can be used to solve various string problems beyond pattern matching
- Be careful with boundary conditions, especially at the beginning and end of the string
- For very long strings, consider chunking the text
- The Z-algorithm can be modified to compute the longest common prefix (LCP) array
- Consider using Z-algorithm for string compression or finding repeated substrings
- For pattern matching in practice, KMP or Boyer-Moore may be more efficient

## Edge-case Checklist
- Empty string: Return empty Z-array
- Single character string: Z[0] is undefined (typically 0), all others are 0
- String with all identical characters: Z[i] = n-i for all i
- No matches in pattern matching: Return empty result
- Pattern longer than text: Return empty result
- Special character appears in pattern or text: Choose a different special character
- Pattern occurs at the beginning or end of text: Should be detected
- Pattern occurs multiple times: All occurrences should be found

## Examples
- Z-array computation:
  - String: "aabcaabxaaz"
  - Z-array: [0, 1, 0, 0, 3, 1, 0, 0, 2, 1, 0]
  - Explanation:
    - Z[0] = 0 (by definition)
    - Z[1] = 1 (longest prefix starting at position 1 is "a")
    - Z[2] = 0 (no prefix match starting at position 2)
    - Z[3] = 0 (no prefix match starting at position 3)
    - Z[4] = 3 (longest prefix starting at position 4 is "aab")
    - Z[5] = 1 (longest prefix starting at position 5 is "a")
    - Z[6] = 0 (no prefix match starting at position 6)
    - Z[7] = 0 (no prefix match starting at position 7)
    - Z[8] = 2 (longest prefix starting at position 8 is "aa")
    - Z[9] = 1 (longest prefix starting at position 9 is "a")
    - Z[10] = 0 (no prefix match starting at position 10)
  
- Pattern matching:
  - Pattern: "aab"
  - Text: "aabcaabxaaz"
  - Concatenated: "aab$aabcaabxaaz"
  - Z-array: [0, 0, 0, 0, 3, 1, 0, 3, 1, 0, 0, 2, 1, 0, 0]
  - Occurrences at positions where Z[i] = pattern.length = 3: [4, 7]
  - Result: [0, 4] (positions in original text)

## Variants / Alternatives
- Extended Z-algorithm: Computes additional information for string analysis
- Bidirectional Z-algorithm: Computes Z-values from both ends
- Suffix array construction: Z-algorithm can be used as a component
- Longest common prefix computation: Related application
- Alternatives:
  - KMP algorithm: Similar linear-time pattern matching
  - Boyer-Moore algorithm: Often faster in practice
  - Suffix trees/arrays: For more complex string problems
  - Rabin-Karp algorithm: Uses hashing for pattern matching

## References
- Gusfield, Dan: "Algorithms on Strings, Trees, and Sequences"
- Crochemore, Maxime and Rytter, Wojciech: "Jewels of Stringology"
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- CP-Algorithms: "Z function and its applications"
- Competitive Programming resources and tutorials
