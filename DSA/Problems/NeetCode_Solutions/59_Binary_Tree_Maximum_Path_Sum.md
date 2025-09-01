# 📝 Binary Tree Maximum Path Sum

## **Problem Statement**

* A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.
* The path sum of a path is the sum of the node's values in the path.
* Given the root of a binary tree, return the maximum path sum of any non-empty path.
* Example 1:
  * Input: root = [1,2,3]
  * Output: 6
  * Explanation: The binary tree looks like:
    ```
        1
       / \
      2   3
    ```
    The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
* Example 2:
  * Input: root = [-10,9,20,null,null,15,7]
  * Output: 42
  * Explanation: The binary tree looks like:
    ```
        -10
        / \
       9  20
         /  \
        15   7
    ```
    The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
* Constraints:
  * The number of nodes in the tree is in the range [1, 3 * 10^4].
  * -1000 <= Node.val <= 1000

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the maximum path sum in a binary tree
* A path can start and end at any node, not necessarily the root
* One approach would be to consider every possible path in the tree and find the maximum sum
* For each node, we can consider it as the "highest point" of a path and compute the maximum path sum that passes through it
* The maximum path sum passing through a node would be the node's value plus the maximum path sum from its left subtree (if positive) plus the maximum path sum from its right subtree (if positive)

---

## **Step 2: Flow Steps (Brute Force)**

1. For each node in the tree:
   1. Compute the maximum path sum that passes through the node
   2. Update the global maximum path sum if necessary
2. Return the global maximum path sum

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def maxPathSum(root):
    # Helper function to compute the maximum path sum from a node to any leaf
    def max_path_from_node(node):
        if not node:
            return 0
        
        # Compute the maximum path sum from the left and right subtrees
        left_max = max(0, max_path_from_node(node.left))
        right_max = max(0, max_path_from_node(node.right))
        
        # Update the global maximum path sum
        nonlocal max_sum
        max_sum = max(max_sum, node.val + left_max + right_max)
        
        # Return the maximum path sum from this node to any leaf
        return node.val + max(left_max, right_max)
    
    # Initialize the global maximum path sum
    max_sum = float('-inf')
    
    # Compute the maximum path sum
    max_path_from_node(root)
    
    return max_sum
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is actually quite efficient and is the standard way to solve this problem.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree [1,2,3]:
  * max_path_from_node(1):
    * max_path_from_node(2) = 2 (since max(0, 2) = 2)
    * max_path_from_node(3) = 3 (since max(0, 3) = 3)
    * max_sum = max(-inf, 1 + 2 + 3) = 6
    * Return 1 + max(2, 3) = 4
  * Return max_sum = 6
* There's no redundant computation in this approach, as we process each node exactly once

---

## **Why are we using this algorithm?**

* For this problem, the recursive approach is actually quite efficient.
* The property that matches this problem is the recursive structure of a binary tree and the fact that a path can be viewed as a node with paths from its left and right subtrees.
* By computing the maximum path sum that passes through each node and updating a global maximum, we can find the maximum path sum in the entire tree.
* Alternatives would be less intuitive and potentially less efficient.
* Precondition: We need to be able to traverse the tree.

---

## **Algo optimization**

While the recursive approach is already efficient, we can make the code more concise and easier to understand.

### **Step 1: Thinking Process (Optimized - Simplified Recursive)**

* We can simplify the recursive approach by focusing on the key insight:
  * For each node, the maximum path sum that passes through it is the node's value plus the maximum path sum from its left subtree (if positive) plus the maximum path sum from its right subtree (if positive)
  * The maximum path sum from a node to any leaf is the node's value plus the maximum of the maximum path sum from its left subtree (if positive) and the maximum path sum from its right subtree (if positive)

### **Step 2: Flow Steps (Optimized - Simplified Recursive)**

1. Define a recursive function to compute the maximum path sum from a node to any leaf:
   1. If the node is null, return 0
   2. Compute the maximum path sum from the left subtree (if positive)
   3. Compute the maximum path sum from the right subtree (if positive)
   4. Update the global maximum path sum with the path sum that passes through the current node
   5. Return the maximum path sum from this node to any leaf
2. Initialize the global maximum path sum
3. Call the recursive function with the root
4. Return the global maximum path sum

### **Step 3: Implementation (Optimized - Simplified Recursive)**

```python
def maxPathSum(root):
    # Initialize the global maximum path sum
    max_sum = float('-inf')
    
    # Helper function to compute the maximum path sum from a node to any leaf
    def max_gain(node):
        nonlocal max_sum
        if not node:
            return 0
        
        # Compute the maximum path sum from the left and right subtrees
        # We take max with 0 because we can choose not to include the subtree
        left_gain = max(0, max_gain(node.left))
        right_gain = max(0, max_gain(node.right))
        
        # The price to start a new path where `node` is the highest point
        price_newpath = node.val + left_gain + right_gain
        
        # Update the global maximum path sum
        max_sum = max(max_sum, price_newpath)
        
        # Return the maximum path sum from this node to any leaf
        return node.val + max(left_gain, right_gain)
    
    # Compute the maximum path sum
    max_gain(root)
    
    return max_sum
```

### **Step 4: Complexity Analysis (Optimized - Simplified Recursive)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach has the same time and space complexity as the brute force approach, but the code is more concise and easier to understand.

### **Step 5: Visualizing Computation (Optimized - Simplified Recursive)**

* For the example tree [1,2,3]:
  * max_gain(1):
    * max_gain(2) = 2 (since max(0, 2) = 2)
    * max_gain(3) = 3 (since max(0, 3) = 3)
    * price_newpath = 1 + 2 + 3 = 6
    * max_sum = max(-inf, 6) = 6
    * Return 1 + max(2, 3) = 4
  * Return max_sum = 6
* We're efficiently computing the maximum path sum with a more concise implementation.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Recursive) | O(n) | O(h) | Computes the maximum path sum for each node |
| Optimized (Simplified Recursive) | O(n) | O(h) | Same approach with more concise code |

---

## **Final Takeaways**

* The key to solving the Binary Tree Maximum Path Sum problem is to recognize that a path can be viewed as a node with paths from its left and right subtrees.
* By computing the maximum path sum that passes through each node and updating a global maximum, we can find the maximum path sum in the entire tree.
* It's important to handle negative values correctly by taking the maximum with 0, as we can choose not to include a subtree if it has a negative sum.
* This problem demonstrates how a recursive approach can efficiently solve complex tree problems.

---

## **Real-life Analogy**

* Imagine you're planning a hiking trip through a mountain range, and you want to find the path with the maximum elevation gain. Each node in the tree represents a peak, and the edges represent trails connecting the peaks. The value of each node represents the elevation gain of that peak. You want to find a continuous path (not necessarily starting or ending at the highest peak) that gives you the maximum total elevation gain. The recursive approach is like exploring all possible paths from each peak and finding the one with the maximum elevation gain.

---

## **Summary**

* We solved the "Binary Tree Maximum Path Sum" problem using a recursive approach that computes the maximum path sum that passes through each node and updates a global maximum. The key insight is to view a path as a node with paths from its left and right subtrees, and to handle negative values correctly by taking the maximum with 0. This approach has O(n) time complexity and O(h) space complexity, where n is the number of nodes in the tree and h is the height of the tree. 