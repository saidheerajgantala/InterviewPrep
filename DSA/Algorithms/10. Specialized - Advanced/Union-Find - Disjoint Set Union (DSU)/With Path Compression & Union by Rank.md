# Union-Find (Disjoint Set Union) with Path Compression & Union by Rank

## Name
- Category: Data Structure
- Summary: Efficient data structure for maintaining disjoint sets, supporting fast union operations and membership queries with near-constant amortized time complexity.

## Preconditions / Assumptions
- Elements can be represented as integers or mapped to integers
- Initially, each element is in its own singleton set
- Need to track which elements belong to the same set
- Need to merge sets efficiently
- Need to query if two elements belong to the same set

## Properties
- Near-constant time operations (amortized)
- Path compression optimization flattens trees during find operations
- Union by rank/size optimization keeps trees balanced
- Maintains disjoint (non-overlapping) sets
- Supports dynamic merging of sets

## Complexity (per variant)
| Variant | Union | Find | Space | Notes |
|---|---|---|---|---|
| Basic | O(n) | O(n) | O(n) | Simple parent array |
| Path Compression only | O(log n) | O(log n) | O(n) | Flattens find paths |
| Union by Rank only | O(log n) | O(log n) | O(n) | Balances trees |
| Path Compression + Union by Rank | O(α(n)) | O(α(n)) | O(n) | α is inverse Ackermann function (practically constant) |

## When to Use / When to Avoid
- Use if:
  - Need to maintain disjoint sets with frequent unions
  - Need to check if elements are in the same set
  - Working with undirected graphs (connected components, cycle detection)
  - Implementing Kruskal's algorithm for MST
- Avoid if:
  - Need to split sets (not supported)
  - Need to delete elements (not directly supported)
  - Need to enumerate all elements in a set efficiently
  - Need complex set operations (intersection, difference)

## Correctness (Proof Sketch)
- Invariant: Each set is represented as a tree with a unique root
- Path compression: Attaches nodes directly to root during find, flattening the tree
- Union by rank: Attaches smaller tree to larger tree, keeping height logarithmic
- Amortized analysis: Sequence of m operations on n elements takes O(m α(n)) time
- Inverse Ackermann function α(n) grows extremely slowly (< 5 for any practical n)
- Termination: Each find operation eventually reaches a root node

## Pseudocode (canonical)
```pseudo
# Initialize n disjoint sets
function initialize(n):
    parent = new array of size n
    rank = new array of size n
    
    for i = 0 to n-1:
        parent[i] = i  # Each element is its own parent initially
        rank[i] = 0    # Initial rank is 0
    
    return (parent, rank)

# Find the representative (root) of the set containing x
function find(parent, x):
    if parent[x] != x:
        parent[x] = find(parent, parent[x])  # Path compression
    return parent[x]

# Union of sets containing x and y
function union(parent, rank, x, y):
    root_x = find(parent, x)
    root_y = find(parent, y)
    
    if root_x == root_y:
        return  # Already in the same set
    
    # Union by rank
    if rank[root_x] < rank[root_y]:
        parent[root_x] = root_y
    else if rank[root_x] > rank[root_y]:
        parent[root_y] = root_x
    else:
        parent[root_y] = root_x
        rank[root_x] += 1

# Check if two elements are in the same set
function sameSet(parent, x, y):
    return find(parent, x) == find(parent, y)
```

## Implementation Notes
- Can be implemented with arrays for parent and rank
- Path compression can be implemented recursively or iteratively
- Union by size (tracking set size instead of rank) is a valid alternative
- For non-integer elements, use a map to assign unique IDs
- Can be extended to track additional information about sets
- Consider using union by size for applications needing set sizes
- Iterative path compression can avoid stack overflow for deep trees
- Thread-safe implementations require careful synchronization

## Edge-case Checklist
- Single element: Trivially handled
- All elements in same set: Correctly identified by find
- Disconnected elements: Each in its own set initially
- Repeated unions of same elements: No effect after first union
- Large number of elements: Works efficiently due to near-constant operations
- Deep trees before path compression: Automatically flattened during traversal

## Examples
- Minimal example:
  - Operations: initialize(5), union(0,1), union(2,3), union(0,4)
  - Result: {0,1,4} and {2,3} form two disjoint sets
  - Checking: sameSet(0,4) returns true, sameSet(1,3) returns false
  
- Connected components example:
  - Graph edges: [(0,1), (1,2), (3,4), (5,6), (6,7)]
  - After processing edges: {0,1,2}, {3,4}, {5,6,7} form three components
  - Query: sameSet(0,2) returns true, sameSet(2,3) returns false

## Variants / Alternatives
- Union by size: Track size of sets instead of rank
- Path splitting: Alternative path compression technique
- Path halving: Another path compression variant
- Weighted quick-union: Similar to union by rank/size
- Persistent DSU: Maintains history of operations
- Alternatives:
  - Adjacency list for connected components (less efficient)
  - Hash map based approaches (more flexible but slower)
  - Tree-based set representations (more operations but slower union)

## References
- Tarjan, R.E. and van Leeuwen, J. "Worst-case Analysis of Set Union Algorithms" (1984)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 21
- Sedgewick, Wayne: "Algorithms, 4th Edition", Section 1.5
- Galil, Z. and Italiano, G.F. "Data Structures and Algorithms for Disjoint Set Union Problems"
