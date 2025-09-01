# 📝 Time-Based Key-Value Store

## **Problem Statement**

* Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.
* Implement the `TimeMap` class:
  * `TimeMap()` Initializes the object of the data structure.
  * `void set(String key, String value, int timestamp)` Stores the key `key` with the value `value` at the given time `timestamp`.
  * `String get(String key, int timestamp)` Returns a value such that `set` was called previously, with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the value associated with the largest `timestamp_prev`. If there are no values, it returns `""`.
* Example:
  * Input:
    * ["TimeMap", "set", "get", "get", "set", "get", "get"]
    * [[], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5]]
  * Output:
    * [null, null, "bar", "bar", null, "bar2", "bar2"]
  * Explanation:
    * TimeMap timeMap = new TimeMap();
    * timeMap.set("foo", "bar", 1);  // store the key "foo" and value "bar" along with timestamp = 1.
    * timeMap.get("foo", 1);         // return "bar"
    * timeMap.get("foo", 3);         // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".
    * timeMap.set("foo", "bar2", 4); // store the key "foo" and value "bar2" along with timestamp = 4.
    * timeMap.get("foo", 4);         // return "bar2"
    * timeMap.get("foo", 5);         // return "bar2"
* Constraints:
  * 1 <= key.length, value.length <= 100
  * key and value consist of lowercase English letters and digits.
  * 1 <= timestamp <= 10^7
  * All the timestamps timestamp of set are strictly increasing.
  * At most 2 * 10^5 calls will be made to set and get.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to design a data structure that can store multiple values for the same key at different timestamps and retrieve the value at a specific timestamp
* For the `set` operation, we need to store the key, value, and timestamp
* For the `get` operation, we need to find the value with the largest timestamp that is less than or equal to the given timestamp
* A simple approach would be to use a hash map where the key is the key string, and the value is a list of (timestamp, value) pairs
* For `set`, we append the (timestamp, value) pair to the list for the given key
* For `get`, we iterate through the list for the given key and find the value with the largest timestamp that is less than or equal to the given timestamp

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a hash map where the key is the key string, and the value is a list of (timestamp, value) pairs
2. For `set(key, value, timestamp)`:
   1. If the key is not in the hash map, create a new list for it
   2. Append the (timestamp, value) pair to the list for the given key
3. For `get(key, timestamp)`:
   1. If the key is not in the hash map, return ""
   2. Iterate through the list for the given key in reverse order (from newest to oldest)
   3. Return the first value with a timestamp less than or equal to the given timestamp
   4. If no such value is found, return ""

---

## **Step 3: Brute Force Implementation (Code)**

```python
class TimeMap:
    def __init__(self):
        self.store = {}  # key -> list of (timestamp, value) pairs
    
    def set(self, key, value, timestamp):
        if key not in self.store:
            self.store[key] = []
        self.store[key].append((timestamp, value))
    
    def get(self, key, timestamp):
        if key not in self.store:
            return ""
        
        # Iterate through the list in reverse order
        for t, value in reversed(self.store[key]):
            if t <= timestamp:
                return value
        
        return ""
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity:
  * `set`: O(1) - We just append to a list, which is a constant time operation.
  * `get`: O(n) where n is the number of timestamps for the given key. In the worst case, we might need to iterate through all timestamps.
* Space Complexity: O(n) where n is the total number of key-value-timestamp triplets.
* This approach is inefficient for the `get` operation, especially when there are many timestamps for a given key.

---

## **Step 5: Visualizing Redundant Computation**

* For the example:
  * `set("foo", "bar", 1)`: store["foo"] = [(1, "bar")]
  * `get("foo", 1)`: Iterate through [(1, "bar")] and return "bar"
  * `get("foo", 3)`: Iterate through [(1, "bar")] and return "bar"
  * `set("foo", "bar2", 4)`: store["foo"] = [(1, "bar"), (4, "bar2")]
  * `get("foo", 4)`: Iterate through [(1, "bar"), (4, "bar2")] in reverse and return "bar2"
  * `get("foo", 5)`: Iterate through [(1, "bar"), (4, "bar2")] in reverse and return "bar2"
* We're iterating through the list of timestamps for each `get` operation, which is inefficient
* Also, we're not taking advantage of the fact that the timestamps are strictly increasing

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Binary Search algorithm.
* The property that matches this problem is the ability to efficiently find the largest timestamp that is less than or equal to a given timestamp in a sorted list.
* By keeping the timestamps sorted, we can use binary search to find the appropriate value in O(log n) time.
* Alternatives like linear search would be O(n), which is less efficient.
* Precondition: The timestamps are strictly increasing, which means they are sorted.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We can still use a hash map where the key is the key string, and the value is a list of (timestamp, value) pairs
* For `set`, we append the (timestamp, value) pair to the list for the given key
* Since the timestamps are strictly increasing, the list will be sorted by timestamp
* For `get`, we can use binary search to find the value with the largest timestamp that is less than or equal to the given timestamp
* This will reduce the time complexity of the `get` operation from O(n) to O(log n)

### **Step 2: Flow Steps (Optimized)**

1. Initialize a hash map where the key is the key string, and the value is a list of (timestamp, value) pairs
2. For `set(key, value, timestamp)`:
   1. If the key is not in the hash map, create a new list for it
   2. Append the (timestamp, value) pair to the list for the given key
3. For `get(key, timestamp)`:
   1. If the key is not in the hash map, return ""
   2. Use binary search to find the value with the largest timestamp that is less than or equal to the given timestamp
   3. If no such value is found, return ""

### **Step 3: Implementation (Optimized)**

```python
class TimeMap:
    def __init__(self):
        self.store = {}  # key -> list of (timestamp, value) pairs
    
    def set(self, key, value, timestamp):
        if key not in self.store:
            self.store[key] = []
        self.store[key].append((timestamp, value))
    
    def get(self, key, timestamp):
        if key not in self.store:
            return ""
        
        values = self.store[key]
        left, right = 0, len(values) - 1
        
        # Binary search to find the largest timestamp <= given timestamp
        while left <= right:
            mid = left + (right - left) // 2
            if values[mid][0] <= timestamp:
                left = mid + 1
            else:
                right = mid - 1
        
        # right is the index of the largest timestamp <= given timestamp
        if right >= 0:
            return values[right][1]
        else:
            return ""
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity:
  * `set`: O(1) - We just append to a list, which is a constant time operation.
  * `get`: O(log n) where n is the number of timestamps for the given key. We use binary search to find the appropriate value.
* Space Complexity: O(n) where n is the total number of key-value-timestamp triplets.
* This approach is much more efficient for the `get` operation, especially when there are many timestamps for a given key.

### **Step 5: Visualizing Computation (Optimized)**

* For the example:
  * `set("foo", "bar", 1)`: store["foo"] = [(1, "bar")]
  * `get("foo", 1)`: Binary search in [(1, "bar")] and return "bar"
  * `get("foo", 3)`: Binary search in [(1, "bar")] and return "bar"
  * `set("foo", "bar2", 4)`: store["foo"] = [(1, "bar"), (4, "bar2")]
  * `get("foo", 4)`: Binary search in [(1, "bar"), (4, "bar2")] and return "bar2"
  * `get("foo", 5)`: Binary search in [(1, "bar"), (4, "bar2")] and return "bar2"
* We're using binary search to efficiently find the appropriate value, which is much faster than iterating through the list.

### **Complexity Summary**

| Approach | Time (set, get) | Space | Notes |
|---|---|---|---|
| Brute Force | O(1), O(n) | O(n) | Iterates through the list of timestamps for each `get` operation |
| Optimized (Algorithm: Binary Search) | O(1), O(log n) | O(n) | Uses binary search to find the appropriate value |

---

## **Final Takeaways**

* Binary search can be used to efficiently find the largest element in a sorted list that is less than or equal to a given value.
* The key insight is to take advantage of the fact that the timestamps are strictly increasing, which means they are sorted.
* This problem demonstrates how understanding the properties of the input data can lead to efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're a historian with a collection of newspaper articles sorted by date. Someone asks you for the most recent article about a specific topic before a given date. The brute force approach would be like going through all the articles from newest to oldest until you find one that's before or on the given date. The optimized approach is like using the fact that the articles are sorted by date to efficiently find the most recent one before the given date - by using a binary search to quickly narrow down to the right article without checking each one individually.

---

## **Summary**

* We designed a "Time-Based Key-Value Store" that can store multiple values for the same key at different timestamps and retrieve the value at a specific timestamp. We first considered a brute force approach that iterates through the list of timestamps for each `get` operation, resulting in O(n) time complexity. We then optimized using a binary search approach that takes advantage of the fact that the timestamps are strictly increasing, reducing the time complexity of the `get` operation to O(log n) while maintaining O(1) time complexity for the `set` operation. The key insight is to leverage the sorted property of the timestamps to efficiently find the appropriate value. 