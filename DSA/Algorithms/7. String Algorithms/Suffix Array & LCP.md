# Suffix Array & LCP

## Name
- Category: String Algorithms, Data Structures
- Summary: Data structures that represent all suffixes of a string in lexicographically sorted order (Suffix Array) and the length of the longest common prefix between adjacent suffixes (LCP Array), enabling efficient string processing operations.

## Preconditions / Assumptions
- Input is a string or sequence of characters
- Characters can be compared for equality and order in constant time
- The string fits in memory
- Empty string is considered a valid input
- String is typically terminated with a unique character (e.g., '$')

## Properties
- Deterministic
- Complete (represents all suffixes)
- Construction time complexity: O(n log n) or O(n) with advanced algorithms
- Space complexity: O(n)
- Enables efficient string operations like pattern matching, longest common substring, etc.
- Can be combined with other string data structures like suffix trees
- More space-efficient than suffix trees

## Complexity (per variant)
| Variant | Construction Time | Space | Query Time | Notes |
|---|---|---|---|---|
| Naive | O(n² log n) | O(n) | O(m log n) | n = string length, m = pattern length |
| Prefix Doubling | O(n log n) | O(n) | O(m log n) | Manber-Myers algorithm |
| SA-IS | O(n) | O(n) | O(m log n) | Induced Sorting, optimal construction |
| DC3 | O(n) | O(n) | O(m log n) | Difference Cover, optimal construction |
| LCP (Kasai) | O(n) | O(n) | O(1) | Requires suffix array |

## When to Use / When to Avoid
- Use if:
  - Need to perform multiple string operations efficiently
  - Space efficiency is important (compared to suffix trees)
  - Need to find patterns, common substrings, or repetitions
  - String operations involve lexicographical ordering
  - Memory usage is a concern but not critical
- Avoid if:
  - Simplicity is more important than efficiency
  - Memory is severely constrained
  - Only need to perform a single string operation
  - Need to perform complex tree-based operations (suffix tree might be better)
  - String is very short (naive approaches may be faster in practice)

## Correctness (Proof Sketch)
- Suffix Array:
  - Contains integers representing starting positions of all suffixes in lexicographical order
  - Correctness follows from the correctness of the sorting algorithm used
  - Each suffix is uniquely identified by its starting position
  - The array covers all suffixes of the string
- LCP Array:
  - LCP[i] represents the length of the longest common prefix between suffixes at SA[i] and SA[i-1]
  - Kasai's algorithm computes LCP by utilizing the properties of adjacent suffixes
  - It processes suffixes in their original order, not lexicographical order
  - The algorithm maintains the invariant that LCP values decrease by at most 1 when moving to the next suffix
  - Time complexity is O(n) because each character is processed at most once

## Pseudocode (canonical)
```pseudo
# Naive Suffix Array Construction
function buildSuffixArray_naive(s):
    n = s.length
    
    # Create array of suffix objects
    suffixes = []
    for i = 0 to n-1:
        suffixes.add((i, s.substring(i)))
    
    # Sort suffixes lexicographically
    sort(suffixes) based on the second element (substring)
    
    # Extract the indices to form the suffix array
    sa = []
    for (index, _) in suffixes:
        sa.add(index)
    
    return sa

# Prefix Doubling Algorithm (Manber-Myers)
function buildSuffixArray_prefixDoubling(s):
    n = s.length
    
    # Initialize arrays
    sa = new array of size n
    rank = new array of size n
    temp = new array of size n
    
    # Initialize rank array with character values
    for i = 0 to n-1:
        rank[i] = ord(s[i])
        sa[i] = i
    
    for k = 1; k < n; k *= 2:
        # Sort by pair (rank[i], rank[i+k])
        countingSort(sa, rank, k)
        
        # Update rank array
        temp[sa[0]] = 0
        for i = 1 to n-1:
            if rank[sa[i]] == rank[sa[i-1]] and 
               rank[(sa[i]+k) % n] == rank[(sa[i-1]+k) % n]:
                temp[sa[i]] = temp[sa[i-1]]
            else:
                temp[sa[i]] = temp[sa[i-1]] + 1
        
        # Copy temp to rank
        for i = 0 to n-1:
            rank[i] = temp[i]
        
        # If all ranks are unique, we're done
        if rank[sa[n-1]] == n-1:
            break
    
    return sa

# Kasai's Algorithm for LCP Array
function buildLCPArray(s, sa):
    n = s.length
    lcp = new array of size n, initialized with 0
    
    # Create inverse suffix array
    # inv[i] = position of suffix starting at i in the suffix array
    inv = new array of size n
    for i = 0 to n-1:
        inv[sa[i]] = i
    
    # Initialize with length of common prefix
    k = 0
    
    for i = 0 to n-1:
        if inv[i] == n-1:
            # Last suffix in SA, no next suffix
            k = 0
            continue
        
        # Next suffix in SA
        j = sa[inv[i] + 1]
        
        # Extend previous LCP
        while i + k < n and j + k < n and s[i + k] == s[j + k]:
            k++
        
        # LCP for the current suffix
        lcp[inv[i]] = k
        
        # When we move to the next suffix, LCP can decrease by at most 1
        if k > 0:
            k--
    
    return lcp

# Pattern matching using Suffix Array
function search(s, sa, pattern):
    n = s.length
    m = pattern.length
    
    # Binary search for the lower bound
    left = 0
    right = n - 1
    
    while left <= right:
        mid = (left + right) / 2
        
        # Compare pattern with suffix at sa[mid]
        result = compare(pattern, s.substring(sa[mid], sa[mid] + m))
        
        if result == 0:  # Match found
            return sa[mid]
        else if result < 0:  # Pattern is lexicographically smaller
            right = mid - 1
        else:  # Pattern is lexicographically larger
            left = mid + 1
    
    return -1  # Pattern not found
```

## Implementation Notes
- For practical implementations, use linear-time construction algorithms like SA-IS or DC3
- Add a unique terminating character (like '$') to ensure all suffixes are distinct
- For large strings, consider using integer arrays and character codes instead of actual substrings
- Implement binary search for pattern matching efficiently
- Consider using the LCP array to optimize pattern matching and other string operations
- Be careful with index calculations and off-by-one errors
- For very long strings, consider chunking or external memory algorithms
- Implement specialized algorithms for common operations like longest common substring
- Consider using sparse tables or segment trees with LCP array for range minimum queries
- For multiple pattern matching, consider using enhanced suffix arrays or suffix trees

## Edge-case Checklist
- Empty string: Return appropriate empty arrays
- Single character string: Simple case, handle directly
- String with all identical characters: All suffixes have LCP values of n-1-i
- String with unique terminating character: Ensure it's handled correctly
- Very long patterns: May require special handling
- Patterns not in the string: Should return -1 or empty result
- Case sensitivity: Define character comparison appropriately
- Special characters: Ensure proper ordering in lexicographical comparison

## Examples
- Minimal example:
  - String: "banana$"
  - Suffixes: "banana$", "anana$", "nana$", "ana$", "na$", "a$", "$"
  - Sorted suffixes: "$", "a$", "ana$", "anana$", "banana$", "na$", "nana$"
  - Suffix Array: [6, 5, 3, 1, 0, 4, 2]
  - LCP Array: [0, 0, 1, 3, 0, 0, 2]
  - Explanation:
    - SA[0] = 6: Suffix "$"
    - SA[1] = 5: Suffix "a$", LCP with previous = 0
    - SA[2] = 3: Suffix "ana$", LCP with previous = 1 (common prefix "a")
    - SA[3] = 1: Suffix "anana$", LCP with previous = 3 (common prefix "ana")
    - SA[4] = 0: Suffix "banana$", LCP with previous = 0
    - SA[5] = 4: Suffix "na$", LCP with previous = 0
    - SA[6] = 2: Suffix "nana$", LCP with previous = 2 (common prefix "na")
  
- Pattern matching:
  - String: "banana$"
  - Pattern: "ana"
  - Binary search in suffix array: [6, 5, 3, 1, 0, 4, 2]
  - Found at position SA[2] = 3 (and also at SA[3] = 1)
  - Result: [1, 3]

## Variants / Alternatives
- Enhanced Suffix Array: Includes additional arrays for more efficient operations
- Sparse Suffix Array: Stores only a subset of suffixes to save space
- Compressed Suffix Array: Uses less space with some performance tradeoff
- Burrows-Wheeler Transform: Related transformation with applications in compression
- Alternatives:
  - Suffix Tree: More space-intensive but supports some operations more efficiently
  - Suffix Automaton: Compact representation for pattern matching
  - FM-Index: Compressed full-text index based on BWT
  - KMP algorithm: For simple pattern matching

## References
- Manber, Udi and Myers, Gene: "Suffix Arrays: A New Method for On-Line String Searches" (1993)
- Kasai, Toru et al.: "Linear-Time Longest-Common-Prefix Computation in Suffix Arrays and Its Applications" (2001)
- Nong, Ge et al.: "Linear Suffix Array Construction by Almost Pure Induced-Sorting" (2009)
- Kärkkäinen, Juha and Sanders, Peter: "Simple Linear Work Suffix Array Construction" (2003)
- Gusfield, Dan: "Algorithms on Strings, Trees, and Sequences"
- Abouelhoda, Mohamed et al.: "Replacing Suffix Trees with Enhanced Suffix Arrays" (2004)
