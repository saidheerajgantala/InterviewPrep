# Line Intersection

## Name
- Category: Computational Geometry
- Summary: Algorithms for detecting and computing the intersection point(s) of line segments in a plane, including both the simple case of two segments and the more complex case of finding all intersections among multiple segments.

## Preconditions / Assumptions
- Lines are represented as segments with two endpoints (x1, y1) and (x2, y2)
- Coordinates are real numbers (typically floating-point)
- For multiple line segments, they are provided as a collection or array
- Intersections occur only at points (not along overlapping segments, unless specified)
- Numerical precision is sufficient for accurate calculations

## Properties
- Deterministic
- Complete (finds all intersections)
- For two segments: O(1) time complexity
- For n segments: O(n log n + k log n) time complexity using sweep line (where k is the number of intersections)
- Handles special cases like parallel lines, overlapping segments, and edge intersections
- Can be extended to handle vertical segments and edge cases

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Two segments | O(1) | O(1) | Simple vector algebra |
| Brute force | O(n²) | O(1) | Check all pairs |
| Sweep line | O(n log n + k log n) | O(n) | k = number of intersections |
| Bentley-Ottmann | O(n log n + k log n) | O(n) | More efficient implementation |
| Output-sensitive | O((n+k) log n) | O(n) | For reporting all intersections |

## When to Use / When to Avoid
- Use if:
  - Need to determine if two line segments intersect
  - Need to find the exact intersection point
  - Need to find all intersections among many segments
  - Geometric computations are required in 2D space
  - Precision is important
- Avoid if:
  - Approximate solutions are acceptable (simpler methods may suffice)
  - Dealing with 3D lines (requires different algorithms)
  - Numerical precision is a concern (consider robust geometric predicates)
  - Very large number of segments with many intersections (consider specialized algorithms)

## Correctness (Proof Sketch)
- For two segments:
  - Two line segments intersect if and only if:
    - Each segment straddles the line containing the other segment, or
    - The segments are collinear and overlap
  - This can be determined using the orientation test
  - The intersection point can be computed using parametric line equations
- For sweep line algorithm:
  - Correctness follows from the invariant that all intersections to the left of the sweep line have been found
  - Events are processed in left-to-right order
  - Only adjacent segments in the status structure can intersect
  - All potential intersections are detected and processed

## Pseudocode (canonical)
```pseudo
# Two-segment intersection
function doSegmentsIntersect(p1, q1, p2, q2):
    # Check if the segments straddle each other
    o1 = orientation(p1, q1, p2)
    o2 = orientation(p1, q1, q2)
    o3 = orientation(p2, q2, p1)
    o4 = orientation(p2, q2, q1)
    
    # General case: segments straddle each other
    if o1 != o2 and o3 != o4:
        return true
    
    # Special cases: collinear segments
    if o1 == 0 and onSegment(p1, p2, q1):
        return true
    if o2 == 0 and onSegment(p1, q2, q1):
        return true
    if o3 == 0 and onSegment(p2, p1, q2):
        return true
    if o4 == 0 and onSegment(p2, q1, q2):
        return true
    
    return false

function orientation(p, q, r):
    val = (q.y - p.y) * (r.x - q.x) - (q.x - p.x) * (r.y - q.y)
    
    if val == 0:
        return 0  # Collinear
    return val > 0 ? 1 : 2  # Clockwise or Counterclockwise

function onSegment(p, q, r):
    return q.x <= max(p.x, r.x) and q.x >= min(p.x, r.x) and
           q.y <= max(p.y, r.y) and q.y >= min(p.y, r.y)

# Computing intersection point
function computeIntersection(p1, q1, p2, q2):
    # Line 1 represented as a1x + b1y = c1
    a1 = q1.y - p1.y
    b1 = p1.x - q1.x
    c1 = a1 * p1.x + b1 * p1.y
    
    # Line 2 represented as a2x + b2y = c2
    a2 = q2.y - p2.y
    b2 = p2.x - q2.x
    c2 = a2 * p2.x + b2 * p2.y
    
    determinant = a1 * b2 - a2 * b1
    
    if determinant == 0:
        # Lines are parallel or coincident
        return null
    
    # Compute intersection point
    x = (b2 * c1 - b1 * c2) / determinant
    y = (a1 * c2 - a2 * c1) / determinant
    
    # Check if intersection point lies on both segments
    if onSegment(p1, (x, y), q1) and onSegment(p2, (x, y), q2):
        return (x, y)
    
    return null

# Sweep line algorithm for multiple segments
function findAllIntersections(segments):
    # Event queue: segment endpoints and intersection points
    events = []
    
    # Add segment endpoints to event queue
    for each segment in segments:
        events.add((segment.left, "LEFT", segment))
        events.add((segment.right, "RIGHT", segment))
    
    # Sort events by x-coordinate
    sort(events)
    
    # Status structure: active segments
    status = new balanced binary search tree
    
    # Intersections found
    intersections = []
    
    while events is not empty:
        event = events.removeMin()
        
        if event.type == "LEFT":
            # Add segment to status
            status.insert(event.segment)
            
            # Check for intersections with segments above and below
            above = status.above(event.segment)
            below = status.below(event.segment)
            
            if above != null:
                checkAndAddIntersection(event.segment, above, events, intersections)
            
            if below != null:
                checkAndAddIntersection(event.segment, below, events, intersections)
        
        else if event.type == "RIGHT":
            # Remove segment from status
            above = status.above(event.segment)
            below = status.below(event.segment)
            
            status.remove(event.segment)
            
            # Check for new intersections between segments above and below
            if above != null and below != null:
                checkAndAddIntersection(above, below, events, intersections)
        
        else:  # Intersection event
            s1 = event.segment1
            s2 = event.segment2
            
            # Add intersection to result
            intersections.add((event.point, s1, s2))
            
            # Swap the order of s1 and s2 in the status
            status.swap(s1, s2)
            
            # Check for new intersections
            above = status.above(s1)
            if above != null:
                checkAndAddIntersection(s1, above, events, intersections)
            
            below = status.below(s2)
            if below != null:
                checkAndAddIntersection(s2, below, events, intersections)
    
    return intersections

function checkAndAddIntersection(s1, s2, events, intersections):
    intersection = computeIntersection(s1.p1, s1.p2, s2.p1, s2.p2)
    
    if intersection != null and intersection.x > currentSweepLinePosition:
        events.add((intersection, "INTERSECTION", s1, s2))
```

## Implementation Notes
- Use robust geometric predicates to handle numerical precision issues
- Consider using integer arithmetic or exact arithmetic for critical applications
- Handle special cases like vertical lines, coincident endpoints, and overlapping segments
- For the sweep line algorithm, use a self-balancing binary search tree for the status structure
- Consider using a priority queue for the event queue
- Be careful with floating-point comparisons and use appropriate epsilon values
- For large datasets, consider spatial partitioning techniques
- Handle degeneracies like multiple segments intersecting at the same point
- Implement proper event ordering for the sweep line algorithm
- Consider using specialized geometric libraries for production code

## Edge-case Checklist
- Parallel lines: No intersection or coincident lines
- Vertical lines: Special handling for infinite slope
- Collinear segments: May overlap or share endpoints
- Segments sharing an endpoint: Count as intersection or not based on requirements
- Degenerate segments (points): Handle according to problem definition
- Multiple segments intersecting at the same point: Handle properly in sweep line
- Numerical precision issues: Use robust geometric predicates
- Empty input: Return empty result
- Single segment: No intersections

## Examples
- Two-segment intersection:
  - Segments: [(0, 0) to (10, 10)] and [(0, 10) to (10, 0)]
  - Orientation tests: o1 = 2, o2 = 1, o3 = 2, o4 = 1
  - Result: Segments intersect at (5, 5)
  
- Multiple segments:
  - Segments: [(0, 0) to (10, 10)], [(0, 10) to (10, 0)], [(5, 0) to (5, 10)]
  - Sweep line processes events from left to right
  - First intersection: (5, 5) between segments 1 and 2
  - Second intersection: (5, 5) between segments 1 and 3
  - Third intersection: (5, 5) between segments 2 and 3
  - Result: Three intersections at point (5, 5)

## Variants / Alternatives
- Bentley-Ottmann algorithm: Efficient implementation of sweep line
- DCEL (Doubly Connected Edge List): For handling planar subdivisions
- Robust geometric predicates: For handling numerical precision
- Line segment intersection in 3D: Different algorithms required
- Alternatives:
  - Spatial partitioning: Grid, quadtree, etc. for large datasets
  - Randomized incremental construction: Alternative to sweep line
  - GPU-based algorithms: For massive parallelism
  - Approximate algorithms: For faster but less precise results

## References
- Preparata, Franco P. and Shamos, Michael Ian: "Computational Geometry: An Introduction" (1985)
- de Berg, Mark et al.: "Computational Geometry: Algorithms and Applications"
- Bentley, Jon L. and Ottmann, Thomas A.: "Algorithms for Reporting and Counting Geometric Intersections" (1979)
- Shewchuk, Jonathan Richard: "Robust Adaptive Floating-Point Geometric Predicates"
- O'Rourke, Joseph: "Computational Geometry in C"
