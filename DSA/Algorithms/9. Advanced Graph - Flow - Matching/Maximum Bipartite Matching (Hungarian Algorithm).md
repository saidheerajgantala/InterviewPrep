# Hungarian Algorithm (Maximum Bipartite Matching)

## Name
- Category: Graph, Matching, Optimization
- Summary: Combinatorial optimization algorithm that solves the assignment problem in polynomial time by finding a perfect matching in a weighted bipartite graph.

## Preconditions / Assumptions
- Complete bipartite graph (every worker can perform every job)
- Number of workers equals number of jobs (can be padded with dummy entries)
- Edge weights represent cost/time/value of assignments
- Goal is to minimize total cost or maximize total value
- Weights are finite (no infinity values)

## Properties
- Deterministic
- Complete (finds optimal solution)
- Polynomial time complexity
- Based on the Kuhn-Munkres theorem
- Primal-dual approach using vertex labeling
- Guarantees optimal assignment

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Original Hungarian | O(n⁴) | O(n²) | n = number of workers/jobs |
| Kuhn-Munkres | O(n³) | O(n²) | Standard implementation |
| Jonker-Volgenant | O(n³) | O(n²) | Faster in practice |
| Using min-cost flow | O(n³) | O(n²) | Alternative formulation |

## When to Use / When to Avoid
- Use if:
  - Need optimal assignment of n workers to n jobs
  - Complete optimal matching required
  - Graph is weighted bipartite
  - Problem size is moderate (n ≤ 1000)
- Avoid if:
  - Graph is not bipartite
  - Very large problem instances (consider approximation)
  - Unequal number of workers and jobs (need modification)
  - Only need a greedy approximate solution

## Correctness (Proof Sketch)
- Kuhn-Munkres theorem: A feasible labeling with a perfect matching is optimal
- Invariant: Maintain feasible labeling throughout the algorithm
- Equality subgraph: Contains only edges (i,j) where label(i) + label(j) = cost(i,j)
- Algorithm iteratively improves labeling until perfect matching exists
- Termination: Each iteration either finds an augmenting path or improves labeling
- Optimality: Final matching is perfect and minimizes total cost

## Pseudocode (canonical)
```pseudo
function hungarianAlgorithm(costMatrix):
    n = costMatrix.rows  # Assume square matrix
    
    # Initialize labels for rows and columns
    rowLabel = new array of size n
    colLabel = new array of size n filled with 0
    
    for i = 0 to n-1:
        rowLabel[i] = min(costMatrix[i,:])  # Minimum value in row i
    
    # Initialize matching
    rowMatch = new array of size n filled with -1
    colMatch = new array of size n filled with -1
    
    for i = 0 to n-1:
        # Find augmenting path
        visited_rows = new boolean array of size n
        visited_cols = new boolean array of size n
        
        if findAugmentingPath(i, costMatrix, rowLabel, colLabel, rowMatch, colMatch, visited_rows, visited_cols):
            continue
        
        # Update labels if no augmenting path found
        delta = infinity
        for j = 0 to n-1:
            if visited_rows[j]:
                for k = 0 to n-1:
                    if !visited_cols[k]:
                        delta = min(delta, costMatrix[j][k] - rowLabel[j] - colLabel[k])
        
        # Update labels to create new equality edges
        for j = 0 to n-1:
            if visited_rows[j]:
                rowLabel[j] += delta
            if visited_cols[j]:
                colLabel[j] -= delta
        
        # Try again with updated labels
        i--
    
    # Calculate total cost
    totalCost = 0
    for i = 0 to n-1:
        totalCost += costMatrix[i][rowMatch[i]]
    
    return rowMatch, totalCost

function findAugmentingPath(start, costMatrix, rowLabel, colLabel, rowMatch, colMatch, visited_rows, visited_cols):
    visited_rows[start] = true
    
    for j = 0 to costMatrix.cols - 1:
        if costMatrix[start][j] == rowLabel[start] + colLabel[j] and !visited_cols[j]:
            visited_cols[j] = true
            
            if colMatch[j] == -1 or findAugmentingPath(colMatch[j], costMatrix, rowLabel, colLabel, rowMatch, colMatch, visited_rows, visited_cols):
                colMatch[j] = start
                rowMatch[start] = j
                return true
    
    return false
```

## Implementation Notes
- For maximization problems, negate all weights
- Handle rectangular matrices by padding with dummy rows/columns
- Use efficient data structures for tracking visited vertices
- Consider using priority queue for finding minimum delta
- Implement epsilon comparison for floating-point weights
- Initialize row labels with minimum values in each row for faster convergence
- Consider using the Jonker-Volgenant variant for better performance
- Can be implemented with adjacency lists for sparse graphs

## Edge-case Checklist
- Empty graph: Return empty matching
- Single worker/job: Trivial assignment
- Infeasible problem: Not possible with complete bipartite graph (always has solution)
- Negative weights: Algorithm works correctly
- Multiple optimal solutions: Returns one of them
- Degenerate cases: Algorithm handles correctly
- Numerical stability: Use appropriate epsilon for floating-point comparisons

## Examples
- Minimal example:
  - Cost matrix:
    ```
    [9, 2, 7]
    [6, 9, 5]
    [5, 3, 8]
    ```
  - Optimal assignment: (0,1), (1,2), (2,0)
  - Total cost: 2 + 5 + 5 = 12
  
- Maximization example:
  - Value matrix (negate for algorithm):
    ```
    [5, 9, 1]
    [10, 3, 2]
    [8, 7, 4]
    ```
  - Optimal assignment: (0,1), (1,0), (2,2)
  - Total value: 9 + 10 + 4 = 23

## Variants / Alternatives
- Jonker-Volgenant algorithm: Faster implementation
- Auction algorithm: Approximate solution with better practical performance
- Cost scaling algorithm: For integer weights
- Successive shortest path algorithm: Alternative formulation
- Alternatives:
  - Ford-Fulkerson for unweighted bipartite matching
  - Network flow algorithms for generalized assignment
  - Greedy approximation for large instances
  - Linear programming formulation

## References
- Kuhn, H.W. "The Hungarian Method for the assignment problem" (1955)
- Munkres, J. "Algorithms for the Assignment and Transportation Problems" (1957)
- Jonker, R. and Volgenant, A. "A Shortest Augmenting Path Algorithm for Dense and Sparse Linear Assignment Problems" (1987)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Papadimitriou, Steiglitz: "Combinatorial Optimization: Algorithms and Complexity"
