DP

1. 1D DP
   1. Longest Increasing subsequesnce(LIS)
2. 2D DP
3. String based
   1. Longest Common DSubsequesnce(LCS)
   2. Print LCS
   3. Edit Distance
   4. Shortest common Supersequence(SCS)
   5. Print SCS
   6. Pallindrome related DP problems
      1. Plandromic Substrings + Blueprint
      2. Longest Plandromic Substring
      3. Longest Pallindromic Subsequence
      4. Pallindrome Partitioning - 1
      5. Pallindrome Partitioning - 2
4. Grid based
5. GameStrategy



# 1-d DP



<details id="70. Climbing Stairs">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">70. Climbing Stairs 
</span></summary>

https://leetcode.com/problems/climbing-stairs/


You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

 

Example 1:

Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
Example 2:

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
 

Constraints:

1 <= n <= 45


```java
// Bottom-up
class Solution {

    public int climbStairs(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;

        for (int i = 2; i < n + 1; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
}

```
```java
// Top down with memo[]
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, -1);
        return recursivelyClimb(0, 0, n, dp);
    }

    private int recursivelyClimb(int currStep, int totalSteps, int target, int[] dp) {
        // base
        if (currStep == target)
            return 1;
        if (currStep > target)
            return 0;
        if (dp[currStep] != -1)
            return dp[currStep];

        int singleStep = recursivelyClimb(currStep + 1, totalSteps + 1, target, dp);
        int doubleStep = recursivelyClimb(currStep + 2, totalSteps + 2, target, dp);

        return dp[currStep] = singleStep + doubleStep;
    }
}
```
</details>



<details id="198. House Robber">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">198. House Robber 
</span>
</summary>

https://leetcode.com/problems/house-robber/description/

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 

Example 1:

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
Example 2:

Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
 

Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 400


```java
// Recursion + memo

class Solution {
    public int rob(int[] nums) {
        int[] dp = new int[nums.length];
        Arrays.fill(dp, -1);
        return houserRobber(nums, 0, dp);
    }

    private int houserRobber(int[] nums, int index, int[] dp) {
        if (index >= nums.length)
            return 0;
        if (dp[index] != -1)
            return dp[index];

        int rob = nums[index] + houserRobber(nums, index + 2, dp);
        int skip = houserRobber(nums, index + 1, dp);
        return dp[index] = Math.max(rob, skip);
    }
}
```
</details>

<details id="300. Longest Increasing Subsequence">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">300. Longest Increasing Subsequence 
</span>
</summary>

https://leetcode.com/problems/longest-increasing-subsequence/description/


Given an integer array nums, return the length of the longest strictly increasing 
subsequence
.

 

Example 1:

Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
Example 2:

Input: nums = [0,1,0,3,2,3]
Output: 4
Example 3:

Input: nums = [7,7,7,7,7,7,7]
Output: 1
 

Constraints:

1 <= nums.length <= 2500
-104 <= nums[i] <= 104
 

Follow up: Can you come up with an algorithm that runs in O(n log(n)) time complexity?


```java
//Approach-1 (TopDown: Recur+Memo) 
//T.C : O(n*n)
class Solution {
    private int n;
    private int[][] t;

    public int lis(int[] nums, int prevIdx, int currIdx) {
        if (currIdx == n)
            return 0;

        if (prevIdx != -1 && t[prevIdx][currIdx] != -1)
            return t[prevIdx][currIdx];

        int taken = 0;
        if (prevIdx == -1 || nums[currIdx] > nums[prevIdx])
            taken = 1 + lis(nums, currIdx, currIdx + 1);

        int notTaken = lis(nums, prevIdx, currIdx + 1);

        if (prevIdx != -1)
            t[prevIdx][currIdx] = Math.max(taken, notTaken);

        return Math.max(taken, notTaken);
    }

    public int lengthOfLIS(int[] nums) {
        t = new int[2501][2501];
        for (int[] row : t) {
            Arrays.fill(row, -1);
        }

        n = nums.length;
        return lis(nums, -1, 0);
    }
}


//Approach-2 (Bottom Up)
//T.C : O(n^2)
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;

        int[] t = new int[n];
        Arrays.fill(t,1);
    
        int maxLIS = 1;
        
        for(int i = 1; i < n; i++){
            for(int j = 0; j < i; j++){
                if(nums[j] < nums[i]) {
                    t[i] = Math.max(t[i], t[j] + 1);
                    maxLIS = Math.max(maxLIS, t[i]);
                }
            }
        }

        return maxLIS;
    }
}


//Approacj-4 (Using concept of Patience Sorting (O(nlogn))
//T.C : O(nlogn)
//S.C : O(n)
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        List<Integer> sorted = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            /*
                Why lower bound?
                We want an increasing subsequence, and hence
                we want to eliminate the duplicates as well.
                lower_bound returns the index of "next greater or equal to."
            */
            int index = binarySearch(sorted, nums[i]);

            if (index == sorted.size())
                sorted.add(nums[i]); // greatest: so insert it
            else
                sorted.set(index, nums[i]); // replace
        }

        return sorted.size();
    }

    private int binarySearch(List<Integer> sorted, int target) {
        int left = 0, right = sorted.size();
        int result = sorted.size();
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (sorted.get(mid) < target) {
                left = mid + 1;
            } else {
                result = mid;
                right = mid;
            }
        }
        return result;
    }
}

	
//Approach-5 (Using same code of Leetcode-2926(Maximum Balanced Subsequence Sum) (YouTube - https://www.youtube.com/watch?v=JrG4tbq6efg)
//T.C : O(nlogn)
//S.C : O(n)
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        TreeMap<Integer, Integer> mp = new TreeMap<>();
        int ans = 0;

        for (int i = 0; i < n; i++) {
            int len = 1;

            Integer key = mp.lowerKey(nums[i]);
            if (key != null) {
                len += mp.get(key);
            }

            mp.put(nums[i], Math.max(mp.getOrDefault(nums[i], 0), len));

            key = mp.higherKey(nums[i]);
            while (key != null && mp.get(key) <= len) {
                mp.remove(key);
                key = mp.higherKey(nums[i]);
            }

            ans = Math.max(ans, len);
        }

        return ans;
    }
}

```


</details>

<details id="646. Maximum Length of Pair Chain">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">646. Maximum Length of Pair Chain 
</span>
</summary>

https://leetcode.com/problems/maximum-length-of-pair-chain/description/


You are given an array of n pairs pairs where pairs[i] = [lefti, righti] and lefti < righti.

A pair p2 = [c, d] follows a pair p1 = [a, b] if b < c. A chain of pairs can be formed in this fashion.

Return the length longest chain which can be formed.

You do not need to use up all the given intervals. You can select pairs in any order.

 

Example 1:

Input: pairs = [[1,2],[2,3],[3,4]]
Output: 2
Explanation: The longest chain is [1,2] -> [3,4].
Example 2:

Input: pairs = [[1,2],[7,8],[4,5]]
Output: 3
Explanation: The longest chain is [1,2] -> [4,5] -> [7,8].
 

Constraints:

n == pairs.length
1 <= n <= 1000
-1000 <= lefti < righti <= 1000

```java
// Recursive + momo

class Solution {
    private int n;
    private int[][] memo = new int[1001][1001];

    private int lis(List<int[]> pairs, int prevIdx, int currIdx) {
        if (currIdx == n) return 0;

        if (prevIdx != -1 && memo[prevIdx][currIdx] != -1) {
            return memo[prevIdx][currIdx];
        }

        int taken = 0;
        if (prevIdx == -1 || pairs.get(currIdx)[0] > pairs.get(prevIdx)[1]) {
            taken = 1 + lis(pairs, currIdx, currIdx + 1);
        }

        int notTaken = lis(pairs, prevIdx, currIdx + 1);

        if (prevIdx != -1) {
            memo[prevIdx][currIdx] = Math.max(taken, notTaken);
        }

        return Math.max(taken, notTaken);
    }

    public int findLongestChain(int[][] pairs) {
        for (int[] row : memo) {
            Arrays.fill(row, -1);
        }
        n = pairs.length;
        Arrays.sort(pairs, (a, b) -> Integer.compare(a[0], b[0])); // Sorting pairs by first element
        return lis(Arrays.asList(pairs), -1, 0);
    }
}






// Bottomup

import java.util.Arrays;

class Solution {
    public int findLongestChain(int[][] pairs) {
        int n = pairs.length;
        Arrays.sort(pairs, (a, b) -> Integer.compare(a[0], b[0])); // Sorting pairs by first element
        
        int[] dp = new int[n];
        Arrays.fill(dp, 1); // Each pair can at least form a chain of length 1
        int maxLen = 1;

        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (pairs[j][1] < pairs[i][0]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                    maxLen = Math.max(maxLen, dp[i]);
                }
            }
        }

        return maxLen;
    }
}


```

</details>





<details id="1048. Longest String Chain">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1048. Longest String Chain 
</span>
</summary>

https://leetcode.com/problems/longest-string-chain/description/

You are given an array of words where each word consists of lowercase English letters.

wordA is a predecessor of wordB if and only if we can insert exactly one letter anywhere in wordA without changing the order of the other characters to make it equal to wordB.

For example, "abc" is a predecessor of "abac", while "cba" is not a predecessor of "bcad".
A word chain is a sequence of words [word1, word2, ..., wordk] with k >= 1, where word1 is a predecessor of word2, word2 is a predecessor of word3, and so on. A single word is trivially a word chain with k == 1.

Return the length of the longest possible word chain with words chosen from the given list of words.

 

Example 1:

Input: words = ["a","b","ba","bca","bda","bdca"]
Output: 4
Explanation: One of the longest word chains is ["a","ba","bda","bdca"].
Example 2:

Input: words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
Output: 5
Explanation: All the words can be put in a word chain ["xb", "xbc", "cxbc", "pcxbc", "pcxbcf"].
Example 3:

Input: words = ["abcd","dbqca"]
Output: 1
Explanation: The trivial word chain ["abcd"] is one of the longest word chains.
["abcd","dbqca"] is not a valid word chain because the ordering of the letters is changed.
 

Constraints:

1 <= words.length <= 1000
1 <= words[i].length <= 16
words[i] only consists of lowercase English letters.

```java
//Approach-1 (Using Simple LIS recursion+memo) - We sort it in the beginning to get words ordered in ascending order based on length
//T.C : O(n*n*n)
class Solution {
    int n;
    int[][] t = new int[1001][1001];

    boolean predecessor(String prev, String curr) {
        int M = prev.length();
        int N = curr.length();

        if (M >= N || N - M != 1)
            return false;

        int i = 0, j = 0;
        while (i < M && j < N) {
            if (prev.charAt(i) == curr.charAt(j)) {
                i++;
            }
            j++;
        }
        return i == M;
    }

    int lis(String[] words, int prevIdx, int currIdx) {
        if (currIdx == n)
            return 0;

        if (prevIdx != -1 && t[prevIdx][currIdx] != -1)
            return t[prevIdx][currIdx];

        int taken = 0;
        if (prevIdx == -1 || predecessor(words[prevIdx], words[currIdx]))
            taken = 1 + lis(words, currIdx, currIdx + 1);

        int notTaken = lis(words, prevIdx, currIdx + 1);

        if (prevIdx != -1)
            t[prevIdx][currIdx] = Math.max(taken, notTaken);

        return Math.max(taken, notTaken);
    }

    int longestStrChain(String[] words) {
        for(int i = 0; i < 1000; i++) {
            for(int j = 0; j < 1000; j++) {
                t[i][j] = -1;
            }
        }
        
        n = words.length;
        Arrays.sort(words, (s1, s2) -> Integer.compare(s1.length(), s2.length())); 
        // You can select pairs in any order.
        return lis(words, -1, 0);
    }
}

//Approach-2 (Using Simple LIS Bottom Up) - We sort it in the beginning to get words ordered in ascending order based on length
//T.C : O(n*n*n)
class Solution {

    public int longestStrChain(String[] words) {
        int n = words.length;
        Arrays.sort(words, (s1, s2) -> Integer.compare(s1.length(), s2.length()));

        int[] t = new int[n];
        Arrays.fill(t, 1);
        int maxL = 1;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {

                if (predecessor(words[j], words[i])) {
                    t[i] = Math.max(t[i], t[j] + 1);
                    maxL = Math.max(maxL, t[i]);
                }
            }
        }

        return maxL;
    }
        
    public boolean predecessor(String prev, String curr) {
        int M = prev.length();
        int N = curr.length();

        if (M >= N || N - M != 1)
            return false;

        int i = 0, j = 0;
        while (i < M && j < N) {
            if (prev.charAt(i) == curr.charAt(j)) {
                i++;
            }
            j++;
        }
        return i == M;
    }

}

```

</details>


<details id="1420. Build Array Where You Can Find The Maximum Exactly K Comparisons">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1420. Build Array Where You Can Find The Maximum Exactly K Comparisons 
</span>
</summary>
</details>







<details id="2926. Maximum Balanced Subsequence Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">2926. Maximum Balanced Subsequence Sum 
</span>
</summary>


https://leetcode.com/problems/maximum-balanced-subsequence-sum/description/


You are given a 0-indexed integer array nums.

A subsequence of nums having length k and consisting of indices i0 < i1 < ... < ik-1 is balanced if the following holds:

nums[ij] - nums[ij-1] >= ij - ij-1, for every j in the range [1, k - 1].
A subsequence of nums having length 1 is considered balanced.

Return an integer denoting the maximum possible sum of elements in a balanced subsequence of nums.

A subsequence of an array is a new non-empty array that is formed from the original array by deleting some (possibly none) of the elements without disturbing the relative positions of the remaining elements.

 

Example 1:

Input: nums = [3,3,5,6]
Output: 14
Explanation: In this example, the subsequence [3,5,6] consisting of indices 0, 2, and 3 can be selected.
nums[2] - nums[0] >= 2 - 0.
nums[3] - nums[2] >= 3 - 2.
Hence, it is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums.
The subsequence consisting of indices 1, 2, and 3 is also valid.
It can be shown that it is not possible to get a balanced subsequence with a sum greater than 14.
Example 2:

Input: nums = [5,-1,-3,8]
Output: 13
Explanation: In this example, the subsequence [5,8] consisting of indices 0 and 3 can be selected.
nums[3] - nums[0] >= 3 - 0.
Hence, it is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums.
It can be shown that it is not possible to get a balanced subsequence with a sum greater than 13.
Example 3:

Input: nums = [-2,-1]
Output: -1
Explanation: In this example, the subsequence [-1] can be selected.
It is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums.



```java
//Approach-1 (Using LIS) - Recursion (TLE) ---> 316 / 345 testcases passed
//T.C : O(n^2) - prev index for every i
public class Solution {
    private Map<String, Long> memo = new HashMap<>();

    public long solve(int i, int prev, int[] nums) {
        if (i >= nums.length) {
            return 0;
        }

        String key = i + "_" + prev;
        if (memo.containsKey(key)) {
            return memo.get(key);
        }

        long taken = Integer.MIN_VALUE;

        if (prev == -1 || nums[i] - i >= nums[prev] - prev) {
            taken = nums[i] + solve(i + 1, i, nums);
        }

        long notTaken = solve(i + 1, prev, nums);
        long result = Math.max(taken, notTaken);
        memo.put(key, result);

        return result;
    }

    public long maxBalancedSubsequenceSum(int[] nums) {
        boolean allNegative = true;
        long maxEl = Integer.MIN_VALUE;
        memo.clear();

        for (int x : nums) {
            maxEl = Math.max(maxEl, x);
            if (x >= 0) {
                allNegative = false;
            }
        }

        if (allNegative) {
            return maxEl;
        }

        return solve(0, -1, nums);
    }
}



//Approach-2 (Using LIS Bottom Up) - TLE (341/345 Test cases passed)
//Time : O(n^2)
class Solution {
    public long maxBalancedSubsequenceSum(int[] nums) {
        int n = nums.length;
        
        int maxEl = Arrays.stream(nums).max().getAsInt();
        if(maxEl <= 0) {
            return maxEl;
        }

        long[] t = new long[n];
        for(int i = 0; i < n; i++) {
            t[i] = nums[i];
        }

        long maxSum = Integer.MIN_VALUE;
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < i; j++) {
                if(nums[i] - i >= nums[j] - j) {
                    t[i] = Math.max(t[i], t[j] + nums[i]);
                    maxSum = Math.max(maxSum, t[i]);
                }
            }
        }

        return maxSum > maxEl ? maxSum : maxEl;
    }
}


//Approach-3 (Using Optimal LIS - Similar to Patience Sorting) - Accepted
//Time : O(nlogn)
class Solution {
    public long maxBalancedSubsequenceSum(int[] nums) {
        int n = nums.length;
        int [] arr = new int[n];
        for(int i = 0; i<n; i++){
            arr[i] = nums[i]-i;
        }
        TreeMap<Integer, Long> map = new TreeMap<>();
        long ans = Integer.MIN_VALUE;
        for(int i = 0; i<n; i++){
            if(nums[i]<=0){
                ans = Math.max(ans, nums[i]);
            }
            else{
                long temp = nums[i];
                if(map.floorKey(arr[i])!=null){
                    temp += map.get(map.floorKey(arr[i]));
                }
                while(map.ceilingKey(arr[i])!=null && map.get(map.ceilingKey(arr[i]))<temp){
                    map.remove(map.ceilingKey(arr[i]));
                }
                if(map.floorKey(arr[i])==null || map.get(map.floorKey(arr[i]))<temp){
                    map.put(arr[i], temp);
                }
                ans = Math.max(ans, temp);
            }
        }
        return ans;
    }
}

```
</details>








<details id="300. Longest Increasing Subsequence">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">300. Longest Increasing Subsequence 
</span>
</summary>

https://leetcode.com/problems/longest-increasing-subsequence/description/

Given an integer array nums, return the length of the longest strictly increasing 
subsequence
.

 

Example 1:

Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
Example 2:

Input: nums = [0,1,0,3,2,3]
Output: 4
Example 3:

Input: nums = [7,7,7,7,7,7,7]
Output: 1
 

Constraints:

1 <= nums.length <= 2500
-104 <= nums[i] <= 104
 

Follow up: Can you come up with an algorithm that runs in O(n log(n)) time complexity?

```java
//Approach-1 (TopDown: Recur+Memo) 
//T.C : O(n*n)
class Solution {
    private int n;
    private int[][] t;

    public int lis(int[] nums, int prevIdx, int currIdx) {
        if (currIdx == n)
            return 0;

        if (prevIdx != -1 && t[prevIdx][currIdx] != -1)
            return t[prevIdx][currIdx];

        int taken = 0;
        if (prevIdx == -1 || nums[currIdx] > nums[prevIdx])
            taken = 1 + lis(nums, currIdx, currIdx + 1);

        int notTaken = lis(nums, prevIdx, currIdx + 1);

        if (prevIdx != -1)
            t[prevIdx][currIdx] = Math.max(taken, notTaken);

        return Math.max(taken, notTaken);
    }

    public int lengthOfLIS(int[] nums) {
        t = new int[2501][2501];
        for (int[] row : t) {
            Arrays.fill(row, -1);
        }

        n = nums.length;
        return lis(nums, -1, 0);
    }
}


//Approach-2 (Bottom Up)
//T.C : O(n^2)
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;

        int[] t = new int[n];
        Arrays.fill(t,1);
    
        int maxLIS = 1;
        
        for(int i = 1; i < n; i++){
            for(int j = 0; j < i; j++){
                if(nums[j] < nums[i]) {
                    t[i] = Math.max(t[i], t[j] + 1);
                    maxLIS = Math.max(maxLIS, t[i]);
                }
            }
        }

        return maxLIS;
    }
}


//Approacj-4 (Using concept of Patience Sorting (O(nlogn))
//T.C : O(nlogn)
//S.C : O(n)
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        List<Integer> sorted = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            /*
                Why lower bound?
                We want an increasing subsequence, and hence
                we want to eliminate the duplicates as well.
                lower_bound returns the index of "next greater or equal to."
            */
            int index = binarySearch(sorted, nums[i]);

            if (index == sorted.size())
                sorted.add(nums[i]); // greatest: so insert it
            else
                sorted.set(index, nums[i]); // replace
        }

        return sorted.size();
    }

    private int binarySearch(List<Integer> sorted, int target) {
        int left = 0, right = sorted.size();
        int result = sorted.size();
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (sorted.get(mid) < target) {
                left = mid + 1;
            } else {
                result = mid;
                right = mid;
            }
        }
        return result;
    }
}

	
//Approach-5 (Using same code of Leetcode-2926(Maximum Balanced Subsequence Sum) (YouTube - https://www.youtube.com/watch?v=JrG4tbq6efg)
//T.C : O(nlogn)
//S.C : O(n)
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        TreeMap<Integer, Integer> mp = new TreeMap<>();
        int ans = 0;

        for (int i = 0; i < n; i++) {
            int len = 1;

            Integer key = mp.lowerKey(nums[i]);
            if (key != null) {
                len += mp.get(key);
            }

            mp.put(nums[i], Math.max(mp.getOrDefault(nums[i], 0), len));

            key = mp.higherKey(nums[i]);
            while (key != null && mp.get(key) <= len) {
                mp.remove(key);
                key = mp.higherKey(nums[i]);
            }

            ans = Math.max(ans, len);
        }

        return ans;
    }
}
```
</details>







<details id="368. Largest Divisible Subset">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">368. Largest Divisible Subset 
</span>
</summary>

https://leetcode.com/problems/largest-divisible-subset/description/


Given a set of distinct positive integers nums, return the largest subset answer such that every pair (answer[i], answer[j]) of elements in this subset satisfies:

answer[i] % answer[j] == 0, or
answer[j] % answer[i] == 0
If there are multiple solutions, return any of them.

 

Example 1:

Input: nums = [1,2,3]
Output: [1,2]
Explanation: [1,3] is also accepted.
Example 2:

Input: nums = [1,2,4,8]
Output: [1,2,4,8]
 

Constraints:

1 <= nums.length <= 1000
1 <= nums[i] <= 2 * 109
All the integers in nums are unique.

```java
//Approach-1 (Using Brute Force same as LIS)
//T.C : O(2^n) without memoization
//S.C : O(n)
public class Solution {

    public List<Integer> largestDivisibleSubset(int[] nums) {
        Arrays.sort(nums);

        List<Integer> result = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();

        generate(0, nums, result, temp, -1);

        return result;
    }

    private void generate(int idx, int[] nums, List<Integer> result, List<Integer> temp, int prev) {
        if (idx >= nums.length) {
            if (temp.size() > result.size()) {
                result.clear();
                result.addAll(temp);
            }
            return;
        }

        if (prev == -1 || nums[idx] % prev == 0) {
            temp.add(nums[idx]);
            generate(idx + 1, nums, result, temp, nums[idx]);
            temp.remove(temp.size() - 1);
        }

        generate(idx + 1, nums, result, temp, prev);
    }
}


//Approach-2 (Using Bottom Up same as LIS) - Just need to keep track of how to print LIS
//T.C : O(n^2)
//S.C : O(n)
public class Solution {

    public List<Integer> largestDivisibleSubset(int[] nums) {
        Arrays.sort(nums);

        int n = nums.length;
        int[] t = new int[n];
        Arrays.fill(t, 1);

        int[] prevIdx = new int[n];
        Arrays.fill(prevIdx, -1);

        int lastChosenIndex = 0;
        int maxL = 1;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] % nums[j] == 0) {
                    if (t[i] < t[j] + 1) {
                        t[i] = t[j] + 1;
                        prevIdx[i] = j;
                    }

                    if (t[i] > maxL) {
                        maxL = t[i];
                        lastChosenIndex = i;
                    }
                }
            }
        }

        List<Integer> result = new ArrayList<>();
        while (lastChosenIndex >= 0) {
            result.add(nums[lastChosenIndex]);
            lastChosenIndex = prevIdx[lastChosenIndex];
        }

        return result;
    }
}

```

</details>

# 2-d DP





<details id="1277. Count Square Submatrices with All Ones">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1277. Count Square Submatrices with All Ones 
</span>
</summary>

https://leetcode.com/problems/count-square-submatrices-with-all-ones/description/?envType=daily-question&envId=2024-10-27

    Given a m * n matrix of ones and zeros, return how many square submatrices have all ones.

    

    Example 1:

    Input: matrix =
    [
    [0,1,1,1],
    [1,1,1,1],
    [0,1,1,1]
    ]
    Output: 15
    Explanation: 
    There are 10 squares of side 1.
    There are 4 squares of side 2.
    There is  1 square of side 3.
    Total number of squares = 10 + 4 + 1 = 15.
    Example 2:

    Input: matrix = 
    [
    [1,0,1],
    [1,1,0],
    [1,1,0]
    ]
    Output: 7
    Explanation: 
    There are 6 squares of side 1.  
    There is 1 square of side 2. 
    Total number of squares = 6 + 1 = 7.
    

    Constraints:

    1 <= arr.length <= 300
    1 <= arr[0].length <= 300
    0 <= arr[i][j] <= 1


```java
class Solution {
    public int countSquares(int[][] matrix) {
        int count = 0;
        int m = matrix.length;
        int n = matrix[0].length;
        int[][] dp = new int[m][n];
        for (int[] d : dp)
            Arrays.fill(d, -1);

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                count += solve(i, j, matrix, dp);
            }
        }
        return count;
    }

    private int solve(int r, int j, int[][] matrix, int[][] dp) {
        if (r >= matrix.length || j >= matrix[0].length || matrix[r][j] == 0)
            return 0;
        if (dp[r][j] != -1)
            return dp[r][j];
        int right = solve(r + 1, j, matrix, dp);
        int diagonal = solve(r + 1, j + 1, matrix, dp);
        int bottom = solve(r, j + 1, matrix, dp);
        return dp[r][j] = 1 + Math.min(right, Math.min(diagonal, bottom));
    }
}

```

</details>
















































# String based DP






<details id="Longest Common Subsequence">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Longest Common Subsequence 
</span>
</summary>

https://www.geeksforgeeks.org/printing-longest-common-subsequence/

Printing Longest Common Subsequence
Last Updated : 26 Oct, 2023
Given two sequences, print the longest subsequence present in both of them.

Examples: 

LCS for input Sequences “ABCDGH” and “AEDFHR” is “ADH” of length 3. 
LCS for input Sequences “AGGTAB” and “GXTXAYB” is “GTAB” of length 4.
We have discussed Longest Common Subsequence (LCS) problem in a previous post. The function discussed there was mainly to find the length of LCS. To find length of LCS, a 2D table L[][] was constructed. In this post, the function to construct and print LCS is discussed.

Following is detailed algorithm to print the LCS. It uses the same 2D table L[][].

Construct L[m+1][n+1] using the steps discussed in previous post.
The value L[m][n] contains length of LCS. Create a character array lcs[] of length equal to the length of lcs plus 1 (one extra to store \0).
Traverse the 2D array starting from L[m][n]. Do following for every cell L[i][j] 
If characters (in X and Y) corresponding to L[i][j] are same (Or X[i-1] == Y[j-1]), then include this character as part of LCS. 
Else compare values of L[i-1][j] and L[i][j-1] and go in direction of greater value.
The following table (taken from Wiki) shows steps (highlighted) followed by the above algorithm.

```java
//Using LCS code
//T.C : O(m*n)
//S.C : O(m*n)
public class Solution {
    public void printLongestCommonSubsequence(String s1, String s2) {
        int m = s1.length();
        int n = s2.length();

        int[][] t = new int[m + 1][n + 1];

        // first row and first column will be 0
        for (int row = 0; row < m + 1; row++) {
            t[row][0] = 0;
        }

        for (int col = 0; col < n + 1; col++) {
            t[0][col] = 0;
        }

        for (int i = 1; i < m + 1; i++) {
            for (int j = 1; j < n + 1; j++) {
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    t[i][j] = 1 + t[i - 1][j - 1];
                } else {
                    t[i][j] = Math.max(t[i - 1][j], t[i][j - 1]);
                }
            }
        }

        StringBuilder lcs = new StringBuilder();

        int i = m, j = n;

        while (i > 0 && j > 0) {
            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                lcs.append(s1.charAt(i - 1));
                i--;
                j--;
            } else {
                if (t[i - 1][j] > t[i][j - 1]) {
                    i--;
                } else {
                    j--;
                }
            }
        }

        System.out.println(lcs.reverse().toString());
    }
}
```

</details>



<details id="Shortest Common Supersequence">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Shortest Common Supersequence 
</span>
</summary>

https://www.geeksforgeeks.org/problems/shortest-common-supersequence0322/1

Given two strings X and Y of lengths m and n respectively, find the length of the smallest string which has both, X and Y as its sub-sequences.
Note: X and Y can have both uppercase and lowercase letters.

Example 1

Input:
X = abcd, Y = xycd
Output: 6
Explanation: Shortest Common Supersequence
would be abxycd which is of length 6 and
has both the strings as its subsequences.
Example 2

Input:
X = efgh, Y = jghi
Output: 6
Explanation: Shortest Common Supersequence
would be ejfghi which is of length 6 and
has both the strings as its subsequences.
Your Task:
You don't have to take any input or print anything. Your task is to complete shortestCommonSupersequence() function that takes X, Y, m, and n as arguments and returns the length of the required string.

Expected Time Complexity: O(Length(X) * Length(Y)).
Expected Auxiliary Space: O(Length(X) * Length(Y)).

Constraints:
1<= |X|, |Y| <= 100


```java
// Approach-1 (Recursion + Memoization)
// T.C : O(m*n)
// S.C : O(m*n)
class Solution {
    int[][] t;

    public int solve(String s1, String s2, int m, int n) {
        if (m == 0 || n == 0) {
            return m + n;
        }

        if (t[m][n] != -1) {
            return t[m][n];
        }

        if (s1.charAt(m - 1) == s2.charAt(n - 1)) {
            return t[m][n] = 1 + solve(s1, s2, m - 1, n - 1);
        } else {
            return t[m][n] = 1 + Math.min(solve(s1, s2, m - 1, n), solve(s1, s2, m, n - 1));
        }
    }

    // Function to find length of shortest common supersequence of two strings.
    public int shortestCommonSupersequence(String s1, String s2, int m, int n) {
        t = new int[m + 1][n + 1];
        for (int[] row : t) {
            Arrays.fill(row, -1);
        }
        return solve(s1, s2, m, n);
    }
}

// Approach-2 (Bottom Up)
// T.C : O(m*n)
// S.C : O(m*n)
class Solution2 {
    // Function to find length of shortest common supersequence of two strings.
    public int shortestCommonSupersequence(String s1, String s2, int m, int n) {
        int[][] t = new int[m + 1][n + 1];

        for (int i = 0; i < m + 1; i++) {
            for (int j = 0; j < n + 1; j++) {
                if (i == 0 || j == 0) {
                    t[i][j] = i + j;
                } else if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    t[i][j] = 1 + t[i - 1][j - 1];
                } else {
                    t[i][j] = 1 + Math.min(t[i - 1][j], t[i][j - 1]);
                }
            }
        }

        return t[m][n];
    }
}

// Approach-3 (Using LCS Code)
// You need to write the common letters only once and then write remaining letters of s1 and s2
// T.C : O(m*n)
// S.C : O(m*n)
class Solution3 {
    // Function to find length of shortest common supersequence of two strings.
    public int shortestCommonSupersequence(String s1, String s2, int m, int n) {
        int[][] t = new int[m + 1][n + 1];

        for (int i = 0; i < m + 1; i++) {
            for (int j = 0; j < n + 1; j++) {
                if (i == 0 || j == 0)
                    t[i][j] = 0;
                else if (s1.charAt(i - 1) == s2.charAt(j - 1))
                    t[i][j] = 1 + t[i - 1][j - 1];
                else
                    t[i][j] = Math.max(t[i - 1][j], t[i][j - 1]);
            }
        }

        int LCS = t[m][n];

        int letters_from_s1 = m - LCS;
        int letters_from_s2 = n - LCS;

        return LCS + letters_from_s1 + letters_from_s2;
    }
}

```
</details>





<details id="1092. Shortest Common Supersequence ">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1092. Shortest Common Supersequence  
</span>
</summary>

https://leetcode.com/problems/shortest-common-supersequence/description/


Given two strings str1 and str2, return the shortest string that has both str1 and str2 as subsequences. If there are multiple valid strings, return any of them.

A string s is a subsequence of string t if deleting some number of characters from t (possibly 0) results in the string s.

 

Example 1:

Input: str1 = "abac", str2 = "cab"
Output: "cabac"
Explanation: 
str1 = "abac" is a subsequence of "cabac" because we can delete the first "c".
str2 = "cab" is a subsequence of "cabac" because we can delete the last "ac".
The answer provided is the shortest such string that satisfies these properties.
Example 2:

Input: str1 = "aaaaaaaa", str2 = "aaaaaaaa"
Output: "aaaaaaaa"
 

Constraints:

1 <= str1.length, str2.length <= 1000
str1 and str2 consist of lowercase English letters.

```java

//T.C : O(m*n)
//S.C : O(m*n)
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {
        int m=str1.length();
        int n=str2.length();
        int[][] dp=new int[m+1][n+1];

        // Making the first column as 0
        for(int i=0;i<=m;i++){
            dp[i][0]=i;
        }

        // Making the first row as 0
        for(int i=0;i<=n;i++){
            dp[0][i]=i;
        }

        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(str1.charAt(i-1) == str2.charAt(j-1)){
                    dp[i][j] = 1 + dp[i-1][j-1];
                }else{
                    dp[i][j]= 1 + Math.min(dp[i][j-1],dp[i-1][j]) ;
                }
            }
        }

        StringBuilder commonSuperSeq = new StringBuilder();
        int i = m, j = n;

        while (i > 0 && j > 0) {
            if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                commonSuperSeq.append(str1.charAt(i - 1));
                i--;
                j--;
            } else if (dp[i - 1][j] < dp[i][j - 1]) {
                commonSuperSeq.append(str1.charAt(i - 1));
                i--;
            } else {
                commonSuperSeq.append(str2.charAt(j - 1));
                j--;
            }
        }

        while (i > 0) {
            commonSuperSeq.append(str1.charAt(i - 1));
            i--;
        }
        while (j > 0) {
            commonSuperSeq.append(str2.charAt(j - 1));
            j--;
        }

        return commonSuperSeq.reverse().toString();
    }
}
```
</details>


<details id="72. Edit Distance">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">72. Edit Distance 
</span>
</summary>

https://leetcode.com/problems/edit-distance/description/

Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.

You have the following three operations permitted on a word:

Insert a character
Delete a character
Replace a character
 

Example 1:

Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
Example 2:

Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
 

Constraints:

0 <= word1.length, word2.length <= 500
word1 and word2 consist of lowercase English letters.



```java

/// Recursion (Not accepted)
class Solution {
    public int minDistance(String word1, String word2) {
        return minEditDist(word1, word2, word1.length() - 1, word2.length() - 1);
    }

    private int minEditDist(String word1, String word2, int i, int j) {
        if (i < 0 && j < 0)
            return 0;

        if (i < 0)
            return j+1;

        if (j < 0)
            return i+1;

        if(word1.charAt(i) == word2.charAt(j))
            return minEditDist(word1, word2, i - 1, j - 1);

        int insert = minEditDist(word1, word2, i, j - 1);
        int delete = minEditDist(word1, word2, i - 1, j);
        int replace = minEditDist(word1, word2, i - 1, j - 1);
        return 1 + Math.min(insert, Math.min(delete, replace));
    }
}
```


```java
// Recursive + memoized (accepted)
class Solution {
    public int minDistance(String word1, String word2) {
        int[][] dp=new int[word1.length()+1][word2.length() + 1];
        for(int[] array:dp)Arrays.fill(array, -1);

        return minEditDist(word1, word2, word1.length() - 1, word2.length() - 1, dp);
    }

    private int minEditDist(String word1, String word2, int i, int j, int[][] dp) {

        if (i < 0)
            return j+1;

        if (j < 0)
            return i+1;
        
        if(dp[i][j]!=-1)return dp[i][j];

        if(word1.charAt(i) == word2.charAt(j))
            return dp[i][j]= minEditDist(word1, word2, i - 1, j - 1, dp);

        int insert = minEditDist(word1, word2, i, j - 1, dp);
        int delete = minEditDist(word1, word2, i - 1, j , dp);
        int replace = minEditDist(word1, word2, i - 1, j - 1, dp);
        return  dp[i][j]= 1 + Math.min(insert, Math.min(delete, replace));
    }
}
```


```java
// Bootom-up
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        int[][] dp = new int[m + 1][n + 1];

        for(int i=0;i<=m;i++){
            dp[i][0]=i;
        }
        for(int i=0;i<=n;i++){
            dp[0][i]=i;
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // Same charaacter requires no additional operation
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // Check for all the operation
                    dp[i][j] = 1 + Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1]));
                }
            }
        }
        return dp[m][n];
    }
}
```




</details>




<details id="647. Palindromic Substrings">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">647. Palindromic Substrings 
</span>
</summary>

https://leetcode.com/problems/palindromic-substrings/description/

Given a string s, return the number of palindromic substrings in it.

A string is a palindrome when it reads the same backward as forward.

A substring is a contiguous sequence of characters within the string.

 

Example 1:

Input: s = "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
Example 2:

Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
 

Constraints:

1 <= s.length <= 1000
s consists of lowercase English letters.

```java
// Approach-1 (Simply check all substrings possible)
// T.C : O(n^3)
// S.C : O(1)
class Solution1 {
    public boolean check(String s, int i, int j) {
        if (i >= j) {
            return true;
        }

        if (s.charAt(i) == s.charAt(j)) {
            return check(s, i + 1, j - 1);
        }

        return false;
    }

    public int countSubstrings(String s) {
        int n = s.length();

        int count = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) { // check all possible substrings
                if (check(s, i, j)) {
                    count++;
                }
            }
        }

        return count;
    }
}

// Approach-2 (Memoize the approach above)
// T.C : O(n^2) - Every subproblem is being computed only once and after that, it's being reused
// S.C : O(n^2)
class Solution {
    int[][] t;

    public boolean check(String s, int i, int j) {
        if (i >= j) {
            return true;
        }

        if (t[i][j] != -1) {
            return t[i][j] == 1;
        }

        if (s.charAt(i) == s.charAt(j)) {
            boolean val = check(s, i+1, j-1);
            if(val == true)
                t[i][j] = 1;
            else
                t[i][j] = 0;
            return val;
        }

        t[i][j] = 0;
        return false;
    }

    public int countSubstrings(String s) {
        int n = s.length();
        t = new int[n][n];
        for (int[] row : t) {
            Arrays.fill(row, -1);
        }

        int count = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) { // check all possible substrings
                if (check(s, i, j)) {
                    count++;
                }
            }
        }

        return count;
    }
}


// Approach-3 (Bottom Up - My Favourite Blueprint of Palindrome Questions)
// T.C : O(n^2)
// S.C : O(n^2)
class Solution3 {
    public int countSubstrings(String s) {
        int n = s.length();
        boolean[][] t = new boolean[n][n];

        int count = 0;

        for (int L = 1; L <= n; L++) {
            for (int i = 0; i + L <= n; i++) {
                int j = i + L - 1;

                if (i == j) {
                    t[i][i] = true; // Single characters are palindrome
                } else if (i + 1 == j) {
                    t[i][j] = (s.charAt(i) == s.charAt(j)); // Strings of 2 Length
                } else {
                    t[i][j] = (s.charAt(i) == s.charAt(j) && t[i + 1][j - 1]);
                }

                if (t[i][j]) {
                    count++;
                }
            }
        }

        return count;
    }
}


// Approach-4 (Smart approach)
// T.C : O(n^2)
// S.C : O(1)
class Solution4 {
    private int count = 0;

    private void check(String s, int i, int j, int n) {
        while (i >= 0 && j < n && s.charAt(i) == s.charAt(j)) {
            count++;
            i--; // expanding from center
            j++; // expanding from center
        }
    }

    public int countSubstrings(String s) {
        int n = s.length();
        count = 0;

        /*
         * Every single character in the string is a center for possible odd-length
         * palindromes: check(s, i, i); Every pair of consecutive characters in the
         * string is a center for possible even-length palindromes: check(s, i, i+1);
         */
        for (int i = 0; i < n; i++) {
            check(s, i, i, n);
            check(s, i, i + 1, n);
        }
        return count;
    }
}
```
</details>






<details id="5. Longest Palindromic Substring">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">5. Longest Palindromic Substring 
</span>
</summary>

https://leetcode.com/problems/longest-palindromic-substring/description/

Given a string s, return the longest 
palindromic
 
substring
 in s.

 

Example 1:

Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
Example 2:

Input: s = "cbbd"
Output: "bb"
 

Constraints:

1 <= s.length <= 1000
s consist of only digits and English letters.

```java
//Approach 1 - Recursion + Memoization
//T.C : O(n^2) - Because the AMortized Time Complexity of solve() will become 1 due to memoization.
//S.C : O(n^2)
public class Solution {
    private int[][] t;

    public String longestPalindrome(String s) {
        int n = s.length();
        int maxlen = Integer.MIN_VALUE;
        int startingIndex = 0;
        t = new int[n][n];
        for (int i = 0; i < n; i++) {
            Arrays.fill(t[i], -1);
        }

        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                if (solve(s, i, j) && j - i + 1 > maxlen) {
                    startingIndex = i;
                    maxlen = j - i + 1;
                }
            }
        }

        return s.substring(startingIndex, startingIndex + maxlen);
    }

    private boolean solve(String s, int l, int r) {
        if (l >= r) {
            return true;
        }

        if (t[l][r] != -1) {
            return t[l][r] == 1;
        }

        if (s.charAt(l) == s.charAt(r)) {
            t[l][r] = solve(s, l + 1, r - 1) ? 1 : 0;
        } else {
            t[l][r] = 0;
        }

        return t[l][r] == 1;
    }
}

```

```java
//Approach 2 - Looping simply in solve()
//T.C : O(n^3)
public class Solution {
    private boolean solve(String s, int l, int r) {
        while (l <= r) {
            if (s.charAt(l) != s.charAt(r))
                return false;
            l++;
            r--;
        }
        return true;
    }

    public String longestPalindrome(String s) {
        int n = s.length();
        int maxlen = Integer.MIN_VALUE;
        int startingIndex = 0;

        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                if (solve(s, i, j)) {
                    if (j - i + 1 > maxlen) {
                        startingIndex = i;
                        maxlen = j - i + 1;
                    }
                }
            }
        }

        return s.substring(startingIndex, startingIndex + maxlen);
    }
}

```



```java


// Bottom up O(n^2)
class Solution {
    public String longestPalindrome(String s) {
        int m = s.length();
        // isPalindrome[i][j] signifies if the substring starting from i ending at j,
        // both inclusive is a palindrome.
        boolean[][] isPalindrome = new boolean[m][m];
        String maxLengthPalindrome = "";
        for (int L = 1; L <= m; L++) {
            for (int i = 0; i + L - 1 < m; i++) {
                int j = i + L - 1;

                // 1 length string is always palindrome
                if (i == j)
                    isPalindrome[i][j] = true;

                // 2 length strings
                else if (i + 1 == j)
                    isPalindrome[i][j] = s.charAt(i) == s.charAt(j);

                else {
                    isPalindrome[i][j] = s.charAt(i) == s.charAt(j) && isPalindrome[i + 1][j - 1];
                }

                if (isPalindrome[i][j] && L>maxLengthPalindrome.length()) {
                    maxLengthPalindrome = s.substring(i,j+1);
                }
            }
        }
        return maxLengthPalindrome;
    }
}

```
</details>













<details id="1584. Min Cost to Connect All Points">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1584. Min Cost to Connect All Points 
</span>
</summary>

https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/description/

Given a string s. In one step you can insert any character at any index of the string.

Return the minimum number of steps to make s palindrome.

A Palindrome String is one that reads the same backward as well as forward.

 

Example 1:

Input: s = "zzazz"
Output: 0
Explanation: The string "zzazz" is already palindrome we do not need any insertions.
Example 2:

Input: s = "mbadm"
Output: 2
Explanation: String can be "mbdadbm" or "mdbabdm".
Example 3:

Input: s = "leetcode"
Output: 5
Explanation: Inserting 5 characters the string becomes "leetcodocteel".
 

Constraints:

1 <= s.length <= 500
s consists of lowercase English letters.


```java
//Approach-1 (Recur + Memo - Using Straight Forward Pallindromic Property)
//T.C : O(n*n)
//S.C : O(n*n)
class Solution {
    private int[][] t;

    private int solve(int i, int j, String s) {
        if (i >= j)
            return 0;

        if (t[i][j] != -1)
            return t[i][j];

        if (s.charAt(i) == s.charAt(j))
            return t[i][j] = solve(i + 1, j - 1, s);

        return t[i][j] = 1 + Math.min(solve(i, j - 1, s), solve(i + 1, j, s));
    }

    public int minInsertions(String s) {
        int n = s.length();
        t = new int[n][n];

        for (int[] row : t)
            Arrays.fill(row, -1);

        return solve(0, n - 1, s);
    }
}




//Approach-2 (Bottom Up - Using my favourite Palindrome Blue Print)
//T.C : O(n*n)
//S.C : O(n*n)
class Solution {
    public int minInsertions(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];
        // State: dp[i][j] = min insertion to make s[i..j] a palindrome
        
        for (int L = 2; L <= n; L++) {
            for (int i = 0; i < n - L + 1; i++) {
                int j = i + L - 1;
                if (s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = dp[i + 1][j - 1];
                } else {
                    dp[i][j] = 1 + Math.min(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        
        return dp[0][n - 1];
    }
}




//Approach-3 (Using LCS)
//T.C : O(n*n)
//S.C : O(n*n)
class Solution {
    private int[][] t;

    private int LCS(String s1, String s2, int m, int n) {
        if (m == 0 || n == 0) {
            return t[m][n] = 0;
        }

        if (t[m][n] != -1) {
            return t[m][n];
        }

        if (s1.charAt(m - 1) == s2.charAt(n - 1)) {
            return t[m][n] = 1 + LCS(s1, s2, m - 1, n - 1);
        }

        return t[m][n] = Math.max(LCS(s1, s2, m, n - 1), LCS(s1, s2, m - 1, n));
    }

    public int minInsertions(String s) {
        int m = s.length();
        t = new int[m + 1][m + 1];
        for (int[] row : t) {
            Arrays.fill(row, -1);
        }

        String temp = new StringBuilder(s).reverse().toString();
        int lcs_length = LCS(s, temp, m, m);

        return m - lcs_length;
    }
}
```
</details>




<details id="131. Palindrome Partitioning">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">131. Palindrome Partitioning 
</span>
</summary>

https://leetcode.com/problems/palindrome-partitioning/description/

Given a string s, partition s such that every 
substring
 of the partition is a 
palindrome
. Return all possible palindrome partitioning of s.

 

Example 1:

Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
Example 2:

Input: s = "a"
Output: [["a"]]
 

Constraints:

1 <= s.length <= 16
s contains only lowercase English letters.

```java
//Approach-1 (Using Bakctracking Khandani Template)
//Whenever a question asks for "Generating all possible" something, think about Backtracking once
//T.C : O(n * 2^n) - For a string of length n, there are 2^(𝑛 − 1) potential ways to partition it (since each position can either be a cut or not). and we also check palindrome O(n)
//S.C : O(n * 2^n) - Number of partitions * their length
class Solution {
    private int n;
    
    private boolean isPalindrome(String s, int l, int r) {
        while (l < r) {
            if (s.charAt(l) != s.charAt(r))
                return false;
            l++;
            r--;
        }
        return true;
    }
    
    private void backtrack(String s, int idx, List<String> curr, List<List<String>> result) {
        if (idx == n) {
            result.add(new ArrayList<>(curr));
            return;
        }
        
        for (int i = idx; i < n; i++) {
            if (isPalindrome(s, idx, i)) {
                curr.add(s.substring(idx, i + 1));
                backtrack(s, i + 1, curr, result);
                curr.remove(curr.size() - 1);
            }
        }
    }
    
    public List<List<String>> partition(String s) {
        n = s.length();
        List<List<String>> result = new ArrayList<>();
        List<String> curr = new ArrayList<>();
        
        backtrack(s, 0, curr, result);
        
        return result;
    }
}


//Approach-2 (Using DP + Backtracking)
//T.C : O(2^n)
//S.C : O(n^2)
class Solution {
    public void solve(String s, int i, List<String> currPartition, boolean[][] t, List<List<String>> result) {
        if (i == s.length()) {
            result.add(new ArrayList<>(currPartition));
            return;
        }

        for (int j = i; j < s.length(); j++) {
            if (t[i][j]) { // palindrome
                currPartition.add(s.substring(i, j + 1));
                solve(s, j + 1, currPartition, t, result);
                currPartition.remove(currPartition.size() - 1);
            }
        }
    }

    public List<List<String>> partition(String s) {
        int n = s.length();
        boolean[][] t = new boolean[n][n];

        // Initialize the DP table for palindromic substrings
        // t[i][j] = true -> s[i...j] is a palindrome

        for (int i = 0; i < n; ++i) {
            t[i][i] = true; // substring of a single character is always a palindrome
        }

        for (int L = 2; L <= n; ++L) {
            for (int i = 0; i < n - L + 1; ++i) {
                int j = i + L - 1;
                if (s.charAt(i) == s.charAt(j)) {
                    if (L == 2) {
                        t[i][j] = true;
                    } else {
                        t[i][j] = t[i + 1][j - 1];
                    }
                }
            }
        }

        List<List<String>> result = new ArrayList<>();
        List<String> currPartition = new ArrayList<>();
        solve(s, 0, currPartition, t, result);

        return result;
    }
}

```



</details>




<details id="132. Palindrome Partitioning II">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">132. Palindrome Partitioning II 
</span>
</summary>

https://leetcode.com/problems/palindrome-partitioning-ii/description/

Given a string s, partition s such that every 
substring
 of the partition is a 
palindrome
.

Return the minimum cuts needed for a palindrome partitioning of s.

 

Example 1:

Input: s = "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
Example 2:

Input: s = "a"
Output: 0
Example 3:

Input: s = "ab"
Output: 1
 

Constraints:

1 <= s.length <= 2000
s consists of lowercase English letters only.


```java
//Approach-1 (Recursion + Memoization) - (TLE)
//T.C : O(n^3)
//S.C : O(n^2)
class Solution {
    private int[][] dp;

    private boolean isPalindrome(String s, int i, int j) {
        while (i < j) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
            i++;
            j--;
        }
        return true;
    }

    private int solve(String s, int i, int j) {
        if (i >= j) {
            return 0;
        }
        if (dp[i][j] != -1) {
            return dp[i][j];
        }
        
        if (isPalindrome(s, i, j)) {
            return dp[i][j] = 0;
        }

        int minCuts = Integer.MAX_VALUE;
        for (int k = i; k <= j - 1; k++) {
            int temp = 1 + solve(s, i, k) + solve(s, k + 1, j);
            minCuts = Math.min(minCuts, temp);
        }

        return dp[i][j] = minCuts;
    }

    public int minCut(String s) {
        int n = s.length();
        dp = new int[n][n];

        // Initialize the dp array to -1
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }

        return solve(s, 0, n - 1);
    }
}


//Approach-2 (Bottom-Up) : Accepted
//T.C : O(n^2)
//S.C : O(n^2)
class Solution {
    public int solve(String s) {
        int n = s.length();
        int[] t = new int[n];
        boolean[][] P = new boolean[n][n];

        // Length = 1 substrings are always palindromes
        for (int i = 0; i < n; i++) {
            P[i][i] = true;
        }

        // Length = 2+ substrings
        for (int L = 2; L <= n; L++) {
            for (int i = 0; i < n - L + 1; i++) {
                int j = i + L - 1;

                if (L == 2) {
                    P[i][j] = (s.charAt(i) == s.charAt(j));
                } else {
                    P[i][j] = (s.charAt(i) == s.charAt(j)) && P[i + 1][j - 1];
                }
            }
        }

        // Compute minimum cuts using dynamic programming
        for (int i = 0; i < n; i++) {
            if (P[0][i]) {
                t[i] = 0;
            } else {
                t[i] = Integer.MAX_VALUE;
                for (int k = 0; k < i; k++) {
                    if (P[k + 1][i] && 1 + t[k] < t[i]) {
                        t[i] = 1 + t[k];
                    }
                }
            }
        }

        return t[n - 1];
    }

    public int minCut(String s) {
        int n = s.length();
        if (n == 0 || n == 1) {
            return 0;
        }

        return solve(s);
    }
}

```
</details>




<details id="139. Word Break">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">139. Word Break 
</span>
</summary>

```java
// recursion (O(2^n))
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> dict = new HashSet(wordDict);

        return canPartitionSubstring(0, 0, s, dict);
    }

    private boolean canPartitionSubstring(int l, int r, String s, Set<String> dict) {
        // Base Case
        if (l == r && l == s.length()) {
            return true;
        }
        if(r>=s.length()){
            return false;
        }
        boolean withPartition = false;
        boolean withoutPartition = false;
        // Propagation
        if (dict.contains(s.substring(l, r + 1))) {
            // Partition
            withPartition = canPartitionSubstring(r + 1, r + 1, s, dict);
        }
        // without partition
        withoutPartition = canPartitionSubstring(l, r + 1, s, dict);
        return withoutPartition || withPartition;
    }
}
```


```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> dict = new HashSet(wordDict);
        int [][] dp=new int[s.length()+1][s.length()+1];

        for(int[] d:dp)Arrays.fill(d, -1);

        return canPartitionSubstring(0, 0, s, dict, dp);
    }

    private boolean canPartitionSubstring(int l, int r, String s, Set<String> dict, int[][] dp) {
        // Base Case
        if (l == r && l == s.length()) {
             dp[l][r]=1;
            return true;
        }
        if(r>=s.length()){
            dp[l][r]=0;
            return false;
        }
        if(dp[l][r]!=-1)return dp[l][r]==1?true:false;


        boolean withPartition = false;
        boolean withoutPartition = false;
        // Propagation
        if (dict.contains(s.substring(l, r + 1))) {
            // Partition
            withPartition = canPartitionSubstring(r + 1, r + 1, s, dict, dp);
        }
        // without partition
        withoutPartition = canPartitionSubstring(l, r + 1, s, dict, dp);
        dp[l][r]=(withoutPartition || withPartition)?1:0 ;
        return withoutPartition || withPartition;
    }
}

```
</details>




<details id="322. Coin Change">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">322. Coin Change 
</span>
</summary>

https://leetcode.com/problems/coin-change/description/

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

 

Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
Example 2:

Input: coins = [2], amount = 3
Output: -1
Example 3:

Input: coins = [1], amount = 0
Output: 0
 

Constraints:

1 <= coins.length <= 12
1 <= coins[i] <= 231 - 1
0 <= amount <= 104


```java

// Recursive O()
class Solution {
    int ans = Integer.MAX_VALUE;

    public int coinChange(int[] coins, int amount) {
        makeAmount(coins, amount, 0, 0);
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }
    // coins = [1,2,5], amount = 11
    private void makeAmount(int[] coins, int target, int i, int coinCount) {
        // Base Case
        if (target < 0 || i >= coins.length)
            return;

        if (target == 0) {
            ans = Math.min(ans, coinCount);
            return;
        }
        int denomination = coins[i]; //1
        // Take this coin
        makeAmount(coins, target - denomination, i, 1 + coinCount);

        // Skip this coin
        makeAmount(coins, target, i + 1, coinCount);
    }
}



=====================================================================================

// Recursion + Memoization
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[][] dp = new int[coins.length][amount + 1];
        for (int[] d : dp) Arrays.fill(d, -1); // Initialize dp with -1 (unvisited)

        int result = makeAmount(coins, amount, 0, dp);
        return result == Integer.MAX_VALUE ? -1 : result; // If no solution, return -1
    }

    private int makeAmount(int[] coins, int target, int i, int[][] dp) {
        // Base Cases
        if (target == 0) return 0; // No coins needed if the target is zero
        if (target < 0 || i >= coins.length) return Integer.MAX_VALUE; // No solution possible

        if (dp[i][target] != -1) return dp[i][target]; // Use memoized result

        // Calculate minimum coins: take the current coin or skip it
        int take = makeAmount(coins, target - coins[i], i, dp); // Take the coin
        if (take != Integer.MAX_VALUE) take += 1; // Increment coin count if valid

        int skip = makeAmount(coins, target, i + 1, dp); // Skip the coin

        // Memoize the result for current i and target
        dp[i][target] = Math.min(take, skip);

        return dp[i][target];
    }
}

```



</details>


# Grid based DP

<details id="62. Unique Paths">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">62. Unique Paths 
</span>
</summary>

https://leetcode.com/problems/unique-paths/description/

There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to 2 * 109.

 

```java
//Approach-1 - Recursion + Memoization
//T.C : O(m*n)
//S.C : O(m*n)
class Solution {
    public int solve(int i, int j, int m, int n, int[][] t) {
        // Base case: Reached the bottom-right cell
        if (i == m - 1 && j == n - 1) {
            return 1;
        }

        // Out of bounds
        if (i < 0 || i >= m || j < 0 || j >= n) {
            return 0;
        }

        // If already computed, return the stored result
        if (t[i][j] != -1) {
            return t[i][j];
        }

        // Calculate the number of paths by going right and down
        int right = solve(i, j + 1, m, n, t);
        int down = solve(i + 1, j, m, n, t);

        // Store the result in the memoization table
        return t[i][j] = right + down;
    }

    public int uniquePaths(int m, int n) {
        // Create a memoization table initialized with -1
        int[][] t = new int[m][n];
        for (int[] row : t) {
            Arrays.fill(row, -1);
        }

        // Start the recursive computation from the top-left cell
        return solve(0, 0, m, n, t);
    }
}

//Approach-2 (using Bottom Up)
//T.C : O(m*n)
//S.C : O(m*n)
//Note : You can write C++ code above as simple as this one but I commented the code above for clarity and added some extra line of code for clarity
class Solution {
    public int uniquePaths(int m, int n) {
        // Create a 2D array for storing the number of ways to reach each cell
        int[][] t = new int[m][n];

        // Initialize the first row
        for (int col = 0; col < n; col++) {
            t[0][col] = 1; // Only one way to reach any cell in the first row
        }

        // Initialize the first column
        for (int row = 0; row < m; row++) {
            t[row][0] = 1; // Only one way to reach any cell in the first column
        }

        // Fill the rest of the table using the relation:
        // t[i][j] = t[i-1][j] + t[i][j-1]
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                t[i][j] = t[i - 1][j] + t[i][j - 1];
            }
        }

        // The bottom-right cell contains the total number of unique paths
        return t[m - 1][n - 1];
    }
}

```
</details>




<details id="63. Unique Paths II">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">63. Unique Paths II 
</span>
</summary>

https://leetcode.com/problems/unique-paths-ii/description/

You are given an m x n integer array grid. There is a robot initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

An obstacle and space are marked as 1 or 0 respectively in grid. A path that the robot takes cannot include any square that is an obstacle.

Return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The testcases are generated so that the answer will be less than or equal to 2 * 109.


```java
//Approach-1 (Recursion + Memo)
//Recursion T.C : O(m*n)
//Memo T.C      : O(m*n)
class Solution {
    Integer t[][]=new Integer[101][101];
    int m, n;
    
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        m = obstacleGrid.length;
        n = obstacleGrid[0].length;
        
        return solve(obstacleGrid, 0, 0);
    }
    int solve(int[][]obstacleGrid, int i, int j){
         if(i < 0 || i >= m || j < 0 || j >= n || obstacleGrid[i][j] != 0) {
            return 0;
        }
        
        if(t[i][j] != null)
            return t[i][j];
        
        if(i == m-1 && j == n-1)
            return 1;
        
        //Why we are not making [i][j] visited ?
        //Because robot can only move down or right so it will never visit any visited cell again
        //int temp = obstacleGrid[i][j];
        //obstacleGrid[i][j] = -1;
        
        int right = solve(obstacleGrid, i, j+1);
        int down  = solve(obstacleGrid, i+1, j);
        
        //obstacleGrid[i][j] = temp;
        
        return t[i][j] = right + down;
    }
}

class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int r = obstacleGrid.length;
        int c = obstacleGrid[0].length;
        if (obstacleGrid[0][0] == 1 || obstacleGrid[r - 1][c - 1] == 1) {
            return 0;
        }
        int[][] dp = new int[r][c];
        dp[0][0] = 1;

        for (int i = 1; i < c; i++) {
            if (obstacleGrid[0][i] == 0) {
                dp[0][i] = 1;
            } else {
                break;
            }
        }
        for (int i = 1; i < r; i++) {
            if (obstacleGrid[i][0] == 0) {
                dp[i][0] = 1;
            } else {
               break;
            }
        }

        for (int i = 1; i < r; i++) {
            for (int j = 1; j < c; j++) {
                if (obstacleGrid[i][j] == 0) {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }
        return dp[r - 1][c - 1];
    }
}

```
</details>




<details id="1594. Maximum Non Negative Product in a Matrix">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1594. Maximum Non Negative Product in a Matrix 
</span>
</summary>

https://leetcode.com/problems/maximum-non-negative-product-in-a-matrix/description/


You are given a m x n matrix grid. Initially, you are located at the top-left corner (0, 0), and in each step, you can only move right or down in the matrix.

Among all possible paths starting from the top-left corner (0, 0) and ending in the bottom-right corner (m - 1, n - 1), find the path with the maximum non-negative product. The product of a path is the product of all integers in the grid cells visited along the path.

Return the maximum non-negative product modulo 109 + 7. If the maximum product is negative, return -1.

Notice that the modulo is performed after getting the maximum product.

```java

//Approach - 1 (Recursion + Memoization)
//T.C : O(m*n)
//S.C : O(m*n)
class Solution {
    int m, n;
    final int MOD = 1000000007;

    // Memoization table for storing (max, min) pairs
    Pair<Long, Long>[][] t;

    public Pair<Long, Long> solve(int i, int j, int[][] grid) {
        // Base case: If we're at the bottom-right corner, return the value
        if (i == m - 1 && j == n - 1) {
            return new Pair<>((long) grid[i][j], (long) grid[i][j]);
        }

        long maxVal = Long.MIN_VALUE;
        long minVal = Long.MAX_VALUE;

        // If already computed, return the memoized result
        if (t[i][j] != null) {
            return t[i][j];
        }

        // Move down
        if (i + 1 < m) {
            Pair<Long, Long> down = solve(i + 1, j, grid);
            maxVal = Math.max(maxVal, Math.max(grid[i][j] * down.getKey(), grid[i][j] * down.getValue()));
            minVal = Math.min(minVal, Math.min(grid[i][j] * down.getKey(), grid[i][j] * down.getValue()));
        }

        // Move right
        if (j + 1 < n) {
            Pair<Long, Long> right = solve(i, j + 1, grid);
            maxVal = Math.max(maxVal, Math.max(grid[i][j] * right.getKey(), grid[i][j] * right.getValue()));
            minVal = Math.min(minVal, Math.min(grid[i][j] * right.getKey(), grid[i][j] * right.getValue()));
        }

        // Memoize results
        t[i][j] = new Pair<>(maxVal, minVal);

        return t[i][j];
    }

    public int maxProductPath(int[][] grid) {
        m = grid.length;
        n = grid[0].length;

        // Initialize the memoization table
        t = new Pair[m][n];

        // Get the result from the top-left corner
        Pair<Long, Long> result = solve(0, 0, grid);

        // If the result is negative, return -1, otherwise return the result modulo MOD
        return result.getKey() < 0 ? -1 : (int) (result.getKey() % MOD);
    }
}


//Approach - 2 (Bottom Up)
//T.C : O(m*n)
//S.C : O(m*n)
class Solution {
    final int MOD = 1000000007;

    public int maxProductPath(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;

        // Initialize the DP table
        Pair<Long, Long>[][] t = new Pair[m][n];

        // Base case: starting point
        t[0][0] = new Pair<>((long) grid[0][0], (long) grid[0][0]);

        // Fill the first row
        for (int j = 1; j < n; j++) {
            t[0][j] = new Pair<>(t[0][j - 1].getKey() * grid[0][j], t[0][j - 1].getValue() * grid[0][j]);
        }

        // Fill the first column
        for (int i = 1; i < m; i++) {
            t[i][0] = new Pair<>(t[i - 1][0].getKey() * grid[i][0], t[i - 1][0].getValue() * grid[i][0]);
        }

        // Fill the rest of the DP table
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                long upMax = t[i - 1][j].getKey();
                long upMin = t[i - 1][j].getValue();

                long leftMax = t[i][j - 1].getKey();
                long leftMin = t[i][j - 1].getValue();

                t[i][j] = new Pair<>(
                    Math.max(Math.max(upMax * grid[i][j], upMin * grid[i][j]), Math.max(leftMax * grid[i][j], leftMin * grid[i][j])),
                    Math.min(Math.min(upMax * grid[i][j], upMin * grid[i][j]), Math.min(leftMax * grid[i][j], leftMin * grid[i][j]))
                );
            }
        }

        // Get the result from the bottom-right corner
        long maxProd = t[m - 1][n - 1].getKey();

        // If the result is negative, return -1, otherwise return the result modulo MOD
        return maxProd < 0 ? -1 : (int) (maxProd % MOD);
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




