# Coin Change Problem

## Name
- Category: Dynamic Programming (1D)
- Summary: Find the minimum number of coins needed to make a given amount (Min Coins), or count the total number of ways to make change (Count Ways).

## Preconditions / Assumptions
- Set of coin denominations is given
- Unlimited supply of each coin denomination
- Target amount is a non-negative integer
- Coin denominations are positive integers
- For Min Coins: Solution exists (can be guaranteed by including a coin of value 1)

## Properties
- Deterministic
- Complete (finds optimal solution if it exists)
- Optimal substructure (optimal solution contains optimal solutions to subproblems)
- Overlapping subproblems (same subproblems solved multiple times)
- Bottom-up approach typically more efficient than top-down

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Min Coins | O(amount * n) | O(amount) | n = number of coin denominations |
| Count Ways | O(amount * n) | O(amount) | n = number of coin denominations |
| Space-optimized | O(amount * n) | O(amount) | For both variants |
| Recursive + Memo | O(amount * n) | O(amount) | Plus recursion stack |

## When to Use / When to Avoid
- Use if:
  - Need to find optimal coin combination or count all ways
  - Coin denominations and amount are moderate
  - Unlimited supply of each coin
- Avoid if:
  - Limited supply of coins (need constrained knapsack)
  - Very large amount or many denominations (consider approximation)
  - Need to enumerate all actual combinations (not just count)

## Correctness (Proof Sketch)
- Min Coins:
  - Base case: 0 coins needed for amount 0
  - Recurrence: dp[amount] = 1 + min(dp[amount - coin]) for all valid coins
  - Optimal substructure: Minimum coins for amount includes minimum coins for (amount - coin)
  - Termination: All amounts from 0 to target are computed

- Count Ways:
  - Base case: 1 way to make amount 0 (using no coins)
  - Recurrence: dp[amount] += dp[amount - coin] for all valid coins
  - Overlapping subproblems: Same subamounts computed multiple times
  - Order of considering coins affects only computation, not result

## Pseudocode (canonical)
```pseudo
# Minimum Coins
function minCoins(coins, amount):
    # Initialize dp array with infinity
    dp = new array of size (amount + 1), filled with infinity
    dp[0] = 0  # Base case: 0 coins needed for amount 0
    
    for coin in coins:
        for i = coin to amount:
            dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] != infinity else -1

# Count Ways
function countWays(coins, amount):
    # Initialize dp array with 0
    dp = new array of size (amount + 1), filled with 0
    dp[0] = 1  # Base case: 1 way to make amount 0
    
    for coin in coins:
        for i = coin to amount:
            dp[i] += dp[i - coin]
    
    return dp[amount]
```

## Implementation Notes
- For Min Coins, initialize dp array with infinity (or amount+1 as practical infinity)
- For Count Ways, be careful about integer overflow for large amounts
- Order of loops matters:
  - Outer loop over coins, inner loop over amounts: Each coin can be used multiple times
  - Outer loop over amounts, inner loop over coins: Each coin can be used once (different problem)
- Can reconstruct the actual coins used by tracking decisions
- Consider using a sentinel value (like -1) to indicate impossible amounts
- For large amounts, watch for integer overflow in Count Ways

## Edge-case Checklist
- Amount = 0: Return 0 for Min Coins, 1 for Count Ways
- No coins: Return -1 for Min Coins (impossible), 0 for Count Ways
- Impossible amount: Return -1 or infinity for Min Coins
- Single coin denomination: Simple division for Min Coins
- Large amounts: Watch for integer overflow in Count Ways
- Duplicate coin denominations: Remove duplicates or handle appropriately

## Examples
- Min Coins example:
  - Coins: [1, 2, 5], Amount: 11
  - dp: [0, 1, 1, 2, 2, 1, 2, 2, 3, 3, 2, 3]
  - Result: 3 (5 + 5 + 1)
  
- Count Ways example:
  - Coins: [1, 2, 5], Amount: 5
  - dp: [1, 1, 2, 2, 3, 4]
  - Result: 4 (ways: [5], [1,1,1,1,1], [1,1,1,2], [1,2,2])

## Variants / Alternatives
- Coin Change with limited supply: Each coin can be used at most k times
- Minimum number of coins with specific constraints (must use certain coins)
- Maximum number of ways with value less than or equal to amount
- Coin Change with fixed number of coins: Exactly k coins to make amount
- Alternatives:
  - Greedy approach: Works only for canonical coin systems
  - Integer Linear Programming: For complex constraints
  - Knapsack formulation: For limited coin supply

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Kleinberg, Tardos: "Algorithm Design", Chapter on Dynamic Programming
- Dasgupta, Papadimitriou, Vazirani: "Algorithms"
- LeetCode problems: #322 (Min Coins), #518 (Count Ways)
