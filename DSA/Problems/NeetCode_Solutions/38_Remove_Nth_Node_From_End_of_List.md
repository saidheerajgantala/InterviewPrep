# 📝 Remove Nth Node From End of List

## **Problem Statement**

* Given the head of a linked list, remove the nth node from the end of the list and return its head.
* Example:
  * Input: head = [1,2,3,4,5], n = 2
  * Output: [1,2,3,5]
  * Explanation: The 2nd node from the end is node with value 4, which is removed.
* Constraints:
  * The number of nodes in the list is sz.
  * 1 <= sz <= 30
  * 0 <= Node.val <= 100
  * 1 <= n <= sz

---

## **Step 1: Thinking Process (Brute Force)**

* We need to remove the nth node from the end of a linked list
* One approach would be to first count the total number of nodes in the list
* Then, we can calculate the position of the node to be removed from the beginning of the list
* Finally, we traverse the list again to remove that node
* This approach requires two passes through the list

---

## **Step 2: Flow Steps (Brute Force)**

1. Count the total number of nodes in the list (length)
2. Calculate the position of the node to be removed from the beginning: position = length - n
3. Traverse the list to the (position-1)th node (the node before the one to be removed)
4. Remove the nth node from the end by updating the next pointer of the (position-1)th node
5. Return the head of the modified list

---

## **Step 3: Brute Force Implementation (Code)**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def removeNthFromEnd(head, n):
    # Count the total number of nodes
    length = 0
    current = head
    while current:
        length += 1
        current = current.next
    
    # Calculate the position from the beginning
    position = length - n
    
    # Handle the case where the head node is to be removed
    if position == 0:
        return head.next
    
    # Traverse to the node before the one to be removed
    current = head
    for _ in range(position - 1):
        current = current.next
    
    # Remove the nth node from the end
    current.next = current.next.next
    
    return head
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(L) where L is the length of the linked list. We traverse the list twice: once to count the nodes and once to remove the nth node.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach requires two passes through the list, which is not optimal.

---

## **Step 5: Visualizing Redundant Computation**

* For head = [1,2,3,4,5] and n = 2:
  * Count nodes: length = 5
  * Calculate position: position = 5 - 2 = 3
  * Traverse to the node before the one to be removed: current = 3
  * Remove the nth node: 3.next = 5
  * Result: [1,2,3,5]
* We're traversing the list twice, which is not necessary
* We can optimize this by using a one-pass approach

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Two-Pointer approach.
* The property that matches this problem is the ability to find a node that is n positions away from another node in a linked list.
* By using two pointers with a gap of n nodes between them, we can find the nth node from the end in a single pass.
* Alternatives like the two-pass approach would be less efficient.
* Precondition: The linked list has at least n nodes.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can solve this problem in one pass using two pointers: first and second
* The idea is to maintain a gap of n nodes between the two pointers
* We first move the first pointer n nodes ahead
* Then, we move both pointers one step at a time until the first pointer reaches the end of the list
* At this point, the second pointer will be at the node just before the one to be removed
* We can then remove the nth node from the end by updating the next pointer of the second pointer

### **Step 2: Flow Steps (Optimized)**

1. Create a dummy node and set its next pointer to the head of the list
2. Initialize two pointers, first and second, to the dummy node
3. Move the first pointer n+1 steps ahead
4. Move both pointers one step at a time until the first pointer reaches the end of the list
5. Remove the nth node from the end by updating the next pointer of the second pointer
6. Return the next of the dummy node (the head of the modified list)

### **Step 3: Implementation (Optimized)**

```python
def removeNthFromEnd(head, n):
    # Create a dummy node
    dummy = ListNode(0)
    dummy.next = head
    
    # Initialize two pointers
    first = dummy
    second = dummy
    
    # Move the first pointer n+1 steps ahead
    for _ in range(n + 1):
        first = first.next
    
    # Move both pointers until the first pointer reaches the end
    while first:
        first = first.next
        second = second.next
    
    # Remove the nth node from the end
    second.next = second.next.next
    
    return dummy.next
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(L) where L is the length of the linked list. We traverse the list once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is optimal in terms of both time and space complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For head = [1,2,3,4,5] and n = 2:
  * Initialize dummy.next = 1, first = dummy, second = dummy
  * Move first 3 steps ahead: first = 3
  * Move both pointers until first reaches the end:
    * first = 4, second = 1
    * first = 5, second = 2
    * first = None, second = 3
  * Remove the nth node: 3.next = 5
  * Result: [1,2,3,5]
* We're efficiently removing the nth node from the end in a single pass.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Two-Pass) | O(L) | O(1) | Counts nodes first, then removes the nth node |
| Optimized (Algorithm: Two-Pointer) | O(L) | O(1) | Uses two pointers with a gap of n nodes |

---

## **Final Takeaways**

* The key to removing the nth node from the end of a linked list in one pass is to use two pointers with a gap of n nodes between them.
* By using a dummy node, we can simplify the code and handle edge cases elegantly, such as removing the head node.
* This problem demonstrates how the two-pointer technique can be used to solve linked list problems efficiently.

---

## **Real-life Analogy**

* Imagine you're walking along a line of people and you want to remove the nth person from the end. The brute force approach would be like counting all the people first, then walking back to the appropriate position to remove that person. The optimized approach is like having two people walk along the line, with the second person starting n positions behind the first. When the first person reaches the end of the line, the second person will be exactly at the position where you need to remove someone - this way, you only need to walk through the line once.

---

## **Summary**

* We solved the "Remove Nth Node From End of List" problem by first considering a two-pass approach that counts the total number of nodes and then removes the nth node, resulting in O(L) time complexity. We then optimized using a one-pass approach with two pointers, maintaining a gap of n nodes between them, which still has O(L) time complexity but is more efficient as it only requires a single pass through the list. The key insight is to use a dummy node and two pointers to efficiently locate and remove the nth node from the end in a single pass. 