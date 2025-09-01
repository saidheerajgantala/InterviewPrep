# 📝 Count Good Nodes in Binary Tree

## **Problem Statement**

* Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.
* Return the number of good nodes in the binary tree.
* Example:
  * Input: root = [3,1,4,3,null,1,5]
  * Output: 4
  * Explanation: The binary tree looks like:
    ```
        3
       / \
      1   4
     /   / \
    3   1   5
    ```
    Nodes in blue are good:
    - Root Node (3) is always good.
    - Node 4 -> (3,4) is the maximum value in the path 3 -> 4.
    - Node 5 -> (3,4,5) is the maximum value in the path 3 -> 4 -> 5.
    - Node 3 -> (3,1,3) is the maximum value in the path 3 -> 1 -> 3.
* Constraints:
  * The number of nodes in the binary tree is in the range [1, 10^5].
  * Each node's value is between [-10^4, 10^4].

---

## **Step 1: Thinking Process (Brute Force)**

* We need to count the number of nodes where the node's value is greater than or equal to all values in the path from the root to that node
* One approach would be to traverse the tree and keep track of the maximum value seen so far in the path
* For each node, if its value is greater than or equal to the maximum value seen so far, it's a good node
* We can use a depth-first search (DFS) to traverse the tree

---

## **Step 2: Flow Steps (Brute Force)**

1. Define a recursive function to perform DFS with the maximum value seen so far:
   1. If the current node is null, return 0
   2. If the current node's value is greater than or equal to the maximum value seen so far, it's a good node
   3. Update the maximum value seen so far to be the maximum of the current maximum and the current node's value
   4. Recursively process the left and right subtrees with the updated maximum value
   5. Return the total number of good nodes
2. Call the recursive function with the root and negative infinity as the initial maximum value
3. Return the result

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def goodNodes(root):
    def dfs(node, max_so_far):
        if not node:
            return 0
        
        # Check if the current node is a good node
        is_good = node.val >= max_so_far
        
        # Update the maximum value seen so far
        max_so_far = max(max_so_far, node.val)
        
        # Recursively process the left and right subtrees
        left_count = dfs(node.left, max_so_far)
        right_count = dfs(node.right, max_so_far)
        
        # Return the total number of good nodes
        return (1 if is_good else 0) + left_count + right_count
    
    return dfs(root, float('-inf'))
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is actually quite efficient and is the standard way to solve this problem.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree:
  * dfs(3, -inf):
    * 3 >= -inf, so it's a good node
    * max_so_far = 3
    * dfs(1, 3):
      * 1 < 3, so it's not a good node
      * max_so_far = 3
      * dfs(3, 3):
        * 3 >= 3, so it's a good node
        * max_so_far = 3
        * dfs(null, 3): return 0
        * dfs(null, 3): return 0
        * Return 1 + 0 + 0 = 1
      * dfs(null, 3): return 0
      * Return 0 + 1 + 0 = 1
    * dfs(4, 3):
      * 4 >= 3, so it's a good node
      * max_so_far = 4
      * dfs(1, 4):
        * 1 < 4, so it's not a good node
        * max_so_far = 4
        * dfs(null, 4): return 0
        * dfs(null, 4): return 0
        * Return 0 + 0 + 0 = 0
      * dfs(5, 4):
        * 5 >= 4, so it's a good node
        * max_so_far = 5
        * dfs(null, 5): return 0
        * dfs(null, 5): return 0
        * Return 1 + 0 + 0 = 1
      * Return 1 + 0 + 1 = 2
    * Return 1 + 1 + 2 = 4
* There's no redundant computation in this approach, as we process each node exactly once

---

## **Why are we using this algorithm?**

* For this problem, the recursive DFS approach is the most natural and efficient.
* The property that matches this problem is the need to keep track of the maximum value seen so far in the path from the root to the current node.
* By using DFS, we can efficiently traverse the tree and check if each node is a good node.
* Alternatives like BFS would also work but might be less intuitive for this problem.
* Precondition: We need to be able to traverse the tree.

---

## **Algo optimization**

While the recursive DFS approach is already efficient, we can consider an iterative approach using a stack.

### **Step 1: Thinking Process (Optimized - Iterative DFS)**

* We can use an iterative depth-first search (DFS) to traverse the tree
* We'll use a stack to keep track of the nodes to visit and the maximum value seen so far in the path
* For each node, if its value is greater than or equal to the maximum value seen so far, it's a good node
* This approach avoids the potential stack overflow issue for very deep trees

### **Step 2: Flow Steps (Optimized - Iterative DFS)**

1. If the root is null, return 0
2. Initialize a count of good nodes to 0
3. Initialize a stack with a tuple of (root, negative infinity)
4. While the stack is not empty:
   1. Pop a node and the maximum value seen so far from the stack
   2. If the node's value is greater than or equal to the maximum value seen so far, increment the count
   3. Update the maximum value seen so far to be the maximum of the current maximum and the node's value
   4. Push the right and left children (if they exist) to the stack with the updated maximum value
5. Return the count

### **Step 3: Implementation (Optimized - Iterative DFS)**

```python
def goodNodes(root):
    if not root:
        return 0
    
    count = 0
    stack = [(root, float('-inf'))]
    
    while stack:
        node, max_so_far = stack.pop()
        
        # Check if the current node is a good node
        if node.val >= max_so_far:
            count += 1
        
        # Update the maximum value seen so far
        max_so_far = max(max_so_far, node.val)
        
        # Push the children to the stack
        if node.right:
            stack.append((node.right, max_so_far))
        if node.left:
            stack.append((node.left, max_so_far))
    
    return count
```

### **Step 4: Complexity Analysis (Optimized - Iterative DFS)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach has the same time and space complexity as the recursive approach but avoids the potential stack overflow issue for very deep trees.

### **Step 5: Visualizing Computation (Optimized - Iterative DFS)**

* For the example tree:
  * Initialize count = 0, stack = [(3, -inf)]
  * Pop (3, -inf):
    * 3 >= -inf, so count = 1
    * max_so_far = 3
    * Push (4, 3) and (1, 3) to the stack: stack = [(1, 3), (4, 3)]
  * Pop (1, 3):
    * 1 < 3, so count = 1
    * max_so_far = 3
    * Push (3, 3) to the stack: stack = [(4, 3), (3, 3)]
  * Pop (3, 3):
    * 3 >= 3, so count = 2
    * max_so_far = 3
    * No children to push
  * Pop (4, 3):
    * 4 >= 3, so count = 3
    * max_so_far = 4
    * Push (5, 4) and (1, 4) to the stack: stack = [(1, 4), (5, 4)]
  * Pop (5, 4):
    * 5 >= 4, so count = 4
    * max_so_far = 5
    * No children to push
  * Pop (1, 4):
    * 1 < 4, so count = 4
    * max_so_far = 4
    * No children to push
  * Stack is empty, return count = 4
* We're efficiently counting the good nodes using an iterative approach.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive DFS) | O(n) | O(h) | Uses recursion to traverse the tree |
| Optimized (Iterative DFS) | O(n) | O(h) | Uses a stack to traverse the tree |

---

## **Final Takeaways**

* Both recursive and iterative DFS approaches can be used to count the good nodes in a binary tree.
* The recursive approach is more concise and intuitive for this problem, while the iterative approach avoids the potential stack overflow issue for very deep trees.
* The key insight is to keep track of the maximum value seen so far in the path from the root to the current node.
* The choice between the two approaches might depend on the expected shape of the tree and the programming language's limitations.

---

## **Real-life Analogy**

* Imagine you're hiking along a trail with several viewpoints. A viewpoint is considered "good" if it offers a better view (higher elevation) than all previous viewpoints on your path. The recursive approach would be like exploring all possible paths from the starting point, keeping track of the highest elevation seen so far on each path, and counting the good viewpoints. The iterative approach would be like using a map to explore the paths one by one, again keeping track of the highest elevation seen so far on each path.

---

## **Summary**

* We solved the "Count Good Nodes in Binary Tree" problem by first considering a recursive depth-first search (DFS) approach that keeps track of the maximum value seen so far in the path from the root to the current node, resulting in O(n) time complexity and O(h) space complexity. We then explored an iterative DFS approach using a stack, also with O(n) time complexity and O(h) space complexity. Both approaches efficiently count the good nodes in the binary tree, with the choice between them depending on the expected shape of the tree and programming language constraints. 