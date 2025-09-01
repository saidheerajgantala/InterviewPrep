# 📝 LRU Cache

## **Problem Statement**

* Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.
* Implement the `LRUCache` class:
  * `LRUCache(int capacity)` Initialize the LRU cache with positive size capacity.
  * `int get(int key)` Return the value of the key if the key exists, otherwise return -1.
  * `void put(int key, int value)` Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
* The functions `get` and `put` must each run in O(1) average time complexity.
* Example:
  * Input:
    * ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
    * [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
  * Output:
    * [null, null, null, 1, null, -1, null, -1, 3, 4]
  * Explanation:
    * LRUCache lRUCache = new LRUCache(2);
    * lRUCache.put(1, 1); // cache is {1=1}
    * lRUCache.put(2, 2); // cache is {1=1, 2=2}
    * lRUCache.get(1);    // return 1
    * lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
    * lRUCache.get(2);    // returns -1 (not found)
    * lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
    * lRUCache.get(1);    // return -1 (not found)
    * lRUCache.get(3);    // return 3
    * lRUCache.get(4);    // return 4
* Constraints:
  * 1 <= capacity <= 3000
  * 0 <= key <= 10^4
  * 0 <= value <= 10^5
  * At most 2 * 10^5 calls will be made to get and put.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to design an LRU cache with O(1) time complexity for both get and put operations
* The cache should have a fixed capacity and evict the least recently used item when it's full
* A straightforward approach would be to use a hash map for O(1) access and an ordered list to track the usage order
* For each operation, we would update the usage order by moving the accessed item to the front of the list
* However, updating the usage order in a regular list would take O(n) time, which doesn't meet the requirement

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a hash map to store key-value pairs and a list to track the usage order
2. For get(key):
   1. If the key is not in the hash map, return -1
   2. Otherwise, move the key to the front of the list (mark it as most recently used)
   3. Return the value associated with the key
3. For put(key, value):
   1. If the key already exists, update its value and move it to the front of the list
   2. Otherwise, add the key-value pair to the hash map and the key to the front of the list
   3. If the cache exceeds its capacity, remove the least recently used key (the last item in the list)

---

## **Step 3: Brute Force Implementation (Code)**

```python
class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = {}  # Hash map for O(1) access
        self.usage_order = []  # List to track usage order
    
    def get(self, key):
        if key not in self.cache:
            return -1
        
        # Update usage order (move key to the front)
        self.usage_order.remove(key)
        self.usage_order.insert(0, key)
        
        return self.cache[key]
    
    def put(self, key, value):
        if key in self.cache:
            # Update value and usage order
            self.cache[key] = value
            self.usage_order.remove(key)
            self.usage_order.insert(0, key)
        else:
            # Add new key-value pair
            self.cache[key] = value
            self.usage_order.insert(0, key)
            
            # Evict least recently used key if necessary
            if len(self.cache) > self.capacity:
                lru_key = self.usage_order.pop()
                del self.cache[lru_key]
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity:
  * get: O(n) where n is the number of items in the cache. This is due to the remove operation in the list.
  * put: O(n) for the same reason.
* Space Complexity: O(capacity) for storing the cache and usage order.
* This approach doesn't meet the requirement of O(1) time complexity for both operations.

---

## **Step 5: Visualizing Redundant Computation**

* For the example:
  * Initialize capacity = 2, cache = {}, usage_order = []
  * put(1, 1): cache = {1: 1}, usage_order = [1]
  * put(2, 2): cache = {1: 1, 2: 2}, usage_order = [2, 1]
  * get(1): move 1 to the front, usage_order = [1, 2], return 1
  * put(3, 3): evict 2, cache = {1: 1, 3: 3}, usage_order = [3, 1]
  * get(2): not found, return -1
  * put(4, 4): evict 1, cache = {3: 3, 4: 4}, usage_order = [4, 3]
  * get(1): not found, return -1
  * get(3): move 3 to the front, usage_order = [3, 4], return 3
  * get(4): move 4 to the front, usage_order = [4, 3], return 4
* The operations on the usage_order list are inefficient, especially the remove operation which takes O(n) time

---

## **Why are we using this algorithm?**

* For optimization, we'll use a combination of a Hash Map and a Doubly Linked List.
* The property that matches this problem is the ability to efficiently access, add, and remove elements in O(1) time.
* By using a hash map for O(1) access and a doubly linked list for O(1) updates to the usage order, we can achieve the required time complexity.
* Alternatives like using just a hash map or a list would not meet the O(1) time complexity requirement.
* Precondition: We need to keep track of both the key-value pairs and the usage order.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can use a combination of a hash map and a doubly linked list
* The hash map provides O(1) access to the nodes in the linked list
* The doubly linked list allows us to efficiently update the usage order in O(1) time
* When a key is accessed, we move its node to the front of the linked list
* When the cache is full, we remove the node at the end of the linked list (the least recently used)

### **Step 2: Flow Steps (Optimized)**

1. Define a doubly linked list node class with key, value, prev, and next pointers
2. Initialize a hash map to store key-node pairs, and a doubly linked list with dummy head and tail nodes
3. For get(key):
   1. If the key is not in the hash map, return -1
   2. Otherwise, move the corresponding node to the front of the linked list
   3. Return the value of the node
4. For put(key, value):
   1. If the key already exists, update its value and move its node to the front of the linked list
   2. Otherwise, create a new node, add it to the hash map, and add it to the front of the linked list
   3. If the cache exceeds its capacity, remove the least recently used node (the one before the tail)

### **Step 3: Implementation (Optimized)**

```python
class Node:
    def __init__(self, key=0, value=0):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = {}  # Hash map for O(1) access
        
        # Initialize doubly linked list with dummy head and tail nodes
        self.head = Node()
        self.tail = Node()
        self.head.next = self.tail
        self.tail.prev = self.head
    
    def _add_node(self, node):
        # Always add the new node right after the head
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node
    
    def _remove_node(self, node):
        # Remove an existing node from the doubly linked list
        prev = node.prev
        new = node.next
        prev.next = new
        new.prev = prev
    
    def _move_to_head(self, node):
        # Move a node to the front (mark it as most recently used)
        self._remove_node(node)
        self._add_node(node)
    
    def _pop_tail(self):
        # Remove and return the least recently used node
        res = self.tail.prev
        self._remove_node(res)
        return res
    
    def get(self, key):
        if key not in self.cache:
            return -1
        
        # Update usage order (move node to the front)
        node = self.cache[key]
        self._move_to_head(node)
        
        return node.value
    
    def put(self, key, value):
        if key in self.cache:
            # Update value and usage order
            node = self.cache[key]
            node.value = value
            self._move_to_head(node)
        else:
            # Add new node
            node = Node(key, value)
            self.cache[key] = node
            self._add_node(node)
            
            # Evict least recently used node if necessary
            if len(self.cache) > self.capacity:
                lru = self._pop_tail()
                del self.cache[lru.key]
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity:
  * get: O(1) as we directly access the node using the hash map and update the linked list in constant time.
  * put: O(1) for the same reason.
* Space Complexity: O(capacity) for storing the cache and the doubly linked list.
* This approach meets the requirement of O(1) time complexity for both operations.

### **Step 5: Visualizing Computation (Optimized)**

* For the example:
  * Initialize capacity = 2, cache = {}, head <-> tail
  * put(1, 1): cache = {1: node1}, head <-> node1 <-> tail
  * put(2, 2): cache = {1: node1, 2: node2}, head <-> node2 <-> node1 <-> tail
  * get(1): move node1 to the front, head <-> node1 <-> node2 <-> tail, return 1
  * put(3, 3): evict node2, cache = {1: node1, 3: node3}, head <-> node3 <-> node1 <-> tail
  * get(2): not found, return -1
  * put(4, 4): evict node1, cache = {3: node3, 4: node4}, head <-> node4 <-> node3 <-> tail
  * get(1): not found, return -1
  * get(3): move node3 to the front, head <-> node3 <-> node4 <-> tail, return 3
  * get(4): move node4 to the front, head <-> node4 <-> node3 <-> tail, return 4
* All operations are now O(1) time complexity

### **Complexity Summary**

| Approach | Time (get, put) | Space | Notes |
|---|---|---|---|
| Brute Force (List) | O(n), O(n) | O(capacity) | Uses a list for usage order, which is inefficient |
| Optimized (Algorithm: Hash Map + Doubly Linked List) | O(1), O(1) | O(capacity) | Uses a hash map for O(1) access and a doubly linked list for O(1) updates |

---

## **Final Takeaways**

* The key to implementing an efficient LRU cache is to use a combination of a hash map and a doubly linked list.
* The hash map provides O(1) access to the nodes, while the doubly linked list allows for O(1) updates to the usage order.
* This problem demonstrates how combining different data structures can lead to optimal solutions that meet specific requirements.

---

## **Real-life Analogy**

* Imagine a bookshelf where you keep your most frequently used books. When you use a book, you place it at the leftmost position on the shelf. When the shelf is full and you need to add a new book, you remove the rightmost book (the least recently used). The hash map is like an index that tells you exactly where each book is on the shelf, allowing you to quickly find and move any book. The doubly linked list is like the shelf itself, allowing you to easily add books to the left, remove books from the right, and move books from one position to another.

---

## **Summary**

* We designed an "LRU Cache" that supports get and put operations in O(1) time complexity. We first considered a brute force approach using a hash map and a list, resulting in O(n) time complexity for both operations. We then optimized using a combination of a hash map and a doubly linked list, reducing the time complexity to O(1) for both operations while maintaining O(capacity) space complexity. The key insight is to use the hash map for O(1) access to the nodes and the doubly linked list for O(1) updates to the usage order. 