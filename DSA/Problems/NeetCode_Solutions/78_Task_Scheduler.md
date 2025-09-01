# 📝 Task Scheduler

## **Problem Statement**

* Given a characters array `tasks`, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.
* However, there is a non-negative integer `n` that represents the cooldown period between two same tasks (the same letter in the array), that is that there must be at least `n` units of time between any two same tasks.
* Return the least number of units of time that the CPU will take to finish all the given tasks.
* Example 1:
  * Input: tasks = ["A","A","A","B","B","B"], n = 2
  * Output: 8
  * Explanation: 
    * A -> B -> idle -> A -> B -> idle -> A -> B
    * There is at least 2 units of time between any two same tasks.
* Example 2:
  * Input: tasks = ["A","A","A","B","B","B"], n = 0
  * Output: 6
  * Explanation: On this case any permutation of size 6 would work since n = 0.
    * ["A","A","A","B","B","B"]
    * ["A","B","A","B","A","B"]
    * ["B","B","B","A","A","A"]
    * ...
    * And so on.
* Example 3:
  * Input: tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2
  * Output: 16
  * Explanation: 
    * One possible solution is
    * A -> B -> C -> A -> D -> E -> A -> F -> G -> A -> idle -> idle -> A -> idle -> idle -> A
* Constraints:
  * 1 <= task.length <= 10^4
  * tasks[i] is upper-case English letter.
  * The integer n is in the range [0, 100].

---

## **Step 1: Thinking Process (Brute Force)**

* We need to schedule tasks with a cooldown period between the same tasks
* One approach would be to simulate the scheduling process:
  * Keep track of the time when each task was last executed
  * At each time step, choose a task that is not in cooldown
  * If no task is available, be idle
* This approach is intuitive but might be inefficient for large inputs

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a map to store the last execution time of each task
2. Initialize the current time to 0
3. While there are tasks remaining:
   1. Find a task that is not in cooldown (last execution time + n < current time)
   2. If such a task exists, execute it and update its last execution time
   3. If no task is available, be idle
   4. Increment the current time
4. Return the current time

---

## **Step 3: Brute Force Implementation (Code)**

```python
from collections import Counter

def leastInterval(tasks, n):
    task_counts = Counter(tasks)
    last_execution = {}
    current_time = 0
    
    while task_counts:
        # Find a task that is not in cooldown
        executable_task = None
        for task in task_counts:
            if task not in last_execution or last_execution[task] + n < current_time:
                executable_task = task
                break
        
        if executable_task:
            # Execute the task
            task_counts[executable_task] -= 1
            if task_counts[executable_task] == 0:
                del task_counts[executable_task]
            last_execution[executable_task] = current_time
        
        # Increment the time
        current_time += 1
    
    return current_time
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(m * n) where m is the total number of tasks and n is the cooldown period. In the worst case, we might need to check all tasks at each time step.
* Space Complexity: O(k) where k is the number of distinct tasks, for storing the task counts and last execution times.
* This approach is inefficient for large inputs due to the repeated checking of tasks at each time step.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tasks = ["A","A","A","B","B","B"], n = 2:
  * Initialize task_counts = {"A": 3, "B": 3}, last_execution = {}, current_time = 0
  * Time 0: Execute A, task_counts = {"A": 2, "B": 3}, last_execution = {"A": 0}
  * Time 1: Execute B, task_counts = {"A": 2, "B": 2}, last_execution = {"A": 0, "B": 1}
  * Time 2: No task available, be idle
  * Time 3: Execute A, task_counts = {"A": 1, "B": 2}, last_execution = {"A": 3, "B": 1}
  * Time 4: Execute B, task_counts = {"A": 1, "B": 1}, last_execution = {"A": 3, "B": 4}
  * Time 5: No task available, be idle
  * Time 6: Execute A, task_counts = {"B": 1}, last_execution = {"A": 6, "B": 4}
  * Time 7: Execute B, task_counts = {}, last_execution = {"A": 6, "B": 7}
  * Return current_time = 8
* We're repeatedly checking all tasks at each time step, which is inefficient

---

## **Why are we using this algorithm?**

* For optimization, we'll use a mathematical approach based on the frequency of tasks.
* The property that matches this problem is that the most frequent task determines the minimum time required.
* By arranging tasks in order of frequency and considering the cooldown period, we can derive a formula for the minimum time.
* Alternatives like the brute force approach would be less efficient for large inputs.
* Precondition: We need to be able to count the frequency of each task.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Mathematical Approach)**

* The key insight is that the most frequent task determines the minimum time required
* We can arrange the tasks in chunks, with the most frequent task appearing first in each chunk
* The number of chunks is determined by the frequency of the most frequent task
* The length of each chunk is determined by the cooldown period
* If there are multiple tasks with the same highest frequency, we need to account for them

### **Step 2: Flow Steps (Optimized - Mathematical Approach)**

1. Count the frequency of each task
2. Find the maximum frequency (f_max)
3. Count the number of tasks with the maximum frequency (max_count)
4. Calculate the minimum time required:
   * If there are enough tasks to fill all the idle slots: max(len(tasks), (f_max - 1) * (n + 1) + max_count)
   * Otherwise: (f_max - 1) * (n + 1) + max_count

### **Step 3: Implementation (Optimized - Mathematical Approach)**

```python
from collections import Counter

def leastInterval(tasks, n):
    task_counts = Counter(tasks)
    max_freq = max(task_counts.values())
    max_count = sum(1 for count in task_counts.values() if count == max_freq)
    
    # Calculate the minimum time required
    return max(len(tasks), (max_freq - 1) * (n + 1) + max_count)
```

### **Step 4: Complexity Analysis (Optimized - Mathematical Approach)**

* Time Complexity: O(m) where m is the total number of tasks. We need to count the frequency of each task and find the maximum frequency.
* Space Complexity: O(k) where k is the number of distinct tasks, for storing the task counts.
* This approach is much more efficient than the brute force approach, especially for large inputs.

### **Step 5: Visualizing Computation (Optimized - Mathematical Approach)**

* For the example tasks = ["A","A","A","B","B","B"], n = 2:
  * task_counts = {"A": 3, "B": 3}
  * max_freq = 3
  * max_count = 2 (both A and B have the maximum frequency)
  * minimum time = max(6, (3 - 1) * (2 + 1) + 2) = max(6, 6 + 2) = 8
* For the example tasks = ["A","A","A","B","B","B"], n = 0:
  * task_counts = {"A": 3, "B": 3}
  * max_freq = 3
  * max_count = 2
  * minimum time = max(6, (3 - 1) * (0 + 1) + 2) = max(6, 2 + 2) = 6
* For the example tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2:
  * task_counts = {"A": 6, "B": 1, "C": 1, "D": 1, "E": 1, "F": 1, "G": 1}
  * max_freq = 6
  * max_count = 1 (only A has the maximum frequency)
  * minimum time = max(12, (6 - 1) * (2 + 1) + 1) = max(12, 15 + 1) = 16
* The mathematical approach efficiently calculates the minimum time without simulating the scheduling process

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Simulation) | O(m * n) | O(k) | Inefficient for large inputs |
| Optimized (Mathematical) | O(m) | O(k) | Efficient for all inputs |

---

## **Final Takeaways**

* The key to efficiently solving the Task Scheduler problem is to recognize that the most frequent task determines the minimum time required.
* By arranging tasks in chunks and considering the cooldown period, we can derive a formula for the minimum time.
* This problem demonstrates how mathematical insights can lead to efficient solutions without simulating the entire process.

---

## **Real-life Analogy**

* Imagine you're a manager scheduling employees for shifts. Each employee can only work once every n days due to labor laws. The employees who need to work the most shifts will determine the minimum number of days needed to complete all shifts. If you have many employees who can fill in the gaps, you might be able to complete all shifts without any idle days. Otherwise, you'll need to have some idle days to respect the cooldown period for the most frequent employees.

---

## **Summary**

* We solved the Task Scheduler problem by first considering a brute force approach that simulates the scheduling process, resulting in O(m * n) time complexity. We then optimized using a mathematical approach based on the frequency of tasks, reducing the time complexity to O(m). The key insight is that the most frequent task determines the minimum time required, and we can derive a formula for the minimum time based on the maximum frequency, the cooldown period, and the number of tasks with the maximum frequency. 