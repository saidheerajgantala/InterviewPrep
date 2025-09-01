# 📝 Reverse Bits

## **Problem Statement**

* Reverse bits of a given 32 bits unsigned integer.
* Example:
  * Input: n = 00000010100101000001111010011100
  * Output: 00111001011110000010100101000000 (964176192)
  * Input: n = 11111111111111111111111111111101
  * Output: 10111111111111111111111111111111 (3221225471)
* Constraints:
  * The input must be a binary string of length 32.

---

## **Step 1: Thinking Process (Brute Force)**

* To reverse the bits of an integer, we need to swap the positions of the bits.
* The first bit becomes the last, the second becomes the second-to-last, and so on.
* We can iterate through the bits from right to left and construct the reversed integer.
* Let's implement this approach.

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize result = 0.
2. For each bit position i from 0 to 31:
   1. Extract the bit at position i from the input.
   2. If the bit is 1, set the bit at position (31 - i) in the result.
3. Return the result.

---

## **Step 3: Brute Force Implementation (Code)**

```python
def reverseBits(n):
    result = 0
    
    for i in range(32):
        # Extract the bit at position i
        bit = (n >> i) & 1
        
        # Set the bit at position (31 - i) in the result
        result |= (bit << (31 - i))
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(1) as we always process 32 bits.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is efficient and straightforward.

---

## **Step 5: Visualizing Computation**

* For n = 00000010100101000001111010011100 (decimal 43261596):
  * Initialize result = 0
  * For i = 0:
    * bit = (n >> 0) & 1 = 0
    * result |= (0 << 31) = 0
  * For i = 1:
    * bit = (n >> 1) & 1 = 0
    * result |= (0 << 30) = 0
  * ... (continue for all bits)
  * For i = 30:
    * bit = (n >> 30) & 1 = 0
    * result |= (0 << 1) = 0
  * For i = 31:
    * bit = (n >> 31) & 1 = 0
    * result |= (0 << 0) = 0
  * Return result = 00111001011110000010100101000000 (decimal 964176192)

---

## **Why are we using this algorithm?**

* The brute force approach is already quite efficient, but there are more elegant ways to reverse bits.
* We can use a divide-and-conquer approach to swap bits in groups, which can be more efficient for larger bit lengths.
* However, for 32 bits, the performance difference is negligible.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We'll use a divide-and-conquer approach to swap bits in groups.
* First, we swap odd and even bits.
* Then, we swap pairs of 2 bits.
* Then, we swap groups of 4 bits.
* Then, we swap groups of 8 bits.
* Finally, we swap groups of 16 bits.
* This approach is more elegant and can be more efficient for larger bit lengths.

### **Step 2: Flow Steps (Optimized)**

1. Swap odd and even bits:
   1. Extract all odd bits and shift them right by 1.
   2. Extract all even bits and shift them left by 1.
   3. Combine the results.
2. Swap pairs of 2 bits:
   1. Extract all pairs at positions 0, 1, 4, 5, ... and shift them right by 2.
   2. Extract all pairs at positions 2, 3, 6, 7, ... and shift them left by 2.
   3. Combine the results.
3. Swap groups of 4 bits:
   1. Extract all groups at positions 0, 1, 2, 3, 8, 9, ... and shift them right by 4.
   2. Extract all groups at positions 4, 5, 6, 7, 12, 13, ... and shift them left by 4.
   3. Combine the results.
4. Swap groups of 8 bits:
   1. Extract all groups at positions 0, 1, ..., 7, 16, 17, ... and shift them right by 8.
   2. Extract all groups at positions 8, 9, ..., 15, 24, 25, ... and shift them left by 8.
   3. Combine the results.
5. Swap groups of 16 bits:
   1. Extract all bits at positions 0, 1, ..., 15 and shift them right by 16.
   2. Extract all bits at positions 16, 17, ..., 31 and shift them left by 16.
   3. Combine the results.
6. Return the result.

### **Step 3: Implementation (Optimized)**

```python
def reverseBits(n):
    # Swap odd and even bits
    n = ((n & 0x55555555) << 1) | ((n & 0xAAAAAAAA) >> 1)
    # Swap pairs of 2 bits
    n = ((n & 0x33333333) << 2) | ((n & 0xCCCCCCCC) >> 2)
    # Swap groups of 4 bits
    n = ((n & 0x0F0F0F0F) << 4) | ((n & 0xF0F0F0F0) >> 4)
    # Swap groups of 8 bits
    n = ((n & 0x00FF00FF) << 8) | ((n & 0xFF00FF00) >> 8)
    # Swap groups of 16 bits
    n = ((n & 0x0000FFFF) << 16) | ((n & 0xFFFF0000) >> 16)
    
    return n
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(1) as we always perform a fixed number of operations.
* Space Complexity: O(1) as we only use a constant amount of extra space.
* This approach is more elegant and can be more efficient for larger bit lengths, but for 32 bits, the performance difference is negligible.

### **Step 5: Visualizing Computation (Optimized)**

* For n = 00000010100101000001111010011100 (decimal 43261596):
  * Swap odd and even bits:
    * n & 0x55555555 = 00000000000001000000010000001000
    * (n & 0x55555555) << 1 = 00000000000010000000100000010000
    * n & 0xAAAAAAAA = 00000010100100000001101010010100
    * (n & 0xAAAAAAAA) >> 1 = 00000001010010000000110101001010
    * Result = 00000001010010000000110101001010 | 00000000000010000000100000010000 = 00000001010110000001010101011010
  * ... (continue for all swaps)
  * Final result = 00111001011110000010100101000000 (decimal 964176192)

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(1) | O(1) | Iterates through each bit |
| Optimized (Divide and Conquer) | O(1) | O(1) | Swaps bits in groups |

---

## **Final Takeaways**

* The Reverse Bits problem is a classic example of bit manipulation.
* Both approaches (iterating through each bit and divide-and-conquer) have the same time and space complexity, but the latter is more elegant and can be more efficient for larger bit lengths.
* This problem demonstrates how understanding bit manipulation can lead to elegant and efficient solutions.

---

## **Real-life Analogy**

* Imagine you have a row of 32 light switches, and you want to reverse their positions. The brute force approach would be to go through each switch one by one and move it to its new position. The optimized approach would be to swap groups of switches at once, which can be more efficient if you have many switches.

---

## **Summary**

* We tackled the "Reverse Bits" problem by using both a brute force approach of iterating through each bit and a more elegant divide-and-conquer approach of swapping bits in groups. Both approaches have a time complexity of O(1) as we always process 32 bits, and a space complexity of O(1) as we only use a constant amount of extra space. The divide-and-conquer approach is more elegant and can be more efficient for larger bit lengths, but for 32 bits, the performance difference is negligible. 