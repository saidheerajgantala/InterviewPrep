# 📝 Gas Station

## **Problem Statement**

* There are n gas stations along a circular route, where the amount of gas at the i-th station is `gas[i]`.
* You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the i-th station to its next (i+1)-th station.
* You begin the journey with an empty tank at one of the gas stations.
* Given two arrays `gas` and `cost`, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.
* If there exists a solution, it is guaranteed to be unique.
* Example:
  * Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
  * Output: 3 (Start at station 3, you can complete the circuit)
* Constraints:
  * n == gas.length == cost.length
  * 1 <= n <= 10^5
  * 0 <= gas[i], cost[i] <= 10^4

---

## **Step 1: Thinking Process (Brute Force)**

* The most straightforward approach is to try each gas station as a starting point
* For each starting point, we simulate the journey around the circuit
* We keep track of the current gas in the tank and check if we can complete the circuit
* If we can't reach the next station at any point, we try the next starting point

---

## **Step 2: Flow Steps (Brute Force)**

1. For each starting point i from 0 to n-1:
   1. Initialize current gas = 0
   2. For each station j from i to i+n-1 (wrapping around if necessary):
      1. Add gas[j % n] to current gas
      2. Subtract cost[j % n] from current gas
      3. If current gas < 0, break and try the next starting point
   3. If we've visited all stations and current gas >= 0, return i
2. If no valid starting point is found, return -1

---

## **Step 3: Brute Force Implementation (Code)**

```python
def canCompleteCircuit(gas, cost):
    n = len(gas)
    
    for start in range(n):
        current_gas = 0
        possible = True
        
        for i in range(n):
            station = (start + i) % n
            current_gas += gas[station]
            current_gas -= cost[station]
            
            if current_gas < 0:
                possible = False
                break
        
        if possible:
            return start
    
    return -1
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n²) where n is the number of gas stations. For each of the n possible starting points, we might need to simulate a journey of n steps.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is inefficient for large inputs due to the quadratic time complexity.

---

## **Step 5: Visualizing Redundant Computation**

* For gas = [1,2,3,4,5], cost = [3,4,5,1,2]:
  * Start at station 0: 1-3=-2 (Fail)
  * Start at station 1: 2-4=-2 (Fail)
  * Start at station 2: 3-5=-2 (Fail)
  * Start at station 3: 4-1=3, 3+5-2=6, 6+1-3=4, 4+2-4=2, 2+3-5=0 (Success)
  * We're recalculating many of the same gas-cost differences multiple times.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a greedy approach.
* The key insight is that if the total gas is greater than or equal to the total cost, there must be a valid starting point.
* We can find this starting point in a single pass through the array.
* If at any point our tank becomes negative, we know we can't start at any of the stations we've visited so far.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* First, we check if there's enough gas in total to complete the circuit
* If the sum of gas is less than the sum of cost, it's impossible to complete the circuit
* If it's possible, we need to find the starting point
* We can do this by keeping track of the current gas and finding the point where the tank would never go negative

### **Step 2: Flow Steps (Optimized)**

1. Check if sum(gas) < sum(cost), if true, return -1 (impossible)
2. Initialize variables:
   - total_gas = 0 (running sum of gas - cost)
   - current_gas = 0 (current gas in tank)
   - start_station = 0 (potential starting station)
3. Iterate through all stations:
   1. Add gas[i] - cost[i] to current_gas and total_gas
   2. If current_gas < 0, reset current_gas to 0 and update start_station to i+1
4. If total_gas >= 0, return start_station, otherwise return -1

### **Step 3: Implementation (Optimized)**

```python
def canCompleteCircuit(gas, cost):
    n = len(gas)
    
    # Check if there's enough gas in total
    if sum(gas) < sum(cost):
        return -1
    
    total_gas = 0
    current_gas = 0
    start_station = 0
    
    for i in range(n):
        # Update the gas in the tank
        current_gas += gas[i] - cost[i]
        total_gas += gas[i] - cost[i]
        
        # If we can't reach the next station
        if current_gas < 0:
            # Try starting from the next station
            start_station = i + 1
            current_gas = 0
    
    # If total_gas >= 0, there must be a valid starting point
    return start_station
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n) where n is the number of gas stations. We only need to iterate through the array once.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* Much more efficient than the brute force approach, especially for large inputs.

### **Step 5: Visualizing Computation (Optimized)**

* For gas = [1,2,3,4,5], cost = [3,4,5,1,2]:
  * Start: total_gas = 0, current_gas = 0, start_station = 0
  * Process station 0: current_gas = 1-3 = -2, total_gas = -2
    * current_gas < 0, so start_station = 1, current_gas = 0
  * Process station 1: current_gas = 0 + (2-4) = -2, total_gas = -4
    * current_gas < 0, so start_station = 2, current_gas = 0
  * Process station 2: current_gas = 0 + (3-5) = -2, total_gas = -6
    * current_gas < 0, so start_station = 3, current_gas = 0
  * Process station 3: current_gas = 0 + (4-1) = 3, total_gas = -3
  * Process station 4: current_gas = 3 + (5-2) = 6, total_gas = 0
  * total_gas = 0 >= 0, so return start_station = 3

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n²) | O(1) | Tries each station as a starting point |
| Optimized (Algorithm: Greedy) | O(n) | O(1) | Uses the insight that if total gas >= total cost, there must be a solution |

---

## **Final Takeaways**

* The Gas Station problem is a classic example where a greedy approach works well.
* The key insight is that if the total gas is sufficient, there must be a valid starting point.
* If at any point our tank becomes negative, we know we can't start at any of the stations we've visited so far.

---

## **Real-life Analogy**

* Imagine planning a road trip around a circular route with various gas stations. You need to find a starting point where you won't run out of gas. If the total amount of gas available across all stations is less than what you need for the entire journey, it's impossible. Otherwise, you need to find a starting point where your tank never goes empty.

---

## **Summary**

* We tackled the "Gas Station" problem first with a brute force approach that tries each station as a starting point, resulting in O(n²) time complexity. We then optimized using a greedy approach that identifies the valid starting point in a single pass, reducing the time complexity to O(n). This demonstrates how understanding the properties of a problem (in this case, that a solution exists if and only if total gas >= total cost) can lead to an efficient algorithm. 