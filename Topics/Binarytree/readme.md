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