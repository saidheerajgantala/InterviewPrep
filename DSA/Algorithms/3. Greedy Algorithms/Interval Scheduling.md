# Interval Scheduling

## Name
- Category: Greedy
- Summary: Algorithm to select the maximum number of non-overlapping intervals from a set of intervals, by repeatedly selecting the interval with the earliest finish time.

## Preconditions / Assumptions
- Each interval has a start time and finish time
- Intervals are well-defined (start time ≤ finish time)
- Goal is to maximize the number of selected intervals
- Selected intervals must not overlap
- All intervals have equal value/weight

## Properties
- Deterministic
- Complete (finds optimal solution)
- Greedy approach (selects by earliest finish time)
- Optimal substructure (optimal solution contains optimal solutions to subproblems)
- Linear time after sorting

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(n log n) | O(n) | Dominated by sorting |
| Pre-sorted | O(n) | O(n) | If intervals already sorted by finish time |
| Weighted | O(n log n) | O(n) | Different problem, requires dynamic programming |
| With resources | O(n log n) | O(n) | Interval partitioning problem |

## When to Use / When to Avoid
- Use if:
  - Need to select maximum number of non-overlapping intervals
  - All intervals have equal importance
  - Greedy approach is acceptable
  - Intervals can be preempted (stopped and not resumed)
- Avoid if:
  - Intervals have different weights/values (use weighted interval scheduling)
  - Need to minimize number of resources (use interval partitioning)
  - Intervals have dependencies or constraints
  - All intervals must be scheduled

## Correctness (Proof Sketch)
- Greedy choice: Always select the interval with earliest finish time
- Exchange argument: If a later-finishing interval is selected instead, we can replace it with an earlier-finishing one and select at least as many intervals
- Invariant: At each step, we have selected the optimal set of intervals that finish before the current time
- Optimal substructure: After selecting an interval, the subproblem is identical with reduced time range
- Termination: All intervals are considered exactly once

## Pseudocode (canonical)
```pseudo
function intervalScheduling(intervals):  # Each interval has start and finish time
    # Sort intervals by finish time
    sort(intervals, by=finish_time)
    
    selected = []
    last_finish_time = -infinity
    
    for each interval in intervals:
        if interval.start >= last_finish_time:
            # This interval doesn't overlap with the last selected interval
            selected.add(interval)
            last_finish_time = interval.finish
    
    return selected
```

## Implementation Notes
- Sort intervals by finish time first (crucial for correctness)
- Handle edge cases like empty input or single interval
- For weighted interval scheduling, use dynamic programming instead
- For interval partitioning (assign intervals to minimum number of resources), use a different approach
- Consider using a priority queue for more complex variants
- Be careful with inclusive vs. exclusive interval endpoints
- Can be extended to handle real-time scheduling scenarios

## Edge-case Checklist
- Empty interval list: Return empty result
- Single interval: Always selected
- All intervals overlap: Only earliest-finishing selected
- No overlaps: All intervals selected
- Equal finish times: Any consistent tie-breaking works
- Zero-duration intervals: Can be packed at same time point
- Large time values: Watch for integer overflow

## Examples
- Minimal example:
  - Intervals: [(1,4), (2,6), (3,5), (5,7), (7,9)]
    - Format: (start_time, finish_time)
  - Sorted by finish time: [(1,4), (3,5), (2,6), (5,7), (7,9)]
  - Selection process:
    - Select (1,4), last_finish_time = 4
    - Select (5,7), last_finish_time = 7 (skipping (3,5) and (2,6) as they overlap)
    - Select (7,9), last_finish_time = 9
  - Result: [(1,4), (5,7), (7,9)]
  
- Visualization:
  ```
  1     4
    2         6
      3    5
            5    7
                  7    9
  ```
  Selected: 1st, 4th, 5th intervals

## Variants / Alternatives
- Weighted Interval Scheduling: Intervals have weights, maximize total weight (requires dynamic programming)
- Interval Partitioning: Assign all intervals to minimum number of resources
- Real-time scheduling: With deadlines and processing times
- Interval Coloring: Assign colors to intervals so that overlapping intervals have different colors
- Alternatives:
  - Activity Selection: Same problem, different name
  - Earliest Deadline First (EDF): For real-time scheduling
  - Dynamic programming for weighted variant
  - Minimum number of rooms required for meetings

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms", Chapter 16.1
- Kleinberg, Tardos: "Algorithm Design", Chapter 4
- Brucker, Peter: "Scheduling Algorithms"
- Pinedo, Michael L.: "Scheduling: Theory, Algorithms, and Systems"
