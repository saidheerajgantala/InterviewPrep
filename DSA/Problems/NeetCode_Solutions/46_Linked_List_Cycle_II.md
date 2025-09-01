# 📝 Linked List Cycle II

## **Problem Statement**

* Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.
* There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to (0-indexed). It is -1 if there is no cycle. Note that pos is not passed as a parameter.
* Do not modify the linked list.
* Example:
  * Input: head = [3,2,0,-4], pos = 1
  * Output: tail connects to node index 1
  * Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
* Constraints:
  * The number of the nodes in the list is in the range [0, 10^4].
  * -10^5 <= Node.val <= 10^5
  * pos is -1 or a valid index in the linked-list.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the node where the cycle begins in a linked list
* One approach would be to keep track of all nodes we've visited
* As we traverse the list, we check if the current node has been visited before
* If we encounter a node that we've already visited, then that node is the start of the cycle
* If we reach the end of the list (null), then there is no cycle

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a hash set to store visited nodes
2. Initialize a current pointer to the head of the list
3. While current is not null:
   1. If current is in the hash set, return current (cycle detected)
   2. Add current to the hash set
   3. Move current to the next node
4. Return null (no cycle detected)

---

## **Step 3: Brute Force Implementation (Code)**

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def detectCycle(head):
    visited = set()
    current = head
    
    while current:
        if current in visited:
            return current
        visited.add(current)
        current = current.next
    
    return None
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the linked list. We traverse the list once.
* Space Complexity: O(n) for storing the visited nodes in the hash set.
* This approach is efficient in terms of time complexity, but it uses extra space.

---

## **Step 5: Visualizing Redundant Computation**

* For head = [3,2,0,-4], pos = 1:
  * Initialize visited = {}, current = 3
  * Iteration 1: current = 3, visited = {3}
  * Iteration 2: current = 2, visited = {3, 2}
  * Iteration 3: current = 0, visited = {3, 2, 0}
  * Iteration 4: current = -4, visited = {3, 2, 0, -4}
  * Iteration 5: current = 2, 2 is in visited, return 2
* We're using extra space to store all visited nodes
* We can optimize this by using a more space-efficient approach

---

## **Why are we using this algorithm?**

* For optimization, we'll use Floyd's Cycle-Finding Algorithm (Tortoise and Hare).
* The property that matches this problem is the ability to find the start of a cycle in a linked list without using extra space.
* By using two pointers that move at different speeds, we can determine if there is a cycle and find its starting point.
* Alternatives like using a hash set would use O(n) extra space, which is not optimal.
* Precondition: We need to be able to traverse the linked list.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can use Floyd's Cycle-Finding Algorithm (also known as the "tortoise and hare" algorithm)
* The algorithm consists of two phases:
  1. Detect if there is a cycle using two pointers (slow and fast)
  2. Find the start of the cycle
* In the first phase, the slow pointer moves one step at a time, while the fast pointer moves two steps at a time
* If there is a cycle, the fast pointer will eventually catch up to the slow pointer
* In the second phase, we reset one pointer to the head and move both pointers one step at a time
* The point where they meet is the start of the cycle

### **Step 2: Flow Steps (Optimized)**

1. Initialize two pointers, slow and fast, to the head of the list
2. Phase 1: Detect if there is a cycle
   1. While fast is not null and fast.next is not null:
      1. Move slow one step forward: slow = slow.next
      2. Move fast two steps forward: fast = fast.next.next
      3. If slow and fast are the same node, break the loop (cycle detected)
   2. If fast is null or fast.next is null, return null (no cycle)
3. Phase 2: Find the start of the cycle
   1. Reset slow to the head of the list
   2. While slow is not equal to fast:
      1. Move slow one step forward: slow = slow.next
      2. Move fast one step forward: fast = fast.next
   3. Return slow (the start of the cycle)

### **Step 3: Implementation (Optimized)**

```python
def detectCycle(head):
    if not head or not head.next:
        return None
    
    # Phase 1: Detect if there is a cycle
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            # Cycle detected, move to phase 2
            break
    
    # If we reached the end of the list, there is no cycle
    if not fast or not fast.next:
        return None
    
    # Phase 2: Find the start of the cycle
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next
    
    return slow
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the number of nodes in the linked list. In the worst case, we traverse the list twice: once to detect the cycle and once to find its start.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is optimal in terms of both time and space complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For head = [3,2,0,-4], pos = 1:
  * Initialize slow = 3, fast = 3
  * Phase 1: Detect if there is a cycle
    * Iteration 1: slow = 2, fast = 0
    * Iteration 2: slow = 0, fast = 2
    * Iteration 3: slow = -4, fast = -4, slow == fast, break
  * Phase 2: Find the start of the cycle
    * Reset slow = 3, keep fast = -4
    * Iteration 1: slow = 2, fast = 2, slow == fast, return 2
* We're efficiently finding the start of the cycle without using extra space.

### **Mathematical Explanation**

Let's denote:
- The distance from the head to the start of the cycle as x
- The distance from the start of the cycle to the meeting point as y
- The length of the cycle as c

When the slow and fast pointers meet:
- The slow pointer has traveled x + y steps
- The fast pointer has traveled x + y + k*c steps, where k is some integer
- Since the fast pointer travels twice as fast as the slow pointer, we have: 2(x + y) = x + y + k*c
- Simplifying: x + y = k*c
- This means: x = k*c - y = (k-1)*c + (c-y)

Now, if we reset the slow pointer to the head and move both pointers one step at a time, they will meet at the start of the cycle after x steps:
- The slow pointer will be at distance x from the head, which is the start of the cycle
- The fast pointer will be at distance x from the meeting point, which is also the start of the cycle

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Hash Set) | O(n) | O(n) | Uses a hash set to store visited nodes |
| Optimized (Algorithm: Floyd's Cycle-Finding) | O(n) | O(1) | Uses slow and fast pointers |

---

## **Final Takeaways**

* The key to finding the start of a cycle in a linked list without using extra space is to use Floyd's Cycle-Finding Algorithm.
* The algorithm consists of two phases: detecting if there is a cycle and finding its start.
* By understanding the mathematical relationship between the distances traveled by the slow and fast pointers, we can efficiently find the start of the cycle.
* This problem demonstrates how mathematical insights can lead to space-efficient algorithms.

---

## **Real-life Analogy**

* Imagine two runners on a circular track, one running twice as fast as the other. If they start at the same position, the faster runner will eventually lap the slower runner if they're on a circular track (a cycle). The point where they meet is not necessarily the start of the track, but it gives us information about the structure of the track. By resetting one runner to the starting position and having both run at the same speed, they will meet at the start of the circular portion of the track.

---

## **Summary**

* We solved the "Linked List Cycle II" problem by first considering a hash set approach that stores visited nodes, resulting in O(n) time and space complexity. We then optimized using Floyd's Cycle-Finding Algorithm (the "tortoise and hare" algorithm) with two phases: detecting if there is a cycle and finding its start. This approach reduces the space complexity to O(1) while maintaining O(n) time complexity. The key insight is to understand the mathematical relationship between the distances traveled by the slow and fast pointers, which allows us to find the start of the cycle without using extra space. 