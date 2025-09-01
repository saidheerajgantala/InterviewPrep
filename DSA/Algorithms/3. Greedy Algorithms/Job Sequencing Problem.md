# Job Sequencing Problem

## Name
- Category: Greedy
- Summary: Algorithm to schedule jobs with deadlines and profits to maximize total profit, by selecting jobs in decreasing order of profit and scheduling them as late as possible within their deadlines.

## Preconditions / Assumptions
- Each job has a deadline and associated profit
- Each job takes unit time to complete
- Only one job can be processed at a time
- Jobs can be scheduled only up to their deadlines
- Goal is to maximize total profit

## Properties
- Deterministic
- Complete (finds optimal solution)
- Greedy approach (selects jobs by profit)
- May not schedule all jobs
- Schedules jobs as late as possible within deadlines

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Standard | O(n²) | O(n) | n is number of jobs |
| With Sorting | O(n log n) | O(n) | Dominated by sorting |
| With Disjoint Set | O(n log n) | O(n) | More efficient implementation |
| Variable Duration | O(n log n) | O(n) | Different problem variant |

## When to Use / When to Avoid
- Use if:
  - Jobs have unit processing time
  - Each job has a deadline and profit
  - Goal is to maximize profit
  - Greedy approach is acceptable
- Avoid if:
  - Jobs have variable processing times (use different algorithms)
  - Jobs have dependencies
  - Preemption is allowed (can pause and resume jobs)
  - All jobs must be scheduled

## Correctness (Proof Sketch)
- Greedy choice: Always select the job with highest profit first
- Exchange argument: If a lower-profit job is scheduled instead of a higher one, swapping them won't decrease total profit
- Invariant: At each step, jobs are scheduled in optimal slots
- Scheduling jobs as late as possible creates maximum flexibility for remaining jobs
- Termination: All jobs are considered exactly once
- Optimality: Can be proven by showing no better solution exists

## Pseudocode (canonical)
```pseudo
function jobSequencing(jobs):  # Each job has id, deadline, profit
    # Sort jobs in decreasing order of profit
    sort(jobs, by=profit, descending=true)
    
    # Find maximum deadline
    max_deadline = maximum deadline among all jobs
    
    # Initialize result array and slot status
    result = array of size max_deadline, initialized with -1
    slot_filled = array of size max_deadline, initialized with false
    
    # Schedule jobs one by one
    for each job in jobs:
        # Find latest available slot before deadline
        for slot = min(job.deadline, max_deadline) - 1 downto 0:
            if not slot_filled[slot]:
                result[slot] = job.id
                slot_filled[slot] = true
                break
    
    # Collect scheduled jobs
    scheduled_jobs = []
    total_profit = 0
    
    for i = 0 to max_deadline - 1:
        if slot_filled[i]:
            scheduled_jobs.add(result[i])
            total_profit += profit of job with id result[i]
    
    return scheduled_jobs, total_profit

# More efficient implementation using Disjoint Set Union
function jobSequencingWithDSU(jobs):
    # Sort jobs in decreasing order of profit
    sort(jobs, by=profit, descending=true)
    
    # Find maximum deadline
    max_deadline = maximum deadline among all jobs
    
    # Initialize DSU
    dsu = DisjointSetUnion(max_deadline + 1)
    
    # Initialize result
    result = []
    total_profit = 0
    
    for each job in jobs:
        # Find available slot
        available_slot = dsu.find(min(job.deadline, max_deadline))
        
        # If slot is available (greater than 0)
        if available_slot > 0:
            # Add job to result
            result.add(job.id)
            total_profit += job.profit
            
            # Mark this slot as filled by unioning with previous slot
            dsu.union(available_slot, available_slot - 1)
    
    return result, total_profit
```

## Implementation Notes
- Sort jobs by profit in descending order first
- For each job, try to schedule it as close to its deadline as possible
- Use a boolean array or set to track which slots are filled
- Consider using a disjoint set union data structure for more efficient slot finding
- For large number of jobs or deadlines, optimize the slot search
- Zero-indexing vs. one-indexing: adjust deadline calculations accordingly
- For variable duration jobs, consider weighted job scheduling with dynamic programming

## Edge-case Checklist
- Empty job list: Return empty result, zero profit
- Single job: Schedule if deadline allows
- All jobs with same deadline: Only highest profit jobs will be scheduled
- All jobs with same profit: Schedule by deadline
- Deadlines out of order: Sort by profit, not deadline
- Zero deadline: Job cannot be scheduled
- Very large deadlines: May need to cap maximum deadline for efficiency

## Examples
- Minimal example:
  - Jobs: [(1, 4, 20), (2, 1, 10), (3, 1, 40), (4, 1, 30)]
    - Format: (id, deadline, profit)
  - Sorted by profit: [(3, 1, 40), (4, 1, 30), (1, 4, 20), (2, 1, 10)]
  - Scheduling:
    - Job 3: Scheduled at time 0
    - Job 4: Cannot be scheduled (slot 0 filled)
    - Job 1: Scheduled at time 3 (last slot before deadline)
    - Job 2: Cannot be scheduled (all slots before deadline filled)
  - Result: [3, 1], Total profit: 40 + 20 = 60
  
- Visualization:
  ```
  Timeline: 0    1    2    3
           [J3] [--] [--] [J1]
  ```

## Variants / Alternatives
- Job sequencing with deadlines and variable processing times
- Job sequencing with release times
- Weighted job scheduling (jobs have different durations)
- Job sequencing with penalties for missing deadlines
- Alternatives:
  - Earliest Deadline First (EDF): Different objective (maximize jobs completed)
  - Interval Scheduling: Similar but with fixed start and end times
  - Dynamic programming approach for weighted job scheduling
  - Linear programming formulation for complex constraints

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Kleinberg, Tardos: "Algorithm Design", Chapter 4
- Brucker, Peter: "Scheduling Algorithms"
- Pinedo, Michael L.: "Scheduling: Theory, Algorithms, and Systems"
