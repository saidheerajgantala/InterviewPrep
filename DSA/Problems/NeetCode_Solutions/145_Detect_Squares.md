# 📝 Detect Squares

## **Problem Statement**

* You are given a stream of points on the 2D plane. Design an algorithm that:
  * Adds new points from the stream into a data structure.
  * Given a query point, counts the number of ways to form axis-aligned squares with this point as one of the vertices.
* Implement the `DetectSquares` class:
  * `DetectSquares()` Initializes the object with an empty data structure.
  * `void add(int[] point)` Adds a new point `point = [x, y]` to the data structure.
  * `int count(int[] point)` Counts the number of ways to form axis-aligned squares with the given point `point = [x, y]` as one of the vertices.
* Example:
  * Input: ["DetectSquares", "add", "add", "add", "count", "count", "add", "count"]
    [[], [[3, 10]], [[11, 2]], [[3, 2]], [[11, 10]], [[14, 8]], [[11, 2]], [[11, 10]]]
  * Output: [null, null, null, null, 1, 0, null, 2]
  * Explanation:
    * DetectSquares detectSquares = new DetectSquares();
    * detectSquares.add([3, 10]);
    * detectSquares.add([11, 2]);
    * detectSquares.add([3, 2]);
    * detectSquares.count([11, 10]); // return 1. You can choose:
                                    //   - The first, second, and third points to form a square with the query point.
    * detectSquares.count([14, 8]);  // return 0. No square can be formed with the query point.
    * detectSquares.add([11, 2]);    // Adding duplicate points is allowed.
    * detectSquares.count([11, 10]); // return 2. You can choose:
                                    //   - The first, second, and third points to form a square with the query point.
                                    //   - The first, third, and fourth points to form a square with the query point.
* Constraints:
  * `point.length == 2`
  * `0 <= x, y <= 1000`
  * At most 3000 calls in total will be made to `add` and `count`.

---

## **Step 1: Thinking Process (Brute Force)**

* To detect squares, we need to keep track of all points that have been added.
* For each query point, we need to find all possible squares that can be formed with this point as one of the vertices.
* A square has 4 vertices, and we already have 1 (the query point). We need to find the other 3 vertices.
* For an axis-aligned square, if we have a query point (x, y), the other 3 vertices would be at positions:
  * (x + d, y), (x, y + d), and (x + d, y + d) for some distance d, or
  * (x - d, y), (x, y + d), and (x - d, y + d) for some distance d, or
  * (x + d, y), (x, y - d), and (x + d, y - d) for some distance d, or
  * (x - d, y), (x, y - d), and (x - d, y - d) for some distance d.
* We need to check if these points exist in our data structure.

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a data structure to store the points.
2. For the `add` method:
   1. Add the point to the data structure.
3. For the `count` method:
   1. Get the query point (x, y).
   2. Initialize a counter to 0.
   3. For each point (px, py) in our data structure:
      1. If px != x and py != y (i.e., the point is diagonally opposite to the query point):
         1. Check if the points (x, py) and (px, y) also exist in our data structure.
         2. If they do, increment the counter.
   4. Return the counter.

---

## **Step 3: Brute Force Implementation (Code)**

```python
class DetectSquares:
    def __init__(self):
        self.points = []
        
    def add(self, point):
        self.points.append(point)
        
    def count(self, point):
        x, y = point
        count = 0
        
        for px, py in self.points:
            # Skip points that are not diagonally opposite to the query point
            if px == x or py == y or abs(px - x) != abs(py - y):
                continue
                
            # Check if the other two vertices exist
            if [x, py] in self.points and [px, y] in self.points:
                count += 1
                
        return count
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity:
  * `add`: O(1) to append a point to the list.
  * `count`: O(n^2) where n is the number of points. For each point, we check if two other points exist in the list, which takes O(n) time each.
* Space Complexity: O(n) to store the points.
* This approach is inefficient for the `count` method, especially when there are many points.

---

## **Step 5: Visualizing Redundant Computation**

* For the example:
  * Add (3, 10), (11, 2), (3, 2)
  * Count (11, 10):
    * For point (3, 10):
      * Diagonal distance: abs(11 - 3) = 8, abs(10 - 10) = 0, not equal, skip.
    * For point (11, 2):
      * Diagonal distance: abs(11 - 11) = 0, abs(10 - 2) = 8, not equal, skip.
    * For point (3, 2):
      * Diagonal distance: abs(11 - 3) = 8, abs(10 - 2) = 8, equal.
      * Check if (11, 2) and (3, 10) exist, they do.
      * Increment count to 1.
  * Return 1.

---

## **Why are we using this algorithm?**

* The brute force approach is inefficient because we're checking if points exist in the list, which takes O(n) time.
* We can optimize this by using a hash map to store the count of each point, allowing for O(1) lookups.
* Additionally, we can optimize the way we find diagonal points by directly calculating the coordinates of the other vertices based on the query point and each point in our data structure.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of storing points in a list, we'll use a hash map to store the count of each point. This allows for O(1) lookups.
* For the `count` method, we'll iterate through all points and for each point that shares either the x or y coordinate with the query point, we'll calculate the coordinates of the other two vertices of the square and check if they exist in our hash map.
* This approach reduces the time complexity of the `count` method from O(n^2) to O(n).

### **Step 2: Flow Steps (Optimized)**

1. Initialize a hash map to store the count of each point.
2. For the `add` method:
   1. Increment the count of the point in the hash map.
3. For the `count` method:
   1. Get the query point (x, y).
   2. Initialize a counter to 0.
   3. Iterate through all points in the hash map:
      1. For each point (px, py):
         1. If px != x and py != y and abs(px - x) == abs(py - y) (i.e., the point forms a diagonal with the query point):
            1. Check if the points (x, py) and (px, y) exist in the hash map.
            2. If they do, increment the counter by the product of their counts.
   4. Return the counter.

### **Step 3: Implementation (Optimized)**

```python
from collections import defaultdict

class DetectSquares:
    def __init__(self):
        self.points_count = defaultdict(int)
        
    def add(self, point):
        self.points_count[tuple(point)] += 1
        
    def count(self, point):
        x, y = point
        count = 0
        
        for (px, py), freq in self.points_count.items():
            # Skip points that are not diagonally opposite to the query point
            if px == x or py == y or abs(px - x) != abs(py - y):
                continue
                
            # Check if the other two vertices exist
            if (x, py) in self.points_count and (px, y) in self.points_count:
                count += freq * self.points_count[(x, py)] * self.points_count[(px, y)]
                
        return count
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity:
  * `add`: O(1) to update the hash map.
  * `count`: O(n) where n is the number of unique points. For each point, we check if two other points exist in the hash map, which takes O(1) time each.
* Space Complexity: O(n) to store the points in the hash map.
* This approach is much more efficient than the brute force approach, especially when there are many points.

### **Step 5: Visualizing Computation (Optimized)**

* For the example:
  * Add (3, 10), (11, 2), (3, 2)
  * points_count = {(3, 10): 1, (11, 2): 1, (3, 2): 1}
  * Count (11, 10):
    * For point (3, 10):
      * Diagonal distance: abs(11 - 3) = 8, abs(10 - 10) = 0, not equal, skip.
    * For point (11, 2):
      * Diagonal distance: abs(11 - 11) = 0, abs(10 - 2) = 8, not equal, skip.
    * For point (3, 2):
      * Diagonal distance: abs(11 - 3) = 8, abs(10 - 2) = 8, equal.
      * Check if (11, 2) and (3, 10) exist, they do.
      * Increment count by 1 * 1 * 1 = 1.
  * Return 1.

### **Complexity Summary**

| Approach | Time (add) | Time (count) | Space | Notes |
|---|---|---|---|---|
| Brute Force | O(1) | O(n^2) | O(n) | Stores points in a list |
| Optimized | O(1) | O(n) | O(n) | Uses a hash map to store point counts |

---

## **Final Takeaways**

* The Detect Squares problem is a good example of how using appropriate data structures can significantly improve the efficiency of an algorithm.
* By using a hash map to store the count of each point, we reduced the time complexity of the `count` method from O(n^2) to O(n).
* This problem also demonstrates the importance of understanding the geometric properties of the problem (in this case, the properties of axis-aligned squares).

---

## **Real-life Analogy**

* Imagine you're a city planner trying to identify all possible locations for square parks. Instead of checking every possible combination of four points, you can be more efficient by first identifying pairs of points that could form the diagonal of a square, and then checking if the other two corners of the square are available.

---

## **Summary**

* We tackled the "Detect Squares" problem by using a hash map to efficiently store and look up points. For each query point, we identified potential diagonal points and checked if the other two vertices of the square existed in our data structure. This approach has a time complexity of O(1) for the `add` method and O(n) for the `count` method, where n is the number of unique points. This is much more efficient than the brute force approach, which has a time complexity of O(n^2) for the `count` method. 