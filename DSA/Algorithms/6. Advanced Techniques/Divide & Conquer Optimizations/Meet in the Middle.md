# Meet in the Middle

## Name
- Category: Divide and Conquer, Optimization
- Summary: Technique that splits the input into two roughly equal parts, solves each part separately, and then combines the results to solve the original problem, typically reducing exponential complexity from O(2^n) to O(2^(n/2)).

## Preconditions / Assumptions
- Problem can be divided into two independent subproblems
- Brute force approach would be too slow (typically O(2^n))
- Problem exhibits properties that allow efficient combination of subproblem solutions
- Memory usage is not a critical constraint
- Input size is moderate (typically n ≤ 40-50)

## Properties
- Deterministic
- Complete (finds optimal solution if one exists)
- Space-time tradeoff (uses more memory to reduce time)
- Applicable to problems with exponential complexity
- Often combined with other techniques like dynamic programming or binary search
- Reduces time complexity at the cost of increased space complexity

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(2^(n/2)) | O(2^(n/2)) | n is input size |
| With sorting | O(2^(n/2) * log(2^(n/2))) | O(2^(n/2)) | For binary search in combination step |
| With hashing | O(2^(n/2)) | O(2^(n/2)) | Average case with hash table |
| With pruning | O(2^(n/2)) | O(2^(n/2)) | Better average case |

## When to Use / When to Avoid
- Use if:
  - Problem has exponential complexity with brute force
  - Input size is too large for brute force but too small for polynomial algorithms
  - Problem can be naturally divided into two parts
  - Memory usage is not a critical constraint
  - Combination step can be done efficiently
- Avoid if:
  - Memory is severely constrained
  - Problem cannot be divided into independent subproblems
  - Input size is very large (even 2^(n/2) would be too slow)
  - More efficient specialized algorithms exist
  - Problem doesn't exhibit combinatorial explosion

## Correctness (Proof Sketch)
- The problem is divided into two subproblems of roughly equal size
- All possible solutions for each subproblem are computed and stored
- The combination step correctly identifies valid combinations from the two subproblems
- The optimal solution must consist of an optimal solution from the first subproblem and an optimal solution from the second subproblem
- By considering all combinations, we guarantee finding the optimal solution
- The division doesn't miss any potential solutions

## Pseudocode (canonical)
```pseudo
function meetInTheMiddle(array, target):
    n = array.length
    mid = n / 2
    
    # Generate all subset sums for the first half
    firstHalf = array[0...mid-1]
    secondHalf = array[mid...n-1]
    
    firstSubsetSums = generateAllSubsetSums(firstHalf)
    secondSubsetSums = generateAllSubsetSums(secondHalf)
    
    # Sort the second subset sums for binary search
    sort(secondSubsetSums)
    
    # Find the best solution
    bestSum = 0
    
    for sum1 in firstSubsetSums:
        # Binary search for the complement in the second half
        complement = target - sum1
        if binarySearch(secondSubsetSums, complement):
            bestSum = max(bestSum, sum1 + complement)
        
        # Alternative: find closest value not exceeding complement
        closestSum = findClosestNotExceeding(secondSubsetSums, complement)
        bestSum = max(bestSum, sum1 + closestSum)
    
    return bestSum

function generateAllSubsetSums(array):
    n = array.length
    result = []
    
    # Generate all 2^n subset sums using bitmasks
    for mask = 0 to 2^n - 1:
        sum = 0
        for i = 0 to n-1:
            if (mask & (1 << i)) != 0:
                sum += array[i]
        result.add(sum)
    
    return result

# Variant: Subset Sum Problem
function subsetSum(array, target):
    n = array.length
    mid = n / 2
    
    # Generate all subset sums for the first half
    firstHalf = array[0...mid-1]
    secondHalf = array[mid...n-1]
    
    firstSubsetSums = generateAllSubsetSums(firstHalf)
    secondSubsetSums = generateAllSubsetSums(secondHalf)
    
    # Sort the second subset sums for binary search
    sort(secondSubsetSums)
    
    # Check if target sum is possible
    for sum1 in firstSubsetSums:
        complement = target - sum1
        if binarySearch(secondSubsetSums, complement):
            return true
    
    return false
```

## Implementation Notes
- Use efficient data structures for storing and searching subset sums (e.g., hash tables, sorted arrays)
- For problems requiring exact matches, hash tables can provide O(1) lookup
- For problems requiring closest matches, sorted arrays with binary search are better
- Consider using bit manipulation for generating all subsets efficiently
- Be careful with duplicate values in the input
- For optimization problems, store additional information with subset sums
- Consider pruning techniques to reduce the number of subsets generated
- Watch for integer overflow when computing subset sums
- For very large inputs, consider using parallel processing

## Edge-case Checklist
- Empty array: Handle appropriately
- Single element: Trivial solution
- No valid solution exists: Return appropriate indicator
- Multiple valid solutions: Define criteria for selecting one
- All zeros or all same value: May have special handling
- Negative numbers: Be careful with ordering and comparisons
- Very large numbers: Watch for overflow
- Memory limits: May need to optimize space usage

## Examples
### Subset Sum Problem
- Problem: Determine if there's a subset of array that sums to target
- Input: array = [3, 34, 4, 12, 5, 2], target = 9
- Approach:
  - Split into [3, 34, 4] and [12, 5, 2]
  - First half subsets: {}, {3}, {34}, {4}, {3,34}, {3,4}, {34,4}, {3,34,4}
  - First half sums: {0, 3, 34, 4, 37, 7, 38, 41}
  - Second half subsets: {}, {12}, {5}, {2}, {12,5}, {12,2}, {5,2}, {12,5,2}
  - Second half sums: {0, 12, 5, 2, 17, 14, 7, 19}
  - Check if any sum from first half + any sum from second half equals 9
  - Found: 7 (from first half) + 2 (from second half) = 9
  - Result: true

### Knapsack Problem
- Problem: Find maximum value subset with weight constraint
- Input: weights = [5, 4, 6, 3], values = [10, 40, 30, 50], capacity = 10
- Approach:
  - Split into [5, 4] and [6, 3]
  - Generate all subset combinations with their weights and values
  - For each subset from first half, find the best complementary subset from second half
  - Result: Items with weights [4, 6] and values [40, 30], total value 70

## Variants / Alternatives
- With sorting and binary search: For efficient combination step
- With hashing: For faster lookup of complements
- With pruning: To reduce the number of subsets generated
- Parallel meet in the middle: Distribute subset generation across processors
- Alternatives:
  - Dynamic programming: For problems with optimal substructure
  - Branch and bound: For optimization with pruning
  - Approximation algorithms: When exact solution is not required
  - Integer linear programming: For certain optimization problems

## References
- Horowitz, Ellis and Sahni, Sartaj: "Computing Partitions with Applications to the Knapsack Problem" (1974)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Competitive Programming resources (e.g., CP-Algorithms)
- "Meet in the Middle" technique in competitive programming contests
- "Handbook of Exact Algorithms for NP-Hard Problems"
