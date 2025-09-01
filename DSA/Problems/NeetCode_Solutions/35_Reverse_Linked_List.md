# 📝 Reverse Linked List

## **Problem Statement**

* Given the head of a singly linked list, reverse the list, and return the reversed list.
* Example:
  * Input: head = [1,2,3,4,5]
  * Output: [5,4,3,2,1]
* Constraints:
  * The number of nodes in the list is the range [0, 5000].
  * -5000 <= Node.val <= 5000

---

## **Step 1: Thinking Process (Brute Force)**

* We need to reverse a singly linked list
* A singly linked list consists of nodes where each node has a value and a reference to the next node
* To reverse the list, we need to change the direction of the references
* One approach would be to store all the node values in an array, reverse the array, and then create a new linked list
* However, this approach uses O(n) extra space, which is not optimal

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty array
2. Traverse the linked list and store each node's value in the array
3. Reverse the array
4. Create a new linked list using the reversed array
5. Return the head of the new linked list

---

## **Step 3: Brute Force Implementation (Code)**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseList(head):
    # Store all values in an array
    values = []
    current = head
    while current:
        values.append(current.val)
        current = current.next
    
    # Reverse the array
    values.reverse()
    
    # Create a new linked list
    dummy = ListNode(0)
    current = dummy
    for val in values:
        current.next = ListNode(val)
        current = current.next
    
    return dummy.next
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the linked list. We traverse the list once to store the values, and once to create the new list.
* Space Complexity: O(n) for storing the values array and the new linked list.
* This approach is not optimal in terms of space complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For head = [1,2,3,4,5]:
  * Store values: [1,2,3,4,5]
  * Reverse values: [5,4,3,2,1]
  * Create new list: [5,4,3,2,1]
* We're creating a new linked list, which is unnecessary
* We can reverse the list in-place by changing the direction of the references

---

## **Why are we using this algorithm?**

* For optimization, we'll use an iterative in-place reversal algorithm.
* The property that matches this problem is the ability to reverse a linked list by changing the direction of the references.
* By using three pointers (prev, current, and next), we can reverse the list in-place without using extra space.
* Alternatives like creating a new list would use O(n) extra space, which is not optimal.
* Precondition: We need to be able to modify the original list.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can reverse the linked list in-place by changing the direction of the references
* We need to keep track of three nodes: the previous node, the current node, and the next node
* For each node, we change its next pointer to point to the previous node
* We then move the previous and current pointers one step forward
* We continue this process until we reach the end of the list
* The new head of the reversed list will be the last node of the original list

### **Step 2: Flow Steps (Optimized)**

1. Initialize prev = None, current = head
2. While current is not None:
   1. Save the next node: next = current.next
   2. Reverse the current node's pointer: current.next = prev
   3. Move prev and current one step forward:
      1. prev = current
      2. current = next
3. Return prev (the new head of the reversed list)

### **Step 3: Implementation (Optimized)**

```python
def reverseList(head):
    prev = None
    current = head
    
    while current:
        next_temp = current.next  # Save the next node
        current.next = prev       # Reverse the current node's pointer
        prev = current            # Move prev one step forward
        current = next_temp       # Move current one step forward
    
    return prev  # prev is the new head of the reversed list
```

### **Recursive Approach**

Another approach is to use recursion:

```python
def reverseList(head):
    # Base case: empty list or list with only one node
    if not head or not head.next:
        return head
    
    # Recursive case: reverse the rest of the list
    new_head = reverseList(head.next)
    
    # Change the next node's pointer to point to the current node
    head.next.next = head
    
    # Set the current node's next pointer to None to avoid cycles
    head.next = None
    
    return new_head
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the number of nodes in the linked list. We traverse the list once.
* Space Complexity: O(1) for the iterative approach, as we only use a constant amount of extra space. O(n) for the recursive approach due to the call stack.
* The iterative approach is more space-efficient.

### **Step 5: Visualizing Computation (Optimized)**

* For head = [1,2,3,4,5]:
  * Initialize prev = None, current = 1
  * Iteration 1:
    * next_temp = 2
    * 1.next = None
    * prev = 1
    * current = 2
  * Iteration 2:
    * next_temp = 3
    * 2.next = 1
    * prev = 2
    * current = 3
  * Iteration 3:
    * next_temp = 4
    * 3.next = 2
    * prev = 3
    * current = 4
  * Iteration 4:
    * next_temp = 5
    * 4.next = 3
    * prev = 4
    * current = 5
  * Iteration 5:
    * next_temp = None
    * 5.next = 4
    * prev = 5
    * current = None
  * Return prev = 5
* The reversed list is [5,4,3,2,1]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n) | O(n) | Creates a new linked list |
| Optimized (Algorithm: Iterative) | O(n) | O(1) | Reverses the list in-place |
| Alternative (Algorithm: Recursive) | O(n) | O(n) | Uses recursion, which requires stack space |

---

## **Final Takeaways**

* The key to reversing a linked list in-place is to change the direction of the references.
* By using three pointers (prev, current, and next), we can efficiently reverse the list without using extra space.
* This problem demonstrates how understanding the structure of the data can lead to space-efficient algorithms.

---

## **Real-life Analogy**

* Imagine you have a line of people, each holding the shoulder of the person in front of them. The brute force approach would be like taking a photo of everyone, rearranging them in reverse order on paper, and then having a new line of people stand according to the photo. The optimized approach is like having each person turn around and hold the shoulder of the person who was behind them - this way, you reverse the line in-place without needing any extra space or creating a new line.

---

## **Summary**

* We solved the "Reverse Linked List" problem by first considering a brute force approach that creates a new linked list, resulting in O(n) time and space complexity. We then optimized using an iterative in-place reversal algorithm that changes the direction of the references, reducing the space complexity to O(1) while maintaining O(n) time complexity. The key insight is to use three pointers (prev, current, and next) to efficiently reverse the list without using extra space. 