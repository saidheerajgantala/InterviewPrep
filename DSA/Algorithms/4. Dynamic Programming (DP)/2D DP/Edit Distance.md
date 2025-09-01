# Edit Distance

## Name
- Category: Dynamic Programming (2D)
- Summary: Algorithm to compute the minimum number of operations (insertions, deletions, substitutions) required to transform one string into another.

## Preconditions / Assumptions
- Two input strings (source and target)
- Operations allowed: insertion, deletion, substitution
- Each operation has a cost of 1 (can be modified for weighted operations)
- Goal is to minimize the total number of operations

## Properties
- Deterministic
- Complete (finds optimal solution)
- Optimal substructure (solution built from solutions to overlapping subproblems)
- 2D dynamic programming approach
- Also known as Levenshtein distance

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard DP | O(m*n) | O(m*n) | m, n are lengths of strings |
| Space-optimized | O(m*n) | O(min(m,n)) | Only need two rows/columns |
| With backtracking | O(m*n) | O(m*n) | For reconstructing operations |
| Recursive + Memo | O(m*n) | O(m*n) | Plus recursion stack |

## When to Use / When to Avoid
- Use if:
  - Need to measure string similarity
  - Need to find minimum edit operations
  - Spell checking or correction
  - DNA sequence alignment
  - Fuzzy string matching
- Avoid if:
  - Very long strings (consider approximation algorithms)
  - Only need to check if strings are exactly equal
  - Need specialized distance metrics (e.g., Hamming distance for equal-length strings)
  - Memory is severely constrained

## Correctness (Proof Sketch)
- Base cases:
  - Empty source: Insert all characters from target
  - Empty target: Delete all characters from source
- Recurrence relation:
  - If current characters match: dp[i][j] = dp[i-1][j-1]
  - If current characters don't match: dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
    - dp[i-1][j]: Deletion
    - dp[i][j-1]: Insertion
    - dp[i-1][j-1]: Substitution
- Optimal substructure: Solution for i,j depends on solutions for smaller subproblems
- Termination: After filling the entire dp table, dp[m][n] contains the optimal solution

## Pseudocode (canonical)
```pseudo
function editDistance(source, target):
    m = source.length
    n = target.length
    
    # Create DP table
    dp = new table[0...m][0...n]
    
    # Base cases: transforming to/from empty string
    for i = 0 to m:
        dp[i][0] = i  # Delete i characters from source
    
    for j = 0 to n:
        dp[0][j] = j  # Insert j characters from target
    
    # Fill the DP table
    for i = 1 to m:
        for j = 1 to n:
            if source[i-1] == target[j-1]:
                dp[i][j] = dp[i-1][j-1]  # No operation needed
            else:
                dp[i][j] = 1 + min(
                    dp[i-1][j],    # Deletion
                    dp[i][j-1],    # Insertion
                    dp[i-1][j-1]   # Substitution
                )
    
    return dp[m][n]

# Backtracking to find actual operations
function getEditOperations(source, target, dp):
    i = source.length
    j = target.length
    operations = []
    
    while i > 0 or j > 0:
        if i > 0 and j > 0 and source[i-1] == target[j-1]:
            # Characters match, no operation
            i--
            j--
        else if i > 0 and j > 0 and dp[i][j] == dp[i-1][j-1] + 1:
            # Substitution
            operations.add("Replace " + source[i-1] + " with " + target[j-1])
            i--
            j--
        else if i > 0 and dp[i][j] == dp[i-1][j] + 1:
            # Deletion
            operations.add("Delete " + source[i-1])
            i--
        else:
            # Insertion
            operations.add("Insert " + target[j-1])
            j--
    
    return reverse(operations)  # Operations are in reverse order
```

## Implementation Notes
- Space optimization: Only need to store two rows of the DP table at a time
- For weighted operations, modify the cost of each operation type
- Can be extended to allow transpositions (swapping adjacent characters)
- For backtracking, store operation decisions or reconstruct from DP table
- Consider using memoization for recursive approach
- For very long strings, consider using approximate algorithms or heuristics
- Can be generalized to multiple strings or sequences

## Edge-case Checklist
- Empty strings: Handle base cases correctly
- Equal strings: Edit distance should be 0
- One empty string: Edit distance is the length of the other string
- No common characters: Maximum possible edit distance
- Case sensitivity: Define character equality appropriately
- Unicode/multi-byte characters: Ensure proper character handling

## Examples
- Minimal example:
  - Source: "kitten"
  - Target: "sitting"
  - DP table:
    ```
        |   | "" | s  | i  | t  | t  | i  | n  | g  |
        |---|---|----|----|----|----|----|----|----| 
        | "" | 0 | 1  | 2  | 3  | 4  | 5  | 6  | 7  |
        | k  | 1 | 1  | 2  | 3  | 4  | 5  | 6  | 7  |
        | i  | 2 | 2  | 1  | 2  | 3  | 4  | 5  | 6  |
        | t  | 3 | 3  | 2  | 1  | 2  | 3  | 4  | 5  |
        | t  | 4 | 4  | 3  | 2  | 1  | 2  | 3  | 4  |
        | e  | 5 | 5  | 4  | 3  | 2  | 2  | 3  | 4  |
        | n  | 6 | 6  | 5  | 4  | 3  | 3  | 2  | 3  |
    ```
  - Edit distance: 3
  - Operations: Substitute 'k' with 's', Substitute 'e' with 'i', Insert 'g'
  
- Edge case example:
  - Source: "abc"
  - Target: ""
  - Edit distance: 3 (delete all characters)

## Variants / Alternatives
- Weighted Edit Distance: Different costs for different operations
- Damerau-Levenshtein Distance: Allows transpositions
- Longest Common Subsequence: Related problem (edit distance with only insertions and deletions)
- Hamming Distance: For strings of equal length (only substitutions)
- Jaro-Winkler Distance: For name matching and record linkage
- Alternatives:
  - Approximate string matching algorithms
  - N-gram based similarity measures
  - Phonetic algorithms (Soundex, Metaphone)

## References
- Levenshtein, Vladimir I. "Binary codes capable of correcting deletions, insertions, and reversals." (1966)
- Wagner, Robert A., and Michael J. Fischer. "The string-to-string correction problem." (1974)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Dan Gusfield: "Algorithms on Strings, Trees, and Sequences"
- LeetCode Problem #72: "Edit Distance"
