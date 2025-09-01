# 📝 Lowest Common Ancestor of a Binary Search Tree

## **Problem Statement**

* Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.
* According to the definition of LCA on Wikipedia: "The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself)."
* Example 1:
  * Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
  * Output: 6
  * Explanation: The binary tree looks like:
    ```
          6
        /   \
       2     8
      / \   / \
     0   4 7   9
        / \
       3   5
    ```
    The LCA of nodes 2 and 8 is 6.
* Example 2:
  * Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
  * Output: 2
  * Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
* Constraints:
  * The number of nodes in the tree is in the range [2, 10^5].
  * -10^9 <= Node.val <= 10^9
  * All Node.val are unique.
  * p != q
  * p and q will exist in the BST.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the lowest common ancestor (LCA) of two nodes in a binary search tree (BST)
* One approach would be to find the paths from the root to both nodes and then find the last common node in these paths
* This approach works for any binary tree, not just a BST
* However, we can leverage the properties of a BST to solve this problem more efficiently

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
    # Find the path from root to a node
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
* This approach doesn't leverage the properties of a BST and is not the most efficient for this specific problem.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree with p = 2 and q = 8:
  * Path to p: [6, 2]
  * Path to q: [6, 8]
  * Last common node: 6
* We're traversing the tree twice to find the paths to p and q, which is not necessary
* We can leverage the properties of a BST to find the LCA more efficiently

---

## **Why are we using this algorithm?**

* For optimization, we'll use a BST Property-Based approach.
* The property that matches this problem is that in a BST, all nodes in the left subtree have values less than the node's value, and all nodes in the right subtree have values greater than the node's value.
* By comparing the values of p and q with the current node's value, we can determine which subtree to search next.
* Alternatives like the brute force approach would be less efficient as they don't leverage the BST property.
* Precondition: The tree is a valid BST, and both p and q exist in the tree.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - BST Property-Based)**

* We can leverage the properties of a BST to find the LCA more efficiently
* If both p and q are smaller than the current node, then the LCA must be in the left subtree
* If both p and q are larger than the current node, then the LCA must be in the right subtree
* If one is smaller and one is larger (or one is equal to the current node), then the current node is the LCA

### **Step 2: Flow Steps (Optimized - BST Property-Based)**

1. Start from the root of the BST
2. If both p and q are smaller than the current node, recursively search in the left subtree
3. If both p and q are larger than the current node, recursively search in the right subtree
4. Otherwise, the current node is the LCA (one is in the left subtree and one is in the right subtree, or one of them is the current node)
5. Return the LCA

### **Step 3: Implementation (Optimized - BST Property-Based)**

```python
def lowestCommonAncestor(root, p, q):
    # If both p and q are smaller than the current node, search in the left subtree
    if p.val < root.val and q.val < root.val:
        return lowestCommonAncestor(root.left, p, q)
    
    # If both p and q are larger than the current node, search in the right subtree
    if p.val > root.val and q.val > root.val:
        return lowestCommonAncestor(root.right, p, q)
    
    # Otherwise, the current node is the LCA
    return root
```

### **Alternative Approach: Iterative**

We can also implement the same approach iteratively:

```python
def lowestCommonAncestor(root, p, q):
    current = root
    
    while current:
        # If both p and q are smaller than the current node, search in the left subtree
        if p.val < current.val and q.val < current.val:
            current = current.left
        # If both p and q are larger than the current node, search in the right subtree
        elif p.val > current.val and q.val > current.val:
            current = current.right
        # Otherwise, the current node is the LCA
        else:
            return current
    
    return None  # This should not happen given the constraints
```

### **Step 4: Complexity Analysis (Optimized - BST Property-Based)**

* Time Complexity: O(h) where h is the height of the tree. In the worst case, we might need to traverse from the root to a leaf, which takes O(h) time. For a balanced BST, h is O(log n).
* Space Complexity: O(h) for the recursive approach due to the recursion stack, and O(1) for the iterative approach.
* This approach is much more efficient than the brute force approach, especially for balanced BSTs.

### **Step 5: Visualizing Computation (Optimized - BST Property-Based)**

* For the example tree with p = 2 and q = 8:
  * Start at root = 6
  * p.val = 2 < 6 and q.val = 8 > 6, so the LCA is 6
* For the example tree with p = 2 and q = 4:
  * Start at root = 6
  * p.val = 2 < 6 and q.val = 4 < 6, so search in the left subtree
  * Now at node = 2
  * p.val = 2 == 2 and q.val = 4 > 2, so the LCA is 2
* We're efficiently finding the LCA by leveraging the BST property.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Find Paths) | O(n) | O(h) | Finds paths from root to p and q |
| Optimized (BST Property-Based) - Recursive | O(h) | O(h) | Leverages BST property |
| Optimized (BST Property-Based) - Iterative | O(h) | O(1) | Leverages BST property without recursion |

---

## **Final Takeaways**

* The key to efficiently finding the LCA in a BST is to leverage the BST property: all nodes in the left subtree have values less than the node's value, and all nodes in the right subtree have values greater than the node's value.
* By comparing the values of p and q with the current node's value, we can determine which subtree to search next.
* Both recursive and iterative approaches can be used, with the iterative approach being more space-efficient.
* This problem demonstrates how understanding the properties of the data structure can lead to more efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're looking for the lowest common ancestor in a family tree where all members are arranged by age (younger members to the left, older members to the right). If you're looking for the LCA of two people, you can start from the oldest ancestor and ask: "Are both people younger than me?" If yes, look at my younger child. "Are both people older than me?" If yes, look at my older child. Otherwise, I am the LCA because one person is in my younger branch and one is in my older branch (or one of them is me).

---

## **Summary**

* We solved the "Lowest Common Ancestor of a Binary Search Tree" problem by first considering a brute force approach that finds the paths from the root to both nodes and then finds the last common node, resulting in O(n) time complexity. We then optimized using a BST property-based approach that leverages the fact that all nodes in the left subtree have values less than the node's value, and all nodes in the right subtree have values greater than the node's value, reducing the time complexity to O(h). We explored both recursive and iterative implementations of the optimized approach, with the iterative approach being more space-efficient. Both optimized approaches efficiently find the LCA in a BST, with the choice between them depending on the specific requirements of the problem. 