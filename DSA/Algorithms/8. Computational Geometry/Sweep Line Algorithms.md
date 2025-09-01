# Sweep Line Algorithms

## Name
- Category: Computational Geometry
- Summary: Family of algorithms that solve geometric problems by simulating a line sweeping across the plane, processing geometric objects as they are encountered, with applications in intersection detection, area calculation, and proximity problems.

## Preconditions / Assumptions
- Geometric objects (points, line segments, polygons) are in a 2D plane
- Objects can be sorted by some coordinate (typically x or y)
- Events can be processed in order
- Data structures are available for efficient insertion, deletion, and querying
- Numerical precision is sufficient for accurate calculations

## Properties
- Deterministic
- Complete (finds all solutions)
- Efficient for many geometric problems
- Reduces dimensionality by processing one dimension at a time
- Maintains a "status" data structure representing the current state of the sweep
- Processes events in order (typically left-to-right or bottom-to-top)
- Can handle various geometric objects and problems

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Line segment intersection | O(n log n + k log n) | O(n) | k = number of intersections |
| Rectangle area union | O(n log n) | O(n) | n = number of rectangles |
| Closest pair of points | O(n log n) | O(n) | n = number of points |
| Voronoi diagram | O(n log n) | O(n) | n = number of sites |
| Fortune's algorithm | O(n log n) | O(n) | For Voronoi diagrams |

## When to Use / When to Avoid
- Use if:
  - Problem involves geometric objects in a plane
  - Objects can be ordered along one dimension
  - Efficient processing of events is required
  - Problem exhibits locality in one dimension
  - Need to find intersections, unions, or proximity relationships
- Avoid if:
  - Problem is inherently three-dimensional or higher
  - Objects cannot be meaningfully ordered
  - Simpler algorithms exist for the specific problem
  - Numerical precision is a critical concern
  - Memory usage is severely constrained

## Correctness (Proof Sketch)
- The sweep line paradigm maintains invariants about the processed and unprocessed portions of the plane
- All events are processed in order, ensuring no event is missed
- The status structure correctly represents the current state of the sweep line
- For line segment intersection:
  - Only segments that intersect the sweep line are in the status structure
  - Only adjacent segments in the status structure can intersect
  - All intersections to the left of the sweep line have been found
- The algorithm terminates when all events have been processed
- Correctness follows from the invariants and the completeness of the event processing

## Pseudocode (canonical)
```pseudo
# Generic sweep line algorithm
function sweepLine(objects):
    # Initialize event queue with all events
    events = new priority queue
    
    # Add initial events
    for each object in objects:
        events.add(createEvent(object))
    
    # Initialize status structure
    status = new balanced binary search tree
    
    # Process events in order
    while events is not empty:
        event = events.removeMin()
        
        # Update sweep line position
        sweepLinePosition = event.position
        
        # Process event based on type
        if event.type == "START":
            handleStartEvent(event, status, events)
        else if event.type == "END":
            handleEndEvent(event, status, events)
        else if event.type == "INTERSECTION":
            handleIntersectionEvent(event, status, events)
    
    return result

# Line segment intersection example
function findIntersections(segments):
    # Initialize event queue with segment endpoints
    events = new priority queue
    
    for each segment in segments:
        events.add((segment.left, "LEFT", segment))
        events.add((segment.right, "RIGHT", segment))
    
    # Initialize status structure and result
    status = new balanced binary search tree
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
                checkForIntersection(event.segment, above, events, intersections)
            
            if below != null:
                checkForIntersection(event.segment, below, events, intersections)
        
        else if event.type == "RIGHT":
            # Remove segment from status
            above = status.above(event.segment)
            below = status.below(event.segment)
            
            status.remove(event.segment)
            
            # Check for new intersections between segments above and below
            if above != null and below != null:
                checkForIntersection(above, below, events, intersections)
        
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
                checkForIntersection(s1, above, events, intersections)
            
            below = status.below(s2)
            if below != null:
                checkForIntersection(s2, below, events, intersections)
    
    return intersections

function checkForIntersection(s1, s2, events, intersections):
    intersection = computeIntersection(s1, s2)
    
    if intersection != null and intersection.x > currentSweepLinePosition:
        events.add((intersection, "INTERSECTION", s1, s2))
```

## Implementation Notes
- Use a self-balancing binary search tree for the status structure
- Use a priority queue for the event queue
- Handle degeneracies and special cases carefully
- Be careful with floating-point precision issues
- Consider using robust geometric predicates
- For large datasets, consider spatial partitioning techniques
- Handle edge cases like vertical segments and coincident endpoints
- Implement proper event ordering (e.g., LEFT events before INTERSECTION events at the same x-coordinate)
- Use appropriate data structures for the specific problem
- Consider using specialized geometric libraries for production code

## Edge-case Checklist
- Empty input: Return empty result
- Single object: Trivial processing
- Vertical line segments: Special handling required
- Collinear segments: May require special handling
- Segments sharing an endpoint: Handle properly in event ordering
- Multiple segments intersecting at the same point: Handle degeneracies
- Numerical precision issues: Use robust geometric predicates
- Overlapping segments: Define behavior based on requirements
- Degenerate objects: Handle according to problem definition

## Examples
### Line Segment Intersection
- Input: Segments [(0, 0) to (10, 10)], [(0, 10) to (10, 0)], [(5, 0) to (5, 10)]
- Events (sorted by x-coordinate):
  1. LEFT event for segment 1 at x=0
  2. LEFT event for segment 2 at x=0
  3. LEFT event for segment 3 at x=5
  4. INTERSECTION event at x=5, y=5 (segments 1 and 2)
  5. INTERSECTION event at x=5, y=5 (segments 1 and 3)
  6. INTERSECTION event at x=5, y=5 (segments 2 and 3)
  7. RIGHT event for segment 1 at x=10
  8. RIGHT event for segment 2 at x=10
  9. RIGHT event for segment 3 at x=5
- Result: Three intersections at point (5, 5)

### Rectangle Union Area
- Input: Rectangles [(0, 0, 3, 2), (1, 1, 4, 3), (2, 0, 5, 2)]
- Events (sorted by x-coordinate):
  1. LEFT event for rectangle 1 at x=0
  2. LEFT event for rectangle 3 at x=2
  3. RIGHT event for rectangle 1 at x=3
  4. LEFT event for rectangle 2 at x=1
  5. RIGHT event for rectangle 3 at x=5
  6. RIGHT event for rectangle 2 at x=4
- Status structure tracks vertical segments at each event
- Result: Total area = 11

## Variants / Alternatives
- Bentley-Ottmann algorithm: For line segment intersection
- Fortune's algorithm: For Voronoi diagrams
- Horizontal sweep: For problems better suited to vertical ordering
- Radial sweep: For problems involving angles around a central point
- Alternatives:
  - Divide and conquer: For some geometric problems
  - Randomized incremental construction: Alternative paradigm
  - Spatial partitioning: Grid, quadtree, etc. for large datasets
  - GPU-based algorithms: For massive parallelism

## References
- Preparata, Franco P. and Shamos, Michael Ian: "Computational Geometry: An Introduction" (1985)
- de Berg, Mark et al.: "Computational Geometry: Algorithms and Applications"
- Bentley, Jon L. and Ottmann, Thomas A.: "Algorithms for Reporting and Counting Geometric Intersections" (1979)
- Fortune, Steven: "A Sweepline Algorithm for Voronoi Diagrams" (1986)
- O'Rourke, Joseph: "Computational Geometry in C"
