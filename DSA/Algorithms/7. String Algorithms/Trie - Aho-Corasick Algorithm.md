# Aho-Corasick Algorithm

## Name
- Category: String Matching
- Summary: Linear-time algorithm for simultaneously finding all occurrences of multiple patterns in a text using an automaton built from a trie with failure links.

## Preconditions / Assumptions
- Set of pattern strings to search for
- Text to search within
- Characters from a finite alphabet
- Need to find all occurrences of all patterns
- Memory sufficient for automaton construction

## Properties
- Deterministic
- Complete (finds all occurrences)
- Linear time complexity in text length and pattern lengths
- Preprocessing-based approach
- Automaton construction with failure links
- No backtracking in text

## Complexity (per variant)
| Variant | Preprocessing | Search | Space | Notes |
|---|---|---|---|---|
| Standard | O(m) | O(n + z) | O(m) | m: total pattern length, n: text length, z: matches |
| With output links | O(m) | O(n + z) | O(m) | Faster output generation |
| Compressed | O(m) | O(n + z) | O(m) | Reduced memory usage |
| Bit-parallel | O(m) | O(n) | O(m) | For small patterns |

## When to Use / When to Avoid
- Use if:
  - Need to find multiple patterns simultaneously
  - Text is large and patterns are reused
  - All occurrences of all patterns needed
  - Preprocessing time is acceptable
- Avoid if:
  - Single pattern search (use KMP or Boyer-Moore)
  - Very large pattern set with memory constraints
  - Need approximate matching
  - Patterns change frequently (preprocessing cost)

## Correctness (Proof Sketch)
- Trie construction: Each pattern is represented as a path from root
- Failure links: Connect to longest proper suffix that is also a prefix of another pattern
- Dictionary links: Connect to patterns that are suffixes of current state
- Invariant: Current state represents longest suffix of processed text that matches a prefix of any pattern
- Termination: All text characters are processed exactly once
- Completeness: All matches are found due to failure and dictionary links

## Pseudocode (canonical)
```pseudo
function buildAhoCorasick(patterns):
    # Build trie
    root = new TrieNode()
    for each pattern in patterns:
        node = root
        for each char c in pattern:
            if node.children[c] doesn't exist:
                node.children[c] = new TrieNode()
            node = node.children[c]
        node.isEnd = true
        node.patterns.add(pattern)
    
    # Build failure links using BFS
    queue = new Queue()
    
    # For root's children, failure link to root
    for each child in root.children:
        child.fail = root
        queue.enqueue(child)
    
    while queue is not empty:
        current = queue.dequeue()
        
        for each char c and child node in current.children:
            queue.enqueue(child)
            
            # Find failure link
            failNode = current.fail
            while failNode != null and failNode.children[c] doesn't exist:
                failNode = failNode.fail
            
            child.fail = failNode ? failNode.children[c] : root
            
            # Add output links
            child.patterns.addAll(child.fail.patterns)
    
    return root

function search(text, root):
    current = root
    results = []
    
    for i = 0 to text.length - 1:
        char = text[i]
        
        # Follow failure links until we find a node with this character
        while current != root and current.children[char] doesn't exist:
            current = current.fail
        
        # If found transition, follow it
        if current.children[char] exists:
            current = current.children[char]
        
        # Check for matches at current node
        for each pattern in current.patterns:
            results.add((i - pattern.length + 1, pattern))
    
    return results
```

## Implementation Notes
- Use a queue for breadth-first construction of failure links
- Consider using a map or array for children based on alphabet size
- For large alphabets, use a map instead of an array for transitions
- Implement output links for faster reporting of matches
- Consider using compressed or bit-parallel representation for memory efficiency
- Can be extended to handle case-insensitive matching
- For very large pattern sets, consider using a compressed trie

## Edge-case Checklist
- Empty text: No matches
- Empty pattern set: No matches
- Single character patterns: Handled correctly
- Overlapping patterns: All matches reported
- Patterns that are substrings of other patterns: All matches reported
- Patterns with common prefixes: Trie handles efficiently
- Large alphabet: Consider sparse representation of transitions

## Examples
- Minimal example:
  - Patterns: ["he", "she", "his", "hers"]
  - Text: "ushers"
  - Matches: "she" at position 1, "he" at position 2, "hers" at position 2
  
- Trie visualization:
  ```
       root
      /  |  \
     h   s   u
    / \   \
   e   i   h
  /     \   \
 #(he)   s   e
         |    \
         #(his) #(she)
  ```

## Variants / Alternatives
- Commentz-Walter algorithm: Combines Aho-Corasick with Boyer-Moore
- AC-BM: Hybrid of Aho-Corasick and Boyer-Moore
- Compressed Aho-Corasick: Reduces memory usage
- Bit-parallel Aho-Corasick: For small patterns
- Alternatives:
  - Multiple KMP searches (less efficient)
  - Rabin-Karp with multiple patterns
  - Suffix automaton
  - Wu-Manber algorithm

## References
- Aho, Alfred V., and Margaret J. Corasick. "Efficient string matching: an aid to bibliographic search." Communications of the ACM 18.6 (1975): 333-340
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Dan Gusfield: "Algorithms on Strings, Trees, and Sequences"
- CP-Algorithms: "Aho-Corasick algorithm" (https://cp-algorithms.com/string/aho_corasick.html)
