# 📝 Combination Sum II

## **Problem Statement**

* Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.
* Each number in `candidates` may only be used once in the combination.
* Note: The solution set must not contain duplicate combinations.
* Example 1:
  * Input: candidates = [10,1,2,7,6,1,5], target = 8
  * Output: [[1,1,6],[1,2,5],[1,7],[2,6]]
* Example 2:
  * Input: candidates = [2,5,2,1,2], target = 5
  * Output: [[1,2,2],[5]]
* Constraints:
  * 1 <= candidates.length <= 100
  * 1 <= candidates[i] <= 50
  * 1 <= target <= 30

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find all unique combinations in `candidates` where the numbers sum to `target`
* Unlike the original Combination Sum problem, each number can only be used once in a combination
* Also, the input may contain duplicate numbers, but we need to avoid duplicate combinations in the output
* One approach would be to generate all possible combinations of numbers from `candidates` and check if they sum to `target`
* This approach would be very inefficient due to the large number of possible combinations

---

## **Step 2: Flow Steps (Brute Force)**

1. Generate all possible combinations of numbers from `candidates`
2. For each combination, calculate the sum
3. If the sum equals `target`, add the combination to the result list
4. Remove duplicate combinations from the result list
5. Return the result list

---

## **Step 3: Brute Force Implementation (Code)**

```python
def combinationSum2(candidates, target):
    result = []
    
    def generate_combinations(start, combination, current_sum):
        if current_sum == target:
            # Sort the combination to handle duplicates
            sorted_combination = sorted(combination)
            if sorted_combination not in result:
                result.append(sorted_combination)
            return
        
        if current_sum > target or start >= len(candidates):
            return
        
        for i in range(start, len(candidates)):
            combination.append(candidates[i])
            generate_combinations(i + 1, combination, current_sum + candidates[i])
            combination.pop()
    
    generate_combinations(0, [], 0)
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n * n) where n is the length of the `candidates` array. In the worst case, we generate 2^n combinations, and for each combination, we need O(n) time to check for duplicates.
* Space Complexity: O(n) for the recursion stack and to store each combination.
* This approach is very inefficient for large inputs due to the exponential growth of the number of combinations and the need to check for duplicates.

---

## **Step 5: Visualizing Redundant Computation**

* For the example candidates = [10,1,2,7,6,1,5], target = 8:
  * Start with an empty combination: []
  * Try 10: [10], sum = 10 > 8, backtrack
  * Try 1: [1], sum = 1
    * Try 2: [1,2], sum = 3
      * Try 7: [1,2,7], sum = 10 > 8, backtrack
      * Try 6: [1,2,6], sum = 9 > 8, backtrack
      * Try 1: [1,2,1], sum = 4
        * Try 5: [1,2,1,5], sum = 9 > 8, backtrack
      * ...and so on
  * ...and so on
* There's a lot of redundant computation because we're exploring paths that clearly exceed the target and we're not handling duplicates efficiently

---

## **Why are we using this algorithm?**

* For optimization, we'll use a backtracking approach with pruning and sorting.
* The property that matches this problem is that we can build combinations incrementally and prune branches that exceed the target.
* By sorting the candidates array, we can easily skip duplicates and avoid generating duplicate combinations.
* Alternatives like the brute force approach would be inefficient due to the large number of possible combinations and the need to check for duplicates.
* Precondition: We need to be able to make decisions for each candidate.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Backtracking with Pruning and Sorting)**

* We can use a backtracking approach to generate combinations
* To avoid duplicate combinations, we can sort the candidates array and skip duplicates during the backtracking process
* We can also prune branches where the current sum exceeds the target
* This approach efficiently handles both the constraint that each number can only be used once and the requirement to avoid duplicate combinations

### **Step 2: Flow Steps (Optimized - Backtracking with Pruning and Sorting)**

1. Sort the candidates array
2. Initialize an empty result list
3. Define a backtracking function that takes the current combination, the current sum, and the current index:
   1. If the current sum equals the target, add the current combination to the result list
   2. If the current sum exceeds the target, return (prune the branch)
   3. For each candidate starting from the current index:
      1. If the current candidate is the same as the previous candidate, skip it to avoid duplicates
      2. Include the candidate in the current combination
      3. Recursively call the backtracking function with the updated combination, sum, and the next index
      4. Remove the candidate from the current combination (backtrack)
4. Call the backtracking function with an empty combination, sum 0, and index 0
5. Return the result list

### **Step 3: Implementation (Optimized - Backtracking with Pruning and Sorting)**

```python
def combinationSum2(candidates, target):
    # Sort the candidates to handle duplicates
    candidates.sort()
    result = []
    
    def backtrack(start, combination, current_sum):
        if current_sum == target:
            result.append(combination.copy())
            return
        
        if current_sum > target:
            return
        
        for i in range(start, len(candidates)):
            # Skip duplicates
            if i > start and candidates[i] == candidates[i-1]:
                continue
            
            combination.append(candidates[i])
            backtrack(i + 1, combination, current_sum + candidates[i])
            combination.pop()
    
    backtrack(0, [], 0)
    return result
```

### **Step 4: Complexity Analysis (Optimized - Backtracking with Pruning and Sorting)**

* Time Complexity: O(2^n) where n is the length of the `candidates` array. In the worst case, we generate 2^n combinations, but pruning and skipping duplicates significantly reduce the number of combinations explored.
* Space Complexity: O(n) for the recursion stack and to store each combination.
* This approach is much more efficient than the brute force approach due to pruning and handling duplicates efficiently.

### **Step 5: Visualizing Computation (Optimized - Backtracking with Pruning and Sorting)**

* For the example candidates = [10,1,2,7,6,1,5], target = 8:
  * Sort candidates: [1,1,2,5,6,7,10]
  * Start with an empty combination: []
  * Try 1: [1], sum = 1
    * Try 1: [1,1], sum = 2
      * Try 2: [1,1,2], sum = 4
        * Try 5: [1,1,2,5], sum = 9 > 8, prune
        * Try 6: [1,1,2,6], sum = 10 > 8, prune
        * Try 7: [1,1,2,7], sum = 11 > 8, prune
        * Try 10: [1,1,2,10], sum = 14 > 8, prune
      * Try 5: [1,1,5], sum = 7
        * Try 6: [1,1,5,6], sum = 13 > 8, prune
        * Try 7: [1,1,5,7], sum = 14 > 8, prune
        * Try 10: [1,1,5,10], sum = 17 > 8, prune
      * Try 6: [1,1,6], sum = 8 == 8, add to result
      * Try 7: [1,1,7], sum = 9 > 8, prune
      * Try 10: [1,1,10], sum = 12 > 8, prune
    * Try 2: [1,2], sum = 3
      * Try 5: [1,2,5], sum = 8 == 8, add to result
      * Try 6: [1,2,6], sum = 10 > 8, prune
      * Try 7: [1,2,7], sum = 11 > 8, prune
      * Try 10: [1,2,10], sum = 13 > 8, prune
    * ...and so on
  * Skip the second 1 (duplicate)
  * Try 2: [2], sum = 2
    * Try 5: [2,5], sum = 7
      * Try 6: [2,5,6], sum = 13 > 8, prune
      * Try 7: [2,5,7], sum = 14 > 8, prune
      * Try 10: [2,5,10], sum = 17 > 8, prune
    * Try 6: [2,6], sum = 8 == 8, add to result
    * Try 7: [2,7], sum = 9 > 8, prune
    * Try 10: [2,10], sum = 12 > 8, prune
  * ...and so on
* The backtracking approach with pruning and sorting efficiently generates combinations by avoiding paths that exceed the target and skipping duplicates

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(2^n * n) | O(n) | Inefficient due to duplicates |
| Optimized (Backtracking with Pruning and Sorting) | O(2^n) | O(n) | Efficient due to pruning and handling duplicates |

---

## **Final Takeaways**

* The key to efficiently solving the Combination Sum II problem is to use backtracking with pruning and sorting.
* By sorting the candidates array, we can easily skip duplicates and avoid generating duplicate combinations.
* By pruning branches where the current sum exceeds the target, we can avoid exploring many combinations that would not lead to a valid solution.
* This problem demonstrates the power of backtracking for generating combinations with specific constraints.

---

## **Real-life Analogy**

* Imagine you're a cashier trying to make change for a customer using a limited set of coins, where each coin can only be used once. You need to find all unique ways to make the exact amount. This is similar to finding all unique combinations of candidates that sum to the target, where each number can only be used once.

---

## **Summary**

* We solved the Combination Sum II problem by using a backtracking approach with pruning and sorting, resulting in O(2^n) time complexity. The key insights are to sort the candidates array to handle duplicates efficiently, skip duplicates during the backtracking process, and prune branches where the current sum exceeds the target. This approach efficiently generates all unique combinations of candidates that sum to the target, where each number can only be used once. 