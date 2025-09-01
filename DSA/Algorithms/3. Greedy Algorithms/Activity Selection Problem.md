# Activity Selection Problem

## Name
- Category: Greedy
- Summary: Select the maximum number of non-overlapping activities that can be performed by a single person/resource.

## Preconditions / Assumptions
- Each activity has a start time and finish time
- Activities are mutually exclusive (can't do multiple at once)
- Goal is to maximize the number of activities (not total duration)
- Activities can be sorted by finish time efficiently

## Properties
- Deterministic
- Complete (finds optimal solution)
- Greedy (makes locally optimal choice at each step)
- Optimal substructure (optimal solution contains optimal solutions to subproblems)

## Complexity (per variant)
| Variant | Best | Average | Worst | Space | Notes |
|---|---|---|---|---|---|
| Sort + Greedy | O(n log n) | O(n log n) | O(n log n) | O(1) | Dominated by sorting |
| Pre-sorted | O(n) | O(n) | O(n) | O(1) | If already sorted by finish time |
| Recursive | O(n log n) | O(n log n) | O(n log n) | O(n) | Stack space |

## When to Use / When to Avoid
- Use if:
  - Need to maximize number of non-overlapping intervals
  - Each activity has equal importance/value
  - Activities can be preempted (stopped and not resumed)
- Avoid if:
  - Activities have different values/weights (use weighted activity selection)
  - Need to maximize total duration (different problem)
  - Activities can't be preempted (need to complete once started)

## Correctness (Proof Sketch)
- Greedy choice: Always select the activity with earliest finish time
- Proof by exchange argument: Any solution not including the earliest-finishing activity can be improved
- Optimal substructure: After selecting an activity, the subproblem is identical with reduced time range
- Induction: If greedy works for k activities, it works for k+1 by selecting earliest-finishing compatible activity

## Pseudocode (canonical)
```pseudo
function activitySelection(activities):
    # Sort activities by finish time
    sort activities by finish_time
    
    selected = [activities[0]]  # Select first activity
    last_finish = activities[0].finish
    
    for i = 1 to activities.length - 1:
        if activities[i].start >= last_finish:
            selected.add(activities[i])
            last_finish = activities[i].finish
            
    return selected
```

## Implementation Notes
- Sort activities by finish time first (crucial for correctness)
- For clarity, handle empty input separately
- Can be implemented iteratively or recursively
- Tied finish times can be broken arbitrarily (or by start time for better practical performance)
- To reconstruct solution, keep track of selected activities

## Edge-case Checklist
- Empty activity list: Return empty result
- Single activity: Always selected
- All activities overlap: Only earliest-finishing selected
- Identical start/finish times: Treat as separate activities
- Zero-duration activities: Valid, can be packed at same time point

## Examples
- Minimal example:
  - Activities: [(1,4), (3,5), (0,6), (5,7), (3,9), (5,9), (6,10), (8,11), (8,12), (2,14), (12,16)]
  - Output: [(1,4), (5,7), (8,11), (12,16)]
  
- Tricky counterexample:
  - Sorting by start time fails:
  - Activities: [(1,4), (0,6), (5,7)]
  - Sorting by start time would select [(0,6), (5,7)] = 2 activities
  - Optimal is [(1,4), (5,7)] = 2 activities (same count but different selection)
  - With larger examples, start time sorting can give suboptimal count

## Variants / Alternatives
- Weighted Activity Selection: Activities have values/weights (use DP)
- Interval Scheduling: Same problem, different name
- Interval Partitioning: Assign activities to minimum number of resources
- Job Sequencing with Deadlines: Each job has processing time and deadline
- Meeting Room Allocation: Minimum rooms needed for all meetings

## References
- Cormen, Leiserson, Rivest, Stein: Introduction to Algorithms, Chapter 16.1
- Kleinberg, Tardos: Algorithm Design, Chapter 4
- Dasgupta, Papadimitriou, Vazirani: Algorithms, Chapter 5
