## Next greater element

given array of number find next greater element [33,44,3,1,99,3,2,4,500,99,87,32,65]


```java
// Online Java Compiler
// Use this editor to write, compile and run your Java code online
import java.util.*;

class Main {
    public static void main(String[] args) {
        int[] nums=new int[]{33,44,3,1,99,3,2,4};
        int len =nums.length;
    Stack<Integer> mono=new Stack<>();
    int[] ans=new int[len];
    
    for(int i=len-1;i>=0;i--){
        while(!mono.isEmpty() && nums[i]>= mono.peek()){
            mono.pop();
        }
        ans[i]=!mono.isEmpty()?mono.peek():-1;
        mono.push(nums[i]);
    
    }
    for(int i:ans)System.out.print(i+", ") ;
    }
}

```

given array of number find next smaller element [33,44,3,1,99,3,2,4,500,99,87,32,65]


```java
// Online Java Compiler
// Use this editor to write, compile and run your Java code online
import java.util.*;

class Main {
    public static void main(String[] args) {
        int[] nums=new int[]{33,44,3,1,99,3,2,4};
        int len =nums.length;
    Stack<Integer> mono=new Stack<>();
    int[] ans=new int[len];
    
    for(int i=len-1;i>=0;i--){
        while(!mono.isEmpty() && nums[i]<>= mono.peek()){
            mono.pop();
        }
        ans[i]=!mono.isEmpty()?mono.peek():-1;
        mono.push(nums[i]);
    
    }
    for(int i:ans)System.out.print(i+", ") ;
    }
}

```





<details id="1944. Number of Visible People in a Queue">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1944. Number of Visible People in a Queue 
</span>
</summary>

https://leetcode.com/problems/number-of-visible-people-in-a-queue/description/

There are n people standing in a queue, and they numbered from 0 to n - 1 in left to right order. You are given an array heights of distinct integers where heights[i] represents the height of the ith person.

A person can see another person to their right in the queue if everybody in between is shorter than both of them. More formally, the ith person can see the jth person if i < j and min(heights[i], heights[j]) > max(heights[i+1], heights[i+2], ..., heights[j-1]).

Return an array answer of length n where answer[i] is the number of people the ith person can see to their right in the queue.


```java
class Solution {
    public int[] canSeePersonsCount(int[] heights) {
        int len = heights.length;
        int[] discoverable = new int[len];

        Stack<Integer> incHeight = new Stack<>();

        for (int i = len - 1; i >= 0; i--) {
            int num = 0;
            while (!incHeight.isEmpty() && incHeight.peek() <= heights[i]) {
                num++;
                incHeight.pop();
            }
            if (!incHeight.isEmpty()) {
                num++;
            }
            discoverable[i] = num;
            incHeight.push(heights[i]);
        }
        return discoverable;
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

