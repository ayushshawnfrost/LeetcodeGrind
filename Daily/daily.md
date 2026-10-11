


<details id="2333. Minimum Sum of Squared Difference">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">2333. Minimum Sum of Squared Difference 
</span>
</summary>

https://leetcode.com/problems/minimum-sum-of-squared-difference/description/?envType=daily-question&envId=2026-10-10

Medium
Topics
premium lock icon
Companies
Hint
You are given two positive 0-indexed integer arrays nums1 and nums2, both of length n.

The sum of squared difference of arrays nums1 and nums2 is defined as the sum of (nums1[i] - nums2[i])2 for each 0 <= i < n.

You are also given two positive integers k1 and k2. You can modify any of the elements of nums1 by +1 or -1 at most k1 times. Similarly, you can modify any of the elements of nums2 by +1 or -1 at most k2 times.

Return the minimum sum of squared difference after modifying array nums1 at most k1 times and modifying array nums2 at most k2 times.

Note: You are allowed to modify the array elements to become negative integers.

 

Example 1:

Input: nums1 = [1,2,3,4], nums2 = [2,10,20,19], k1 = 0, k2 = 0
Output: 579
Explanation: The elements in nums1 and nums2 cannot be modified because k1 = 0 and k2 = 0. 
The sum of square difference will be: (1 - 2)2 + (2 - 10)2 + (3 - 20)2 + (4 - 19)2 = 579.
Example 2:

Input: nums1 = [1,4,10,12], nums2 = [5,8,6,9], k1 = 1, k2 = 1
Output: 43
Explanation: One way to obtain the minimum sum of square difference is: 
- Increase nums1[0] once.
- Increase nums2[2] once.
The minimum of the sum of square difference will be: 
(2 - 5)2 + (4 - 8)2 + (10 - 7)2 + (12 - 9)2 = 43.
Note that, there are other ways to obtain the minimum of the sum of square difference, but there is no way to obtain a sum smaller than 43.
 

Constraints:

n == nums1.length == nums2.length
1 <= n <= 105
0 <= nums1[i], nums2[i] <= 105
0 <= k1, k2 <= 109


```java
//Approach-1 (Brute Force - TLE)
//T.C : O((n + k1 + k2) * log n), k is huge
//S.C : O(n)
class Solution {
    public long minSumSquareDiff(int[] nums1, int[] nums2, int k1, int k2) {
        int n = nums1.length;

        // Max-heap: top is always the largest diff
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
        for (int i = 0; i < n; i++) {
            pq.offer(Math.abs(nums1[i] - nums2[i]));
        }

        long K = (long) k1 + k2;

        while (K > 0 && pq.peek() > 0) {
            int largestDiff = pq.poll();
            pq.offer(largestDiff - 1);
            K--;
        }

        long result = 0;
        while (!pq.isEmpty()) {
            long d = pq.poll();
            result += d * d;
        }

        return result;
    }
}
```

```java
//Approach-2 (Using Counting Sort)
//T.C : O(n + maxDiff), maxDiff <= 10^5
//S.C : O(n)
class Solution {
    public long minSumSquareDiff(int[] nums1, int[] nums2, int k1, int k2) {
        int n = nums1.length;

        int[] diff = new int[n];
        int maxDiff = 0;
        for (int i = 0; i < n; i++) {
            diff[i] = Math.abs(nums1[i] - nums2[i]);
            maxDiff = Math.max(maxDiff, diff[i]);
        }

        // countDiff[d] = count of indices with diff exactly d
        int[] countDiff = new int[maxDiff + 1];
        for (int d : diff) {
            countDiff[d]++;
        }

        long K = (long) k1 + k2;

        for (int currDiff = maxDiff; currDiff > 0 && K > 0; currDiff--) {
            int countOps = (int) Math.min(countDiff[currDiff], K);

            countDiff[currDiff]     -= countOps;
            countDiff[currDiff - 1] += countOps;
            K                       -= countOps;
        }

        long result = 0;
        for (long d = 1; d <= maxDiff; d++) {
            result += countDiff[(int) d] * d * d;
        }

        return result;
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

