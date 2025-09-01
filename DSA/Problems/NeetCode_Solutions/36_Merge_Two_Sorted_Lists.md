# 📝 Merge Two Sorted Lists

## **Problem Statement**

* You are given the heads of two sorted linked lists `list1` and `list2`.
* Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists.
* Return the head of the merged linked list.
* Example:
  * Input: list1 = [1,2,4], list2 = [1,3,4]
  * Output: [1,1,2,3,4,4]
* Constraints:
  * The number of nodes in both lists is in the range [0, 50].
  * -100 <= Node.val <= 100
  * Both list1 and list2 are sorted in non-decreasing order.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to merge two sorted linked lists into one sorted linked list
* A straightforward approach would be to create a new linked list and add nodes from both lists in sorted order
* We can compare the current nodes of both lists and add the smaller one to the result
* We continue this process until we've processed all nodes from both lists
* This approach is actually optimal in terms of time complexity, but it uses extra space for the new list

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a dummy node to serve as the head of the result list
2. Initialize a current pointer to the dummy node
3. While both lists have nodes:
   1. Compare the values of the current nodes of both lists
   2. Add the smaller node to the result list
   3. Move the current pointer and the pointer of the list from which we took the node
4. If one list still has nodes, append them all to the result list
5. Return the next of the dummy node (the head of the merged list)

---

## **Step 3: Brute Force Implementation (Code)**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def mergeTwoLists(list1, list2):
    # Create a dummy node
    dummy = ListNode(0)
    current = dummy
    
    # Traverse both lists and add the smaller node to the result
    while list1 and list2:
        if list1.val <= list2.val:
            current.next = ListNode(list1.val)
            list1 = list1.next
        else:
            current.next = ListNode(list2.val)
            list2 = list2.next
        current = current.next
    
    # Add any remaining nodes
    while list1:
        current.next = ListNode(list1.val)
        list1 = list1.next
        current = current.next
    
    while list2:
        current.next = ListNode(list2.val)
        list2 = list2.next
        current = current.next
    
    return dummy.next
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n + m) where n and m are the lengths of the two lists. We traverse both lists once.
* Space Complexity: O(n + m) for creating the new merged list.
* This approach is optimal in terms of time complexity, but it uses extra space for the new list.

---

## **Step 5: Visualizing Redundant Computation**

* For list1 = [1,2,4] and list2 = [1,3,4]:
  * Initialize dummy = 0, current = dummy
  * Compare 1 and 1: Add 1 from list1, list1 = [2,4], current.next = 1
  * Compare 2 and 1: Add 1 from list2, list2 = [3,4], current.next = 1->1
  * Compare 2 and 3: Add 2 from list1, list1 = [4], current.next = 1->1->2
  * Compare 4 and 3: Add 3 from list2, list2 = [4], current.next = 1->1->2->3
  * Compare 4 and 4: Add 4 from list1, list1 = [], current.next = 1->1->2->3->4
  * Add remaining 4 from list2, current.next = 1->1->2->3->4->4
  * Return dummy.next = 1->1->2->3->4->4
* We're creating new nodes for each value, which is unnecessary
* We can reuse the existing nodes from both lists to avoid using extra space

---

## **Why are we using this algorithm?**

* For optimization, we'll use an in-place merging algorithm.
* The property that matches this problem is the ability to merge two sorted linked lists by rearranging the existing nodes.
* By changing the next pointers of the existing nodes, we can merge the lists without creating new nodes.
* Alternatives like creating a new list would use O(n + m) extra space, which is not optimal.
* Precondition: Both lists are sorted in non-decreasing order.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of creating new nodes, we can reuse the existing nodes from both lists
* We can still use a dummy node to simplify the code
* We compare the current nodes of both lists and link the smaller one to the result
* We continue this process until we've processed all nodes from both lists
* This approach is optimal in terms of both time and space complexity

### **Step 2: Flow Steps (Optimized)**

1. Create a dummy node to serve as the head of the result list
2. Initialize a current pointer to the dummy node
3. While both lists have nodes:
   1. Compare the values of the current nodes of both lists
   2. Link the smaller node to the result list
   3. Move the current pointer and the pointer of the list from which we took the node
4. If one list still has nodes, link them all to the result list
5. Return the next of the dummy node (the head of the merged list)

### **Step 3: Implementation (Optimized)**

```python
def mergeTwoLists(list1, list2):
    # Create a dummy node
    dummy = ListNode(0)
    current = dummy
    
    # Traverse both lists and link the smaller node to the result
    while list1 and list2:
        if list1.val <= list2.val:
            current.next = list1
            list1 = list1.next
        else:
            current.next = list2
            list2 = list2.next
        current = current.next
    
    # Link any remaining nodes
    if list1:
        current.next = list1
    else:
        current.next = list2
    
    return dummy.next
```

### **Recursive Approach**

Another approach is to use recursion:

```python
def mergeTwoLists(list1, list2):
    # Base cases
    if not list1:
        return list2
    if not list2:
        return list1
    
    # Recursive case
    if list1.val <= list2.val:
        list1.next = mergeTwoLists(list1.next, list2)
        return list1
    else:
        list2.next = mergeTwoLists(list1, list2.next)
        return list2
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n + m) where n and m are the lengths of the two lists. We traverse both lists once.
* Space Complexity: O(1) for the iterative approach, as we only use a constant amount of extra space. O(n + m) for the recursive approach due to the call stack.
* The iterative approach is more space-efficient.

### **Step 5: Visualizing Computation (Optimized)**

* For list1 = [1,2,4] and list2 = [1,3,4]:
  * Initialize dummy = 0, current = dummy
  * Compare 1 and 1: Link 1 from list1, list1 = [2,4], current.next = 1
  * Compare 2 and 1: Link 1 from list2, list2 = [3,4], current.next = 1->1
  * Compare 2 and 3: Link 2 from list1, list1 = [4], current.next = 1->1->2
  * Compare 4 and 3: Link 3 from list2, list2 = [4], current.next = 1->1->2->3
  * Compare 4 and 4: Link 4 from list1, list1 = [], current.next = 1->1->2->3->4
  * Link remaining 4 from list2, current.next = 1->1->2->3->4->4
  * Return dummy.next = 1->1->2->3->4->4
* We're reusing the existing nodes from both lists, which is more space-efficient.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n + m) | O(n + m) | Creates new nodes for the merged list |
| Optimized (Algorithm: Iterative) | O(n + m) | O(1) | Reuses existing nodes |
| Alternative (Algorithm: Recursive) | O(n + m) | O(n + m) | Uses recursion, which requires stack space |

---

## **Final Takeaways**

* The key to merging two sorted linked lists in-place is to rearrange the existing nodes by changing their next pointers.
* By using a dummy node and a current pointer, we can simplify the code and handle edge cases elegantly.
* This problem demonstrates how understanding the structure of the data can lead to space-efficient algorithms.

---

## **Real-life Analogy**

* Imagine you have two sorted stacks of books, and you want to merge them into a single sorted stack. The brute force approach would be like reading the titles of all the books, writing them down in sorted order, and then creating a new stack of books based on that list. The optimized approach is like comparing the top books from both stacks and moving the smaller one to a new stack, without having to create copies of the books - this way, you reuse the existing books and save space.

---

## **Summary**

* We solved the "Merge Two Sorted Lists" problem by first considering an approach that creates new nodes for the merged list, resulting in O(n + m) time and space complexity. We then optimized using an in-place merging algorithm that reuses the existing nodes, reducing the space complexity to O(1) while maintaining O(n + m) time complexity. The key insight is to rearrange the existing nodes by changing their next pointers, which allows us to merge the lists without using extra space. 