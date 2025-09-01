# 📝 Contains Duplicate

## **Problem Statement**

* Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.
* Example:
  * Input: nums = [1,2,3,1]
  * Output: true (because 1 appears twice)
* Constraints:
  * 1 <= nums.length <= 10^5
  * -10^9 <= nums[i] <= 10^9

---

## **Step 1: Thinking Process (Brute Force)**

* The most obvious way is to check each number against every other number in the array
* For each number, we'll iterate through the rest of the array to see if it appears again
* If we find a duplicate, we return true immediately
* If we check all numbers and find no duplicates, we return false

---

## **Step 2: Flow Steps (Brute Force)**

1. For each index i from 0 to n-1:
   1. For each index j from i+1 to n-1:
      1. If nums[i] equals nums[j], return true
2. Return false (no duplicates found)

---

## **Step 3: Brute Force Implementation (Code)**

```python
def containsDuplicate(nums):
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            if nums[i] == nums[j]:
                return True
    return False
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the length of the array. We have two nested loops.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This is inefficient because for each element, we're checking all other elements, leading to quadratic time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For array [1,2,3,1]:
  * We compare 1 with 2, 3, 1 (finds duplicate)
* For array [1,2,3,4]:
  * We compare 1 with 2, 3, 4
  * We compare 2 with 3, 4
  * We compare 3 with 4
* We're repeatedly comparing elements that we've already seen.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Hash Set algorithm.
* The property that matches this problem is the O(1) lookup time for sets.
* A hash set allows us to check if an element exists in constant time.
* Alternatives like sorting would work but would be O(n log n), which is less efficient.
* Precondition: Hash function distributes elements uniformly (which is generally true for integers).

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of checking each element against all others, we can use a hash set to track elements we've seen.
* As we iterate through the array, we check if the current element is already in our set.
* If it is, we've found a duplicate and return true.
* If not, we add it to our set and continue.

### **Step 2: Flow Steps (Optimized)**

1. Create an empty hash set
2. For each element in the array:
   1. If the element is already in the set, return true
   2. Otherwise, add the element to the set
3. Return false (no duplicates found)

### **Step 3: Implementation (Optimized)**

```python
def containsDuplicate(nums):
    seen = set()
    for num in nums:
        if num in seen:
            return True
        seen.add(num)
    return False
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. We only need to iterate through the array once.
* Space Complexity: O(n) in the worst case where all elements are unique and we store all of them in our set.
* Much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For array [1,2,3,1]:
  * Process 1: Add to set -> {1}
  * Process 2: Add to set -> {1,2}
  * Process 3: Add to set -> {1,2,3}
  * Process 1: Already in set -> Return true
* We only need to check each element once against our set of previously seen elements.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Compares each element with every other element |
| Optimized (Algorithm: Hash Set) | O(n) | O(n) | Uses a set for O(1) lookups |

---

## **Final Takeaways**

* Using a hash set allows us to trade space for time, significantly improving the time complexity.
* This is a common pattern: using extra space (a data structure) to reduce time complexity.
* The key insight is recognizing when we're doing redundant comparisons and finding a way to store previous results.

---

## **Real-life Analogy**

* Imagine checking attendance in a classroom. The brute force approach would be like asking each student if they have a namesake in the room, requiring each student to check with every other student. The optimized approach is like having students sign their name on a board as they enter - if someone sees their name already there, we know we have a duplicate.

---

## **Summary**

* We tackled the "Contains Duplicate" problem by first using a brute force approach of comparing each element with all others, resulting in O(n²) time complexity. We then identified the redundancy in repeatedly checking elements we've already seen and optimized using a hash set to track seen elements, reducing time complexity to O(n) at the cost of O(n) space. This demonstrates the common algorithmic technique of trading space for time. 