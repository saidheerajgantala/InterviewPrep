# A* Search Algorithm

## Name
- Category: Graph, Shortest Path, Heuristic Search
- Summary: Informed search algorithm that finds the shortest path from a start node to a goal node using a best-first search guided by a heuristic function that estimates the distance to the goal.

## Preconditions / Assumptions
- Graph is represented in a way that allows neighbor enumeration
- Edge weights are non-negative (typically)
- A heuristic function h(n) is available that estimates distance from node n to goal
- Heuristic is admissible (never overestimates the actual distance)
- For optimal paths, heuristic should be consistent (satisfies triangle inequality)

## Properties
- Deterministic
- Complete (always finds a solution if one exists)
- Optimal if heuristic is admissible
- More efficient than Dijkstra's algorithm by using domain knowledge
- Combines features of greedy best-first search and Dijkstra's algorithm
- Uses f(n) = g(n) + h(n) to prioritize nodes (g = known cost, h = estimated cost)

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(b^d) | O(b^d) | b = branching factor, d = depth of solution |
| With consistent heuristic | O(E) | O(V) | E = edges, V = vertices |
| Bidirectional | O(b^(d/2)) | O(b^(d/2)) | Searches from both ends |
| Weighted A* | O(E) | O(V) | Trades optimality for speed |
| Memory-bounded | O(E) | O(k) | k is memory limit |

## When to Use / When to Avoid
- Use if:
  - Finding shortest path in a spatial or graph problem
  - Good heuristic function is available
  - Need to reduce search space compared to Dijkstra's
  - Domain knowledge can guide the search
  - Single-source, single-target shortest path is needed
- Avoid if:
  - No good heuristic is available (use Dijkstra's instead)
  - Multiple targets (use Dijkstra's)
  - Negative edge weights exist (use Bellman-Ford)
  - Memory is severely constrained
  - Graph is very dense

## Correctness (Proof Sketch)
- A* evaluates nodes based on f(n) = g(n) + h(n)
- g(n) is the exact cost from start to n
- h(n) is the estimated cost from n to goal
- If h(n) is admissible (never overestimates), A* finds optimal path
- If h(n) is consistent (h(n) ≤ cost(n,m) + h(m) for any edge n→m), A* expands nodes in order of increasing f-value
- When a goal is dequeued, the optimal path is found
- Proof follows from properties of best-first search with admissible heuristics

## Pseudocode (canonical)
```pseudo
function aStar(graph, start, goal, heuristic):
    # Priority queue ordered by f-value (g + h)
    openSet = new PriorityQueue()
    openSet.add(start, heuristic(start, goal))
    
    # g[n] is the cost of the best path from start to n currently known
    g = new Map()
    g[start] = 0
    
    # parent[n] is the node immediately preceding n on the best path from start to n
    parent = new Map()
    
    while not openSet.isEmpty():
        current = openSet.extractMin()
        
        if current == goal:
            return reconstructPath(parent, current)
        
        for each neighbor of current:
            # Tentative g score
            tentative_g = g[current] + distance(current, neighbor)
            
            if neighbor not in g or tentative_g < g[neighbor]:
                # This path to neighbor is better than any previous one
                parent[neighbor] = current
                g[neighbor] = tentative_g
                f = tentative_g + heuristic(neighbor, goal)
                
                if neighbor in openSet:
                    openSet.decreaseKey(neighbor, f)
                else:
                    openSet.add(neighbor, f)
    
    return "No path exists"

function reconstructPath(parent, current):
    path = [current]
    while current in parent:
        current = parent[current]
        path.prepend(current)
    return path

# Example heuristic for grid-based pathfinding: Manhattan distance
function manhattanDistance(node, goal):
    return abs(node.x - goal.x) + abs(node.y - goal.y)

# Example heuristic for geographic coordinates: Haversine distance
function haversineDistance(node, goal):
    # Calculate great-circle distance between two points on Earth
    # ... implementation details ...
    return distance_in_kilometers
```

## Implementation Notes
- Use a priority queue (min-heap) for efficient extraction of minimum f-value
- For grid-based pathfinding, common heuristics include:
  - Manhattan distance: |x1-x2| + |y1-y2| (for 4-way movement)
  - Diagonal distance: max(|x1-x2|, |y1-y2|) + (√2-1) * min(|x1-x2|, |y1-y2|) (for 8-way movement)
  - Euclidean distance: √((x1-x2)² + (y1-y2)²) (for any-angle movement)
- Tie-breaking: When nodes have equal f-values, prefer nodes with higher g-values
- Closed set optimization: Keep track of expanded nodes to avoid re-expanding
- Bidirectional A*: Search from both start and goal simultaneously
- Consider using Jump Point Search for grid-based pathfinding
- For memory-bounded variants, consider IDA* or SMA*
- Weighted A* (f = g + w*h, w > 1) trades optimality for speed

## Edge-case Checklist
- Empty graph: Return appropriate error or empty path
- Start equals goal: Return path with just the start node
- No path exists: Return appropriate indicator
- Multiple optimal paths: Return any one of them
- Infinite graph: May not terminate without proper constraints
- Disconnected graph: Handle unreachable nodes
- Zero-cost edges: Be careful with priority queue updates
- Heuristic returning zero: Degenerates to Dijkstra's algorithm

## Examples
- Grid-based pathfinding:
  - Start: (0, 0), Goal: (4, 4)
  - Obstacles at: (1, 1), (2, 1), (3, 1), (3, 2), (3, 3)
  - Using Manhattan distance heuristic
  - A* expands fewer nodes than Dijkstra's by focusing search toward goal
  - Final path: (0,0) → (1,0) → (2,0) → (2,1) → (2,2) → (2,3) → (3,4) → (4,4)
  
- Road network:
  - Heuristic: Straight-line (Euclidean) distance
  - A* expands primarily nodes in the general direction of the destination
  - Much more efficient than Dijkstra's for long-distance routing

## Variants / Alternatives
- IDA* (Iterative Deepening A*): Uses iterative deepening to bound memory usage
- D* (Dynamic A*): For replanning when environment changes
- Jump Point Search: Optimization for uniform-cost grid maps
- Bidirectional A*: Searches from both start and goal
- Weighted A*: Uses weighted heuristic to trade optimality for speed
- Alternatives:
  - Dijkstra's algorithm: When no heuristic is available
  - Greedy Best-First Search: Uses only heuristic, not optimal
  - Breadth-First Search: For unweighted graphs
  - Contraction Hierarchies: For very large road networks

## References
- Hart, P.E., Nilsson, N.J., Raphael, B.: "A Formal Basis for the Heuristic Determination of Minimum Cost Paths" (1968)
- Russell, Stuart and Norvig, Peter: "Artificial Intelligence: A Modern Approach"
- Amit Patel: "Introduction to A*" (Red Blob Games)
- Korf, Richard E.: "Depth-first Iterative-Deepening: An Optimal Admissible Tree Search" (1985)
- Harabor, D. and Grastien, A.: "Online Graph Pruning for Pathfinding on Grid Maps" (2011) (Jump Point Search)
