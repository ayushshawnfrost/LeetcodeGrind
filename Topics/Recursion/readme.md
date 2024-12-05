

<details id="17. Letter Combinations of a Phone Number">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">17. Letter Combinations of a Phone Number
</span>
</summary>

https://leetcode.com/problems/letter-combinations-of-a-phone-number/

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.


 

Example 1:

Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
Example 2:

Input: digits = ""
Output: []
Example 3:

Input: digits = "2"
Output: ["a","b","c"]
 

Constraints:

0 <= digits.length <= 4
digits[i] is a digit in the range ['2', '9'].

```java
class Solution {

    public List<String> letterCombinations(String digits) {
        HashMap<Character, List<Character>> map = new HashMap<>();
        map.put('2', new ArrayList<>(Arrays.asList('a', 'b', 'c')));
        map.put('3', new ArrayList<>(Arrays.asList('d', 'e', 'f')));
        map.put('4', new ArrayList<>(Arrays.asList('g', 'h', 'i')));
        map.put('5', new ArrayList<>(Arrays.asList('j', 'k', 'l')));
        map.put('6', new ArrayList<>(Arrays.asList('m', 'n', 'o')));
        map.put('7', new ArrayList<>(Arrays.asList('p', 'q', 'r', 's')));
        map.put('8', new ArrayList<>(Arrays.asList('t', 'u', 'v')));
        map.put('9', new ArrayList<>(Arrays.asList('w', 'x', 'y', 'z')));

        List<String> possibleStrings = new ArrayList<>();
        makeStrings(map, new StringBuilder(), digits, 0, possibleStrings);
        return possibleStrings;
    }

    public void makeStrings(HashMap<Character, List<Character>> map, StringBuilder sub, String digits, int i,
            List<String> ans) {
        if (i >= digits.length()) {
            if (sub.length() > 0)
                ans.add(new String(sub.toString()));
            return;
        }

        char currDigit = digits.charAt(i);
        for (char c : map.get(currDigit)) {
            sub.append(c);
            makeStrings(map, sub, digits, i + 1, ans);
            sub.deleteCharAt(sub.length() - 1);
        }
    }
}
```
</details>






<details id="22. Generate Parentheses">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">22. Generate Parentheses
</span>
</summary>

```java
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

 

Example 1:

Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
Example 2:

Input: n = 1
Output: ["()"]
 

Constraints:

1 <= n <= 8


class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<>();
        dfs(new StringBuilder(), 0, 0, n, ans);
        return ans;
    }

    private void dfs(StringBuilder str, int open, int close, int size, List<String> ans) {
        if (str.length() == size * 2 && open == close) {
            ans.add(str.toString());
            return;
        }
        str.append("(");
        if (open < size)
            dfs(str, open + 1, close, size, ans);
        str.deleteCharAt(str.length() - 1);
        
        str.append(")");
        if (open > close)
            dfs(str, open, close + 1, size, ans);
        str.deleteCharAt(str.length() - 1);
    }
}


Time Complexity:

Each valid combination has length 2n
At each position, we spend O(1) time (append/delete operations on StringBuilder)
Number of valid combinations is Cn (nth Catalan number)
For each combination, we do O(2n) work to build it


Therefore:

Total time = O(2n * Cn)
Substituting the formula for Cn:
Time = O(2n * (2n)! / ((n+1)!n!))
Using Stirlings approximation for factorials:
This simplifies to O(4^n / √n)



Therefore, the exact time complexity is O(4^n / √n).
```
</details>




<details id="79. Word Search">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">79. Word Search
</span></summary>

https://leetcode.com/problems/word-search/

Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

 

Example 1:

![Alt text](image-28.png)

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true

Example 2:

![Alt text](image-29.png)

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true

Example 3:

![Alt text](image-30.png)

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
 

Constraints:

m == board.length
n = board[i].length
1 <= m, n <= 6
1 <= word.length <= 15
board and word consists of only lowercase and uppercase English letters.
 

Follow up: Could you use search pruning to make your solution faster with a larger board?


```java


//This is a typical backtracking problem similar to Number of Islands.
//In number of Isalnds, we sinked the islands. Here, we will do the same by adding changing the value of the char.
//I added 100 because it will exceed the ascii limit for characters and will change it to some ascii value which is not an alphabet.


class Solution {

    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == word.charAt(0) && checkIfStringExist(board, word, 0, i, j)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean checkIfStringExist(char[][] board, String word, int i, int r, int c) {
        // Base Case
        if (i == word.length())
            return true;

        // check validity
        if (r < 0 || c < 0 || r >= board.length || c >= board[0].length || board[r][c] != word.charAt(i)) {
            return false;
        }
        
        // Marking as visited
        board[r][c] += 100;

        // check Neighbours
        boolean top = checkIfStringExist(board, word, i + 1, r - 1, c);
        boolean down = checkIfStringExist(board, word, i + 1, r + 1, c);
        boolean left = checkIfStringExist(board, word, i + 1, r, c - 1);
        boolean right = checkIfStringExist(board, word, i + 1, r, c + 1);
        
        // Marking as unvisited
        board[r][c] -=100;

        return top || down || left || right;
    }
}

First, let's understand the components:

Board size: M × N (where M is number of rows, N is number of columns)
Word length: L
At each cell, we can move in 4 directions (up, down, left, right)


Outer Loop Analysis:
javaCopyfor (int i = 0; i < m; i++) {
    for (int j = 0; j < n; j++) {

We need to check every starting position: O(M × N)


DFS Analysis (checkIfStringExist function):

For each starting position that matches first character:
At each step:

We can move in 4 directions
We need to explore until word length L
We mark cells as visited and unmark them when backtracking




Branching Factor Analysis:

At each position, we have 4 choices (up, down, left, right)
Maximum depth of recursion is L (word length)
However, we can't revisit cells (due to marking visited cells)


Therefore:

For each starting position: O(4^L) maximum theoretical branches
We check all starting positions: O(M × N)
At each recursive call, we do O(1) work (checking bounds, character match)


Final Time Complexity:

O(M × N × 4^L)



Understanding the practical complexity:

The actual runtime will often be better than this theoretical bound because:

We can only move to unvisited cells (marking with +100)
We stop early if character doesn't match
We return immediately when word is found
Not all starting positions will match the first character



Space Complexity:

Recursive call stack depth: O(L)
No extra space besides the call stack since we modify the board in-place to mark visited cells
```
</details>



<details id="212. Word Search II">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">212. Word Search II
</span>
</summary>

https://leetcode.com/problems/word-search-ii/description/

```java

// TLE
class Solution {

    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == word.charAt(0) && checkIfStringExist(board, word, 0, i, j)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean checkIfStringExist(char[][] board, String word, int i, int r, int c) {
        // Base Case
        if (i == word.length())
            return true;

        // check validity
        if (r < 0 || c < 0 || r >= board.length || c >= board[0].length || board[r][c] != word.charAt(i)) {
            return false;
        }
        
        // Marking as visited
        board[r][c] += 100;

        // check Neighbours
        boolean top = checkIfStringExist(board, word, i + 1, r - 1, c);
        boolean down = checkIfStringExist(board, word, i + 1, r + 1, c);
        boolean left = checkIfStringExist(board, word, i + 1, r, c - 1);
        boolean right = checkIfStringExist(board, word, i + 1, r, c + 1);
        
        // Marking as unvisited
        board[r][c] -=100;

        return top || down || left || right;
    }
}




// A More Optimized solution

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


Let me break down the time complexity analysis for this Word Search II solution:

Let's define the variables first:
- M = number of rows in the board
- N = number of columns in the board
- W = total number of words in the input array
- L = maximum length of any word in the input array

The time complexity can be broken down into two main parts:

1. **Building the Trie**:
   - For each word, we perform L operations (inserting each character)
   - We do this for W words
   - Time complexity for Trie construction: O(W * L)

2. **Searching the Board**:
   - For each cell in the board (M * N positions), we might need to explore in all 4 directions
   - At each cell, we can potentially traverse until the maximum word length (L)
   - However, we can't revisit cells during a single DFS path (due to marking cells as visited)
   - Each cell can be part of paths starting from all other cells
   - Therefore, each cell can be visited multiple times, once from each starting position

So, for searching:
- Starting positions: O(M * N)
- From each starting position: O(4^L) because at each step we have 4 choices and can go up to L steps
- Total searching complexity: O(M * N * 4^L)

**Overall Time Complexity**: O(W * L + M * N * 4^L)

Space Complexity:
1. Trie: O(W * L) - In worst case, all words have unique characters
2. Recursion Stack: O(L) - Maximum depth of recursion is L
3. Output Set: O(W) - To store found words

**Overall Space Complexity**: O(W * L)

To understand why this is not optimal for large test cases, consider:
- When L = 10, 4^L = 1,048,576
- This means for even a small 10x10 board, you might need to explore a million paths from each starting position

Here's a visualization to help understand the exponential growth:

Optimization Possibilities:

1. **Pruning Invalid Paths**: Stop DFS as soon as we find a prefix that doesn't exist in the Trie
2. **Removing Found Words**: Remove words from Trie once found to avoid searching for them again
3. **Character Frequency Pruning**: Check if required characters exist in sufficient quantity before searching

Here's how we could implement the word removal optimization:

This optimized version:
1. Maintains a count of words in each Trie node's subtree
2. Removes words after finding them
3. Stops searching paths when the subtree count becomes 0

While the worst-case time complexity remains the same, in practice, this performs much better because:
- We stop searching as soon as we find all words
- We don't search for the same word multiple times
- We can quickly prune paths that won't lead to any remaining words

However, for extremely large boards or word lists, you might want to consider alternative approaches like:
1. Using a HashSet for constant-time prefix checking
2. Pre-processing the board to build character position maps
3. Using bit manipulation for visited cell tracking

graph TD
    A[Board Cell] --> B1[Direction 1]
    A --> B2[Direction 2]
    A --> B3[Direction 3]
    A --> B4[Direction 4]
    
    B1 --> C1[4 choices]
    B1 --> C2[4 choices]
    B1 --> C3[...]
    B1 --> C4[4 choices]
    
    B2 --> D1[4 choices]
    B2 --> D2[...]
    
    B3 --> E1[...]
    
    B4 --> F1[...]

    style A fill:#f9f,stroke:#333,stroke-width:4px
    style B1 fill:#bbf,stroke:#333,stroke-width:2px
    style B2 fill:#bbf,stroke:#333,stroke-width:2px
    style B3 fill:#bbf,stroke:#333,stroke-width:2px
    style B4 fill:#bbf,stroke:#333,stroke-width:2px



```java
class Solution {
    class TrieNode {
        TrieNode[] children = new TrieNode[26];
        String word = null;
        int count = 0; // Count of words in subtree
    }
    
    private TrieNode buildTrie(String[] words) {
        TrieNode root = new TrieNode();
        for (String word : words) {
            TrieNode node = root;
            for (char c : word.toCharArray()) {
                int idx = c - 'a';
                if (node.children[idx] == null) {
                    node.children[idx] = new TrieNode();
                }
                node = node.children[idx];
                node.count++; // Increment count for each node in path
            }
            node.word = word;
        }
        return root;
    }
    
    public List<String> findWords(char[][] board, String[] words) {
        List<String> result = new ArrayList<>();
        TrieNode root = buildTrie(words);
        
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                dfs(board, i, j, root, result);
            }
        }
        return result;
    }
    
    private void dfs(char[][] board, int i, int j, TrieNode node, List<String> result) {
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) return;
        
        char c = board[i][j];
        if (c == '#' || node.children[c - 'a'] == null || node.children[c - 'a'].count == 0) return;
        
        node = node.children[c - 'a'];
        if (node.word != null) {
            result.add(node.word);
            node.word = null;  // Remove found word
            // Decrease count in path
            TrieNode temp = root;
            for (char ch : node.word.toCharArray()) {
                temp = temp.children[ch - 'a'];
                temp.count--;
            }
        }
        
        board[i][j] = '#';  // mark as visited
        dfs(board, i + 1, j, node, result);
        dfs(board, i - 1, j, node, result);
        dfs(board, i, j + 1, node, result);
        dfs(board, i, j - 1, node, result);
        board[i][j] = c;    // restore
    }
}

```
</details>


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details>






<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details>



<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details>




<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details>






<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> -->