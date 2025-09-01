# 📝 Implement Trie (Prefix Tree)

## **Problem Statement**

* A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.
* Implement the Trie class:
  * `Trie()` Initializes the trie object.
  * `void insert(String word)` Inserts the string `word` into the trie.
  * `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
  * `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.
* Example:
  * Input:
    ```
    ["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
    [[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
    ```
  * Output:
    ```
    [null, null, true, false, true, null, true]
    ```
  * Explanation:
    ```
    Trie trie = new Trie();
    trie.insert("apple");
    trie.search("apple");   // return True
    trie.search("app");     // return False
    trie.startsWith("app"); // return True
    trie.insert("app");
    trie.search("app");     // return True
    ```
* Constraints:
  * 1 <= word.length, prefix.length <= 2000
  * word and prefix consist only of lowercase English letters.
  * At most 3 * 10^4 calls in total will be made to insert, search, and startsWith.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to implement a trie data structure with three operations: insert, search, and startsWith
* One approach would be to use a simple list to store all the words and perform linear search for search and startsWith operations
* This approach is straightforward but inefficient for large datasets

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty list to store words
2. For `insert(word)`:
   1. Add the word to the list if it's not already present
3. For `search(word)`:
   1. Check if the word is in the list
4. For `startsWith(prefix)`:
   1. Check if any word in the list starts with the given prefix

---

## **Step 3: Brute Force Implementation (Code)**

```python
class Trie:
    def __init__(self):
        self.words = []
    
    def insert(self, word):
        if word not in self.words:
            self.words.append(word)
    
    def search(self, word):
        return word in self.words
    
    def startsWith(self, prefix):
        for word in self.words:
            if word.startswith(prefix):
                return True
        return False
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity:
  * `insert`: O(n) where n is the length of the list, as we need to check if the word is already in the list
  * `search`: O(n) for linear search in the list
  * `startsWith`: O(n * m) where n is the length of the list and m is the average length of words, as we need to check each word for the prefix
* Space Complexity: O(n * m) where n is the number of words and m is the average length of words
* This approach is inefficient for large datasets, especially for the `startsWith` operation

---

## **Step 5: Visualizing Redundant Computation**

* For the example:
  * `insert("apple")`: Add "apple" to the list, list = ["apple"]
  * `search("apple")`: Check if "apple" is in the list, return true
  * `search("app")`: Check if "app" is in the list, return false
  * `startsWith("app")`: Check if any word in the list starts with "app", find "apple", return true
  * `insert("app")`: Add "app" to the list, list = ["apple", "app"]
  * `search("app")`: Check if "app" is in the list, return true
* The redundant computation is in checking each word for the prefix in the `startsWith` operation, especially when there are many words with common prefixes

---

## **Why are we using this algorithm?**

* For optimization, we'll use a proper trie data structure.
* The property that matches this problem is that a trie efficiently stores strings in a way that allows for quick prefix lookups.
* By using a trie, we can avoid redundant checks for common prefixes.
* Alternatives like the brute force approach would be less efficient for large datasets.
* Precondition: We need to be able to represent the trie structure with nodes and links.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Trie)**

* We'll use a proper trie data structure where each node represents a character
* Each node will have:
  * A dictionary to store links to child nodes, with characters as keys
  * A boolean flag to indicate if the node represents the end of a word
* This structure allows for efficient prefix lookups and word searches

### **Step 2: Flow Steps (Optimized - Trie)**

1. Initialize a trie with a root node
2. For `insert(word)`:
   1. Start from the root node
   2. For each character in the word:
      1. If the character is not in the current node's children, create a new node
      2. Move to the child node
   3. Mark the last node as the end of a word
3. For `search(word)`:
   1. Start from the root node
   2. For each character in the word:
      1. If the character is not in the current node's children, return false
      2. Move to the child node
   3. Return whether the last node is marked as the end of a word
4. For `startsWith(prefix)`:
   1. Start from the root node
   2. For each character in the prefix:
      1. If the character is not in the current node's children, return false
      2. Move to the child node
   3. Return true (since we found all characters of the prefix)

### **Step 3: Implementation (Optimized - Trie)**

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
    
    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word
    
    def startsWith(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```

### **Step 4: Complexity Analysis (Optimized - Trie)**

* Time Complexity:
  * `insert`: O(m) where m is the length of the word
  * `search`: O(m) where m is the length of the word
  * `startsWith`: O(m) where m is the length of the prefix
* Space Complexity: O(n * m) where n is the number of words and m is the average length of words. In the worst case, if there are no common prefixes, the trie will have n * m nodes. However, in practice, the space complexity is often much better due to common prefixes.
* This approach is much more efficient than the brute force approach, especially for large datasets with common prefixes.

### **Step 5: Visualizing Computation (Optimized - Trie)**

* For the example:
  * `insert("apple")`:
    * Create nodes for 'a', 'p', 'p', 'l', 'e'
    * Mark the last node ('e') as the end of a word
  * `search("apple")`:
    * Follow the path 'a' -> 'p' -> 'p' -> 'l' -> 'e'
    * Check if the last node is marked as the end of a word, return true
  * `search("app")`:
    * Follow the path 'a' -> 'p' -> 'p'
    * Check if the last node is marked as the end of a word, return false
  * `startsWith("app")`:
    * Follow the path 'a' -> 'p' -> 'p'
    * Return true since we found all characters of the prefix
  * `insert("app")`:
    * Follow the path 'a' -> 'p' -> 'p'
    * Mark the last node ('p') as the end of a word
  * `search("app")`:
    * Follow the path 'a' -> 'p' -> 'p'
    * Check if the last node is marked as the end of a word, return true
* The trie approach efficiently handles common prefixes and avoids redundant checks

### **Complexity Summary**

| Approach | Time (insert) | Time (search) | Time (startsWith) | Space | Notes |
|---|---|---|---|---|---|
| Brute Force (List) | O(n) | O(n) | O(n * m) | O(n * m) | Inefficient for large datasets |
| Optimized (Trie) | O(m) | O(m) | O(m) | O(n * m) | Efficient for prefix operations |

---

## **Final Takeaways**

* The key to efficiently implementing a trie is to use a tree-like structure where each node represents a character and has links to child nodes.
* A trie provides efficient operations for inserting words, searching for words, and checking if a prefix exists.
* This data structure is particularly useful for applications like autocomplete, spell checking, and IP routing.

---

## **Real-life Analogy**

* Imagine a library with books organized by their titles. The brute force approach would be like scanning through all the books one by one to find a specific title or to check if any book starts with a certain prefix. The trie approach would be like having the books organized in a hierarchical structure where books with the same first letter are grouped together, then further grouped by the second letter, and so on. This makes it much faster to find a specific book or to check if any book starts with a certain prefix.

---

## **Summary**

* We implemented a trie data structure by first considering a brute force approach using a list, resulting in inefficient operations for large datasets. We then optimized using a proper trie structure with nodes and links, reducing the time complexity for all operations to O(m) where m is the length of the word or prefix. The key insight is to use a tree-like structure where each node represents a character and has links to child nodes, allowing for efficient prefix operations. 