# Tower of Hanoi

## Name
- Category: Recursion, Divide and Conquer
- Summary: Classical recursive algorithm to solve the Tower of Hanoi puzzle by moving a stack of disks from one rod to another using a third rod as auxiliary, following specific rules.

## Preconditions / Assumptions
- Three rods (source, auxiliary, and target)
- n disks of different sizes, initially stacked on the source rod in ascending order of size (smallest at top)
- Only one disk can be moved at a time
- No disk may be placed on top of a smaller disk
- Goal is to move all disks from source to target rod

## Properties
- Deterministic
- Complete (always finds the optimal solution)
- Recursive divide-and-conquer approach
- Optimal (minimum number of moves)
- Exponential time complexity
- Elegant mathematical solution

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Recursive | O(2^n) | O(n) | n is number of disks, stack space |
| Iterative | O(2^n) | O(1) | Using binary counting or iterative formula |
| Optimized | O(2^n) | O(n) | With memoization for subproblems |
| Multi-peg | O(2^n) | O(n) | For standard 3-peg problem |

## When to Use / When to Avoid
- Use if:
  - Teaching recursion and divide-and-conquer
  - Need to solve the Tower of Hanoi puzzle optimally
  - Number of disks is small to moderate
  - Demonstrating exponential algorithms
- Avoid if:
  - Number of disks is very large (exponential growth)
  - Memory constraints prevent recursive implementation
  - Need approximate solutions for large instances
  - Real-time constraints exist

## Correctness (Proof Sketch)
- Base case: For n=1, simply move the disk from source to target
- Recursive case: For n>1, break into three subproblems:
  1. Move n-1 disks from source to auxiliary
  2. Move the largest disk from source to target
  3. Move n-1 disks from auxiliary to target
- Invariant: Smaller disks always remain above larger disks
- Optimality: Can be proven that 2^n - 1 moves are necessary and sufficient
- Termination: Recursion depth is bounded by n

## Pseudocode (canonical)
```pseudo
function towerOfHanoi(n, source, auxiliary, target):
    if n == 1:
        # Base case: Move the smallest disk directly
        print("Move disk 1 from " + source + " to " + target)
        return 1  # 1 move made
    
    # Move n-1 disks from source to auxiliary using target as auxiliary
    moves1 = towerOfHanoi(n-1, source, target, auxiliary)
    
    # Move the largest disk from source to target
    print("Move disk " + n + " from " + source + " to " + target)
    
    # Move n-1 disks from auxiliary to target using source as auxiliary
    moves2 = towerOfHanoi(n-1, auxiliary, source, target)
    
    # Total moves = moves for n-1 disks + 1 + moves for n-1 disks
    return moves1 + 1 + moves2

# Iterative solution using binary counting
function towerOfHanoiIterative(n):
    total_moves = 2^n - 1
    
    for move = 1 to total_moves:
        # Determine which disk to move based on move number
        disk = getLeastSignificantSetBit(move)
        
        # Determine source and target based on disk number and move number
        if n % 2 == 0:
            # Even number of disks
            if disk % 3 == 1: source = 'A', target = 'B'
            else if disk % 3 == 2: source = 'A', target = 'C'
            else: source = 'B', target = 'C'
        else:
            # Odd number of disks
            if disk % 3 == 1: source = 'A', target = 'C'
            else if disk % 3 == 2: source = 'A', target = 'B'
            else: source = 'B', target = 'C'
        
        print("Move disk " + disk + " from " + source + " to " + target)
```

## Implementation Notes
- Recursive implementation is cleaner but may cause stack overflow for large n
- Iterative implementation avoids recursion stack but is more complex
- For visualization, can track the state of each rod
- Can be implemented with actual data structures (stacks) to simulate the rods
- For large n, consider using the closed-form formula 2^n - 1 for total moves
- Can be extended to solve variants with more than three pegs
- Consider using memoization if subproblems are repeated (rarely needed for standard version)

## Edge-case Checklist
- n = 0: No disks to move, return 0 moves
- n = 1: Simple case, just one move
- Large n: Watch for stack overflow in recursive implementation
- Large n: Integer overflow when calculating 2^n - 1
- Memory constraints: Consider iterative implementation

## Examples
- Minimal example:
  - n = 3 disks
  - Solution steps:
    1. Move disk 1 from A to C
    2. Move disk 2 from A to B
    3. Move disk 1 from C to B
    4. Move disk 3 from A to C
    5. Move disk 1 from B to A
    6. Move disk 2 from B to C
    7. Move disk 1 from A to C
  - Total moves: 2^3 - 1 = 7
  
- Visualization:
  ```
  Initial:    A: [3,2,1]  B: []  C: []
  Step 1:     A: [3,2]    B: []  C: [1]
  Step 2:     A: [3]      B: [2] C: [1]
  Step 3:     A: [3]      B: [2,1] C: []
  Step 4:     A: []       B: [2,1] C: [3]
  Step 5:     A: [1]      B: [2]   C: [3]
  Step 6:     A: [1]      B: []    C: [3,2]
  Step 7:     A: []       B: []    C: [3,2,1]
  ```

## Variants / Alternatives
- Frame-Stewart algorithm: For Tower of Hanoi with more than three pegs
- Iterative solution: Using binary counting or iterative formula
- Cyclic Tower of Hanoi: Disks must move in a cyclic fashion
- Tower of Hanoi with constraints: Additional rules on disk movements
- Alternatives:
  - Dynamic programming approach for variants
  - Graphical state-space search for complex variants
  - Binet's formula for calculating moves without recursion

## References
- Édouard Lucas (inventor of the puzzle, 1883)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Rosen, Kenneth H.: "Discrete Mathematics and Its Applications"
- Knuth, Donald E.: "The Art of Computer Programming, Volume 1: Fundamental Algorithms"
- Recursive formula for minimum moves: M(n) = 2^n - 1
