
<details id="34. Find First and Last Position of Element in Sorted Array">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">34. Find First and Last Position of Element in Sorted Array 
</span>
</summary>

https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/

Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
Example 2:

Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
Example 3:

Input: nums = [], target = 0
Output: [-1,-1]
 

Constraints:

0 <= nums.length <= 105
-109 <= nums[i] <= 109
nums is a non-decreasing array.
-109 <= target <= 109


```java

//Approach-1 (Using Two Custom Binary Search)
//T.C : O(nlogn)
class Solution {
    public int findFirstPosition(int[] nums, int target) {
        int l = 0, r = nums.length - 1;
        int result = -1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (nums[mid] == target) {
                result = mid; // possibly my answer
                r = mid - 1; // but lets look at left more
            } else if (nums[mid] > target) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return result;
    }

    public int findLastPosition(int[] nums, int target) {
        int l = 0, r = nums.length - 1;
        int result = -1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (nums[mid] == target) {
                result = mid; // possibly my answer
                l = mid + 1; // but lets look at right more
            } else if (nums[mid] > target) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return result;
    }

    public int[] searchRange(int[] nums, int target) {
        int firstPosition = findFirstPosition(nums, target);
        int lastPosition = findLastPosition(nums, target);
        return new int[]{firstPosition, lastPosition};
    }
}


//Approach-2 (Using Own written lower and upper bound in java)
//T.C : O(nlogn)
import java.util.Arrays;
import java.util.List;

class Solution {
    public int[] searchRange(int[] nums, int target) {
        int l = lowerBound(nums, target);
        int r = upperBound(nums, target);
        
        if (l == nums.length || nums[l] != target) {
            return new int[]{-1, -1};
        }
        
        return new int[]{l, r - 1};
    }
    
    private int lowerBound(int[] nums, int target) {
        int left = 0;
        int right = nums.length;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        return left;
    }
    
    private int upperBound(int[] nums, int target) {
        int left = 0;
        int right = nums.length;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] <= target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        return left;
    }
}
```

</details>


<details id="74. Search a 2D Matrix">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">74. Search a 2D Matrix 
</span>
</summary>

https://leetcode.com/problems/search-a-2d-matrix/description/

```java
//Approach-2 (Using Binary Search) O(log(m*n))
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        int n = matrix[0].size();
        
        int start = 0;
        int end   = m*n-1;
        
        while(start <= end) {
            int mid = start + (end-start)/2;
            
            int row = mid/n;
            int col = mid%n;
            
            if(matrix[row][col] > target) {
                end = mid-1;
            } else if(matrix[row][col] < target) {
                start = mid+1;
            } else {
                return true;
            }
        }
        
        return false;
        
    }
};

```

</details>

<details id="81. Search in Rotated Sorted Array II">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">81. Search in Rotated Sorted Array II 
</span>
</summary>

https://leetcode.com/problems/search-in-rotated-sorted-array-ii/description/

```java
class Solution {
    public boolean search(int[] nums, int target) {
        int pivit=findPivit(nums);
        if(binarySearch(nums, 0, pivit-1, target))return true;
        return binarySearch(nums, pivit, nums.length-1, target);
    }

    boolean binarySearch(int[] nums, int start, int end, int target){
        int l=start, r=end;

        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]==target)return true;
            if(nums[mid]>target){
                r=mid-1;
            }else l=mid+1;
        }
        return false;
    }

    int findPivit(int[] nums){
        int l=0, r=nums.length-1;

        while(l<r){
            while(l<r && nums[l]==nums[l+1])l++;
            while(l<r && nums[r]==nums[r-1])r--;
            int mid=l+(r-l)/2;
            if(nums[mid]>nums[r])l=mid+1;
            else r=mid;
        }
        return r;
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

