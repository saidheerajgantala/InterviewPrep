# Rabin-Karp Algorithm

## Name
- Category: String Matching
- Summary: String searching algorithm that uses hashing to find patterns in text, comparing hash values instead of comparing the strings character by character, with average-case linear time complexity.

## Preconditions / Assumptions
- Text and pattern are strings or sequences of characters
- Pattern length is less than or equal to text length
- A suitable hash function is available
- Both text and pattern fit in memory
- Hash collisions are possible but rare

## Properties
- Deterministic
- Complete (finds all occurrences)
- Uses rolling hash for efficiency
- Average-case linear time complexity
- Worst-case quadratic time complexity (with many hash collisions)
- Suitable for multiple pattern matching
- Good for patterns with repeated structures

## Complexity (per variant)
| Variant | Time (Worst) | Time (Average) | Space | Notes |
|---|---|---|---|---|
| Standard | O(n*m) | O(n+m) | O(1) | n = text length, m = pattern length |
| Multiple patterns | O(n*m*k) | O(n+m*k) | O(k) | k = number of patterns |
| With perfect hash | O(n+m) | O(n+m) | O(1) | No collisions |
| With fingerprinting | O(n+m) | O(n+m) | O(1) | Probabilistic correctness |

## When to Use / When to Avoid
- Use if:
  - Average-case linear time is important
  - Multiple pattern matching is needed
  - Pattern has repeated structures
  - Hash function is efficient for the character set
  - Some probability of false positives is acceptable (with verification)
- Avoid if:
  - Worst-case guarantees are required
  - Pattern is very short (naive algorithm may be faster)
  - Hash function is expensive for the character set
  - No good rolling hash function is available
  - Zero probability of false positives is required without verification

## Correctness (Proof Sketch)
- The algorithm computes hash values for the pattern and each m-length substring of the text
- Hash values are compared, and only if they match, the actual strings are compared
- Rolling hash allows O(1) computation of the next substring hash from the previous one
- If the hash function has low collision probability, most non-matches are rejected by hash comparison
- All actual matches will be found because their hash values will match
- False positives (hash collisions) are handled by explicit string comparison
- Termination is guaranteed as the algorithm processes at most n-m+1 positions

## Pseudocode (canonical)
```pseudo
function rabinKarp(text, pattern):
    n = text.length
    m = pattern.length
    d = radix (e.g., 256 for ASCII)
    q = prime number (large enough to avoid overflow)
    occurrences = []
    
    # Compute hash value for pattern and first window of text
    pattern_hash = 0
    text_hash = 0
    h = 1
    
    # Compute h = d^(m-1) % q
    for i = 1 to m-1:
        h = (h * d) % q
    
    # Compute initial hash values
    for i = 0 to m-1:
        pattern_hash = (d * pattern_hash + pattern[i]) % q
        text_hash = (d * text_hash + text[i]) % q
    
    # Slide pattern over text one by one
    for i = 0 to n-m:
        # Check if hash values match
        if pattern_hash == text_hash:
            # Verify character by character
            match = true
            for j = 0 to m-1:
                if text[i+j] != pattern[j]:
                    match = false
                    break
            
            if match:
                occurrences.add(i)
        
        # Compute hash value for next window
        if i < n-m:
            text_hash = (d * (text_hash - text[i] * h) + text[i+m]) % q
            
            # Ensure hash is positive
            if text_hash < 0:
                text_hash += q
    
    return occurrences

# Multiple pattern variant
function rabinKarpMultiplePatterns(text, patterns):
    n = text.length
    d = radix
    q = prime number
    occurrences = new map()
    
    # Compute hash values for all patterns
    pattern_hashes = new map()
    for each pattern in patterns:
        m = pattern.length
        hash = 0
        for i = 0 to m-1:
            hash = (d * hash + pattern[i]) % q
        pattern_hashes[pattern] = hash
        occurrences[pattern] = []
    
    # For each possible pattern length
    for each unique length m in patterns:
        # Compute initial hash for text window
        text_hash = 0
        h = 1
        
        # Compute h = d^(m-1) % q
        for i = 1 to m-1:
            h = (h * d) % q
        
        for i = 0 to m-1:
            text_hash = (d * text_hash + text[i]) % q
        
        # Slide pattern over text
        for i = 0 to n-m:
            # Check if hash matches any pattern of this length
            for each pattern of length m:
                if text_hash == pattern_hashes[pattern]:
                    # Verify match
                    match = true
                    for j = 0 to m-1:
                        if text[i+j] != pattern[j]:
                            match = false
                            break
                    
                    if match:
                        occurrences[pattern].add(i)
            
            # Compute hash for next window
            if i < n-m:
                text_hash = (d * (text_hash - text[i] * h) + text[i+m]) % q
                if text_hash < 0:
                    text_hash += q
    
    return occurrences
```

## Implementation Notes
- Choose a good hash function with low collision probability
- Select a large prime number for modulo operation to minimize collisions
- Use efficient rolling hash computation to update hash values in O(1)
- Be careful with integer overflow when computing hash values
- Consider using double hashing to reduce collision probability
- For multiple patterns, group by length for efficiency
- Optimize verification step for expected match frequency
- Consider precomputing pattern hashes for repeated searches
- For very long texts, consider chunking the text

## Edge-case Checklist
- Empty text: Return empty result
- Empty pattern: Special handling required
- Pattern longer than text: Return empty result
- Single character text and pattern: Simple equality check
- Pattern occurs at the beginning or end of text: Should be detected
- Pattern occurs multiple times: All occurrences should be found
- Hash collisions: Verify matches by comparing actual strings
- Integer overflow: Use modular arithmetic carefully
- Negative hash values: Ensure hash values remain positive

## Examples
- Minimal example:
  - Text: "ABABDABACDABABCABAB"
  - Pattern: "ABABC"
  - Hash function: h(s) = (s[0] * 256^4 + s[1] * 256^3 + ... + s[4]) mod 101
  - Pattern hash: h("ABABC") = 13
  - Window hashes:
    - h("ABABD") = 14 (mismatch)
    - h("BABDA") = 42 (mismatch)
    - ...
    - h("ABABC") = 13 (hash match, verify: "ABABC" = "ABABC", actual match)
    - ...
  - Result: [10]
  
- Multiple patterns:
  - Text: "ABABCABABABA"
  - Patterns: ["ABA", "ABAB", "ABABC"]
  - Result: {"ABA": [0, 5, 7, 9], "ABAB": [0, 5], "ABABC": [0]}

## Variants / Alternatives
- Karp-Rabin fingerprinting: Focus on probabilistic guarantees
- Multiple pattern matching: Search for multiple patterns simultaneously
- 2D Rabin-Karp: For pattern matching in 2D arrays or matrices
- Rolling hash variants: Different hash functions for specific applications
- Alternatives:
  - KMP algorithm: O(n+m) worst-case with O(m) preprocessing
  - Boyer-Moore algorithm: Sublinear average case
  - Aho-Corasick algorithm: For multiple pattern matching
  - Suffix trees/arrays: For complex string problems

## References
- Rabin, Michael O. and Karp, Richard M.: "Efficient randomized pattern-matching algorithms" (1987)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Sedgewick, Wayne: "Algorithms, 4th Edition"
- Gusfield, Dan: "Algorithms on Strings, Trees, and Sequences"
- Charras, Christian and Lecroq, Thierry: "Handbook of Exact String Matching Algorithms"
