# 📝 Two Sum

## **Problem Statement**

* Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.
* You may assume that each input would have exactly one solution, and you may not use the same element twice.
* You can return the answer in any order.
* Example:
  * Input: nums = [2,7,11,15], target = 9
  * Output: [0,1] (because nums[0] + nums[1] = 2 + 7 = 9)
* Constraints:
  * 2 <= nums.length <= 10^4
  * -10^9 <= nums[i] <= 10^9
  * -10^9 <= target <= 10^9
  * Only one valid answer exists

---

## **Step 1: Thinking Process (Brute Force)**

* The most straightforward approach is to check every possible pair of numbers
* For each number, we can iterate through the rest of the array to find its complement
* If we find two numbers that add up to the target, we return their indices
* This requires checking all possible pairs of elements

---

## **Step 2: Flow Steps (Brute Force)**

1. For each index i from 0 to n-1:
   1. For each index j from i+1 to n-1:
      1. If nums[i] + nums[j] equals target, return [i, j]
2. Return empty array (no solution found, though problem guarantees one exists)

---

## **Step 3: Brute Force Implementation (Code)**

```python
def twoSum(nums, target):
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []  # No solution found (though problem guarantees one exists)
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the length of the array. We have two nested loops.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This is inefficient because we're checking every possible pair, which grows quadratically with the size of the input.

---

## **Step 5: Visualizing Redundant Computation**

* For array [2,7,11,15] with target 9:
  * Compare 2 with 7, 11, 15
  * Compare 7 with 11, 15
  * Compare 11 with 15
* For each element, we're repeatedly calculating what its complement would be (target - current number) and then searching for it.
* We're not utilizing the fact that we know exactly what value we're looking for at each step.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Hash Table (Map) algorithm.
* The property that matches this problem is the ability to find complements in constant time.
* A hash map allows us to store elements we've seen and look them up in O(1) time.
* Alternatives like sorting would work but would lose the original indices.
* Precondition: Hash function distributes elements uniformly (generally true for integers).

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of checking every pair, we can use a hash map to store elements we've seen
* For each element, we calculate its complement (target - current number)
* If the complement exists in our hash map, we've found our pair
* If not, we add the current element to the hash map and continue

### **Step 2: Flow Steps (Optimized)**

1. Create an empty hash map to store (value -> index) pairs
2. For each index i and value nums[i]:
   1. Calculate complement = target - nums[i]
   2. If complement exists in hash map, return [hash_map[complement], i]
   3. Otherwise, add nums[i] to hash map with its index: hash_map[nums[i]] = i
3. Return empty array (no solution found, though problem guarantees one exists)

### **Step 3: Implementation (Optimized)**

```python
def twoSum(nums, target):
    num_map = {}  # value -> index
    
    for i, num in enumerate(nums):
        complement = target - num
        if complement in num_map:
            return [num_map[complement], i]
        num_map[num] = i
    
    return []  # No solution found (though problem guarantees one exists)
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array. We only need to iterate through the array once.
* Space Complexity: O(n) in the worst case where we might need to store almost all elements in our hash map.
* Much more efficient than the brute force approach, especially for large arrays.

### **Step 5: Visualizing Computation (Optimized)**

* For array [2,7,11,15] with target 9:
  * Process 2: Complement is 7, not in map → Add {2:0} to map
  * Process 7: Complement is 2, found in map → Return [0,1]
* We only need to check each element once and use the hash map to find complements in constant time.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Checks all possible pairs |
| Optimized (Algorithm: Hash Map) | O(n) | O(n) | Uses a map for O(1) lookups of complements |

---

## **Final Takeaways**

* Using a hash map allows us to trade space for time, significantly improving the time complexity.
* The key insight is to store elements we've seen and check if the complement exists as we go.
* This "complement finding" pattern using hash maps is common in many problems that involve pairs or sums.

---

## **Real-life Analogy**

* Imagine you're at a shoe store looking for a pair that costs exactly $100. The brute force approach would be like checking every possible pair of shoes to see if they add up to $100. The optimized approach is like keeping a list of shoes you've seen and their prices - for each new shoe priced at $x, you immediately check if you've seen a shoe priced at $(100-x) before.

---

## **Summary**

* We solved the "Two Sum" problem by first using a brute force approach that checks all possible pairs, resulting in O(n²) time complexity. We then optimized using a hash map to store elements we've seen and check for complements in constant time, reducing the time complexity to O(n) at the cost of O(n) space. This demonstrates the common technique of using a hash map to find complements or pairs that satisfy certain conditions in a single pass. 