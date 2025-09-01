# 📝 Encode and Decode Strings

## **Problem Statement**

* Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings.
* Implement the `encode` and `decode` methods:
  * `encode(strs)`: Encodes a list of strings to a single string.
  * `decode(s)`: Decodes a single string to a list of strings.
* Example:
  * Input: strs = ["Hello","World"]
  * encode(strs) should return a string that can be decoded back to the original list.
  * decode(encode(strs)) should return ["Hello","World"]
* Constraints:
  * 1 <= strs.length <= 200
  * 0 <= strs[i].length <= 200
  * strs[i] contains any possible characters out of 256 valid ASCII characters

---

## **Step 1: Thinking Process (Brute Force)**

* We need to encode a list of strings into a single string and then decode it back
* The challenge is to handle strings that might contain any character, including delimiters
* A simple approach would be to use a special character as a delimiter between strings
* However, this fails if the strings themselves contain the delimiter
* We need a way to uniquely identify the boundaries between strings

---

## **Step 2: Flow Steps (Brute Force)**

1. Encode:
   1. Choose a delimiter (e.g., ",")
   2. Join all strings with this delimiter
   3. Return the joined string
2. Decode:
   1. Split the encoded string by the delimiter
   2. Return the resulting list of strings

---

## **Step 3: Brute Force Implementation (Code)**

```python
def encode(strs):
    # Join all strings with a comma delimiter
    return ','.join(strs)

def decode(s):
    # Split the string by comma
    return s.split(',')
```

---

## **Step 4: Complexity Analysis (Brute Force)**

* Time Complexity: 
  * Encode: O(n) where n is the total length of all strings.
  * Decode: O(n) where n is the length of the encoded string.
* Space Complexity: O(n) for both encode and decode.
* This approach is simple but incorrect because it fails if any of the original strings contain the delimiter character.

---

## **Step 5: Visualizing Redundant Computation**

* For input ["Hello","World"]:
  * Encode: "Hello,World"
  * Decode: ["Hello", "World"] ✓
* But for input ["Hello,","World"]:
  * Encode: "Hello,,World"
  * Decode: ["Hello", "", "World"] ✗
* The issue is that we can't distinguish between a delimiter and the same character in the original string.

---

## **Why are we using this algorithm?**

* For optimization, we'll use a Length Prefixing algorithm.
* The property that matches this problem is the need to uniquely identify string boundaries regardless of content.
* By prefixing each string with its length and a delimiter, we can unambiguously parse the encoded string.
* Alternatives like escaping special characters would be more complex and error-prone.
* Precondition: We need a way to represent string lengths that doesn't conflict with the string content.

---

## **Algo optimization**

### **Step 1: Thinking Process (Optimized)**

* Instead of just using a delimiter, we'll prefix each string with its length
* The format will be: "length#string" for each string in the list
* For example, "Hello" becomes "5#Hello"
* This way, even if the strings contain special characters, we can still decode them correctly

### **Step 2: Flow Steps (Optimized)**

1. Encode:
   1. For each string in the list:
      1. Compute its length
      2. Append "length#string" to the result
   2. Return the concatenated result
2. Decode:
   1. Initialize an empty result list
   2. Initialize a pointer i to 0
   3. While i < length of encoded string:
      1. Find the position of '#' starting from i
      2. Extract the length prefix and convert to integer
      3. Extract the string of the specified length
      4. Add the string to the result list
      5. Update i to point to the next string
   4. Return the result list

### **Step 3: Implementation (Optimized)**

```python
def encode(strs):
    # Encode each string as "length#string"
    result = ""
    for s in strs:
        result += str(len(s)) + "#" + s
    return result

def decode(s):
    result = []
    i = 0
    
    while i < len(s):
        # Find the position of '#'
        j = i
        while s[j] != '#':
            j += 1
        
        # Extract the length
        length = int(s[i:j])
        
        # Extract the string
        string = s[j+1:j+1+length]
        result.append(string)
        
        # Move to the next string
        i = j + 1 + length
    
    return result
```

### **Step 4: Complexity Analysis (Optimized)**

* Time Complexity: 
  * Encode: O(n) where n is the total length of all strings.
  * Decode: O(n) where n is the length of the encoded string.
* Space Complexity: O(n) for both encode and decode.
* This approach correctly handles strings that might contain any character.

### **Step 5: Visualizing Computation (Optimized)**

* For input ["Hello","World"]:
  * Encode: "5#Hello5#World"
  * Decode: 
    * Extract length 5, then string "Hello"
    * Extract length 5, then string "World"
    * Result: ["Hello", "World"] ✓
* For input ["Hello,","World"]:
  * Encode: "6#Hello,5#World"
  * Decode: 
    * Extract length 6, then string "Hello,"
    * Extract length 5, then string "World"
    * Result: ["Hello,", "World"] ✓

### **Complexity Summary**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Brute Force (Simple Delimiter) | O(n) | O(n) | Fails if strings contain the delimiter |
| Optimized (Algorithm: Length Prefixing) | O(n) | O(n) | Works for all possible inputs |

---

## **Final Takeaways**

* When dealing with arbitrary strings, using length prefixes is a robust way to encode and decode them.
* This is a common technique in network protocols and serialization formats.
* The key insight is to include metadata (length) that allows unambiguous parsing, rather than relying on special characters that might appear in the data.

---

## **Real-life Analogy**

* Imagine you're packing multiple items into a single box for shipping. The brute force approach would be like placing dividers between items, but if an item itself looks like a divider, the receiver might get confused. The optimized approach is like attaching a label to each item that specifies its size, so the receiver knows exactly where each item begins and ends, regardless of what the items look like.

---

## **Summary**

* We solved the "Encode and Decode Strings" problem by first considering a simple delimiter-based approach, which fails for strings containing the delimiter. We then optimized using a length prefixing technique, where each string is prefixed with its length followed by a delimiter. This allows for unambiguous parsing of the encoded string, regardless of the content of the original strings. Both approaches have the same time and space complexity, but the optimized one correctly handles all possible inputs. 