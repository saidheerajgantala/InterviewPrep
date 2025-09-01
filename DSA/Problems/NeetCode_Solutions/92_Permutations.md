# 📝 Permutations

## **Problem Statement**

* Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in any order.
* Example 1:
  * Input: nums = [1,2,3]
  * Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
* Example 2:
  * Input: nums = [0,1]
  * Output: [[0,1],[1,0]]
* Example 3:
  * Input: nums = [1]
  * Output: [[1]]
* Constraints:
  * 1 <= nums.length <= 6
  * -10 <= nums[i] <= 10
  * All the integers of `nums` are unique.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to generate all possible permutations of the given array
* For an array of size n, there are n! possible permutations
* One approach would be to use a recursive algorithm to generate all permutations
* For each position in the permutation, we try each of the remaining numbers
* This approach is straightforward but might not be the most efficient

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty result list to store all permutations
2. Define a recursive function that takes the current permutation and the remaining numbers:
   1. If there are no remaining numbers, add the current permutation to the result list
   2. For each remaining number:
      1. Add the number to the current permutation
      2. Recursively call the function with the updated permutation and the remaining numbers (excluding the current number)
      3. Remove the number from the current permutation (backtrack)
3. Call the recursive function with an empty permutation and all numbers
4. Return the result list

---

## **Step 3: Brute Force Implementation (Code)**

```python
def permute(nums):
    result = []
    
    def backtrack(current, remaining):
        if not remaining:
            result.append(current.copy())
            return
        
        for i in range(len(remaining)):
            current.append(remaining[i])
            backtrack(current, remaining[:i] + remaining[i+1:])
            current.pop()
    
    backtrack([], nums)
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n * n!) where n is the length of the array. We generate n! permutations, and for each permutation, we may need to copy O(n) elements.
* Space Complexity: O(n * n!) for storing all the permutations. There are n! permutations, and each permutation has n elements. Additionally, we use O(n) space for the recursion stack.
* This approach is efficient for small arrays but can be inefficient for large arrays due to the factorial growth of the number of permutations.

---

## **Step 5: Visualizing Redundant Computation**

* For the example nums = [1,2,3]:
  * Start with an empty permutation: []
  * Try 1: [1], remaining = [2,3]
    * Try 2: [1,2], remaining = [3]
      * Try 3: [1,2,3], remaining = [], add to result
    * Try 3: [1,3], remaining = [2]
      * Try 2: [1,3,2], remaining = [], add to result
  * Try 2: [2], remaining = [1,3]
    * Try 1: [2,1], remaining = [3]
      * Try 3: [2,1,3], remaining = [], add to result
    * Try 3: [2,3], remaining = [1]
      * Try 1: [2,3,1], remaining = [], add to result
  * Try 3: [3], remaining = [1,2]
    * Try 1: [3,1], remaining = [2]
      * Try 2: [3,1,2], remaining = [], add to result
    * Try 2: [3,2], remaining = [1]
      * Try 1: [3,2,1], remaining = [], add to result
* There's no redundant computation in this approach, as we generate each permutation exactly once

---

## **Why are we using this algorithm?**

* For optimization, we'll consider an in-place approach.
* The property that matches this problem is that we can generate permutations by swapping elements in the array.
* By using an in-place approach, we can avoid creating new arrays for the remaining elements in each recursive call.
* Alternatives like the brute force approach would be equally efficient for this problem, but the in-place approach uses less space.
* Precondition: We need to be able to modify the array in-place.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - In-place Swapping)**

* We can use an in-place approach to generate all permutations
* For each position in the permutation, we try each of the remaining elements by swapping them with the current position
* This approach avoids creating new arrays for the remaining elements in each recursive call
* It's more space-efficient than the brute force approach

### **Step 2: Flow Steps (Optimized - In-place Swapping)**

1. Initialize an empty result list to store all permutations
2. Define a recursive function that takes the current index and the array:
   1. If the current index is equal to the length of the array, add a copy of the array to the result list
   2. For each index i from the current index to the end of the array:
      1. Swap the elements at the current index and index i
      2. Recursively call the function with the next index
      3. Swap the elements back (backtrack)
3. Call the recursive function with index 0 and the input array
4. Return the result list

### **Step 3: Implementation (Optimized - In-place Swapping)**

```python
def permute(nums):
    result = []
    
    def backtrack(start):
        if start == len(nums):
            result.append(nums.copy())
            return
        
        for i in range(start, len(nums)):
            # Swap elements
            nums[start], nums[i] = nums[i], nums[start]
            
            # Recursively generate permutations for the next position
            backtrack(start + 1)
            
            # Swap back (backtrack)
            nums[start], nums[i] = nums[i], nums[start]
    
    backtrack(0)
    return result
```

### **Alternative Approach: Using Built-in Function**

Python's `itertools` module provides a built-in function `permutations` that generates all permutations of an iterable:

```python
from itertools import permutations

def permute(nums):
    return list(map(list, permutations(nums)))
```

### **Step 4: Complexity Analysis (Optimized - In-place Swapping)**

* Time Complexity: O(n * n!) where n is the length of the array. We generate n! permutations, and for each permutation, we may need to copy O(n) elements.
* Space Complexity: O(n * n!) for storing all the permutations. There are n! permutations, and each permutation has n elements. Additionally, we use O(n) space for the recursion stack.
* This approach has the same time complexity as the brute force approach, but it uses less space during the recursion because it modifies the array in-place.

### **Step 5: Visualizing Computation (Optimized - In-place Swapping)**

* For the example nums = [1,2,3]:
  * Start with nums = [1,2,3], start = 0
    * Swap nums[0] and nums[0]: nums = [1,2,3]
      * Recursive call with start = 1
        * Swap nums[1] and nums[1]: nums = [1,2,3]
          * Recursive call with start = 2
            * Swap nums[2] and nums[2]: nums = [1,2,3]
              * Recursive call with start = 3
                * Add [1,2,3] to result
            * Swap back: nums = [1,2,3]
        * Swap nums[1] and nums[2]: nums = [1,3,2]
          * Recursive call with start = 2
            * Swap nums[2] and nums[2]: nums = [1,3,2]
              * Recursive call with start = 3
                * Add [1,3,2] to result
            * Swap back: nums = [1,3,2]
        * Swap back: nums = [1,2,3]
    * Swap nums[0] and nums[1]: nums = [2,1,3]
      * ...and so on
* The in-place swapping approach efficiently generates all permutations by modifying the array in-place

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n * n!) | O(n * n!) | Creates new arrays for remaining elements |
| Optimized (In-place Swapping) | O(n * n!) | O(n * n!) | Modifies the array in-place |
| Built-in Function | O(n * n!) | O(n * n!) | Uses Python's built-in function |

---

## **Final Takeaways**

* The key to generating all permutations is to consider each element at each position.
* Both the brute force and in-place swapping approaches have the same time complexity, but the in-place approach uses less space during the recursion.
* The built-in function approach is the most concise and is recommended for most practical applications.
* This problem demonstrates the power of backtracking for generating all possible arrangements of elements.

---

## **Real-life Analogy**

* Imagine you're arranging a group of people in a line for a photo. You want to try all possible arrangements to find the best one. For each position in the line, you try each person who hasn't been placed yet. This is similar to generating all permutations of a set of elements.

---

## **Summary**

* We solved the Permutations problem by first considering a brute force approach that creates new arrays for the remaining elements in each recursive call, resulting in O(n * n!) time complexity. We then optimized using an in-place swapping approach, which has the same time complexity but uses less space during the recursion. We also explored using Python's built-in function for generating permutations. The key insight is to consider each element at each position and use backtracking to explore all possible arrangements. 