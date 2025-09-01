# Knapsack Problem

## Name
- Category: Dynamic Programming
- Summary: Maximize value of items in a capacity-constrained container where each item has a weight and value.

## Preconditions / Assumptions
- Non-negative weights and values
- Integer weights (for standard approach)
- Integer capacity (for standard approach)
- For 0/1: Each item can be used at most once
- For unbounded: Each item can be used any number of times

## Properties
- Deterministic
- Complete (finds optimal solution)
- NP-complete (no known polynomial solution in terms of number of items)
- Pseudo-polynomial time (polynomial in terms of capacity)

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| 0/1 Knapsack | O(nW) | O(nW) | O(nW) | O(nW) | n = items, W = capacity |
| 0/1 Space-optimized | O(nW) | O(nW) | O(nW) | O(W) | Only previous row needed |
| Unbounded | O(nW) | O(nW) | O(nW) | O(W) | Simpler recurrence |
| FPTAS | O(n²/ε) | O(n²/ε) | O(n²/ε) | O(n/ε) | Approximation with error ε |

## When to Use / When to Avoid
- Use if:
  - Need exact optimal solution for moderate capacities
  - Items have discrete weights and values
  - Problem maps to resource allocation with constraints
- Avoid if:
  - Capacity or number of items is extremely large (use approximation)
  - Fractional items are allowed (use Greedy instead)
  - Real-time solution needed for large instances

## Correctness (Proof Sketch)
- Optimal substructure: Optimal solution contains optimal solutions to subproblems
- For 0/1: Either include item i or don't - take maximum value
- For unbounded: Consider all ways to include each item (0, 1, 2, ... times)
- Base case: Zero capacity or zero items gives zero value
- Induction: Optimal value at each step builds on previous optimal values

## Pseudocode (canonical)
```pseudo
# 0/1 Knapsack
function knapsack01(values, weights, capacity, n):
    # dp[i][w] = max value using first i items with capacity w
    dp[0...n][0...capacity] = 0
    
    for i = 1 to n:
        for w = 0 to capacity:
            if weights[i-1] <= w:
                # Max of including or excluding current item
                dp[i][w] = max(values[i-1] + dp[i-1][w-weights[i-1]], dp[i-1][w])
            else:
                # Can't include current item
                dp[i][w] = dp[i-1][w]
                
    return dp[n][capacity]

# Unbounded Knapsack
function unboundedKnapsack(values, weights, capacity, n):
    # dp[w] = max value with capacity w
    dp[0...capacity] = 0
    
    for w = 0 to capacity:
        for i = 0 to n-1:
            if weights[i] <= w:
                dp[w] = max(dp[w], dp[w-weights[i]] + values[i])
                
    return dp[capacity]
```

## Implementation Notes
- For 0/1, space can be optimized to O(W) by using only two rows or a single row with reverse iteration
- Watch for off-by-one errors in array indexing
- For large capacities, consider scaling values or weights for FPTAS approximation
- Can be solved with recursive memoization, but bottom-up typically more efficient
- For small weights and large capacity, consider using a value-based DP instead of weight-based

## Edge-case Checklist
- Empty item list: Return 0
- Zero capacity: Return 0
- Single item larger than capacity: Return 0
- Negative weights/values: Not handled in standard algorithm
- Integer overflow: Watch for large sums of values

## Examples
- Minimal example (0/1):
  - Items: [(value=60, weight=10), (value=100, weight=20), (value=120, weight=30)]
  - Capacity: 50
  - Output: 220 (take items 1 and 2)
  
- Tricky counterexample:
  - Greedy by value/weight ratio fails:
  - Items: [(value=60, weight=10), (value=100, weight=20), (value=120, weight=30)]
  - Capacity: 50
  - Greedy by ratio would pick items 0 and 1 for value 160, but optimal is 220

## Variants / Alternatives
- 0/1 Knapsack: Each item can be used at most once
- Unbounded Knapsack: Each item can be used any number of times
- Bounded Knapsack: Each item has a specific limit
- Multiple Knapsack: Multiple containers with different capacities
- Multi-dimensional Knapsack: Multiple constraints (weight, volume, etc.)
- Fractional Knapsack: Items can be broken (solved with Greedy)
- FPTAS: Fully Polynomial Time Approximation Scheme for large instances

## References
- Cormen, Leiserson, Rivest, Stein: Introduction to Algorithms
- Martello, Toth: Knapsack Problems: Algorithms and Computer Implementations
- Kellerer, Pferschy, Pisinger: Knapsack Problems
