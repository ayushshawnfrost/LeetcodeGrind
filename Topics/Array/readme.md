

<details id="334. Increasing Triplet Subsequence">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">334. Increasing Triplet Subsequence 
</span>
</summary>

https://leetcode.com/problems/increasing-triplet-subsequence/description/

Given an integer array nums, return true if there exists a triple of indices (i, j, k) such that i < j < k and nums[i] < nums[j] < nums[k]. If no such indices exists, return false.

 

Example 1:

Input: nums = [1,2,3,4,5]
Output: true
Explanation: Any triplet where i < j < k is valid.
Example 2:

Input: nums = [5,4,3,2,1]
Output: false
Explanation: No triplet exists.
Example 3:

Input: nums = [2,1,5,0,4,6]
Output: true
Explanation: The triplet (3, 4, 5) is valid because nums[3] == 0 < nums[4] == 4 < nums[5] == 6.
 

Constraints:

1 <= nums.length <= 5 * 105
-231 <= nums[i] <= 231 - 1
 

Follow up: Could you implement a solution that runs in O(n) time complexity and O(1) space complexity?


    Solution Approach
    There can be multiple approaches to solve this. First using nested for loops for (n^3). 

    num1 must be smallest then num2 then num3. Start searching for num3 and meanwhile check if num3 is less than num1 (true-> update num1) then check with num2. 

```java
// O(n)
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int n1 = Integer.MAX_VALUE;
        int n2 = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length; i++) {
            int num = nums[i];
            if (num <= n1) {
                n1 = num;
            } else if (num <= n2) {
                n2 = num;
            } else {
                return true;
            }
        }
        return false;
    }
}


//T.C. O(n)
//S.C. O(n)

 class Solution {
     public boolean increasingTriplet(int[] nums) {
         int l= nums.length;
         int[] left= new int[l];
         int[] right= new int[l];
         left[0]= nums[0];
         for(int i=1;i<l;i++){ //Find left min
             left[i]=Math.min(nums[i], left[i-1]);
         }
         right[l-1]= nums[l-1];
         for(int i=l-2;i>=0;i--){ //Find right max
             right[i]=Math.max(nums[i], right[i+1]);
         }
         for(int i=0;i<l;i++){ 
             if(left[i]< nums[i] && nums[i]< right[i]) 
                 return true;
         }
         return false;
     }
 }

// Dynamic Programming
// We can also use DP method to solve it.

// Let dp[i] represents the maximum length of a increasing sequence.

// dp[i]={ 
// 1
// dp[j]+1
// ​
  
// dp[i]≤dp[j],0<j<i
// dp[i]>dp[j],0<j<i
// ​
 
// ​
// We can reduce the time complexity to O(n 
// 2
//  ), but it still will TLE.

    public boolean increasingTriplet(int[] nums) {
        int len = nums.length;

        int[] dp = new int[len];
        Arrays.fill(dp, 1);

        for (int i = 0; i < len; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = dp[j] + 1;
                }

                if (dp[i] >= 3) {
                    return true;
                }
            }
        }

        return false;
    }



// Longest Increasing Subsequence 
class Solution {
    public boolean increasingTriplet(int[] nums) {
        List<Integer> LIS = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            int currNum = nums[i];
            int index = lowerBound(LIS, currNum);

            // Check sequence till length 3
            if (LIS.size() >= 3) {
                return true; 
            }
            if (index == -1) {
                LIS.add(currNum);
            } else {
                LIS.set(index, currNum);
            }
        }
        return LIS.size() >= 3;
    }

    private int lowerBound(List<Integer> LIS, int target) {
        int l = 0;
        int r = LIS.size() - 1;
        int index = -1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (LIS.get(mid) >= target) {
                index = mid;
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return index;
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

