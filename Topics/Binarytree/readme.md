# Binary Tree

<details id="199. Binary Tree Right Side View">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">199. Binary Tree Right Side View
</span>
</summary>

```java
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> rsv=new ArrayList<>();
        Queue<TreeNode> queue=new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            int size=queue.size();
            for(int i=0;i<size;i++){
                TreeNode node=queue.poll();
                if(node!=null){
                    if(node.left !=null) queue.add(node.left);
                    if(node.right !=null) queue.add(node.right);
                    if(i == size-1){
                        rsv.add(node.val);
                    }
                }
            }
        }
        return rsv;
    }
```
</details>

<details id="105. Construct Binary Tree from Preorder and Inorder Traversal">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">105. Construct Binary Tree from Preorder and Inorder Traversal
</span>
</summary>

```java
class Solution {
    int i=0;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n= preorder.length-1;
        return makeTreeRecursively(preorder, inorder, 0,n);
    }
    private TreeNode makeTreeRecursively(int[] preorder, int[] inorder,  int l, int r){
        if(l>r)return null;
        int rootVal=preorder[i];
        int rootIndex=l;
        for(int idx=l;idx<=r;idx++){
            if(inorder[idx]==rootVal){
                rootIndex=idx;break;
            }
        }
        TreeNode root=new TreeNode(rootVal);
        i++;
        root.left= makeTreeRecursively( preorder, inorder,l , rootIndex-1);
        root.right= makeTreeRecursively( preorder, inorder, rootIndex+1, r);
        return root;
    }
}

// Optimization using Map

class Solution {
    int i=0;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n= preorder.length-1;
        Map<Integer,Integer> map=new HashMap<>();
        for(int i=0;i<inorder.length;i++)map.put(inorder[i], i);
        return makeTreeRecursively(preorder, inorder,map, 0,n);
    }
    private TreeNode makeTreeRecursively(int[] preorder, int[] inorder,Map<Integer,Integer> map,  int l, int r){
        if(l>r)return null;
        int rootVal=preorder[i];
        int rootIndex=map.get(rootVal);
        
        TreeNode root=new TreeNode(rootVal);
        i++;
        root.left= makeTreeRecursively( preorder, inorder,map,l , rootIndex-1);
        root.right= makeTreeRecursively( preorder, inorder,map, rootIndex+1, r);
        return root;
    }
}
```
</details>

<details id="236. Lowest Common Ancestor of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">236. Lowest Common Ancestor of a Binary Tree
</span>
</summary>

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null)return null;
        if(root.val == p.val || root.val == q.val){
            return root;
        }
        TreeNode left=lowestCommonAncestor(root.left, p,q);
        TreeNode right=lowestCommonAncestor(root.right, p,q);

        if(left!=null && right!=null)return root;
        if(left==null)return right;
        if(right==null)return left;
        return null;
    }
}
```
</details>

<details id="958. Check Completeness of a Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">958. Check Completeness of a Binary Tree
</span>
</summary>

```java
class Solution {
    public boolean isCompleteTree(TreeNode root) {
        Queue<TreeNode> q=new LinkedList<>();
        if(root==null)return true;
        q.offer(root);
        while(!q.isEmpty()){
                TreeNode node=q.poll();
                if(node != null )q.offer(node.left);
                if(node != null )q.offer(node.right);
                if(node==null){
                    while(!q.isEmpty()){
                        if(q.poll()!=null)return false;
                    }
                }
            }
    return true;
    }
}
```

</details>

<details id="1110. Delete Nodes And Return Forest">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">1110. Delete Nodes And Return Forest
</span>
</summary>

```java
class Solution {
    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
        List<TreeNode> ans=new ArrayList<>();
        Set<Integer> set=new HashSet<>();

        for(int i:to_delete)set.add(i);
        bottomUpDelete(ans, set, root);
        if(!set.contains(root.val))ans.add(root);
        return ans;
    }
    private TreeNode bottomUpDelete(List<TreeNode> ans,  Set<Integer> set, TreeNode root){
        if(root==null) return null;
        root.left=bottomUpDelete(ans, set, root.left);
        root.right=bottomUpDelete(ans, set, root.right);
        if(set.contains(root.val)){
            if(root.left!=null)ans.add(root.left);
            if(root.right!=null)ans.add(root.right);
            return null;
        }
        return root;
    }
}
```

</details>

<details id="814. Binary Tree Pruning">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">814. Binary Tree Pruning
</span>
</summary>

```java
class Solution {
    public TreeNode pruneTree(TreeNode root) {
        if(root==null)return null;

        root.left=pruneTree(root.left);
        root.right=pruneTree(root.right);

        if(root.left==null && root.right==null && root.val == 0){
            return null;
        }
        return root;
    }
}
```

</details>


<details id="113. Path Sum II">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">113. Path Sum II
</span>
</summary>

```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> ans=new ArrayList<>();
        if(root==null)return ans;
        dfs(root, targetSum, ans, new ArrayList<Integer>());
        return ans;
    }
    public void dfs(TreeNode root, int target, List<List<Integer>> ans, List<Integer> sub){
        if(root.left==null && root.right==null && target==root.val){
            sub.add(root.val);
            ans.add(new ArrayList<>(sub));
            sub.remove(sub.size()-1);
            return;
        }
        sub.add(root.val);
        if(root.left!=null)dfs(root.left, target-root.val, ans, sub);
        if(root.right!=null)dfs(root.right, target-root.val, ans, sub);
        sub.remove(sub.size()-1);
    }
}
```

</details>



<details id="623. Add One Row to Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">623. Add One Row to Tree
</span>
</summary>

```java
class Solution {
    public TreeNode addOneRow(TreeNode root, int val, int depth) {
        if(depth ==1){
            TreeNode newRoot=new TreeNode(val);
            newRoot.left=root;
            return newRoot;
        }
        int curr=1;
        return add(root, val, depth, curr); 
    }
    private TreeNode add(TreeNode root, int val, int depth, int curr){
        if(root==null)return null;
        
        if(curr == depth-1){
            TreeNode templ=root.left;
            TreeNode tempr=root.right;

            root.left=new TreeNode(val);
            root.right=new TreeNode(val);

            root.left.left=templ;
            root.right.right=tempr;
            return root;
        }
        root.left=add(root.left,val,depth, curr+1);
        root.right=add(root.right,val,depth, curr+1);

        return root;
    }
}
```
</details>


<details id="222. Count Complete Tree Nodes">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">222. Count Complete Tree Nodes
</span>
</summary>

```java
class Solution {
    private int leftHeight(TreeNode root){
        if(root == null)return 0;
        int height=0;
        while(root!=null){
            height++;
            root=root.left;
        }
        return height;
    } 
    private int rightHeight(TreeNode root){
        if(root == null)return 0;
        int height=0;
        while(root!=null){
            height++;
            root=root.right;
        }
        return height;
    } 
    public int countNodes(TreeNode root) {
        if(root == null)return 0;

        int left=leftHeight(root);
        int right=rightHeight(root);

        if(left == right)return  (int)Math.pow(2,left)-1;

        return countNodes(root.left) +  countNodes(root.right) + 1;
    }
}
```
</details>




<details id="1026. Maximum Difference Between Node and Ancestor">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">1026. Maximum Difference Between Node and Ancestor
</span>
</summary>

```java
class Solution {
    public int maxAncestorDiff(TreeNode root) {
        return dfs(root, Integer.MAX_VALUE, Integer.MIN_VALUE);
    }
    private int dfs(TreeNode root, int min,int max){
        if(root ==null)return Math.abs(min-max);
        int minimum=Math.min(min, root.val);
        int maximum=Math.max(max, root.val);
        return Math.max(dfs(root.left, minimum, maximum), dfs(root.right, minimum, maximum));
    }
}class Solution {
    public int maxAncestorDiff(TreeNode root) {
        return dfs(root, Integer.MAX_VALUE, Integer.MIN_VALUE);
    }
    private int dfs(TreeNode root, int min,int max){
        if(root ==null)return Math.abs(min-max);
        int minimum=Math.min(min, root.val);
        int maximum=Math.max(max, root.val);
        return Math.max(dfs(root.left, minimum, maximum), dfs(root.right, minimum, maximum));
    }
}
```
</details>




<details id="124. Binary Tree Maximum Path Sum">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">124. Binary Tree Maximum Path Sum
</span>
</summary>

```java
class Solution {
    int maxSum=Integer.MIN_VALUE;
    private int dfs(TreeNode root) {
        if(root==null)return 0;

        int left=dfs(root.left);
        int right=dfs(root.right);

        // we found a path
        maxSum=Math.max(maxSum, left+right+root.val);
        // take left and root and return that 
        maxSum=Math.max(maxSum, left+root.val);
        // take right and root and return that 
        maxSum=Math.max(maxSum, right+root.val);
        // just take root and return that
        maxSum=Math.max(maxSum, root.val);
        return Math.max(root.val, Math.max(right + root.val, left+root.val));
    }
    public int maxPathSum(TreeNode root) {
        dfs(root);
        return maxSum;
    }
}
```
</details>




<details id="652. Find Duplicate Subtrees">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">652. Find Duplicate Subtrees
</span>
</summary>

```java
class Solution {
    public String dfs(TreeNode root,  HashMap<String,Integer> hm, List<TreeNode> ans) {
        if(root==null)return "NULL";
        String str=root.val+","+dfs(root.left,hm,ans)+","+dfs(root.right,hm,ans);
        if(hm.containsKey(str) && hm.get(str)==1){
            ans.add(root);
        }
        hm.put(str,hm.getOrDefault(str,0)+1);
        return str;
    }
    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        HashMap<String,Integer> hm=new HashMap<>();
        List<TreeNode> ans=new ArrayList<>();
        dfs(root,hm,ans);
        return ans;
    }
}
```
</details>




<details id="2458. Height of Binary Tree After Subtree Removal Queries">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">2458. Height of Binary Tree After Subtree Removal Queries
</span>
</summary>

```java

https://leetcode.com/problems/height-of-binary-tree-after-subtree-removal-queries/description/?envType=daily-question&envId=2024-10-26


You are given the root of a binary tree with n nodes. Each node is assigned a unique value from 1 to n. You are also given an array queries of size m.

You have to perform m independent queries on the tree where in the ith query you do the following:

Remove the subtree rooted at the node with the value queries[i] from the tree. It is guaranteed that queries[i] will not be equal to the value of the root.
Return an array answer of size m where answer[i] is the height of the tree after performing the ith query.

Note:

The queries are independent, so the tree returns to its initial state after each query.
The height of a tree is the number of edges in the longest simple path from the root to some node in the tree.
 

Example 1:


Input: root = [1,3,4,2,null,6,5,null,null,null,null,null,7], queries = [4]
Output: [2]
Explanation: The diagram above shows the tree after removing the subtree rooted at node with value 4.
The height of the tree is 2 (The path 1 -> 3 -> 2).
Example 2:


Input: root = [5,8,9,2,1,3,7,4,6], queries = [3,2,4,8]
Output: [3,2,3,2]
Explanation: We have the following queries:
- Removing the subtree rooted at node with value 3. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 4).
- Removing the subtree rooted at node with value 2. The height of the tree becomes 2 (The path 5 -> 8 -> 1).
- Removing the subtree rooted at node with value 4. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 6).
- Removing the subtree rooted at node with value 8. The height of the tree becomes 2 (The path 5 -> 9 -> 3).
 

Constraints:

The number of nodes in the tree is n.
2 <= n <= 105
1 <= Node.val <= n
All the values in the tree are unique.
m == queries.length
1 <= m <= min(n, 104)
1 <= queries[i] <= n
queries[i] != root.val
```java

class Solution {
    public int[] treeQueries(TreeNode root, int[] queries) {
        int[] heightsMap = new int[(int) Math.pow(10, 5) + 1];
        int[] nodeLevel = new int[(int) Math.pow(10, 5) + 1];
        int[] maxHeightNodeAtLevel = new int[(int) Math.pow(10, 5) + 1];
        int[] secondMaxHeightNodeAtLevel = new int[(int) Math.pow(10, 5) + 1];

        initialize(heightsMap, nodeLevel, maxHeightNodeAtLevel, secondMaxHeightNodeAtLevel, root, 0);
        int[] heightQueries = new int[queries.length];
        for (int i = 0; i < queries.length; i++) {
            int L = nodeLevel[queries[i]];
            int tempResultantHeight = heightsMap[queries[i]] == maxHeightNodeAtLevel[L]
                    ? secondMaxHeightNodeAtLevel[L]
                    : maxHeightNodeAtLevel[L];
            heightQueries[i] = L + tempResultantHeight - 1;
        }
        return heightQueries;
    }

    private int initialize(int[] heightsMap, int[] nodeLevel, int[] maxHeightNodeAtLevel, int[] secondMaxHeightNodeAtLevel,
            TreeNode root, int level) {
        if (root == null)
            return 0;

        int left = initialize(heightsMap, nodeLevel, maxHeightNodeAtLevel, secondMaxHeightNodeAtLevel, root.left, level + 1);
        int right = initialize(heightsMap, nodeLevel, maxHeightNodeAtLevel, secondMaxHeightNodeAtLevel, root.right, level + 1);
        int height = 1 + Math.max(left, right);
        nodeLevel[root.val] = level;

        if (height > maxHeightNodeAtLevel[level]) {
            secondMaxHeightNodeAtLevel[level] = maxHeightNodeAtLevel[level];
            maxHeightNodeAtLevel[level] = height;
        } else if (height > secondMaxHeightNodeAtLevel[level]) {
            secondMaxHeightNodeAtLevel[level] = height;
        }

        return heightsMap[root.val] = 1 + Math.max(left, right);
    }
}
```





</details>





<details id="297. Serialize and Deserialize Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">297. Serialize and Deserialize Binary Tree
</span>
</summary>

https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

Clarification: The input/output format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

 

Example 1:


Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
Example 2:

Input: root = []
Output: []
 

Constraints:

The number of nodes in the tree is in the range [0, 104].
-1000 <= Node.val <= 1000

```java

/**
 *DFS based solution O(n) time and space 
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        List<String> s_tree = new ArrayList<>();
        dfs_serilize(root, s_tree);
        return String.join(",", s_tree);
    }

    public void dfs_serilize(TreeNode root, List<String> s_tree) {
        if(root == null){
            s_tree.add("N");
            return;
        }
        s_tree.add(String.valueOf(root.val));
        dfs_serilize(root.left, s_tree);
        dfs_serilize(root.right, s_tree);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] s_tree=data.split(",");
        int[] index=new int[]{0};
        return dfs_deserialize(s_tree, index);
    }

    public TreeNode dfs_deserialize(String[] s_tree, int[] index){
        if(s_tree[index[0]].equals("N")){
            return null;
        }
        TreeNode newNode=new TreeNode(Integer.parseInt(s_tree[index[0]]));
        index[0]++;
        newNode.left= dfs_deserialize(s_tree, index);
        index[0]++;
        newNode.right= dfs_deserialize(s_tree, index);
        return newNode;
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