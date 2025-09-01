# 📝 Last Stone Weight

## **Problem Statement**

* You are given an array of integers `stones` where `stones[i]` is the weight of the `i`th stone.
* We are playing a game with the stones. On each turn, we choose the heaviest two stones and smash them together. Suppose the heaviest two stones have weights `x` and `y` with `x <= y`. The result of this smash is:
  * If `x == y`, both stones are destroyed.
  * If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y - x`.
* At the end of the game, there is at most one stone left.
* Return the weight of the last remaining stone. If there are no stones left, return `0`.
* Example 1:
  * Input: stones = [2,7,4,1,8,1]
  * Output: 1
  * Explanation: 
    * We combine 7 and 8 to get 1 so the array converts to [2,4,1,1,1]
    * We combine 2 and 4 to get 2 so the array converts to [2,1,1,1]
    * We combine 2 and 1 to get 1 so the array converts to [1,1,1]
    * We combine 1 and 1 to get 0 so the array converts to [1]
    * There is only one stone left, so we return 1.
* Example 2:
  * Input: stones = [1]
  * Output: 1
  * Explanation: There is only one stone, so we return 1.
* Constraints:
  * 1 <= stones.length <= 30
  * 1 <= stones[i] <= 1000

---

## **Step 1: Thinking Process (Brute Force)**

* We need to simulate the game of smashing stones together
* In each step, we need to find the two heaviest stones, smash them, and update the list of stones
* One approach would be to sort the array after each smash operation
* This approach is straightforward but might be inefficient for large arrays

---

## **Step 2: Flow Steps (Brute Force)**

1. While there are at least two stones:
   1. Sort the array in ascending order
   2. Take the two heaviest stones (the last two elements)
   3. Smash them together according to the rules
   4. Update the array with the result
2. If there is one stone left, return its weight
3. If there are no stones left, return 0

---

## **Step 3: Brute Force Implementation (Code)**

```python
def lastStoneWeight(stones):
    while len(stones) > 1:
        stones.sort()
        y = stones.pop()  # Heaviest stone
        x = stones.pop()  # Second heaviest stone
        
        if x != y:
            stones.append(y - x)  # Add the remaining weight back
    
    return stones[0] if stones else 0
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n^2 * log n) where n is the number of stones. In the worst case, we need to perform n-1 smash operations, and each operation involves sorting the array, which takes O(n log n) time.
* Space Complexity: O(1) if we sort the array in-place.
* This approach is inefficient for large arrays due to the repeated sorting.

---

## **Step 5: Visualizing Redundant Computation**

* For the example stones = [2,7,4,1,8,1]:
  * Sort: [1,1,2,4,7,8]
  * Smash 7 and 8: 8 - 7 = 1, array becomes [1,1,2,4,1]
  * Sort: [1,1,1,2,4]
  * Smash 2 and 4: 4 - 2 = 2, array becomes [1,1,1,2]
  * Sort: [1,1,1,2]
  * Smash 1 and 2: 2 - 1 = 1, array becomes [1,1,1]
  * Sort: [1,1,1]
  * Smash 1 and 1: 1 - 1 = 0, array becomes [1]
  * Return 1
* We're sorting the array after each smash operation, which is redundant

---

## **Why are we using this algorithm?**

* For optimization, we'll use a max-heap data structure.
* The property that matches this problem is that we always need to find and remove the two largest elements, which is efficiently supported by a max-heap.
* By using a max-heap, we can avoid sorting the entire array after each smash operation.
* Alternatives like the brute force approach would be less efficient due to the repeated sorting.
* Precondition: We need to be able to efficiently find and remove the two largest elements.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Max-Heap)**

* We can use a max-heap to efficiently find and remove the two heaviest stones
* In Python, the `heapq` module provides a min-heap, so we'll negate the values to simulate a max-heap
* After each smash operation, we'll add the result back to the heap if necessary
* This approach avoids sorting the array after each operation

### **Step 2: Flow Steps (Optimized - Max-Heap)**

1. Create a max-heap from the stones array by negating each value
2. While there are at least two stones in the heap:
   1. Extract the heaviest stone (the root of the max-heap)
   2. Extract the second heaviest stone
   3. If they are not equal, add the result of the smash back to the heap
3. If there is one stone left in the heap, return its weight (negated)
4. If there are no stones left, return 0

### **Step 3: Implementation (Optimized - Max-Heap)**

```python
import heapq

def lastStoneWeight(stones):
    # Convert to max-heap by negating values
    stones = [-s for s in stones]
    heapq.heapify(stones)
    
    while len(stones) > 1:
        y = -heapq.heappop(stones)  # Heaviest stone
        x = -heapq.heappop(stones)  # Second heaviest stone
        
        if x != y:
            heapq.heappush(stones, -(y - x))  # Add the remaining weight back
    
    return -stones[0] if stones else 0
```

### **Step 4: Complexity Analysis (Optimized - Max-Heap)**

* Time Complexity: O(n log n) where n is the number of stones. Building the heap takes O(n) time, and each of the n-1 smash operations takes O(log n) time for heap operations.
* Space Complexity: O(n) for storing the heap.
* This approach is much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized - Max-Heap)**

* For the example stones = [2,7,4,1,8,1]:
  * Create max-heap: [-8,-7,-4,-2,-1,-1]
  * Extract -8 and -7: 8 and 7, smash result is 1, heap becomes [-4,-2,-1,-1,-1]
  * Extract -4 and -2: 4 and 2, smash result is 2, heap becomes [-2,-1,-1,-1]
  * Extract -2 and -1: 2 and 1, smash result is 1, heap becomes [-1,-1,-1]
  * Extract -1 and -1: 1 and 1, smash result is 0, heap becomes [-1]
  * Return 1
* The max-heap approach efficiently finds and removes the two largest elements without sorting the entire array

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Sort) | O(n^2 * log n) | O(1) | Inefficient due to repeated sorting |
| Optimized (Max-Heap) | O(n log n) | O(n) | Efficient for finding largest elements |

---

## **Final Takeaways**

* The key to efficiently solving the Last Stone Weight problem is to use a data structure that supports finding and removing the maximum elements efficiently.
* A max-heap is an ideal choice for this problem, as it provides O(log n) time complexity for extracting the maximum element.
* This problem demonstrates how choosing the right data structure can significantly improve the efficiency of an algorithm.

---

## **Real-life Analogy**

* Imagine you're playing a game where you have a pile of stones, and in each round, you need to take the two heaviest stones and smash them together. The brute force approach would be like sorting all the stones by weight after each smash, which is inefficient. The max-heap approach would be like keeping the stones in a special pile where the heaviest stones are always at the top, making it easy to find and remove them.

---

## **Summary**

* We solved the Last Stone Weight problem by first considering a brute force approach that sorts the array after each smash operation, resulting in O(n^2 * log n) time complexity. We then optimized using a max-heap to efficiently find and remove the two largest elements, reducing the time complexity to O(n log n). The key insight is to use a data structure that supports finding and removing the maximum elements efficiently. 