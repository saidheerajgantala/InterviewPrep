# 📝 Copy List with Random Pointer

## **Problem Statement**

* A linked list of length n is given such that each node contains an additional random pointer, which could point to any node in the list, or null.
* Construct a deep copy of the list. The deep copy should consist of exactly n brand new nodes, where each new node has its value set to the value of its corresponding original node. Both the next and random pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. None of the pointers in the new list should point to nodes in the original list.
* For example, if there are two nodes X and Y in the original list, where X.random --> Y, then for the corresponding two nodes x and y in the copied list, x.random --> y.
* Return the head of the copied linked list.
* Example:
  * Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
  * Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
  * Explanation: The linked list has 5 nodes. The 1st node (val = 7)'s random pointer points to null. The 2nd node (val = 13)'s random pointer points to the 1st node. The 3rd node (val = 11)'s random pointer points to the 5th node. The 4th node (val = 10)'s random pointer points to the 3rd node. The 5th node (val = 1)'s random pointer points to the 1st node.
* Constraints:
  * 0 <= n <= 1000
  * -10^4 <= Node.val <= 10^4
  * Node.random is null or is pointing to some node in the linked list.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to create a deep copy of a linked list with random pointers
* The challenge is to correctly set the random pointers in the copied list
* A straightforward approach would be to:
  1. Create a copy of each node without setting the pointers
  2. Use a hash map to map original nodes to their copies
  3. Iterate through the original list again to set the next and random pointers in the copied list

---

## **Step 2: Flow Steps (Brute Force)**

1. Create a hash map to map original nodes to their copies
2. First pass: Create a copy of each node and store the mapping
3. Second pass: Set the next and random pointers for each copied node using the hash map
4. Return the head of the copied list

---

## **Step 3: Brute Force Implementation (Code)**

```python
class Node:
    def __init__(self, x, next=None, random=None):
        self.val = int(x)
        self.next = next
        self.random = random

def copyRandomList(head):
    if not head:
        return None
    
    # Hash map to store the mapping from original nodes to their copies
    node_map = {}
    
    # First pass: Create a copy of each node and store the mapping
    current = head
    while current:
        node_map[current] = Node(current.val)
        current = current.next
    
    # Second pass: Set the next and random pointers for each copied node
    current = head
    while current:
        # Set the next pointer
        if current.next:
            node_map[current].next = node_map[current.next]
        
        # Set the random pointer
        if current.random:
            node_map[current].random = node_map[current.random]
        
        current = current.next
    
    return node_map[head]
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the linked list. We traverse the list twice, once to create the copies and once to set the pointers.
* Space Complexity: O(n) for the hash map that stores the mapping from original nodes to their copies.
* This approach is actually quite efficient, but there are ways to optimize the space complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For the example input:
  * First pass: Create copies of nodes 7, 13, 11, 10, 1 and store the mapping
  * Second pass: Set the next and random pointers for each copied node
* We're traversing the list twice, which is efficient in terms of time complexity
* However, we're using extra space for the hash map
* We can optimize the space complexity by using an interweaving approach

---

## **Why are we using this algorithm?**

* For optimization, we'll use an Interweaving approach.
* The property that matches this problem is the ability to create a deep copy of a linked list with random pointers without using extra space.
* By interweaving the original and copied nodes, we can establish the relationships between them without using a hash map.
* Alternatives like using a hash map would use O(n) extra space, which is not optimal.
* Precondition: We need to be able to modify the original list temporarily.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can optimize the space complexity by using an interweaving approach
* The idea is to create a copy of each node and insert it right after the original node
* This way, for each original node, its copy is the next node
* Once we have this interweaved list, we can set the random pointers for the copied nodes
* Finally, we separate the original and copied lists

### **Step 2: Flow Steps (Optimized)**

1. First pass: Create a copy of each node and insert it right after the original node
   * For each node A, create a copy A' and insert it between A and A.next
   * The resulting list will be A -> A' -> B -> B' -> ...
2. Second pass: Set the random pointers for the copied nodes
   * For each original node A, its copy A' is A.next
   * If A.random is C, then A'.random should be C.next (which is C')
3. Third pass: Separate the original and copied lists
   * Restore the original list: A -> B -> C -> ...
   * Extract the copied list: A' -> B' -> C' -> ...
4. Return the head of the copied list

### **Step 3: Implementation (Optimized)**

```python
def copyRandomList(head):
    if not head:
        return None
    
    # First pass: Create a copy of each node and insert it right after the original node
    current = head
    while current:
        # Create a copy of the current node
        copy = Node(current.val)
        
        # Insert the copy between current and current.next
        copy.next = current.next
        current.next = copy
        
        # Move to the next original node
        current = copy.next
    
    # Second pass: Set the random pointers for the copied nodes
    current = head
    while current:
        # Set the random pointer for the copy
        if current.random:
            current.next.random = current.random.next
        
        # Move to the next original node
        current = current.next.next
    
    # Third pass: Separate the original and copied lists
    original = head
    copy_head = head.next
    copy_current = copy_head
    
    while original:
        # Restore the original list
        original.next = original.next.next
        
        # Extract the copied list
        if copy_current.next:
            copy_current.next = copy_current.next.next
        
        # Move to the next nodes
        original = original.next
        copy_current = copy_current.next
    
    return copy_head
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the number of nodes in the linked list. We traverse the list three times, which is still O(n).
* Space Complexity: O(1) as we only use a constant amount of extra space. The space for the copied list is not counted as extra space because it's part of the output.
* This approach is optimal in terms of both time and space complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For the example input:
  * First pass: Create interweaved list 7 -> 7' -> 13 -> 13' -> 11 -> 11' -> 10 -> 10' -> 1 -> 1'
  * Second pass: Set random pointers for copied nodes
    * 7'.random = null
    * 13'.random = 7'
    * 11'.random = 1'
    * 10'.random = 11'
    * 1'.random = 7'
  * Third pass: Separate the lists
    * Original: 7 -> 13 -> 11 -> 10 -> 1
    * Copied: 7' -> 13' -> 11' -> 10' -> 1'
* We're efficiently creating a deep copy without using extra space.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Hash Map) | O(n) | O(n) | Uses a hash map to map original nodes to their copies |
| Optimized (Algorithm: Interweaving) | O(n) | O(1) | Uses interweaving to establish relationships without extra space |

---

## **Final Takeaways**

* The key to creating a deep copy of a linked list with random pointers without using extra space is to interweave the original and copied nodes.
* By inserting each copied node right after its original node, we establish a relationship between them that allows us to set the random pointers correctly.
* This problem demonstrates how creative manipulation of the linked list structure can lead to space-efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're copying a complex document where each page has a reference to another page. The brute force approach would be like creating a table that maps original pages to their copies, and then using this table to set the references in the copied document. The optimized approach is like temporarily attaching each copied page right after its original page, setting the references, and then separating the original and copied documents - this way, you don't need to maintain a separate mapping table.

---

## **Summary**

* We solved the "Copy List with Random Pointer" problem by first considering a hash map approach that creates a mapping from original nodes to their copies, resulting in O(n) time and space complexity. We then optimized using an interweaving approach that creates a copy of each node and inserts it right after the original node, reducing the space complexity to O(1) while maintaining O(n) time complexity. The key insight is to establish a relationship between original and copied nodes by interweaving them, which allows us to set the random pointers correctly without using extra space. 