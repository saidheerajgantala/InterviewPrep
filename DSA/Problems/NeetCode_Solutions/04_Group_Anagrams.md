# 📝 Group Anagrams

## **Problem Statement**

* Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.
* An anagram is a word formed by rearranging the letters of another word, using all the original letters exactly once.
* Example:
  * Input: strs = ["eat","tea","tan","ate","nat","bat"]
  * Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
* Constraints:
  * 1 <= strs.length <= 10^4
  * 0 <= strs[i].length <= 100
  * strs[i] consists of lowercase English letters

---

## **Step 1: Thinking Process (Brute Force)**

* We need to group strings that are anagrams of each other
* For each string, we need to check if it's an anagram of any existing group
* If it is, we add it to that group
* If not, we create a new group with this string
* To check if two strings are anagrams, we can sort them and compare

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty list of groups
2. For each string in the input array:
   1. Found = false
   2. For each existing group:
      1. Take the first string in the group
      2. Check if the current string is an anagram of this representative (by sorting both and comparing)
      3. If they are anagrams, add the current string to this group and set found = true
   3. If not found, create a new group with the current string
3. Return the list of groups

---

## **Step 3: Brute Force Implementation (Code)**

```python
def groupAnagrams(strs):
    if not strs:
        return []
    
    groups = []
    
    for s in strs:
        found = False
        for group in groups:
            # Check if s is an anagram of the first string in the group
            if sorted(s) == sorted(group[0]):
                group.append(s)
                found = True
                break
        
        if not found:
            groups.append([s])
    
    return groups
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(n^2 * k log k), where n is the number of strings and k is the maximum length of a string.
  * For each string (O(n)), we might need to check all existing groups (O(n))
  * Each check involves sorting both strings (O(k log k))
* Space Complexity: O(n * k) to store all the strings in their groups.
* This is inefficient because we're repeatedly sorting strings and comparing them.

---

## **Step 5: Visualizing Redundant Computation**

* For input ["eat","tea","tan","ate","nat","bat"]:
  * Process "eat": Create new group ["eat"]
  * Process "tea": Sort "tea" -> "aet", sort "eat" -> "aet", match! Add to group -> ["eat", "tea"]
  * Process "tan": Sort "tan" -> "ant", sort "eat" -> "aet", no match. Create new group -> ["tan"]
  * Process "ate": Sort "ate" -> "aet", sort "eat" -> "aet", match! Add to group -> ["eat", "tea", "ate"]
  * Process "nat": Sort "nat" -> "ant", sort "eat" -> "aet", no match. Sort "tan" -> "ant", match! Add to group -> ["tan", "nat"]
  * Process "bat": No matches, create new group -> ["bat"]
* We're repeatedly sorting the same strings multiple times.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Hash Table with Sorted String Keys.
* The property that matches this problem is the ability to group items by a common characteristic.
* A hash table allows us to group anagrams efficiently by using the sorted string as a key.
* Alternatives like comparing each string with every other would be O(n²), which is less efficient.
* Precondition: Hash function distributes elements uniformly (generally true for strings).

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of repeatedly sorting and comparing strings, we can use a hash map
* The key will be the sorted version of each string (all anagrams will have the same sorted string)
* The value will be a list of all strings that, when sorted, match the key
* This way, we only need to sort each string once

### **Step 2: Flow Steps (Optimized)**

1. Create an empty hash map where keys are sorted strings and values are lists of original strings
2. For each string in the input array:
   1. Sort the string to get the key
   2. If the key exists in the hash map, append the original string to its list
   3. If the key doesn't exist, create a new entry with the key and a list containing the original string
3. Return the values of the hash map (the lists of grouped anagrams)

### **Step 3: Implementation (Optimized)**

```python
def groupAnagrams(strs):
    anagram_map = {}
    
    for s in strs:
        # Sort the string to use as a key
        sorted_s = ''.join(sorted(s))
        
        # If the sorted string is already a key, append to its list
        # Otherwise, create a new list
        if sorted_s in anagram_map:
            anagram_map[sorted_s].append(s)
        else:
            anagram_map[sorted_s] = [s]
    
    # Return all the groups
    return list(anagram_map.values())
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: O(n * k log k), where n is the number of strings and k is the maximum length of a string.
  * We sort each string once (O(k log k)) and do this for all n strings.
* Space Complexity: O(n * k) to store all the strings in the hash map.
* This is much more efficient than the brute force approach, especially for large inputs.

### **Step 5: Visualizing Computation (Optimized)**

* For input ["eat","tea","tan","ate","nat","bat"]:
  * Process "eat": Sort to "aet", create entry: {"aet": ["eat"]}
  * Process "tea": Sort to "aet", append: {"aet": ["eat", "tea"]}
  * Process "tan": Sort to "ant", create entry: {"aet": ["eat", "tea"], "ant": ["tan"]}
  * Process "ate": Sort to "aet", append: {"aet": ["eat", "tea", "ate"], "ant": ["tan"]}
  * Process "nat": Sort to "ant", append: {"aet": ["eat", "tea", "ate"], "ant": ["tan", "nat"]}
  * Process "bat": Sort to "abt", create entry: {"aet": ["eat", "tea", "ate"], "ant": ["tan", "nat"], "abt": ["bat"]}
  * Return [["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force | O(n² * k log k) | O(n * k) | Repeatedly sorts and compares strings |
| Optimized (Algorithm: Hash Table) | O(n * k log k) | O(n * k) | Uses sorted strings as keys in a hash map |

---

## **Final Takeaways**

* Using a hash map with a carefully chosen key can dramatically reduce time complexity.
* The key insight is identifying a canonical form (sorted string) that's the same for all anagrams.
* This pattern of using a transformed version of the input as a hash key is common in grouping problems.

---

## **Real-life Analogy**

* Imagine sorting books in a library. The brute force approach would be like comparing each new book with representatives from every existing shelf to see if they belong together. The optimized approach is like assigning each book a unique code based on its content (like sorting the letters), and then simply putting books with the same code on the same shelf - much more efficient!

---

## **Summary**

* We solved the "Group Anagrams" problem by first using a brute force approach that compared each string with representatives from existing groups, resulting in O(n² * k log k) time complexity. We then optimized using a hash map with sorted strings as keys, reducing the time complexity to O(n * k log k). This demonstrates the power of using hash tables with carefully chosen keys to group related items efficiently. 