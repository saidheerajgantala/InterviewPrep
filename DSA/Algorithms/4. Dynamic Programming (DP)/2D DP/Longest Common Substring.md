# Longest Common Substring

## Name
- Category: Dynamic Programming, String Algorithms
- Summary: Algorithm to find the longest substring that appears in two given strings, using a dynamic programming approach to efficiently track common substrings of increasing lengths.

## Preconditions / Assumptions
- Input consists of two strings
- Characters can be compared for equality in constant time
- Substring must be contiguous (unlike subsequence)
- Case sensitivity is defined by the problem (typically case-sensitive)
- Strings fit in memory
- Empty strings are valid inputs

## Properties
- Deterministic
- Complete (finds the optimal solution)
- Time complexity: O(m*n)
- Space complexity: O(m*n), can be optimized to O(min(m,n))
- Bottom-up dynamic programming approach
- Can be extended to find all longest common substrings
- Can be modified to work with more than two strings

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard DP | O(m*n) | O(m*n) | m, n are string lengths |
| Space-optimized | O(m*n) | O(min(m,n)) | Using two rows |
| Suffix tree | O(m+n) | O(m+n) | More complex implementation |
| Suffix array | O((m+n) log(m+n)) | O(m+n) | Better space efficiency |
| Multiple strings | O(n*k*l) | O(n*k) | k strings of average length l |

## When to Use / When to Avoid
- Use if:
  - Need to find exact matching substrings
  - Strings are not extremely long
  - Memory constraints are reasonable
  - Exact solution is required
  - Problem involves text similarity or plagiarism detection
- Avoid if:
  - Need to find common subsequences (use LCS instead)
  - Strings are extremely long (consider suffix-based approaches)
  - Approximate matching is sufficient
  - Memory is severely constrained
  - Need to find all common substrings efficiently

## Correctness (Proof Sketch)
- Define dp[i][j] as the length of the longest common suffix ending at positions i and j
- Base case: dp[i][0] = dp[0][j] = 0 (empty string has no common substring)
- Recurrence relation:
  - If s1[i-1] == s2[j-1], then dp[i][j] = dp[i-1][j-1] + 1
  - Otherwise, dp[i][j] = 0 (reset, as substrings must be contiguous)
- The longest common substring length is the maximum value in dp table
- To recover the substring, track the position of the maximum value
- The algorithm correctly builds all possible common suffixes of increasing lengths
- Time complexity is O(m*n) as we fill an m×n table with constant-time operations

## Pseudocode (canonical)
```pseudo
function longestCommonSubstring(s1, s2):
    m = s1.length
    n = s2.length
    
    # Initialize DP table
    dp = new table of size (m+1) × (n+1), initialized with 0
    
    # Variables to track the longest substring
    max_length = 0
    end_pos = 0
    
    # Fill the DP table
    for i = 1 to m:
        for j = 1 to n:
            if s1[i-1] == s2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
                
                if dp[i][j] > max_length:
                    max_length = dp[i][j]
                    end_pos = i
            else:
                dp[i][j] = 0
    
    # Extract the longest common substring
    start_pos = end_pos - max_length
    longest_substring = s1.substring(start_pos, end_pos)
    
    return longest_substring, max_length

# Space-optimized version
function longestCommonSubstringOptimized(s1, s2):
    m = s1.length
    n = s2.length
    
    # Ensure s1 is the shorter string for space optimization
    if m > n:
        swap(s1, s2)
        swap(m, n)
    
    # Initialize current and previous row
    prev_row = new array of size n+1, initialized with 0
    curr_row = new array of size n+1, initialized with 0
    
    # Variables to track the longest substring
    max_length = 0
    end_pos_s1 = 0
    
    # Fill the DP table using only two rows
    for i = 1 to m:
        for j = 1 to n:
            if s1[i-1] == s2[j-1]:
                curr_row[j] = prev_row[j-1] + 1
                
                if curr_row[j] > max_length:
                    max_length = curr_row[j]
                    end_pos_s1 = i
            else:
                curr_row[j] = 0
        
        # Swap rows for next iteration
        temp = prev_row
        prev_row = curr_row
        curr_row = temp
        
        # Reset current row
        for j = 0 to n:
            curr_row[j] = 0
    
    # Extract the longest common substring
    start_pos = end_pos_s1 - max_length
    longest_substring = s1.substring(start_pos, end_pos_s1)
    
    return longest_substring, max_length

# Find all longest common substrings
function allLongestCommonSubstrings(s1, s2):
    m = s1.length
    n = s2.length
    
    # Initialize DP table
    dp = new table of size (m+1) × (n+1), initialized with 0
    
    # Variable to track the maximum length
    max_length = 0
    
    # Fill the DP table
    for i = 1 to m:
        for j = 1 to n:
            if s1[i-1] == s2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
                max_length = max(max_length, dp[i][j])
            else:
                dp[i][j] = 0
    
    # Find all occurrences of the longest common substring
    result = []
    
    for i = 1 to m:
        for j = 1 to n:
            if dp[i][j] == max_length:
                end_pos = i
                start_pos = end_pos - max_length
                result.add(s1.substring(start_pos, end_pos))
    
    # Remove duplicates
    return distinct(result), max_length
```

## Implementation Notes
- Use 0-indexed strings but 1-indexed DP table for cleaner code
- For large strings, consider using a suffix tree or suffix array approach
- When memory is a concern, use the space-optimized version
- To find all occurrences, collect all positions with the maximum length
- Consider using hash tables for faster character comparisons
- For multiple strings, extend the approach with multiple dimensions
- Be careful with string indexing and off-by-one errors
- For very long strings, consider chunking or external memory algorithms
- When extracting the substring, ensure proper bounds checking
- Consider case sensitivity requirements of the problem

## Edge-case Checklist
- Empty strings: Return empty string with length 0
- Single character strings: Check if the character matches
- No common substring: Return empty string with length 0
- Identical strings: The string itself is the longest common substring
- Overlapping substrings: All should be found correctly
- Case sensitivity: Handle according to problem requirements
- Special characters: Ensure proper comparison
- Very long strings: Watch for memory limits
- Multiple longest substrings: Return any one or all as required

## Examples
- Minimal example:
  - String 1: "abcdef"
  - String 2: "bcdefg"
  - DP table (partial):
    ```
      | b c d e f g
    --+-------------
    a | 0 0 0 0 0 0
    b | 1 0 0 0 0 0
    c | 0 2 0 0 0 0
    d | 0 0 3 0 0 0
    e | 0 0 0 4 0 0
    f | 0 0 0 0 5 0
    ```
  - Longest common substring: "bcdef", length: 5
  
- Complex example:
  - String 1: "abacadabram"
  - String 2: "abracadabra"
  - Multiple common substrings of length 5: "abra", "acada"
  - Result: Either "abra" or "acada" (or both if finding all)

## Variants / Alternatives
- K-common substring: Find the longest substring common to k strings
- Approximate common substring: Allow a certain number of mismatches
- Weighted common substring: Characters have different weights
- Suffix tree approach: Build suffix tree and find deepest common nodes
- Suffix array approach: Use LCP array to find common substrings
- Rolling hash: Use Rabin-Karp algorithm for faster comparisons
- Alternatives:
  - Longest Common Subsequence: For non-contiguous characters
  - Edit Distance: For measuring string similarity
  - String alignment algorithms: For bioinformatics applications

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Gusfield, Dan: "Algorithms on Strings, Trees, and Sequences"
- Crochemore, Maxime and Rytter, Wojciech: "Jewels of Stringology"
- Sedgewick, Robert and Wayne, Kevin: "Algorithms, 4th Edition"
- Competitive Programming resources (e.g., CP-Algorithms)
