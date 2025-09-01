# Knuth-Morris-Pratt (KMP) Algorithm

## Name
- Category: String Matching
- Summary: Linear time string matching algorithm that uses pattern preprocessing to avoid redundant comparisons by exploiting pattern self-similarities.

## Preconditions / Assumptions
- Pattern and text are sequences of comparable elements
- Pattern preprocessing is affordable
- Random access to elements is O(1)
- Pattern length (m) is typically much smaller than text length (n)

## Properties
- Deterministic
- Complete (finds all occurrences)
- Linear time complexity
- Online algorithm (can process text stream)
- No backtracking in text

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| KMP | O(n) | O(n+m) | O(n+m) | O(m) | n: text length, m: pattern length |
| Naive String Search | O(n) | O(n*m) | O(n*m) | O(1) | For comparison |
| Boyer-Moore | O(n/m) | O(n) | O(n*m) | O(m+σ) | σ: alphabet size |
| Rabin-Karp | O(n+m) | O(n+m) | O(n*m) | O(1) | With rolling hash |

## When to Use / When to Avoid
- Use if:
  - Need linear time guarantee
  - Multiple pattern searches in same text
  - Text cannot be preprocessed (streaming)
  - Pattern has many repeating substrings
- Avoid if:
  - Pattern is very short (naive approach may be faster)
  - Large alphabet with few repeats (Boyer-Moore may be better)
  - Memory is extremely constrained
  - Need approximate matching

## Correctness (Proof Sketch)
- Preprocessing: LPS (Longest Prefix Suffix) array captures pattern self-similarities
- Invariant: At each step, we know the longest prefix of pattern that matches current text window
- When mismatch occurs, LPS array tells exactly how far to shift pattern
- No character comparison is repeated for the same text position
- Termination: Algorithm processes each text character exactly once

## Pseudocode (canonical)
```pseudo
function KMP_search(text, pattern):
    n = text.length
    m = pattern.length
    matches = []
    
    # Preprocess pattern to build LPS array
    lps = computeLPS(pattern)
    
    i = 0  # index for text
    j = 0  # index for pattern
    
    while i < n:
        if pattern[j] == text[i]:
            i++
            j++
        
        if j == m:
            # Found a match
            matches.add(i - j)
            j = lps[j - 1]
        else if i < n and pattern[j] != text[i]:
            if j != 0:
                j = lps[j - 1]
            else:
                i++
    
    return matches

function computeLPS(pattern):
    m = pattern.length
    lps = new array of size m
    lps[0] = 0
    
    len = 0  # length of previous longest prefix suffix
    i = 1
    
    while i < m:
        if pattern[i] == pattern[len]:
            len++
            lps[i] = len
            i++
        else:
            if len != 0:
                len = lps[len - 1]
            else:
                lps[i] = 0
                i++
    
    return lps
```

## Implementation Notes
- Careful with zero-indexing in LPS array
- Can be optimized for specific character sets
- Consider using a more efficient LPS computation
- For multiple patterns, consider Aho-Corasick algorithm instead
- Watch for off-by-one errors in match reporting
- Can be extended to approximate matching with modifications

## Edge-case Checklist
- Empty text or pattern: Handle specially
- Pattern longer than text: No matches possible
- Single character pattern: Degenerate case, consider special handling
- Pattern with no repeating substrings: LPS will be all zeros
- Pattern equals text: Single match at position 0
- Unicode/multi-byte characters: Ensure proper character handling

## Examples
- Minimal example:
  - Text: "ABABDABACDABABCABAB"
  - Pattern: "ABABCABAB"
  - Output: Match at position 10
  
- LPS array example:
  - Pattern: "ABABCABAB"
  - LPS: [0,0,1,2,0,1,2,3,4]
  - Shows that after mismatch at position 8, we can skip to position 4

## Variants / Alternatives
- Z Algorithm: Alternative linear-time string matching
- Boyer-Moore: Often faster in practice for large alphabets
- Rabin-Karp: Uses hashing, good for multiple patterns
- Aho-Corasick: Extension for multiple patterns
- Suffix Array/Tree: For preprocessing text instead of pattern
- Bit-parallel algorithms: For short patterns
- Approximate matching variants: Allow errors/differences

## References
- Knuth, Morris, Pratt: "Fast Pattern Matching in Strings" (1977)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 32
- Sedgewick, Wayne: "Algorithms, 4th Edition", Section 5.3
- Dan Gusfield: "Algorithms on Strings, Trees, and Sequences"
