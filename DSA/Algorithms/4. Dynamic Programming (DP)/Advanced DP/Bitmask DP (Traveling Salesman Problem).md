# Bitmask DP (Traveling Salesman Problem)

## Name
- Category: Dynamic Programming, Bitmask
- Summary: Algorithm to solve the Traveling Salesman Problem (finding the shortest possible route that visits each city exactly once and returns to the origin) using dynamic programming with bitmasks to represent visited cities.

## Preconditions / Assumptions
- Complete weighted graph (distances between all pairs of cities are known)
- Distances can be asymmetric (cost from A to B may differ from B to A)
- Goal is to find the minimum cost Hamiltonian cycle
- Number of cities is small enough for bitmask representation (typically n ≤ 20-25)

## Properties
- Deterministic
- Complete (finds optimal solution)
- Optimal substructure (solution built from solutions to overlapping subproblems)
- Exponential time complexity, but much better than naive approach
- Uses bitmasks to efficiently represent subsets of visited cities

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard Bitmask DP | O(n²·2ⁿ) | O(n·2ⁿ) | n is number of cities |
| Held-Karp | O(n²·2ⁿ) | O(n·2ⁿ) | Classical DP approach |
| Memory-optimized | O(n²·2ⁿ) | O(2ⁿ) | With state compression |
| Branch and Bound | O(n²·2ⁿ) | O(n·2ⁿ) | With pruning (best case better) |

## When to Use / When to Avoid
- Use if:
  - Need exact solution to TSP
  - Number of cities is small (n ≤ 20-25)
  - Complete optimal solution is required
  - Problem can be modeled as finding minimum Hamiltonian cycle
- Avoid if:
  - Number of cities is large (n > 25)
  - Approximate solution is acceptable (use heuristics)
  - Memory constraints are severe
  - Graph is not complete (though can be adapted)

## Correctness (Proof Sketch)
- Base case: dp[i][{i}] = 0 (cost to visit only the starting city is 0)
- Recurrence relation: dp[i][mask] = min(dp[j][mask\{i}] + dist[j][i]) for all j in mask, j ≠ i
  - Represents the minimum cost to visit all cities in mask and end at city i
- Optimal substructure: Solution for a set of cities ending at city i uses solutions for smaller sets
- Final answer: min(dp[i][all_cities] + dist[i][0]) for all i ≠ 0
  - Cost to visit all cities ending at i, plus cost to return to starting city 0
- Termination: After computing dp[i][mask] for all valid i and mask combinations

## Pseudocode (canonical)
```pseudo
function tsp(dist):
    n = number of cities
    # Create DP table: dp[i][mask] = min cost to visit cities in mask and end at city i
    dp = new table[0...n-1][0...2^n-1], initialized with infinity
    
    # Base case: cost to visit only the starting city (0) is 0
    dp[0][1] = 0  # Bitmask 1 represents only city 0 visited
    
    # Iterate through all possible subsets of cities
    for mask = 1 to 2^n - 1:
        # Skip if starting city is not in the mask
        if (mask & 1) == 0:
            continue
        
        # Iterate through all possible ending cities
        for end = 0 to n-1:
            # Skip if ending city is not in the mask
            if (mask & (1 << end)) == 0:
                continue
            
            # Skip base case
            if mask == (1 << end):
                continue
            
            # Try all possible cities before the ending city
            mask_without_end = mask ^ (1 << end)  # Remove end from mask
            
            for prev = 0 to n-1:
                # Skip if prev city is not in the mask or is the same as end
                if (mask_without_end & (1 << prev)) == 0 or prev == end:
                    continue
                
                dp[end][mask] = min(dp[end][mask], dp[prev][mask_without_end] + dist[prev][end])
    
    # Find minimum cost to return to starting city
    all_visited = (1 << n) - 1  # Bitmask with all cities visited
    result = infinity
    
    for end = 1 to n-1:  # Try all cities as the last before returning to 0
        if dp[end][all_visited] < infinity:
            result = min(result, dp[end][all_visited] + dist[end][0])
    
    return result

# Path reconstruction
function tspWithPath(dist):
    n = number of cities
    dp = new table[0...n-1][0...2^n-1], initialized with infinity
    parent = new table[0...n-1][0...2^n-1], initialized with -1
    
    dp[0][1] = 0
    
    for mask = 1 to 2^n - 1:
        if (mask & 1) == 0:
            continue
        
        for end = 0 to n-1:
            if (mask & (1 << end)) == 0:
                continue
            
            if mask == (1 << end):
                continue
            
            mask_without_end = mask ^ (1 << end)
            
            for prev = 0 to n-1:
                if (mask_without_end & (1 << prev)) == 0 or prev == end:
                    continue
                
                if dp[prev][mask_without_end] + dist[prev][end] < dp[end][mask]:
                    dp[end][mask] = dp[prev][mask_without_end] + dist[prev][end]
                    parent[end][mask] = prev
    
    all_visited = (1 << n) - 1
    result = infinity
    last_city = -1
    
    for end = 1 to n-1:
        if dp[end][all_visited] + dist[end][0] < result:
            result = dp[end][all_visited] + dist[end][0]
            last_city = end
    
    # Reconstruct the path
    path = []
    path.add(0)  # Start at city 0
    
    current_city = last_city
    current_mask = all_visited
    
    while current_city != 0:
        path.add(current_city)
        temp = current_city
        current_city = parent[current_city][current_mask]
        current_mask ^= (1 << temp)  # Remove current city from mask
    
    path.add(0)  # Return to city 0
    reverse(path)  # Path was constructed backwards
    
    return result, path
```

## Implementation Notes
- Use bit manipulation for efficient operations on masks
- Consider using memoization instead of bottom-up DP for cleaner code
- For path reconstruction, store the parent/previous city for each state
- Be careful with bitmask indexing and representation
- For larger instances, consider branch and bound or approximation algorithms
- Can be extended to handle constraints like time windows or precedence
- Watch for integer overflow in distance calculations
- Consider using a sparse table if many state combinations are invalid

## Edge-case Checklist
- Single city: Cost is 0 (no travel needed)
- Two cities: Cost is sum of distances between them (round trip)
- Disconnected graph: No valid solution (handle appropriately)
- Negative edge weights: Algorithm works, but may not represent a valid TSP
- Asymmetric distances: Ensure algorithm handles correctly
- Large number of cities: Watch for bitmask overflow (use long integers)

## Examples
- Minimal example:
  - 4 cities with distance matrix:
    ```
    [0, 10, 15, 20]
    [10, 0, 35, 25]
    [15, 35, 0, 30]
    [20, 25, 30, 0]
    ```
  - DP table (partial):
    - dp[0][1] = 0 (base case)
    - dp[1][3] = 10 (visit 0→1)
    - dp[2][5] = 15 (visit 0→2)
    - dp[3][9] = 20 (visit 0→3)
    - ...
    - dp[1][15] = 80 (visit all, end at 1)
    - dp[2][15] = 65 (visit all, end at 2)
    - dp[3][15] = 75 (visit all, end at 3)
  - Optimal tour: 0→2→3→1→0
  - Minimum cost: 65 + 10 = 75
  
- Visualization:
  ```
  0 --- 10 --- 1
  |             |
  |             |
  15            25
  |             |
  |             |
  2 --- 30 --- 3
  ```

## Variants / Alternatives
- Asymmetric TSP: Different costs for each direction
- Metric TSP: Distances satisfy triangle inequality
- Multiple TSP: Multiple salesmen share the cities
- TSP with Time Windows: Cities must be visited within specific time ranges
- Alternatives:
  - Branch and Bound: Exact algorithm with pruning
  - Approximation algorithms: Christofides algorithm (1.5-approximation for metric TSP)
  - Heuristics: Nearest neighbor, 2-opt, Lin-Kernighan
  - Genetic algorithms and other metaheuristics

## References
- Held, M., Karp, R.M. "A Dynamic Programming Approach to Sequencing Problems" (1962)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Skiena, Steven: "The Algorithm Design Manual", Chapter on NP-Complete Problems
- Applegate, Bixby, Chvátal, Cook: "The Traveling Salesman Problem: A Computational Study"
- LeetCode Problem #943: "Find the Shortest Superstring" (similar technique)
