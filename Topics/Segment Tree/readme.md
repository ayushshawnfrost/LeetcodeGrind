# Segment Tree

### What is Segment Tree

    A Segment Tree is a binary tree used for storing information about intervals or segments. It allows querying which segment a given point belongs to, or aggregating data over a range of intervals efficiently. Segment Trees are particularly useful in problems involving:

    Range queries (e.g., finding the sum, minimum, maximum, or greatest common divisor of elements in a specific range).
    Point updates (e.g., modifying an element in the range and recalculating the query result).


### When to use Segment Tree

    Frequent Range Queries: You need to perform operations like sum, min, max, or other associative operations over a range multiple times.
    Dynamic Updates: The array or data structure you're querying can change (e.g., element updates or modifications in the range).
    Performance Constraints: The naive approach of recalculating results for range queries or updates is too slow for the problem at hand.


### Time and Space complexity of Segment Tree

1. Building the Segment Tree:
Time Complexity: 
O(n), where 
n is the size of the input array. Each level of the tree processes all elements, and the height of the tree is 
O(logn).

Space Complexity: 
O(n) for storing the tree in an array (usually 
4n for simplicity).

1. Range Queries:
Time Complexity: 
O(logn). At most, you visit nodes equal to the height of the tree during a query.

2. Point Updates:
Time Complexity: 
O(logn). Updates propagate along the height of the tree.
3. Space Complexity:
Total space required is 
O(n) for the array + 
O(logn) for the recursion stack during operations.


### Representations

    1. It is a Balanced Binary Tree (height diff <1), every node will have 2 children
    2. Root Node will represent the whole array
    3. Leaf Nodes will represent single element in the array
    4. Other Nodes will represent a range in the array
    5. Height of the tree => log(n) base 2 (n is size of array)


### Code for building ST

```java
//Using Segment Tree
//T.C : O(q*log(n))
//S.C : O(4*n)

class Solution{
    void buildSegmentTree(int i, int l, int r, int[] segmentTree, int[] arr) {
        if (l == r) {
            segmentTree[i] = arr[l];
            return;
        }
        
        int mid = l + (r - l) / 2;
        buildSegmentTree(2 * i + 1, l, mid, segmentTree, arr);
        buildSegmentTree(2 * i + 2, mid + 1, r, segmentTree, arr);
        segmentTree[i] = segmentTree[2 * i + 1] + segmentTree[2 * i + 2];
    }
    
    int querySegmentTree(int start, int end, int i, int l, int r, int[] segmentTree) {
        if (l > end || r < start) {
            return 0;
        }
        
        if (l >= start && r <= end) {
            return segmentTree[i];
        }
        
        int mid = l + (r - l) / 2;
        return querySegmentTree(start, end, 2 * i + 1, l, mid, segmentTree) + 
               querySegmentTree(start, end, 2 * i + 2, mid + 1, r, segmentTree);
    }
    
    List<Integer> querySum(int n, int[] arr, int q, int[] queries) {
        int[] segmentTree = new int[4 * n];
        
        buildSegmentTree(0, 0, n - 1, segmentTree, arr);
        
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < 2 * q; i += 2) {
            int start = queries[i] - 1;   // Input is in 1-based indexing
            int end = queries[i + 1] - 1; // Input is in 1-based indexing
            
            result.add(querySegmentTree(start, end, 0, 0, n - 1, segmentTree));
        }
        
        return result;
    }
}

```


https://github.com/MAZHARMIK/Interview_DS_Algo/tree/master/Segment%20Tree