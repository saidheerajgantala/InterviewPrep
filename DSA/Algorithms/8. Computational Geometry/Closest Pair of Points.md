# Closest Pair of Points

## Name
- Category: Computational Geometry, Divide and Conquer
- Summary: Algorithm that finds the pair of points with the smallest Euclidean distance among a set of points in a plane, using a divide-and-conquer approach to achieve O(n log n) time complexity.

## Preconditions / Assumptions
- Input is a set of points in a 2D plane
- Each point has x and y coordinates
- Points are distinct (no duplicates)
- Distance is measured using Euclidean distance
- The number of points is at least 2

## Properties
- Deterministic
- Complete (finds the optimal solution)
- Divide and conquer approach
- Time complexity: O(n log n)
- Space complexity: O(n)
- Can be extended to higher dimensions
- Can be modified to find k-nearest pairs

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Naive | O(n²) | O(1) | Compare all pairs |
| Divide and Conquer | O(n log n) | O(n) | Standard approach |
| With presorted points | O(n log n) | O(n) | Points sorted by x-coordinate |
| Higher dimensions | O(n log^(d-1) n) | O(n) | d = number of dimensions |

## When to Use / When to Avoid
- Use if:
  - Need to find the closest pair of points efficiently
  - Number of points is large
  - Points are in a low-dimensional space (typically 2D or 3D)
  - Optimal solution is required
  - Points are static (not frequently changing)
- Avoid if:
  - Number of points is small (naive approach may be simpler)
  - Approximate solution is acceptable
  - Points are frequently changing (consider spatial data structures)
  - Memory is severely constrained
  - Points are in very high dimensions (curse of dimensionality)

## Correctness (Proof Sketch)
- The algorithm divides the points into two halves by x-coordinate
- It recursively finds the closest pair in each half
- It then considers pairs that might span the dividing line
- For points that span the dividing line, only points within a vertical strip of width 2δ need to be considered (where δ is the minimum distance found in the recursive calls)
- Within this strip, for each point, only at most 7 points above it need to be checked (due to geometric packing)
- This bounds the number of distance calculations in the combine step to O(n)
- The recurrence relation is T(n) = 2T(n/2) + O(n), which resolves to O(n log n)
- The algorithm correctly handles all cases, including when the closest pair spans the dividing line

## Pseudocode (canonical)
```pseudo
function closestPair(points):
    # Sort points by x-coordinate
    sort points by x-coordinate
    
    # Call the recursive function
    return closestPairRecursive(points, 0, points.length - 1)

function closestPairRecursive(points, left, right):
    # Base cases
    if right - left <= 3:
        return bruteForceClosestPair(points, left, right)
    
    # Divide: Find the middle point
    mid = (left + right) / 2
    midPoint = points[mid]
    
    # Recursively find closest pairs in left and right halves
    leftPair = closestPairRecursive(points, left, mid)
    rightPair = closestPairRecursive(points, mid + 1, right)
    
    # Find the smaller of the two distances
    closestPair = min(leftPair, rightPair)
    minDist = distance(closestPair[0], closestPair[1])
    
    # Create a strip of points around the vertical line at midPoint.x
    strip = []
    for i = left to right:
        if abs(points[i].x - midPoint.x) < minDist:
            strip.add(points[i])
    
    # Sort strip by y-coordinate
    sort strip by y-coordinate
    
    # Find the closest pair in the strip
    stripPair = closestPairInStrip(strip, minDist)
    
    # Return the closest pair found
    if stripPair != null and distance(stripPair[0], stripPair[1]) < minDist:
        return stripPair
    else:
        return closestPair

function closestPairInStrip(strip, minDist):
    closestPair = null
    
    # For each point, check at most 7 points ahead
    for i = 0 to strip.length - 1:
        for j = i + 1 to min(i + 7, strip.length - 1):
            if strip[j].y - strip[i].y >= minDist:
                break  # No need to check further points
            
            if distance(strip[i], strip[j]) < minDist:
                minDist = distance(strip[i], strip[j])
                closestPair = (strip[i], strip[j])
    
    return closestPair

function bruteForceClosestPair(points, left, right):
    minDist = infinity
    closestPair = null
    
    for i = left to right:
        for j = i + 1 to right:
            if distance(points[i], points[j]) < minDist:
                minDist = distance(points[i], points[j])
                closestPair = (points[i], points[j])
    
    return closestPair

function distance(p1, p2):
    return sqrt((p1.x - p2.x)² + (p1.y - p2.y)²)
```

## Implementation Notes
- Use a stable sorting algorithm for consistent results
- Consider using squared distances to avoid square root calculations
- For higher dimensions, adapt the strip checking logic accordingly
- Be careful with floating-point precision issues
- Consider using spatial data structures like k-d trees for dynamic point sets
- Optimize the base case threshold for switching to brute force
- For large point sets, consider parallelizing the divide and conquer steps
- Handle edge cases like collinear points carefully
- Consider using a more efficient data structure for the strip to avoid sorting
- For 3D or higher dimensions, consider specialized algorithms

## Edge-case Checklist
- Empty set: Return appropriate error or default value
- Single point: Return appropriate error or default value
- Two points: Return those two points
- All points on a line: Algorithm still works correctly
- Multiple pairs with the same minimum distance: Return any one
- Points very close to each other: Watch for floating-point precision
- Points very far apart: Watch for overflow in distance calculations
- Points with identical coordinates: Handle according to problem definition
- Degenerate cases: Points forming specific patterns

## Examples
- Minimal example:
  - Points: [(1, 1), (2, 2), (3, 3), (4, 4), (5, 5), (1, 5)]
  - Execution:
    - Sort by x: [(1, 1), (1, 5), (2, 2), (3, 3), (4, 4), (5, 5)]
    - Divide at x = 3
    - Left half: [(1, 1), (1, 5), (2, 2)]
    - Right half: [(3, 3), (4, 4), (5, 5)]
    - Left closest pair: (1, 1) and (2, 2), distance = √2
    - Right closest pair: (3, 3) and (4, 4), distance = √2
    - Overall min distance: √2
    - Strip around x = 3: [(2, 2), (3, 3), (4, 4)]
    - No closer pair in strip
    - Result: Any of the pairs with distance √2
  
- Complex example:
  - Points: [(2, 3), (12, 30), (40, 50), (5, 1), (12, 10), (3, 4)]
  - Closest pair: (2, 3) and (3, 4), distance = √2

## Variants / Alternatives
- Higher-dimensional closest pair: Extension to 3D or more dimensions
- k-closest pairs: Finding k pairs with smallest distances
- Closest pair in L1 (Manhattan) distance: Using different distance metric
- Dynamic closest pair: Supporting insertions and deletions
- Approximate closest pair: Trading accuracy for speed
- Alternatives:
  - Spatial data structures: k-d trees, quadtrees, etc.
  - Grid-based approaches: For approximate solutions
  - Randomized algorithms: For expected O(n log n) time
  - Parallel algorithms: For large datasets

## References
- Preparata, Franco P. and Shamos, Michael Ian: "Computational Geometry: An Introduction" (1985)
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Bentley, Jon L. and Shamos, Michael I.: "Divide-and-Conquer in Multidimensional Space" (1976)
- de Berg, Mark et al.: "Computational Geometry: Algorithms and Applications"
- Kleinberg, Jon and Tardos, Eva: "Algorithm Design", Chapter 5
