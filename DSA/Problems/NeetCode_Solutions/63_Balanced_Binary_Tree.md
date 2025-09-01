# 📝 Balanced Binary Tree

## **Problem Statement**

* Given a binary tree, determine if it is height-balanced.
* For this problem, a height-balanced binary tree is defined as: a binary tree in which the left and right subtrees of every node differ in height by no more than 1.
* Example 1:
  * Input: root = [3,9,20,null,null,15,7]
  * Output: true
  * Explanation: The binary tree looks like:
    ```
        3
       / \
      9  20
        /  \
       15   7
    ```
    The left subtree of node 3 has a height of 1, and the right subtree has a height of 2. The difference is 1, which is allowed.
* Example 2:
  * Input: root = [1,2,2,3,3,null,null,4,4]
  * Output: false
  * Explanation: The binary tree looks like:
    ```
           1
          / \
         2   2
        / \
       3   3
      / \
     4   4
    ```
    The left subtree of node 1 has a height of 3, and the right subtree has a height of 1. The difference is 2, which is not allowed.
* Constraints:
  * The number of nodes in the tree is in the range [0, 5000].
  * -10^4 <= Node.val <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine if a binary tree is height-balanced
* A binary tree is height-balanced if the left and right subtrees of every node differ in height by no more than 1
* One approach would be to:
  1. Define a function to calculate the height of a tree
  2. For each node, check if the difference in height between its left and right subtrees is at most 1
  3. Recursively check if both the left and right subtrees are also height-balanced

---

## **Step 2: Flow Steps (Brute Force)**

1. Define a function `height(node)` to calculate the height of a tree:
   1. If the node is null, return 0
   2. Otherwise, return 1 plus the maximum of the heights of the left and right subtrees
2. Define a function `isBalanced(root)` to check if a tree is height-balanced:
   1. If the root is null, return true (an empty tree is height-balanced)
   2. Calculate the height of the left and right subtrees
   3. Check if the absolute difference between the heights is at most 1
   4. Recursively check if both the left and right subtrees are also height-balanced
   5. Return true if all conditions are met, false otherwise

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isBalanced(root):
    # Helper function to calculate the height of a tree
    def height(node):
        if not node:
            return 0
        return 1 + max(height(node.left), height(node.right))
    
    # Base case: an empty tree is height-balanced
    if not root:
        return True
    
    # Check if the current node is balanced
    left_height = height(root.left)
    right_height = height(root.right)
    if abs(left_height - right_height) > 1:
        return False
    
    # Recursively check if both subtrees are balanced
    return isBalanced(root.left) and isBalanced(root.right)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the number of nodes in the tree. For each node, we calculate the height of its subtrees, which takes O(n) time in the worst case. Since we do this for each of the n nodes, the overall time complexity is O(n²).
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is inefficient for large trees due to the redundant calculations of heights.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree [3,9,20,null,null,15,7]:
  * isBalanced(3):
    * height(3.left) = height(9) = 1
    * height(3.right) = height(20):
      * height(20.left) = height(15) = 1
      * height(20.right) = height(7) = 1
      * Return 1 + max(1, 1) = 2
    * abs(1 - 2) = 1 <= 1, so the current node is balanced
    * isBalanced(3.left) = isBalanced(9):
      * height(9.left) = height(null) = 0
      * height(9.right) = height(null) = 0
      * abs(0 - 0) = 0 <= 1, so the current node is balanced
      * isBalanced(9.left) = isBalanced(null) = true
      * isBalanced(9.right) = isBalanced(null) = true
      * Return true
    * isBalanced(3.right) = isBalanced(20):
      * height(20.left) = height(15) = 1
      * height(20.right) = height(7) = 1
      * abs(1 - 1) = 0 <= 1, so the current node is balanced
      * isBalanced(20.left) = isBalanced(15) = true
      * isBalanced(20.right) = isBalanced(7) = true
      * Return true
    * Return true
* We're calculating the height of the same subtrees multiple times, which is inefficient

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Bottom-Up approach.
* The property that matches this problem is that we can check if a tree is balanced and calculate its height in a single traversal.
* By using a post-order traversal, we can first check if the left and right subtrees are balanced, and then check if the current node is balanced.
* Alternatives like the brute force approach would be less efficient due to the redundant calculations of heights.
* Precondition: We need to be able to traverse the tree.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Bottom-Up)**

* Instead of calculating the height of each subtree multiple times, we can use a bottom-up approach
* We'll define a function that returns both whether the tree is balanced and its height
* This way, we can check if a tree is balanced and calculate its height in a single traversal

### **Step 2: Flow Steps (Optimized - Bottom-Up)**

1. Define a helper function `check_balanced(node)` that returns a tuple `(is_balanced, height)`:
   1. If the node is null, return `(True, 0)` (an empty tree is balanced with a height of 0)
   2. Recursively check if the left subtree is balanced and get its height
   3. If the left subtree is not balanced, return `(False, 0)` (the height doesn't matter in this case)
   4. Recursively check if the right subtree is balanced and get its height
   5. If the right subtree is not balanced, return `(False, 0)`
   6. Check if the current node is balanced by comparing the heights of its left and right subtrees
   7. If the current node is balanced, return `(True, 1 + max(left_height, right_height))`
   8. Otherwise, return `(False, 0)`
2. Call the helper function with the root and return the first element of the tuple (whether the tree is balanced)

### **Step 3: Implementation (Optimized - Bottom-Up)**

```python
def isBalanced(root):
    # Helper function to check if a tree is balanced and calculate its height
    def check_balanced(node):
        if not node:
            return True, 0
        
        # Check if the left subtree is balanced and get its height
        left_balanced, left_height = check_balanced(node.left)
        if not left_balanced:
            return False, 0
        
        # Check if the right subtree is balanced and get its height
        right_balanced, right_height = check_balanced(node.right)
        if not right_balanced:
            return False, 0
        
        # Check if the current node is balanced
        is_balanced = abs(left_height - right_height) <= 1
        height = 1 + max(left_height, right_height)
        
        return is_balanced, height
    
    # Return whether the tree is balanced
    return check_balanced(root)[0]
```

### **Step 4: Complexity Analysis (Optimized - Bottom-Up)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is much more efficient than the brute force approach, especially for large trees.

### **Step 5: Visualizing Computation (Optimized - Bottom-Up)**

* For the example tree [3,9,20,null,null,15,7]:
  * check_balanced(3):
    * check_balanced(3.left) = check_balanced(9):
      * check_balanced(9.left) = check_balanced(null) = (True, 0)
      * check_balanced(9.right) = check_balanced(null) = (True, 0)
      * is_balanced = abs(0 - 0) <= 1 = True
      * height = 1 + max(0, 0) = 1
      * Return (True, 1)
    * check_balanced(3.right) = check_balanced(20):
      * check_balanced(20.left) = check_balanced(15):
        * check_balanced(15.left) = check_balanced(null) = (True, 0)
        * check_balanced(15.right) = check_balanced(null) = (True, 0)
        * is_balanced = abs(0 - 0) <= 1 = True
        * height = 1 + max(0, 0) = 1
        * Return (True, 1)
      * check_balanced(20.right) = check_balanced(7):
        * check_balanced(7.left) = check_balanced(null) = (True, 0)
        * check_balanced(7.right) = check_balanced(null) = (True, 0)
        * is_balanced = abs(0 - 0) <= 1 = True
        * height = 1 + max(0, 0) = 1
        * Return (True, 1)
      * is_balanced = abs(1 - 1) <= 1 = True
      * height = 1 + max(1, 1) = 2
      * Return (True, 2)
    * is_balanced = abs(1 - 2) <= 1 = True
    * height = 1 + max(1, 2) = 3
    * Return (True, 3)
  * Return True
* We're efficiently checking if the tree is balanced and calculating its height in a single traversal.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(h) | Calculates heights multiple times |
| Optimized (Bottom-Up) | O(n) | O(h) | Calculates heights once |

---

## **Final Takeaways**

* The key to efficiently determining if a binary tree is height-balanced is to avoid redundant calculations of heights.
* By using a bottom-up approach, we can check if a tree is balanced and calculate its height in a single traversal.
* This problem demonstrates how a post-order traversal can be used to efficiently solve tree problems that require information from subtrees.

---

## **Real-life Analogy**

* Imagine you're checking if a mobile hanging from the ceiling is balanced. The brute force approach would be like measuring the height of each branch multiple times, which is inefficient. The optimized approach would be like starting from the leaves and working your way up, checking if each branch is balanced and calculating its height as you go. This way, you only need to measure each branch once.

---

## **Summary**

* We solved the "Balanced Binary Tree" problem by first considering a brute force approach that calculates the height of each subtree multiple times, resulting in O(n²) time complexity. We then optimized using a bottom-up approach that checks if a tree is balanced and calculates its height in a single traversal, reducing the time complexity to O(n). The key insight is to use a post-order traversal to first check if the left and right subtrees are balanced, and then check if the current node is balanced. 