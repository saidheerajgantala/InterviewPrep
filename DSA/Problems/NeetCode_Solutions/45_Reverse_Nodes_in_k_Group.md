# 📝 Reverse Nodes in k-Group

## **Problem Statement**

* Given the head of a linked list, reverse the nodes of the list k at a time, and return the modified list.
* k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should remain as it is.
* You may not alter the values in the list's nodes, only nodes themselves may be changed.
* Example:
  * Input: head = [1,2,3,4,5], k = 2
  * Output: [2,1,4,3,5]
  * Explanation: First pair (1,2) is reversed, then pair (3,4), and 5 remains as is since we have an odd number of elements.
* Constraints:
  * The number of nodes in the list is n.
  * 1 <= k <= n <= 5000
  * 0 <= Node.val <= 1000

---

## **Step 1: Thinking Process (Brute Force)**

* We need to reverse the linked list k nodes at a time
* One approach would be to:
  1. Count the total number of nodes in the list
  2. Determine how many complete groups of k nodes we have
  3. Reverse each group of k nodes
  4. Leave the remaining nodes (if any) as they are
* This approach is straightforward but requires two passes through the list

---

## **Step 2: Flow Steps (Brute Force)**

1. Count the total number of nodes in the list
2. Calculate the number of complete groups: count / k
3. Initialize a dummy node as the new head
4. For each complete group:
   1. Reverse k nodes
   2. Connect the reversed group to the previous group
5. Return the next of the dummy node (the new head)

---

## **Step 3: Brute Force Implementation (Code)**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseKGroup(head, k):
    if not head or k == 1:
        return head
    
    # Count the total number of nodes
    count = 0
    current = head
    while current:
        count += 1
        current = current.next
    
    # Calculate the number of complete groups
    groups = count // k
    
    dummy = ListNode(0)
    dummy.next = head
    prev_group_end = dummy
    
    # Process each complete group
    for _ in range(groups):
        group_start = prev_group_end.next
        group_end = group_start
        
        # Move group_end to the end of the current group
        for _ in range(k - 1):
            group_end = group_end.next
        
        # Save the start of the next group
        next_group_start = group_end.next
        
        # Reverse the current group
        prev = next_group_start
        current = group_start
        for _ in range(k):
            next_temp = current.next
            current.next = prev
            prev = current
            current = next_temp
        
        # Connect the reversed group to the previous group
        prev_group_end.next = group_end
        prev_group_end = group_start
    
    return dummy.next
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the linked list. We traverse the list twice: once to count the nodes and once to reverse the groups.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is efficient in terms of space complexity, but it requires two passes through the list.

---

## **Step 5: Visualizing Redundant Computation**

* For head = [1,2,3,4,5] and k = 2:
  * Count nodes: 5
  * Calculate groups: 5 / 2 = 2 complete groups
  * First group: Reverse [1,2] to [2,1]
  * Second group: Reverse [3,4] to [4,3]
  * Result: [2,1,4,3,5]
* We're traversing the list twice, which is not necessary
* We can optimize this by reversing the groups in a single pass

---

## **Why are we using this algorithm?**

* For optimization, we'll use a One-Pass Reversal approach.
* The property that matches this problem is the ability to reverse nodes in groups without counting the total number of nodes first.
* By checking if there are enough nodes for a complete group before reversing, we can process the list in a single pass.
* Alternatives like the two-pass approach would be less efficient.
* Precondition: We need to be able to modify the nodes themselves, not just their values.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can optimize the solution by reversing the groups in a single pass
* Before reversing a group, we check if there are enough nodes remaining for a complete group
* If not, we leave the remaining nodes as they are
* This approach avoids the need to count the total number of nodes first

### **Step 2: Flow Steps (Optimized)**

1. Initialize a dummy node as the new head
2. Initialize prev_group_end = dummy
3. While there are nodes remaining:
   1. Check if there are at least k nodes remaining
   2. If not, break the loop
   3. Reverse k nodes
   4. Connect the reversed group to the previous group
   5. Update prev_group_end to the end of the current group
4. Return the next of the dummy node (the new head)

### **Step 3: Implementation (Optimized)**

```python
def reverseKGroup(head, k):
    if not head or k == 1:
        return head
    
    dummy = ListNode(0)
    dummy.next = head
    prev_group_end = dummy
    
    while True:
        # Check if there are at least k nodes remaining
        group_start = prev_group_end.next
        group_end = group_start
        for _ in range(k - 1):
            if not group_end or not group_end.next:
                return dummy.next
            group_end = group_end.next
        
        # Save the start of the next group
        next_group_start = group_end.next
        
        # Reverse the current group
        prev = next_group_start
        current = group_start
        for _ in range(k):
            next_temp = current.next
            current.next = prev
            prev = current
            current = next_temp
        
        # Connect the reversed group to the previous group
        prev_group_end.next = group_end
        prev_group_end = group_start
    
    return dummy.next
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the number of nodes in the linked list. We traverse the list once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is optimal in terms of both time and space complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For head = [1,2,3,4,5] and k = 2:
  * Initialize dummy.next = 1, prev_group_end = dummy
  * First group: Check if there are at least 2 nodes remaining (yes)
    * group_start = 1, group_end = 2
    * next_group_start = 3
    * Reverse [1,2] to [2,1]
    * Connect: dummy.next = 2, 1.next = 3
    * prev_group_end = 1
  * Second group: Check if there are at least 2 nodes remaining (yes)
    * group_start = 3, group_end = 4
    * next_group_start = 5
    * Reverse [3,4] to [4,3]
    * Connect: 1.next = 4, 3.next = 5
    * prev_group_end = 3
  * Third group: Check if there are at least 2 nodes remaining (no)
    * Return dummy.next = 2
  * Result: [2,1,4,3,5]
* We're efficiently reversing the groups in a single pass.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Two-Pass) | O(n) | O(1) | Counts nodes first, then reverses groups |
| Optimized (Algorithm: One-Pass) | O(n) | O(1) | Checks and reverses groups in a single pass |

---

## **Final Takeaways**

* The key to efficiently reversing nodes in k-groups is to check if there are enough nodes for a complete group before reversing.
* By using a dummy node and carefully updating the connections between groups, we can handle the reversal in a single pass.
* This problem demonstrates how understanding the structure of the linked list can lead to efficient algorithms.

---

## **Real-life Analogy**

* Imagine you have a line of people and you want to reverse the order of every k people. The brute force approach would be like first counting how many people are in the line, then calculating how many complete groups of k people you have, and finally reversing each group. The optimized approach is like starting from the beginning of the line, checking if there are at least k people remaining, and if so, reversing their order - this way, you only need to walk through the line once.

---

## **Summary**

* We solved the "Reverse Nodes in k-Group" problem by first considering a two-pass approach that counts the total number of nodes and then reverses the complete groups, resulting in O(n) time complexity. We then optimized using a one-pass approach that checks if there are enough nodes for a complete group before reversing, maintaining O(n) time complexity but improving the efficiency by avoiding the need to count the total number of nodes first. The key insight is to use a dummy node and carefully update the connections between groups, which allows us to handle the reversal in a single pass. 