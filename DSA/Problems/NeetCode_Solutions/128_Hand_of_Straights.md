# 📝 Hand of Straights

## **Problem Statement**

* Alice has some cards, each card has an integer written on it.
* She'd like to rearrange the cards into groups so that each group is of size `groupSize`, and consists of consecutive cards.
* Given an integer array `hand` where `hand[i]` is the value written on the ith card and an integer `groupSize`, return `true` if she can rearrange the cards, or `false` otherwise.
* Example:
  * Input: hand = [1,2,3,6,2,3,4,7,8], groupSize = 3
  * Output: true (Alice can arrange the cards as [1,2,3], [2,3,4], [6,7,8])
  * Input: hand = [1,2,3,4,5], groupSize = 4
  * Output: false (Alice cannot form groups of size 4)
* Constraints:
  * 1 <= hand.length <= 10^4
  * 0 <= hand[i] <= 10^9
  * 1 <= groupSize <= hand.length

---

## **Step 1: Thinking Process (Brute Force)**

* We need to group the cards into consecutive sequences of length `groupSize`
* First, we need to check if the total number of cards is divisible by `groupSize`
* Then, we can try to form groups by repeatedly finding the smallest available card and taking the next `groupSize-1` consecutive cards
* If at any point we can't form a complete group, we return false

---

## **Step 2: Flow Steps (Brute Force)**

1. Check if hand.length % groupSize != 0, if true, return false
2. Count the frequency of each card using a hash map
3. While there are cards left:
   1. Find the smallest card value that still has remaining cards
   2. Try to form a group starting from this card:
      1. For i from smallest to smallest+groupSize-1:
         1. If card i is not available or count[i] == 0, return false
         2. Decrement count[i]
4. Return true if all cards have been used

---

## **Step 3: Brute Force Implementation (Code)**

```python
def isNStraightHand(hand, groupSize):
    if len(hand) % groupSize != 0:
        return False
    
    # Count the frequency of each card
    count = {}
    for card in hand:
        count[card] = count.get(card, 0) + 1
    
    # Sort the unique card values
    unique_cards = sorted(count.keys())
    
    # Try to form groups
    while unique_cards:
        # Get the smallest card
        smallest = unique_cards[0]
        
        # Try to form a group starting from the smallest card
        for i in range(smallest, smallest + groupSize):
            if i not in count or count[i] == 0:
                return False
            
            count[i] -= 1
            if count[i] == 0:
                if i == unique_cards[0]:
                    unique_cards.pop(0)
                else:
                    return False  # Cards must be used in order
    
    return True
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n log n) where n is the number of cards. Sorting the unique cards takes O(k log k) time where k is the number of unique cards, and forming groups takes O(n) time.
* Space Complexity: O(k) where k is the number of unique cards, for storing the frequency count.
* This approach is reasonably efficient but can be optimized further.

---

## **Step 5: Visualizing Redundant Computation**

* For hand = [1,2,3,6,2,3,4,7,8], groupSize = 3:
  * count = {1:1, 2:2, 3:2, 4:1, 6:1, 7:1, 8:1}
  * unique_cards = [1, 2, 3, 4, 6, 7, 8]
  * Form group starting with 1: [1,2,3]
    * Update count = {1:0, 2:1, 3:1, 4:1, 6:1, 7:1, 8:1}
    * Update unique_cards = [2, 3, 4, 6, 7, 8]
  * Form group starting with 2: [2,3,4]
    * Update count = {1:0, 2:0, 3:0, 4:0, 6:1, 7:1, 8:1}
    * Update unique_cards = [6, 7, 8]
  * Form group starting with 6: [6,7,8]
    * Update count = {1:0, 2:0, 3:0, 4:0, 6:0, 7:0, 8:0}
    * Update unique_cards = []
  * All cards used, return true

---

## **Why are we using this algorithm?**

* For optimization, we'll use a more efficient approach using a min-heap and a hash map.
* The key insight is to always start a group with the smallest available card.
* Using a min-heap allows us to efficiently find the smallest card at each step.
* This approach ensures we're always forming groups in the optimal way.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* We'll use a min-heap to efficiently find the smallest card at each step
* We'll also use a hash map to keep track of the frequency of each card
* We'll repeatedly pop the smallest card from the heap and try to form a group
* If at any point we can't form a complete group, we return false

### **Step 2: Flow Steps (Optimized)**

1. Check if hand.length % groupSize != 0, if true, return false
2. Count the frequency of each card using a hash map
3. Create a min-heap of the unique card values
4. While the heap is not empty:
   1. Pop the smallest card value from the heap
   2. If its frequency is 0, continue (it's already been used)
   3. Try to form a group starting from this card:
      1. For i from smallest to smallest+groupSize-1:
         1. If card i is not in the frequency map or its frequency is 0, return false
         2. Decrement the frequency of card i
         3. If the frequency becomes 0 and i+1 is in the heap, remove i+1 from the heap
5. Return true if all cards have been used

### **Step 3: Implementation (Optimized)**

```python
import heapq

def isNStraightHand(hand, groupSize):
    if len(hand) % groupSize != 0:
        return False
    
    # Count the frequency of each card
    count = {}
    for card in hand:
        count[card] = count.get(card, 0) + 1
    
    # Create a min-heap of unique card values
    min_heap = list(count.keys())
    heapq.heapify(min_heap)
    
    # Try to form groups
    while min_heap:
        # Get the smallest card
        smallest = min_heap[0]
        
        # Try to form a group starting from the smallest card
        for i in range(smallest, smallest + groupSize):
            if i not in count or count[i] == 0:
                return False
            
            count[i] -= 1
            
            # If we've used all instances of this card, remove it from consideration
            if count[i] == 0:
                # If we're trying to remove a card that's not the smallest, it means
                # we've broken the consecutive property
                if i != min_heap[0]:
                    return False
                heapq.heappop(min_heap)
    
    return True
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n log k) where n is the number of cards and k is the number of unique cards. Building the heap takes O(k) time, and each of the n cards requires O(log k) time for heap operations.
* Space Complexity: O(k) where k is the number of unique cards, for storing the frequency count and the heap.
* This approach is more efficient than the brute force approach, especially when there are many cards with the same value.

### **Step 5: Visualizing Computation (Optimized)**

* For hand = [1,2,3,6,2,3,4,7,8], groupSize = 3:
  * count = {1:1, 2:2, 3:2, 4:1, 6:1, 7:1, 8:1}
  * min_heap = [1, 2, 3, 4, 6, 7, 8]
  * Form group starting with 1: [1,2,3]
    * Update count = {1:0, 2:1, 3:1, 4:1, 6:1, 7:1, 8:1}
    * Update min_heap = [2, 3, 4, 6, 7, 8]
  * Form group starting with 2: [2,3,4]
    * Update count = {1:0, 2:0, 3:0, 4:0, 6:1, 7:1, 8:1}
    * Update min_heap = [6, 7, 8]
  * Form group starting with 6: [6,7,8]
    * Update count = {1:0, 2:0, 3:0, 4:0, 6:0, 7:0, 8:0}
    * Update min_heap = []
  * All cards used, return true

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n log n) | O(k) | Sorts the unique cards and forms groups |
| Optimized (Algorithm: Min-Heap + Hash Map) | O(n log k) | O(k) | Uses a min-heap to efficiently find the smallest card |

---

## **Final Takeaways**

* The Hand of Straights problem requires forming consecutive groups of cards.
* The key insight is to always start a group with the smallest available card.
* Using a min-heap allows us to efficiently find the smallest card at each step.
* This problem demonstrates how data structures like heaps can be used to optimize algorithms.

---

## **Real-life Analogy**

* Imagine sorting a deck of playing cards into consecutive runs (like in the game of Rummy). You'd naturally start with the lowest card you have and try to form runs from there. If you can't form complete runs with all your cards, then it's not possible to arrange them as required.

---

## **Summary**

* We tackled the "Hand of Straights" problem by first checking if the total number of cards is divisible by the group size. We then used a min-heap to efficiently find the smallest card at each step and tried to form consecutive groups starting from that card. If at any point we couldn't form a complete group, we returned false. This approach has a time complexity of O(n log k) where n is the number of cards and k is the number of unique cards. 