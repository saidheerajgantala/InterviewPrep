# 📝 Subtree of Another Tree

## **Problem Statement**

* Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values as `subRoot` and `false` otherwise.
* A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.
* Example 1:
  * Input: root = [3,4,5,1,2], subRoot = [4,1,2]
  * Output: true
  * Explanation: The binary trees look like:
    ```
        3
       / \
      4   5
     / \
    1   2
    ```
    and
    ```
      4
     / \
    1   2
    ```
    The tree with root [4,1,2] is a subtree of the tree with root [3,4,5,1,2].
* Example 2:
  * Input: root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
  * Output: false
  * Explanation: The binary trees look like:
    ```
        3
       / \
      4   5
     / \
    1   2
         /
        0
    ```
    and
    ```
      4
     / \
    1   2
    ```
    The tree with root [4,1,2] is not a subtree of the tree with root [3,4,5,1,2,null,null,null,null,0] because the node with value 2 in the subtree has a child with value 0, but the corresponding node in the tree does not.
* Constraints:
  * The number of nodes in the `root` tree is in the range [1, 2000].
  * The number of nodes in the `subRoot` tree is in the range [1, 1000].
  * -10^4 <= root.val <= 10^4
  * -10^4 <= subRoot.val <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine if `subRoot` is a subtree of `root`
* One approach would be to traverse the `root` tree and for each node, check if the subtree rooted at that node is identical to the `subRoot` tree
* To check if two trees are identical, we can use a recursive function that compares the values of corresponding nodes and recursively checks their left and right subtrees

---

## **Step 2: Flow Steps (Brute Force)**

1. Define a function `isSubtree(root, subRoot)` that returns true if `subRoot` is a subtree of `root`
2. Define a helper function `isSameTree(p, q)` that returns true if the trees rooted at `p` and `q` are identical
3. In `isSubtree`:
   1. If `root` is null, return false (an empty tree cannot contain a non-empty subtree)
   2. If `isSameTree(root, subRoot)` returns true, return true
   3. Otherwise, recursively check if `subRoot` is a subtree of `root.left` or `root.right`
4. In `isSameTree`:
   1. If both `p` and `q` are null, return true (two empty trees are identical)
   2. If one of `p` or `q` is null but the other is not, return false (the trees have different structures)
   3. If the values of `p` and `q` are different, return false
   4. Otherwise, recursively check if the left subtrees of `p` and `q` are identical and if the right subtrees of `p` and `q` are identical

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isSubtree(root, subRoot):
    # Helper function to check if two trees are identical
    def isSameTree(p, q):
        if not p and not q:
            return True
        if not p or not q:
            return False
        return p.val == q.val and isSameTree(p.left, q.left) and isSameTree(p.right, q.right)
    
    # Base case: if root is None, subRoot cannot be a subtree of root
    if not root:
        return False
    
    # Check if the tree rooted at root is identical to subRoot
    if isSameTree(root, subRoot):
        return True
    
    # Recursively check if subRoot is a subtree of root.left or root.right
    return isSubtree(root.left, subRoot) or isSubtree(root.right, subRoot)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(m * n) where m is the number of nodes in the `root` tree and n is the number of nodes in the `subRoot` tree. In the worst case, we need to check if the subtree rooted at each node of `root` is identical to `subRoot`, and each check takes O(n) time.
* Space Complexity: O(h) where h is the height of the `root` tree. This is due to the recursion stack. In the worst case (a skewed tree), h can be equal to m, resulting in O(m) space complexity.
* This approach is straightforward but can be inefficient for large trees.

---

## **Step 5: Visualizing Redundant Computation**

* For the example root = [3,4,5,1,2] and subRoot = [4,1,2]:
  * isSubtree(3, 4):
    * isSameTree(3, 4): 3 != 4, so return false
    * isSubtree(4, 4):
      * isSameTree(4, 4): 4 == 4, and the left and right subtrees are identical, so return true
    * Return true
* For the example root = [3,4,5,1,2,null,null,null,null,0] and subRoot = [4,1,2]:
  * isSubtree(3, 4):
    * isSameTree(3, 4): 3 != 4, so return false
    * isSubtree(4, 4):
      * isSameTree(4, 4): 4 == 4, but the right subtrees are not identical (one has a child with value 0), so return false
    * isSubtree(5, 4):
      * isSameTree(5, 4): 5 != 4, so return false
      * isSubtree(null, 4): return false
      * isSubtree(null, 4): return false
    * Return false
* There's some redundant computation in checking if trees are identical, especially for large trees with similar structures

---

## **Why are we using this algorithm?**

* For optimization, we'll consider a String Matching approach.
* The property that matches this problem is that we can represent a tree as a string using a traversal algorithm, and then check if the string representation of `subRoot` is a substring of the string representation of `root`.
* By using a more efficient string matching algorithm, we can potentially reduce the time complexity.
* Alternatives like the brute force approach would be less efficient for large trees.
* Precondition: We need to ensure that the string representation uniquely identifies a tree.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - String Matching)**

* We can represent a tree as a string using a traversal algorithm (e.g., preorder traversal)
* To ensure that the string representation uniquely identifies a tree, we need to include null nodes in the traversal
* Once we have the string representations of both trees, we can check if the string representation of `subRoot` is a substring of the string representation of `root`
* This approach can be more efficient than the brute force approach, especially for large trees

### **Step 2: Flow Steps (Optimized - String Matching)**

1. Define a function to serialize a tree into a string using preorder traversal
2. Serialize both `root` and `subRoot` into strings
3. Check if the serialized `subRoot` is a substring of the serialized `root`
4. Return the result

### **Step 3: Implementation (Optimized - String Matching)**

```python
def isSubtree(root, subRoot):
    # Helper function to serialize a tree into a string
    def serialize(node):
        if not node:
            return "#"
        # Use a special delimiter to avoid false positives
        return "^" + str(node.val) + "^" + serialize(node.left) + "^" + serialize(node.right)
    
    # Serialize both trees
    serialized_root = serialize(root)
    serialized_subroot = serialize(subRoot)
    
    # Check if serialized_subroot is a substring of serialized_root
    return serialized_subroot in serialized_root
```

### **Alternative Approach: Improved Brute Force**

While the string matching approach is elegant, it can be inefficient for very large trees due to the overhead of string operations. We can improve the brute force approach by adding an early termination condition:

```python
def isSubtree(root, subRoot):
    # Helper function to check if two trees are identical
    def isSameTree(p, q):
        if not p and not q:
            return True
        if not p or not q:
            return False
        return p.val == q.val and isSameTree(p.left, q.left) and isSameTree(p.right, q.right)
    
    # Base cases
    if not subRoot:
        return True  # An empty tree is a subtree of any tree
    if not root:
        return False  # A non-empty tree cannot be a subtree of an empty tree
    
    # Check if the tree rooted at root is identical to subRoot
    if isSameTree(root, subRoot):
        return True
    
    # Recursively check if subRoot is a subtree of root.left or root.right
    return isSubtree(root.left, subRoot) or isSubtree(root.right, subRoot)
```

### **Step 4: Complexity Analysis (Optimized - String Matching)**

* Time Complexity: O(m + n) where m is the number of nodes in the `root` tree and n is the number of nodes in the `subRoot` tree. Serializing both trees takes O(m) and O(n) time respectively, and checking if one string is a substring of another can be done in O(m + n) time using efficient string matching algorithms like KMP or Rabin-Karp.
* Space Complexity: O(m + n) for storing the serialized strings.
* This approach can be more efficient than the brute force approach for large trees, but it uses more space.

### **Step 5: Visualizing Computation (Optimized - String Matching)**

* For the example root = [3,4,5,1,2] and subRoot = [4,1,2]:
  * Serialize root: "^3^^4^^1^#^#^^2^#^#^^^5^#^#^"
  * Serialize subRoot: "^4^^1^#^#^^2^#^#^"
  * Check if "^4^^1^#^#^^2^#^#^" is a substring of "^3^^4^^1^#^#^^2^#^#^^^5^#^#^": true
* For the example root = [3,4,5,1,2,null,null,null,null,0] and subRoot = [4,1,2]:
  * Serialize root: "^3^^4^^1^#^#^^2^#^^0^#^#^^^5^#^#^"
  * Serialize subRoot: "^4^^1^#^#^^2^#^#^"
  * Check if "^4^^1^#^#^^2^#^#^" is a substring of "^3^^4^^1^#^#^^2^#^^0^#^#^^^5^#^#^": false
* We're efficiently checking if `subRoot` is a subtree of `root` using string matching.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(m * n) | O(h) | Checks each node in root |
| Optimized (String Matching) | O(m + n) | O(m + n) | Uses string representation |
| Improved Brute Force | O(m * n) | O(h) | Adds early termination |

---

## **Final Takeaways**

* The key to efficiently determining if one tree is a subtree of another is to either check each node in the first tree or to use a string representation of the trees.
* The brute force approach is more intuitive and uses less space, but can be inefficient for large trees.
* The string matching approach can be more efficient in terms of time complexity, but uses more space and may have overhead due to string operations.
* The choice between the two approaches might depend on the specific requirements of the problem and the expected sizes of the trees.

---

## **Real-life Analogy**

* Imagine you're trying to find if a smaller family tree is a part of a larger family tree. The brute force approach would be like starting from each person in the larger family tree and checking if the subtree rooted at that person matches the smaller family tree. The string matching approach would be like writing down a description of both family trees and checking if the description of the smaller tree appears within the description of the larger tree.

---

## **Summary**

* We solved the "Subtree of Another Tree" problem by first considering a brute force approach that checks if the subtree rooted at each node of the first tree is identical to the second tree, resulting in O(m * n) time complexity. We then optimized using a string matching approach that represents both trees as strings and checks if one is a substring of the other, potentially reducing the time complexity to O(m + n) but increasing the space complexity to O(m + n). We also explored an improved brute force approach that adds an early termination condition. The choice between these approaches depends on the specific requirements and constraints of the problem. 