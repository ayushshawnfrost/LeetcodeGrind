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

https://leetcode.com/problems/binary-tree-maximum-path-sum/description/

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.

 

Example 1:


Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
Example 2:


Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
 

Constraints:

The number of nodes in the tree is in the range [1, 3 * 104].
-1000 <= Node.val <= 1000


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
```java

public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) return "N";
        StringBuilder res = new StringBuilder();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (node == null) {
                res.append("N,");
            } else {
                res.append(node.val).append(",");
                queue.add(node.left);
                queue.add(node.right);
            }
        }
        return res.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] vals = data.split(",");
        if (vals[0].equals("N")) return null;
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int index = 1;

        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (!vals[index].equals("N")) {
                node.left = new TreeNode(Integer.parseInt(vals[index]));
                queue.add(node.left);
            }
            index++;
            if (!vals[index].equals("N")) {
                node.right = new TreeNode(Integer.parseInt(vals[index]));
                queue.add(node.right);
            }
            index++;
        }
        return root;
    }
}
```
</details>





<details id="449. Serialize and Deserialize BST">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">449. Serialize and Deserialize BST
</span>
</summary>


Serialization is converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary search tree. There is no restriction on how your serialization/deserialization algorithm should work. You need to ensure that a binary search tree can be serialized to a string, and this string can be deserialized to the original tree structure.

The encoded string should be as compact as possible.

 

Example 1:

Input: root = [2,1,3]
Output: [2,1,3]
Example 2:

Input: root = []
Output: []
 

Constraints:

The number of nodes in the tree is in the range [0, 104].
0 <= Node.val <= 104
The input tree is guaranteed to be a binary search tree.
```java


public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serializeHelper(root, sb);
        return sb.toString();
    }

    private void serializeHelper(TreeNode node, StringBuilder sb) {
        if (node == null) {
            return;
        }
        sb.append(node.val).append(",");  // Append the value followed by a comma
        serializeHelper(node.left, sb);   // Serialize left subtree
        serializeHelper(node.right, sb);  // Serialize right subtree
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data.isEmpty()) return null;
        Queue<Integer> nodes = new LinkedList<>();
        for (String s : data.split(",")) {
            nodes.offer(Integer.parseInt(s));
        }
        return deserializeHelper(nodes, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }

    private TreeNode deserializeHelper(Queue<Integer> nodes, int lower, int upper) {
        if (nodes.isEmpty()) return null;

        int val = nodes.peek();
        if (val < lower || val > upper) return null;

        nodes.poll();  // Remove the element from the queue
        TreeNode root = new TreeNode(val);
        root.left = deserializeHelper(nodes, lower, val);   // Values for the left subtree are < val
        root.right = deserializeHelper(nodes, val, upper);  // Values for the right subtree are > val
        return root;
    }
}


```
</details>




<details id="515. Find Largest Value in Each Tree Row">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">515. Find Largest Value in Each Tree Row
</span>
</summary>

https://leetcode.com/problems/find-largest-value-in-each-tree-row/description/


```java

class Solution {
    public List<Integer> largestValues(TreeNode root) {
        Map<Integer, Integer> maxElement = new HashMap<>();
        inorder(root, maxElement, 0);
        return new ArrayList<Integer>(maxElement.values());
    }

    private void inorder(TreeNode root, Map<Integer, Integer> maxElement, int level) {
        if(root==null)return;
        if (maxElement.containsKey(level)) {
            maxElement.put(level, Math.max(maxElement.get(level), root.val));
        } else {
            maxElement.put(level, root.val);
        }
        inorder(root.left, maxElement, level + 1);
        inorder(root.right, maxElement, level + 1);
    }
}
```
</details>






<details id="110. Balanced Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">110. Balanced Binary Tree
</span>
</summary>

Given a binary tree, determine if it is 
height-balanced

    Height-Balanced
    A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

.


```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        // Call the helper function and check if it returns -1 (indicating imbalance)
        return checkHeight(root) != -1;
    }
    
    // Helper function to calculate height and check balance
    private int checkHeight(TreeNode node) {
        // An empty tree is balanced with a height of 0
        if (node == null) return 0;
        
        // Recursively check the height of the left subtree
        int leftHeight = checkHeight(node.left);
        if (leftHeight == -1) return -1; // If left subtree is unbalanced, propagate -1 up
        
        // Recursively check the height of the right subtree
        int rightHeight = checkHeight(node.right);
        if (rightHeight == -1) return -1; // If right subtree is unbalanced, propagate -1 up
        
        // If the difference in heights is more than 1, mark as unbalanced
        if (Math.abs(leftHeight - rightHeight) > 1) return -1;
        
        // Return the height of the current node
        return Math.max(leftHeight, rightHeight) + 1;
    }
}

```
</details>



<details id="543. Diameter of Binary Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">543. Diameter of Binary Tree
</span>
</summary>

https://leetcode.com/problems/diameter-of-binary-tree/description/

Given the root of a binary tree, return the length of the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

The length of a path between two nodes is represented by the number of edges between them.

```java
class Solution {
    public int diameterOfBinaryTree(TreeNode root) {
        return calculateDiameter(root).diameter;
    }

    private Result calculateDiameter(TreeNode node) {
        // Base case: If the node is null, height and diameter are 0
        if (node == null) return new Result(0, 0);

        // Recursively calculate the left and right results
        Result leftResult = calculateDiameter(node.left);
        Result rightResult = calculateDiameter(node.right);
        
        // Calculate height of the current node
        int height = Math.max(leftResult.height, rightResult.height) + 1;
        
        // Calculate diameter passing through the current node
        int diameterThroughNode = leftResult.height + rightResult.height;
        
        // Determine the maximum diameter found so far
        int diameter = Math.max(diameterThroughNode, Math.max(leftResult.diameter, rightResult.diameter));
        
        // Return both height and diameter as a Result object
        return new Result(height, diameter);
    }
    
    // Helper class to store both height and diameter
    private static class Result {
        int height;
        int diameter;

        Result(int height, int diameter) {
            this.height = height;
            this.diameter = diameter;
        }
    }
}
```
</details>

 



<details id="98. Validate Binary Search Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">98. Validate Binary Search Tree
</span>
</summary>

https://leetcode.com/problems/validate-binary-search-tree/description/

Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

The left 
subtree
 of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

```java
lass Solution {
    public boolean isValidBST(TreeNode root) {
        return recursivelyCheckBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    private boolean recursivelyCheckBST(TreeNode root, long min, long max){
        if(root == null) return true;
        if(root.val>=max || root.val<=min)return false;

        return recursivelyCheckBST(root.left, min, root.val) && recursivelyCheckBST(root.right, root.val, max);
    }
}



```
</details>






<details id="101. Symmetric Tree">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">101. Symmetric Tree
</span>
</summary>

https://leetcode.com/problems/symmetric-tree/description/

Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

 

Example 1:


Input: root = [1,2,2,3,4,4,3]
Output: true
Example 2:


Input: root = [1,2,2,null,3,null,3]
Output: false
 

Constraints:

The number of nodes in the tree is in the range [1, 1000].
-100 <= Node.val <= 100
 

Follow up: Could you solve it both recursively and iteratively?

```java

// Recursive Solution
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return check(root.right, root.left);
    }

    public boolean check(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null)
            return true;
        if ((t1 == null && t2 != null) || (t1 != null && t2 == null) || (t1.val != t2.val))
            return false;
        return check(t1.left, t2.right) && check(t1.right, t2.left);
    }
}

// Iterative Solution
class Solution {
    public boolean isSymmetric(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        queue.offer(root);

        while (!queue.isEmpty()) {
            TreeNode l = queue.poll();
            TreeNode r = queue.poll();

            if (l == null && r == null)
                continue;
            if (l ==  null || r == null||l.val != r.val)
                return false;

            queue.offer(l.left);
            queue.offer(r.right);
            queue.offer(l.right);
            queue.offer(r.left);
        }
        return true;
    }
}
```
</details>



<details id="103. Binary Tree Zigzag Level Order Traversal">
<summary> 
<span style="color:pink;font-size:16px;font-weight:bold">103. Binary Tree Zigzag Level Order Traversal
</span>
</summary>

https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/

Given the root of a binary tree, return the zigzag level order traversal of its nodes' values. (i.e., from left to right, then right to left for the next level and alternate between).

 

Example 1:


Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]
Example 2:

Input: root = [1]
Output: [[1]]
Example 3:

Input: root = []
Output: []
 

Constraints:

The number of nodes in the tree is in the range [0, 2000].
-100 <= Node.val <= 100

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> ans=new ArrayList<>();
        Queue<TreeNode> q=new LinkedList<>();
        if(root==null)return ans;
        q.add(root);
        int level=1;
        while(!q.isEmpty()){
            List<Integer> subList=new ArrayList<>();
            int size=q.size();
            while(size!=0){
                TreeNode node=q.poll();
                subList.add(node.val);

                if(node.left!=null)q.add(node.left);
                if(node.right!=null)q.add(node.right);

                size--;
            }
            if(level%2==0){
                Collections.reverse(subList);
            }
            ans.add(subList);
            level++;
        }
        return ans;
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
</details> -->