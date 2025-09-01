# 📝 Two Sum II (Input Array Is Sorted)

## **Problem Statement**

* Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number.
* Return the indices of the two numbers, index1 and index2, added by one as an integer array [index1, index2] of length 2.
* The tests are generated such that there is exactly one solution. You may not use the same element twice.
* Your solution must use only constant extra space.
* Example:
  * Input: numbers = [2,7,11,15], target = 9
  * Output: [1,2]
  * Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].
* Constraints:
  * 2 <= numbers.length <= 3 * 10^4
  * -1000 <= numbers[i] <= 1000
  * numbers is sorted in non-decreasing order.
  * -1000 <= target <= 1000
  * The tests are generated such that there is exactly one solution.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find two numbers in the sorted array that add up to the target
* The most straightforward approach would be to check every possible pair
* For each element, we can iterate through the rest of the array to find its complement
* If we find a pair that adds up to the target, we return their indices (plus one, since it's 1-indexed)
* This is similar to the original Two Sum problem, but now the array is sorted

---

## **Step 2: Flow Steps (Brute Force)**

1. For each index i from 0 to n-2:
   1. For each index j from i+1 to n-1:
      1. If numbers[i] + numbers[j] equals target, return [i+1, j+1]
2. Return empty array (no solution found, though problem guarantees one exists)

---

## **Step 3: Brute Force Implementation (Code)**

```python
def twoSum(numbers, target):
    n = len(numbers)
    
    for i in range(n):
        for j in range(i + 1, n):
            if numbers[i] + numbers[j] == target:
                return [i + 1, j + 1]  # 1-indexed
    
    return []  # No solution found (though problem guarantees one exists)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the length of the array. We have two nested loops.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach doesn't take advantage of the fact that the array is sorted.

---

## **Step 5: Visualizing Redundant Computation**

* For array [2,7,11,15] with target 9:
  * Check (2,7) → 2+7=9 ✓
  * Return [1,2]
* But for larger arrays, we'd be checking many pairs unnecessarily
* Since the array is sorted, we can use this property to avoid checking all pairs
* For example, if the sum is too large, we know we need to consider smaller elements

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Two-Pointer algorithm.
* The property that matches this problem is the ability to narrow down the search space based on the sorted property.
* By using two pointers from opposite ends, we can efficiently find the pair that sums to the target.
* Alternatives like binary search would work but would be more complex.
* Precondition: The array is sorted in non-decreasing order.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Since the array is sorted, we can use a two-pointer approach
* We start with one pointer at the beginning (left) and one at the end (right)
* If the sum of the elements at these pointers is equal to the target, we've found our answer
* If the sum is less than the target, we need a larger sum, so we move the left pointer to the right
* If the sum is greater than the target, we need a smaller sum, so we move the right pointer to the left
* We continue this process until we find the target sum or the pointers meet

### **Step 2: Flow Steps (Optimized)**

1. Initialize two pointers: `left = 0` and `right = len(numbers) - 1`
2. While `left < right`:
   1. Calculate `current_sum = numbers[left] + numbers[right]`
   2. If `current_sum == target`, return `[left + 1, right + 1]` (1-indexed)
   3. If `current_sum < target`, increment `left`
   4. If `current_sum > target`, decrement `right`
3. Return empty array (no solution found, though problem guarantees one exists)

### **Step 3: Implementation (Optimized)**

```python
def twoSum(numbers, target):
    left, right = 0, len(numbers) - 1
    
    while left < right:
        current_sum = numbers[left] + numbers[right]
        
        if current_sum == target:
            return [left + 1, right + 1]  # 1-indexed
        elif current_sum < target:
            left += 1
        else:  # current_sum > target
            right -= 1
    
    return []  # No solution found (though problem guarantees one exists)
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. In the worst case, we might need to scan the entire array once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is much more efficient than the brute force method, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For array [2,7,11,15] with target 9:
  * left=0 (2), right=3 (15) → sum=17 > target, so right--
  * left=0 (2), right=2 (11) → sum=13 > target, so right--
  * left=0 (2), right=1 (7) → sum=9 == target ✓
  * Return [1,2]
* We're efficiently narrowing down the search space based on the comparison of the current sum with the target.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Checks all possible pairs |
| Optimized (Algorithm: Two-Pointer) | O(n) | O(1) | Uses the sorted property to narrow down the search |

---

## **Final Takeaways**

* Using a two-pointer approach allows us to leverage the sorted property of the array to efficiently find the target sum.
* The key insight is that we can narrow down the search space based on whether the current sum is less than, equal to, or greater than the target.
* This problem demonstrates how understanding the properties of the input data (sorted in this case) can lead to more efficient algorithms.

---

## **Real-life Analogy**

* Imagine you're trying to find two weights that add up to a specific total on a balance scale. The brute force approach would be like trying every possible pair of weights. The optimized approach is like arranging the weights in order, then using two pointers from opposite ends: if the current pair is too heavy, you remove the heavier weight; if it's too light, you add a heavier weight. This way, you efficiently narrow down to the correct pair.

---

## **Summary**

* We solved the "Two Sum II" problem by first considering a brute force approach that checks all possible pairs, resulting in O(n²) time complexity. We then optimized using a two-pointer approach that leverages the sorted property of the array, reducing the time complexity to O(n) while maintaining O(1) space complexity. This demonstrates the power of using appropriate algorithms that take advantage of the properties of the input data. 