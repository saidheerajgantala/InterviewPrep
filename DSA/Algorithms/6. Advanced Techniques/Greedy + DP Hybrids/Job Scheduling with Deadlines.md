# Job Scheduling with Deadlines

## Name
- Category: Greedy Algorithms, Dynamic Programming Hybrids
- Summary: Algorithm to schedule jobs with deadlines and profits to maximize total profit, using a combination of greedy approach for job selection and dynamic programming techniques for optimal scheduling.

## Preconditions / Assumptions
- Each job has a processing time, deadline, and profit/value
- Jobs can be processed in any order
- Only one job can be processed at a time
- A job earns profit only if completed before its deadline
- All jobs take unit time (in the basic version) or specified time (in the general version)
- Time is discrete
- All deadlines and processing times are non-negative integers

## Properties
- Deterministic
- Complete (finds the optimal solution for the given constraints)
- Time complexity: O(n²) for unit time jobs, O(n²d) for variable time jobs (d = max deadline)
- Space complexity: O(n) for unit time jobs, O(nd) for variable time jobs
- Combines greedy selection with dynamic programming
- Can handle both unit time and variable time job scheduling
- Maximizes total profit/value
- Can be extended to handle additional constraints

## Complexity (per variant)
| Variant | Time | Space | Notes |
|---|---|---|---|
| Unit time jobs | O(n²) | O(n) | Basic greedy approach |
| Variable time jobs | O(n²d) | O(nd) | d = maximum deadline |
| With precedence constraints | O(n²d) | O(nd) | Additional constraints |
| Weighted completion time | O(n log n) | O(n) | Different objective function |
| Online scheduling | O(n log n) | O(n) | Jobs arrive over time |

## When to Use / When to Avoid
- Use if:
  - Need to maximize profit with deadline constraints
  - Jobs have different values/profits
  - Single processor/resource scheduling
  - All jobs and deadlines are known in advance
  - Exact optimal solution is required
- Avoid if:
  - Multiple processors/resources are available (use multiprocessor scheduling)
  - Jobs have complex dependencies (use critical path scheduling)
  - Online scheduling is required (jobs arrive dynamically)
  - Approximation is acceptable (simpler algorithms may suffice)
  - Other objectives besides profit maximization are important

## Correctness (Proof Sketch)
- For unit time jobs:
  - Sort jobs by decreasing profit
  - For each job, schedule it at the latest possible time before its deadline
  - This greedy approach is optimal because:
    - Each job contributes its full profit if scheduled
    - Scheduling highest-profit jobs first ensures maximum total profit
    - Scheduling jobs as late as possible creates space for more jobs
  - The algorithm never leaves unnecessary gaps in the schedule
- For variable time jobs:
  - Use dynamic programming to find optimal schedule
  - Define dp[i][t] as maximum profit from first i jobs with t time units
  - Consider including or excluding each job based on profit and deadline
  - The optimal solution builds upon optimal solutions to subproblems

## Pseudocode (canonical)
```pseudo
# Unit Time Jobs Scheduling
function scheduleUnitTimeJobs(jobs):
    # Sort jobs by decreasing profit
    sort jobs in decreasing order of profit
    
    # Find the maximum deadline
    max_deadline = maximum deadline among all jobs
    
    # Initialize schedule array with -1 (empty slots)
    schedule = new array of size max_deadline, initialized with -1
    
    # Schedule jobs
    for i = 0 to jobs.length - 1:
        job = jobs[i]
        
        # Find the latest available slot before the deadline
        for j = min(max_deadline - 1, job.deadline - 1) down to 0:
            if schedule[j] == -1:
                schedule[j] = job.id
                break
    
    # Collect scheduled jobs
    result = []
    total_profit = 0
    
    for i = 0 to max_deadline - 1:
        if schedule[i] != -1:
            result.add(schedule[i])
            total_profit += profit of job with id schedule[i]
    
    return result, total_profit

# Variable Time Jobs Scheduling (Dynamic Programming)
function scheduleVariableTimeJobs(jobs, max_time):
    n = jobs.length
    
    # Sort jobs by deadline
    sort jobs in increasing order of deadline
    
    # Initialize DP table
    dp = new table of size (n+1) × (max_time+1), initialized with 0
    
    # Fill the DP table
    for i = 1 to n:
        job = jobs[i-1]
        
        for t = 0 to max_time:
            # Option 1: Don't include this job
            dp[i][t] = dp[i-1][t]
            
            # Option 2: Include this job if possible
            if t >= job.processing_time and t <= job.deadline:
                profit_with_job = dp[i-1][t - job.processing_time] + job.profit
                dp[i][t] = max(dp[i][t], profit_with_job)
    
    # Find maximum profit and reconstruct schedule
    max_profit = dp[n][max_time]
    schedule = reconstructSchedule(dp, jobs, max_time)
    
    return schedule, max_profit

function reconstructSchedule(dp, jobs, max_time):
    n = jobs.length
    schedule = []
    t = max_time
    
    for i = n down to 1:
        job = jobs[i-1]
        
        # Check if this job was included in the optimal solution
        if dp[i][t] != dp[i-1][t]:
            schedule.add(job.id)
            t -= job.processing_time
    
    return schedule.reverse()  # Return in chronological order
```

## Implementation Notes
- For unit time jobs, use a priority queue for efficient job selection
- For variable time jobs, optimize the DP table space usage
- Consider using a disjoint set data structure for efficient slot finding
- Be careful with 0-indexed vs. 1-indexed arrays and job IDs
- For large number of jobs, consider more efficient data structures
- Handle edge cases like no feasible schedule carefully
- For weighted completion time, sort by profit/time ratio
- Consider using backtracking for reconstructing the schedule
- Implement proper error handling for invalid inputs
- For very tight deadlines, check feasibility before running the algorithm

## Edge-case Checklist
- No jobs: Return empty schedule with zero profit
- All jobs have same deadline: Still need to select highest profit jobs
- All jobs have same profit: Schedule by earliest deadline
- No feasible schedule: Return best possible schedule
- Deadlines beyond maximum time horizon: Truncate to time horizon
- Zero processing time: Handle according to problem definition
- Zero profit jobs: May still be useful for scheduling constraints
- Negative profits: Consider excluding these jobs
- Tight deadlines: May not be able to schedule all jobs

## Examples
- Unit Time Jobs:
  - Jobs: [(id=1, profit=100, deadline=2), (id=2, profit=10, deadline=1), (id=3, profit=15, deadline=2), (id=4, profit=27, deadline=1)]
  - Sorted by profit: [1, 4, 3, 2]
  - Schedule: Job 1 at time 1, Job 4 at time 0
  - Total profit: 100 + 27 = 127
  
- Variable Time Jobs:
  - Jobs: [(id=1, profit=50, time=3, deadline=5), (id=2, profit=20, time=1, deadline=2), (id=3, profit=30, time=2, deadline=4)]
  - DP solution: Schedule Job 2 at time 0, Job 3 at time 1, Job 1 at time 3
  - Total profit: 20 + 30 + 50 = 100

## Variants / Alternatives
- Weighted completion time: Minimize sum of weighted completion times
- Multiprocessor scheduling: Schedule jobs on multiple processors
- Preemptive scheduling: Jobs can be interrupted and resumed
- Online scheduling: Jobs arrive over time
- With precedence constraints: Some jobs must be completed before others
- Alternatives:
  - Earliest Deadline First (EDF): Different objective function
  - Least Slack Time First (LST): For real-time systems
  - Critical Path Method (CPM): For jobs with dependencies
  - Linear programming: For more complex constraints

## References
- Cormen, Leiserson, Rivest, Stein: "Introduction to Algorithms"
- Kleinberg, Jon and Tardos, Eva: "Algorithm Design"
- Pinedo, Michael L.: "Scheduling: Theory, Algorithms, and Systems"
- Brucker, Peter: "Scheduling Algorithms"
- Garey, Michael R. and Johnson, David S.: "Computers and Intractability: A Guide to the Theory of NP-Completeness"
