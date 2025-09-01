# 📝 Maximum Depth of Binary Tree

## **Problem Statement**

* Given the root of a binary tree, return its maximum depth.
* A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
* Example:
  * Input: root = [3,9,20,null,null,15,7]
  * Output: 3
  * Explanation: The binary tree looks like:
    ```
        3
       / \
      9  20
        /  \
       15   7
    ```
    The maximum depth is 3.
* Constraints:
  * The number of nodes in the tree is in the range [0, 10^4].
  * -100 <= Node.val <= 100

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the maximum depth of a binary tree
* The maximum depth is the number of nodes along the longest path from the root to a leaf
* One approach would be to traverse the tree and keep track of the depth at each node
* We can use a recursive approach to explore all paths from the root to the leaves
* For each node, the depth is 1 plus the maximum depth of its left and right subtrees

---

## **Step 2: Flow Steps (Brute Force)**

1. If the root is null, return 0 (base case)
2. Recursively calculate the maximum depth of the left subtree
3. Recursively calculate the maximum depth of the right subtree
4. Return 1 plus the maximum of the depths of the left and right subtrees

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def maxDepth(root):
    if not root:
        return 0
    
    left_depth = maxDepth(root.left)
    right_depth = maxDepth(root.right)
    
    return 1 + max(left_depth, right_depth)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is actually quite efficient and is the standard way to solve this problem.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree:
  * maxDepth(root) = 1 + max(maxDepth(root.left), maxDepth(root.right))
  * maxDepth(root.left) = 1 + max(maxDepth(null), maxDepth(null)) = 1
  * maxDepth(root.right) = 1 + max(maxDepth(root.right.left), maxDepth(root.right.right)) = 2
  * maxDepth(root.right.left) = 1 + max(maxDepth(null), maxDepth(null)) = 1
  * maxDepth(root.right.right) = 1 + max(maxDepth(null), maxDepth(null)) = 1
  * So, maxDepth(root) = 1 + max(1, 2) = 3
* There's no redundant computation in this approach, as we process each node exactly once

---

## **Why are we using this algorithm?**

* For this problem, the recursive approach is actually quite efficient.
* The property that matches this problem is the recursive structure of a binary tree.
* By using a depth-first search (DFS) approach, we can calculate the maximum depth in a single traversal.
* Alternatives like breadth-first search (BFS) would also work but might be less intuitive for this problem.
* Precondition: We need to be able to traverse the tree.

---

## **Algo optimization**

While the recursive approach is already efficient, we can consider an iterative approach using BFS or DFS.

### **Step 1: Thinking Process (Optimized - BFS)**

* We can use breadth-first search (BFS) to traverse the tree level by level
* We'll use a queue to keep track of the nodes at each level
* For each level, we process all nodes in the queue and add their children to the queue for the next level
* The maximum depth is the number of levels we process

### **Step 2: Flow Steps (Optimized - BFS)**

1. If the root is null, return 0
2. Initialize a queue with the root node
3. Initialize depth = 0
4. While the queue is not empty:
   1. Increment depth
   2. Process all nodes at the current level:
      1. Dequeue a node
      2. Enqueue its left and right children (if they exist)
5. Return depth

### **Step 3: Implementation (Optimized - BFS)**

```python
from collections import deque

def maxDepth(root):
    if not root:
        return 0
    
    queue = deque([root])
    depth = 0
    
    while queue:
        depth += 1
        level_size = len(queue)
        
        for _ in range(level_size):
            node = queue.popleft()
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return depth
```

### **Step 4: Complexity Analysis (Optimized - BFS)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(w) where w is the maximum width of the tree. In the worst case, the queue might contain all nodes at the widest level of the tree. For a balanced tree, w is approximately n/2 in the last level, which is O(n).
* This approach has the same time complexity as the recursive approach but might have different space complexity depending on the shape of the tree.

### **Step 5: Visualizing Computation (Optimized - BFS)**

* For the example tree:
  * Initialize queue = [3], depth = 0
  * Level 1: queue = [3], level_size = 1, depth = 1
    * Process 3: queue = [9, 20]
  * Level 2: queue = [9, 20], level_size = 2, depth = 2
    * Process 9: queue = [20]
    * Process 20: queue = [15, 7]
  * Level 3: queue = [15, 7], level_size = 2, depth = 3
    * Process 15: queue = [7]
    * Process 7: queue = []
  * Queue is empty, return depth = 3
* We're efficiently calculating the maximum depth by traversing the tree level by level.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (DFS Recursive) | O(n) | O(h) | h is the height of the tree |
| Optimized (BFS Iterative) | O(n) | O(w) | w is the maximum width of the tree |

---

## **Final Takeaways**

* Both recursive (DFS) and iterative (BFS) approaches can be used to find the maximum depth of a binary tree.
* The recursive approach is more concise and intuitive for this problem, while the iterative approach avoids the potential stack overflow issue for very deep trees.
* The choice between the two approaches might depend on the expected shape of the tree and the programming language's limitations.

---

## **Real-life Analogy**

* Imagine you're trying to find the tallest branch in a real tree. The recursive approach would be like starting from the trunk and exploring each branch to its end, keeping track of the longest path. The iterative approach would be like measuring the tree level by level, starting from the trunk and moving outward, counting the number of levels until you reach the outermost leaves.

---

## **Summary**

* We solved the "Maximum Depth of Binary Tree" problem by first considering a recursive depth-first search (DFS) approach that calculates the maximum depth as 1 plus the maximum of the depths of the left and right subtrees, resulting in O(n) time complexity and O(h) space complexity. We then explored an iterative breadth-first search (BFS) approach that traverses the tree level by level, also with O(n) time complexity but with O(w) space complexity where w is the maximum width of the tree. Both approaches efficiently find the maximum depth, with the choice between them depending on the expected shape of the tree and programming language constraints. 