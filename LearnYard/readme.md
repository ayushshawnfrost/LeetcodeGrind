# 2 POINTERS

### ON ARRAYS

<details id="31. Next Permutation">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">31. Next Permutation 
</span>
</summary>
A permutation of an array of integers is an arrangement of its members into a sequence or linear order.

For example, for arr = [1,2,3], the following are all the permutations of arr: [1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1].
The next permutation of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the next permutation of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

For example, the next permutation of arr = [1,2,3] is [1,3,2].
Similarly, the next permutation of arr = [2,3,1] is [3,1,2].
While the next permutation of arr = [3,2,1] is [1,2,3] because [3,2,1] does not have a lexicographical larger rearrangement.
Given an array of integers nums, find the next permutation of nums.

The replacement must be in place and use only constant extra memory.

 

Example 1:

Input: nums = [1,2,3]
Output: [1,3,2]
Example 2:

Input: nums = [3,2,1]
Output: [1,2,3]
Example 3:

Input: nums = [1,1,5]
Output: [1,5,1]
 

Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 100

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int swapIndex=-1;
        // find a position where a next greater digit can be placed
        for(int i=nums.length-2;i>=0;i--){
            if(nums[i]<nums[i+1]){
                swapIndex=i;
                break;
            }
        }
        if(swapIndex == -1){
            swap(nums, 0,nums.length-1);
        }else{
            // find the digit to be swapped with
            for(int i=nums.length-1;i>swapIndex;i--){
                if(nums[i]>nums[swapIndex]){
                    int temp=nums[swapIndex];
                    nums[swapIndex]=nums[i];
                    nums[i]=temp;
                    break;
                }
            }

            // reverse the suffix
            swap(nums, swapIndex+1,nums.length-1);
        }
    }
    private void swap(int[] nums, int i1, int i2){
        while(i1<i2){
            int temp= nums[i1];
            nums[i1]=nums[i2];
            nums[i2]=temp;
            i2--;i1++;
        }
    }
}
```
</details> 


 <details id="556. Next Greater Element III">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">556. Next Greater Element III 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
Given a positive integer n, find the smallest integer which has exactly the same digits existing in the integer n and is greater in value than n. If no such positive integer exists, return -1.

Note that the returned integer should fit in 32-bit integer, if there is a valid answer but it does not fit in 32-bit integer, return -1.

 

Example 1:

Input: n = 12
Output: 21
Example 2:

Input: n = 21
Output: -1
 

Constraints:

1 <= n <= 231 - 1


```java
class Solution {
    public int nextGreaterElement(int n) {
        char[] digits = String.valueOf(n).toCharArray();
        int i = digits.length - 2;
        
        // Step 1: Find pivot
        while (i >= 0 && digits[i] >= digits[i + 1]) {
            i--;
        }
        if (i < 0) return -1;
        
        // Step 2: Find next greater digit
        int j = digits.length - 1;
        while (digits[j] <= digits[i]) {
            j--;
        }
        
        // Step 3: Swap
        swap(digits, i, j);
        
        // Step 4: Reverse suffix
        reverse(digits, i + 1, digits.length - 1);
        
        // Step 5: Convert & Check overflow
        long val = Long.parseLong(new String(digits));
        return (val > Integer.MAX_VALUE) ? -1 : (int) val;
    }
    
    private void swap(char[] arr, int i, int j) {
        char temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    
    private void reverse(char[] arr, int left, int right) {
        while (left < right) {
            swap(arr, left++, right--);
        }
    }
}

```
</details> 

### ON STRING

<details id="151. Reverse Words in a String">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">151. Reverse Words in a String 
</span>
</summary>

Given an input string s, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

 

Example 1:

Input: s = "the sky is blue"
Output: "blue is sky the"
Example 2:

Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
Example 3:

Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
 

Constraints:

1 <= s.length <= 104
s contains English letters (upper-case and lower-case), digits, and spaces ' '.
There is at least one word in s.
 

Follow-up: If the string data type is mutable in your language, can you solve it in-place with O(1) extra space?

```java
class Solution {
    public String reverseWords(String s) {
        s = s.trim();  // Remove leading/trailing spaces
        char[] char_string = s.toCharArray();
        int n = char_string.length;

        // 1. Reverse the entire string
        utility(char_string, 0, n - 1);

        // 2. Process word by word
        int i = 0, r = 0;
        while (i < n) {
            if (char_string[i] != ' ') {
                if (r != 0) char_string[r++] = ' ';  // Add single space before the word if not first
                int start = r;
                while (i < n && char_string[i] != ' ') {
                    char_string[r++] = char_string[i++];
                }
                utility(char_string, start, r - 1);  // Reverse the word back to correct order
            } else {
                i++;  // skip multiple spaces
            }
        }

        return new String(char_string, 0, r);
    }

    private void utility(char[] s, int l, int r) {
        while (l < r) {
            char temp = s[l];
            s[l++] = s[r];
            s[r--] = temp;
        }
    }
}

```
</details> 


<details id="821. Shortest Distance to a Character">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">821. Shortest Distance to a Character 
</span>
</summary>

Given a string s and a character c that occurs in s, return an array of integers answer where answer.length == s.length and answer[i] is the distance from index i to the closest occurrence of character c in s.

The distance between two indices i and j is abs(i - j), where abs is the absolute value function.

 

Example 1:

Input: s = "loveleetcode", c = "e"
Output: [3,2,1,0,1,0,0,1,2,2,1,0]
Explanation: The character 'e' appears at indices 3, 5, 6, and 11 (0-indexed).
The closest occurrence of 'e' for index 0 is at index 3, so the distance is abs(0 - 3) = 3.
The closest occurrence of 'e' for index 1 is at index 3, so the distance is abs(1 - 3) = 2.
For index 4, there is a tie between the 'e' at index 3 and the 'e' at index 5, but the distance is still the same: abs(4 - 3) == abs(4 - 5) = 1.
The closest occurrence of 'e' for index 8 is at index 6, so the distance is abs(8 - 6) = 2.
Example 2:

Input: s = "aaab", c = "b"
Output: [3,2,1,0]
 

Constraints:

1 <= s.length <= 104
s[i] and c are lowercase English letters.
It is guaranteed that c occurs at least once in s.

```java
class Solution {
    public int[] shortestToChar(String s, char c) {
        int[] answer=new int[s.length()];
        int lastIndex=-1;
        for(int i=0;i<s.length();i++){
            if(s.charAt(i)==c){
                lastIndex=i;
                answer[i]=0;
                continue;
            }
            if(lastIndex!=-1){
                answer[i]=Math.abs(lastIndex-i);
                continue;
            }
            if(lastIndex==-1){
                answer[i]=Integer.MAX_VALUE;
                continue;
            }
        }
        lastIndex=-1;
        for(int i=s.length()-1;i>=0;i--){
            if(s.charAt(i)==c){
                lastIndex=i;
            }
            if(lastIndex!=-1){
                answer[i]=Math.min(answer[i], Math.abs(lastIndex-i));
            }
        }
        return answer;
    }
}
```
</details> 

<details id="2825. Make String a Subsequence Using Cyclic Increments">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">2825. Make String a Subsequence Using Cyclic Increments 
</span>
</summary>

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
class Solution {
    public boolean canMakeSubsequence(String str1, String str2) {
        int i=0;
        int j=0;

        while(i<str1.length() && j<str2.length()){
            if(str1.charAt(i) == str2.charAt(j)){
                j++;
            }else if((str1.charAt(i)+1 == str2.charAt(j)) || (str1.charAt(i)-25 == str2.charAt(j)) ){
                j++;
            }
            i++;
        }
        return j == str2.length();
    }
}

 TC= O(m+n)
```
</details>


<details id="696. Count Binary Substrings">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">696. Count Binary Substrings 
</span>
</summary>

Given a binary string s, return the number of non-empty substrings that have the same number of 0's and 1's, and all the 0's and all the 1's in these substrings are grouped consecutively.

Substrings that occur multiple times are counted the number of times they occur.

 

Example 1:

Input: s = "00110011"
Output: 6
Explanation: There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".
Notice that some of these substrings repeat and are counted the number of times they occur.
Also, "00110011" is not a valid substring because all the 0's (and 1's) are not grouped together.
Example 2:

Input: s = "10101"
Output: 4
Explanation: There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.
 

Constraints:

1 <= s.length <= 105
s[i] is either '0' or '1'.

```java
class Solution {
    public int countBinarySubstrings(String s) {
        List<Integer> contigiusCnt = new ArrayList<>();
        int count = 1; // start with 1 for the first char

        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(i) == s.charAt(i - 1)) {
                count++;
            } else {
                contigiusCnt.add(count);
                count = 1;
            }
        }
        contigiusCnt.add(count); // add the last group

        int substrings = 0;
        for (int j = 1; j < contigiusCnt.size(); j++) {
            substrings += Math.min(contigiusCnt.get(j - 1), contigiusCnt.get(j));
        }
        return substrings;
    }
}

```
</details>

<details id="1750. Minimum Length of String After Deleting Similar Ends">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1750. Minimum Length of String After Deleting Similar Ends 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
Hint
Given a string s consisting only of characters 'a', 'b', and 'c'. You are asked to apply the following algorithm on the string any number of times:

Pick a non-empty prefix from the string s where all the characters in the prefix are equal.
Pick a non-empty suffix from the string s where all the characters in this suffix are equal.
The prefix and the suffix should not intersect at any index.
The characters from the prefix and suffix must be the same.
Delete both the prefix and the suffix.
Return the minimum length of s after performing the above operation any number of times (possibly zero times).

 

Example 1:

Input: s = "ca"
Output: 2
Explanation: You can't remove any characters, so the string stays as is.
Example 2:

Input: s = "cabaabac"
Output: 0
Explanation: An optimal sequence of operations is:
- Take prefix = "c" and suffix = "c" and remove them, s = "abaaba".
- Take prefix = "a" and suffix = "a" and remove them, s = "baab".
- Take prefix = "b" and suffix = "b" and remove them, s = "aa".
- Take prefix = "a" and suffix = "a" and remove them, s = "".
Example 3:

Input: s = "aabccabba"
Output: 3
Explanation: An optimal sequence of operations is:
- Take prefix = "aa" and suffix = "a" and remove them, s = "bccabb".
- Take prefix = "b" and suffix = "bb" and remove them, s = "cca".
 

Constraints:

1 <= s.length <= 105
s only consists of characters 'a', 'b', and 'c'.

```java
class Solution {
    public int minimumLength(String s) {
        int l = 0, r = s.length() - 1;
        while (l < r && s.charAt(l) == s.charAt(r)) {
            char sameChar = s.charAt(l);
            while (l <= r && s.charAt(l) == sameChar) {
                l++;
            }
            while (l <= r && s.charAt(r) == sameChar) {
                r--;
            }
        }
        return r - l + 1;
    }
}

```
</details>


<details id="443. String Compression">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">443. String Compression 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
Hint
Given an array of characters chars, compress it using the following algorithm:

Begin with an empty string s. For each group of consecutive repeating characters in chars:

If the group's length is 1, append the character to s.
Otherwise, append the character followed by the group's length.
The compressed string s should not be returned separately, but instead, be stored in the input character array chars. Note that group lengths that are 10 or longer will be split into multiple characters in chars.

After you are done modifying the input array, return the new length of the array.

You must write an algorithm that uses only constant extra space.

 

Example 1:

Input: chars = ["a","a","b","b","c","c","c"]
Output: Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]
Explanation: The groups are "aa", "bb", and "ccc". This compresses to "a2b2c3".
Example 2:

Input: chars = ["a"]
Output: Return 1, and the first character of the input array should be: ["a"]
Explanation: The only group is "a", which remains uncompressed since it's a single character.
Example 3:

Input: chars = ["a","b","b","b","b","b","b","b","b","b","b","b","b"]
Output: Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].
Explanation: The groups are "a" and "bbbbbbbbbbbb". This compresses to "ab12".
 

Constraints:

1 <= chars.length <= 2000
chars[i] is a lowercase English letter, uppercase English letter, digit, or symbol.

```java
class Solution {
    public int compress(char[] chars) {
        int write_index = 0;
        int read_left_index = 0;
        int len = chars.length;

        while (read_left_index < len) {
            int read_right_index = read_left_index;
            char curr_char = chars[read_left_index];

            // Count group length
            while (read_right_index < len && chars[read_right_index] == curr_char) {
                read_right_index++;
            }

            int count = read_right_index - read_left_index;

            // Write the character
            chars[write_index++] = curr_char;

            // Write the count if > 1
            if (count > 1) {
                String countStr = String.valueOf(count);
                for (char c : countStr.toCharArray()) {
                    chars[write_index++] = c;
                }
            }

            read_left_index = read_right_index;
        }
        return write_index;
    }
}

```
</details>

<details id="2938. Separate Black and White Balls">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">2938. Separate Black and White Balls 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
Hint
There are n balls on a table, each ball has a color black or white.

You are given a 0-indexed binary string s of length n, where 1 and 0 represent black and white balls, respectively.

In each step, you can choose two adjacent balls and swap them.

Return the minimum number of steps to group all the black balls to the right and all the white balls to the left.

 

Example 1:

Input: s = "101"
Output: 1
Explanation: We can group all the black balls to the right in the following way:
- Swap s[0] and s[1], s = "011".
Initially, 1s are not grouped together, requiring at least 1 step to group them to the right.
Example 2:

Input: s = "100"
Output: 2
Explanation: We can group all the black balls to the right in the following way:
- Swap s[0] and s[1], s = "010".
- Swap s[1] and s[2], s = "001".
It can be proven that the minimum number of steps needed is 2.
Example 3:

Input: s = "0111"
Output: 0
Explanation: All the black balls are already grouped to the right.
 

Constraints:

1 <= n == s.length <= 105
s[i] is either '0' or '1'.




```java
class Solution {
    public long minimumSteps(String s) {
        long steps = 0;
        long ones_seen = 0;

        for (char c : s.toCharArray()) {
            if (c == '1') {
                ones_seen++;
            } else { // c == '0'
                steps += ones_seen;
            }
        }
        return steps;
    }
}

```
</details>


<details id="2337. Move Pieces to Obtain a String">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">2337. Move Pieces to Obtain a String 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
Hint
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
        int i=0, j=0;
        int len=target.length();
        while(i<len || j<len){
            while(i<len && start.charAt(i)=='_')i++;
            while(j<len && target.charAt(j)=='_')j++;

            if(i == len || j==len){
                return i==len && j==len;
            }

            if(target.charAt(j)!=start.charAt(i))return false;

            if(target.charAt(j)=='R' && i>j)return false;

            if(target.charAt(j)=='L' && i<j)return false;
            i++;j++;
        }
        return true;
    }
}
```
</details>

<details id="1813. Sentence Similarity III">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1813. Sentence Similarity III 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
Hint
You are given two strings sentence1 and sentence2, each representing a sentence composed of words. A sentence is a list of words that are separated by a single space with no leading or trailing spaces. Each word consists of only uppercase and lowercase English characters.

Two sentences s1 and s2 are considered similar if it is possible to insert an arbitrary sentence (possibly empty) inside one of these sentences such that the two sentences become equal. Note that the inserted sentence must be separated from existing words by spaces.

For example,

s1 = "Hello Jane" and s2 = "Hello my name is Jane" can be made equal by inserting "my name is" between "Hello" and "Jane" in s1.
s1 = "Frog cool" and s2 = "Frogs are cool" are not similar, since although there is a sentence "s are" inserted into s1, it is not separated from "Frog" by a space.
Given two sentences sentence1 and sentence2, return true if sentence1 and sentence2 are similar. Otherwise, return false.

 

Example 1:

Input: sentence1 = "My name is Haley", sentence2 = "My Haley"

Output: true

Explanation:

sentence2 can be turned to sentence1 by inserting "name is" between "My" and "Haley".

Example 2:

Input: sentence1 = "of", sentence2 = "A lot of words"

Output: false

Explanation:

No single sentence can be inserted inside one of the sentences to make it equal to the other.

Example 3:

Input: sentence1 = "Eating right now", sentence2 = "Eating"

Output: true

Explanation:

sentence2 can be turned to sentence1 by inserting "right now" at the end of the sentence.

 

Constraints:

1 <= sentence1.length, sentence2.length <= 100
sentence1 and sentence2 consist of lowercase and uppercase English letters and spaces.
The words in sentence1 and sentence2 are separated by a single space.

```java

class Solution {
    public boolean areSentencesSimilar(String sentence1, String sentence2) {
        if(sentence1.length() < sentence2.length())return areSentencesSimilar(sentence2,sentence1);

        String[] s1=sentence1.split(" ");
        String[] s2=sentence2.split(" ");
        int i=0, j=s1.length-1;
        int k=0, l=s2.length-1;

        while(k<=l){
            if(s1[i].equals(s2[k])){
                i++;k++;
            }else if(s1[j].equals(s2[l])){
                j--;l--;
            }else return false;
        }
        return true;
    }
}
```
</details>

# Prefix Sum

### Prefix Sum

### Line Sweep


<details id="238. Product of Array Except Self">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">238. Product of Array Except Self 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
Hint
Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.

 

Example 1:

Input: nums = [1,2,3,4]
Output: [24,12,8,6]
Example 2:

Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
 

Constraints:

2 <= nums.length <= 105
-30 <= nums[i] <= 30
The input is generated such that answer[i] is guaranteed to fit in a 32-bit integer.
 

Follow up: Can you solve the problem in O(1) extra space complexity? (The output array does not count as extra space for space complexity analysis.)

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] prefix=new int[nums.length];
        int[] postfix=new int[nums.length];
        int[] ans=new int[nums.length];
        int prod=1;
        for(int i=0;i<nums.length;i++){
            prod*=nums[i];
            prefix[i]=prod;
        }
        prod=1;
        for(int i=nums.length-1;i>=0;i--){
            prod*=nums[i];
            postfix[i]=prod;
        }
        for(int i=0;i<ans.length;i++){
            if(i==0){
                ans[i]=postfix[i+1];
            }else if(i==ans.length-1){
                ans[i]=prefix[i-1];
            }else{
                ans[i]=prefix[i-1]*postfix[i+1];
            }
        }
        return ans;
    }
}
```
</details>

<details id="303. Range Sum Query - Immutable">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">303. Range Sum Query - Immutable 
</span>
</summary>

Easy
Topics
premium lock icon
Companies
Given an integer array nums, handle multiple queries of the following type:

Calculate the sum of the elements of nums between indices left and right inclusive where left <= right.
Implement the NumArray class:

NumArray(int[] nums) Initializes the object with the integer array nums.
int sumRange(int left, int right) Returns the sum of the elements of nums between indices left and right inclusive (i.e. nums[left] + nums[left + 1] + ... + nums[right]).
 

Example 1:

Input
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
Output
[null, 1, -1, -3]

Explanation
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
 

Constraints:

1 <= nums.length <= 104
-105 <= nums[i] <= 105
0 <= left <= right < nums.length
At most 104 calls will be made to sumRange.


```java
class NumArray {
    int[] nums;
    int[] prefixSum;
    public NumArray(int[] nums) {
        this.nums=nums;
        this.prefixSum=new int[nums.length];
        int sum=0;
        int index=0;
        for(int i:nums){
            sum+=i;
            prefixSum[index]=sum;
            index++;
        }

    }
    
    public int sumRange(int left, int right) {
        return left ==0 ? prefixSum[right]: prefixSum[right]-prefixSum[left-1];
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(left,right);
 */
```
</details>


<details id="1352. Product of the Last K Numbers">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1352. Product of the Last K Numbers 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
Hint
Design an algorithm that accepts a stream of integers and retrieves the product of the last k integers of the stream.

Implement the ProductOfNumbers class:

ProductOfNumbers() Initializes the object with an empty stream.
void add(int num) Appends the integer num to the stream.
int getProduct(int k) Returns the product of the last k numbers in the current list. You can assume that always the current list has at least k numbers.
The test cases are generated so that, at any time, the product of any contiguous sequence of numbers will fit into a single 32-bit integer without overflowing.

 

Example:

Input
["ProductOfNumbers","add","add","add","add","add","getProduct","getProduct","getProduct","add","getProduct"]
[[],[3],[0],[2],[5],[4],[2],[3],[4],[8],[2]]

Output
[null,null,null,null,null,null,20,40,0,null,32]

Explanation
ProductOfNumbers productOfNumbers = new ProductOfNumbers();
productOfNumbers.add(3);        // [3]
productOfNumbers.add(0);        // [3,0]
productOfNumbers.add(2);        // [3,0,2]
productOfNumbers.add(5);        // [3,0,2,5]
productOfNumbers.add(4);        // [3,0,2,5,4]
productOfNumbers.getProduct(2); // return 20. The product of the last 2 numbers is 5 * 4 = 20
productOfNumbers.getProduct(3); // return 40. The product of the last 3 numbers is 2 * 5 * 4 = 40
productOfNumbers.getProduct(4); // return 0. The product of the last 4 numbers is 0 * 2 * 5 * 4 = 0
productOfNumbers.add(8);        // [3,0,2,5,4,8]
productOfNumbers.getProduct(2); // return 32. The product of the last 2 numbers is 4 * 8 = 32 
 

Constraints:

0 <= num <= 100
1 <= k <= 4 * 104
At most 4 * 104 calls will be made to add and getProduct.
The product of the stream at any point in time will fit in a 32-bit integer.
 

Follow-up: Can you implement both GetProduct and Add to work in O(1) time complexity instead of O(k) time complexity?

```java
class ProductOfNumbers {
    List<Integer> prefixProd;
    public ProductOfNumbers() {
        prefixProd = new ArrayList<>();
    }

    public void add(int num) {
        if (num == 0) {
            prefixProd.clear();
        }else if (prefixProd.isEmpty()) {
            prefixProd.add(num);
        } else {
            prefixProd.add(prefixProd.get(prefixProd.size() - 1) * num);
        }
    }

    public int getProduct(int k) {
        if(k == prefixProd.size())return prefixProd.get(prefixProd.size()-1);
        if(k > prefixProd.size())return 0;
        return prefixProd.get(prefixProd.size()-1)/prefixProd.get(prefixProd.size()-1-k);
    }
}

/**
 * Your ProductOfNumbers object will be instantiated and called as such:
 * ProductOfNumbers obj = new ProductOfNumbers();
 * obj.add(num);
 * int param_2 = obj.getProduct(k);
 */

```
</details>

<details id="304. Range Sum Query 2D - Immutable">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">304. Range Sum Query 2D - Immutable 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
Given a 2D matrix matrix, handle multiple queries of the following type:

Calculate the sum of the elements of matrix inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).
Implement the NumMatrix class:

NumMatrix(int[][] matrix) Initializes the object with the integer matrix matrix.
int sumRegion(int row1, int col1, int row2, int col2) Returns the sum of the elements of matrix inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).
You must design an algorithm where sumRegion works on O(1) time complexity.

 

Example 1:


Input
["NumMatrix", "sumRegion", "sumRegion", "sumRegion"]
[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [1, 1, 2, 2], [1, 2, 2, 4]]
Output
[null, 8, 11, 12]

Explanation
NumMatrix numMatrix = new NumMatrix([[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]);
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (i.e sum of the red rectangle)
numMatrix.sumRegion(1, 1, 2, 2); // return 11 (i.e sum of the green rectangle)
numMatrix.sumRegion(1, 2, 2, 4); // return 12 (i.e sum of the blue rectangle)
 

Constraints:

m == matrix.length
n == matrix[i].length
1 <= m, n <= 200
-104 <= matrix[i][j] <= 104
0 <= row1 <= row2 < m
0 <= col1 <= col2 < n
At most 104 calls will be made to sumRegion.

```java
class NumMatrix {
    int[][] pmatrix;

    public NumMatrix(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        pmatrix = new int[m + 1][n + 1]; // 1-indexed to simplify logic

        for (int i = 0; i < m; i++) {
            int rowPrefix = 0; // resets every row
            for (int j = 0; j < n; j++) {
                rowPrefix += matrix[i][j]; 
                pmatrix[i + 1][j + 1] = pmatrix[i][j + 1] + rowPrefix;
            }
        }
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        return pmatrix[row2 + 1][col2 + 1]
             - pmatrix[row1][col2 + 1]
             - pmatrix[row2 + 1][col1]
             + pmatrix[row1][col1];
    }
}


```
</details>




# Graphs

### BFS/DFS

<details id="841. Keys and Rooms">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">841. Keys and Rooms 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
There are n rooms labeled from 0 to n - 1 and all the rooms are locked except for room 0. Your goal is to visit all the rooms. However, you cannot enter a locked room without having its key.

When you visit a room, you may find a set of distinct keys in it. Each key has a number on it, denoting which room it unlocks, and you can take all of them with you to unlock the other rooms.

Given an array rooms where rooms[i] is the set of keys that you can obtain if you visited room i, return true if you can visit all the rooms, or false otherwise.

 

Example 1:

Input: rooms = [[1],[2],[3],[]]
Output: true
Explanation: 
We visit room 0 and pick up key 1.
We then visit room 1 and pick up key 2.
We then visit room 2 and pick up key 3.
We then visit room 3.
Since we were able to visit every room, we return true.
Example 2:

Input: rooms = [[1,3],[3,0,1],[2],[0]]
Output: false
Explanation: We can not enter room number 2 since the only key that unlocks it is in that room.
 

Constraints:

n == rooms.length
2 <= n <= 1000
0 <= rooms[i].length <= 1000
1 <= sum(rooms[i].length) <= 3000
0 <= rooms[i][j] < n
All the values of rooms[i] are unique.

```java
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        int n = rooms.size();
        boolean[] visited = new boolean[n];
        dfs(rooms, visited, 0);
        for (boolean v : visited) {
            if (!v)return false; 
        }
        return true;
    }
    private void dfs(List<List<Integer>> rooms, boolean[] visited, int node){
        if(visited[node])return;
        visited[node]=true;
        for (Integer neighbor : rooms.get(node)) {
            if (visited[neighbor] != true) {
                dfs(rooms, visited, neighbor);
            }
        }
    }
}

```
</details>

<details id="547. Number of Provinces">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">547. Number of Provinces 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.


```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        //make an adj list
        List<List<Integer>> adj = new ArrayList<>();
        int r = isConnected.length;
        int c = isConnected[0].length;

        for (int i = 0; i < r; i++) {
            adj.add(new ArrayList<>());
        }
        for (int i = 0; i < r; i++) {
            for (int j = 0; j < c; j++) {
                if (isConnected[i][j] == 1) {
                    adj.get(i).add(j);
                }
            }
        }
        boolean[] visited = new boolean[r];
        int province = 0;
        for (int i = 0; i < r; i++) {
            if (!visited[i]) {
                province++;
                bfs(adj, i, visited);
            }
        }
        return province;
    }

    private void bfs(List<List<Integer>> adj, int node, boolean[] visited) {
        Queue<Integer> queue = new LinkedList<>();
        queue.add(node);
        visited[node] = true;

        while (!queue.isEmpty()) {
            int parent = queue.poll();

            for (Integer neighbor : adj.get(parent)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.add(neighbor);
                }
            }
        }
    }
}

```
</details>


<details id="1466. Reorder Routes to Make All Paths Lead to the City Zero">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1466. Reorder Routes to Make All Paths Lead to the City Zero 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
Hint
There are n cities numbered from 0 to n - 1 and n - 1 roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.

Roads are represented by connections where connections[i] = [ai, bi] represents a road from city ai to city bi.

This year, there will be a big event in the capital (city 0), and many people want to travel to this city.

Your task consists of reorienting some roads such that each city can visit the city 0. Return the minimum number of edges changed.

It's guaranteed that each city can reach city 0 after reorder.

    **Observation**: When they say that there are n nodes and n-1 edges and there only one path between any 2 nodes that tells that the graph does not have any loops and we have a connected graph (n nodes + n-1 edges + no loops = connected graph)

    **Why this ensures minimum flips**:
    1. We visit each city exactly once (DFS/BFS), we count each edge exactly once.

    2. We only flip edges that directly violate the “point to 0” condition.

    3. This gives the minimum flips, since flipping any fewer would leave at least one city unreachable from 0.


 ```java
// BFS
class Solution {
    public int minReorder(int n, int[][] connections) {
        //make adjcencey
        List<List<int[]>> adj = new ArrayList<>();
        int r = connections.length;
        int c = connections[0].length;

        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        // Build adjacency list with direction info
        for (int[] edge : connections) {
            int from = edge[0];
            int to = edge[1];
            adj.get(from).add(new int[] { to, 1 }); // 1 means needs reversal (from -> to)
            adj.get(to).add(new int[] { from, 0 }); // 0 means correct direction (to -> from)
        }

        // bfs traversal
        Queue<Integer> queue = new LinkedList<>();
        boolean[] visited = new boolean[n];
        queue.add(0);
        visited[0] = true;
        int reversal = 0;

        while (!queue.isEmpty()) {
            int node = queue.poll();
            for (int[] next : adj.get(node)) {
                int neigh = next[0];
                int needsReverse = next[1];
                if (!visited[neigh]) {
                    visited[neigh] = true;
                    reversal += needsReverse;
                    queue.add(neigh);
                }
            }
        }

        return reversal;
    }
}

 ```


 ```java
// DFS

class Solution {
    int changes = 0;

    public int minReorder(int n, int[][] connections) {
        List<List<int[]>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());

        // Build graph: store neighbor + direction
        for (int[] edge : connections) {
            graph.get(edge[0]).add(new int[]{edge[1], 1}); // original direction u->v
            graph.get(edge[1]).add(new int[]{edge[0], 0}); // reverse direction v->u
        }

        dfs(0, -1, graph);
        return changes;
    }

    private void dfs(int node, int parent, List<List<int[]>> graph) {
        for (int[] neighbor : graph.get(node)) {
            if (neighbor[0] == parent) continue; // don't revisit parent
            if (neighbor[1] == 1) changes++; // edge needs reversing
            dfs(neighbor[0], node, graph);
        }
    }
}

 ```
</details>

<details id="127. Word Ladder">
<summary> 
<span style="color:red;font-size:16px;font-weight:bold">127. Word Ladder 
</span>
</summary>

Hard
Topics
premium lock icon
Companies
A transformation sequence from word beginWord to word endWord using a dictionary wordList is a sequence of words beginWord -> s1 -> s2 -> ... -> sk such that:

Every adjacent pair of words differs by a single letter.
Every si for 1 <= i <= k is in wordList. Note that beginWord does not need to be in wordList.
sk == endWord
Given two words, beginWord and endWord, and a dictionary wordList, return the number of words in the shortest transformation sequence from beginWord to endWord, or 0 if no such sequence exists.

 

Example 1:

Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.
Example 2:

Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
 

Constraints:

1 <= beginWord.length <= 10
endWord.length == beginWord.length
1 <= wordList.length <= 5000
wordList[i].length == beginWord.length
beginWord, endWord, and wordList[i] consist of lowercase English letters.
beginWord != endWord
All the words in wordList are unique.


    Takeaways:
    1. Trying out all possible character: make a loop using  **for (char c = 'a'; c <= 'z'; c++)** and also usea character array to replace the chars like **chars[j] = c**
    2. Different positions for transations.

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord)) return 0; // endWord must be in wordList

        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);

        Set<String> visited = new HashSet<>();
        visited.add(beginWord);

        int transitions = 0; // starting from beginWord

        while (!queue.isEmpty()) {
            int levelSize = queue.size(); 
            transitions++;
            for (int i = 0; i < levelSize; i++) {
                String word = queue.poll();

                if (word.equals(endWord)) {
                    return transitions;
                }

                char[] chars = word.toCharArray();
                for (int j = 0; j < chars.length; j++) {
                    char original = chars[j];
                    for (char c = 'a'; c <= 'z'; c++) {
                        if (c == original) continue;

                        chars[j] = c;
                        String newWord = new String(chars);

                        if (wordSet.contains(newWord) && !visited.contains(newWord)) {
                            queue.add(newWord);
                            visited.add(newWord);
                        }
                    }
                    chars[j] = original; // restore
                }
            }
           
        }

        return 0; // no path found
    }
}
```
```java
changed positions of transition stills works fine

class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord))
            return 0; // endWord must be in wordList

        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);

        Set<String> visited = new HashSet<>();
        visited.add(beginWord);

        int transitions = 0; // starting from beginWord

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            transitions++;
            for (int i = 0; i < levelSize; i++) {
                String word = queue.poll();

                char[] chars = word.toCharArray();
                for (int j = 0; j < chars.length; j++) {
                    char original = chars[j];
                    for (char c = 'a'; c <= 'z'; c++) {
                        if (c == original)
                            continue;

                        chars[j] = c;
                        String newWord = new String(chars);
                        if (newWord.equals(endWord)) {
                            return transitions+1;
                        }
                        if (wordSet.contains(newWord) && !visited.contains(newWord)) {
                            queue.add(newWord);
                            visited.add(newWord);
                        }
                    }
                    chars[j] = original; // restore
                }
            }

        }

        return 0; // no path found
    }
}
```
```java
//this also works

class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord)) return 0; // endWord must be in wordList

        Queue<String> queue = new LinkedList<>();
        queue.add(beginWord);

        Set<String> visited = new HashSet<>();
        visited.add(beginWord);

        int transitions = 1; // starting from beginWord

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            for (int i = 0; i < levelSize; i++) {
                String word = queue.poll();

                if (word.equals(endWord)) {
                    return transitions;
                }

                char[] chars = word.toCharArray();
                for (int j = 0; j < chars.length; j++) {
                    char original = chars[j];
                    for (char c = 'a'; c <= 'z'; c++) {
                        if (c == original) continue;

                        chars[j] = c;
                        String newWord = new String(chars);

                        if (wordSet.contains(newWord) && !visited.contains(newWord)) {
                            queue.add(newWord);
                            visited.add(newWord);
                        }
                    }
                    chars[j] = original; // restore
                }
            }
            transitions++;
        }

        return 0; // no path found
    }
}


```
</details>


<details id="2998. Minimum Number of Operations to Make X and Y Equal">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">2998. Minimum Number of Operations to Make X and Y Equal 
</span>
</summary>
Medium
Topics
premium lock icon
Companies
Hint
You are given two positive integers x and y.

In one operation, you can do one of the four following operations:

Divide x by 11 if x is a multiple of 11.
Divide x by 5 if x is a multiple of 5.
Decrement x by 1.
Increment x by 1.
Return the minimum number of operations required to make x and y equal.

 

Example 1:

Input: x = 26, y = 1
Output: 3
Explanation: We can make 26 equal to 1 by applying the following operations: 
1. Decrement x by 1
2. Divide x by 5
3. Divide x by 5
It can be shown that 3 is the minimum number of operations required to make 26 equal to 1.
Example 2:

Input: x = 54, y = 2
Output: 4
Explanation: We can make 54 equal to 2 by applying the following operations: 
1. Increment x by 1
2. Divide x by 11 
3. Divide x by 5
4. Increment x by 1
It can be shown that 4 is the minimum number of operations required to make 54 equal to 2.
Example 3:

Input: x = 25, y = 30
Output: 5
Explanation: We can make 25 equal to 30 by applying the following operations: 
1. Increment x by 1
2. Increment x by 1
3. Increment x by 1
4. Increment x by 1
5. Increment x by 1
It can be shown that 5 is the minimum number of operations required to make 25 equal to 30.
 

Constraints:

1 <= x, y <= 104


```java
class Solution {
    public int minimumOperationsToMakeEqual(int x, int y) {
        if (x <= y) {
            return y - x;  // We can reach y simply by incrementing
        }

        Queue<Integer> queue = new ArrayDeque<>();
        Set<Integer> seen = new HashSet<>();
        queue.offer(x);

        int ans = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            
            for (int i = 0; i < size; i++) {
                int cur = queue.poll();
                if (cur == y) {
                    return ans;
                }
                if (seen.contains(cur)) continue;
                seen.add(cur);

                if (cur % 11 == 0) {
                    queue.offer(cur / 11);
                }
                if (cur % 5 == 0) {
                    queue.offer(cur / 5);
                }
                queue.offer(cur - 1);
                queue.offer(cur + 1);
            }
            ans++;
        }return -1;
    }
}

```


</details>

<details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>

https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/0

Undirected Graph Cycle
Difficulty: MediumAccuracy: 30.13%Submissions: 628K+Points: 4Average Time: 20m
Given an undirected graph with V vertices and E edges, represented as a 2D vector edges[][], where each entry edges[i] = [u, v] denotes an edge between vertices u and v, determine whether the graph contains a cycle or not. The graph can have multiple component.

Examples:

Input: V = 4, E = 4, edges[][] = [[0, 1], [0, 2], [1, 2], [2, 3]]
Output: true
Explanation: 
 
1 -> 2 -> 0 -> 1 is a cycle.
Input: V = 4, E = 3, edges[][] = [[0, 1], [1, 2], [2, 3]]
Output: false
Explanation: 
 
No cycle in the graph.
Constraints:
1 ≤ V ≤ 105
1 ≤ E = edges.size() ≤ 105

Expected Complexities

    Note: Here we dont need inRecursion to detect the back edge, since it is allowed in bi directional graph

```java
//correct code

class Solution {
    public boolean isCycle(int V, int[][] edges) {
        List<List<Integer>> adj=new ArrayList<>();
        boolean[] visited=new boolean[V];
        
        for(int i=0;i<V;i++){
            adj.add(new ArrayList<>());
        }
        
        for(int[] edge:edges){
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }
        
        // checking cycle with connected components
        for(int i=0;i<V;i++){
            if(!visited[i]){

                // return dfs(adj, visited,-1, i); // This is wrong because this will return false once it check 1st node, we need to keep on checking as we get false
                if(dfs(adj, visited,-1, i)){
                    return true;
                }
            }
        }
        return false;
    }
    
    private boolean dfs(List<List<Integer>> adj, boolean[] visited, int parent, int node){
        visited[node]=true;
        
        for(int neighbor: adj.get(node)){
            if(neighbor == parent)continue;
            if(visited[neighbor])return true;
            if(dfs(adj, visited, inRecursion,node, neighbor)){
                return true;
            }
        }
        return false;
    }
}
```
</details>

<details id="Directed Graph Cycle">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Directed Graph Cycle 
</span>
</summary>

```java
class Solution {
    public boolean isCyclic(int V, int[][] edges) {
        // code here
        List<List<Integer>> adj=new ArrayList<>();
        
        for(int i=0;i<V;i++){
            adj.add(new ArrayList<>());
        }
        
        for(int[] edge:edges){
            adj.get(edge[0]).add(edge[1]);
        }
        
        boolean[] visited=new boolean[V];
        boolean[] inRecursion=new boolean[V];
        
        
        for(int i=0;i<V;i++){
            if(!visited[i]){
                if(dfs(adj, i, visited, inRecursion)){
                    return true;
                }
            }
        }
        return false;
    }
    
    private boolean dfs(List<List<Integer>> adj, int node, boolean[] visited, boolean[] inRecursion){
        visited[node]=true;
        inRecursion[node]=true;
        
        for(int neighbor:adj.get(node)){
            if(!visited[neighbor]){
                if(dfs(adj, neighbor, visited, inRecursion)){
                    return true;
                }
            }else{
                if(inRecursion[neighbor]){
                    return true;
                }
            }
        }
        inRecursion[node]=false;
        return false;
    }
}
```
</details>

<details id="1559. Detect Cycles in 2D Grid">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1559. Detect Cycles in 2D Grid 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
Hint
Given a 2D array of characters grid of size m x n, you need to find if there exists any cycle consisting of the same value in grid.

A cycle is a path of length 4 or more in the grid that starts and ends at the same cell. From a given cell, you can move to one of the cells adjacent to it - in one of the four directions (up, down, left, or right), if it has the same value of the current cell.

Also, you cannot move to the cell that you visited in your last move. For example, the cycle (1, 1) -> (1, 2) -> (1, 1) is invalid because from (1, 2) we visited (1, 1) which was the last visited cell.

Return true if any cycle of the same value exists in grid, otherwise, return false.

 

Example 1:



Input: grid = [["a","a","a","a"],["a","b","b","a"],["a","b","b","a"],["a","a","a","a"]]
Output: true
Explanation: There are two valid cycles shown in different colors in the image below:

Example 2:



Input: grid = [["c","c","c","a"],["c","d","c","c"],["c","c","e","c"],["f","c","c","c"]]
Output: true
Explanation: There is only one valid cycle highlighted in the image below:

Example 3:



Input: grid = [["a","b","b"],["b","z","b"],["b","b","a"]]
Output: false
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 500
grid consists only of lowercase English letters.

```java
class Solution {
    public boolean containsCycle(char[][] grid) {
        int row = grid.length;
        int col = grid[0].length;
        boolean[][] visited = new boolean[row][col];

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (!visited[i][j]) {
                    if (dfs(grid, i, j, -1, -1, visited, row, col)) {
                        return true;
                    }
                }
            }
        }
        return false;
    }

    private boolean dfs(char[][] grid, int i, int j, int pi, int pj, boolean[][] visited, int row, int col) {
        visited[i][j] = true;
        int[][] directions = new int[][] { { 1, 0 }, { -1, 0 }, { 0, 1 }, { 0, -1 } };

        for (int[] dir : directions) {
            int x = i + dir[0];
            int y = j + dir[1];
            if (x < row && y < col && x >= 0 && y >= 0 && grid[x][y] == grid[i][j]) {
                if (x == pi && y == pj)
                    continue;
                if (visited[x][y])
                    return true;
                if (dfs(grid, x, y, i, j, visited, row, col)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```
</details>


<details id="2608. Shortest Cycle in a Graph">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">2608. Shortest Cycle in a Graph 
</span>
</summary>

Hard
Topics
premium lock icon
Companies
Hint
There is a bi-directional graph with n vertices, where each vertex is labeled from 0 to n - 1. The edges in the graph are represented by a given 2D integer array edges, where edges[i] = [ui, vi] denotes an edge between vertex ui and vertex vi. Every vertex pair is connected by at most one edge, and no vertex has an edge to itself.

Return the length of the shortest cycle in the graph. If no cycle exists, return -1.

A cycle is a path that starts and ends at the same node, and each edge in the path is used only once.

 

Example 1:


Input: n = 7, edges = [[0,1],[1,2],[2,0],[3,4],[4,5],[5,6],[6,3]]
Output: 3
Explanation: The cycle with the smallest length is : 0 -> 1 -> 2 -> 0 
Example 2:


Input: n = 4, edges = [[0,1],[0,2]]
Output: -1
Explanation: There are no cycles in this graph.
 

Constraints:

2 <= n <= 1000
1 <= edges.length <= 1000
edges[i].length == 2
0 <= ui, vi < n
ui != vi
There are no repeated edges.

```java
class Solution {
    public int findShortestCycle(int n, int[][] edges) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
        for (int[] e : edges) {
            adj.get(e[0]).add(e[1]);
            adj.get(e[1]).add(e[0]);
        }

        int ans = Integer.MAX_VALUE;

        for (int start = 0; start < n; start++) {
            int[] dist = new int[n];
            Arrays.fill(dist, -1);
            Queue<Integer> q = new LinkedList<>();

            dist[start] = 0;
            q.offer(start);

            while (!q.isEmpty()) {
                int u = q.poll();
                for (int v : adj.get(u)) {
                    if (dist[v] == -1) { // not visited
                        dist[v] = dist[u] + 1; // 
                        q.offer(v);
                    } else if (dist[v] >= dist[u]) { // smaller values dipicts that node was visited earlier than v
                        // found a cycle
                        ans = Math.min(ans, dist[v] + dist[u] + 1); // dist[v]-> dist between root node and v similarly dist[u] + dist between u-v which is 1
                    }
                }
            }
        }

        return ans == Integer.MAX_VALUE ? -1 : ans;
    }
}

```

✅ How it works

    For each node, run BFS to compute shortest distances.

    If an edge leads to an already visited node (but not the direct parent), it forms a cycle.

    Cycle length = dist[u] + dist[v] + 1.

    Keep track of the minimum across all BFS runs.

⚡ Complexity:

    Time: O(V * (V + E))

    Space: O(V + E)
</details>

<details id="785. Is Graph Bipartite?">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">785. Is Graph Bipartite? 
</span>
</summary>

Medium
Topics
premium lock icon
Companies
There is an undirected graph with n nodes, where each node is numbered between 0 and n - 1. You are given a 2D array graph, where graph[u] is an array of nodes that node u is adjacent to. More formally, for each v in graph[u], there is an undirected edge between node u and node v. The graph has the following properties:

There are no self-edges (graph[u] does not contain u).
There are no parallel edges (graph[u] does not contain duplicate values).
If v is in graph[u], then u is in graph[v] (the graph is undirected).
The graph may not be connected, meaning there may be two nodes u and v such that there is no path between them.
A graph is bipartite if the nodes can be partitioned into two independent sets A and B such that every edge in the graph connects a node in set A and a node in set B.

Return true if and only if it is bipartite.

 

Example 1:


Input: graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
Output: false
Explanation: There is no way to partition the nodes into two independent sets such that every edge connects a node in one and a node in the other.
Example 2:


Input: graph = [[1,3],[0,2],[1,3],[0,2]]
Output: true
Explanation: We can partition the nodes into two sets: {0, 2} and {1, 3}.
 

Constraints:

graph.length == n
1 <= n <= 100
0 <= graph[u].length < n
0 <= graph[u][i] <= n - 1
graph[u] does not contain u.
All the values of graph[u] are unique.
If graph[u] contains v, then graph[v] contains u.

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int v=graph.length;
        int[] colors=new int[v];
        Arrays.fill(colors, -1);

        for(int i=0;i<v;i++){
            if(colors[i] == -1 & isGraphBipertite(graph, i, -1, colors, 0)){
                return false;
            }
        }
        return true;
    }

    private boolean isGraphBipertite(int[][] graph, int node, int parent, int[] colors, int color){
        colors[node]=color;

        for(int neighbor: graph[node]){
            if(neighbor == parent)continue;
            if(colors[neighbor] == -1 && isGraphBipertite(graph, neighbor, node, colors, 1-color)){
                return true;
            }
            if(colors[neighbor] == color){
                return true;
            }
        }
        return false;
    }
}
```


🔹 Total Time Complexity

    Let:

    V = number of vertices

    E = number of edges

    Visiting all vertices: O(V)

    Traversing all edges in adjacency lists: O(E)

    ✅ So total time complexity:

    O(V+E)
	​

🔹 Space Complexity

    colors[] → O(V)

    Recursion stack in DFS → O(V) in worst case (linear chain)

    ✅ So space complexity:

    O(V)
	​

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