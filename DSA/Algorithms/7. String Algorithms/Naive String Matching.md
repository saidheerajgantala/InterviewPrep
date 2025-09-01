# Naive String Matching

## Name
- Category: String Matching
- Summary: Simple algorithm that finds all occurrences of a pattern string within a text by checking for a match at each possible position in the text.

## Preconditions / Assumptions
- Text and pattern are strings or sequences of characters
- Pattern length is less than or equal to text length
- Characters can be compared for equality in constant time
- Both text and pattern fit in memory

## Properties
- Deterministic
- Complete (finds all occurrences)
- Simple implementation
- No preprocessing required
- Quadratic worst-case time complexity
- Suitable for short patterns or small texts
- Inefficient for large texts or patterns with many repetitions

## Complexity (per variant)
| Variant | Time (Worst) | Time (Average) | Space | Notes |
|---|---|---|---|---|
| Standard | O(n*m) | O(n*m) | O(1) | n = text length, m = pattern length |
| With early exit | O(n*m) | O(n) | O(1) | Better average case |
| With preprocessing | O(n+m) | O(n+m) | O(m) | Not truly naive anymore |
| Rabin-Karp inspired | O(n*m) | O(n+m) | O(1) | With rolling hash |

## When to Use / When to Avoid
- Use if:
  - Simplicity is more important than efficiency
  - Pattern is short (few characters)
  - Text is small
  - No preprocessing is desired
  - Implementation simplicity is critical
- Avoid if:
  - Pattern or text is large
  - Pattern has many repetitions
  - Performance is critical
  - More efficient algorithms are available and implementable

## Correctness (Proof Sketch)
- The algorithm checks every possible position in the text
- At each position, it compares the pattern with the corresponding substring
- If all characters match, an occurrence is found
- Since all positions are checked, all occurrences must be found
- No false positives are possible since exact matching is performed
- Termination is guaranteed as the algorithm processes at most n-m+1 positions

## Pseudocode (canonical)
```pseudo
function naiveStringMatch(text, pattern):
    n = text.length
    m = pattern.length
    occurrences = []
    
    # Check each potential starting position in text
    for i = 0 to n-m:
        match = true
        
        # Check if pattern matches at position i
        for j = 0 to m-1:
            if text[i+j] != pattern[j]:
                match = false
                break
        
        if match:
            occurrences.add(i)
    
    return occurrences

# Variant with early exit optimization
function naiveStringMatchWithEarlyExit(text, pattern):
    n = text.length
    m = pattern.length
    occurrences = []
    
    # Check each potential starting position in text
    for i = 0 to n-m:
        # Check first character before full comparison
        if text[i] == pattern[0]:
            match = true
            
            # Check remaining characters
            for j = 1 to m-1:
                if text[i+j] != pattern[j]:
                    match = false
                    break
            
            if match:
                occurrences.add(i)
    
    return occurrences
```

## Implementation Notes
- For better average-case performance, check the first character before full comparison
- Consider using a sliding window approach to avoid redundant comparisons
- For case-insensitive matching, convert both strings to the same case or use case-insensitive comparison
- For large alphabets, consider character frequency analysis before full search
- Be careful with boundary conditions, especially at the end of the text
- For repeated searches with the same pattern, consider more advanced algorithms
- For Unicode or multi-byte characters, ensure proper character handling
- Consider using language-specific string matching functions which may be optimized

## Edge-case Checklist
- Empty text: Return empty result
- Empty pattern: Special handling required (matches at every position or none)
- Pattern longer than text: Return empty result
- Single character text and pattern: Simple equality check
- Pattern occurs at the beginning or end of text: Should be detected
- Pattern occurs multiple times: All occurrences should be found
- Pattern with repeating characters: May lead to worst-case performance
- Case sensitivity: Define character equality appropriately

## Examples
- Minimal example:
  - Text: "ABABDABACDABABCABAB"
  - Pattern: "ABABC"
  - Execution:
    - Check at position 0: "ABABD" vs "ABABC" (mismatch at position 4)
    - Check at position 1: "BABDA" vs "ABABC" (mismatch at position 0)
    - ...
    - Check at position 10: "ABABC" vs "ABABC" (match)
    - ...
  - Result: [10]
  
- Multiple occurrences:
  - Text: "ABABABABABA"
  - Pattern: "ABA"
  - Result: [0, 2, 4, 6, 8]

## Variants / Alternatives
- Early exit optimization: Check first character before full comparison
- Rolling comparison: Reuse previous comparison results
- Boyer-Moore inspired: Check characters from right to left
- Rabin-Karp inspired: Use rolling hash for faster comparison
- Alternatives:
  - KMP algorithm: O(n+m) time with O(m) preprocessing
  - Boyer-Moore algorithm: Sublinear average case
  - Rabin-Karp algorithm: O(n+m) average case with hashing
  - Suffix trees/arrays: For multiple pattern matching

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Sedgewick, Wayne: "Algorithms, 4th Edition"
- Knuth, Morris, Pratt: "Fast Pattern Matching in Strings" (1977)
- Gusfield, Dan: "Algorithms on Strings, Trees, and Sequences"
- Charras, Christian and Lecroq, Thierry: "Handbook of Exact String Matching Algorithms"
