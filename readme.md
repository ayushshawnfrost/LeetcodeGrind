# Leetcode Grind

## 78. Subsets : 
https://leetcode.com/problems/subsets/
    For example we wantto print all the permutations of [1,2,3]. the permutations would look like.
    Input: nums = [1,2,3]
    Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

```java
public List<List<Integer>> subsets(int[] nums) {
List<List<Integer>> list = new ArrayList<>();
Arrays.sort(nums);
backtrack(list, new ArrayList<>(), nums, 0);
return list;
}

private void backtrack(List<List<Integer>> list , List<Integer> tempList, int [] nums, int start){
    list.add(new ArrayList<>(tempList));
    for(int i = start; i < nums.length; i++){
        tempList.add(nums[i]);
        backtrack(list, tempList, nums, i + 1);
        tempList.remove(tempList.size() - 1);
    }
}
```
    The output will be like [] [1] [1,2] [1,2,3] now after the calls to backtrack() completes, we start removing elements like 3 and then 2. and now we add 3 so the output well be [1,3] [2] [2,3] and finally [3]

    ## Output: [] [1] [1,2] [1,2,3] [1,3] [2] [2,3] [3]


## 90. Subsets II (contains duplicates) : 
https://leetcode.com/problems/subsets-ii/
### 1st Approach
    Here we have some duplicates, so we need to take care and not print duplicate permutations. For example 
    
    Input: nums = [1,2,2]
    Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]

    if we try to apply same logic as "Subsets" problem we will get output 

    [[],[1],[1,2],[1,2,2],[1,2],[2],[2,2],[2]]. here [1,2] and [2] are printed twice.

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(list, new ArrayList<>(), nums, 0);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, int start){
    list.add(new ArrayList<>(tempList));
    for(int i = start; i < nums.length; i++){
        if(i > start && nums[i] == nums[i-1]) continue; // skip duplicates
        tempList.add(nums[i]);
        backtrack(list, tempList, nums, i + 1);
        tempList.remove(tempList.size() - 1);
    }
} 
```
    Hence to deal with duplicates we have added  

    if(i > start && nums[i] == nums[i-1]) continue;

    we will add all the elements if they are in their 1st iteration no matter if they are duplicate, i=start, but when i!=start we will check in the input array nums if the current element is a duplicate. If yes, we will skip this element.
    [] [1] [1,2] 
    [1,2,2] we added last 2, because its in the 1st iteration of the loop.
    [1,2] since this 2 is in a result of i=1 (!=start) we checked for duplicate and reject this permutation.  

    NOTE: don't forget to sort the array, else this will produce duplicate subsets.

### 2nd Approach

    Here insted of rejecting the duplicates, we will generate all the subsets and put it inside hashset to eleminate duplicate sets. This approach is not as intutive as first.

```java
 public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        HashSet<List<Integer>> ans=new HashSet<>();
        backtrack(nums,ans,new ArrayList<>(),0);
        return new ArrayList<>(ans);
    }
    public void backtrack(int[] nums, HashSet<List<Integer>> ans, List<Integer> sub, int start) {
        ans.add(new ArrayList<>(sub));
        for(int i=start;i<nums.length;i++){
            // if(i>start && nums[i]==nums[i-1])continue;
            sub.add(nums[i]);
            backtrack(nums,ans,sub,i+1);
            sub.remove(sub.size()-1);
        }
    }
```

## 46. Permutations 
https://leetcode.com/problems/permutations/

    Here we are dealing with permutations rather than subsets as we saw in the previous problem. 

    Input: nums = [1,2,3]
    Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

```java
public List<List<Integer>> permute(int[] nums) {
   List<List<Integer>> list = new ArrayList<>();
   // Arrays.sort(nums); // not necessary
   backtrack(list, new ArrayList<>(), nums);
   return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums){
   if(tempList.size() == nums.length){
      list.add(new ArrayList<>(tempList));
   } else{
      for(int i = 0; i < nums.length; i++){ 
         if(tempList.contains(nums[i])) continue; // element already exists, skip
         tempList.add(nums[i]);
         backtrack(list, tempList, nums);
         tempList.remove(tempList.size() - 1);
      }
   }
} 
```

## 47.Permutations II (contains duplicates) : 
https://leetcode.com/problems/permutations-ii/

    This problem is exactly the same as previous one just with duplicates.

    Input: nums = [1,1,2]
    Output:[    [1,1,2],    [1,2,1],    [2,1,1]]

```java
public List<List<Integer>> permuteUnique(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(list, new ArrayList<>(), nums, new boolean[nums.length]);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, boolean [] used){
    if(tempList.size() == nums.length){
        list.add(new ArrayList<>(tempList));
    } else{
        for(int i = 0; i < nums.length; i++){
            if(used[i] || (i > 0 && nums[i] == nums[i-1] && !used[i - 1]) continue;
            used[i] = true; 
            tempList.add(nums[i]);
            backtrack(list, tempList, nums, used);
            used[i] = false; 
            tempList.remove(tempList.size() - 1);
        }
    }
}
```

    Here we are using the if statement to eliminate the duplicates. Lets do a quick walk through using [1,1,2] asan example.

    1.) call to backtrack(<>, <>, [1,1,2], [false,false,false]);
        1) (i=0)-> backtrack(<>, <1>, [1,1,2], [true,false,false]);
           1) (i=0)-> continue skip i=0 since used[i]=true;
           2) (i=1)-> backtrack(<>, <1,1>, [1,1,2], [true,true,false]);
              1) (i=0)-> continue skip i=0 since used[0]=true;
              2) (i=1)-> contunue skip i=1 since used[1]=true;
              3) (i=2)-> backtrack(<>, <1,1,2>, [1,1,2], [true,true,true]);
                 1) <1,1,2> added to ans
              4) (i=2) <1,1> <true,true,false>
           3) (i=1)-> <1> <true,false,false>
           4) (i=2)-> backtrack(<>, <1,2>, [1,1,2], [true,false,true]);
              1) (i=0)-> continue skip i=0 since used[0]=true;
              2) (i=1)-> backtrack(<>, <1,2,1>, [1,1,2], [true,true,true]);
                 1) <1,2,1> added to answer
              3) (i=1)-> <1,2> [true,false,true]
              4) (i=2)-> continue skip since used[2]=true;
           5) (i=2)-> <1> [true, false, false]
        2) (i=0)-> <> [false,false,false]
        3) (i=1)=> backtrack(<>, <1>, [1,1,2], [false,true,false]);
        4) -
        5) -

    if used[i] is false, means that that element is not present in current sub array.
    if used[i]=used[i-1] and i-1th element is not present in current sub array. Then we will also skip ith element. because taking ith or i-1th element will make a duplicate sub array. in other words, we dont incluede ith element because we must be having one set where i-1th element is being used and we dont want to make same sub array where ith element is there in place of i-1th element.

    In the above example focus on [1a,1b,2] we dont wanna create [1a,2] and [1b,2] as both are same. 

    Don't overthink that we might not generate both [1a,2] and [1b,2] if we use this algorithm. This will not happen because it only checkes for i-1 th pos for duplicate. Hence for 1st duplicate element, i-1th element is not equal and we will generate a permutation.


## 904. Fruit Into Baskets

    You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

    You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
    Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
    Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

```java
public int totalFruit(int[] fruits) {
        Map<Integer,Integer> hs=new HashMap<>();
        int left =0;
        int ans=0;
        for(int right=0;right<fruits.length;right++){
            hs.put(fruits[right],hs.getOrDefault(fruits[right],0)+1);
            while(hs.size()>2){
                hs.put(fruits[left],hs.get(fruits[left])-1);
                if(hs.get(fruits[left])==0){
                    hs.remove(fruits[left]);
                }
                left++;
            }
            ans=Math.max(ans,right-left+1);
        }
        return ans;
    }
```

    This solution is O(2n) time complexity because left and right pointer traverses the whole array once.


## 456. 132 Pattern

    This is not a straight forward but problem on "monotonously decreasing stack". In this approach, we are keeping track of a maximum element on the top of the stack meanwhile we will also keeping track of a minimum element before that.

    PROBLEM
    Given an array of n integers nums, a 132 pattern is a subsequence of three integers nums[i], nums[j] and nums[k] such that i < j < k and nums[i] < nums[k] < nums[j].

    Return true if there is a 132 pattern in nums, otherwise, return false.

    Input: nums = [1,2,3,4]
    Output: false
    Explanation: There is no 132 pattern in the sequence.

```java
    int min=nums[0];
        Stack<int[]> stack=new Stack<>();
        for(int element:nums){
            while(!stack.empty() && element>=stack.peek()[0]){
                stack.pop();
            }
            if(!stack.empty() && element>stack.peek()[1]){
                return true;
            }
            min=Math.min(min,element);
            stack.push(new int []{element,min});
        }
    return false;
```

    time complexity is )(2n) because we are pushing and popping all the elements n times.


## 150. Evaluate Reverse Polish Notation 
https://leetcode.com/problems/evaluate-reverse-polish-notation/description/

    Input: tokens = ["2","1","+","3","*"]
    Output: 9
    Explanation: ((2 + 1) * 3) = 9

    very simple approach with single stack. we keep on checking if we have any operator, we will pop from stack do the operation and again push onstack. else we push the digit on the stack;

```java
    class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack=new Stack<>();
        for(String s:tokens){
            if(s.equals("+")){
                stack.push(stack.pop()+stack.pop());
            }else if(s.equals("-")){
                int a=stack.pop();
                int b=stack.pop();
                stack.push(b-a);
            }else if(s.equals("*")){
                stack.push(stack.pop()*stack.pop());
            }else if(s.equals("/")){
                int a=stack.pop();
                int b=stack.pop();
                stack.push(b/a);
            }else{
                stack.push(Integer.parseInt(s));
            }
        }return stack.pop();
    }
}
```

    Time complexity is O(n)

## 2390. Removing Stars From a String 
https://leetcode.com/problems/removing-stars-from-a-string/description/

```java
public String removeStars(String s) {
        Stack<Character> stack=new Stack<>();
        StringBuilder sb=new StringBuilder();
        for(Character c:s.toCharArray()){
            if(c=='*'){
                stack.pop();
            }else{
                stack.push(c);
            }
        }
        while(!stack.empty()){
                sb.append(stack.pop());
        }
        return sb.reverse().toString();
    }
```

    Time complexity: O(n)

## 704. Binary Search 
https://leetcode.com/problems/binary-search/description/

```java
public int search(int[] nums, int target) {
        int left=0;
        int right=nums.length-1;
        int mid=(left+right)/2;
        while(left<=right){
            mid=(left+right)/2;
            if(nums[mid]==target){
                return mid;
            }else if(nums[mid]>target){
                right=mid-1;
            }else left=mid+1;
        }
        return -1;
    }
```

## 540. Single Element in a Sorted Array 
https://leetcode.com/problems/single-element-in-a-sorted-array/description/

    Input: nums = [1,1,2,3,3,4,4,8,8]
    Output: 2

```java
public int singleNonDuplicate(int[] nums) {
        if(nums.length == 1)return nums[0];
        if(nums[0]!=nums[1])return nums[0];
        if(nums[nums.length-1]!= nums[nums.length-2])return nums[nums.length-1];
        int left =1;
        int right=nums.length-2;
        int mid=0;
        while(left<=right){
            mid=(left+right)/2;
            if(nums[mid] != nums[mid-1] && nums[mid] != nums[mid+1]){
                return nums[mid];
            }
            if((mid%2==0 && nums[mid-1]==nums[mid]) || (mid%2==1 && nums[mid+1]==nums[mid])){
                    right=mid-1;
                }else{
                    left=mid+1;
                }  
        }
        return -1;
    }
```
    Time complexity is O(log n)


## 1011. Capacity To Ship Packages Within D Days 
https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/description/

    Input: weights = [1,2,3,4,5,6,7,8,9,10], days = 5
    Output: 15
    Explanation: A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
    1st day: 1, 2, 3, 4, 5
    2nd day: 6, 7
    3rd day: 8
    4th day: 9
    5th day: 10

    Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.


```java
public boolean isShip(int[] weights,int capicity, int days){
    int ships=1;
    int currCap=capicity;
    for(int w:weights){
            if(currCap-w<0){
                ships++;
                currCap=capicity;
            }
            currCap-=w;
    }
    return ships<=days;
}
public int shipWithinDays(int[] weights, int days) {
    int lower=0;
    int total=0;
    int ans_sofar=Integer.MAX_VALUE;
    int mid=-1;
    for(int w:weights){
            lower=Math.max(lower,w);
            total+=w;
        }
    while(lower<=total){
        mid=(lower+total)/2;
        if(isShip(weights,mid,days)){
            total=mid-1;
            ans_sofar=Math.min(ans_sofar,mid);
        }else{
            lower=mid+1;
        }
    }
    return ans_sofar;
}
```

## 2300. Successful Pairs of Spells and Potions 
https://leetcode.com/problems/successful-pairs-of-spells-and-potions/description/

    Input: spells = [5,1,3], potions = [1,2,3,4,5], success = 7
    Output: [4,0,3]
    Explanation:
    - 0th spell: 5 * [1,2,3,4,5] = [5,10,15,20,25]. 4 pairs are successful.
    - 1st spell: 1 * [1,2,3,4,5] = [1,2,3,4,5]. 0 pairs are successful.
    - 2nd spell: 3 * [1,2,3,4,5] = [3,6,9,12,15]. 3 pairs are successful.
    Thus, [4,0,3] is returned.

```java 
public int[] successfulPairs(int[] spells, int[] potions, long success) {
        int[] ans = new int[spells.length];
        Arrays.sort(potions);
        for(int i=0;i<spells.length;i++){
            int left=0;
            int right=potions.length-1;
            int mid=-1;
            int min=potions.length;
            while(left<=right){
                mid =left + (right - left) / 2;
                long prod=(long)spells[i]*potions[mid];
                if(prod>=success){
                    right=mid-1;min=mid;
                }else{
                    left=mid+1;
                }
            } 
            ans[i]=potions.length-min;     
        }
        return ans;
    }
```

    for the bruitforce solution with 2 nested arrays, time comp will be O(m*n), but now we applied a binary search algorith so it will be now O(m*log n)

## 200. Number of Islands 
https://leetcode.com/problems/number-of-islands/

```java
public void bfs(char[][] grid,int row, int col){
        if(row<0||col<0||row>grid.length-1||col>grid[0].length-1||grid[row][col]=='0')return;
        grid[row][col]='0';
        bfs(grid,row-1,col);//top
        bfs(grid,row+1,col);//bottom
        bfs(grid,row,col-1);//left
        bfs(grid,row,col+1);//right
    }

    public int numIslands(char[][] grid) {
        int cnt=0;
        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){
                if(grid[i][j]=='1'){
                    cnt++;
                    bfs(grid,i,j);
                }
            }
        }return cnt;
    }
```

    Time complexity willbe O(m * n), since all the blocks have been traversed once.


## 153. Find Minimum in Rotated Sorted Array https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/ (classic)

```java
public int findMin(int[] nums) {
        int left=0;
        int right=nums.length-1;

        while(left<right){
            int mid=left+(right-left)/2;
            if(nums[mid]>nums[right]){
                left=mid+1;
            }else{
                right=mid;
            }
        }
        return nums[left];
}
```
    Time complexity: O(log n)



## 33. Search in Rotated Sorted Array 
https://leetcode.com/problems/search-in-rotated-sorted-array/description/


```java
public int search(int[] nums, int target) {
        if(nums==null||nums.length==0)return -1;
        //Lets first finfd the starting point of rotation, which wil be dipected by left after the 1st binary search.
        int left=0;
        int right=nums.length-1;
        int mid=-1;
        while(left<right){
            mid=left+(right-left)/2;
            if(nums[mid]>nums[right]){
                left=mid+1;
            }else{
                right=mid;
            }
        }
        System.out.println(left);
        int start=left;
        left=0;
        right=nums.length-1;
        // now we have 2 sorted arrays and we wnat to select the one in which the target lies
        if(target>=nums[start] && target<=nums[right]){
            left=start;
        }else{
            right=start;
        }
        // we have select the array, now simply apply the binary search algorithm
        while(left<=right){
            mid=left+(right-left)/2;
            if(target==nums[mid])return mid;
            if(nums[mid]>target){
                right=mid-1;
            }else{
                left=mid+1;
            }
        }
        return -1;
    }
```


## 34. Find First and Last Position of Element in Sorted Array
https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/

```java 
public int[] searchRange(int[] nums, int target) {
        int ans[]={-1,-1};
        ans[0]=search(nums,target,true);
        if(ans[0]!=-1)ans[1]=search(nums,target,false);
        return ans;
    }
    int search(int[] nums, int target, boolean firstIndex) {
        int ans=-1;
        int left=0;
        int right=nums.length-1;
        int mid=0;
        while(left<=right){
            mid=left+(right-left)/2;
            if(nums[mid]<target){
                left=mid+1;
            }else if(nums[mid]>target){
                right=mid-1;
            }else {
                ans=mid;
                if(firstIndex){
                    right=mid-1;
                }else{
                    left=mid+1;
                }
            }
        }return ans;
    }
```

    Time complexity: O(log n)

## 206. Reverse Linked List (Classic)
https://leetcode.com/problems/reverse-linked-list/

### Iterative Solution

```java
public ListNode reverseList(ListNode head) {
        ListNode current=head;
        ListNode nextNode=null;
        ListNode previous=null;

        while(current!=null){
            nextNode=current.next;
            current.next=previous;
            previous=current;
            current=nextNode;
        }
        return previous;
    }
```

    Time complexity: O(n)

### Recursive Solution

```java
    public ListNode reverseList(ListNode head) {
        return rev(head, null);
    }

    public ListNode rev(ListNode node, ListNode pre) {
        if (node == null) return pre;
        ListNode temp = node.next;
        node.next = pre;
        return rev(temp, node);
    }
```
## 21. Merge Two Sorted Lists (Classic)
https://leetcode.com/problems/merge-two-sorted-lists/description/


    Take a pointer, point it to the least valued node, increment both pointer. In the end just point the resulting list to a non empty list if there any.
```java 
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode head=new ListNode();
        ListNode pointer=head;
        while(list1!=null && list2!=null){
            if(list1.val<list2.val){
                pointer.next=list1;
                list1=list1.next;
            }else{
                pointer.next=list2;
                list2=list2.next;
            }
            pointer=pointer.next;
        }
        pointer.next=list1!=null?list1:list2;
        return head.next;
    }
```

## 234. Palindrome Linked List
https://leetcode.com/problems/palindrome-linked-list/

```java
    public boolean isPalindrome(ListNode head) {
        StringBuilder str=new StringBuilder("");
        while(head!=null){
            str.append(head.val);
            head=head.next;
        }
        StringBuilder strrev=new StringBuilder(str).reverse();
        boolean ans=true;
        for(int i=0;i<str.length();i++){
            if(str.charAt(i)!=strrev.charAt(i)){
                ans= false;
            }
        }
        return ans;
    }
```


## 234. Palindrome Linked List
https://leetcode.com/problems/palindrome-linked-list/

    Reverse the later half after reversing just start comparing if at any time the value doesn't match it's not a palindrome, i.e. return false, else it's a palindrome and return true.
    It'll automatically take care of the edge cases of odd and even

```java
class Solution {

    public boolean isPalindrome(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode temp = reverse(slow);
        while (temp != null && head != null) {
            if (temp.val != head.val) return false;
            temp = temp.next;
            head = head.next;
        }
        return true;
    }

    public ListNode reverse(ListNode head) {
        ListNode p = null;
        ListNode q = null;
        ListNode r = head;
        while (r != null) {
            p = q;
            q = r;
            r = r.next;
            q.next = p;
        }
        return q;
    }
}

```