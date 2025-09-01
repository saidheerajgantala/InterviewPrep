# 📝 Partition Labels

## **Problem Statement**

* You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.
* Return a list of integers representing the size of these parts.
* Example:
  * Input: s = "ababcbacadefegdehijhklij"
  * Output: [9,7,8] (The partition is "ababcbaca", "defegde", "hijhklij")
* Constraints:
  * 1 <= s.length <= 500
  * s consists of lowercase English letters.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to partition the string such that each letter appears in at most one part.
* A naive approach would be to try all possible partitions and check if they satisfy the condition.
* However, this would be extremely inefficient.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. Generate all possible partitions of the string.
2. For each partition, check if each letter appears in at most one part.
3. Among the valid partitions, find the one with the maximum number of parts.
4. Return the sizes of the parts in this partition.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def partitionLabels(s):
    def is_valid_partition(partition):
        char_to_part = {}
        for part_idx, part in enumerate(partition):
            for char in part:
                if char in char_to_part and char_to_part[char] != part_idx:
                    return False
                char_to_part[char] = part_idx
        return True
    
    def generate_partitions(start, partition):
        if start == len(s):
            if is_valid_partition(partition):
                return partition
            return []
        
        best_partition = []
        for end in range(start + 1, len(s) + 1):
            new_part = s[start:end]
            new_partition = partition + [new_part]
            result = generate_partitions(end, new_partition)
            if len(result) > len(best_partition):
                best_partition = result
        
        return best_partition
    
    result = generate_partitions(0, [])
    return [len(part) for part in result]
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n) where n is the length of the string. There are 2^(n-1) possible ways to partition a string of length n.
* Space Complexity: O(n) for storing the partition.
* This approach is extremely inefficient and will time out for large inputs.

---

## **Step 5: Visualizing Redundant Computation**

* For s = "ababcbaca":
  * We would check all possible partitions:
    * ["a", "b", "a", "b", "c", "b", "a", "c", "a"] - Invalid because 'a' appears in multiple parts
    * ["a", "b", "a", "b", "c", "b", "a", "ca"] - Invalid
    * ... (many more partitions)
    * ["ababcbaca"] - Valid
  * There are 2^8 = 256 possible partitions to check.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a greedy approach.
* The key insight is that if a letter appears at indices i and j, then all letters in between must be in the same partition.
* We can use this to determine the minimum size of each partition.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* First, we find the last occurrence of each letter in the string.
* Then, we iterate through the string and keep track of the end of the current partition.
* The end of the current partition is the maximum of the last occurrences of all letters seen so far.
* When we reach the end of the current partition, we add its size to the result and start a new partition.

### **Step 2: Flow Steps (Optimized)**

1. Create a dictionary to store the last occurrence index of each letter.
2. Iterate through the string to populate this dictionary.
3. Initialize variables: start = 0 (start of current partition), end = 0 (end of current partition), result = [] (sizes of partitions).
4. Iterate through the string again:
   1. Update end = max(end, last_occurrence[s[i]]).
   2. If i == end, we've reached the end of the current partition:
      1. Add end - start + 1 to result (size of the current partition).
      2. Update start = i + 1 (start of the next partition).
5. Return result.

### **Step 3: Implementation (Optimized)**

```python
def partitionLabels(s):
    # Find the last occurrence of each letter
    last_occurrence = {}
    for i, char in enumerate(s):
        last_occurrence[char] = i
    
    result = []
    start = 0
    end = 0
    
    # Iterate through the string
    for i, char in enumerate(s):
        # Update the end of the current partition
        end = max(end, last_occurrence[char])
        
        # If we've reached the end of the current partition
        if i == end:
            # Add the size of the current partition to the result
            result.append(end - start + 1)
            # Start a new partition
            start = i + 1
    
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the length of the string. We iterate through the string twice.
* Space Complexity: O(k) where k is the number of unique characters in the string (at most 26 for lowercase English letters).
* This approach is much more efficient than the brute force approach.

### **Step 5: Visualizing Computation (Optimized)**

* For s = "ababcbacadefegdehijhklij":
  * Last occurrence of each letter:
    * 'a': 8, 'b': 5, 'c': 7, 'd': 14, 'e': 15, 'f': 11, 'g': 13, 'h': 19, 'i': 22, 'j': 23, 'k': 20, 'l': 21
  * Iteration:
    * i=0, char='a', end=max(0, 8)=8
    * i=1, char='b', end=max(8, 5)=8
    * i=2, char='a', end=max(8, 8)=8
    * i=3, char='b', end=max(8, 5)=8
    * i=4, char='c', end=max(8, 7)=8
    * i=5, char='b', end=max(8, 5)=8
    * i=6, char='a', end=max(8, 8)=8
    * i=7, char='c', end=max(8, 7)=8
    * i=8, char='a', end=max(8, 8)=8
      * i == end, so add 8-0+1=9 to result, start=9
    * i=9, char='d', end=max(0, 14)=14
    * ... (continue for the rest of the string)
  * Final result: [9, 7, 8]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(2^n) | O(n) | Checks all possible partitions |
| Optimized (Algorithm: Greedy) | O(n) | O(k) | Uses the last occurrence of each letter to determine partition boundaries |

---

## **Final Takeaways**

* The Partition Labels problem is a good example of how a greedy approach can be much more efficient than brute force.
* The key insight is that if a letter appears at indices i and j, then all letters in between must be in the same partition.
* This problem demonstrates how understanding the constraints of a problem can lead to a more efficient algorithm.

---

## **Real-life Analogy**

* Imagine you're organizing a series of meetings with different people. If person A needs to attend meetings at 9 AM and 3 PM, and person B needs to attend meetings at 10 AM and 2 PM, then all meetings between 9 AM and 3 PM must include both person A and person B. This is similar to how we determine the partitions in this problem.

---

## **Summary**

* We tackled the "Partition Labels" problem by first finding the last occurrence of each letter in the string. Then, we iterated through the string and kept track of the end of the current partition, which is the maximum of the last occurrences of all letters seen so far. When we reached the end of the current partition, we added its size to the result and started a new partition. This greedy approach has a time complexity of O(n) where n is the length of the string, which is much more efficient than the brute force approach of checking all possible partitions. 