# Convex Hull (Graham Scan, Jarvis March)

## Name
- Category: Computational Geometry
- Summary: Algorithms for finding the convex hull of a set of points in a plane, where Graham Scan uses sorting and a stack-based approach, while Jarvis March (Gift Wrapping) incrementally selects points with the smallest polar angle.

## Preconditions / Assumptions
- Input is a set of points in a 2D plane
- Each point has x and y coordinates
- Points are distinct (or duplicates can be removed)
- At least 3 non-collinear points for a proper convex hull
- Coordinates are real numbers (typically floating-point)

## Properties
- Deterministic
- Complete (finds all vertices of the convex hull)
- Graham Scan: O(n log n) time complexity
- Jarvis March: O(nh) time complexity, where h is the number of hull vertices
- Output is the vertices of the convex hull in counter-clockwise order
- Can handle collinear points
- Robust against numerical precision issues with proper implementation

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Graham Scan | O(n log n) | O(n) | Dominated by sorting step |
| Jarvis March | O(nh) | O(h) | h = number of hull vertices |
| Andrew's Monotone Chain | O(n log n) | O(n) | Alternative to Graham Scan |
| Chan's Algorithm | O(n log h) | O(n) | Optimal output-sensitive algorithm |
| Quickhull | O(n log n) | O(n) | Average case, O(n²) worst case |

## When to Use / When to Avoid
- Use Graham Scan if:
  - Number of points is large
  - Efficiency is important
  - Points are in general position
  - Implementation simplicity is acceptable
- Use Jarvis March if:
  - Expected number of hull vertices is small (h << n)
  - Points are pre-sorted or sorting is expensive
  - Implementation simplicity is preferred
- Avoid if:
  - Points are in higher dimensions (use higher-dimensional algorithms)
  - Numerical precision is a critical concern (use robust geometric predicates)
  - Memory is severely constrained
  - Points are already sorted in some way (consider Andrew's algorithm)

## Correctness (Proof Sketch)
### Graham Scan
- Start with the lowest point (or leftmost of lowest points)
- Sort remaining points by polar angle around the start point
- Process points in order, maintaining a convex hull of processed points
- For each new point, remove points that would create a non-convex angle
- The invariant is that the stack always contains a convex chain
- After processing all points, the stack contains the convex hull
- Correctness follows from the property that any point inside the current convex chain cannot be part of the final hull

### Jarvis March
- Start with the leftmost point (guaranteed to be on the hull)
- Repeatedly find the point with the smallest polar angle from the current point
- This "gift wrapping" process continues until returning to the start point
- Each selected point must be on the convex hull by construction
- Correctness follows from the property that the point with the smallest polar angle from the current hull point must be the next hull vertex

## Pseudocode (canonical)
```pseudo
# Graham Scan Algorithm
function grahamScan(points):
    if points.length < 3:
        return points  # Convex hull is the points themselves
    
    # Find the point with the lowest y-coordinate (and leftmost if tie)
    pivot = findLowestPoint(points)
    
    # Sort points by polar angle around pivot
    sortByPolarAngle(points, pivot)
    
    # Initialize stack with first three points
    stack = [points[0], points[1]]
    
    # Process remaining points
    for i = 2 to points.length - 1:
        # Remove points that would make a non-left turn
        while stack.length > 1 and !isLeftTurn(stack[stack.length-2], stack[stack.length-1], points[i]):
            stack.pop()
        
        # Add current point to the hull
        stack.push(points[i])
    
    return stack

function isLeftTurn(p1, p2, p3):
    # Cross product to determine turn direction
    return (p2.x - p1.x) * (p3.y - p1.y) - (p2.y - p1.y) * (p3.x - p1.x) > 0

# Jarvis March (Gift Wrapping) Algorithm
function jarvisMarch(points):
    if points.length < 3:
        return points  # Convex hull is the points themselves
    
    # Find the leftmost point
    leftmost = findLeftmostPoint(points)
    
    hull = []
    current = leftmost
    
    # Loop until we return to the start point
    do:
        hull.push(current)
        
        # Find the next point with the smallest polar angle
        next = points[0]
        if next == current:
            next = points[1]
        
        for i = 0 to points.length - 1:
            # If points[i] is more counter-clockwise than next
            if current == next or orientation(current, points[i], next) > 0:
                next = points[i]
        
        current = next
    
    while current != leftmost
    
    return hull

function orientation(p, q, r):
    val = (q.y - p.y) * (r.x - q.x) - (q.x - p.x) * (r.y - q.y)
    
    if val == 0:
        return 0  # Collinear
    return val > 0 ? 1 : 2  # Clockwise or Counterclockwise

# Andrew's Monotone Chain Algorithm (alternative to Graham Scan)
function andrewMonotoneChain(points):
    # Sort points lexicographically
    sort(points) by x-coordinate, then by y-coordinate
    
    # Build lower hull
    lower = []
    for i = 0 to points.length - 1:
        while lower.length >= 2 and !isLeftTurn(lower[lower.length-2], lower[lower.length-1], points[i]):
            lower.pop()
        lower.push(points[i])
    
    # Build upper hull
    upper = []
    for i = points.length - 1 to 0 step -1:
        while upper.length >= 2 and !isLeftTurn(upper[upper.length-2], upper[upper.length-1], points[i]):
            upper.pop()
        upper.push(points[i])
    
    # Remove duplicate points
    upper.pop()
    lower.pop()
    
    # Concatenate hulls
    return lower.concat(upper)
```

## Implementation Notes
- Use a stable sorting algorithm for consistent results
- Be careful with floating-point precision issues
- Consider using integer arithmetic or exact arithmetic for critical applications
- Handle collinear points according to the requirements (include or exclude)
- For Graham Scan, the pivot point should be included in the sorted points
- For Jarvis March, handle the case where multiple points have the same angle
- Consider using a more robust orientation test for numerical stability
- Handle edge cases like empty set, single point, or all collinear points
- For large point sets, consider parallelizing the sorting step in Graham Scan
- Implement proper comparison functions for sorting by polar angle

## Edge-case Checklist
- Empty set: Return empty hull
- Single point: Return the point itself
- Two points: Return both points
- All collinear points: Hull is the two extreme points
- Multiple points with same polar angle: Handle according to requirements
- Numerical precision issues: Use robust geometric predicates
- Duplicate points: Remove before processing or handle in the algorithm
- Points forming a regular polygon: Both algorithms work correctly
- Very large coordinates: Watch for overflow in calculations

## Examples
- Minimal example:
  - Points: [(0, 0), (1, 0), (0, 1), (1, 1), (0.5, 0.5)]
  - Graham Scan:
    - Pivot: (0, 0)
    - Sorted by polar angle: [(0, 0), (1, 0), (1, 1), (0, 1)]
    - Processing: Stack = [(0, 0), (1, 0), (1, 1), (0, 1)]
    - Convex Hull: [(0, 0), (1, 0), (1, 1), (0, 1)]
  - Jarvis March:
    - Start: (0, 0)
    - Next point: (1, 0)
    - Next point: (1, 1)
    - Next point: (0, 1)
    - Next point: (0, 0) (return to start)
    - Convex Hull: [(0, 0), (1, 0), (1, 1), (0, 1)]
  
- Complex example:
  - Points forming a star pattern with interior points
  - Both algorithms correctly identify the outer points forming the hull
  - Graham Scan processes all points but only keeps hull vertices
  - Jarvis March directly identifies hull vertices without processing interior points

## Variants / Alternatives
- Andrew's Monotone Chain: Alternative to Graham Scan, builds upper and lower hulls separately
- Quickhull: Recursive divide-and-conquer approach
- Chan's Algorithm: Combines Jarvis March and Graham Scan for optimal output-sensitive complexity
- Incremental Algorithm: Adds points one by one, updating the hull
- Dynamic Convex Hull: Supports insertions and deletions
- 3D Convex Hull: Extension to three dimensions (e.g., Gift Wrapping, Incremental, Divide-and-Conquer)
- Alternatives:
  - Kirkpatrick–Seidel algorithm: Output-sensitive O(n log h) algorithm
  - Akl-Toussaint heuristic: Preprocessing step to reduce input size
  - Melkman's algorithm: For simple polygons

## References
- Graham, R.L.: "An Efficient Algorithm for Determining the Convex Hull of a Finite Planar Set" (1972)
- Jarvis, R.A.: "On the Identification of the Convex Hull of a Finite Set of Points in the Plane" (1973)
- Andrew, A.M.: "Another Efficient Algorithm for Convex Hulls in Two Dimensions" (1979)
- de Berg, Mark et al.: "Computational Geometry: Algorithms and Applications"
- Preparata, Franco P. and Shamos, Michael Ian: "Computational Geometry: An Introduction"
- O'Rourke, Joseph: "Computational Geometry in C"
