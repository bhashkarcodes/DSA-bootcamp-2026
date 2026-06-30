## Trie Data Structure 

## 1.Introduction to Trie

A Trie is an advanced, tree-like data structure used to efficiently store and retrieve keys in a dynamic dataset of strings. Unlike standard binary search trees, individual nodes in a Trie do not store the key associated with that node. Instead, a node's absolute position within the tree hierarchy defines the key or prefix it represents. 

## Properties of a Trie: 

- Root Node: Every Trie has an explicit, empty root node that serves as the entry point and does not represent any valid character. 

- Edges as Characters: Each edge connecting a parent node to a child node implicitly represents a unique character. 

- Alphabet Pointers: Every node contains an array or hash map of child pointers corresponding to the alphabet size ( _Σ_ ), along with a boolean flag ( `isEndOfWord` ) indicating whether a valid word terminates at this specific node. 

- Prefix Sharing: Strings that share a common prefix share the exact same sequence of ancestor nodes, dramatically optimizing space for sets of overlapping strings. 

## Trie vs. Hash Table Comparison 

|Feature|Hash Table|Trie Data Structure|
|---|---|---|
|Search Time<br>Complexity|_O(1)_(Average case; susceptible to<br>degradation via collisions)|_O(L)_where_L_is the length of the string<br>(Deterministic)|
|Prefx-Based<br>Search|Inefcient (_O(N × L)_) requires<br>checking all entries|Extremely efcient (_O(L)_) by following the<br>prefx branch|
|Sorting & Ordered<br>Traversal|Keys are unordered; sorting requires<br>external algorithms|Keys are implicitly ordered alphabetically<br>via standard DFS traversal|
|Collision Overhead|Requires handling overhead<br>(Chaining or Open Addressing)|Completely deterministic; zero collision<br>handling required|



## 2. Structural Node Representation 

For standard English lowercase alphabets ( `'a'` through `'z'` ), each node contains an array of 26 pointers, where index `0` maps to `'a'` and index `25` maps to `'z'` . 

## C++ Structural Node 

```
struct TrieNode {
    TrieNode* children[26];
    bool isEndOfWord;
    TrieNode() {
        isEndOfWord = false;
        for (int i = 0; i < 26; i++) {
            children[i] = nullptr;
        }
    }
};
```

## Java Structural Node 

```
class TrieNode {
    TrieNode[] children;
    boolean isEndOfWord;
    public TrieNode() {
        children = new TrieNode[26];
        isEndOfWord = false;
    }
}
```

## Python Structural Node 

```
class TrieNode:
    def __init__(self):
        self.children = [None] * 26
        self.is_end_of_word = False
```

## 3. Operations: Insertion and Search 

Insertion: To insert a word, we start from the root node and traverse down character by character. If the pointer at the computed index ( `character - 'a'` ) is `null` , we instantiate a new node. Finally, the terminal node's `isEndOfWord` flag is set to `true` . 

Search: We traverse down the branches matching the characters of the word. If any child pointer is missing along the path, the word does not exist ( _false_ ). If the path completes, we return the value of `isEndOfWord` . 

## C++ 

```
class Trie {
private:
    TrieNode* root;
public:
    Trie() { root = new TrieNode(); }
    void insert(string word) {
        TrieNode* curr = root;
        for (char ch : word) {
            int idx = ch - 'a';
            if (curr->children[idx] == nullptr) {
                curr->children[idx] = new TrieNode();
            }
            curr = curr->children[idx];
        }
        curr->isEndOfWord = true;
    }
    bool search(string word) {
        TrieNode* curr = root;
        for (char ch : word) {
            int idx = ch - 'a';
            if (curr->children[idx] == nullptr) return false;
            curr = curr->children[idx];
        }
        return curr->isEndOfWord;
    }
};
```

Java 

```
public class Trie {
    private TrieNode root;
    public Trie() { root = new TrieNode(); }
    public void insert(String word) {
        TrieNode curr = root;
        for (int i = 0; i < word.length(); i++) {
            int idx = word.charAt(i) - 'a';
            if (curr.children[idx] == null) {
                curr.children[idx] = new TrieNode();
            }
            curr = curr.children[idx];
        }
        curr.isEndOfWord = true;
    }
    public boolean search(String word) {
        TrieNode curr = root;
        for (int i = 0; i < word.length(); i++) {
            int idx = word.charAt(i) - 'a';
            if (curr.children[idx] == null) return false;
            curr = curr.children[idx];
        }
        return curr.isEndOfWord;
    }
}
```


## Python 

```
class Trie:
    def __init__(self):
        self.root = TrieNode()
    def insert(self, word: str) -> None:
        curr = self.root
        for ch in word:
            idx = ord(ch) - ord('a')
            if curr.children[idx] is None:
                curr.children[idx] = TrieNode()
            curr = curr.children[idx]
        curr.is_end_of_word = True
    def search(self, word: str) -> bool:
        curr = self.root
        for ch in word:
            idx = ord(ch) - ord('a')
            if curr.children[idx] is None:
                return False
            curr = curr.children[idx]
        return curr.is_end_of_word
```

## 4. Deletion Operation in trie 

Deleting a string requires recursive backtracking. Nodes cannot be blindly removed because they may be part of an overlapping prefix for other words. The logic evaluates three conditions during post-order traversal: 

- Case 1: The deleted word is a structural prefix of another word (e.g., delete `"an"` while `"and"` exists) → Switch `isEndOfWord = false` . Do not free memory. 

- Case 2: The word shares a partial prefix (e.g., delete `"and"` while `"ant"` exists) → Delete nodes from the bottom up until reaching the joint node. 

- Case 3: The word is completely unique → Free all nodes along the path from the end down to the root branches. 

## C++ Deletion Code 

```
class AdvancedTrie {
private:
    TrieNode* root;
    bool isEmpty(TrieNode* node) {
        for (int i = 0; i < 26; i++) {
            if (node->children[i] != nullptr) return false;
        }
        return true;
    }
    TrieNode* removeHelper(TrieNode* curr, string& word, int depth) {
        if (!curr) return nullptr;
        if (depth == word.length()) {
            if (curr->isEndOfWord) curr->isEndOfWord = false;
            if (isEmpty(curr)) {
                delete curr;
                curr = nullptr;
            }
            return curr;
        }
        int idx = word[depth] - 'a';
        curr->children[idx] = removeHelper(curr->children[idx], word, depth + 1);
        if (isEmpty(curr) && !curr->isEndOfWord) {
            delete curr;
            curr = nullptr;
        }
        return curr;
    }
public:
```

```
        curr->children[idx] = removeHelper(curr->children[idx], word, depth + 1);
```

```
    AdvancedTrie() { root = new TrieNode(); }
```

```
    void remove(string word) { root = removeHelper(root, word, 0); }
};
```
 

Java Deletion Code 

```
public class AdvancedTrie {

    private TrieNode root = new TrieNode();

    private boolean isEmpty(TrieNode node) {
        for (TrieNode child : node.children) {
            if (child != null) return false;
        return true;

    private TrieNode removeHelper(TrieNode curr, String word, int depth) {
        if (curr == null) return null;

        if (depth == word.length()) {
            if (curr.isEndOfWord) curr.isEndOfWord = false;
            if (isEmpty(curr)) curr = null;
            return curr;
        }
        int idx = word.charAt(depth) - 'a';
        curr.children[idx] = removeHelper(curr.children[idx], word, depth + 1);
        if (isEmpty(curr) && !curr.isEndOfWord) curr = null;
        return curr;
    }
    public void remove(String word) { root = removeHelper(root, word, 0); }
}
```

## Python Deletion Code 

```
class AdvancedTrie:
    def __init__(self):
        self.root = TrieNode()
    def _is_empty(self, node: TrieNode) -> bool:
        return all(child is None for child in node.children)
    def _remove_helper(self, curr: TrieNode, word: str, depth: int) -> TrieNode:
        if not curr:
            return None
        if depth == len(word):
            if curr.is_end_of_word:
                curr.is_end_of_word = False
            if self._is_empty(curr):
                curr = None
            return curr
        idx = ord(word[depth]) - ord('a')
        curr.children[idx] = self._remove_helper(curr.children[idx], word, depth + 1)
        if self._is_empty(curr) and not curr.is_end_of_word:
            curr = None
        return curr
    def remove(self, word: str) -> None:
        self.root = self._remove_helper(self.root, word, 0)
```

## 5. Complexity Analysis 

|Operation|Time Complexity|Space Complexity|
|---|---|---|
|Insertion|_O(L)_|_O(L × Σ)_(Worst case if creating an entirely unique path)|
|Searching|_O(L)_|_O(1)_|
|Deletion|_O(L)_|_O(L)_(Auxiliary call stack space)|



_Note:_ _L represents the absolute length of the key/word string, and_ _Σ represents the character set alphabet space (26 for standard lowercase alphabets)._ 


## 6.Problem Set 

A list of problems focusing on the Trie data structure.

## Easy Problems
*   [Displaying Content of a Trie](https://www.geeksforgeeks.org/dsa/trie-insert-and-search/) 
*   [Sorting Array of Strings Using Trie](https://geeksforgeeks.org) 


## Medium Problems
*   [Trie Delete](https://geeksforgeeks.org)
*   [Auto-complete Feature Using Trie](https://geeksforgeeks.org) 
*   [Unique Rows in a Binary Matrix](https://www.geeksforgeeks.org/problems/unique-rows-in-boolean-matrix/1) 
*   [Count of Distinct Substrings](https://www.geeksforgeeks.org/problems/count-of-distinct-substrings/1)
*   [Pattern Searching Using a Trie of All Suffixes](https://geeksforgeeks.org)
*   [Maximum XOR of Two Numbers in an Array](https://leetcode.com)


## Hard Problems
*   [Minimum Word Break](https://www.leetcode.com/problems/word-break/) 
*   [Word Boggle (Word Search II)](https://www.leetcode.com/problems/word-search-ii/) 
*   [Maximum XOR with an Element From an Array](https://leetcode.com) 


## Resources

### GeeksforGeeks 
* [Trie insert and search](https://www.geeksforgeeks.org/dsa/trie-insert-and-search/)
* [Trie delete](https://www.geeksforgeeks.org/dsa/trie-delete/)
* [Problems on Trie](https://www.geeksforgeeks.org/dsa/commonly-asked-data-structure-interview-questions-on-tries/)    