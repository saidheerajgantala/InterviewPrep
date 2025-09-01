# 📝 Merge K Sorted Lists

## **Problem Statement**

* You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.
* Merge all the linked-lists into one sorted linked-list and return it.
* Example:
  * Input: lists = [[1,4,5],[1,3,4],[2,6]]
  * Output: [1,1,2,3,4,4,5,6]
  * Explanation: The linked-lists are:
    * [1->4->5]
    * [1->3->4]
    * [2->6]
    * merging them into one sorted list: 1->1->2->3->4->4->5->6
* Constraints:
  * k == lists.length
  * 0 <= k <= 10^4
  * 0 <= lists[i].length <= 500
  * -10^4 <= lists[i][j] <= 10^4
  * lists[i] is sorted in ascending order.
  * The sum of lists[i].length will not exceed 10^4.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to merge k sorted linked lists into one sorted linked list
* One straightforward approach would be to merge the lists one by one
* We can use the "merge two sorted lists" algorithm as a building block
* We start with the first list, then merge it with the second list, then merge the result with the third list, and so on
* This approach is simple but not the most efficient

---

## **Step 2: Flow Steps (Brute Force)**

1. If the input is empty, return null
2. Initialize result = lists[0]
3. For each list from index 1 to k-1:
   1. Merge result with lists[i] using the "merge two sorted lists" algorithm
   2. Update result to the merged list
4. Return result

---

## **Step 3: Brute Force Implementation (Code)**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def mergeKLists(lists):
    if not lists:
        return None
    
    # Helper function to merge two sorted lists
    def mergeTwoLists(l1, l2):
        dummy = ListNode(0)
        current = dummy
        
        while l1 and l2:
            if l1.val <= l2.val:
                current.next = l1
                l1 = l1.next
            else:
                current.next = l2
                l2 = l2.next
            current = current.next
        
        current.next = l1 if l1 else l2
        return dummy.next
    
    # Merge lists one by one
    result = lists[0]
    for i in range(1, len(lists)):
        result = mergeTwoLists(result, lists[i])
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(N * k) where N is the total number of nodes across all lists and k is the number of lists. We merge two lists at a time, and each merge operation takes O(n1 + n2) time where n1 and n2 are the lengths of the two lists. In the worst case, we're merging a list of length N/k with a list of length N - N/k, which is O(N).
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient for large values of k.

---

## **Step 5: Visualizing Redundant Computation**

* For lists = [[1,4,5],[1,3,4],[2,6]]:
  * Merge [1,4,5] and [1,3,4]: [1,1,3,4,4,5]
  * Merge [1,1,3,4,4,5] and [2,6]: [1,1,2,3,4,4,5,6]
* We're repeatedly merging lists, and the size of the result grows with each merge
* This leads to redundant comparisons, especially for the nodes that are processed multiple times

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Divide and Conquer approach or a Priority Queue.
* The property that matches this problem is the ability to efficiently merge multiple sorted lists.
* By using divide and conquer, we can merge pairs of lists in a balanced way, reducing the time complexity.
* Alternatives like merging one by one would be less efficient for large values of k.
* Precondition: All input lists are sorted in ascending order.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Divide and Conquer)**

* Instead of merging lists one by one, we can use a divide and conquer approach
* We divide the k lists into pairs, merge each pair, then merge the results recursively
* This approach is more balanced and efficient, especially for large values of k

### **Step 2: Flow Steps (Optimized - Divide and Conquer)**

1. If the input is empty, return null
2. Define a recursive function to merge lists in a given range:
   1. If start == end, return lists[start]
   2. Calculate mid = start + (end - start) // 2
   3. Recursively merge lists from start to mid
   4. Recursively merge lists from mid + 1 to end
   5. Merge the two results using the "merge two sorted lists" algorithm
3. Call the recursive function with the range [0, k-1]

### **Step 3: Implementation (Optimized - Divide and Conquer)**

```python
def mergeKLists(lists):
    if not lists:
        return None
    
    # Helper function to merge two sorted lists
    def mergeTwoLists(l1, l2):
        dummy = ListNode(0)
        current = dummy
        
        while l1 and l2:
            if l1.val <= l2.val:
                current.next = l1
                l1 = l1.next
            else:
                current.next = l2
                l2 = l2.next
            current = current.next
        
        current.next = l1 if l1 else l2
        return dummy.next
    
    # Helper function to merge lists in a given range
    def merge(start, end):
        if start == end:
            return lists[start]
        if start > end:
            return None
        
        mid = start + (end - start) // 2
        left = merge(start, mid)
        right = merge(mid + 1, end)
        return mergeTwoLists(left, right)
    
    return merge(0, len(lists) - 1)
```

### **Alternative Approach: Priority Queue**

Another approach is to use a priority queue (min heap):

```python
import heapq

def mergeKLists(lists):
    # Initialize a min heap with the first node of each list
    heap = []
    for i, lst in enumerate(lists):
        if lst:
            heapq.heappush(heap, (lst.val, i, lst))
    
    dummy = ListNode(0)
    current = dummy
    
    # Process nodes from the heap
    while heap:
        val, i, node = heapq.heappop(heap)
        current.next = node
        current = current.next
        
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))
    
    return dummy.next
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity:
  * Divide and Conquer: O(N * log k) where N is the total number of nodes across all lists and k is the number of lists. We perform log k levels of merging, and each level involves merging all N nodes.
  * Priority Queue: O(N * log k) as we perform N heap operations, each taking O(log k) time.
* Space Complexity:
  * Divide and Conquer: O(log k) for the recursion stack.
  * Priority Queue: O(k) for storing k nodes in the heap.
* Both approaches are more efficient than the brute force approach for large values of k.

### **Step 5: Visualizing Computation (Optimized - Divide and Conquer)**

* For lists = [[1,4,5],[1,3,4],[2,6]]:
  * Divide: [1,4,5], [1,3,4], [2,6]
  * Conquer:
    * Merge [1,4,5] and [1,3,4]: [1,1,3,4,4,5]
    * Merge [1,1,3,4,4,5] and [2,6]: [1,1,2,3,4,4,5,6]
* We're merging lists in a balanced way, which reduces the number of comparisons

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Merge One by One) | O(N * k) | O(1) | Inefficient for large values of k |
| Optimized (Algorithm: Divide and Conquer) | O(N * log k) | O(log k) | More balanced and efficient |
| Alternative (Algorithm: Priority Queue) | O(N * log k) | O(k) | Uses a min heap to efficiently merge lists |

---

## **Final Takeaways**

* The key to efficiently merging k sorted lists is to use a divide and conquer approach or a priority queue.
* By merging lists in a balanced way or using a priority queue to always select the smallest node, we can reduce the time complexity from O(N * k) to O(N * log k).
* This problem demonstrates how algorithmic techniques like divide and conquer and data structures like priority queues can be used to optimize solutions.

---

## **Real-life Analogy**

* Imagine you have k sorted stacks of books, and you want to merge them into a single sorted stack. The brute force approach would be like taking the first stack, merging it with the second stack, then merging the result with the third stack, and so on. This becomes inefficient as the merged stack grows larger with each merge. The divide and conquer approach is like merging pairs of stacks, then merging pairs of the resulting stacks, and so on - this way, you're always merging stacks of similar sizes. The priority queue approach is like always selecting the smallest book from the top of each stack, which ensures you're building the merged stack in the correct order.

---

## **Summary**

* We solved the "Merge K Sorted Lists" problem by first considering a brute force approach that merges lists one by one, resulting in O(N * k) time complexity. We then optimized using a divide and conquer approach that merges lists in a balanced way, reducing the time complexity to O(N * log k) while using O(log k) space for the recursion stack. We also explored an alternative approach using a priority queue, which also has O(N * log k) time complexity but uses O(k) space for the heap. The key insight is to merge lists in a balanced way or to always select the smallest node using a priority queue, which significantly improves the efficiency of the algorithm. 