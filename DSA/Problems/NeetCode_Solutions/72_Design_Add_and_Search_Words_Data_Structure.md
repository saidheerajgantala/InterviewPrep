# 📝 Design Add and Search Words Data Structure

## **Problem Statement**

* Design a data structure that supports adding new words and finding if a string matches any previously added string.
* Implement the `WordDictionary` class:
  * `WordDictionary()` Initializes the object.
  * `void addWord(word)` Adds `word` to the data structure, it can be matched later.
  * `bool search(word)` Returns `true` if there is any string in the data structure that matches `word`, or `false` otherwise. `word` may contain dots `'.'` where a dot can be matched with any letter.
* Example:
  * Input:
    ```
    ["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
    [[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
    ```
  * Output:
    ```
    [null,null,null,null,false,true,true,true]
    ```
  * Explanation:
    ```
    WordDictionary wordDictionary = new WordDictionary();
    wordDictionary.addWord("bad");
    wordDictionary.addWord("dad");
    wordDictionary.addWord("mad");
    wordDictionary.search("pad"); // return False
    wordDictionary.search("bad"); // return True
    wordDictionary.search(".ad"); // return True
    wordDictionary.search("b.."); // return True
    ```
* Constraints:
  * `1 <= word.length <= 25`
  * `word` in `addWord` consists of lowercase English letters.
  * `word` in `search` consists of lowercase English letters or dots `'.'`.
  * There will be at most `10^4` calls in total to `addWord` and `search`.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to design a data structure that can efficiently add words and search for words with wildcards
* One approach would be to store all words in a list and then check each word during search
* For the search operation, we would need to handle the wildcard character '.' by comparing it with any character
* This approach is straightforward but might be inefficient for large numbers of words

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty list to store words
2. For `addWord(word)`:
   1. Simply append the word to the list
3. For `search(word)`:
   1. Iterate through each stored word
   2. For each stored word, check if it matches the search word
   3. When comparing characters, if the search word has a '.' at a position, it matches any character
   4. If a match is found, return `true`
   5. If no match is found after checking all words, return `false`

---

## **Step 3: Brute Force Implementation (Code)**

```python
class WordDictionary:
    def __init__(self):
        self.words = []
    
    def addWord(self, word: str) -> None:
        self.words.append(word)
    
    def search(self, word: str) -> bool:
        def is_match(stored_word, search_word):
            if len(stored_word) != len(search_word):
                return False
            
            for i in range(len(search_word)):
                if search_word[i] != '.' and search_word[i] != stored_word[i]:
                    return False
            
            return True
        
        for stored_word in self.words:
            if is_match(stored_word, word):
                return True
        
        return False
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity:
  * `addWord(word)`: O(1) - simply appending to a list
  * `search(word)`: O(N * L) where N is the number of words stored and L is the average length of the words. We need to check each word and compare each character.
* Space Complexity: O(N * L) where N is the number of words and L is the average length of the words.
* This approach is inefficient for large numbers of words, especially for search operations.

---

## **Step 5: Visualizing Redundant Computation**

* For the example:
  * `addWord("bad")` - Store "bad" in the list
  * `addWord("dad")` - Store "dad" in the list
  * `addWord("mad")` - Store "mad" in the list
  * `search("pad")`:
    * Check "bad" - Not a match
    * Check "dad" - Not a match
    * Check "mad" - Not a match
    * Return false
  * `search("bad")`:
    * Check "bad" - Match found
    * Return true
  * `search(".ad")`:
    * Check "bad" - Match found (. matches 'b')
    * Return true
  * `search("b..")`:
    * Check "bad" - Match found (. matches 'a' and 'd')
    * Return true
* The brute force approach repeatedly checks each word character by character, which is inefficient

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Trie (Prefix Tree) data structure.
* The property that matches this problem is that a Trie can efficiently store and search for words, especially with wildcard characters.
* By using a Trie, we can avoid checking words that don't share the same prefix, making the search operation more efficient.
* Alternatives like the brute force approach would be less efficient for large numbers of words.
* Precondition: We need to be able to handle wildcard characters in the search operation.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Trie)**

* We can use a Trie (Prefix Tree) data structure to efficiently store and search for words
* Each node in the Trie represents a character in a word
* Words with the same prefix share the same path in the Trie
* For the search operation with wildcards, we can use a recursive approach to explore all possible paths when encountering a '.'

### **Step 2: Flow Steps (Optimized - Trie)**

1. Initialize a Trie with a root node
2. For `addWord(word)`:
   1. Start from the root node
   2. For each character in the word, create a new node if it doesn't exist
   3. Mark the last node as the end of a word
3. For `search(word)`:
   1. Define a recursive function to search from a given node and position in the word
   2. If the position is equal to the length of the word, return whether the current node is the end of a word
   3. If the current character is '.', recursively search all possible paths
   4. Otherwise, check if the current character exists in the Trie and recursively search that path
   5. Start the search from the root node and position 0

### **Step 3: Implementation (Optimized - Trie)**

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class WordDictionary:
    def __init__(self):
        self.root = TrieNode()
    
    def addWord(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
    
    def search(self, word: str) -> bool:
        def dfs(node, index):
            if index == len(word):
                return node.is_end_of_word
            
            char = word[index]
            if char == '.':
                for child in node.children.values():
                    if dfs(child, index + 1):
                        return True
                return False
            else:
                if char not in node.children:
                    return False
                return dfs(node.children[char], index + 1)
        
        return dfs(self.root, 0)
```

### **Step 4: Complexity Analysis (Optimized - Trie)**

* Time Complexity:
  * `addWord(word)`: O(L) where L is the length of the word. We need to traverse or create a node for each character.
  * `search(word)`: 
    * Best case: O(L) when there are no wildcards
    * Worst case: O(N * L) when all characters are wildcards, where N is the number of words stored and L is the length of the word. This is because we might need to explore all paths in the Trie.
* Space Complexity: O(N * L) where N is the number of words and L is the average length of the words. This is for storing the Trie.
* This approach is much more efficient than the brute force approach, especially for search operations with few or no wildcards.

### **Step 5: Visualizing Computation (Optimized - Trie)**

* For the example:
  * `addWord("bad")`:
    * Create nodes for 'b', 'a', 'd'
    * Mark 'd' as the end of a word
  * `addWord("dad")`:
    * Create node for 'd'
    * Reuse node for 'a'
    * Reuse node for 'd'
    * Mark 'd' as the end of a word
  * `addWord("mad")`:
    * Create node for 'm'
    * Reuse node for 'a'
    * Reuse node for 'd'
    * Mark 'd' as the end of a word
  * `search("pad")`:
    * Start from the root
    * No child node for 'p'
    * Return false
  * `search("bad")`:
    * Start from the root
    * Follow path for 'b', 'a', 'd'
    * 'd' is marked as the end of a word
    * Return true
  * `search(".ad")`:
    * Start from the root
    * For '.', explore all child nodes ('b', 'd', 'm')
    * For 'b', follow path for 'a', 'd'
    * 'd' is marked as the end of a word
    * Return true
  * `search("b..")`:
    * Start from the root
    * Follow path for 'b'
    * For '.', explore all child nodes ('a')
    * For 'a', for '.', explore all child nodes ('d')
    * 'd' is marked as the end of a word
    * Return true
* The Trie approach efficiently prunes the search space by only exploring relevant paths

### **Complexity Summary**

| Approach | Time (addWord) | Time (search) | Space | Notes |
|---|---|---|---|---|
| Brute Force (List) | O(1) | O(N * L) | O(N * L) | Simple but inefficient for search |
| Optimized (Trie) | O(L) | O(L) to O(N * L) | O(N * L) | Efficient for search with few wildcards |

---

## **Final Takeaways**

* The key to efficiently implementing a word dictionary with wildcard search is to use a Trie data structure.
* A Trie allows us to efficiently store words with common prefixes and perform searches, especially when there are few or no wildcards.
* For wildcard searches, we can use a recursive approach to explore all possible paths in the Trie.
* This problem demonstrates how a specialized data structure can be much more efficient than a brute force approach for specific operations.

---

## **Real-life Analogy**

* Imagine you're looking for a specific book in a library. The brute force approach would be like checking every book one by one, which is inefficient. The Trie approach is like using the library's categorization system - first, you go to the right section (first letter), then the right shelf (second letter), and so on. If you're not sure about a letter (wildcard), you might need to check multiple sections, but you still don't need to check every book in the library.

---

## **Summary**

* We designed a data structure for adding words and searching with wildcards by first considering a brute force approach using a list, resulting in O(N * L) time complexity for search operations. We then optimized using a Trie data structure, which reduces the search time to O(L) for searches without wildcards and makes the search operation more efficient by pruning the search space. The key insight is to use a recursive approach to handle wildcard characters by exploring all possible paths in the Trie. 