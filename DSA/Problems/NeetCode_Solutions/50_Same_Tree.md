# 📝 Same Tree

## **Problem Statement**

* Given the roots of two binary trees p and q, write a function to check if they are the same or not.
* Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.
* Example 1:
  * Input: p = [1,2,3], q = [1,2,3]
  * Output: true
  * Explanation: The binary trees look like:
    ```
        1         1
       / \       / \
      2   3     2   3
    ```
    They are structurally identical and have the same values.
* Example 2:
  * Input: p = [1,2], q = [1,null,2]
  * Output: false
  * Explanation: The binary trees look like:
    ```
        1         1
       /           \
      2             2
    ```
    They are structurally different.
* Constraints:
  * The number of nodes in both trees is in the range [0, 100].
  * -10^4 <= Node.val <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to check if two binary trees are the same
* Two trees are the same if they have the same structure and the same values at corresponding nodes
* One approach would be to traverse both trees simultaneously and compare each node
* We can use a recursive approach to check if the current nodes are the same and then recursively check their left and right subtrees

---

## **Step 2: Flow Steps (Brute Force)**

1. If both p and q are null, return true (both trees are empty)
2. If one of p or q is null but the other is not, return false (different structure)
3. If the values of p and q are different, return false (different values)
4. Recursively check if the left subtrees of p and q are the same
5. Recursively check if the right subtrees of p and q are the same
6. Return true if both left and right subtrees are the same

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isSameTree(p, q):
    # If both trees are empty, they are the same
    if not p and not q:
        return True
    
    # If one tree is empty but the other is not, they are different
    if not p or not q:
        return False
    
    # If the values of the current nodes are different, the trees are different
    if p.val != q.val:
        return False
    
    # Recursively check the left and right subtrees
    return isSameTree(p.left, q.left) and isSameTree(p.right, q.right)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is actually quite efficient and is the standard way to solve this problem.

---

## **Step 5: Visualizing Redundant Computation**

* For the example trees p = [1,2,3] and q = [1,2,3]:
  * isSameTree(p, q):
    * p.val = 1, q.val = 1, they are equal
    * isSameTree(p.left, q.left):
      * p.left.val = 2, q.left.val = 2, they are equal
      * isSameTree(p.left.left, q.left.left): both are null, return true
      * isSameTree(p.left.right, q.left.right): both are null, return true
      * Return true
    * isSameTree(p.right, q.right):
      * p.right.val = 3, q.right.val = 3, they are equal
      * isSameTree(p.right.left, q.right.left): both are null, return true
      * isSameTree(p.right.right, q.right.right): both are null, return true
      * Return true
    * Return true
* There's no redundant computation in this approach, as we process each node exactly once

---

## **Why are we using this algorithm?**

* For this problem, the recursive approach is actually quite efficient.
* The property that matches this problem is the recursive structure of binary trees.
* By using a depth-first search (DFS) approach, we can compare the trees in a single traversal.
* Alternatives like breadth-first search (BFS) would also work but might be less intuitive for this problem.
* Precondition: We need to be able to traverse both trees.

---

## **Algo optimization**

While the recursive approach is already efficient, we can consider an iterative approach using BFS or DFS.

### **Step 1: Thinking Process (Optimized - BFS)**

* We can use breadth-first search (BFS) to traverse both trees level by level
* We'll use two queues to keep track of the nodes at each level in both trees
* For each level, we process all nodes in the queues and compare them
* If at any point the nodes are different, we return false
* If we finish traversing both trees without finding any differences, we return true

### **Step 2: Flow Steps (Optimized - BFS)**

1. If both p and q are null, return true (both trees are empty)
2. If one of p or q is null but the other is not, return false (different structure)
3. Initialize two queues, one for each tree, with the root nodes
4. While both queues are not empty:
   1. Dequeue a node from each queue
   2. Compare the values of the nodes
   3. If the values are different, return false
   4. Enqueue the left and right children of both nodes (if they exist)
   5. If one node has a child but the other doesn't, return false (different structure)
5. Return true if we finish traversing both trees without finding any differences

### **Step 3: Implementation (Optimized - BFS)**

```python
from collections import deque

def isSameTree(p, q):
    # If both trees are empty, they are the same
    if not p and not q:
        return True
    
    # If one tree is empty but the other is not, they are different
    if not p or not q:
        return False
    
    # Initialize queues for BFS
    queue_p = deque([p])
    queue_q = deque([q])
    
    while queue_p and queue_q:
        node_p = queue_p.popleft()
        node_q = queue_q.popleft()
        
        # Compare the values of the current nodes
        if node_p.val != node_q.val:
            return False
        
        # Check the left children
        if (node_p.left and not node_q.left) or (not node_p.left and node_q.left):
            return False
        if node_p.left and node_q.left:
            queue_p.append(node_p.left)
            queue_q.append(node_q.left)
        
        # Check the right children
        if (node_p.right and not node_q.right) or (not node_p.right and node_q.right):
            return False
        if node_p.right and node_q.right:
            queue_p.append(node_p.right)
            queue_q.append(node_q.right)
    
    # If we've processed all nodes without finding any differences, the trees are the same
    return True
```

### **Step 4: Complexity Analysis (Optimized - BFS)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(w) where w is the maximum width of the tree. In the worst case, the queue might contain all nodes at the widest level of the tree. For a balanced tree, w is approximately n/2 in the last level, which is O(n).
* This approach has the same time complexity as the recursive approach but might have different space complexity depending on the shape of the tree.

### **Step 5: Visualizing Computation (Optimized - BFS)**

* For the example trees p = [1,2,3] and q = [1,2,3]:
  * Initialize queue_p = [1], queue_q = [1]
  * Dequeue node_p = 1, node_q = 1, they are equal
  * Enqueue their children: queue_p = [2, 3], queue_q = [2, 3]
  * Dequeue node_p = 2, node_q = 2, they are equal
  * They have no children
  * Dequeue node_p = 3, node_q = 3, they are equal
  * They have no children
  * Both queues are empty, return true
* We're efficiently comparing the trees level by level.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (DFS Recursive) | O(n) | O(h) | h is the height of the tree |
| Optimized (BFS Iterative) | O(n) | O(w) | w is the maximum width of the tree |

---

## **Final Takeaways**

* Both recursive (DFS) and iterative (BFS) approaches can be used to check if two binary trees are the same.
* The recursive approach is more concise and intuitive for this problem, while the iterative approach avoids the potential stack overflow issue for very deep trees.
* The choice between the two approaches might depend on the expected shape of the tree and the programming language's limitations.

---

## **Real-life Analogy**

* Imagine you're comparing two family trees to see if they are identical. The recursive approach would be like starting from the root (the oldest ancestor) and comparing each person and their descendants one by one. The iterative approach would be like comparing the family trees generation by generation, making sure that each generation has the same people with the same relationships.

---

## **Summary**

* We solved the "Same Tree" problem by first considering a recursive depth-first search (DFS) approach that checks if the current nodes are the same and then recursively checks their left and right subtrees, resulting in O(n) time complexity and O(h) space complexity. We then explored an iterative breadth-first search (BFS) approach that traverses both trees level by level, also with O(n) time complexity but with O(w) space complexity where w is the maximum width of the tree. Both approaches efficiently check if two binary trees are the same, with the choice between them depending on the expected shape of the trees and programming language constraints. 