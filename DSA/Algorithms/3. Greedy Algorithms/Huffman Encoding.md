# Huffman Encoding

## Name
- Category: Greedy, Data Compression
- Summary: Lossless data compression algorithm that assigns variable-length codes to input characters based on their frequencies, with shorter codes for more common characters.

## Preconditions / Assumptions
- Input is a sequence of characters or symbols
- Frequency or probability of each character is known or can be calculated
- Characters are independent (no context-based probabilities)
- Prefix-free codes are required (no codeword is a prefix of another)

## Properties
- Deterministic
- Complete (finds optimal prefix-free encoding)
- Greedy approach (always merges least frequent nodes)
- Produces variable-length codes
- Generates optimal prefix codes (minimum expected codeword length)
- Encodes more frequent characters with shorter codes

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(n log n) | O(n) | n is number of unique characters |
| With sorted input | O(n log n) | O(n) | Dominated by tree construction |
| With priority queue | O(n log n) | O(n) | For efficient node selection |
| Adaptive | O(n log n) | O(n) | Updates frequencies during compression |
| Canonical | O(n log n) | O(n) | Standardized code representation |

## When to Use / When to Avoid
- Use if:
  - Lossless compression is required
  - Character frequencies vary significantly
  - Static encoding is acceptable
  - Prefix-free codes are needed
  - Optimal compression ratio is desired
- Avoid if:
  - Character frequencies are uniform (no compression benefit)
  - Very fast decompression is required (consider fixed-length codes)
  - Context-based compression would be more effective
  - Extremely large alphabet size (consider other methods)

## Correctness (Proof Sketch)
- Greedy choice: Always merge two nodes with lowest frequencies
- Exchange argument: Any optimal solution must use the two least frequent symbols with the same codeword length
- Invariant: Internal nodes in the tree represent merged frequencies
- Sibling property: Huffman tree has the sibling property (nodes can be paired)
- Optimality: Can be proven to generate codes with minimum weighted path length
- Termination: After n-1 merges, a single tree is formed

## Pseudocode (canonical)
```pseudo
function huffmanEncoding(characters, frequencies):
    # Create leaf nodes for each character
    nodes = []
    for i = 0 to characters.length - 1:
        nodes.add(new Node(characters[i], frequencies[i]))
    
    # Build Huffman tree
    while nodes.size > 1:
        # Extract two nodes with lowest frequencies
        nodes.sort(by=frequency)  # Or use priority queue
        left = nodes.removeFirst()
        right = nodes.removeFirst()
        
        # Create parent node with these two as children
        parent = new Node(null, left.frequency + right.frequency)
        parent.left = left
        parent.right = right
        
        # Add the parent back to the list
        nodes.add(parent)
    
    # Root of the Huffman tree
    root = nodes[0]
    
    # Generate codes by traversing the tree
    codes = {}
    generateCodes(root, "", codes)
    
    return codes, root

function generateCodes(node, currentCode, codes):
    if node == null:
        return
    
    # If leaf node, store the code
    if node.character != null:
        codes[node.character] = currentCode
        return
    
    # Traverse left (add 0)
    generateCodes(node.left, currentCode + "0", codes)
    
    # Traverse right (add 1)
    generateCodes(node.right, currentCode + "1", codes)

function encode(text, codes):
    result = ""
    for each character in text:
        result += codes[character]
    return result

function decode(encodedText, root):
    result = ""
    current = root
    
    for each bit in encodedText:
        if bit == '0':
            current = current.left
        else:
            current = current.right
        
        # If leaf node, we found a character
        if current.character != null:
            result += current.character
            current = root
    
    return result
```

## Implementation Notes
- Use a priority queue (min-heap) for efficient extraction of minimum frequency nodes
- Store the Huffman tree or a decoding table for later decompression
- Consider using a canonical Huffman code for standardized representation
- For large alphabets, use an efficient tree representation
- Bit-level operations are typically needed for the actual compression
- Consider storing the frequency table or tree structure in the compressed output
- For adaptive Huffman coding, update frequencies and rebuild tree as needed
- Handle edge case of single character input

## Edge-case Checklist
- Empty input: Return empty result
- Single character input: Use code "0" for the character
- All characters with equal frequency: Results in a balanced tree
- Very skewed frequencies: Results in very unbalanced tree
- Large alphabet: May require significant memory for the tree
- Binary vs. text data: Consider appropriate bit handling

## Examples
- Minimal example:
  - Characters: ['a', 'b', 'c', 'd']
  - Frequencies: [5, 1, 6, 3]
  - Huffman tree construction:
    1. Initial nodes: (a:5), (b:1), (c:6), (d:3)
    2. Merge b & d: (a:5), (c:6), (bd:4)
    3. Merge a & bd: (c:6), (abd:9)
    4. Merge c & abd: (cabd:15) [root]
  - Resulting codes:
    - a: 10
    - b: 110
    - c: 0
    - d: 111
  - Encoded "abcd": "10110011"
  
- Visualization:
  ```
      (15)
     /    \
   (6)    (9)
   /     /   \
  c    (5)   (4)
       /     /  \
      a     b    d
  ```

## Variants / Alternatives
- Canonical Huffman coding: Standardized code assignment
- Adaptive Huffman coding: Updates frequencies during compression
- Extended Huffman coding: Groups symbols for better compression
- Length-limited Huffman coding: Restricts maximum code length
- Alternatives:
  - Arithmetic coding: Better compression but more complex
  - Shannon-Fano coding: Similar but not always optimal
  - Run-length encoding: For repeated characters
  - Dictionary-based methods (LZ77, LZ78, LZW)

## References
- Huffman, D.A. "A Method for the Construction of Minimum-Redundancy Codes" (1952)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 16.3
- Sedgewick, Wayne: "Algorithms, 4th Edition"
- Sayood, Khalid: "Introduction to Data Compression"
- Witten, Moffat, Bell: "Managing Gigabytes: Compressing and Indexing Documents and Images"
