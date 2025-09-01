# 📝 Kth Smallest Element in a BST

## **Problem Statement**

* Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.
* Example 1:
  * Input: root = [3,1,4,null,2], k = 1
  * Output: 1
  * Explanation: The binary tree looks like:
    ```
        3
       / \
      1   4
       \
        2
    ```
    The smallest element is 1.
* Example 2:
  * Input: root = [5,3,6,2,4,null,null,1], k = 3
  * Output: 3
  * Explanation: The binary tree looks like:
    ```
        5
       / \
      3   6
     / \
    2   4
   /
  1
    ```
    The 3rd smallest element is 3.
* Constraints:
  * The number of nodes in the tree is n.
  * 1 <= k <= n <= 10^4
  * 0 <= Node.val <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the kth smallest element in a binary search tree (BST)
* One approach would be to perform an inorder traversal of the BST, which visits the nodes in ascending order
* We can store the values in an array during the traversal and then return the kth element
* This approach is straightforward and leverages the property of BSTs that an inorder traversal visits the nodes in ascending order

---

## **Step 2: Flow Steps (Brute Force)**

1. Perform an inorder traversal of the BST and store the values in an array
2. Return the (k-1)th element of the array (since k is 1-indexed)

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def kthSmallest(root, k):
    # Perform inorder traversal and store the values
    def inorder(node):
        if not node:
            return []
        return inorder(node.left) + [node.val] + inorder(node.right)
    
    # Get the values in ascending order
    values = inorder(root)
    
    # Return the kth smallest element
    return values[k-1]
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n) where n is the number of nodes in the tree. We visit each node exactly once during the inorder traversal.
* Space Complexity: O(n) for storing the values array and the recursion stack.
* This approach is efficient in terms of time complexity, but it uses extra space to store all the values, which is not necessary if we only need the kth smallest element.

---

## **Step 5: Visualizing Redundant Computation**

* For the example tree [3,1,4,null,2] and k = 1:
  * Inorder traversal: [1, 2, 3, 4]
  * Return values[1-1] = values[0] = 1
* We're storing all the values in the tree, even though we only need the kth smallest element
* We can optimize this by stopping the traversal once we've found the kth smallest element

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Modified Inorder Traversal approach.
* The property that matches this problem is that an inorder traversal of a BST visits the nodes in ascending order.
* By keeping track of the number of nodes visited during the traversal, we can stop once we've found the kth smallest element.
* Alternatives like storing all values would use extra space, which is not necessary.
* Precondition: The tree is a valid BST.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Modified Inorder Traversal)**

* We can modify the inorder traversal to stop once we've found the kth smallest element
* We'll keep track of the number of nodes visited during the traversal
* When we've visited k nodes, we've found the kth smallest element
* This approach avoids storing all the values and can terminate early

### **Step 2: Flow Steps (Optimized - Modified Inorder Traversal)**

1. Initialize a counter to keep track of the number of nodes visited
2. Define a recursive function to perform the modified inorder traversal:
   1. If the current node is null, return null
   2. Recursively process the left subtree
   3. Increment the counter
   4. If the counter equals k, return the current node's value
   5. Recursively process the right subtree
3. Call the recursive function with the root
4. Return the result

### **Step 3: Implementation (Optimized - Modified Inorder Traversal)**

```python
def kthSmallest(root, k):
    # Initialize counter and result
    counter = 0
    result = None
    
    def inorder(node):
        nonlocal counter, result
        if not node or result is not None:
            return
        
        # Process left subtree
        inorder(node.left)
        
        # Process current node
        counter += 1
        if counter == k:
            result = node.val
            return
        
        # Process right subtree
        inorder(node.right)
    
    inorder(root)
    return result
```

### **Alternative Approach: Iterative Inorder Traversal**

Another approach is to use an iterative inorder traversal with a stack:

```python
def kthSmallest(root, k):
    stack = []
    curr = root
    count = 0
    
    while curr or stack:
        # Traverse to the leftmost node
        while curr:
            stack.append(curr)
            curr = curr.left
        
        # Process the current node
        curr = stack.pop()
        count += 1
        
        # If we've found the kth smallest element, return it
        if count == k:
            return curr.val
        
        # Move to the right subtree
        curr = curr.right
    
    return -1  # This should not happen given the constraints
```

### **Step 4: Complexity Analysis (Optimized - Modified Inorder Traversal)**

* Time Complexity: O(H + k) where H is the height of the tree and k is the input parameter. In the worst case, we might need to traverse down to the leftmost leaf (which takes O(H) time) and then visit k nodes.
* Space Complexity: O(H) for the recursion stack or the explicit stack in the iterative approach.
* This approach is more efficient than the brute force approach, especially for large trees and small values of k.

### **Step 5: Visualizing Computation (Optimized - Modified Inorder Traversal)**

* For the example tree [3,1,4,null,2] and k = 1:
  * inorder(3):
    * inorder(1):
      * inorder(null): return
      * counter = 1, k = 1, so result = 1
      * Return
    * Return
  * Return result = 1
* We're efficiently finding the kth smallest element without storing all the values.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Store All Values) | O(n) | O(n) | Stores all values in an array |
| Optimized (Modified Inorder Traversal) | O(H + k) | O(H) | Stops once the kth smallest element is found |
| Alternative (Iterative Inorder Traversal) | O(H + k) | O(H) | Uses a stack instead of recursion |

---

## **Final Takeaways**

* The key to efficiently finding the kth smallest element in a BST is to leverage the property that an inorder traversal visits the nodes in ascending order.
* The modified inorder traversal approach is more efficient than storing all values, especially for large trees and small values of k.
* Both recursive and iterative approaches can be used, with the choice depending on personal preference and the specific requirements of the problem.

---

## **Real-life Analogy**

* Imagine you're looking for the kth smallest book on a bookshelf arranged in alphabetical order. The brute force approach would be like scanning the entire bookshelf, writing down the titles of all books, and then picking the kth one from your list. The optimized approach would be like scanning the bookshelf from left to right, keeping a count of the books you've seen, and stopping once you've counted k books - this way, you don't need to write down all the titles.

---

## **Summary**

* We solved the "Kth Smallest Element in a BST" problem by first considering a brute force approach that performs an inorder traversal and stores all values, resulting in O(n) time and space complexity. We then optimized using a modified inorder traversal that stops once the kth smallest element is found, reducing the time complexity to O(H + k) and the space complexity to O(H). We also explored an iterative approach using a stack, which has the same time and space complexity as the recursive approach. Both optimized approaches efficiently find the kth smallest element in a BST, with the choice between them depending on personal preference and the specific requirements of the problem. 