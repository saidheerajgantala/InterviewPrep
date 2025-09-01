# Longest Common Subsequence (LCS)

## Name
- Category: Dynamic Programming (2D)
- Summary: Find the longest subsequence common to two sequences, where elements need not be contiguous but must appear in the same relative order.

## Preconditions / Assumptions
- Two input sequences (strings, arrays, etc.)
- Elements can be compared for equality
- Subsequence order matters (elements must appear in same relative order)
- Need to find length and/or the actual subsequence

## Properties
- Deterministic
- Complete (finds optimal solution)
- Optimal substructure (solution built from solutions to overlapping subproblems)
- 2D dynamic programming approach
- Multiple optimal solutions may exist

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Basic DP | O(m*n) | O(m*n) | m, n are lengths of sequences |
| Space-optimized | O(m*n) | O(min(m,n)) | Only need two rows/columns |
| Hirschberg's | O(m*n) | O(min(m,n)) | Linear space for actual subsequence |
| Sparse sequences | O(r log r) | O(r) | r is matches between sequences |
| Bit-parallel | O(mn/w) | O(n) | w is word size (e.g., 32 or 64) |

## When to Use / When to Avoid
- Use if:
  - Need longest common pattern between sequences
  - Elements must maintain relative order
  - Sequences are of moderate size
  - Exact solution required
- Avoid if:
  - Very large sequences (consider specialized algorithms)
  - Need contiguous matching (use Longest Common Substring)
  - Approximate solution is sufficient
  - Memory is severely constrained

## Correctness (Proof Sketch)
- Base case: Empty sequence has LCS of length 0 with any sequence
- Recursive case:
  - If last characters match: 1 + LCS of sequences without last characters
  - If last characters don't match: max(LCS without last char of seq1, LCS without last char of seq2)
- Optimal substructure: Optimal solution contains optimal solutions to subproblems
- Overlapping subproblems: Same subproblems solved multiple times (memoized)
- Termination: Table filled in bottom-up manner, guaranteed to complete

## Pseudocode (canonical)
```pseudo
function LCS_length(X, Y):
    m = X.length
    n = Y.length
    
    # Create DP table
    dp = new table[0...m][0...n]
    
    # Fill table bottom-up
    for i = 0 to m:
        dp[i][0] = 0  # Empty Y gives LCS of 0
    for j = 0 to n:
        dp[0][j] = 0  # Empty X gives LCS of 0
    
    for i = 1 to m:
        for j = 1 to n:
            if X[i-1] == Y[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]

# To reconstruct the actual LCS
function reconstruct_LCS(X, Y, dp):
    i = X.length
    j = Y.length
    lcs = empty string
    
    while i > 0 and j > 0:
        if X[i-1] == Y[j-1]:
            lcs = X[i-1] + lcs
            i--
            j--
        else if dp[i-1][j] >= dp[i][j-1]:
            i--
        else:
            j--
    
    return lcs
```

## Implementation Notes
- Use space-optimized version for long sequences (only need two rows)
- For actual subsequence, either store backtracking info or reconstruct from DP table
- Can be extended to multiple sequences (becomes k-dimensional DP)
- Consider using bit-parallel algorithms for short sequences
- For sparse matches, use Hunt-Szymanski algorithm
- Watch for off-by-one errors due to 0-indexing vs 1-indexing

## Edge-case Checklist
- Empty sequences: LCS is empty
- One empty sequence: LCS is empty
- Identical sequences: LCS is the sequence itself
- No common elements: LCS is empty
- Single character sequences: LCS is either empty or the character
- Case sensitivity: Define equality appropriately for strings
- Unicode/multi-byte characters: Handle properly in string comparisons

## Examples
- Minimal example:
  - X = "ABCBDAB"
  - Y = "BDCABA"
  - LCS = "BCBA" (length 4)
  
- DP table for example:
  ```
    |   | Ø | B | D | C | A | B | A |
    |---|---|---|---|---|---|---|---|
    | Ø | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
    | A | 0 | 0 | 0 | 0 | 1 | 1 | 1 |
    | B | 0 | 1 | 1 | 1 | 1 | 2 | 2 |
    | C | 0 | 1 | 1 | 2 | 2 | 2 | 2 |
    | B | 0 | 1 | 1 | 2 | 2 | 3 | 3 |
    | D | 0 | 1 | 2 | 2 | 2 | 3 | 3 |
    | A | 0 | 1 | 2 | 2 | 3 | 3 | 4 |
    | B | 0 | 1 | 2 | 2 | 3 | 4 | 4 |
  ```

## Variants / Alternatives
- Longest Common Substring: Requires contiguous matching
- Shortest Common Supersequence: Sequence containing both sequences
- Longest Increasing Subsequence: Special case of LCS
- Longest Palindromic Subsequence: LCS of string and its reverse
- Diff algorithms: Based on LCS for text comparison
- Edit Distance: Related problem (can be derived from LCS)
- Alternatives:
  - Hunt-Szymanski algorithm: For sparse matches
  - Hirschberg's algorithm: Linear space reconstruction
  - Myers' algorithm: Bit-parallel approach

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 15.4
- Dan Gusfield: "Algorithms on Strings, Trees, and Sequences"
- Sedgewick, Wayne: "Algorithms, 4th Edition"
- Hirschberg, D.S. "A linear space algorithm for computing maximal common subsequences" (1975)
