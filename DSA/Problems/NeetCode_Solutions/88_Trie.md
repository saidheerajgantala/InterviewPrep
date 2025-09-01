# 📝 Trie

## **Concept Overview**

* A trie (pronounced as "try") or prefix tree is a tree-based data structure used to efficiently store and retrieve keys in a dataset of strings.
* It is particularly useful for applications like autocomplete, spell checking, and IP routing.
* The main advantages of a trie are:
  * Fast lookups: O(m) time complexity for lookups, where m is the length of the key.
  * Space efficiency: Common prefixes are stored only once.
  * Ordered traversal: Keys are automatically stored in alphabetical order.

---

## **Structure and Properties**

* A trie is a tree where each node represents a single character of a string.
* The root node is typically an empty node.
* Each node can have multiple children, one for each possible character.
* Nodes can be marked as "end of word" to indicate that the path from the root to that node forms a complete word.
* The path from the root to any node represents a prefix of one or more words in the trie.

---

## **Key Operations and Complexity**

* **Insert**: Add a word to the trie
  * Time Complexity: O(m) where m is the length of the word
  * Process: Start from the root, follow or create nodes for each character, mark the last node as the end of a word
* **Search**: Check if a word exists in the trie
  * Time Complexity: O(m) where m is the length of the word
  * Process: Start from the root, follow nodes for each character, check if the last node is marked as the end of a word
* **StartsWith**: Check if there is any word in the trie that starts with a given prefix
  * Time Complexity: O(m) where m is the length of the prefix
  * Process: Start from the root, follow nodes for each character, return true if all characters are found
* **Delete**: Remove a word from the trie
  * Time Complexity: O(m) where m is the length of the word
  * Process: Start from the root, follow nodes for each character, unmark the last node as the end of a word, and remove nodes that are no longer needed

---

## **Implementation in Python**

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
    
    def starts_with(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
    
    def delete(self, word):
        def _delete_helper(node, word, index):
            if index == len(word):
                # If the word exists in the trie, unmark it
                if node.is_end_of_word:
                    node.is_end_of_word = False
                # Return True if the node has no children and is not the end of another word
                return not node.children and not node.is_end_of_word
            
            char = word[index]
            if char not in node.children:
                return False
            
            # Recursively delete the rest of the word
            should_delete_child = _delete_helper(node.children[char], word, index + 1)
            
            # If the child should be deleted, remove it from the children dictionary
            if should_delete_child:
                del node.children[char]
            
            # Return True if this node should also be deleted
            return not node.children and not node.is_end_of_word
        
        _delete_helper(self.root, word, 0)
```

---

## **Common Applications**

1. **Autocomplete**: Suggesting words as a user types.
2. **Spell Checking**: Checking if a word is spelled correctly.
3. **IP Routing**: Efficiently routing IP addresses.
4. **Word Games**: Finding valid words in games like Boggle or Scrabble.
5. **Prefix Matching**: Finding all words with a common prefix.
6. **Dictionary Implementation**: Efficiently storing and retrieving words.
7. **Search Engines**: Implementing features like search suggestions.

---

## **Trie vs. Other Data Structures**

| Operation | Trie | Hash Table | Binary Search Tree |
|---|---|---|---|
| Search | O(m) | O(m) | O(m log n) |
| Insert | O(m) | O(m) | O(m log n) |
| Delete | O(m) | O(m) | O(m log n) |
| Prefix Search | O(m + k) | O(n * m) | O(m log n + k) |
| Space | O(n * m) | O(n * m) | O(n * m) |

Where:
* n is the number of keys
* m is the length of the key
* k is the number of matches

---

## **Trie Variants**

1. **Compressed Trie (Radix Tree)**: Merges nodes with only one child to save space.
2. **Suffix Trie**: Stores all suffixes of a given string, useful for pattern matching.
3. **Ternary Search Trie**: Each node has three children (left, middle, right) instead of one child per character.
4. **PATRICIA Trie (Practical Algorithm to Retrieve Information Coded in Alphanumeric)**: A space-optimized trie where nodes with only one child are merged with their parents.

---

## **Real-life Analogy**

* Imagine a library with books organized by their titles. The books are arranged in a hierarchical structure where books with the same first letter are grouped together, then further grouped by the second letter, and so on. This makes it much faster to find a specific book or to check if any book starts with a certain prefix. This is similar to how a trie organizes strings.

---

## **Implementation Considerations**

* **Memory Usage**: Tries can use a lot of memory, especially for large datasets. Consider using compressed tries or other space-optimized variants.
* **Character Set**: The size of the character set affects the branching factor of the trie. A larger character set (e.g., Unicode) will require more memory.
* **Deletion**: Deletion is more complex than insertion or search, as it may require removing nodes that are no longer needed.
* **Balancing**: Unlike binary search trees, tries do not need to be balanced.

---

## **Summary**

* A trie is a tree-based data structure that efficiently stores and retrieves strings.
* It provides O(m) time complexity for insert, search, and prefix search operations, where m is the length of the string.
* Tries are particularly useful for applications like autocomplete, spell checking, and IP routing.
* The main advantages of tries are fast lookups, space efficiency for common prefixes, and ordered traversal.
* Various optimizations and variants exist to improve the space efficiency and performance of tries for specific use cases. 