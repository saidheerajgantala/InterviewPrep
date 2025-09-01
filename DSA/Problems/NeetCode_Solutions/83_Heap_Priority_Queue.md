# 📝 Heap / Priority Queue

## **Concept Overview**

* A heap is a specialized tree-based data structure that satisfies the heap property:
  * In a max heap, for any given node C, if P is a parent node of C, then the key (value) of P is greater than or equal to the key of C.
  * In a min heap, for any given node C, if P is a parent node of C, then the key (value) of P is less than or equal to the key of C.
* A priority queue is an abstract data type similar to a regular queue or stack, but where each element has a "priority" associated with it. Elements with higher priorities are served before elements with lower priorities.
* Heaps are commonly used to implement priority queues because they provide efficient operations for finding and removing the maximum or minimum element.

---

## **Key Operations and Complexity**

* **Insert**: Add an element to the heap
  * Time Complexity: O(log n)
  * Process: Add the element at the end of the heap, then "bubble up" to maintain the heap property
* **Extract Max/Min**: Remove and return the maximum (for max heap) or minimum (for min heap) element
  * Time Complexity: O(log n)
  * Process: Remove the root, replace it with the last element, then "bubble down" to maintain the heap property
* **Peek**: View the maximum (for max heap) or minimum (for min heap) element without removing it
  * Time Complexity: O(1)
  * Process: Simply return the value of the root node
* **Heapify**: Build a heap from an array
  * Time Complexity: O(n)
  * Process: Start from the last non-leaf node and bubble down each node

---

## **Implementation in Python**

Python provides a built-in module `heapq` that implements a min heap:

```python
import heapq

# Create a heap
heap = []

# Insert elements (heappush)
heapq.heappush(heap, 3)
heapq.heappush(heap, 1)
heapq.heappush(heap, 4)
heapq.heappush(heap, 2)

print(heap)  # Output: [1, 2, 4, 3]

# Extract the minimum element (heappop)
min_element = heapq.heappop(heap)
print(min_element)  # Output: 1
print(heap)  # Output: [2, 3, 4]

# Peek at the minimum element
peek_min = heap[0]
print(peek_min)  # Output: 2

# Heapify an existing list
arr = [3, 1, 4, 2]
heapq.heapify(arr)
print(arr)  # Output: [1, 2, 4, 3]
```

To implement a max heap in Python using `heapq`, you can negate the values:

```python
import heapq

# Create a max heap
max_heap = []

# Insert elements (negate values for max heap behavior)
heapq.heappush(max_heap, -3)
heapq.heappush(max_heap, -1)
heapq.heappush(max_heap, -4)
heapq.heappush(max_heap, -2)

print([-x for x in max_heap])  # Output: [4, 2, 3, 1]

# Extract the maximum element
max_element = -heapq.heappop(max_heap)
print(max_element)  # Output: 4
print([-x for x in max_heap])  # Output: [3, 2, 1]
```

---

## **Common Applications**

1. **Priority Queue**: Implementing a priority queue where elements with higher priority are served first.
2. **Dijkstra's Algorithm**: Finding the shortest path in a graph.
3. **Huffman Coding**: Building Huffman trees for data compression.
4. **K-th Largest/Smallest Element**: Finding the k-th largest or smallest element in an array.
5. **Merge K Sorted Lists**: Merging multiple sorted lists into a single sorted list.
6. **Median of a Stream**: Finding the median of a stream of numbers.
7. **Task Scheduler**: Scheduling tasks with cooldown periods.
8. **Meeting Rooms**: Finding the minimum number of meeting rooms required.

---

## **Implementation from Scratch**

Here's a simple implementation of a binary min heap from scratch:

```python
class MinHeap:
    def __init__(self):
        self.heap = []
    
    def parent(self, i):
        return (i - 1) // 2
    
    def left_child(self, i):
        return 2 * i + 1
    
    def right_child(self, i):
        return 2 * i + 2
    
    def get_min(self):
        if not self.heap:
            return None
        return self.heap[0]
    
    def insert(self, key):
        self.heap.append(key)
        self._bubble_up(len(self.heap) - 1)
    
    def extract_min(self):
        if not self.heap:
            return None
        
        min_val = self.heap[0]
        last_val = self.heap.pop()
        
        if self.heap:
            self.heap[0] = last_val
            self._bubble_down(0)
        
        return min_val
    
    def _bubble_up(self, i):
        parent = self.parent(i)
        if i > 0 and self.heap[i] < self.heap[parent]:
            self.heap[i], self.heap[parent] = self.heap[parent], self.heap[i]
            self._bubble_up(parent)
    
    def _bubble_down(self, i):
        smallest = i
        left = self.left_child(i)
        right = self.right_child(i)
        
        if left < len(self.heap) and self.heap[left] < self.heap[smallest]:
            smallest = left
        
        if right < len(self.heap) and self.heap[right] < self.heap[smallest]:
            smallest = right
        
        if smallest != i:
            self.heap[i], self.heap[smallest] = self.heap[smallest], self.heap[i]
            self._bubble_down(smallest)
```

---

## **Heap vs. Other Data Structures**

| Operation | Heap | Sorted Array | Unsorted Array | Binary Search Tree (Balanced) |
|---|---|---|---|---|
| Find Min/Max | O(1) | O(1) | O(n) | O(log n) |
| Insert | O(log n) | O(n) | O(1) | O(log n) |
| Delete Min/Max | O(log n) | O(1) | O(n) | O(log n) |
| Search for arbitrary element | O(n) | O(log n) | O(n) | O(log n) |
| Space | O(n) | O(n) | O(n) | O(n) |

---

## **Real-life Analogy**

* Imagine a hospital emergency room where patients are treated based on the severity of their condition rather than their arrival time. This is a real-life example of a priority queue. Patients with life-threatening conditions (higher priority) are treated before patients with minor injuries (lower priority), regardless of when they arrived.

---

## **Summary**

* A heap is a specialized tree-based data structure that satisfies the heap property, making it efficient for finding the maximum or minimum element.
* A priority queue is an abstract data type where elements have priorities, and elements with higher priorities are served first.
* Heaps are commonly used to implement priority queues because they provide O(1) access to the maximum or minimum element and O(log n) insertion and deletion operations.
* Common applications include Dijkstra's algorithm, Huffman coding, finding the k-th largest/smallest element, and scheduling tasks.
* In Python, the `heapq` module provides a built-in implementation of a min heap, which can be adapted to function as a max heap by negating values. 