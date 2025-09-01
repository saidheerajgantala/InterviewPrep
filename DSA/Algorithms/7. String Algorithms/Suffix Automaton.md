# Suffix Automaton

## Name
- Category: String Algorithms, Automata
- Summary: Linear-time algorithm that constructs a minimal deterministic finite automaton recognizing all suffixes of a string, enabling efficient string processing operations like pattern matching, substring counting, and more.

## Preconditions / Assumptions
- Input is a string or sequence of characters
- Characters can be compared for equality in constant time
- The string fits in memory
- Empty string is considered a valid input
- Alphabet size is reasonable (typically constant or logarithmic in input size)

## Properties
- Deterministic
- Complete (recognizes all suffixes)
- Linear time complexity for construction: O(n)
- Linear space complexity: O(n)
- Minimal (has the minimum possible number of states)
- Enables efficient string operations like pattern matching, substring counting, etc.
- More space-efficient than suffix trees for some applications
- Can be built online (incrementally)

## Complexity (per variant)
| Variant | Construction Time | Space | Query Time | Notes |
|---|---|---|---|---|
| Standard | O(n) | O(n) | O(m) | n = string length, m = pattern length |
| With links | O(n) | O(n) | O(m) | Includes suffix links |
| Extended | O(n) | O(n) | O(m) | With additional information |
| Persistent | O(n log n) | O(n log n) | O(m) | For version control |

## When to Use / When to Avoid
- Use if:
  - Need to perform multiple string operations efficiently
  - Need a minimal automaton for suffix recognition
  - Space efficiency is important
  - Online construction is required
  - Need to count distinct substrings
  - Need to find the lexicographically kth substring
- Avoid if:
  - Simplicity is more important than efficiency
  - Memory is severely constrained
  - Only need to perform a single string operation
  - Suffix tree or suffix array would be more appropriate
  - String is very short (naive approaches may be faster in practice)

## Correctness (Proof Sketch)
- The suffix automaton is a directed acyclic graph (DAG) where:
  - Each state corresponds to an equivalence class of substrings
  - Two substrings are equivalent if they have the same set of following suffixes
  - Each state has a len value representing the longest substring in its class
  - Suffix links connect states to their longest proper suffix's state
- Construction maintains the invariant that the automaton is minimal and deterministic
- Each state represents a right end-point set (endpos) of substrings
- The number of states is at most 2n-1 (where n is the string length)
- The number of transitions is at most 3n-4 (for binary alphabet)
- Time complexity is O(n) because each character is processed once and each state creates at most |Σ| transitions

## Pseudocode (canonical)
```pseudo
# Suffix Automaton Construction
function buildSuffixAutomaton(s):
    # Initialize automaton
    automaton = new SuffixAutomaton()
    automaton.addState(0, -1)  # Add initial state
    
    last = 0  # Pointer to the last state
    
    for i = 0 to s.length-1:
        c = s[i]
        curr = automaton.addState(i + 1, -1)  # Create new state
        
        # Add transitions from existing states
        p = last
        while p != -1 and automaton.transitions[p][c] == null:
            automaton.transitions[p][c] = curr
            p = automaton.link[p]
        
        if p == -1:
            # No suffix link needed
            automaton.link[curr] = 0
        else:
            q = automaton.transitions[p][c]
            if automaton.len[p] + 1 == automaton.len[q]:
                # Direct suffix link
                automaton.link[curr] = q
            else:
                # Need to clone state q
                clone = automaton.addState(automaton.len[p] + 1, -1)
                
                # Copy transitions from q to clone
                for each character d in alphabet:
                    if automaton.transitions[q][d] != null:
                        automaton.transitions[clone][d] = automaton.transitions[q][d]
                
                # Set suffix link for clone
                automaton.link[clone] = automaton.link[q]
                
                # Update suffix links
                automaton.link[q] = clone
                automaton.link[curr] = clone
                
                # Redirect transitions to clone
                while p != -1 and automaton.transitions[p][c] == q:
                    automaton.transitions[p][c] = clone
                    p = automaton.link[p]
        
        last = curr
    
    return automaton

# Pattern matching using Suffix Automaton
function search(automaton, pattern):
    state = 0  # Start from initial state
    
    # Follow transitions according to pattern characters
    for i = 0 to pattern.length-1:
        c = pattern[i]
        if automaton.transitions[state][c] == null:
            return false  # Pattern not found
        state = automaton.transitions[state][c]
    
    return true  # Pattern is a substring

# Count number of distinct substrings
function countDistinctSubstrings(automaton):
    # Dynamic programming approach
    dp = new array of size automaton.size, initialized with 0
    
    function dfs(state):
        if dp[state] != 0:
            return dp[state]
        
        # Each state represents at least one substring
        dp[state] = 1
        
        for each character c in alphabet:
            if automaton.transitions[state][c] != null:
                dp[state] += dfs(automaton.transitions[state][c])
        
        return dp[state]
    
    # Count all substrings starting from initial state
    # Subtract 1 for empty string
    return dfs(0) - 1
```

## Implementation Notes
- Use adjacency lists or maps for transitions to save space
- Consider using a more compact representation for small alphabets
- For large alphabets, use hash maps for transitions
- Implement suffix links carefully, they're crucial for correctness
- Be careful with state indexing and initialization
- Consider using a class or struct to represent states
- For memory efficiency, don't store explicit len values for all states
- Implement specialized algorithms for common operations like substring counting
- For very long strings, consider chunking or external memory algorithms
- Consider using persistent data structures for version control

## Edge-case Checklist
- Empty string: Create only the initial state
- Single character string: Create two states (initial and one for the character)
- String with all identical characters: Creates n+1 states
- Very long patterns: May require special handling
- Patterns not in the string: Should return false
- Case sensitivity: Define character comparison appropriately
- Special characters: Ensure proper handling in transitions
- Large alphabet: Consider using sparse representation for transitions

## Examples
- Minimal example:
  - String: "abcbc"
  - States:
    - 0: Initial state (len=0, link=-1)
    - 1: "a" (len=1, link=0)
    - 2: "ab" (len=2, link=0)
    - 3: "abc" (len=3, link=0)
    - 4: "abcb" (len=4, link=5)
    - 5: "bc" (len=2, link=0)
    - 6: "abcbc" (len=5, link=7)
    - 7: "bcbc" (len=4, link=8)
    - 8: "cbc" (len=3, link=9)
    - 9: "c" (len=1, link=0)
  - Transitions:
    - 0 --a--> 1, 0 --b--> 0, 0 --c--> 9
    - 1 --b--> 2
    - 2 --c--> 3
    - 3 --b--> 4
    - 4 --c--> 6
    - 5 --c--> 3
    - 9 --b--> 5
    - etc.
  
- Pattern matching:
  - String: "abcbc"
  - Pattern: "bc"
  - Trace: state 0 --b--> state 0 --c--> state 9 (not a match)
  - Correct trace: Need to find all occurrences, which requires additional processing
  - Result: Found at positions 1 and 3

## Variants / Alternatives
- Extended Suffix Automaton: With additional information for more operations
- Persistent Suffix Automaton: For version control and historical queries
- Online construction: Building the automaton incrementally
- Compressed representation: Using less space with some performance tradeoff
- Alternatives:
  - Suffix Tree: More space-intensive but supports some operations more efficiently
  - Suffix Array: More space-efficient for some applications
  - Aho-Corasick Automaton: For multiple pattern matching
  - KMP algorithm: For simple pattern matching

## References
- Blumer, Anselm et al.: "The Smallest Automation Recognizing the Subwords of a Text" (1983)
- Crochemore, Maxime and Rytter, Wojciech: "Jewels of Stringology"
- Gusfield, Dan: "Algorithms on Strings, Trees, and Sequences"
- CP-Algorithms: "Suffix Automaton"
- Ivantsov, Alexey: "Suffix Automaton. Implementation and Applications"
