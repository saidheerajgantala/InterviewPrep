# 📝 Diameter of Binary Tree

## **Problem Statement**

* Given the root of a binary tree, return the length of the diameter of the tree.
* The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.
* The length of a path between two nodes is represented by the number of edges between them.
* Example 1:
  * Input: root = [1,2,3,4,5]
  * Output: 3
  * Explanation: The binary tree looks like:
    ```
          1
         / \
        2   3
       / \
      4   5
    ```
    The longest path is [4,2,1,3] or [5,2,1,3], which has a length of 3.
* Example 2:
  * Input: root = [1,2]
  * Output: 1
  * Explanation: The binary tree looks like:
    ```
          1
         /
        2
    ```
    The longest path is [1,2], which has a length of 1.
* Constraints:
  * The number of nodes in the tree is in the range [1, 10^4].
  * -100 <= Node.val <= 100

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the diameter of a binary tree, which is the length of the longest path between any two nodes
* The path may or may not pass through the root
* One approach would be to:
  1. For each node, calculate the longest path that passes through it
  2. The longest path through a node is the sum of the heights of its left and right subtrees
  3. Keep track of the maximum diameter found so far
  4. Return the maximum diameter

---

## **Step 2: Flow Steps (Brute Force)**

1. Define a function `height(node)` to calculate the height of a tree:
   1. If the node is null, return 0
   2. Otherwise, return 1 plus the maximum of the heights of the left and right subtrees
2. Define a function `diameterOfBinaryTree(root)` to find the diameter:
   1. If the root is null, return 0
   2. Calculate the height of the left and right subtrees
   3. Calculate the diameter passing through the root: left_height + right_height
   4. Recursively calculate the diameter of the left and right subtrees
   5. Return the maximum of the three diameters

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def diameterOfBinaryTree(root):
    # Helper function to calculate the height of a tree
    def height(node):
        if not node:
            return 0
        return 1 + max(height(node.left), height(node.right))
    
    # Base case: an empty tree has a diameter of 0
    if not root:
        return 0
    
    # Calculate the height of the left and right subtrees
    left_height = height(root.left)
    right_height = height(root.right)
    
    # Calculate the diameter passing through the root
    diameter_through_root = left_height + right_height
    
    # Recursively calculate the diameter of the left and right subtrees
    left_diameter = diameterOfBinaryTree(root.left)
    right_diameter = diameterOfBinaryTree(root.right)
    
    # Return the maximum of the three diameters
    return max(diameter_through_root, left_diameter, right_diameter)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the number of nodes in the tree. For each node, we calculate the height of its subtrees, which takes O(n) time in the worst case. Since we do this for each of the n nodes, the overall time complexity is O(n²).
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is inefficient for large trees due to the redundant calculations of heights.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree [1,2,3,4,5]:
  * diameterOfBinaryTree(1):
    * height(1.left) = height(2):
      * height(2.left) = height(4) = 1
      * height(2.right) = height(5) = 1
      * Return 1 + max(1, 1) = 2
    * height(1.right) = height(3) = 1
    * diameter_through_root = 2 + 1 = 3
    * left_diameter = diameterOfBinaryTree(2):
      * height(2.left) = height(4) = 1
      * height(2.right) = height(5) = 1
      * diameter_through_root = 1 + 1 = 2
      * left_diameter = diameterOfBinaryTree(4) = 0
      * right_diameter = diameterOfBinaryTree(5) = 0
      * Return max(2, 0, 0) = 2
    * right_diameter = diameterOfBinaryTree(3) = 0
    * Return max(3, 2, 0) = 3
* We're calculating the height of the same subtrees multiple times, which is inefficient

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Bottom-Up approach.
* The property that matches this problem is that we can calculate the height of a tree and update the maximum diameter in a single traversal.
* By using a post-order traversal, we can first calculate the heights of the left and right subtrees, and then update the maximum diameter.
* Alternatives like the brute force approach would be less efficient due to the redundant calculations of heights.
* Precondition: We need to be able to traverse the tree.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Bottom-Up)**

* Instead of calculating the height of each subtree multiple times, we can use a bottom-up approach
* We'll define a function that calculates the height of a tree and updates the maximum diameter in a single traversal
* This way, we can avoid redundant calculations of heights

### **Step 2: Flow Steps (Optimized - Bottom-Up)**

1. Initialize a variable `max_diameter` to keep track of the maximum diameter found so far
2. Define a helper function `height(node)` that calculates the height of a tree and updates `max_diameter`:
   1. If the node is null, return 0
   2. Calculate the height of the left and right subtrees
   3. Update `max_diameter` with the maximum of the current value and the sum of the left and right heights
   4. Return 1 plus the maximum of the left and right heights
3. Call the helper function with the root
4. Return `max_diameter`

### **Step 3: Implementation (Optimized - Bottom-Up)**

```python
def diameterOfBinaryTree(root):
    max_diameter = 0
    
    # Helper function to calculate the height of a tree and update max_diameter
    def height(node):
        nonlocal max_diameter
        if not node:
            return 0
        
        # Calculate the height of the left and right subtrees
        left_height = height(node.left)
        right_height = height(node.right)
        
        # Update max_diameter with the diameter passing through the current node
        max_diameter = max(max_diameter, left_height + right_height)
        
        # Return the height of the current node
        return 1 + max(left_height, right_height)
    
    # Calculate the height of the tree and update max_diameter
    height(root)
    
    return max_diameter
```

### **Step 4: Complexity Analysis (Optimized - Bottom-Up)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is much more efficient than the brute force approach, especially for large trees.

### **Step 5: Visualizing Computation (Optimized - Bottom-Up)**

* For the example tree [1,2,3,4,5]:
  * Initialize max_diameter = 0
  * height(1):
    * height(1.left) = height(2):
      * height(2.left) = height(4):
        * height(4.left) = height(null) = 0
        * height(4.right) = height(null) = 0
        * max_diameter = max(0, 0 + 0) = 0
        * Return 1 + max(0, 0) = 1
      * height(2.right) = height(5):
        * height(5.left) = height(null) = 0
        * height(5.right) = height(null) = 0
        * max_diameter = max(0, 0 + 0) = 0
        * Return 1 + max(0, 0) = 1
      * max_diameter = max(0, 1 + 1) = 2
      * Return 1 + max(1, 1) = 2
    * height(1.right) = height(3):
      * height(3.left) = height(null) = 0
      * height(3.right) = height(null) = 0
      * max_diameter = max(2, 0 + 0) = 2
      * Return 1 + max(0, 0) = 1
    * max_diameter = max(2, 2 + 1) = 3
    * Return 1 + max(2, 1) = 3
  * Return max_diameter = 3
* We're efficiently calculating the height of each subtree and updating the maximum diameter in a single traversal.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(h) | Calculates heights multiple times |
| Optimized (Bottom-Up) | O(n) | O(h) | Calculates heights once |

---

## **Final Takeaways**

* The key to efficiently finding the diameter of a binary tree is to avoid redundant calculations of heights.
* By using a bottom-up approach, we can calculate the height of a tree and update the maximum diameter in a single traversal.
* This problem demonstrates how a post-order traversal can be used to efficiently solve tree problems that require information from subtrees.

---

## **Real-life Analogy**

* Imagine you're trying to find the longest path in a family tree. The brute force approach would be like measuring the height of each branch multiple times, which is inefficient. The optimized approach would be like starting from the leaves and working your way up, calculating the height of each branch and updating the longest path as you go. This way, you only need to measure each branch once.

---

## **Summary**

* We solved the "Diameter of Binary Tree" problem by first considering a brute force approach that calculates the height of each subtree multiple times, resulting in O(n²) time complexity. We then optimized using a bottom-up approach that calculates the height of a tree and updates the maximum diameter in a single traversal, reducing the time complexity to O(n). The key insight is to use a post-order traversal to first calculate the heights of the left and right subtrees, and then update the maximum diameter. 