# 📝 Reorder List

## **Problem Statement**

* You are given the head of a singly linked list. The list can be represented as: L0 → L1 → … → Ln - 1 → Ln
* Reorder the list to be: L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …
* You may not modify the values in the list's nodes. Only nodes themselves may be changed.
* Example:
  * Input: head = [1,2,3,4]
  * Output: [1,4,2,3]
* Constraints:
  * The number of nodes in the list is in the range [1, 5 * 10^4].
  * 1 <= Node.val <= 1000

---

## **Step 1: Thinking Process (Brute Force)**

* We need to reorder a linked list such that the first node is followed by the last node, then the second node, then the second-to-last node, and so on
* One approach would be to store all nodes in an array, and then reorder the list by changing the next pointers
* This approach uses O(n) extra space, but it's a good starting point

---

## **Step 2: Flow Steps (Brute Force)**

1. Traverse the linked list and store all nodes in an array
2. Initialize a pointer to the head of the list
3. For each pair of nodes (one from the beginning, one from the end):
   1. Set the next pointer of the current node to the node from the end
   2. Set the next pointer of the node from the end to the next node from the beginning
   3. Move the current pointer to the next node from the beginning
4. Set the next pointer of the last node to null

---

## **Step 3: Brute Force Implementation (Code)**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reorderList(head):
    if not head or not head.next:
        return
    
    # Store all nodes in an array
    nodes = []
    current = head
    while current:
        nodes.append(current)
        current = current.next
    
    # Reorder the list
    n = len(nodes)
    for i in range(n // 2):
        # Connect the i-th node to the (n-1-i)-th node
        nodes[i].next = nodes[n-1-i]
        
        # Connect the (n-1-i)-th node to the (i+1)-th node
        if i < n // 2 - 1 or n % 2 == 1:
            nodes[n-1-i].next = nodes[i+1]
    
    # Set the next pointer of the last node to null
    if n % 2 == 0:
        nodes[n//2].next = None
    else:
        nodes[n//2+1].next = None
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the linked list. We traverse the list once to store the nodes and once to reorder them.
* Space Complexity: O(n) for storing all nodes in the array.
* This approach is not optimal in terms of space complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For head = [1,2,3,4]:
  * Store nodes: [1,2,3,4]
  * Reorder:
    * nodes[0].next = nodes[3], nodes[3].next = nodes[1]
    * nodes[1].next = nodes[2], nodes[2].next = None
  * Result: 1 -> 4 -> 2 -> 3 -> None
* We're using extra space to store all nodes, which is not necessary
* We can optimize this by using a more space-efficient approach

---

## **Why are we using this algorithm?**

* For optimization, we'll use a three-step approach:
  1. Find the middle of the linked list
  2. Reverse the second half of the linked list
  3. Merge the two halves
* This approach uses O(1) extra space and is more efficient.
* Alternatives like storing all nodes in an array would use O(n) extra space, which is not optimal.
* Precondition: We need to be able to modify the next pointers of the nodes.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can solve this problem in three steps:
  1. Find the middle of the linked list using the slow and fast pointer technique
  2. Reverse the second half of the linked list
  3. Merge the two halves by alternating nodes
* This approach uses O(1) extra space and is more efficient

### **Step 2: Flow Steps (Optimized)**

1. Find the middle of the linked list:
   1. Initialize slow and fast pointers to the head
   2. Move slow one step and fast two steps at a time
   3. When fast reaches the end, slow is at the middle
2. Reverse the second half of the linked list:
   1. Initialize prev = None, current = slow
   2. While current is not None:
      1. Save the next node: next_temp = current.next
      2. Reverse the current node's pointer: current.next = prev
      3. Move prev and current one step forward:
         1. prev = current
         2. current = next_temp
3. Merge the two halves:
   1. Initialize first = head, second = prev
   2. While second.next is not None:
      1. Save the next nodes: first_next = first.next, second_next = second.next
      2. Connect first to second: first.next = second
      3. Connect second to first_next: second.next = first_next
      4. Move first and second to their next positions: first = first_next, second = second_next

### **Step 3: Implementation (Optimized)**

```python
def reorderList(head):
    if not head or not head.next:
        return
    
    # Step 1: Find the middle of the linked list
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    # Step 2: Reverse the second half of the linked list
    prev = None
    current = slow
    while current:
        next_temp = current.next
        current.next = prev
        prev = current
        current = next_temp
    
    # Step 3: Merge the two halves
    first = head
    second = prev
    while second.next:
        first_next = first.next
        second_next = second.next
        
        first.next = second
        second.next = first_next
        
        first = first_next
        second = second_next
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the number of nodes in the linked list. We traverse the list once to find the middle, once to reverse the second half, and once to merge the two halves.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is optimal in terms of both time and space complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For head = [1,2,3,4]:
  * Find the middle: slow = 3
  * Reverse the second half: [1,2] and [4,3]
  * Merge the two halves:
    * first = 1, second = 4
    * first.next = 4, second.next = 2
    * first = 2, second = 3
    * first.next = 3, second.next = None
  * Result: 1 -> 4 -> 2 -> 3 -> None
* We're efficiently reordering the list without using extra space.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n) | O(n) | Stores all nodes in an array |
| Optimized (Algorithm: Three-step) | O(n) | O(1) | Uses slow/fast pointers, reversal, and merging |

---

## **Final Takeaways**

* The key to reordering a linked list in-place is to break it down into simpler operations: finding the middle, reversing the second half, and merging the two halves.
* By using the slow and fast pointer technique, we can efficiently find the middle of a linked list in one pass.
* This problem demonstrates how combining multiple linked list operations can solve a complex problem efficiently.

---

## **Real-life Analogy**

* Imagine you have a line of people and you want to reorder them so that the first person is followed by the last person, then the second person, then the second-to-last person, and so on. The brute force approach would be like taking a photo of everyone, writing down their positions, and then asking them to rearrange themselves according to the new order. The optimized approach is like splitting the line in the middle, reversing the second half, and then merging the two halves by alternating people from each half - this way, you reorder the line without needing any extra space or reference.

---

## **Summary**

* We solved the "Reorder List" problem by first considering a brute force approach that stores all nodes in an array, resulting in O(n) time and space complexity. We then optimized using a three-step approach: finding the middle of the linked list using slow and fast pointers, reversing the second half of the linked list, and merging the two halves by alternating nodes. This approach reduces the space complexity to O(1) while maintaining O(n) time complexity. The key insight is to break down the complex reordering operation into simpler linked list operations that can be performed in-place. 