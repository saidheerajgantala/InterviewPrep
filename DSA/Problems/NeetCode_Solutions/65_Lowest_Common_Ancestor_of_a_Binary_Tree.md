# 📝 Lowest Common Ancestor of a Binary Tree

## **Problem Statement**

* Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.
* According to the definition of LCA on Wikipedia: "The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself)."
* Example 1:
  * Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
  * Output: 3
  * Explanation: The binary tree looks like:
    ```
            3
           / \
          5   1
         / \ / \
        6  2 0  8
          / \
         7   4
    ```
    The LCA of nodes 5 and 1 is 3.
* Example 2:
  * Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
  * Output: 5
  * Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
* Constraints:
  * The number of nodes in the tree is in the range [2, 10^5].
  * -10^9 <= Node.val <= 10^9
  * All Node.val are unique.
  * p != q
  * p and q will exist in the tree.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the lowest common ancestor (LCA) of two nodes in a binary tree
* One approach would be to find the paths from the root to both nodes and then find the last common node in these paths
* This approach works for any binary tree

---

## **Step 2: Flow Steps (Brute Force)**

1. Find the path from the root to node p
2. Find the path from the root to node q
3. Compare the two paths and find the last common node
4. Return the last common node as the LCA

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

def lowestCommonAncestor(root, p, q):
    # Helper function to find the path from root to a node
    def find_path(node, target, path):
        if not node:
            return False
        
        path.append(node)
        
        if node == target:
            return True
        
        if find_path(node.left, target, path) or find_path(node.right, target, path):
            return True
        
        path.pop()
        return False
    
    # Find paths from root to p and q
    path_p = []
    path_q = []
    
    find_path(root, p, path_p)
    find_path(root, q, path_q)
    
    # Find the last common node in the two paths
    i = 0
    while i < len(path_p) and i < len(path_q) and path_p[i] == path_q[i]:
        i += 1
    
    # The last common node is at index i-1
    return path_p[i-1]
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the tree. In the worst case, we might need to traverse the entire tree to find the paths to p and q.
* Space Complexity: O(h) where h is the height of the tree. This is for storing the paths to p and q. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is straightforward but requires two traversals of the tree and extra space to store the paths.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree with p = 5 and q = 1:
  * Find path to p: [3, 5]
  * Find path to q: [3, 1]
  * Compare paths: The last common node is 3
* For the example tree with p = 5 and q = 4:
  * Find path to p: [3, 5]
  * Find path to q: [3, 5, 2, 4]
  * Compare paths: The last common node is 5
* We're traversing the tree twice to find the paths to p and q, which is not necessary

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Recursive approach.
* The property that matches this problem is that the LCA of two nodes p and q is the node that first encounters p or q in a post-order traversal, and also encounters the other node in its subtree.
* By using a post-order traversal, we can efficiently find the LCA in a single traversal.
* Alternatives like the brute force approach would be less efficient as they require two traversals of the tree.
* Precondition: Both p and q exist in the tree.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Recursive)**

* We can use a recursive approach to find the LCA in a single traversal
* The key insight is that if we find either p or q, we return that node
* If we find p in one subtree and q in the other subtree, then the current node is the LCA
* If we find p or q in one subtree and nothing in the other subtree, then the found node is the LCA

### **Step 2: Flow Steps (Optimized - Recursive)**

1. If the current node is null, return null
2. If the current node is p or q, return the current node
3. Recursively search for p and q in the left and right subtrees
4. If both left and right recursive calls return non-null values, then the current node is the LCA
5. Otherwise, return the non-null value returned by the recursive calls

### **Step 3: Implementation (Optimized - Recursive)**

```python
def lowestCommonAncestor(root, p, q):
    # Base case: if the current node is null, return null
    if not root:
        return None
    
    # If the current node is p or q, return the current node
    if root == p or root == q:
        return root
    
    # Recursively search for p and q in the left and right subtrees
    left = lowestCommonAncestor(root.left, p, q)
    right = lowestCommonAncestor(root.right, p, q)
    
    # If both left and right recursive calls return non-null values,
    # then the current node is the LCA
    if left and right:
        return root
    
    # Otherwise, return the non-null value returned by the recursive calls
    return left if left else right
```

### **Step 4: Complexity Analysis (Optimized - Recursive)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node at most once.
* Space Complexity: O(h) where h is the height of the tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to n, resulting in O(n) space complexity.
* This approach is more efficient than the brute force approach as it requires only one traversal of the tree and doesn't need extra space to store the paths.

### **Step 5: Visualizing Computation (Optimized - Recursive)**

* For the example tree with p = 5 and q = 1:
  * lowestCommonAncestor(3, 5, 1):
    * root = 3, not p or q
    * left = lowestCommonAncestor(5, 5, 1):
      * root = 5, equals p, return 5
    * right = lowestCommonAncestor(1, 5, 1):
      * root = 1, equals q, return 1
    * left and right are both non-null, so return 3
* For the example tree with p = 5 and q = 4:
  * lowestCommonAncestor(3, 5, 4):
    * root = 3, not p or q
    * left = lowestCommonAncestor(5, 5, 4):
      * root = 5, equals p, return 5
      * (Note: The recursive call continues to search for q in the subtree of 5, but we're simplifying here)
    * right = lowestCommonAncestor(1, 5, 4):
      * root = 1, not p or q
      * left = lowestCommonAncestor(0, 5, 4) = null
      * right = lowestCommonAncestor(8, 5, 4) = null
      * both left and right are null, so return null
    * left is non-null, right is null, so return left = 5
* We're efficiently finding the LCA in a single traversal.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Find Paths) | O(n) | O(h) | Requires two traversals and extra space |
| Optimized (Recursive) | O(n) | O(h) | Requires one traversal |

---

## **Final Takeaways**

* The key to efficiently finding the lowest common ancestor in a binary tree is to use a post-order traversal.
* By checking if the current node is p or q, and recursively searching for p and q in the left and right subtrees, we can find the LCA in a single traversal.
* This problem demonstrates how a post-order traversal can be used to efficiently solve tree problems that require information from subtrees.

---

## **Real-life Analogy**

* Imagine you're trying to find the lowest common ancestor in a family tree. The brute force approach would be like tracing the paths from the root to each person and then finding the last common ancestor. The optimized approach would be like starting from the root and asking each person: "Do you have person P in your descendants? Do you have person Q in your descendants?" If a person has both P and Q in their descendants, but neither of their children has both P and Q, then that person is the lowest common ancestor.

---

## **Summary**

* We solved the "Lowest Common Ancestor of a Binary Tree" problem by first considering a brute force approach that finds the paths from the root to both nodes and then finds the last common node, resulting in O(n) time complexity but requiring two traversals. We then optimized using a recursive approach that finds the LCA in a single traversal, also with O(n) time complexity but more efficient in practice. The key insight is to use a post-order traversal to check if the current node is p or q, and recursively search for p and q in the left and right subtrees. 