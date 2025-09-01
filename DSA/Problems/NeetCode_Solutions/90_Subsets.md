# 📝 Subsets

## **Problem Statement**

* Given an integer array `nums` of unique elements, return all possible subsets (the power set).
* The solution set must not contain duplicate subsets. Return the solution in any order.
* Example 1:
  * Input: nums = [1,2,3]
  * Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
* Example 2:
  * Input: nums = [0]
  * Output: [[],[0]]
* Constraints:
  * 1 <= nums.length <= 10
  * -10 <= nums[i] <= 10
  * All the numbers of `nums` are unique.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to generate all possible subsets of the given array
* One approach would be to use the property that for an array of size n, there are 2^n possible subsets
* Each element can either be included or excluded in a subset
* We can use a binary representation to generate all subsets: for each position i, if the ith bit is 1, include the ith element; otherwise, exclude it
* This approach is straightforward and works for small arrays

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty result list to store all subsets
2. Iterate from 0 to 2^n - 1, where n is the length of the array:
   1. For each number i, create a new subset
   2. For each position j in the binary representation of i:
      1. If the jth bit is 1, include the jth element of the array in the subset
   3. Add the subset to the result list
3. Return the result list

---

## **Step 3: Brute Force Implementation (Code)**

```python
def subsets(nums):
    n = len(nums)
    result = []
    
    # Iterate through all possible combinations (2^n)
    for i in range(1 << n):  # 1 << n is equivalent to 2^n
        subset = []
        for j in range(n):
            # Check if the jth bit is set
            if (i & (1 << j)) > 0:
                subset.append(nums[j])
        result.append(subset)
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n * 2^n) where n is the length of the array. We generate 2^n subsets, and for each subset, we check n bits to determine which elements to include.
* Space Complexity: O(n * 2^n) for storing all the subsets. There are 2^n subsets, and each subset can have up to n elements.
* This approach is efficient for small arrays but can be inefficient for large arrays due to the exponential growth of the number of subsets.

---

## **Step 5: Visualizing Redundant Computation**

* For the example nums = [1,2,3]:
  * i = 0 (000): subset = []
  * i = 1 (001): subset = [1]
  * i = 2 (010): subset = [2]
  * i = 3 (011): subset = [1,2]
  * i = 4 (100): subset = [3]
  * i = 5 (101): subset = [1,3]
  * i = 6 (110): subset = [2,3]
  * i = 7 (111): subset = [1,2,3]
* There's no redundant computation in this approach, as we generate each subset exactly once

---

## **Why are we using this algorithm?**

* For optimization, we'll consider a backtracking approach.
* The property that matches this problem is that we can build subsets incrementally by making decisions to include or exclude each element.
* By using backtracking, we can generate all subsets without using the binary representation.
* Alternatives like the brute force approach would be equally efficient for this problem, but backtracking can be more intuitive and easier to extend for more complex problems.
* Precondition: We need to be able to make decisions for each element.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Backtracking)**

* We can use a backtracking approach to generate all subsets
* For each element, we have two choices: include it in the current subset or exclude it
* We can recursively explore both choices for each element
* This approach is more intuitive and can be extended to more complex problems

### **Step 2: Flow Steps (Optimized - Backtracking)**

1. Initialize an empty result list to store all subsets
2. Define a backtracking function that takes the current subset, the current index, and the array:
   1. Add the current subset to the result list
   2. For each element starting from the current index:
      1. Include the element in the current subset
      2. Recursively call the backtracking function with the updated subset and the next index
      3. Remove the element from the current subset (backtrack)
3. Call the backtracking function with an empty subset and index 0
4. Return the result list

### **Step 3: Implementation (Optimized - Backtracking)**

```python
def subsets(nums):
    result = []
    
    def backtrack(start, current):
        # Add the current subset to the result
        result.append(current.copy())
        
        # Explore all possible elements to add
        for i in range(start, len(nums)):
            # Include the current element
            current.append(nums[i])
            
            # Recursively generate subsets with the current element included
            backtrack(i + 1, current)
            
            # Exclude the current element (backtrack)
            current.pop()
    
    backtrack(0, [])
    return result
```

### **Alternative Approach: Iterative**

Another efficient approach is to build the subsets iteratively:

```python
def subsets(nums):
    result = [[]]
    
    for num in nums:
        # For each existing subset, create a new subset by adding the current element
        result.extend([subset + [num] for subset in result])
    
    return result
```

### **Step 4: Complexity Analysis (Optimized - Backtracking)**

* Time Complexity: O(n * 2^n) where n is the length of the array. We generate 2^n subsets, and for each subset, we may need to copy up to n elements.
* Space Complexity: O(n * 2^n) for storing all the subsets. There are 2^n subsets, and each subset can have up to n elements. Additionally, we use O(n) space for the recursion stack.
* This approach has the same time and space complexity as the brute force approach, but it's more intuitive and can be extended to more complex problems.

### **Step 5: Visualizing Computation (Optimized - Backtracking)**

* For the example nums = [1,2,3]:
  * Start with an empty subset: []
  * Include 1: [1]
    * Include 2: [1,2]
      * Include 3: [1,2,3]
      * Exclude 3: [1,2]
    * Exclude 2:
      * Include 3: [1,3]
      * Exclude 3: [1]
  * Exclude 1:
    * Include 2: [2]
      * Include 3: [2,3]
      * Exclude 3: [2]
    * Exclude 2:
      * Include 3: [3]
      * Exclude 3: []
* Result: [[],[1],[1,2],[1,2,3],[1,3],[2],[2,3],[3]]
* The backtracking approach efficiently generates all subsets by exploring all possible choices for each element

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Binary) | O(n * 2^n) | O(n * 2^n) | Uses binary representation |
| Optimized (Backtracking) | O(n * 2^n) | O(n * 2^n) | More intuitive and extensible |
| Iterative | O(n * 2^n) | O(n * 2^n) | Simple and efficient |

---

## **Final Takeaways**

* The key to generating all subsets is to consider each element as either included or excluded.
* Both the binary representation and backtracking approaches have the same time and space complexity, but backtracking can be more intuitive and easier to extend for more complex problems.
* The iterative approach is also efficient and can be easier to understand for some people.
* This problem demonstrates the power of backtracking for generating all possible combinations of elements.

---

## **Real-life Analogy**

* Imagine you're planning a party and need to decide which friends to invite from a list. For each friend, you have two choices: invite them or don't invite them. By considering all possible combinations of these choices, you generate all possible guest lists. This is similar to generating all subsets of a set.

---

## **Summary**

* We solved the Subsets problem by first considering a brute force approach using binary representation, resulting in O(n * 2^n) time complexity. We then optimized using a backtracking approach, which has the same time complexity but is more intuitive and extensible. We also explored an iterative approach that builds subsets incrementally. The key insight is to consider each element as either included or excluded in a subset. 