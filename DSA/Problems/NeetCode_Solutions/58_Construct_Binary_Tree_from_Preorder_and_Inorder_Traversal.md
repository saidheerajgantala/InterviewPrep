# 📝 Construct Binary Tree from Preorder and Inorder Traversal

## **Problem Statement**

* Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return the binary tree.
* Example:
  * Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
  * Output: [3,9,20,null,null,15,7]
  * Explanation: The binary tree looks like:
    ```
        3
       / \
      9  20
        /  \
       15   7
    ```
* Constraints:
  * 1 <= preorder.length <= 3000
  * inorder.length == preorder.length
  * -3000 <= preorder[i], inorder[i] <= 3000
  * preorder and inorder consist of unique values.
  * Each value of inorder also appears in preorder.
  * preorder is guaranteed to be the preorder traversal of the tree.
  * inorder is guaranteed to be the inorder traversal of the tree.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to construct a binary tree from its preorder and inorder traversals
* The preorder traversal visits the root first, then the left subtree, then the right subtree
* The inorder traversal visits the left subtree first, then the root, then the right subtree
* We can use these properties to recursively construct the tree:
  * The first element in the preorder traversal is the root of the tree
  * We can find this element in the inorder traversal to determine the sizes of the left and right subtrees
  * We can then recursively construct the left and right subtrees

---

## **Step 2: Flow Steps (Brute Force)**

1. If the preorder or inorder traversal is empty, return null
2. The first element in the preorder traversal is the root of the tree
3. Find the index of the root in the inorder traversal
4. The elements to the left of the root in the inorder traversal form the left subtree
5. The elements to the right of the root in the inorder traversal form the right subtree
6. Recursively construct the left and right subtrees
7. Return the root

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def buildTree(preorder, inorder):
    if not preorder or not inorder:
        return None
    
    # The first element in preorder is the root
    root_val = preorder[0]
    root = TreeNode(root_val)
    
    # Find the index of the root in inorder
    root_index = inorder.index(root_val)
    
    # Recursively construct the left and right subtrees
    root.left = buildTree(preorder[1:root_index+1], inorder[:root_index])
    root.right = buildTree(preorder[root_index+1:], inorder[root_index+1:])
    
    return root
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the number of nodes in the tree. For each node, we spend O(n) time finding its index in the inorder traversal and creating subarrays, resulting in O(n²) time overall.
* Space Complexity: O(n) for the recursion stack and the new subarrays created at each recursive call.
* This approach is inefficient for large trees due to the repeated linear searches and array slicing.

---

## **Step 5: Visualizing Redundant Computation**

* For preorder = [3,9,20,15,7] and inorder = [9,3,15,20,7]:
  * Root = 3, root_index = 1 in inorder
  * Left subtree: preorder = [9], inorder = [9]
  * Right subtree: preorder = [20,15,7], inorder = [15,20,7]
  * For the left subtree:
    * Root = 9, root_index = 0 in inorder
    * Left subtree: preorder = [], inorder = []
    * Right subtree: preorder = [], inorder = []
  * For the right subtree:
    * Root = 20, root_index = 1 in inorder
    * Left subtree: preorder = [15], inorder = [15]
    * Right subtree: preorder = [7], inorder = [7]
* We're repeatedly searching for the root's index in the inorder traversal and creating new subarrays, which is inefficient
* We can optimize this by using a hash map to store the indices of elements in the inorder traversal and by using indices instead of slicing arrays

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Hash Map and Index-Based approach.
* The property that matches this problem is the relationship between preorder and inorder traversals: the first element in preorder is the root, and the elements to its left and right in inorder form the left and right subtrees.
* By using a hash map to store the indices of elements in the inorder traversal, we can avoid repeated linear searches.
* By using indices instead of slicing arrays, we can avoid the overhead of creating new subarrays.
* Alternatives like the brute force approach would be less efficient due to the repeated linear searches and array slicing.
* Precondition: The preorder and inorder traversals are valid and represent the same tree.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Hash Map and Index-Based)**

* We can optimize the brute force approach in two ways:
  * Use a hash map to store the indices of elements in the inorder traversal, allowing O(1) lookups
  * Use indices to represent subarrays instead of actually creating new subarrays
* This will reduce the time complexity from O(n²) to O(n)

### **Step 2: Flow Steps (Optimized - Hash Map and Index-Based)**

1. Create a hash map to store the indices of elements in the inorder traversal
2. Define a recursive function to build the tree from a range of indices in the preorder and inorder traversals
3. In the recursive function:
   1. If the range is empty, return null
   2. The first element in the preorder range is the root of the tree
   3. Find the index of the root in the inorder traversal using the hash map
   4. Calculate the size of the left subtree
   5. Recursively construct the left and right subtrees
   6. Return the root
4. Call the recursive function with the full range of indices
5. Return the result

### **Step 3: Implementation (Optimized - Hash Map and Index-Based)**

```python
def buildTree(preorder, inorder):
    # Create a hash map to store the indices of elements in the inorder traversal
    inorder_map = {val: idx for idx, val in enumerate(inorder)}
    
    def build(preorder_start, preorder_end, inorder_start, inorder_end):
        if preorder_start > preorder_end or inorder_start > inorder_end:
            return None
        
        # The first element in preorder is the root
        root_val = preorder[preorder_start]
        root = TreeNode(root_val)
        
        # Find the index of the root in inorder
        root_index = inorder_map[root_val]
        
        # Calculate the size of the left subtree
        left_size = root_index - inorder_start
        
        # Recursively construct the left and right subtrees
        root.left = build(preorder_start + 1, preorder_start + left_size, inorder_start, root_index - 1)
        root.right = build(preorder_start + left_size + 1, preorder_end, root_index + 1, inorder_end)
        
        return root
    
    return build(0, len(preorder) - 1, 0, len(inorder) - 1)
```

### **Step 4: Complexity Analysis (Optimized - Hash Map and Index-Based)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We process each node exactly once, and each operation (hash map lookup, index calculation) takes O(1) time.
* Space Complexity: O(n) for the hash map and the recursion stack.
* This approach is much more efficient than the brute force approach, especially for large trees.

### **Step 5: Visualizing Computation (Optimized - Hash Map and Index-Based)**

* For preorder = [3,9,20,15,7] and inorder = [9,3,15,20,7]:
  * inorder_map = {9: 0, 3: 1, 15: 2, 20: 3, 7: 4}
  * build(0, 4, 0, 4):
    * root_val = preorder[0] = 3
    * root_index = inorder_map[3] = 1
    * left_size = 1 - 0 = 1
    * root.left = build(1, 1, 0, 0):
      * root_val = preorder[1] = 9
      * root_index = inorder_map[9] = 0
      * left_size = 0 - 0 = 0
      * root.left = build(2, 1, 0, -1): return None
      * root.right = build(2, 1, 1, 0): return None
      * Return TreeNode(9)
    * root.right = build(2, 4, 2, 4):
      * root_val = preorder[2] = 20
      * root_index = inorder_map[20] = 3
      * left_size = 3 - 2 = 1
      * root.left = build(3, 3, 2, 2):
        * root_val = preorder[3] = 15
        * root_index = inorder_map[15] = 2
        * left_size = 2 - 2 = 0
        * root.left = build(4, 3, 2, 1): return None
        * root.right = build(4, 3, 3, 2): return None
        * Return TreeNode(15)
      * root.right = build(4, 4, 4, 4):
        * root_val = preorder[4] = 7
        * root_index = inorder_map[7] = 4
        * left_size = 4 - 4 = 0
        * root.left = build(5, 4, 4, 3): return None
        * root.right = build(5, 4, 5, 4): return None
        * Return TreeNode(7)
      * Return TreeNode(20, TreeNode(15), TreeNode(7))
    * Return TreeNode(3, TreeNode(9), TreeNode(20, TreeNode(15), TreeNode(7)))
* We're efficiently constructing the tree without repeated linear searches or array slicing.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(n) | Uses linear search and array slicing |
| Optimized (Hash Map and Index-Based) | O(n) | O(n) | Uses hash map and indices |

---

## **Final Takeaways**

* The key to efficiently constructing a binary tree from preorder and inorder traversals is to understand the relationship between these traversals.
* By using a hash map to store the indices of elements in the inorder traversal, we can avoid repeated linear searches.
* By using indices to represent subarrays instead of actually creating new subarrays, we can avoid the overhead of array slicing.
* This problem demonstrates how understanding the properties of tree traversals and using appropriate data structures can lead to more efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're trying to reconstruct a family tree based on two different lists of family members: one list (preorder) where the oldest ancestor is first, followed by all descendants of the first child, then all descendants of the second child, and so on; and another list (inorder) where all descendants of the first child are listed first, then the ancestor, then all descendants of the second child. By combining these two lists and understanding their relationship, you can reconstruct the entire family tree. The optimized approach is like having a directory that tells you exactly where each person is in the inorder list, so you don't have to search for them every time.

---

## **Summary**

* We solved the "Construct Binary Tree from Preorder and Inorder Traversal" problem by first considering a brute force approach that uses linear search and array slicing, resulting in O(n²) time complexity. We then optimized using a hash map to store the indices of elements in the inorder traversal and using indices to represent subarrays, reducing the time complexity to O(n). The key insight is to leverage the relationship between preorder and inorder traversals: the first element in preorder is the root, and the elements to its left and right in inorder form the left and right subtrees. 