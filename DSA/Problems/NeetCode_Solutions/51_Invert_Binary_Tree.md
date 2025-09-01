# 📝 Invert Binary Tree

## **Problem Statement**

* Given the root of a binary tree, invert the tree, and return its root.
* Example:
  * Input: root = [4,2,7,1,3,6,9]
  * Output: [4,7,2,9,6,3,1]
  * Explanation: The binary tree looks like:
    ```
         4                 4
       /   \             /   \
      2     7    =>     7     2
     / \   / \         / \   / \
    1   3 6   9       9   6 3   1
    ```
* Constraints:
  * The number of nodes in the tree is in the range [0, 100].
  * -100 <= Node.val <= 100

---

## **Step 1: Thinking Process (Brute Force)**

* We need to invert a binary tree, which means swapping the left and right children of every node
* One approach would be to traverse the tree and swap the left and right children of each node
* We can use a recursive approach to invert the left and right subtrees, and then swap them
* This approach is straightforward and efficient

---

## **Step 2: Flow Steps (Brute Force)**

1. If the root is null, return null (base case)
2. Recursively invert the left subtree
3. Recursively invert the right subtree
4. Swap the left and right children of the current node
5. Return the root

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def invertTree(root):
    if not root:
        return None
    
    # Recursively invert the left and right subtrees
    left_inverted = invertTree(root.left)
    right_inverted = invertTree(root.right)
    
    # Swap the left and right children
    root.left = right_inverted
    root.right = left_inverted
    
    return root
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is actually quite efficient and is the standard way to solve this problem.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree:
  * invertTree(root):
    * left_inverted = invertTree(root.left) = invertTree(2)
      * left_inverted = invertTree(2.left) = invertTree(1) = 1 (no children to swap)
      * right_inverted = invertTree(2.right) = invertTree(3) = 3 (no children to swap)
      * Swap 2.left and 2.right: 2.left = 3, 2.right = 1
      * Return 2
    * right_inverted = invertTree(root.right) = invertTree(7)
      * left_inverted = invertTree(7.left) = invertTree(6) = 6 (no children to swap)
      * right_inverted = invertTree(7.right) = invertTree(9) = 9 (no children to swap)
      * Swap 7.left and 7.right: 7.left = 9, 7.right = 6
      * Return 7
    * Swap root.left and root.right: root.left = 7, root.right = 2
    * Return root
* There's no redundant computation in this approach, as we process each node exactly once

---

## **Why are we using this algorithm?**

* For this problem, the recursive approach is actually quite efficient.
* The property that matches this problem is the recursive structure of binary trees.
* By using a depth-first search (DFS) approach, we can invert the tree in a single traversal.
* Alternatives like breadth-first search (BFS) would also work but might be less intuitive for this problem.
* Precondition: We need to be able to traverse the tree.

---

## **Algo optimization**

While the recursive approach is already efficient, we can consider an iterative approach using BFS or DFS.

### **Step 1: Thinking Process (Optimized - BFS)**

* We can use breadth-first search (BFS) to traverse the tree level by level
* We'll use a queue to keep track of the nodes at each level
* For each node, we swap its left and right children
* This approach is iterative and avoids the potential stack overflow issue for very deep trees

### **Step 2: Flow Steps (Optimized - BFS)**

1. If the root is null, return null
2. Initialize a queue with the root node
3. While the queue is not empty:
   1. Dequeue a node
   2. Swap its left and right children
   3. Enqueue its left and right children (if they exist)
4. Return the root

### **Step 3: Implementation (Optimized - BFS)**

```python
from collections import deque

def invertTree(root):
    if not root:
        return None
    
    # Initialize queue for BFS
    queue = deque([root])
    
    while queue:
        node = queue.popleft()
        
        # Swap the left and right children
        node.left, node.right = node.right, node.left
        
        # Enqueue the children
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    
    return root
```

### **Step 4: Complexity Analysis (Optimized - BFS)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(w) where w is the maximum width of the tree. In the worst case, the queue might contain all nodes at the widest level of the tree. For a balanced tree, w is approximately n/2 in the last level, which is O(n).
* This approach has the same time complexity as the recursive approach but might have different space complexity depending on the shape of the tree.

### **Step 5: Visualizing Computation (Optimized - BFS)**

* For the example tree:
  * Initialize queue = [4]
  * Dequeue 4, swap its children: 4.left = 7, 4.right = 2, queue = []
  * Enqueue 4's children: queue = [7, 2]
  * Dequeue 7, swap its children: 7.left = 9, 7.right = 6, queue = [2]
  * Enqueue 7's children: queue = [2, 9, 6]
  * Dequeue 2, swap its children: 2.left = 3, 2.right = 1, queue = [9, 6]
  * Enqueue 2's children: queue = [9, 6, 3, 1]
  * Dequeue 9, 6, 3, 1 (no children to swap)
  * Queue is empty, return root
* We're efficiently inverting the tree level by level.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (DFS Recursive) | O(n) | O(h) | h is the height of the tree |
| Optimized (BFS Iterative) | O(n) | O(w) | w is the maximum width of the tree |

---

## **Final Takeaways**

* Both recursive (DFS) and iterative (BFS) approaches can be used to invert a binary tree.
* The recursive approach is more concise and intuitive for this problem, while the iterative approach avoids the potential stack overflow issue for very deep trees.
* The choice between the two approaches might depend on the expected shape of the tree and the programming language's limitations.

---

## **Real-life Analogy**

* Imagine you're looking at a mirror image of a tree. The recursive approach would be like starting from the trunk and flipping each branch and its sub-branches one by one. The iterative approach would be like flipping the tree level by level, starting from the trunk, then the main branches, then the smaller branches, and so on.

---

## **Summary**

* We solved the "Invert Binary Tree" problem by first considering a recursive depth-first search (DFS) approach that inverts the left and right subtrees and then swaps them, resulting in O(n) time complexity and O(h) space complexity. We then explored an iterative breadth-first search (BFS) approach that traverses the tree level by level and swaps the left and right children of each node, also with O(n) time complexity but with O(w) space complexity where w is the maximum width of the tree. Both approaches efficiently invert the binary tree, with the choice between them depending on the expected shape of the tree and programming language constraints. 