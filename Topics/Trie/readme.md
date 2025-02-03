# Trie

# What is a Trie Data Structure?

A **Trie**, also known as a **prefix tree**, is a tree-like data structure used to store a dynamic set of strings where each node represents a single character of a string. Tries are particularly efficient for tasks involving sequences of characters, such as words in a dictionary.

## Key Characteristics of a Trie:
- Each node contains:
  - An array (or map) of child pointers for each possible character.
  - A flag to indicate the end of a word.
  - (Optional) Additional data such as the word stored at that node.
  
- The root node represents an empty string.
- Each path from the root to a node represents a prefix or a complete word.

## Where is Trie Useful?

Tries are particularly useful for:
1. **Autocomplete systems**: Quickly suggest words based on a prefix.
2. **Spell checking**: Checking if a word exists or suggesting corrections.
3. **IP routing (longest prefix matching)**.
4. **DNA sequencing**: Storing and querying genetic sequences.
5. **Word games**: Efficiently finding all words that can be formed from a set of letters.
6. **Search engines**: Finding all strings with a common prefix.

## Hints to Identify if a LeetCode Question Should Be Solved Using Trie:
1. **Prefix-related problems**: If the problem involves searching or storing prefixes of words (e.g., autocomplete, search by prefix).
2. **Word dictionary problems**: If you're asked to store and query a large set of words efficiently.
3. **Word search**: Problems involving finding all possible words from a board/grid of characters.
4. **Multiple queries on a large dictionary**: When the problem involves repeatedly querying a fixed set of words or prefixes.

## Common Replacement for Trie:
In some scenarios, **HashSets** or **HashMaps** can replace Tries, particularly when:
1. **Prefix requirement is minimal**: If only checking for the presence or absence of words, a HashSet is more straightforward.
2. **Memory efficiency is crucial**: Tries can be memory-intensive due to storing child pointers for every possible character.
3. **When word length is short**: A HashMap with strings as keys may suffice when the maximum word length is small, and there is no need to handle a large number of prefix queries efficiently.

## Example Use Case on LeetCode:
- **Trie Suitable Problem**: "Implement Trie (Prefix Tree)" or "Word Search II".
- **Replacement with HashMap/Set**: "Word Break" where you're only checking if words exist in a dictionary. Here, a HashSet can store all dictionary words, and dynamic programming or backtracking can be used without needing a Trie.


<details id="212. Word Search II">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">212. Word Search II 
</span>
</summary>

https://leetcode.com/problems/word-search-ii/description/

Given an m x n board of characters and a list of strings words, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

 

Example 1:


Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]
Example 2:


Input: board = [["a","b"],["c","d"]], words = ["abcb"]
Output: []
 

Constraints:

m == board.length
n == board[i].length
1 <= m, n <= 12
board[i][j] is a lowercase English letter.
1 <= words.length <= 3 * 104
1 <= words[i].length <= 10
words[i] consists of lowercase English letters.
All the strings of words are unique.

```java
class Solution {
    class TrieNode {
        String word;
        boolean wordEnd;
        TrieNode[] children;

        TrieNode() {
            this.children = new TrieNode[26];
        }
    }

    private void insertInTrie(String word, TrieNode root) {
        TrieNode node = root;
        for (char ch : word.toCharArray()) {
            if (node.children[ch - 'a'] == null) {
                node.children[ch - 'a'] = new TrieNode();
            }
            node = node.children[ch - 'a'];
        }
        node.word = word;  // Store word at the end node
        node.wordEnd = true;
    }

    public List<String> findWords(char[][] board, String[] words) {
        int m = board.length;
        int n = board[0].length;
        Set<String> answer = new HashSet<>();
        TrieNode trieRoot = new TrieNode();

        // Build Trie
        for (String word : words) {
            insertInTrie(word, trieRoot);
        }

        // Search each cell as starting point
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                char startChar = board[i][j];
                if (trieRoot.children[startChar - 'a'] != null) {
                    dfs(board, i, j, trieRoot, answer);
                }
            }
        }
        return new ArrayList<>(answer);
    }

    private void dfs(char[][] board, int r, int c, TrieNode node, Set<String> ans) {
        // Check bounds and validity
        if (r < 0 || c < 0 || r >= board.length || c >= board[0].length || 
            board[r][c] == '$' || node.children[board[r][c] - 'a'] == null) {
            return;
        }

        char currentChar = board[r][c];
        node = node.children[currentChar - 'a'];
        
        // Check if we've found a word
        if (node.wordEnd) {
            ans.add(node.word);
            // Don't return here - there might be longer words with this as prefix
        }

        // Mark as visited
        board[r][c] = '$';

        // Check all four directions
        dfs(board, r - 1, c, node, ans);
        dfs(board, r + 1, c, node, ans);
        dfs(board, r, c - 1, node, ans);
        dfs(board, r, c + 1, node, ans);

        // Backtrack
        board[r][c] = currentChar;
    }
}

```




</details>

<details id="208. Implement Trie (Prefix Tree)">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">208. Implement Trie (Prefix Tree) 
</span>
</summary>

https://leetcode.com/problems/implement-trie-prefix-tree/description/

A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

Trie() Initializes the trie object.
void insert(String word) Inserts the string word into the trie.
boolean search(String word) Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
boolean startsWith(String prefix) Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.
 

Example 1:

Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True
 

Constraints:

1 <= word.length, prefix.length <= 2000
word and prefix consist only of lowercase English letters.
At most 3 * 104 calls in total will be made to insert, search, and startsWith.


```java
class Trie {
    TrieNode root;

    // TrieNode class represents each node of the Trie
    class TrieNode {
        TrieNode[] children; // Array to store references to child nodes (26 for each letter a-z)
        boolean end; // Flag to mark the end of a word
        String word; // Store the word at the end node

        TrieNode() {
            this.children = new TrieNode[26]; // Initialize child nodes array
            this.end = false; // Initially, it's not the end of any word
            this.word = ""; // Initially, no word is stored
        }
    }

    // Constructor initializes the root node of the Trie
    public Trie() {
        this.root = new TrieNode();
    }

    // Insert method to add a word into the Trie
    public void insert(String word) {
        TrieNode crawler = this.root;
        for (char c : word.toCharArray()) {
            // Create a new TrieNode if the child node for the current character is null
            if (crawler.children[c - 'a'] == null) {
                crawler.children[c - 'a'] = new TrieNode();
            }
            // Move to the next node
            crawler = crawler.children[c - 'a'];
        }
        // Mark the end of the word and store the word at the end node
        crawler.end = true;
        crawler.word = word;
    }

    // Search method to find if a word is in the Trie
    public boolean search(String word) {
        TrieNode crawler = this.root;
        for (char c : word.toCharArray()) {
            // If the child node for the current character is null, the word is not present
            if (crawler.children[c - 'a'] == null)
                return false;
            // Move to the next node
            crawler = crawler.children[c - 'a'];
        }
        // Check if the word ends at the current node
        return crawler.end;
    }

    // StartsWith method to check if there is any word in the Trie that starts with the given prefix
    public boolean startsWith(String prefix) {
        TrieNode crawler = this.root;
        for (char c : prefix.toCharArray()) {
            // If the child node for the current character is null, no word with the given prefix exists
            if (crawler.children[c - 'a'] == null)
                return false;
            // Move to the next node
            crawler = crawler.children[c - 'a'];
        }
        // Return true if all characters of the prefix are found
        return true;
    }
}

/**
 * Complexity Analysis:
 * 
 * - Space Complexity:
 *   - In the worst case, every node in the Trie has 26 children, leading to an exponential space requirement.
 *   - Space complexity is O(M * N), where M is the number of words inserted, and N is the average length of the words.
 * 
 * - Time Complexity:
 *   - Insert: O(N), where N is the length of the word being inserted.
 *     - For each character in the word, we either traverse or create a node in the Trie.
 *   - Search: O(N), where N is the length of the word being searched.
 *     - For each character in the word, we traverse the Trie to find the word.
 *   - StartsWith: O(N), where N is the length of the prefix.
 *     - For each character in the prefix, we traverse the Trie to find the prefix.
 */


```


</details>


<details id="421. Maximum XOR of Two Numbers in an Array">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">421. Maximum XOR of Two Numbers in an Array 
</span>
</summary>

[421. Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/description/)

Given an integer array nums, return the maximum result of nums[i] XOR nums[j], where 0 <= i <= j < n.

 

Example 1:

Input: nums = [3,10,5,25,2,8]
Output: 28
Explanation: The maximum result is 5 XOR 25 = 28.
Example 2:

Input: nums = [14,70,53,83,49,91,36,80,92,51,66,70]
Output: 127
 

Constraints:

1 <= nums.length <= 2 * 105
0 <= nums[i] <= 231 - 1


```java
//T.C : O(32*n)
public class Solution {
    static class TrieNode {
        TrieNode left;
        TrieNode right;
    }

    public void insert(TrieNode head, int num) {
        TrieNode pCrawl = head;
        for (int i = 31; i >= 0; i--) {
            int ithBit = (num >> i) & 1;
            if (ithBit == 0) {
                if (pCrawl.left == null) {
                    pCrawl.left = new TrieNode();
                }
                pCrawl = pCrawl.left;
            } else {
                if (pCrawl.right == null) {
                    pCrawl.right = new TrieNode();
                }
                pCrawl = pCrawl.right;
            }
        }
    }

    public int maxXor(TrieNode head, int num) {
        int maxXor = 0;
        TrieNode pCrawl = head;
        //I am moving from left most bit(MSB) to right most(LSB) to get max answer so as to get set bit 1 in left most position (MSB) to get large decimal value
        for (int i = 31; i >= 0; i--) {
            int ithBit = (num >> i) & 1;
            if (ithBit == 1) {
                if (pCrawl.left != null) {
                    maxXor += Math.pow(2, i);
                    pCrawl = pCrawl.left;
                } else {
                    pCrawl = pCrawl.right;
                }
            } else {
                if (pCrawl.right != null) {
                    maxXor += Math.pow(2, i);
                    pCrawl = pCrawl.right;
                } else {
                    pCrawl = pCrawl.left;
                }
            }
        }
        return maxXor;
    }

    public int findMaximumXOR(int[] nums) {
        TrieNode root = new TrieNode();
        for (int x : nums) {
            insert(root, x);
        }

        int result = 0;

        for (int x : nums) {
            result = Math.max(result, maxXor(root, x));
        }
        return result;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        int[] nums = {3, 10, 5, 25, 2, 8};
        System.out.println(solution.findMaximumXOR(nums));
    }
}
```

</details>




<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->




<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->




<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->




<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->




<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->




<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->




<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->




<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->




<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->



