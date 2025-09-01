# 📝 K Closest Points to Origin

## **Problem Statement**

* Given an array of `points` where `points[i] = [xi, yi]` represents a point on the X-Y plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.
* The distance between two points on the X-Y plane is the Euclidean distance (i.e., `√(x1 - x2)² + (y1 - y2)²`).
* You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).
* Example 1:
  * Input: points = [[1,3],[-2,2]], k = 1
  * Output: [[-2,2]]
  * Explanation: The distance between (1, 3) and the origin is sqrt(10). The distance between (-2, 2) and the origin is sqrt(8). Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin. We only want the closest k = 1 points from the origin, so the answer is just [[-2, 2]].
* Example 2:
  * Input: points = [[3,3],[5,-1],[-2,4]], k = 2
  * Output: [[3,3],[-2,4]]
  * Explanation: The answer [[-2,4],[3,3]] would also be accepted.
* Constraints:
  * 1 <= k <= points.length <= 10^4
  * -10^4 <= xi, yi <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the k closest points to the origin (0, 0)
* The distance from a point (x, y) to the origin is sqrt(x² + y²)
* One approach would be to calculate the distance for each point, sort the points by distance, and return the first k points
* This approach is straightforward and works for small arrays

---

## **Step 2: Flow Steps (Brute Force)**

1. Calculate the distance from each point to the origin
2. Sort the points by distance
3. Return the first k points

---

## **Step 3: Brute Force Implementation (Code)**

```python
def kClosest(points, k):
    # Calculate the distance for each point and store it along with the point
    distances = []
    for point in points:
        x, y = point
        distance = x**2 + y**2  # We can use squared distance to avoid the sqrt operation
        distances.append((distance, point))
    
    # Sort by distance
    distances.sort()
    
    # Return the first k points
    return [point for _, point in distances[:k]]
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n log n) where n is the number of points. This is due to the sorting operation.
* Space Complexity: O(n) for storing the distances array.
* This approach is simple but might not be the most efficient for large arrays or when k is small compared to n.

---

## **Step 5: Visualizing Redundant Computation**

* For the example points = [[1,3],[-2,2]], k = 1:
  * Calculate distances: [(1²+3²=10, [1,3]), ((-2)²+2²=8, [-2,2])]
  * Sort by distance: [(8, [-2,2]), (10, [1,3])]
  * Return the first k=1 points: [[-2,2]]
* For the example points = [[3,3],[5,-1],[-2,4]], k = 2:
  * Calculate distances: [(3²+3²=18, [3,3]), (5²+(-1)²=26, [5,-1]), ((-2)²+4²=20, [-2,4])]
  * Sort by distance: [(18, [3,3]), (20, [-2,4]), (26, [5,-1])]
  * Return the first k=2 points: [[3,3], [-2,4]]
* We're sorting the entire array, which is unnecessary if we only need the k closest points

---

## **Why are we using this algorithm?**

* For optimization, we'll consider two approaches: using a max-heap and using QuickSelect.
* The property that matches this problem is that we only need to find the k closest points, not sort all points by distance.
* By using a max-heap of size k, we can efficiently keep track of the k closest points.
* Alternatively, QuickSelect can find the k closest points in expected O(n) time.
* Alternatives like the brute force approach would be less efficient for large arrays or when k is small compared to n.
* Precondition: We need to be able to calculate the distance from each point to the origin.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Max-Heap)**

* We can use a max-heap to keep track of the k closest points
* We'll iterate through the points and add each point to the heap
* If the heap size exceeds k, we'll remove the farthest point (the root of the max-heap)
* After processing all points, the heap will contain the k closest points

### **Step 2: Flow Steps (Optimized - Max-Heap)**

1. Initialize a max-heap
2. Iterate through the points:
   1. Calculate the distance from the point to the origin
   2. Add the point to the heap along with its negative distance (to simulate a max-heap)
   3. If the heap size exceeds k, remove the farthest point (the root of the max-heap)
3. Return the k points in the heap

### **Step 3: Implementation (Optimized - Max-Heap)**

```python
import heapq

def kClosest(points, k):
    heap = []
    
    for point in points:
        x, y = point
        distance = x**2 + y**2
        
        # Add the point to the heap with negative distance to simulate a max-heap
        heapq.heappush(heap, (-distance, point))
        
        # If the heap size exceeds k, remove the farthest point
        if len(heap) > k:
            heapq.heappop(heap)
    
    # Return the k points in the heap
    return [point for _, point in heap]
```

### **Alternative Approach: QuickSelect**

Another efficient approach is to use the QuickSelect algorithm, which is based on the QuickSort algorithm but only explores one partition:

```python
def kClosest(points, k):
    def distance(point):
        return point[0]**2 + point[1]**2
    
    def quickselect(points, k):
        if len(points) == k:
            return points
        
        pivot = distance(points[len(points) // 2])
        left = [point for point in points if distance(point) < pivot]
        equal = [point for point in points if distance(point) == pivot]
        right = [point for point in points if distance(point) > pivot]
        
        if k <= len(left):
            return quickselect(left, k)
        elif k <= len(left) + len(equal):
            return left + equal[:k - len(left)]
        else:
            return left + equal + quickselect(right, k - len(left) - len(equal))
    
    return quickselect(points, k)
```

### **Step 4: Complexity Analysis (Optimized - Max-Heap)**

* Time Complexity: O(n log k) where n is the number of points and k is the given parameter. We perform n heap operations, each taking O(log k) time.
* Space Complexity: O(k) for storing the heap.
* This approach is more efficient than the brute force approach when k is small compared to n.

### **Complexity Analysis (QuickSelect)**

* Time Complexity: 
  * Average case: O(n) where n is the number of points.
  * Worst case: O(n²) if the pivot is always the smallest or largest element.
* Space Complexity: O(n) for the recursive calls and the partitioning.
* This approach is very efficient on average but can be slow in the worst case.

### **Step 5: Visualizing Computation (Optimized - Max-Heap)**

* For the example points = [[1,3],[-2,2]], k = 1:
  * Initialize an empty max-heap
  * Process [1,3]: distance = 1²+3²=10, heap = [(-10, [1,3])]
  * Process [-2,2]: distance = (-2)²+2²=8, heap = [(-10, [1,3]), (-8, [-2,2])], remove the farthest point, heap = [(-8, [-2,2])]
  * Return the points in the heap: [[-2,2]]
* For the example points = [[3,3],[5,-1],[-2,4]], k = 2:
  * Initialize an empty max-heap
  * Process [3,3]: distance = 3²+3²=18, heap = [(-18, [3,3])]
  * Process [5,-1]: distance = 5²+(-1)²=26, heap = [(-26, [5,-1]), (-18, [3,3])], remove the farthest point, heap = [(-18, [3,3])]
  * Process [-2,4]: distance = (-2)²+4²=20, heap = [(-20, [-2,4]), (-18, [3,3])], remove the farthest point, heap = [(-18, [3,3]), (-20, [-2,4])]
  * Return the points in the heap: [[3,3], [-2,4]]
* The max-heap approach efficiently keeps track of the k closest points

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Sort) | O(n log n) | O(n) | Simple but inefficient for large arrays |
| Optimized (Max-Heap) | O(n log k) | O(k) | Efficient when k is small compared to n |
| QuickSelect | O(n) average, O(n²) worst | O(n) | Very efficient on average |

---

## **Final Takeaways**

* The key to efficiently finding the k closest points to the origin is to avoid sorting the entire array.
* Using a max-heap of size k allows us to efficiently keep track of the k closest points.
* QuickSelect is another efficient algorithm that can find the k closest points in expected O(n) time.
* The choice between these approaches depends on the specific requirements and constraints of the problem.

---

## **Real-life Analogy**

* Imagine you're a real estate agent trying to find the k closest houses to downtown. The brute force approach would be like measuring the distance from each house to downtown, sorting all houses by distance, and then picking the first k. The max-heap approach would be like maintaining a list of the k closest houses you've seen so far, and updating it as you visit each house. The QuickSelect approach would be like dividing the houses into groups based on a pivot distance, and then focusing only on the group that contains the k closest houses.

---

## **Summary**

* We solved the "K Closest Points to Origin" problem by first considering a brute force approach that sorts all points by distance, resulting in O(n log n) time complexity. We then optimized using a max-heap to keep track of the k closest points, reducing the time complexity to O(n log k). We also explored the QuickSelect algorithm, which has an expected time complexity of O(n). The key insight is to avoid sorting the entire array and focus only on finding the k closest points. 