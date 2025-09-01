# 📝 Validate Binary Search Tree

## **Problem Statement**

* Given the root of a binary tree, determine if it is a valid binary search tree (BST).
* A valid BST is defined as follows:
  * The left subtree of a node contains only nodes with keys less than the node's key.
  * The right subtree of a node contains only nodes with keys greater than the node's key.
  * Both the left and right subtrees must also be binary search trees.
* Example 1:
  * Input: root = [2,1,3]
  * Output: true
  * Explanation: The binary tree looks like:
    ```
        2
       / \
      1   3
    ```
    It is a valid BST.
* Example 2:
  * Input: root = [5,1,4,null,null,3,6]
  * Output: false
  * Explanation: The binary tree looks like:
    ```
        5
       / \
      1   4
         / \
        3   6
    ```
    The value of the root's right child is 4, but it should be greater than 5.
* Constraints:
  * The number of nodes in the tree is in the range [1, 10^4].
  * -2^31 <= Node.val <= 2^31 - 1

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine if a binary tree is a valid binary search tree (BST)
* One approach would be to check if the tree satisfies the BST property:
  * All nodes in the left subtree have values less than the node's value
  * All nodes in the right subtree have values greater than the node's value
  * Both the left and right subtrees are also valid BSTs
* We can use a recursive approach to check these conditions for each node

---

## **Step 2: Flow Steps (Brute Force)**

1. Define a recursive function to check if a subtree is a valid BST:
   1. If the current node is null, return true (an empty tree is a valid BST)
   2. Check if all nodes in the left subtree have values less than the current node's value
   3. Check if all nodes in the right subtree have values greater than the current node's value
   4. Recursively check if the left and right subtrees are valid BSTs
   5. Return true if all conditions are met, false otherwise
2. Call the recursive function with the root
3. Return the result

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isValidBST(root):
    def is_valid_subtree(node):
        if not node:
            return True
        
        # Check if all nodes in the left subtree have values less than the current node's value
        if node.left:
            left_max = find_max(node.left)
            if left_max >= node.val:
                return False
        
        # Check if all nodes in the right subtree have values greater than the current node's value
        if node.right:
            right_min = find_min(node.right)
            if right_min <= node.val:
                return False
        
        # Recursively check if the left and right subtrees are valid BSTs
        return is_valid_subtree(node.left) and is_valid_subtree(node.right)
    
    def find_max(node):
        while node.right:
            node = node.right
        return node.val
    
    def find_min(node):
        while node.left:
            node = node.left
        return node.val
    
    return is_valid_subtree(root)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the number of nodes in the tree. In the worst case, for each node, we traverse its subtree to find the maximum and minimum values, which can take O(n) time. Since we do this for each of the n nodes, the overall time complexity is O(n²).
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is inefficient for large trees due to the redundant computations in finding the maximum and minimum values for each subtree.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree [5,1,4,null,null,3,6]:
  * is_valid_subtree(5):
    * Check left subtree: find_max(1) = 1, 1 < 5, so it's valid
    * Check right subtree: find_min(4) = 3, 3 < 5, so it's not valid
    * Return false
* We're repeatedly traversing the subtrees to find the maximum and minimum values, which is inefficient
* We can optimize this by passing the valid range for each subtree during the recursive calls

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Range-Based Recursive approach.
* The property that matches this problem is the BST property: all nodes in the left subtree have values less than the node's value, and all nodes in the right subtree have values greater than the node's value.
* By passing the valid range for each subtree during the recursive calls, we can avoid the redundant computations in finding the maximum and minimum values.
* Alternatives like the brute force approach would be less efficient due to the redundant computations.
* Precondition: We need to be able to traverse the tree.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Range-Based Recursive)**

* Instead of checking all nodes in the subtree, we can pass the valid range for each node during the recursive calls
* For the left subtree, the upper bound is the parent node's value
* For the right subtree, the lower bound is the parent node's value
* This way, we can check if a node's value is within the valid range in constant time

### **Step 2: Flow Steps (Optimized - Range-Based Recursive)**

1. Define a recursive function to check if a subtree is a valid BST with a valid range:
   1. If the current node is null, return true (an empty tree is a valid BST)
   2. Check if the current node's value is within the valid range
   3. Recursively check if the left subtree is a valid BST with an updated upper bound
   4. Recursively check if the right subtree is a valid BST with an updated lower bound
   5. Return true if all conditions are met, false otherwise
2. Call the recursive function with the root and the initial range of (-infinity, infinity)
3. Return the result

### **Step 3: Implementation (Optimized - Range-Based Recursive)**

```python
def isValidBST(root):
    def is_valid_bst(node, lower=float('-inf'), upper=float('inf')):
        if not node:
            return True
        
        # Check if the current node's value is within the valid range
        if node.val <= lower or node.val >= upper:
            return False
        
        # Recursively check the left and right subtrees with updated ranges
        return (is_valid_bst(node.left, lower, node.val) and
                is_valid_bst(node.right, node.val, upper))
    
    return is_valid_bst(root)
```

### **Alternative Approach: Inorder Traversal**

Another approach is to use an inorder traversal, which visits the nodes in ascending order for a valid BST:

```python
def isValidBST(root):
    def inorder(node):
        if not node:
            return []
        return inorder(node.left) + [node.val] + inorder(node.right)
    
    # Get the values in inorder traversal
    values = inorder(root)
    
    # Check if the values are in strictly increasing order
    for i in range(1, len(values)):
        if values[i] <= values[i-1]:
            return False
    
    return True
```

### **Step 4: Complexity Analysis (Optimized - Range-Based Recursive)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once.
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is much more efficient than the brute force approach, especially for large trees.

### **Step 5: Visualizing Computation (Optimized - Range-Based Recursive)**

* For the example tree [5,1,4,null,null,3,6]:
  * is_valid_bst(5, -inf, inf):
    * 5 is within the range (-inf, inf), so check its subtrees
    * is_valid_bst(1, -inf, 5):
      * 1 is within the range (-inf, 5), so check its subtrees
      * is_valid_bst(null, -inf, 1): return true
      * is_valid_bst(null, 1, 5): return true
      * Return true
    * is_valid_bst(4, 5, inf):
      * 4 is not within the range (5, inf), so return false
    * Return false
* We're efficiently checking if the tree is a valid BST without redundant computations.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(h) | Repeatedly traverses subtrees to find max and min values |
| Optimized (Range-Based Recursive) | O(n) | O(h) | Passes valid range for each subtree |
| Alternative (Inorder Traversal) | O(n) | O(n) | Checks if inorder traversal is sorted |

---

## **Final Takeaways**

* The key to efficiently validating a binary search tree is to leverage the BST property and avoid redundant computations.
* The range-based recursive approach is more efficient than the brute force approach, as it avoids repeatedly traversing subtrees to find the maximum and minimum values.
* The inorder traversal approach is also efficient and can be more intuitive for some, but it requires additional space to store the traversal result.
* The choice between the two optimized approaches might depend on the specific requirements of the problem and personal preference.

---

## **Real-life Analogy**

* Imagine you're organizing books on a shelf by their publication year, with older books on the left and newer books on the right. The brute force approach would be like checking every book on the left to ensure it's older than the current book, and every book on the right to ensure it's newer, for each book on the shelf. The optimized approach would be like setting a valid range for each section of the shelf and ensuring that all books in that section fall within that range. For example, all books in the left section must be published before 2000, and all books in the right section must be published after 2000.

---

## **Summary**

* We solved the "Validate Binary Search Tree" problem by first considering a brute force approach that checks if all nodes in the left subtree have values less than the node's value and all nodes in the right subtree have values greater than the node's value, resulting in O(n²) time complexity. We then optimized using a range-based recursive approach that passes the valid range for each subtree during the recursive calls, reducing the time complexity to O(n). We also explored an alternative approach using inorder traversal, which also has O(n) time complexity but requires additional space to store the traversal result. Both optimized approaches efficiently validate a binary search tree, with the choice between them depending on the specific requirements and personal preference. 