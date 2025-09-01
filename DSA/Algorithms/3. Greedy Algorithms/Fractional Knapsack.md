# Fractional Knapsack

## Name
- Category: Greedy
- Summary: Maximize value in a capacity-constrained knapsack by taking fractions of items based on value-to-weight ratio.

## Preconditions / Assumptions
- Items can be divided into fractions (continuous)
- Each item has a weight and value
- Non-negative weights and values
- Knapsack has a maximum weight capacity
- Goal is to maximize total value while respecting capacity

## Properties
- Deterministic
- Complete (finds optimal solution)
- Greedy (selects items by value-to-weight ratio)
- Polynomial time complexity
- Always produces optimal solution (unlike 0/1 knapsack)

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Sort-based | O(n log n) | O(n) | Dominated by sorting |
| Heap-based | O(n log n) | O(n) | Using priority queue |
| Linear selection | O(n) | O(n) | Using quickselect for median |

## When to Use / When to Avoid
- Use if:
  - Items can be divided into arbitrary fractions
  - Need optimal solution with efficient algorithm
  - Value is proportional to amount taken
- Avoid if:
  - Items must be taken whole (use 0/1 knapsack)
  - Items have dependencies or constraints
  - Taking partial items is not meaningful in context

## Correctness (Proof Sketch)
- Greedy choice property: Always optimal to take items with highest value-to-weight ratio first
- Exchange argument: Any solution not following this order can be improved
- Proof by contradiction: If a non-greedy solution were optimal, swapping items would increase value
- Optimality: Unlike 0/1 knapsack, fractional allows taking the best portions of each item
- Termination: Algorithm stops when knapsack is full or all items are considered

## Pseudocode (canonical)
```pseudo
function fractionalKnapsack(items, capacity):
    # Calculate value-to-weight ratio for each item
    for each item in items:
        item.ratio = item.value / item.weight
    
    # Sort items by value-to-weight ratio in descending order
    sort(items, by=ratio, descending=true)
    
    totalValue = 0
    remainingCapacity = capacity
    
    for each item in items:
        if remainingCapacity >= item.weight:
            # Take the whole item
            totalValue += item.value
            remainingCapacity -= item.weight
        else:
            # Take a fraction of the item
            fraction = remainingCapacity / item.weight
            totalValue += fraction * item.value
            remainingCapacity = 0
            break
    
    return totalValue
```

## Implementation Notes
- Sort items by value-to-weight ratio in descending order
- Can use a priority queue instead of sorting
- Handle edge cases like zero weights (infinite ratio)
- For large datasets, consider in-place sorting
- Can be extended to track which items were selected and in what amounts
- Consider using a stable sort if equal ratios should maintain original order
- Use appropriate numeric types to avoid precision issues with fractions

## Edge-case Checklist
- Empty item list: Return 0
- Zero capacity: Return 0
- Zero weight items: Handle infinite value-to-weight ratio
- Negative weights or values: Not typically allowed
- All items fit: Take all items completely
- No items fit completely: Take fractions of highest ratio item
- Equal value-to-weight ratios: Order doesn't matter for optimality

## Examples
- Minimal example:
  - Items: [(value=60, weight=10), (value=100, weight=20), (value=120, weight=30)]
  - Capacity: 50
  - Value-to-weight ratios: [6, 5, 4]
  - Solution: Take all of item 1 (60), all of item 2 (100), and 2/3 of item 3 (80)
  - Total value: 240
  
- Edge case example:
  - Items: [(value=60, weight=50), (value=100, weight=30), (value=120, weight=60)]
  - Capacity: 10
  - Value-to-weight ratios: [1.2, 3.33, 2]
  - Solution: Take 1/3 of item 2 (33.33)
  - Total value: 33.33

## Variants / Alternatives
- Multi-objective fractional knapsack: Optimize multiple values
- Bounded fractional knapsack: Upper limit on fraction of each item
- Continuous knapsack with variable density: Value-to-weight ratio varies with amount
- Online fractional knapsack: Items arrive one by one
- Alternatives:
  - 0/1 Knapsack: When items can't be divided (dynamic programming)
  - Multiple Knapsack: Multiple containers with different capacities
  - Multi-dimensional Knapsack: Multiple resource constraints

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 16
- Kleinberg, Tardos: "Algorithm Design", Chapter 4
- Martello, Toth: "Knapsack Problems: Algorithms and Computer Implementations"
- Dantzig, George B.: "Discrete-Variable Extremum Problems"
