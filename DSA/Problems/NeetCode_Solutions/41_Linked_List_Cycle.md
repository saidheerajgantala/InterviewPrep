# 📝 Linked List Cycle

## **Problem Statement**

* Given head, the head of a linked list, determine if the linked list has a cycle in it.
* There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.
* Return true if there is a cycle in the linked list. Otherwise, return false.
* Example:
  * Input: head = [3,2,0,-4], pos = 1
  * Output: true
  * Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
* Constraints:
  * The number of the nodes in the list is in the range [0, 10^4].
  * -10^5 <= Node.val <= 10^5
  * pos is -1 or a valid index in the linked-list.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine if a linked list has a cycle
* One approach would be to keep track of all nodes we've visited
* As we traverse the list, we check if the current node has been visited before
* If we encounter a node that we've already visited, then there is a cycle
* If we reach the end of the list (null), then there is no cycle

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a hash set to store visited nodes
2. Initialize a current pointer to the head of the list
3. While current is not null:
   1. If current is in the hash set, return true (cycle detected)
   2. Add current to the hash set
   3. Move current to the next node
4. Return false (no cycle detected)

---

## **Step 3: Brute Force Implementation (Code)**

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def hasCycle(head):
    visited = set()
    current = head
    
    while current:
        if current in visited:
            return True
        visited.add(current)
        current = current.next
    
    return False
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
  * Iteration 5: current = 2, 2 is in visited, return true
* We're using extra space to store all visited nodes
* We can optimize this by using a more space-efficient approach

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Two-Pointer approach (Floyd's Cycle-Finding Algorithm).
* The property that matches this problem is the ability to detect a cycle in a linked list without using extra space.
* By using two pointers that move at different speeds, we can determine if there is a cycle in the list.
* Alternatives like using a hash set would use O(n) extra space, which is not optimal.
* Precondition: We need to be able to traverse the linked list.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can use Floyd's Cycle-Finding Algorithm (also known as the "tortoise and hare" algorithm)
* The idea is to use two pointers, slow and fast, that move at different speeds
* The slow pointer moves one step at a time, while the fast pointer moves two steps at a time
* If there is a cycle, the fast pointer will eventually catch up to the slow pointer
* If there is no cycle, the fast pointer will reach the end of the list

### **Step 2: Flow Steps (Optimized)**

1. Initialize two pointers, slow and fast, to the head of the list
2. While fast is not null and fast.next is not null:
   1. Move slow one step forward: slow = slow.next
   2. Move fast two steps forward: fast = fast.next.next
   3. If slow and fast are the same node, return true (cycle detected)
3. Return false (no cycle detected)

### **Step 3: Implementation (Optimized)**

```python
def hasCycle(head):
    if not head or not head.next:
        return False
    
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            return True
    
    return False
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the number of nodes in the linked list. In the worst case, the slow pointer will traverse the entire list once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is optimal in terms of both time and space complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For head = [3,2,0,-4], pos = 1:
  * Initialize slow = 3, fast = 3
  * Iteration 1: slow = 2, fast = 0
  * Iteration 2: slow = 0, fast = 2
  * Iteration 3: slow = -4, fast = -4, slow == fast, return true
* We're efficiently detecting the cycle without using extra space.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Hash Set) | O(n) | O(n) | Uses a hash set to store visited nodes |
| Optimized (Algorithm: Two-Pointer) | O(n) | O(1) | Uses slow and fast pointers |

---

## **Final Takeaways**

* The key to detecting a cycle in a linked list without using extra space is to use two pointers that move at different speeds.
* If there is a cycle, the fast pointer will eventually catch up to the slow pointer.
* This problem demonstrates how the two-pointer technique can be used to solve linked list problems efficiently.

---

## **Real-life Analogy**

* Imagine two runners on a circular track, one running twice as fast as the other. If they start at the same position, the faster runner will eventually lap the slower runner if they're on a circular track (a cycle). If the track is not circular (no cycle), the faster runner will reach the end of the track and the race will be over without them meeting again.

---

## **Summary**

* We solved the "Linked List Cycle" problem by first considering a hash set approach that stores visited nodes, resulting in O(n) time and space complexity. We then optimized using Floyd's Cycle-Finding Algorithm (the "tortoise and hare" algorithm) with two pointers moving at different speeds, reducing the space complexity to O(1) while maintaining O(n) time complexity. The key insight is that if there is a cycle, the fast pointer will eventually catch up to the slow pointer, allowing us to detect the cycle without using extra space. 