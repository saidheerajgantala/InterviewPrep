# Convex Hull Trick

## Name
- Category: Dynamic Programming Optimization
- Summary: Technique for optimizing dynamic programming recurrences of the form dp[i] = min/max(dp[j] + b[j] * a[i]) for all j < i, by maintaining a convex hull of lines and efficiently querying the minimum/maximum value.

## Preconditions / Assumptions
- DP recurrence has the form: dp[i] = min/max(dp[j] + b[j] * a[i]) for all j < i
- For basic CHT: If minimizing, lines are added with increasing slopes; if maximizing, with decreasing slopes
- For dynamic CHT: No restriction on slope order, but query x-coordinates must be non-decreasing
- For fully dynamic CHT: No restrictions on slopes or queries, but more complex implementation
- Problem exhibits convexity/concavity property

## Properties
- Deterministic
- Complete (finds optimal solution)
- Reduces time complexity from O(n²) to O(n log n) or O(n)
- Maintains a convex hull of lines
- Exploits geometric properties to optimize DP transitions
- Works for both minimization and maximization problems

## Complexity (per variant)
| Variant | Time (Add Line) | Time (Query) | Space | Notes |
|---|---|---|---|---|
| Basic CHT | O(1) amortized | O(log n) | O(n) | Slopes must be ordered |
| Dynamic CHT | O(log n) | O(log n) | O(n) | Queries must be ordered |
| Fully Dynamic CHT | O(log n) | O(log n) | O(n) | No restrictions |
| Li Chao Tree | O(log n) | O(log n) | O(n) | Works for non-linear functions |

## When to Use / When to Avoid
- Use if:
  - DP recurrence has the form dp[i] = min/max(dp[j] + b[j] * a[i]) for all j < i
  - Problem exhibits convexity/concavity property
  - Reducing time complexity from O(n²) to O(n log n) or O(n) is necessary
  - Working with linear functions in DP optimization
- Avoid if:
  - DP recurrence doesn't fit the required form
  - Problem doesn't exhibit convexity/concavity
  - Simpler optimization techniques (like Divide & Conquer DP) are applicable
  - Functions are not linear (unless using Li Chao Tree variant)

## Correctness (Proof Sketch)
- Each line represents a potential DP transition function
- Convex hull property ensures that only relevant lines are maintained
- For minimization: Lines with smaller y-values at each x-coordinate are optimal
- For maximization: Lines with larger y-values at each x-coordinate are optimal
- Convexity ensures that once a line becomes suboptimal, it remains suboptimal
- Ordered slopes/queries allow for efficient maintenance of the convex hull
- The optimal line at any query point gives the optimal DP transition

## Pseudocode (canonical)
```pseudo
# Basic Convex Hull Trick (for minimization with increasing slopes)
class ConvexHullTrick:
    function initialize():
        lines = []  # List of (slope, intercept) pairs
    
    # Add a line of the form y = m*x + b
    function addLine(m, b):
        # While the last line can be removed
        while lines.size >= 2:
            last = lines[lines.size - 1]
            second_last = lines[lines.size - 2]
            
            # Check if the new line makes the last line irrelevant
            # (intersection of second_last and new line is to the left of
            # intersection of second_last and last line)
            if (b - second_last.b) * (second_last.m - last.m) <= 
               (last.b - second_last.b) * (second_last.m - m):
                lines.pop()  # Remove the last line
            else:
                break
        
        lines.push((m, b))
    
    # Query the minimum value at point x
    function query(x):
        if lines.empty():
            return infinity
        
        # Binary search for the best line
        left = 0
        right = lines.size - 1
        
        while left < right:
            mid = (left + right) / 2
            
            # Compare values at point x
            if lines[mid].m * x + lines[mid].b <= 
               lines[mid+1].m * x + lines[mid+1].b:
                right = mid
            else:
                left = mid + 1
        
        return lines[left].m * x + lines[left].b

# Dynamic Convex Hull Trick (for minimization with arbitrary slopes)
class DynamicCHT:
    function initialize():
        hull = new balanced binary search tree
    
    # Add a line of the form y = m*x + b
    function addLine(m, b):
        # Insert the new line
        hull.insert((m, b))
        
        # Check if the new line makes adjacent lines irrelevant
        it = hull.find((m, b))
        
        # Check if the current line is redundant
        if isRedundant(prev(it), it, next(it)):
            hull.erase(it)
            return
        
        # Remove redundant lines before the current line
        while it != hull.begin() and prev(it) != hull.begin() and
              isRedundant(prev(prev(it)), prev(it), it):
            hull.erase(prev(it))
        
        # Remove redundant lines after the current line
        while next(it) != hull.end() and next(next(it)) != hull.end() and
              isRedundant(it, next(it), next(next(it))):
            hull.erase(next(it))
    
    # Check if the middle line is redundant
    function isRedundant(line1, line2, line3):
        if line1 == null or line3 == null:
            return false
        
        # Cross product to check if the middle line is below the intersection
        # of the first and third lines
        return (line3.b - line1.b) * (line1.m - line2.m) >=
               (line2.b - line1.b) * (line1.m - line3.m)
    
    # Query the minimum value at point x (assuming x is non-decreasing)
    function query(x):
        if hull.empty():
            return infinity
        
        # Find the line with the smallest value at x
        # For non-decreasing x, we can maintain a pointer to the best line
        while next(pointer) != hull.end() and
              next(pointer).m * x + next(pointer).b < 
              pointer.m * x + pointer.b:
            pointer = next(pointer)
        
        return pointer.m * x + pointer.b
```

## Implementation Notes
- For basic CHT, maintain lines in a deque and use binary search for queries
- For dynamic CHT, use a balanced binary search tree (like std::set in C++)
- When implementing for maximization, negate slopes and intercepts or reverse inequalities
- Be careful with floating-point precision issues; consider using integer arithmetic when possible
- For fully dynamic CHT, consider using Li Chao Tree
- In practice, check if the problem constraints allow for simplifications
- For non-linear functions, use Li Chao Tree variant
- Handle edge cases like empty convex hull or single line carefully
- Consider using long long or arbitrary precision arithmetic for large values

## Edge-case Checklist
- Empty convex hull: Return appropriate default value
- Single line: No need for convexity checks
- Parallel lines: Only keep the one with better intercept
- Vertical lines: May need special handling
- Collinear points: Avoid redundant lines
- Precision issues: Be careful with floating-point comparisons
- Integer overflow: Use appropriate data types for large values
- Query outside the range of the convex hull: May need extrapolation

## Examples
- Minimization example:
  - Lines: y = 1x + 3, y = 2x + 1, y = 3x + 0
  - Query at x = 2:
    - Line 1: 1*2 + 3 = 5
    - Line 2: 2*2 + 1 = 5
    - Line 3: 3*2 + 0 = 6
    - Minimum value: 5
  - Convex hull contains only lines 1 and 2 (line 3 is redundant for x < 3)
  
- DP optimization example:
  - Problem: dp[i] = min(dp[j] + a[i]*b[j]) for all j < i
  - Without CHT: O(n²) time complexity
  - With CHT:
    - Lines: y = b[j]*x + dp[j] for all j
    - Query: x = a[i] to get minimum value
    - Time complexity: O(n log n) or O(n)

## Variants / Alternatives
- Li Chao Tree: For non-linear functions and fully dynamic operations
- Dynamic CHT with Treap: Alternative to balanced BST implementation
- Online CHT: For when all lines are known in advance
- 3D Convex Hull Trick: For higher dimensional problems
- Alternatives:
  - Divide & Conquer DP: Another optimization technique for certain DP problems
  - Monotone Queue Optimization: For specific recurrence forms
  - Knuth's Optimization: For certain types of DP recurrences
  - Slope Trick: Related technique for convex functions

## References
- Competitive Programming resources (e.g., CP-Algorithms)
- "Convex Hull Trick and Li Chao Tree" by KACTL
- "Dynamic Programming Optimizations" by Codeforces user PrinceOfPersia
- "Convex Hull Optimization" by Algorithms Live!
- CSES Problem Set: "Dynamic Programming" section
- "Competitive Programmer's Handbook" by Antti Laaksonen 