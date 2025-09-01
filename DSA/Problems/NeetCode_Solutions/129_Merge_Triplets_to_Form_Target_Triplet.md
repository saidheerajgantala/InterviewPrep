# 📝 Merge Triplets to Form Target Triplet

## **Problem Statement**

* A triplet is an array of three integers. You are given a 2D array `triplets` where `triplets[i] = [ai, bi, ci]` describes the ith triplet.
* You are also given a target triplet `target = [x, y, z]`.
* You can select from the array `triplets` and perform the following operation:
  * If you select `triplets[i]`, you will receive a triplet `[max(a0, a1, ..., ai), max(b0, b1, ..., bi), max(c0, c1, ..., ci)]`.
* Return `true` if it's possible to obtain the target triplet `[x, y, z]` by selecting from the array `triplets`. Otherwise, return `false`.
* Example:
  * Input: triplets = [[2,5,3],[1,8,4],[1,7,5]], target = [2,7,5]
  * Output: true (Select the first and third triplets [2,5,3] and [1,7,5], and the resulting triplet is [max(2,1), max(5,7), max(3,5)] = [2,7,5])
  * Input: triplets = [[3,4,5],[4,5,6]], target = [3,2,5]
  * Output: false (It's impossible to obtain [3,2,5] from the given triplets)
* Constraints:
  * 1 <= triplets.length <= 10^5
  * triplets[i].length == target.length == 3
  * 1 <= ai, bi, ci, x, y, z <= 1000

---

## **Step 1: Thinking Process (Brute Force)**

* We need to determine if it's possible to select a subset of triplets such that the maximum of each component equals the target.
* A naive approach would be to try all possible combinations of triplets and check if any combination gives the target.
* However, this would be extremely inefficient with 2^n combinations for n triplets.
* Let's think more carefully about the problem.

---

## **Step 2: Flow Steps (Brute Force)**

1. Generate all possible subsets of triplets (2^n possibilities).
2. For each subset:
   1. Calculate the resulting triplet by taking the maximum of each component.
   2. Check if the resulting triplet equals the target.
3. Return true if any subset produces the target, false otherwise.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def mergeTriplets(triplets, target):
    def check_subset(subset):
        if not subset:
            return False
        
        result = [0, 0, 0]
        for i in subset:
            triplet = triplets[i]
            result[0] = max(result[0], triplet[0])
            result[1] = max(result[1], triplet[1])
            result[2] = max(result[2], triplet[2])
        
        return result == target
    
    n = len(triplets)
    for mask in range(1, 1 << n):
        subset = [i for i in range(n) if (mask & (1 << i))]
        if check_subset(subset):
            return True
    
    return False
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(2^n * n) where n is the number of triplets. We generate 2^n subsets, and for each subset, we spend O(n) time calculating the resulting triplet.
* Space Complexity: O(n) for storing the subset.
* This approach is extremely inefficient and will time out for large inputs.

---

## **Step 5: Visualizing Redundant Computation**

* For triplets = [[2,5,3],[1,8,4],[1,7,5]], target = [2,7,5]:
  * We would check all 7 non-empty subsets:
    * [0]: [2,5,3] != [2,7,5]
    * [1]: [1,8,4] != [2,7,5]
    * [2]: [1,7,5] != [2,7,5]
    * [0,1]: [max(2,1), max(5,8), max(3,4)] = [2,8,4] != [2,7,5]
    * [0,2]: [max(2,1), max(5,7), max(3,5)] = [2,7,5] == [2,7,5] (Found a match!)
    * [1,2]: [max(1,1), max(8,7), max(4,5)] = [1,8,5] != [2,7,5]
    * [0,1,2]: [max(2,1,1), max(5,8,7), max(3,4,5)] = [2,8,5] != [2,7,5]

---

## **Why are we using this algorithm?**

* For optimization, we'll use a greedy approach.
* The key insight is that we only want to consider triplets that don't exceed the target in any component.
* If a triplet has any component greater than the corresponding target component, it can't be part of the solution.
* Among the valid triplets, we want to check if we can achieve each target component.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* First, we filter out triplets that have any component greater than the corresponding target component.
* Then, we check if we can achieve each target component from the remaining valid triplets.
* If we can achieve all three target components, we return true; otherwise, false.

### **Step 2: Flow Steps (Optimized)**

1. Filter out triplets that have any component greater than the corresponding target component.
2. Initialize three boolean variables to track if we can achieve each target component: can_get_x, can_get_y, can_get_z.
3. Iterate through the valid triplets:
   1. If a triplet has the first component equal to the target's first component, set can_get_x to true.
   2. If a triplet has the second component equal to the target's second component, set can_get_y to true.
   3. If a triplet has the third component equal to the target's third component, set can_get_z to true.
4. Return true if can_get_x, can_get_y, and can_get_z are all true; otherwise, return false.

### **Step 3: Implementation (Optimized)**

```python
def mergeTriplets(triplets, target):
    x, y, z = target
    valid_triplets = []
    
    # Filter out triplets that have any component greater than the target
    for a, b, c in triplets:
        if a <= x and b <= y and c <= z:
            valid_triplets.append([a, b, c])
    
    # Check if we can achieve each target component
    can_get_x = False
    can_get_y = False
    can_get_z = False
    
    for a, b, c in valid_triplets:
        if a == x:
            can_get_x = True
        if b == y:
            can_get_y = True
        if c == z:
            can_get_z = True
        
        # Early exit if we can achieve all components
        if can_get_x and can_get_y and can_get_z:
            return True
    
    return can_get_x and can_get_y and can_get_z
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the number of triplets. We iterate through the triplets twice.
* Space Complexity: O(n) in the worst case for storing the valid triplets.
* This approach is much more efficient than the brute force approach.

### **Step 5: Visualizing Computation (Optimized)**

* For triplets = [[2,5,3],[1,8,4],[1,7,5]], target = [2,7,5]:
  * Filter valid triplets:
    * [2,5,3]: All components <= target, so it's valid.
    * [1,8,4]: 8 > 7, so it's not valid.
    * [1,7,5]: All components <= target, so it's valid.
  * valid_triplets = [[2,5,3], [1,7,5]]
  * Check if we can achieve each target component:
    * [2,5,3]: a = 2 == x, can_get_x = true
    * [1,7,5]: b = 7 == y, can_get_y = true; c = 5 == z, can_get_z = true
  * can_get_x && can_get_y && can_get_z = true, so return true

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(2^n * n) | O(n) | Checks all possible subsets |
| Optimized (Algorithm: Greedy) | O(n) | O(n) | Filters valid triplets and checks if we can achieve each target component |

---

## **Final Takeaways**

* The Merge Triplets problem is a good example of how a greedy approach can be much more efficient than brute force.
* The key insight is to filter out triplets that can't be part of the solution and then check if we can achieve each target component.
* This problem demonstrates how understanding the constraints of a problem can lead to a more efficient algorithm.

---

## **Real-life Analogy**

* Imagine you're trying to create a specific RGB color (target) by mixing different paint colors (triplets). You would first eliminate any paints that have any component (R, G, or B) exceeding your target color. Then, from the remaining paints, you check if you can achieve each of the R, G, and B values of your target color.

---

## **Summary**

* We tackled the "Merge Triplets to Form Target Triplet" problem by first filtering out triplets that have any component greater than the corresponding target component. Then, we checked if we can achieve each target component from the remaining valid triplets. This greedy approach has a time complexity of O(n) where n is the number of triplets, which is much more efficient than the brute force approach of checking all possible subsets. 