# 📝 Binary Tree Right Side View

## **Problem Statement**

* Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.
* Example:
  * Input: root = [1,2,3,null,5,null,4]
  * Output: [1,3,4]
  * Explanation: The binary tree looks like:
    ```
        1            <---
       / \
      2   3          <---
       \   \
        5   4        <---
    ```
    From the right side, you can see nodes 1, 3, and 4.
* Constraints:
  * The number of nodes in the tree is in the range [0, 100].
  * -100 <= Node.val <= 100

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the rightmost node at each level of the binary tree
* One approach would be to perform a level-order traversal (BFS) of the tree
* For each level, we only need to include the rightmost node in the result
* This approach is straightforward and efficient

---

## **Step 2: Flow Steps (Brute Force)**

1. If the root is null, return an empty list
2. Initialize a result list to store the right side view
3. Initialize a queue with the root node
4. While the queue is not empty:
   1. Get the number of nodes at the current level
   2. Process all nodes at the current level:
      1. Dequeue a node
      2. If it's the last node at the current level, add its value to the result
      3. Enqueue its left and right children (if they exist)
5. Return the result

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

from collections import deque

def rightSideView(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        
        for i in range(level_size):
            node = queue.popleft()
            
            # If it's the last node at the current level, add its value to the result
            if i == level_size - 1:
                result.append(node.val)
            
            # Enqueue the children
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(w) where w is the maximum width of the tree. In the worst case, the queue might contain all nodes at the widest level of the tree, which is O(n/2) = O(n) for a balanced tree.
* This approach is efficient and is the standard way to solve this problem.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree:
  * Initialize result = [], queue = [1]
  * Level 1: queue = [1], level_size = 1
    * Process 1: It's the last node, so add 1 to result: result = [1]
    * Enqueue 1's children: queue = [2, 3]
  * Level 2: queue = [2, 3], level_size = 2
    * Process 2: Not the last node
    * Process 3: It's the last node, so add 3 to result: result = [1, 3]
    * Enqueue 2's children: queue = [3, 5]
    * Enqueue 3's children: queue = [5, 4]
  * Level 3: queue = [5, 4], level_size = 2
    * Process 5: Not the last node
    * Process 4: It's the last node, so add 4 to result: result = [1, 3, 4]
    * Enqueue 5's children: queue = []
    * Enqueue 4's children: queue = []
  * Queue is empty, return result = [1, 3, 4]
* There's no redundant computation in this approach, as we process each node exactly once

---

## **Why are we using this algorithm?**

* For this problem, the BFS approach is the most natural and efficient.
* The property that matches this problem is the need to find the rightmost node at each level.
* By using a queue and processing nodes level by level, we can easily identify the rightmost node at each level.
* Alternatives like DFS would require additional logic to keep track of the level information.
* Precondition: We need to be able to traverse the tree.

---

## **Algo optimization**

While the BFS approach is already efficient, we can consider a recursive approach using DFS with level information.

### **Step 1: Thinking Process (Optimized - DFS)**

* We can use depth-first search (DFS) to traverse the tree
* We'll traverse the tree in a right-to-left manner (i.e., visit the right child before the left child)
* We'll keep track of the level of each node during the traversal
* For each level, the first node we encounter will be the rightmost node at that level
* This approach is less intuitive but can be more concise

### **Step 2: Flow Steps (Optimized - DFS)**

1. If the root is null, return an empty list
2. Initialize a result list to store the right side view
3. Define a recursive function to perform DFS with level information:
   1. If the current level is equal to the length of the result list, add the current node's value to the result
   2. Recursively process the right child with level + 1
   3. Recursively process the left child with level + 1
4. Call the recursive function with the root and level 0
5. Return the result

### **Step 3: Implementation (Optimized - DFS)**

```python
def rightSideView(root):
    if not root:
        return []
    
    result = []
    
    def dfs(node, level):
        if not node:
            return
        
        # If this is the first node we encounter at this level
        if level == len(result):
            result.append(node.val)
        
        # Process the right child first, then the left child
        dfs(node.right, level + 1)
        dfs(node.left, level + 1)
    
    dfs(root, 0)
    return result
```

### **Step 4: Complexity Analysis (Optimized - DFS)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree for the recursion stack, plus O(d) for the result list where d is the depth of the tree. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach has the same time complexity as the BFS approach but might have different space complexity depending on the shape of the tree.

### **Step 5: Visualizing Computation (Optimized - DFS)**

* For the example tree:
  * Initialize result = []
  * dfs(1, 0):
    * level = 0, result = [], so add 1 to result: result = [1]
    * dfs(3, 1):
      * level = 1, result = [1], so add 3 to result: result = [1, 3]
      * dfs(4, 2):
        * level = 2, result = [1, 3], so add 4 to result: result = [1, 3, 4]
        * dfs(null, 3): return
        * dfs(null, 3): return
      * dfs(null, 2): return
    * dfs(2, 1):
      * level = 1, result = [1, 3], already has a value for this level, so don't add
      * dfs(null, 2): return
      * dfs(5, 2):
        * level = 2, result = [1, 3, 4], already has a value for this level, so don't add
        * dfs(null, 3): return
        * dfs(null, 3): return
  * Return result = [1, 3, 4]
* We're efficiently finding the rightmost node at each level by traversing the tree in a right-to-left manner.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (BFS) | O(n) | O(w) | Uses a queue to traverse level by level |
| Optimized (DFS) | O(n) | O(h) | Uses recursion with level information |

---

## **Final Takeaways**

* Both BFS and DFS approaches can be used to find the right side view of a binary tree.
* The BFS approach is more intuitive, as it naturally processes nodes level by level and identifies the rightmost node at each level.
* The DFS approach is more concise and can be more efficient in terms of space complexity for deep trees, but it requires a specific traversal order (right-to-left).
* The choice between the two approaches might depend on the specific requirements of the problem and the expected shape of the tree.

---

## **Real-life Analogy**

* Imagine you're standing to the right of a tree and taking a photo. The BFS approach would be like examining the tree level by level, from top to bottom, and noting down the rightmost branch at each level. The DFS approach would be like starting from the top and always prioritizing the rightmost branch first, noting down the first branch you see at each level.

---

## **Summary**

* We solved the "Binary Tree Right Side View" problem by first considering a breadth-first search (BFS) approach that traverses the tree level by level and includes the rightmost node at each level in the result, resulting in O(n) time complexity and O(w) space complexity where w is the maximum width of the tree. We then explored a depth-first search (DFS) approach that traverses the tree in a right-to-left manner and includes the first node encountered at each level in the result, also with O(n) time complexity but with O(h) space complexity where h is the height of the tree. Both approaches efficiently find the right side view of the binary tree, with the choice between them depending on the specific requirements and the expected shape of the tree. 