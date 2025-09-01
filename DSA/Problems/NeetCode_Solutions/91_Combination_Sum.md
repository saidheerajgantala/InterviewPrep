# 📝 Combination Sum

## **Problem Statement**

* Given an array of distinct integers `candidates` and a target integer `target`, return a list of all unique combinations of `candidates` where the chosen numbers sum to `target`.
* You may return the combinations in any order.
* The same number may be chosen from `candidates` an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.
* Example 1:
  * Input: candidates = [2,3,6,7], target = 7
  * Output: [[2,2,3],[7]]
  * Explanation:
    * 2 + 2 + 3 = 7
    * 7 = 7
* Example 2:
  * Input: candidates = [2,3,5], target = 8
  * Output: [[2,2,2,2],[2,3,3],[3,5]]
  * Explanation:
    * 2 + 2 + 2 + 2 = 8
    * 2 + 3 + 3 = 8
    * 3 + 5 = 8
* Example 3:
  * Input: candidates = [2], target = 1
  * Output: []
  * Explanation: There are no combinations that sum to 1.
* Constraints:
  * 1 <= candidates.length <= 30
  * 1 <= candidates[i] <= 200
  * All elements of candidates are distinct.
  * 1 <= target <= 500

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find all unique combinations of numbers from `candidates` that sum to `target`
* One approach would be to generate all possible combinations of numbers from `candidates` and check if they sum to `target`
* Since we can use each number multiple times, we need to consider all possible frequencies for each number
* This approach would be very inefficient due to the large number of possible combinations

---

## **Step 2: Flow Steps (Brute Force)**

1. Generate all possible combinations of numbers from `candidates`, allowing repetition
2. For each combination, calculate the sum
3. If the sum equals `target`, add the combination to the result list
4. Return the result list

---

## **Step 3: Brute Force Implementation (Code)**

```python
def combinationSum(candidates, target):
    result = []
    
    def generate_combinations(start, combination, current_sum):
        if current_sum == target:
            result.append(combination.copy())
            return
        
        if current_sum > target:
            return
        
        for i in range(start, len(candidates)):
            combination.append(candidates[i])
            generate_combinations(i, combination, current_sum + candidates[i])
            combination.pop()
    
    generate_combinations(0, [], 0)
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n^target) where n is the length of the `candidates` array. In the worst case, if all candidates are 1, we would need to consider all possible combinations of length `target`.
* Space Complexity: O(target) for the recursion stack and to store each combination.
* This approach is very inefficient for large values of `target` or when there are many small values in `candidates`.

---

## **Step 5: Visualizing Redundant Computation**

* For the example candidates = [2,3,6,7], target = 7:
  * Start with an empty combination: []
  * Try adding 2: [2], sum = 2
    * Try adding 2: [2,2], sum = 4
      * Try adding 2: [2,2,2], sum = 6
        * Try adding 2: [2,2,2,2], sum = 8 > 7, backtrack
        * Try adding 3: [2,2,2,3], sum = 9 > 7, backtrack
        * Try adding 6: [2,2,2,6], sum = 12 > 7, backtrack
        * Try adding 7: [2,2,2,7], sum = 13 > 7, backtrack
      * Try adding 3: [2,2,3], sum = 7 == 7, add to result
      * Try adding 6: [2,2,6], sum = 10 > 7, backtrack
      * Try adding 7: [2,2,7], sum = 11 > 7, backtrack
    * Try adding 3: [2,3], sum = 5
      * Try adding 3: [2,3,3], sum = 8 > 7, backtrack
      * Try adding 6: [2,3,6], sum = 11 > 7, backtrack
      * Try adding 7: [2,3,7], sum = 12 > 7, backtrack
    * ...and so on
* There's a lot of redundant computation because we're exploring paths that clearly exceed the target

---

## **Why are we using this algorithm?**

* For optimization, we'll use a backtracking approach with pruning.
* The property that matches this problem is that we can build combinations incrementally and prune branches that exceed the target.
* By using backtracking with pruning, we can avoid exploring combinations that will clearly exceed the target.
* Alternatives like the brute force approach would be inefficient due to the large number of possible combinations.
* Precondition: We need to be able to make decisions for each candidate.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Backtracking with Pruning)**

* We can use a backtracking approach to generate combinations
* For each candidate, we have two choices: include it in the current combination or move on to the next candidate
* We can prune branches where the current sum exceeds the target
* We can also sort the candidates to potentially prune more branches early

### **Step 2: Flow Steps (Optimized - Backtracking with Pruning)**

1. Sort the candidates array (optional, but can help with pruning)
2. Initialize an empty result list
3. Define a backtracking function that takes the current combination, the current sum, and the current index:
   1. If the current sum equals the target, add the current combination to the result list
   2. If the current sum exceeds the target, return (prune the branch)
   3. For each candidate starting from the current index:
      1. Include the candidate in the current combination
      2. Recursively call the backtracking function with the updated combination, sum, and the same index (since we can reuse the same candidate)
      3. Remove the candidate from the current combination (backtrack)
4. Call the backtracking function with an empty combination, sum 0, and index 0
5. Return the result list

### **Step 3: Implementation (Optimized - Backtracking with Pruning)**

```python
def combinationSum(candidates, target):
    result = []
    
    def backtrack(start, combination, current_sum):
        if current_sum == target:
            result.append(combination.copy())
            return
        
        if current_sum > target:
            return
        
        for i in range(start, len(candidates)):
            combination.append(candidates[i])
            backtrack(i, combination, current_sum + candidates[i])
            combination.pop()
    
    backtrack(0, [], 0)
    return result
```

### **Step 4: Complexity Analysis (Optimized - Backtracking with Pruning)**

* Time Complexity: O(n^(target/min)) where n is the length of the `candidates` array and min is the minimum value in `candidates`. This is because in the worst case, we can have at most target/min candidates in a combination.
* Space Complexity: O(target/min) for the recursion stack and to store each combination.
* This approach is much more efficient than the brute force approach due to pruning.

### **Step 5: Visualizing Computation (Optimized - Backtracking with Pruning)**

* For the example candidates = [2,3,6,7], target = 7:
  * Start with an empty combination: []
  * Try adding 2: [2], sum = 2
    * Try adding 2: [2,2], sum = 4
      * Try adding 2: [2,2,2], sum = 6
        * Try adding 2: [2,2,2,2], sum = 8 > 7, prune
        * Try adding 3: [2,2,2,3], sum = 9 > 7, prune
        * Try adding 6: [2,2,2,6], sum = 12 > 7, prune
        * Try adding 7: [2,2,2,7], sum = 13 > 7, prune
      * Try adding 3: [2,2,3], sum = 7 == 7, add to result
      * Try adding 6: [2,2,6], sum = 10 > 7, prune
      * Try adding 7: [2,2,7], sum = 11 > 7, prune
    * Try adding 3: [2,3], sum = 5
      * Try adding 3: [2,3,3], sum = 8 > 7, prune
      * Try adding 6: [2,3,6], sum = 11 > 7, prune
      * Try adding 7: [2,3,7], sum = 12 > 7, prune
    * ...and so on
* The backtracking approach with pruning efficiently generates combinations by avoiding paths that exceed the target

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n^target) | O(target) | Inefficient for large targets |
| Optimized (Backtracking with Pruning) | O(n^(target/min)) | O(target/min) | Efficient due to pruning |

---

## **Final Takeaways**

* The key to efficiently solving the Combination Sum problem is to use backtracking with pruning.
* By pruning branches where the current sum exceeds the target, we can avoid exploring many combinations that would not lead to a valid solution.
* Sorting the candidates array can potentially help with pruning, but it's not necessary for the algorithm to work correctly.
* This problem demonstrates the power of backtracking for generating combinations with specific constraints.

---

## **Real-life Analogy**

* Imagine you're a cashier trying to make change for a customer using a limited set of coin denominations. You need to find all possible ways to make the exact amount. You can use each denomination as many times as needed. This is similar to finding all combinations of candidates that sum to the target.

---

## **Summary**

* We solved the Combination Sum problem by using a backtracking approach with pruning, resulting in O(n^(target/min)) time complexity. The key insight is to build combinations incrementally and prune branches where the current sum exceeds the target. This approach efficiently generates all unique combinations of candidates that sum to the target. 