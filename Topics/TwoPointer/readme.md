<details id="2825. Make String a Subsequence Using Cyclic Increments">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">2825. Make String a Subsequence Using Cyclic Increments 
</span>
</summary>

https://leetcode.com/problems/make-string-a-subsequence-using-cyclic-increments/description/?envType=daily-question&envId=2024-12-04

You are given two 0-indexed strings str1 and str2.

In an operation, you select a set of indices in str1, and for each index i in the set, increment str1[i] to the next character cyclically. That is 'a' becomes 'b', 'b' becomes 'c', and so on, and 'z' becomes 'a'.

Return true if it is possible to make str2 a subsequence of str1 by performing the operation at most once, and false otherwise.

Note: A subsequence of a string is a new string that is formed from the original string by deleting some (possibly none) of the characters without disturbing the relative positions of the remaining characters.

 

Example 1:

Input: str1 = "abc", str2 = "ad"
Output: true
Explanation: Select index 2 in str1.
Increment str1[2] to become 'd'. 
Hence, str1 becomes "abd" and str2 is now a subsequence. Therefore, true is returned.
Example 2:

Input: str1 = "zc", str2 = "ad"
Output: true
Explanation: Select indices 0 and 1 in str1. 
Increment str1[0] to become 'a'. 
Increment str1[1] to become 'd'. 
Hence, str1 becomes "ad" and str2 is now a subsequence. Therefore, true is returned.
Example 3:

Input: str1 = "ab", str2 = "d"
Output: false
Explanation: In this example, it can be shown that it is impossible to make str2 a subsequence of str1 using the operation at most once. 
Therefore, false is returned.
 

Constraints:

1 <= str1.length <= 105
1 <= str2.length <= 105
str1 and str2 consist of only lowercase English letters.

```java
// Resursive + Memoization TLE
class Solution {
    public boolean canMakeSubsequence(String str1, String str2) {
        if(str1.length()<str2.length())return false;
        Boolean[][] canMakeSubstr = new Boolean[str1.length()][str2.length()];
        return canMakeSubsequences(0, 0, str1, str2, canMakeSubstr);
    }

    boolean canMakeSubsequences(int i, int j, String S1, String S2, Boolean[][] canMakeSubstr) {
        // base Condition
        if (j >= S2.length())
            return true;

        if (i >= S1.length())
            return false;

        // Check if we already have answer
        if (canMakeSubstr[i][j] != null)
            return canMakeSubstr[i][j] ;

        // Case 1 char matched
        if (S1.charAt(i) == S2.charAt(j)) {
            return canMakeSubstr[i][j] = canMakeSubsequences(i + 1, j + 1, S1, S2, canMakeSubstr) || canMakeSubsequences(i + 1, j, S1, S2, canMakeSubstr);
        }

        // Case 2 char matched with next circular char
        char c = S1.charAt(i);
        if (c == 'z') {
            c = 'a';
        } else {
            c++;
        }
        if (c == S2.charAt(j)) {
             return canMakeSubstr[i][j] = canMakeSubsequences(i + 1, j + 1, S1, S2, canMakeSubstr) || canMakeSubsequences(i + 1, j, S1, S2, canMakeSubstr);
        }

        // Case 3 char
        return canMakeSubstr[i][j] = canMakeSubsequences(i + 1, j, S1, S2, canMakeSubstr);
    }
}


// Two Pointer O(str1.length())
class Solution {
    public boolean canMakeSubsequence(String str1, String str2) {
        int idx2 = 0;
        for (int i = 0; i < str1.length(); i++) {
            if (str1.charAt(i) == str2.charAt(idx2) || (str1.charAt(i) - 'a' + 1) % 26 == str2.charAt(idx2) - 'a') {
                idx2++;
            }
            if (idx2 == str2.length()) {
                return true;
            }
        }
        return false;
    }
}
```

</details>


<details id="2337. Move Pieces to Obtain a String">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">2337. Move Pieces to Obtain a String 
</span>
</summary>

https://leetcode.com/problems/move-pieces-to-obtain-a-string/description/?envType=daily-question&envId=2024-12-05

You are given two strings start and target, both of length n. Each string consists only of the characters 'L', 'R', and '_' where:

The characters 'L' and 'R' represent pieces, where a piece 'L' can move to the left only if there is a blank space directly to its left, and a piece 'R' can move to the right only if there is a blank space directly to its right.
The character '_' represents a blank space that can be occupied by any of the 'L' or 'R' pieces.
Return true if it is possible to obtain the string target by moving the pieces of the string start any number of times. Otherwise, return false.

 

Example 1:

Input: start = "_L__R__R_", target = "L______RR"
Output: true
Explanation: We can obtain the string target from start by doing the following moves:
- Move the first piece one step to the left, start becomes equal to "L___R__R_".
- Move the last piece one step to the right, start becomes equal to "L___R___R".
- Move the second piece three steps to the right, start becomes equal to "L______RR".
Since it is possible to get the string target from start, we return true.
Example 2:

Input: start = "R_L_", target = "__LR"
Output: false
Explanation: The 'R' piece in the string start can move one step to the right to obtain "_RL_".
After that, no pieces can move anymore, so it is impossible to obtain the string target from start.
Example 3:

Input: start = "_R", target = "R_"
Output: false
Explanation: The piece in the string start can move only to the right, so it is impossible to obtain the string target from start.
 

Constraints:

n == start.length == target.length
1 <= n <= 105
start and target consist of the characters 'L', 'R', and '_'.


```java

class Solution {
    public boolean canChange(String start, String target) {
        int n = start.length();
        int s = 0, t = 0;

        // Iterate through both strings
        while (s < n || t < n) {
            // Skip underscores in both strings
            while (s < n && start.charAt(s) == '_') s++;
            while (t < n && target.charAt(t) == '_') t++;

            // If both pointers reach the end, the strings are valid
            if (s == n && t == n) return true;

            // If one pointer reaches the end but the other does not, invalid
            if (s == n || t == n) return false;

            // Check if the characters at the current position are the same
            if (start.charAt(s) != target.charAt(t)) return false;

            // Check movement rules for 'L' and 'R'
            if (start.charAt(s) == 'L' && t > s) return false; // 'L' cannot move right
            if (start.charAt(s) == 'R' && t < s) return false; // 'R' cannot move left

            // Move both pointers to the next non-underscore character
            s++;
            t++;
        }
        
        return true; // Valid if we reach here
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

<!-- <details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>
</details> -->

