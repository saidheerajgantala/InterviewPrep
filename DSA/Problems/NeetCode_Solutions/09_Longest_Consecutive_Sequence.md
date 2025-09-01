# 📝 Longest Consecutive Sequence

## **Problem Statement**

* Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.
* You must write an algorithm that runs in O(n) time.
* Example:
  * Input: nums = [100,4,200,1,3,2]
  * Output: 4
  * Explanation: The longest consecutive elements sequence is [1,2,3,4]. Therefore its length is 4.
* Constraints:
  * 0 <= nums.length <= 10^5
  * -10^9 <= nums[i] <= 10^9

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the longest sequence of consecutive integers in the array
* The most straightforward approach would be to sort the array first
* After sorting, we can iterate through the array and count consecutive sequences
* For each element, check if it's consecutive to the previous element
* If it is, increment our current sequence length
* If not, start a new sequence
* Keep track of the maximum sequence length we've seen

---

## **Step 2: Flow Steps (Brute Force)**

1. Sort the array in ascending order
2. Initialize variables: `current_streak = 1`, `longest_streak = 0`
3. Iterate through the sorted array:
   1. If the current element is equal to the previous element, continue (skip duplicates)
   2. If the current element is consecutive to the previous element (current = previous + 1), increment `current_streak`
   3. Otherwise, reset `current_streak = 1`
   4. Update `longest_streak = max(longest_streak, current_streak)`
4. Return `longest_streak`

---

## **Step 3: Brute Force Implementation (Code)**

```python
def longestConsecutive(nums):
    if not nums:
        return 0
    
    # Sort the array
    nums.sort()
    
    current_streak = 1
    longest_streak = 1
    
    # Iterate through the sorted array
    for i in range(1, len(nums)):
        # Skip duplicates
        if nums[i] == nums[i-1]:
            continue
        
        # Check if consecutive
        if nums[i] == nums[i-1] + 1:
            current_streak += 1
        else:
            current_streak = 1
        
        longest_streak = max(longest_streak, current_streak)
    
    return longest_streak
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n log n) due to the sorting operation, where n is the length of the array.
* Space Complexity: O(1) if we sort in-place, or O(n) if the sorting algorithm uses extra space.
* This approach doesn't meet the requirement of O(n) time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For array [100,4,200,1,3,2]:
  * After sorting: [1,2,3,4,100,200]
  * We iterate through and find the sequence [1,2,3,4]
* The sorting operation is expensive (O(n log n)) and unnecessary
* We're only interested in finding consecutive sequences, not the entire sorted order
* We can use a more efficient approach to directly identify consecutive sequences

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Hash Set algorithm.
* The property that matches this problem is the ability to check for the existence of elements in O(1) time.
* A hash set allows us to quickly determine if a number is part of a sequence.
* Alternatives like sorting would be O(n log n), which doesn't meet the O(n) requirement.
* Precondition: Hash function distributes elements uniformly (generally true for integers).

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of sorting, we can use a hash set to store all numbers for O(1) lookups
* For each number, we check if it's the start of a sequence (i.e., num-1 is not in the set)
* If it is a start, we count how many consecutive numbers follow it
* This way, we only do work for numbers that are the start of a sequence
* We keep track of the longest sequence we find

### **Step 2: Flow Steps (Optimized)**

1. Create a hash set from the input array to remove duplicates and enable O(1) lookups
2. Initialize `longest_streak = 0`
3. Iterate through each number in the hash set:
   1. If the number is the start of a sequence (num-1 is not in the set):
      1. Initialize `current_streak = 1`
      2. Check for consecutive numbers (num+1, num+2, ...) in the set
      3. Update `current_streak` accordingly
      4. Update `longest_streak = max(longest_streak, current_streak)`
4. Return `longest_streak`

### **Step 3: Implementation (Optimized)**

```python
def longestConsecutive(nums):
    if not nums:
        return 0
    
    # Create a hash set from the input array
    num_set = set(nums)
    
    longest_streak = 0
    
    # Iterate through each number
    for num in num_set:
        # Check if it's the start of a sequence
        if num - 1 not in num_set:
            current_num = num
            current_streak = 1
            
            # Count consecutive numbers
            while current_num + 1 in num_set:
                current_num += 1
                current_streak += 1
            
            # Update longest streak
            longest_streak = max(longest_streak, current_streak)
    
    return longest_streak
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the array.
  * Creating the hash set takes O(n) time
  * The outer loop iterates through each number once, which is O(n)
  * The inner while loop might seem like it could make the algorithm O(n²), but each number is only visited once across all iterations of the while loop
* Space Complexity: O(n) for the hash set.
* This approach meets the O(n) time complexity requirement.

### **Step 5: Visualizing Computation (Optimized)**

* For array [100,4,200,1,3,2]:
  * Hash set: {1,2,3,4,100,200}
  * For num=1: It's a start (0 not in set), sequence = [1,2,3,4], length = 4
  * For num=2: Not a start (1 in set), skip
  * For num=3: Not a start (2 in set), skip
  * For num=4: Not a start (3 in set), skip
  * For num=100: It's a start (99 not in set), sequence = [100], length = 1
  * For num=200: It's a start (199 not in set), sequence = [200], length = 1
  * Longest sequence = 4
* We only do significant work for numbers that are the start of a sequence.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Sorting) | O(n log n) | O(1) or O(n) | Sorts the array first |
| Optimized (Algorithm: Hash Set) | O(n) | O(n) | Uses a hash set for O(1) lookups |

---

## **Final Takeaways**

* Using a hash set allows us to achieve O(1) lookups, which is crucial for meeting the O(n) time requirement.
* The key insight is to only do work for numbers that are the start of a sequence, avoiding redundant checks.
* This problem demonstrates how proper data structure selection can dramatically improve algorithm efficiency.

---

## **Real-life Analogy**

* Imagine you have a pile of numbered tickets and need to find the longest consecutive sequence. The brute force approach would be like sorting all tickets numerically and then checking for consecutive numbers. The optimized approach is like putting all tickets in separate boxes by number, then for each ticket, checking if the next consecutive number exists in another box - without having to sort everything first.

---

## **Summary**

* We solved the "Longest Consecutive Sequence" problem by first considering a sorting-based approach with O(n log n) time complexity. We then optimized using a hash set to achieve O(n) time complexity by only examining numbers that are the start of a sequence and leveraging O(1) lookups to check for consecutive numbers. This demonstrates how choosing the right data structure can help meet strict time complexity requirements. 