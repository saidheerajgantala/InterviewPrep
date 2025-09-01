# 📝 Koko Eating Bananas

## **Problem Statement**

* Koko loves to eat bananas. There are `n` piles of bananas, the `i`th pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.
* Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.
* Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.
* Return the minimum integer `k` such that she can eat all the bananas within `h` hours.
* Example:
  * Input: piles = [3,6,7,11], h = 8
  * Output: 4
  * Explanation: Koko can eat 4 bananas per hour in this way:
    * Hour 1: Eat 3 bananas from the first pile, which has 3 bananas.
    * Hour 2: Eat 4 bananas from the second pile, which has 6 bananas. The pile still has 2 bananas.
    * Hour 3: Eat 2 bananas from the second pile, which has 2 bananas.
    * Hour 4: Eat 4 bananas from the third pile, which has 7 bananas. The pile still has 3 bananas.
    * Hour 5: Eat 3 bananas from the third pile, which has 3 bananas.
    * Hour 6: Eat 4 bananas from the fourth pile, which has 11 bananas. The pile still has 7 bananas.
    * Hour 7: Eat 4 bananas from the fourth pile, which has 7 bananas. The pile still has 3 bananas.
    * Hour 8: Eat 3 bananas from the fourth pile, which has 3 bananas.
* Constraints:
  * 1 <= piles.length <= 10^4
  * piles.length <= h <= 10^9
  * 1 <= piles[i] <= 10^9

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find the minimum eating speed `k` such that Koko can eat all bananas within `h` hours
* For a given eating speed `k`, we can calculate how many hours it takes to eat all bananas
* The time to eat a pile of size `p` with speed `k` is ceil(p/k), or (p + k - 1) // k in integer division
* One approach is to try all possible values of `k` starting from 1 and find the first one that allows Koko to finish within `h` hours
* The maximum value of `k` we need to consider is the maximum pile size, as eating any faster wouldn't reduce the time further

---

## **Step 2: Flow Steps (Brute Force)**

1. Find the maximum pile size, max_pile
2. For each possible eating speed k from 1 to max_pile:
   1. Calculate the total hours needed to eat all piles at speed k
   2. If the total hours is less than or equal to h, return k

---

## **Step 3: Brute Force Implementation (Code)**

```python
def minEatingSpeed(piles, h):
    def time_to_eat(speed):
        # Calculate total hours to eat all piles at given speed
        return sum((pile + speed - 1) // speed for pile in piles)
    
    max_pile = max(piles)
    
    # Try each possible speed from 1 to max_pile
    for k in range(1, max_pile + 1):
        if time_to_eat(k) <= h:
            return k
    
    # This should not happen given the constraints
    return max_pile
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n * m) where n is the number of piles and m is the maximum pile size. For each possible speed (up to m), we calculate the time to eat all piles, which takes O(n) time.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient for large pile sizes and will likely result in a time limit exceeded error.

---

## **Step 5: Visualizing Redundant Computation**

* For piles = [3,6,7,11] and h = 8:
  * Try k = 1: time = ceil(3/1) + ceil(6/1) + ceil(7/1) + ceil(11/1) = 3 + 6 + 7 + 11 = 27 > 8, so k = 1 is not enough
  * Try k = 2: time = ceil(3/2) + ceil(6/2) + ceil(7/2) + ceil(11/2) = 2 + 3 + 4 + 6 = 15 > 8, so k = 2 is not enough
  * Try k = 3: time = ceil(3/3) + ceil(6/3) + ceil(7/3) + ceil(11/3) = 1 + 2 + 3 + 4 = 10 > 8, so k = 3 is not enough
  * Try k = 4: time = ceil(3/4) + ceil(6/4) + ceil(7/4) + ceil(11/4) = 1 + 2 + 2 + 3 = 8 <= 8, so k = 4 is the answer
* We're trying each possible value of k one by one, which is inefficient for large pile sizes
* We can observe that the time to eat decreases as k increases, which suggests we can use binary search to find the minimum valid k

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Binary Search algorithm.
* The property that matches this problem is the monotonicity of the time to eat as a function of the eating speed.
* By using binary search on the possible values of k, we can efficiently find the minimum valid eating speed.
* Alternatives like linear search would be O(m), which is inefficient for large pile sizes.
* Precondition: The time to eat decreases as the eating speed increases, forming a monotonic function.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of trying each possible value of k, we can use binary search to find the minimum valid k
* We know that k must be between 1 and the maximum pile size
* For a given k, we can calculate the total hours needed to eat all piles
* If the total hours is less than or equal to h, k is a valid eating speed, but we want to find the minimum such k
* So, if the total hours is less than or equal to h, we search for a smaller k in the left half
* If the total hours is greater than h, k is not a valid eating speed, so we search for a larger k in the right half

### **Step 2: Flow Steps (Optimized)**

1. Define a function to calculate the total hours needed to eat all piles at a given speed
2. Initialize left = 1 and right = max(piles)
3. While left <= right:
   1. Calculate mid = left + (right - left) // 2
   2. Calculate the total hours needed to eat all piles at speed mid
   3. If the total hours is less than or equal to h, search for a smaller speed in the left half: right = mid - 1
   4. If the total hours is greater than h, search for a larger speed in the right half: left = mid + 1
4. Return left as the minimum valid eating speed

### **Step 3: Implementation (Optimized)**

```python
def minEatingSpeed(piles, h):
    def time_to_eat(speed):
        # Calculate total hours to eat all piles at given speed
        return sum((pile + speed - 1) // speed for pile in piles)
    
    left, right = 1, max(piles)
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if time_to_eat(mid) <= h:
            # This speed is valid, but we want to find the minimum valid speed
            right = mid - 1
        else:
            # This speed is not valid, we need a faster speed
            left = mid + 1
    
    return left
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n * log(m)) where n is the number of piles and m is the maximum pile size. We perform binary search on the range [1, max(piles)], which takes O(log(m)) iterations, and for each iteration, we calculate the time to eat all piles, which takes O(n) time.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This is much more efficient than the brute force approach, especially for large pile sizes.

### **Step 5: Visualizing Computation (Optimized)**

* For piles = [3,6,7,11] and h = 8:
  * max(piles) = 11, so left = 1, right = 11
  * mid = 6, time = ceil(3/6) + ceil(6/6) + ceil(7/6) + ceil(11/6) = 1 + 1 + 2 + 2 = 6 <= 8, so right = mid - 1 = 5
  * left = 1, right = 5, mid = 3, time = ceil(3/3) + ceil(6/3) + ceil(7/3) + ceil(11/3) = 1 + 2 + 3 + 4 = 10 > 8, so left = mid + 1 = 4
  * left = 4, right = 5, mid = 4, time = ceil(3/4) + ceil(6/4) + ceil(7/4) + ceil(11/4) = 1 + 2 + 2 + 3 = 8 <= 8, so right = mid - 1 = 3
  * left = 4, right = 3, left > right, so we exit the loop and return left = 4
* We're efficiently finding the minimum valid eating speed using binary search.

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n * m) | O(1) | Tries each possible speed from 1 to max(piles) |
| Optimized (Algorithm: Binary Search) | O(n * log(m)) | O(1) | Uses binary search to find the minimum valid speed |

---

## **Final Takeaways**

* Binary search can be applied to problems beyond just searching in a sorted array.
* The key insight is to recognize when a problem has a monotonic property that allows us to use binary search to find the optimal value.
* This problem demonstrates how binary search can be used to efficiently find the minimum value that satisfies a given condition.

---

## **Real-life Analogy**

* Imagine you're trying to determine the minimum speed at which you need to drive to reach your destination within a given time limit. The brute force approach would be like trying each possible speed, starting from 1 mph and incrementing by 1 mph each time, until you find a speed that gets you there in time. The optimized approach is like making an educated guess about a reasonable speed, checking if it's fast enough, and then adjusting your guess higher or lower based on the result - this way, you can quickly narrow down to the minimum required speed without trying every possibility.

---

## **Summary**

* We solved the "Koko Eating Bananas" problem by first considering a linear search approach that tries each possible eating speed, resulting in O(n * m) time complexity. We then optimized using a binary search approach that leverages the monotonicity of the time to eat as a function of the eating speed, reducing the time complexity to O(n * log(m)) while maintaining O(1) space complexity. The key insight is to recognize that the problem has a monotonic property that allows us to use binary search to efficiently find the minimum valid eating speed. 