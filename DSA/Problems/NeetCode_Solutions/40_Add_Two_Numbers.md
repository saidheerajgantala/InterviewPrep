# 📝 Add Two Numbers

## **Problem Statement**

* You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.
* You may assume the two numbers do not contain any leading zero, except the number 0 itself.
* Example:
  * Input: l1 = [2,4,3], l2 = [5,6,4]
  * Output: [7,0,8]
  * Explanation: 342 + 465 = 807.
* Constraints:
  * The number of nodes in each linked list is in the range [1, 100].
  * 0 <= Node.val <= 9
  * It is guaranteed that the list represents a number that does not have leading zeros.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to add two numbers represented as linked lists in reverse order
* Since the lists are already in reverse order, we can simply traverse both lists simultaneously and add the corresponding digits
* We need to keep track of the carry that may result from adding two digits
* We create a new linked list to store the result

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a dummy head for the result linked list
2. Initialize a current pointer to the dummy head
3. Initialize a carry variable to 0
4. While there are digits left in either list or there's a carry:
   1. Get the values of the current nodes (or 0 if the list has ended)
   2. Calculate the sum: sum = val1 + val2 + carry
   3. Update the carry: carry = sum // 10
   4. Create a new node with value sum % 10 and append it to the result list
   5. Move to the next nodes in both lists
5. Return the next of the dummy head (the head of the result list)

---

## **Step 3: Brute Force Implementation (Code)**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def addTwoNumbers(l1, l2):
    dummy = ListNode(0)
    current = dummy
    carry = 0
    
    while l1 or l2 or carry:
        # Get the values of the current nodes (or 0 if the list has ended)
        val1 = l1.val if l1 else 0
        val2 = l2.val if l2 else 0
        
        # Calculate the sum and carry
        sum_val = val1 + val2 + carry
        carry = sum_val // 10
        
        # Create a new node with value sum % 10 and append it to the result list
        current.next = ListNode(sum_val % 10)
        current = current.next
        
        # Move to the next nodes in both lists
        l1 = l1.next if l1 else None
        l2 = l2.next if l2 else None
    
    return dummy.next
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(max(n, m)) where n and m are the lengths of the two lists. We traverse both lists once.
* Space Complexity: O(max(n, m)) for the result linked list. The space for the result list is not counted as extra space because it's part of the output.
* This approach is actually optimal in terms of both time and space complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For l1 = [2,4,3] and l2 = [5,6,4]:
  * Initialize dummy = 0, current = dummy, carry = 0
  * Iteration 1: val1 = 2, val2 = 5, sum_val = 7, carry = 0, result = [7]
  * Iteration 2: val1 = 4, val2 = 6, sum_val = 10, carry = 1, result = [7,0]
  * Iteration 3: val1 = 3, val2 = 4, sum_val = 8 (including carry), carry = 0, result = [7,0,8]
  * No more nodes and no carry, so return result = [7,0,8]
* There's no redundant computation in this approach. We process each digit exactly once.

---

## **Why are we using this algorithm?**

* For this problem, the brute force approach is actually optimal.
* The property that matches this problem is the ability to add two numbers represented as linked lists in reverse order.
* By traversing both lists simultaneously and keeping track of the carry, we can efficiently add the two numbers.
* Alternatives like converting the linked lists to integers, adding them, and then converting back to a linked list would be less efficient and could lead to integer overflow.
* Precondition: The linked lists represent non-negative integers in reverse order.

---

## **Algo optimization**

Since the brute force approach is already optimal, there's no need for further optimization. However, we can make the code more concise by using a slightly different implementation.

### **Step 1: Thinking Process (Optimized)**

* The approach is the same as before: traverse both lists simultaneously and add the corresponding digits
* We can simplify the code by using a single loop and handling the carry more elegantly

### **Step 2: Flow Steps (Optimized)**

1. Initialize a dummy head for the result linked list
2. Initialize a current pointer to the dummy head
3. Initialize a carry variable to 0
4. While there are digits left in either list or there's a carry:
   1. Get the values of the current nodes (or 0 if the list has ended)
   2. Calculate the sum: sum = val1 + val2 + carry
   3. Update the carry: carry = sum // 10
   4. Create a new node with value sum % 10 and append it to the result list
   5. Move to the next nodes in both lists
5. Return the next of the dummy head (the head of the result list)

### **Step 3: Implementation (Optimized)**

```python
def addTwoNumbers(l1, l2):
    dummy = ListNode(0)
    current = dummy
    carry = 0
    
    while l1 or l2 or carry:
        # Get the values of the current nodes (or 0 if the list has ended)
        val1 = l1.val if l1 else 0
        val2 = l2.val if l2 else 0
        
        # Calculate the sum and carry
        sum_val = val1 + val2 + carry
        carry = sum_val // 10
        
        # Create a new node with value sum % 10 and append it to the result list
        current.next = ListNode(sum_val % 10)
        current = current.next
        
        # Move to the next nodes in both lists
        l1 = l1.next if l1 else None
        l2 = l2.next if l2 else None
    
    return dummy.next
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(max(n, m)) where n and m are the lengths of the two lists. We traverse both lists once.
* Space Complexity: O(max(n, m)) for the result linked list. The space for the result list is not counted as extra space because it's part of the output.
* This approach is optimal in terms of both time and space complexity.

### **Step 5: Visualizing Computation (Optimized)**

* For l1 = [2,4,3] and l2 = [5,6,4]:
  * Initialize dummy = 0, current = dummy, carry = 0
  * Iteration 1: val1 = 2, val2 = 5, sum_val = 7, carry = 0, result = [7]
  * Iteration 2: val1 = 4, val2 = 6, sum_val = 10, carry = 1, result = [7,0]
  * Iteration 3: val1 = 3, val2 = 4, sum_val = 8 (including carry), carry = 0, result = [7,0,8]
  * No more nodes and no carry, so return result = [7,0,8]
* The computation is the same as before, but the code is more concise.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force / Optimized | O(max(n, m)) | O(max(n, m)) | Traverses both lists once and creates a new list for the result |

---

## **Final Takeaways**

* The key to adding two numbers represented as linked lists is to traverse both lists simultaneously and keep track of the carry.
* By using a dummy head, we can simplify the code and handle edge cases elegantly.
* This problem demonstrates how linked lists can be used to represent and manipulate large numbers efficiently.

---

## **Real-life Analogy**

* Imagine you're adding two large numbers on paper, starting from the rightmost digit and moving left. You add the corresponding digits, write down the result, and carry over any excess to the next position. The linked list approach is similar - we start from the least significant digit (the head of the list), add the corresponding digits, and carry over any excess to the next position. The fact that the linked lists are in reverse order actually makes the addition process more natural, as it mimics how we add numbers on paper from right to left.

---

## **Summary**

* We solved the "Add Two Numbers" problem by traversing both linked lists simultaneously, adding the corresponding digits, and keeping track of the carry. This approach has O(max(n, m)) time complexity and O(max(n, m)) space complexity, which is optimal for this problem. The key insight is to leverage the reverse order of the linked lists, which allows us to add the numbers from the least significant digit to the most significant digit, just like we would on paper. 