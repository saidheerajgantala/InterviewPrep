# 📝 Word Search II

## **Problem Statement**

* Given an m x n board of characters and a list of strings words, return all words on the board.
* Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.
* Example 1:
  * Input: 
    * board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]]
    * words = ["oath","pea","eat","rain"]
  * Output: ["eat","oath"]
  * Explanation: The board looks like:
    ```
    o a a n
    e t a e
    i h k r
    i f l v
    ```
    * "oath" can be formed by starting at the top-left 'o', moving right to 'a', down to 't', and right to 'h'.
    * "eat" can be formed by starting at the bottom-left 'e', moving up to 'e', right to 'a', and right to 't'.
    * The words "pea" and "rain" cannot be formed on this board.
* Example 2:
  * Input: 
    * board = [["a","b"],["c","d"]]
    * words = ["abcb"]
  * Output: []
  * Explanation: The word "abcb" cannot be formed on this board.
* Constraints:
  * m == board.length
  * n == board[i].length
  * 1 <= m, n <= 12
  * board[i][j] is a lowercase English letter.
  * 1 <= words.length <= 3 * 10^4
  * 1 <= words[i].length <= 10
  * words[i] consists of lowercase English letters.
  * All the strings of words are unique.

---

## **Step 1: Thinking Process (Brute Force)**

* We need to find all words from the given list that can be formed on the board
* One approach would be to check each word by searching for it on the board
* For each word, we can start from each cell and try to form the word by exploring adjacent cells
* This approach is straightforward but might be inefficient for a large number of words

---

## **Step 2: Flow Steps (Brute Force)**

1. Initialize an empty result list to store found words
2. For each word in the words list:
   1. For each cell in the board:
      1. If the cell matches the first character of the word, start a depth-first search (DFS) from that cell
      2. In the DFS, mark the current cell as visited, then recursively check adjacent cells for the next character
      3. If we can form the entire word, add it to the result list
3. Return the result list

---

## **Step 3: Brute Force Implementation (Code)**

```python
def findWords(board, words):
    if not board or not words:
        return []
    
    m, n = len(board), len(board[0])
    result = []
    
    def dfs(i, j, word, index, visited):
        if index == len(word):
            return True
        
        if i < 0 or i >= m or j < 0 or j >= n or visited[i][j] or board[i][j] != word[index]:
            return False
        
        visited[i][j] = True
        found = (dfs(i+1, j, word, index+1, visited) or
                dfs(i-1, j, word, index+1, visited) or
                dfs(i, j+1, word, index+1, visited) or
                dfs(i, j-1, word, index+1, visited))
        visited[i][j] = False
        
        return found
    
    for word in words:
        found = False
        for i in range(m):
            for j in range(n):
                if board[i][j] == word[0]:
                    visited = [[False for _ in range(n)] for _ in range(m)]
                    if dfs(i, j, word, 0, visited):
                        result.append(word)
                        found = True
                        break
            if found:
                break
    
    return result
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: O(k * m * n * 4^L) where:
  * k is the number of words
  * m and n are the dimensions of the board
  * L is the maximum length of a word
  * For each word, we might need to check every cell on the board (m * n)
  * For each starting cell, we perform a DFS with up to 4 choices (up, down, left, right) for each of the L characters
* Space Complexity: O(m * n + L) for the visited array and the recursion stack
* This approach is very inefficient for a large number of words or a large board

---

## **Step 5: Visualizing Redundant Computation**

* For the example board and words:
  * For "oath":
    * Start at each cell and check if it's 'o'
    * From 'o', check adjacent cells for 'a'
    * From 'a', check adjacent cells for 't'
    * From 't', check adjacent cells for 'h'
  * For "pea":
    * Start at each cell and check if it's 'p'
    * No 'p' on the board, so "pea" can't be formed
  * For "eat":
    * Start at each cell and check if it's 'e'
    * From 'e', check adjacent cells for 'a'
    * From 'a', check adjacent cells for 't'
  * For "rain":
    * Start at each cell and check if it's 'r'
    * From 'r', check adjacent cells for 'a'
    * From 'a', check adjacent cells for 'i'
    * From 'i', check adjacent cells for 'n'
    * Can't form "rain" on the board
* We're repeatedly searching for common prefixes among the words, which is inefficient

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Trie (Prefix Tree) data structure.
* The property that matches this problem is that a Trie can efficiently store and search for words with common prefixes.
* By using a Trie, we can avoid redundant searches for common prefixes and prune the search space early.
* Alternatives like the brute force approach would be less efficient for a large number of words with common prefixes.
* Precondition: We need to be able to efficiently check if a prefix is part of any word in the list.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized - Trie + DFS)**

* We can use a Trie to store all the words from the given list
* Then, we can perform a DFS on the board, checking if the current path forms a prefix in the Trie
* If a prefix is not in the Trie, we can prune the search
* If we find a complete word in the Trie, we add it to the result list
* This approach avoids redundant searches for common prefixes

### **Step 2: Flow Steps (Optimized - Trie + DFS)**

1. Build a Trie from the words list
2. Initialize an empty result list to store found words
3. For each cell in the board:
   1. Start a DFS from that cell
   2. In the DFS, check if the current path forms a prefix in the Trie
   3. If it's not a prefix, backtrack
   4. If it's a complete word, add it to the result list and mark it as found in the Trie
   5. Continue the DFS to find more words
4. Return the result list

### **Step 3: Implementation (Optimized - Trie + DFS)**

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False
        self.word = None

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
        node.word = word

def findWords(board, words):
    if not board or not words:
        return []
    
    # Build the Trie
    trie = Trie()
    for word in words:
        trie.insert(word)
    
    m, n = len(board), len(board[0])
    result = []
    
    def dfs(i, j, node):
        if i < 0 or i >= m or j < 0 or j >= n or board[i][j] == '#' or board[i][j] not in node.children:
            return
        
        char = board[i][j]
        curr_node = node.children[char]
        
        # If we found a word, add it to the result
        if curr_node.is_end_of_word:
            result.append(curr_node.word)
            curr_node.is_end_of_word = False  # Mark as visited to avoid duplicates
        
        # Mark the cell as visited
        board[i][j] = '#'
        
        # Explore adjacent cells
        dfs(i+1, j, curr_node)
        dfs(i-1, j, curr_node)
        dfs(i, j+1, curr_node)
        dfs(i, j-1, curr_node)
        
        # Restore the cell
        board[i][j] = char
    
    # Start DFS from each cell
    for i in range(m):
        for j in range(n):
            dfs(i, j, trie.root)
    
    return result
```

### **Step 4: Complexity Analysis (Optimized - Trie + DFS)**

* Time Complexity: O(m * n * 4^L) where:
  * m and n are the dimensions of the board
  * L is the maximum length of a word
  * We start a DFS from each cell on the board (m * n)
  * For each starting cell, we perform a DFS with up to 4 choices (up, down, left, right) for each of the L characters
  * Building the Trie takes O(k * L) time where k is the number of words
* Space Complexity: O(k * L) for the Trie and O(L) for the recursion stack
* This approach is much more efficient than the brute force approach, especially for a large number of words with common prefixes

### **Step 5: Visualizing Computation (Optimized - Trie + DFS)**

* For the example board and words:
  * Build a Trie with "oath", "pea", "eat", and "rain"
  * Start DFS from each cell:
    * For cell (0, 0) with 'o':
      * 'o' is in the Trie, continue DFS
      * Check adjacent cells for the next character in the Trie
      * Eventually find "oath" by following the path 'o' -> 'a' -> 't' -> 'h'
    * For cell (2, 0) with 'i':
      * 'i' is not the start of any word in the Trie, prune the search
    * For cell (1, 0) with 'e':
      * 'e' is in the Trie, continue DFS
      * Check adjacent cells for the next character in the Trie
      * Eventually find "eat" by following the path 'e' -> 'a' -> 't'
  * Return ["oath", "eat"]
* The Trie approach efficiently prunes the search space by only exploring paths that form prefixes of words in the list

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Check Each Word) | O(k * m * n * 4^L) | O(m * n + L) | Inefficient for many words |
| Optimized (Trie + DFS) | O(m * n * 4^L) | O(k * L) | Efficient for words with common prefixes |

---

## **Final Takeaways**

* The key to efficiently solving the Word Search II problem is to use a Trie to store the words and perform a DFS on the board.
* A Trie allows us to efficiently check if a prefix is part of any word in the list, which helps prune the search space early.
* This problem demonstrates how combining data structures (Trie) with algorithms (DFS) can lead to efficient solutions for complex problems.

---

## **Real-life Analogy**

* Imagine you're playing a word search puzzle with a friend. The brute force approach would be like your friend giving you a word, and you scanning the entire puzzle to find it, then repeating for each word. The Trie + DFS approach would be like you scanning the puzzle once and finding all the words from a list simultaneously, skipping paths that don't lead to any word in the list.

---

## **Summary**

* We solved the Word Search II problem by first considering a brute force approach that checks each word individually, resulting in O(k * m * n * 4^L) time complexity. We then optimized using a Trie to store the words and perform a DFS on the board, which has the same worst-case time complexity but is much more efficient in practice due to pruning the search space early. The key insight is to use a Trie to efficiently check if a prefix is part of any word in the list. 