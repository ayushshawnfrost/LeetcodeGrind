

# Sorting

## Selection Sort

Selection Sort divides the array into two parts:
A sorted subarray at the beginning.
An unsorted subarray for the rest.
It repeatedly selects the smallest (or largest) element from the unsorted part and moves it to the sorted part.

    Steps:
        1. Select minimum
        2. Swap minimum with the starting index, increment starting index

    Obserations:
        1. Sort from left to right
        2. O(n^2)
        3. Not Stable

```java
int[] selectionSort(int[] nums){
    int len=nums.length;
    for(int i=0;i<len-1;i++){

        int min = i;
        for(int j=i;j<len;j++){
            if(nums[j]<nums[min]){
                min=j;
            }
        }
        // Swap
        int temp=nums[min];
        nums[min]=nums[i];
        nums[i]=temp;
    }
    return nums;
}

```
## Bubble Sort

Bubble Sort repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order.
After each pass, the largest (or smallest) element is "bubbled" to its correct position.

    Steps:
        1. push the max element to its right place
        2. By adjcent swapping

    Obserations:
        3. Sort from right to left
        4. O(n^2)
        5. Not Stable

```java
  static void bubble_sort(int[] arr, int n) {
        for (int i = n - 1; i >= 0; i--) {
            for (int j = 0; j <= i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }

```

<details id="4. Median of Two Sorted Arrays">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">4. Median of Two Sorted Arrays
</span>
</summary>

https://leetcode.com/problems/median-of-two-sorted-arrays/description/

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log (m+n)).

 

Example 1:

Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
Example 2:

Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
 

Constraints:

nums1.length == m
nums2.length == n
0 <= m <= 1000
0 <= n <= 1000
1 <= m + n <= 2000
-106 <= nums1[i], nums2[i] <= 106

    Strategry:

    We need to find the median, the middle element. We will divide the 2 arrays in 2 parts each. 
    We will try to fix the size of 1st part of 1st array. This will fix all the other parameters like the size of 2nd part of 1st array, the size of 1st and 2nd part of the 2nd array. We will use 4 corner elements to run the Binary search 

    A1, A2, A3 ||  A4, A5

        B1, B2 ||  B3, B4
    


```java
// Bruteforce O(n+m)
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int size1 = nums1.length;
        int size2 = nums2.length;
        int index_1 = 0;
        int index_2 = 0;

        int first_median_index = -1;
        int second_median_index = -1;

        int median1 = -1;
        int median2 = -1;

        if ((size1 + size2) % 2 == 0) {
            first_median_index = (size1 + size2) / 2 - 1;
            second_median_index = (size1 + size2) / 2;
        } else {
            first_median_index = (size1 + size2) / 2;
        }

        while (index_1 < size1 && index_2 < size2) {
            if (index_1 + index_2 == first_median_index) {
                median1 = nums1[index_1] < nums2[index_2] ? nums1[index_1] : nums2[index_2];
            } else if (index_1 + index_2 == second_median_index) {
                median2 = nums1[index_1] < nums2[index_2] ? nums1[index_1] : nums2[index_2];
            }
            if (nums1[index_1] < nums2[index_2]) {
                index_1++;
            } else {
                index_2++;
            }
        }

        while (index_1 < size1) {
             if (index_1 + index_2 == first_median_index) {
                median1 = nums1[index_1];
            } else if (index_1 + index_2 == second_median_index) {
                median2 = nums1[index_1];
            }
            index_1++;
        }
        
        while (index_2 < size2) {
             if (index_1 + index_2 == first_median_index) {
                median1 = nums2[index_2];
            } else if (index_1 + index_2 == second_median_index) {
                median2 = nums2[index_2];
            }
            index_2++;
        }

        if((size1+size2)%2 == 0){
            return (median1+median2)/2.0;
        }
        return median1 * 1.0;
    }
}
```

```java
// Optimized 
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int m = nums2.length;

        if (n > m)
            return findMedianSortedArrays(nums2, nums1);

        /*
         * i would like to divide the arrays in 2 pieces each
         * The first piece will be of length [(m+n+1)/2] and the second pieces combined
         * will be [m+n - (m+n+1)/2]
         * int partA=(m+n+1)/2
         * int partB=m+n - (m+n+1)/2
         */
        
        int l = 0;
        int r = n;

        while (l <= r) {
            int partition1  = l + (r - l) / 2; // mid
            int partition2  = (m + n + 1) / 2 - partition1;

            // Value of partition can be 
            
            // left side waale
            int leftMax1  = partition1  == 0 ? Integer.MIN_VALUE : nums1[partition1  - 1]; // 1
            int leftMax2  = partition2  == 0 ? Integer.MIN_VALUE : nums2[partition2  - 1]; // 3

            // right side waale
            int rightMin1  = partition1  == n ? Integer.MAX_VALUE : nums1[partition1 ]; // 2
            int rightMin2  = partition2  == m ? Integer.MAX_VALUE : nums2[partition2 ]; // MAX

            if (leftMax1  <= rightMin2  && leftMax2  <= rightMin1 ) {
                if ((m + n) % 2 == 1) {
                    return Math.max(leftMax1 , leftMax2 );
                } else {
                    return (Math.max(leftMax1 , leftMax2 ) + Math.min(rightMin1 , rightMin2 )) / 2.0;
                }
            }
            if (leftMax1  > rightMin2 ) {
                r = partition1  - 1;
            } else {
                l = partition1  + 1;
            }
        }
        return -1;
    }
}
```
</details>






<details id="33. Search in Rotated Sorted Array">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">33. Search in Rotated Sorted Array
</span>
</summary>

https://leetcode.com/problems/search-in-rotated-sorted-array/description/

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
Example 3:

Input: nums = [1], target = 0
Output: -1
 

Constraints:

1 <= nums.length <= 5000
-104 <= nums[i] <= 104
All values of nums are unique.
nums is an ascending array that is possibly rotated.
-104 <= target <= 104



```java
```



why we compare nums[mid] with nums[r] in finding the pivot element of a rotated sorted array?
Let's break this down step by step:

First, let's understand what we're looking for. In a rotated sorted array like [4,5,6,7,0,1,2], we're trying to find the smallest element (0 in this case), which is our pivot.
Now, let's analyze why comparing with right (nums[r]) works:
CopyOriginal array: [4,5,6,7,0,1,2]
                    ↑     ↑     ↑
                    l    mid    r

When nums[mid] > nums[r]: (6 > 2)
- This means mid is in the left portion of the rotated array
- The pivot must be to the right of mid
- So we can safely do l = mid + 1

If we tried to compare with left (nums[l]), it wouldn't work reliably. Here's why:
CopyCase 1: [4,5,6,7,0,1,2]
            ↑     ↑
            l    mid
nums[mid] > nums[l] (6 > 4)

Case 2: [6,7,0,1,2,4,5]
        ↑     ↑
        l    mid
nums[mid] < nums[l] (1 < 6)

The problem with comparing with nums[l] is:

When nums[mid] > nums[l], the pivot could be either to the left OR right of mid
When nums[mid] < nums[l], the pivot could also be either to the left OR right of mid
There's no reliable way to determine which half to eliminate


However, comparing with nums[r] always gives us a definite answer:

If nums[mid] > nums[r]: pivot MUST be to the right
If nums[mid] ≤ nums[r]: pivot MUST be at mid or to the left



Here's a visualization of different cases:
CopyCase 1: [4,5,6,7,0,1,2]  // Rotated array
            ↑     ↑     ↑
            l    mid    r
nums[mid] > nums[r] (6 > 2) => pivot is to the right

Case 2: [6,7,0,1,2,4,5]  // Different rotation
         ↑     ↑     ↑
         l    mid    r
nums[mid] ≤ nums[r] (1 ≤ 5) => pivot is at mid or to the left

Case 3: [2,4,5,6,7,0,1]  // Another rotation
         ↑     ↑     ↑
         l    mid    r
nums[mid] > nums[r] (6 > 1) => pivot is to the right
Therefore, comparing with nums[r] gives us a reliable way to determine which half of the array to eliminate in our binary search, while comparing with nums[l] doesn't provide this guarantee.
A key principle in binary search is that we need a reliable way to eliminate half of the search space in each iteration. The comparison with nums[r] provides exactly that, while comparison with nums[l] doesn't.



What about if you want to find the largest element?

Yes, that's a great observation! When finding the largest element in a rotated sorted array, comparing nums[mid] with nums[l] works better. Let me explain why:

First, let's understand what we're looking for. In a rotated sorted array like [4,5,6,7,0,1,2], we're trying to find the largest element (7 in this case).
Here's how comparing with left (nums[l]) works for finding the maximum:

javaCopyprivate int findMax(int[] nums) {
    int l = 0, r = nums.length - 1;
    
    while (l < r) {
        int mid = l + (r - l) / 2;
        
        if (nums[mid] >= nums[l]) {
            // Maximum must be in the right portion or at mid
            // We keep mid as a candidate
            l = mid;
        } else {
            // Maximum must be in the left portion
            r = mid - 1;
        }
    }
    return nums[l];
}
Let's analyze why this works:
CopyCase 1: [4,5,6,7,0,1,2]
         ↑     ↑     
         l    mid    
nums[mid] >= nums[l] (6 >= 4)
- This means mid is in ascending sequence from l
- Maximum could be at mid or to the right
- So we do l = mid (keeping mid as candidate)

Case 2: [6,7,0,1,2,4,5]
         ↑     ↑     
         l    mid    
nums[mid] < nums[l] (1 < 6)
- This means we've crossed the rotation point
- Maximum must be to the left of mid
- So we do r = mid - 1

Case 3: [2,4,5,6,7,0,1]
         ↑     ↑     
         l    mid    
nums[mid] >= nums[l] (6 >= 2)
- Ascending sequence from l to mid
- Maximum could be at mid or to the right
- So we do l = mid
The key difference between finding minimum (pivot) and maximum:

For minimum (pivot):

javaCopyif (nums[mid] > nums[r]) {
    l = mid + 1;  // pivot is to the right
} else {
    r = mid;      // pivot is at mid or to the left
}

For maximum:

javaCopyif (nums[mid] >= nums[l]) {
    l = mid;      // max is at mid or to the right
} else {
    r = mid - 1;  // max is to the left
}
This pattern emerges because:

The minimum (pivot) element is where the descending step happens in the array
The maximum element is right before the descending step
By comparing with left for maximum, we can tell if we're still in the ascending sequence
By comparing with right for minimum, we can tell if we've passed the descending step

So you're absolutely correct - the comparison point (left vs right) changes based on whether we're looking for the maximum or minimum element in the rotated sorted array!




</details>



<details id="Sum of Query II (Segment Tree)">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">Sum of Query II (Segment Tree)
</span>
</summary>

https://www.geeksforgeeks.org/problems/sum-of-query-ii5310/1

You are given an array arr[] of n integers and q queries in an array queries[] of length 2*q containing l, r pair for all q queries. You need to compute the following sum over q queries.

Note : Array is 1-Indexed.

Examples :

Input: n = 4, arr = {1, 2, 3, 4}, q = 2, queries = {1, 4, 2, 3}
Output: 10 5
Explaination: In the first query we need sum from 1 to 4 which is 1+2+3+4 = 10. In the second query we need sum from 2 to 3 which is 2 + 3 = 5.
Input: n = 5, arr = {26, 30, 48, 29, 8}, q = 2, queries = {4, 4, 2, 3}
Output: 29 78
Explaination: In the first query we need sum from 4 to 4 which is 29. In the second query we need sum from 2 to 3 which is 30 + 48 = 78.
Your Task:
You don't need to read input or print anything. Your task is to complete the function querySum() which takes n, arr, q and queries as input parameters and returns the answer for all the queries.

Expected Time Complexity: O(n+q)
Expected Auxiliary Space: O(n)

Constraints:
1 ≤ n, q ≤ 105
1 ≤ arri ≤ 103
1 ≤ l ≤ r ≤ n


```java


// Segment Tree Solution

class Solution {
    List<Integer> querySum(int arraySize, int arr[], int queryCount, int queries[]) {
        // Build the segment tree
        int[] segmentTree = new int[4 * arraySize];
        buildSegmentTree(arr, 0, arraySize - 1, segmentTree, 0);

        // Prepare the result list
        List<Integer> resultSums = new ArrayList<>();

        // Process each query
        for (int i = 0; i < 2 * queryCount; i += 2) {
            int queryLeft = queries[i] - 1;  // Convert 1-indexed to 0-indexed
            int queryRight = queries[i + 1] - 1;
            resultSums.add(rangeQuery(0, arraySize - 1, queryLeft, queryRight, segmentTree, 0));
        }

        return resultSums;
    }

    // Build the segment tree
    void buildSegmentTree(int[] inputArray, int segmentStart, int segmentEnd, int[] segmentTree, int treeIndex) {
        if (segmentStart == segmentEnd) {
            segmentTree[treeIndex] = inputArray[segmentStart];
            return;
        }

        int mid = segmentStart + (segmentEnd - segmentStart) / 2;
        buildSegmentTree(inputArray, segmentStart, mid, segmentTree, 2 * treeIndex + 1);
        buildSegmentTree(inputArray, mid + 1, segmentEnd, segmentTree, 2 * treeIndex + 2);

        segmentTree[treeIndex] = segmentTree[2 * treeIndex + 1] + segmentTree[2 * treeIndex + 2];
    }

    // Query the segment tree for the range sum
    int rangeQuery(int segmentStart, int segmentEnd, int queryLeft, int queryRight, int[] segmentTree, int treeIndex) {
        // Completely outside the range
        if (segmentStart > queryRight || segmentEnd < queryLeft) {
            return 0;
        }

        // Completely inside the range
        if (segmentStart >= queryLeft && segmentEnd <= queryRight) {
            return segmentTree[treeIndex];
        }

        // Partially overlapping range
        int mid = segmentStart + (segmentEnd - segmentStart) / 2;
        return rangeQuery(segmentStart, mid, queryLeft, queryRight, segmentTree, 2 * treeIndex + 1) +
               rangeQuery(mid + 1, segmentEnd, queryLeft, queryRight, segmentTree, 2 * treeIndex + 2);
    }
}


```
</details>




<details id="Range Minimum Query">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">Range Minimum Query
</span>
</summary>

https://www.geeksforgeeks.org/problems/range-minimum-query/1

Given an array A[ ] and its size N your task is to complete two functions  a constructST  function which builds the segment tree  and a function RMQ which finds range minimum query in a range [a,b] of the given array.

Input:
The task is to complete two functions constructST and RMQ.
The constructST function builds the segment tree and takes two arguments the array A[ ] and the size of the array N.
It returns a pointer to the first element of the segment tree array.
The RMQ function takes 4 arguments the first being the segment tree st constructed, second being the size N and then third and forth arguments are the range of query a and b. The function RMQ returns the min of the elements in the array from index range a and b. There are multiple test cases. For each test case, this method will be called individually.

Output:
The function RMQ should return the min element in the array from range a to b.

Example:

Input (To be used only for expected output) 
1
4
1 2 3 4
2
0 2 2 3
Output
1 3
Explanation
1. For query 1 ie 0 2 the element in this range are 1 2 3 
   and the min element is 1. 
2. For query 2 ie 2 3 the element in this range are 3 4 
   and the min element is 3.
Constraints:
1<=T<=100
1<=N<=10^3+1

1<=A[i]<=10^9
1<=Q(no of queries)<=10000
0<=a<=b




```java
class GfG
{
    static int st[];
    
    public static int[] constructST(int arr[], int n)
    {
          // Build the segment tree
        int[] segmentTree = new int[4 * n]; // Correct size allocation
        buildSegmentTree(arr, 0, n - 1, segmentTree, 0);
        return segmentTree;
    }
    
    // Build the segment tree
    static void buildSegmentTree(int[] inputArray, int segmentStart, int segmentEnd, int[] segmentTree, int treeIndex) {
        if (segmentStart == segmentEnd) {
            segmentTree[treeIndex] = inputArray[segmentStart];
            return;
        }

        int mid = segmentStart + (segmentEnd - segmentStart) / 2;
        buildSegmentTree(inputArray, segmentStart, mid, segmentTree, 2 * treeIndex + 1);
        buildSegmentTree(inputArray, mid + 1, segmentEnd, segmentTree, 2 * treeIndex + 2);

        segmentTree[treeIndex] = Math.min(segmentTree[2 * treeIndex + 1], segmentTree[2 * treeIndex + 2]);
    }
    
    /* The functions returns the
      min element in the range
      from l and r */
       public static int RMQ(int segmentTree[], int n, int l, int r) {
        return rangeQuery(0, n - 1, l, r, segmentTree, 0);
    }

    static int rangeQuery(int segmentStart, int segmentEnd, int queryLeft, int queryRight, int[] segmentTree, int treeIndex) {
        // Completely outside the range
        if (segmentStart > queryRight || segmentEnd < queryLeft) {
            return Integer.MAX_VALUE;
        }

        // Completely inside the range
        if (segmentStart >= queryLeft && segmentEnd <= queryRight) {
            return segmentTree[treeIndex];
        }

        // Partially overlapping range
        int mid = segmentStart + (segmentEnd - segmentStart) / 2;
        return Math.min(rangeQuery(segmentStart, mid, queryLeft, queryRight, segmentTree, 2 * treeIndex + 1),
                rangeQuery(mid + 1, segmentEnd, queryLeft, queryRight, segmentTree, 2 * treeIndex + 2));
    }
    
    
}
```
</details>




<details id="Merge Sort">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">Merge Sort
</span>
</summary>


https://www.naukri.com/code360/problems/merge-sort_920442?utm_source=youtube&utm_medium=affiliate&utm_campaign=striver_Arrayproblems



```java
class Solution {
    private static void merge(int[] arr, int low, int mid, int high) {
        ArrayList<Integer> temp = new ArrayList<>(); // temporary array
        int left = low;      // starting index of left half of arr
        int right = mid + 1;   // starting index of right half of arr

        //storing elements in the temporary array in a sorted manner//

        while (left <= mid && right <= high) {
            if (arr[left] <= arr[right]) {
                temp.add(arr[left]);
                left++;
            } else {
                temp.add(arr[right]);
                right++;
            }
        }

        // if elements on the left half are still left //

        while (left <= mid) {
            temp.add(arr[left]);
            left++;
        }

        //  if elements on the right half are still left //
        while (right <= high) {
            temp.add(arr[right]);
            right++;
        }

        // transfering all elements from temporary to arr //
        for (int i = low; i <= high; i++) {
            arr[i] = temp.get(i - low);
        }
    }

    public static void mergeSort(int[] arr, int low, int high) {
        if (low >= high) return;
        int mid = (low + high) / 2 ;
        mergeSort(arr, low, mid);  // left half
        mergeSort(arr, mid + 1, high); // right half
        merge(arr, low, mid, high);  // merging sorted halves
    }
}
public class tUf {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        int n = 7;
        int arr[] = { 9, 4, 7, 6, 3, 1, 5 };
        System.out.println("Before sorting array: ");
        for (int i = 0; i < n; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
        Solution.mergeSort(arr, 0, n - 1);
        System.out.println("After sorting array: ");
        for (int i = 0; i < n; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }

}
```

    Time complexity: O(nlogn) 

    Reason: At each step, we divide the whole array, for that logn and we assume n steps are taken to get a sorted array, so overall time complexity will be nlogn

    Space complexity: O(n)  

    Reason: We are using a temporary array to store elements in sorted order.

    Auxiliary Space Complexity: O(n)
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
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


<!--

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
```
</details> 
-->


