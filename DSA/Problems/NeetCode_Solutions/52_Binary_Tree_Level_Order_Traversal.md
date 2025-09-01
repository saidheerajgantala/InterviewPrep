# 📝 Binary Tree Level Order Traversal

## **Problem Statement**

* Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).
* Example:
  * Input: root = [3,9,20,null,null,15,7]
  * Output: [[3],[9,20],[15,7]]
  * Explanation: The binary tree looks like:
    ```
        3
       / \
      9  20
        /  \
       15   7
    ```
    Level order traversal: [[3], [9, 20], [15, 7]]
* Constraints:
  * The number of nodes in the tree is in the range [0, 2000].
  * -1000 <= Node.val <= 1000

---

## **Step 1: Thinking Process (Brute Force)**

* We need to traverse a binary tree level by level and return the values at each level
* The natural approach for level-order traversal is to use Breadth-First Search (BFS)
* We can use a queue to keep track of the nodes at each level
* For each level, we process all nodes in the queue and add their children to the queue for the next level
* This approach is actually the standard way to solve this problem

---

## **Step 2: Flow Steps (Brute Force)**

1. If the root is null, return an empty list
2. Initialize a result list to store the level-order traversal
3. Initialize a queue with the root node
4. While the queue is not empty:
   1. Get the number of nodes at the current level
   2. Process all nodes at the current level:
      1. Dequeue a node
      2. Add its value to the current level's list
      3. Enqueue its left and right children (if they exist)
   3. Add the current level's list to the result
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

def levelOrder(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        current_level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            current_level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(current_level)
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(n) for the queue and the result list. In the worst case, the queue might contain all nodes at the widest level of the tree, which is O(n/2) = O(n) for a balanced tree.
* This approach is actually quite efficient and is the standard way to solve this problem.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree:
  * Initialize result = [], queue = [3]
  * Level 1: queue = [3], level_size = 1
    * Process 3: current_level = [3], queue = []
    * Enqueue 3's children: queue = [9, 20]
    * Add current_level to result: result = [[3]]
  * Level 2: queue = [9, 20], level_size = 2
    * Process 9: current_level = [9], queue = [20]
    * Process 20: current_level = [9, 20], queue = []
    * Enqueue 9's children: queue = []
    * Enqueue 20's children: queue = [15, 7]
    * Add current_level to result: result = [[3], [9, 20]]
  * Level 3: queue = [15, 7], level_size = 2
    * Process 15: current_level = [15], queue = [7]
    * Process 7: current_level = [15, 7], queue = []
    * Enqueue 15's children: queue = []
    * Enqueue 7's children: queue = []
    * Add current_level to result: result = [[3], [9, 20], [15, 7]]
  * Queue is empty, return result = [[3], [9, 20], [15, 7]]
* There's no redundant computation in this approach, as we process each node exactly once

---

## **Why are we using this algorithm?**

* For this problem, the BFS approach is the most natural and efficient.
* The property that matches this problem is the need to traverse the tree level by level.
* By using a queue, we can efficiently process nodes at each level and keep track of their children for the next level.
* Alternatives like DFS would require additional logic to keep track of the level information.
* Precondition: We need to be able to traverse the tree.

---

## **Algo optimization**

While the BFS approach is already efficient, we can consider a recursive approach using DFS with level information.

### **Step 1: Thinking Process (Optimized - DFS)**

* We can use depth-first search (DFS) to traverse the tree
* We'll keep track of the level of each node during the traversal
* For each node, we add its value to the corresponding level in the result list
* This approach is less intuitive for level-order traversal but can be useful in some scenarios

### **Step 2: Flow Steps (Optimized - DFS)**

1. If the root is null, return an empty list
2. Initialize a result list to store the level-order traversal
3. Define a recursive function to perform DFS with level information:
   1. If the current level's list doesn't exist in the result, add an empty list
   2. Add the current node's value to the corresponding level's list
   3. Recursively process the left and right children with level + 1
4. Call the recursive function with the root and level 0
5. Return the result

### **Step 3: Implementation (Optimized - DFS)**

```python
def levelOrder(root):
    if not root:
        return []
    
    result = []
    
    def dfs(node, level):
        if not node:
            return
        
        # If this is the first node at this level
        if len(result) <= level:
            result.append([])
        
        # Add the current node's value to the corresponding level
        result[level].append(node.val)
        
        # Recursively process the children
        dfs(node.left, level + 1)
        dfs(node.right, level + 1)
    
    dfs(root, 0)
    return result
```

### **Step 4: Complexity Analysis (Optimized - DFS)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree for the recursion stack, plus O(n) for the result list. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach has the same time complexity as the BFS approach but might have different space complexity depending on the shape of the tree.

### **Step 5: Visualizing Computation (Optimized - DFS)**

* For the example tree:
  * Initialize result = []
  * dfs(3, 0):
    * Add 3 to result[0]: result = [[3]]
    * dfs(9, 1):
      * Add 9 to result[1]: result = [[3], [9]]
      * dfs(null, 2): return
      * dfs(null, 2): return
    * dfs(20, 1):
      * Add 20 to result[1]: result = [[3], [9, 20]]
      * dfs(15, 2):
        * Add 15 to result[2]: result = [[3], [9, 20], [15]]
        * dfs(null, 3): return
        * dfs(null, 3): return
      * dfs(7, 2):
        * Add 7 to result[2]: result = [[3], [9, 20], [15, 7]]
        * dfs(null, 3): return
        * dfs(null, 3): return
  * Return result = [[3], [9, 20], [15, 7]]
* We're efficiently traversing the tree and organizing the nodes by level.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (BFS) | O(n) | O(n) | Uses a queue to traverse level by level |
| Optimized (DFS) | O(n) | O(h) + O(n) | Uses recursion with level information |

---

## **Final Takeaways**

* Both BFS and DFS approaches can be used to perform level-order traversal of a binary tree.
* The BFS approach is more intuitive for level-order traversal, as it naturally processes nodes level by level.
* The DFS approach requires additional logic to keep track of the level information but can be useful in some scenarios.
* The choice between the two approaches might depend on the specific requirements of the problem and the programming language's limitations.

---

## **Real-life Analogy**

* Imagine you're organizing a family photo where you want to arrange people by generation. The BFS approach would be like gathering all people of the same generation and taking their photos before moving to the next generation. The DFS approach would be like following a family line down through all generations, keeping track of which generation each person belongs to, and then organizing the photos by generation afterward.

---

## **Summary**

* We solved the "Binary Tree Level Order Traversal" problem by first considering a breadth-first search (BFS) approach that uses a queue to traverse the tree level by level, resulting in O(n) time and space complexity. We then explored a depth-first search (DFS) approach that uses recursion with level information, also with O(n) time complexity but with additional O(h) space complexity for the recursion stack. Both approaches efficiently perform level-order traversal, with the BFS approach being more intuitive for this problem. 