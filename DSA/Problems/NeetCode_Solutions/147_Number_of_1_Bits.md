# 📝 Number of 1 Bits

## **Problem Statement**

* Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).
* Example:
  * Input: n = 00000000000000000000000000001011
  * Output: 3 (The input binary string 00000000000000000000000000001011 has a total of three '1' bits)
  * Input: n = 00000000000000000000000010000000
  * Output: 1 (The input binary string 00000000000000000000000010000000 has a total of one '1' bit)
  * Input: n = 11111111111111111111111111111101
  * Output: 31 (The input binary string 11111111111111111111111111111101 has a total of thirty one '1' bits)
* Constraints:
  * The input must be a binary string of length 32.

---

## **Step 1: Thinking Process (Brute Force)**

* To count the number of '1' bits in an integer, we can iterate through each bit and check if it's a 1.
* We can use the bitwise AND operation to check if a specific bit is set.
* Alternatively, we can convert the integer to its binary representation and count the number of '1's in the string.
* Let's explore these approaches.

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize a counter to 0.
2. For each bit position from 0 to 31:
   1. Check if the bit at that position is set (i.e., it's a 1).
   2. If it is, increment the counter.
3. Return the counter.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def hammingWeight(n):
    count = 0
    
    # Check each bit position
    for i in range(32):
        # Check if the bit at position i is set
        if (n & (1 << i)) != 0:
            count += 1
    
    return count
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(1) as we always check 32 bits (for a 32-bit integer).
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is actually quite efficient, but there are even more efficient ways to solve this problem.

---

## **Step 5: Visualizing Computation**

* For n = 00000000000000000000000000001011 (decimal 11):
  * Initialize count = 0
  * Check bit 0: 11 & (1 << 0) = 11 & 1 = 1, so increment count to 1
  * Check bit 1: 11 & (1 << 1) = 11 & 2 = 2, so increment count to 2
  * Check bit 2: 11 & (1 << 2) = 11 & 4 = 0, so don't increment count
  * Check bit 3: 11 & (1 << 3) = 11 & 8 = 8, so increment count to 3
  * Check bits 4 to 31: all 0, so don't increment count
  * Return count = 3

---

## **Why are we using this algorithm?**

* The brute force approach is already quite efficient, but there's a more elegant way to count the number of set bits.
* We can use the bit manipulation trick: n & (n - 1) clears the least significant set bit of n.
* By repeatedly applying this trick until n becomes 0, we can count the number of set bits.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We'll use the bit manipulation trick: n & (n - 1) clears the least significant set bit of n.
* By repeatedly applying this trick until n becomes 0, we can count the number of set bits.
* This approach is more efficient than checking each bit position, especially for integers with few set bits.

### **Step 2: Flow Steps (Optimized)**

1. Initialize a counter to 0.
2. While n is not 0:
   1. Clear the least significant set bit of n: n = n & (n - 1).
   2. Increment the counter.
3. Return the counter.

### **Step 3: Implementation (Optimized)**

```python
def hammingWeight(n):
    count = 0
    
    while n:
        # Clear the least significant set bit
        n &= (n - 1)
        count += 1
    
    return count
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(k) where k is the number of set bits in n. In the worst case (all bits are set), this is O(32) for a 32-bit integer, which is still O(1).
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is more efficient than the brute force approach, especially for integers with few set bits.

### **Step 5: Visualizing Computation (Optimized)**

* For n = 00000000000000000000000000001011 (decimal 11):
  * Initialize count = 0
  * n = 11, n - 1 = 10, n & (n - 1) = 11 & 10 = 10, increment count to 1
  * n = 10, n - 1 = 9, n & (n - 1) = 10 & 9 = 8, increment count to 2
  * n = 8, n - 1 = 7, n & (n - 1) = 8 & 7 = 0, increment count to 3
  * n = 0, so exit the loop
  * Return count = 3

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(1) | O(1) | Checks each bit position |
| Optimized | O(k) | O(1) | Clears the least significant set bit in each iteration |

---

## **Final Takeaways**

* The Number of 1 Bits problem is a classic example of bit manipulation.
* The trick n & (n - 1) is a common bit manipulation technique that clears the least significant set bit.
* This problem demonstrates how understanding bit manipulation can lead to elegant and efficient solutions.

---

## **Real-life Analogy**

* Imagine you're counting the number of lit bulbs in a string of lights. The brute force approach would be to check each bulb one by one. The optimized approach would be to turn off one lit bulb at a time and count how many times you need to do this until all bulbs are off.

---

## **Summary**

* We tackled the "Number of 1 Bits" problem by using the bit manipulation trick: n & (n - 1) clears the least significant set bit of n. By repeatedly applying this trick until n becomes 0, we can count the number of set bits. This approach has a time complexity of O(k) where k is the number of set bits in n, and a space complexity of O(1). This is more efficient than the brute force approach of checking each bit position, especially for integers with few set bits. 