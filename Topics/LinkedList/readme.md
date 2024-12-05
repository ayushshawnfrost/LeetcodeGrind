## Linked List

## Basic
<details id="2. Add Two Numbers">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">2. Add Two Numbers 
</span>
</summary>


https://leetcode.com/problems/add-two-numbers/description/

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

 

Example 1:


Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
Example 2:

Input: l1 = [0], l2 = [0]
Output: [0]
Example 3:

Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
 

Constraints:

The number of nodes in each linked list is in the range [1, 100].
0 <= Node.val <= 9
It is guaranteed that the list represents a number that does not have leading zeros.


```java

class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(-1);
        ListNode pointer = head;
        int carry = 0;
        while (l1 != null && l2 != null) {
            int d1 = l1.val;
            int d2 = l2.val;

            int sum = d1 + d2 + carry;
            int digit = sum;
            carry=0;
            if (sum > 9) {
                carry = sum / 10;
                digit = sum % 10;
            }
            ListNode temp = new ListNode(digit);
            pointer.next = temp;
            pointer = temp;
            l1 = l1.next;
            l2 = l2.next;
        }
        if (l1 != null) {
            while (l1 != null) {
                int sum = l1.val + carry;
                int digit = sum;
                 carry=0;
                if (sum > 9) {
                    carry = sum / 10;
                    digit = sum % 10;
                }
                ListNode temp = new ListNode(digit);
                pointer.next = temp;
                pointer = temp;
                l1 = l1.next;
            }
        }
        if (l2 != null) {
            while (l2 != null) {
                int sum = l2.val + carry;
                 carry=0;
                int digit = sum;
                if (sum > 9) {
                    carry = sum / 10;
                    digit = sum % 10;
                }
                ListNode temp = new ListNode(digit);
                pointer.next = temp;
                pointer = temp;
                l2 = l2.next;
            }
        }
        if (carry!=0){
            ListNode temp = new ListNode(carry);
            pointer.next = temp;
        }
            return head.next;
    }
}
```
</details>

<details id="206. Reverse Linked List">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">206. Reverse Linked List 
</span>
</summary>

https://leetcode.com/problems/reverse-linked-list/description/

Given the head of a singly linked list, reverse the list, and return the reversed list.

 

Example 1:


Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
Example 2:


Input: head = [1,2]
Output: [2,1]
Example 3:

Input: head = []
Output: []
 

Constraints:

The number of nodes in the list is the range [0, 5000].
-5000 <= Node.val <= 5000
 

Follow up: A linked list can be reversed either iteratively or recursively. Could you implement both?



```java

// Iterative
class Solution {

    // 1 -> 2 -> 3 ->
    // 3 -> 2 -> 1 ->

    // Step 1: Save refrence to the next node (2)
    // Step 2: Make link with the prev node (1 -> NULL)
    // Step 3: Update previous node (1)
    // Step 4: update current node (2)
    // Step 5: return the head of the reversed List (3)
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
}

// Recursive
class Solution {
    public ListNode reverseList(ListNode head) {
        return recur(head,null);
    }
    public ListNode recur(ListNode node, ListNode previous){
        if(node==null)return previous;
        ListNode nextNode=node.next;
        node.next=previous;
        previous=node;
        return recur(nextNode,previous);
    }
}

```
</details>







<details id="21. Merge Two Sorted Lists">
<summary> 
<span style="color:red;font-size:16px;font-weight:bold">21. Merge Two Sorted Lists 
</span>
</summary>

https://leetcode.com/problems/merge-two-sorted-lists/description/

You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

 

Example 1:


Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
Example 2:

Input: list1 = [], list2 = []
Output: []
Example 3:

Input: list1 = [], list2 = [0]
Output: [0]
 

Constraints:

The number of nodes in both lists is in the range [0, 50].
-100 <= Node.val <= 100
Both list1 and list2 are sorted in non-decreasing order.


```java

class Solution {
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
}
```
</details>



<details id="25. Reverse Nodes in k-Group">
<summary> 
<span style="color:red;font-size:16px;font-weight:bold">25. Reverse Nodes in k-Group 
</span>
</summary>

https://leetcode.com/problems/reverse-nodes-in-k-group/description/

Given the head of a linked list, reverse the nodes of the list k at a time, and return the modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
Example 2:


Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]

```java
class Solution {

    /**
     * Reverse a link list between begin and end exclusively
     * an example:
     * a linked list:
     * 0->1->2->3->4->5->6
     * | |
     * begin end
     * after call begin = reverse(begin, end)
     * 
     * 0->3->2->1->4->5->6
     * | |
     * begin end
     * 
     * @return the reversed list's 'begin' node, which is the precedence of node end
     */

    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummyHead = new ListNode(-1);
        ListNode begin = dummyHead;
        dummyHead.next = head;
        int i = 0;
        while (head != null) {
            i++;
            if (i % k == 0) {
                begin = reverseKNodes(begin, head.next); // begin.next will always have the start node of the list to be reversed
                head = begin.next;
            } else {
                head = head.next;
            }
        }
        return dummyHead.next;
    }

    private ListNode reverseKNodes(ListNode dummyHead, ListNode dummyTail) {
        ListNode previous = dummyHead;
        ListNode current = dummyHead.next;
        ListNode first = dummyHead.next; // Get refrence of the first node of original list
        ListNode nextNode = null;

        while (current != dummyTail) {
            nextNode = current.next;
            current.next = previous;
            previous = current;
            current = nextNode;
        }
        dummyHead.next = previous; // previous denotes the head of reversed list
        first.next = current;
        return first; // return the first node, which is now the tail of reversed list
    }
}

visualize:
https://cscircles.cemc.uwaterloo.ca/java_visualize/#code=++class+ListNode+%7B%0A++int+val%3B%0A++ListNode+next%3B%0A++ListNode()+%7B%7D%0AListNode(int+val)+%7B+this.val+%3D+val%3B+%7D%0A++ListNode(int+val,+ListNode+next)+%7B+this.val+%3D+val%3B+this.next+%3D+next%3B+%7D%0A++%7D%0A%0Apublic+class+Solution+%7B%0A%0A++++/**%0A+++++*+Reverse+a+link+list+between+begin+and+end+exclusively%0A+++++*+an+example%3A%0A+++++*+a+linked+list%3A%0A+++++*+0-%3E1-%3E2-%3E3-%3E4-%3E5-%3E6%0A+++++*+%7C+%7C%0A+++++*+begin+end%0A+++++*+after+call+begin+%3D+reverse(begin,+end)%0A+++++*+%0A+++++*+0-%3E3-%3E2-%3E1-%3E4-%3E5-%3E6%0A+++++*+%7C+%7C%0A+++++*+begin+end%0A+++++*+%0A+++++*+%40return+the+reversed+list's+'begin'+node,+which+is+the+precedence+of+node+end%0A+++++*/%0A%0A++++public+static+ListNode+reverseKGroup(ListNode+head,+int+k)+%7B%0A++++++++ListNode+dummyHead+%3D+new+ListNode(-1)%3B%0A++++++++ListNode+begin+%3D+dummyHead%3B%0A++++++++dummyHead.next+%3D+head%3B%0A++++++++int+i+%3D+0%3B%0A++++++++while+(head+!%3D+null)+%7B%0A++++++++++++i%2B%2B%3B%0A++++++++++++if+(i+%25+k+%3D%3D+0)+%7B%0A++++++++++++++++begin+%3D+reverseKNodes(begin,+head.next)%3B%0A++++++++++++++++head+%3D+begin.next%3B%0A++++++++++++%7D+else+%7B%0A++++++++++++++++head+%3D+head.next%3B%0A++++++++++++%7D%0A++++++++%7D%0A++++++++return+dummyHead.next%3B%0A++++%7D%0A%0A++++private+static+ListNode+reverseKNodes(ListNode+dummyHead,+ListNode+dummyTail)+%7B%0A++++++++ListNode+previous+%3D+dummyHead%3B%0A++++++++ListNode+current+%3D+dummyHead.next%3B%0A++++++++ListNode+first+%3D+dummyHead.next%3B+//+Get+refrence+of+the+first+node+of+original+list+(1)%0A++++++++ListNode+nextNode+%3D+null%3B%0A%0A++++++++while+(current+!%3D+dummyTail)+%7B%0A++++++++++++nextNode+%3D+current.next%3B%0A++++++++++++current.next+%3D+previous%3B%0A++++++++++++previous+%3D+current%3B%0A++++++++++++current+%3D+nextNode%3B%0A++++++++%7D%0A++++++++dummyHead.next+%3D+previous%3B+//+previous+denotes+the+head+of+reversed+list(dummyHead--%3E+2)%0A++++++++first.next+%3D+current%3B%0A++++++++return+first%3B+//+return+the+first+node,+which+is+now+the+tail+of+reversed+list%0A++++%7D%0A++++%0A++++public+static+void+main(String%5B%5D+args)+%7B%0A++++++ListNode+head%3D+new+ListNode(1)%3B%0A++++++head.next%3Dnew+ListNode(2)%3B%0A+++++++head.next.next%3Dnew+ListNode(3)+%3B%0A+++++++head.next.next.next%3Dnew+ListNode(4)+%3B%0A+++++++head.next.next.next.next%3Dnew+ListNode(5)%3B%0A+++++++%0A+++++++head%3DreverseKGroup(head,+2)%3B%0A+++++++%0A++%7D%0A%7D&mode=display&curInstr=102

```

</details>

























<details id="138. Copy List with Random Pointer">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">138. Copy List with Random Pointer 
</span>
</summary>

https://leetcode.com/problems/copy-list-with-random-pointer/description/

A linked list of length n is given such that each node contains an additional random pointer, which could point to any node in the list, or null.

Construct a deep copy of the list. The deep copy should consist of exactly n brand new nodes, where each new node has its value set to the value of its corresponding original node. Both the next and random pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. None of the pointers in the new list should point to nodes in the original list.

For example, if there are two nodes X and Y in the original list, where X.random --> Y, then for the corresponding two nodes x and y in the copied list, x.random --> y.

Return the head of the copied linked list.

The linked list is represented in the input/output as a list of n nodes. Each node is represented as a pair of [val, random_index] where:

val: an integer representing Node.val
random_index: the index of the node (range from 0 to n-1) that the random pointer points to, or null if it does not point to any node.
Your code will only be given the head of the original linked list.

 

Example 1:


Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
Example 2:


Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
Example 3:



Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]
 

Constraints:

0 <= n <= 1000
-104 <= Node.val <= 104
Node.random is null or is pointing to some node in the linked list.

```java
class Solution {

    public Node copyRandomList(Node head) {

        // Map <OriginalNode --> CloneNode>
        Node cur = head;
        HashMap<Node, Node> map = new HashMap<>();
        while (cur != null) {
            map.put(cur, new Node(cur.val));
            cur = cur.next;
        }
        cur = head;
        while (cur != null) {
            map.get(cur).next = map.get(cur.next);
            map.get(cur).random = map.get(cur.random);
            cur = cur.next;
        }
        return map.get(head);
    }
}

```
</details>



<details id="23. Merge k Sorted Lists">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">23. Merge k Sorted Lists 
</span>
</summary>



https://leetcode.com/problems/merge-k-sorted-lists/




Let me break down the time complexity analysis step by step:

1) First, let's define our variables:
- k = number of linked lists (length of lists array)
- n = average length of each linked list

2) The algorithm uses a divide and conquer approach:
- It repeatedly divides the k lists into two halves until we have pairs of lists to merge
- This creates a recursion tree of depth log k

3) At each level of the recursion tree:
- We're merging lists of approximately equal size
- Each element from all lists needs to be compared and merged
- Total elements at each level = O(n*k) where n is average list length

4) For merging two sorted lists:
- Time complexity is O(n1 + n2) where n1, n2 are lengths of the lists being merged
- In this case, at each level we're processing all n*k elements

5) Putting it all together:
- We have log k levels (from divide and conquer)
- At each level, we process n*k elements
- Therefore, total time complexity = O(k*n * log k)

The space complexity is O(log k) due to the recursion stack depth.

To give an example:
If you have 8 lists (k=8) of length 5 each (n=5):
- Level 1: Merge 8 into 4 pairs = 40 operations
- Level 2: Merge 4 into 2 pairs = 40 operations
- Level 3: Merge 2 into 1 = 40 operations
Total = O(40 * log 8) = O(40 * 3) = O(120) operations

Therefore, the final time complexity is O(k*n * log k) where:
- k is the number of lists
- n is the average length of each list
- log k is the number of levels in the merge process

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        int n = lists.length - 1;
        return partitionAndMerge(lists, 0, n);
    }

    private ListNode partitionAndMerge(ListNode[] lists, int l, int r) { // logk n
        if (l > r)
            return null;
        if (l == r)
            return lists[l];

        int mid = l + (r - l) / 2;
        ListNode L1 = partitionAndMerge(lists, l, mid);
        ListNode L2 = partitionAndMerge(lists, mid + 1, r);
        return mergeTwoLinkedList(L1, L2);
    }

    private ListNode mergeTwoLinkedList(ListNode L1, ListNode L2) {
        if (L1 == null)
            return L2;
        if (L2 == null)
            return L1;

        if (L1.val <= L2.val) {
            L1.next = mergeTwoLinkedList(L1.next, L2);
            return L1;
        } else {
            L2.next = mergeTwoLinkedList(L1, L2.next);
            return L2;
        }
    }
}

```
</details>

## Design Pattern



<details id="146. LRU Cache">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">146. LRU Cache 
</span>
</summary>

https://leetcode.com/problems/lru-cache/


Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the LRUCache class:

LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
int get(int key) Return the value of the key if the key exists, otherwise return -1.
void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
The functions get and put must each run in O(1) average time complexity.

 

Example 1:

Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
 

Constraints:

1 <= capacity <= 3000
0 <= key <= 104
0 <= value <= 105
At most 2 * 105 calls will be made to get and put.



```java
// Bruteforce TLE

class LRUCache {
    List<int[]> cache;
    int capacity;

    public LRUCache(int capacity) {
        this.cache = new ArrayList<>();
        this.capacity = capacity;
    }

    public int get(int key) {
        for (int i = 0; i < cache.size(); i++) {
            if (cache.get(i)[0] == key) {
                int[] entry = cache.get(i);
                cache.remove(i);
                cache.add(entry);
                return entry[1];
            }
        }
        return -1;
    }

    public void put(int key, int value) {
        for (int i = 0; i < cache.size(); i++) {
            if (cache.get(i)[0] == key) {
                cache.remove(i);
                cache.add(new int[] { key, value });
                return;
            }
        }
        if (cache.size() == capacity) {
            cache.remove(0);
        }
        cache.add(new int[] { key, value });
    }
}

```

```java
// LRU Cache implementation using a doubly linked list and hashmap
class LRUCache {
    class Node {
        int key;
        int val;
        Node prev;    
        Node next;    

        Node(int key, int val) {
            this.key = key;
            this.val = val;
        }
    }

    // Dummy head and tail nodes to avoid edge cases
    Node head = new Node(-1, -1);
    Node tail = new Node(-1, -1);
    int cap;  
    HashMap<Integer, Node> cache = new HashMap<>();

    public LRUCache(int capacity) {
        cap = capacity;
        head.next = tail;
        tail.prev = head;
    }


    private void addNode(Node newnode) {
        Node tempNext = head.next;

        // Update the connections to insert newnode
        newnode.next = tempNext;
        newnode.prev = head;

        head.next = newnode;
        tempNext.prev = newnode;
    }

    private void deleteNode(Node delnode) {
        Node prevNode = delnode.prev;
        Node nextNode = delnode.next;

        // Update connections to remove delnode
        prevNode.next = nextNode;
        nextNode.prev = prevNode;
    }

    public int get(int key) {
        if (cache.containsKey(key)) {
            Node resNode = cache.get(key);
            int ans = resNode.val;

            // Remove node from current position
            cache.remove(key);
            deleteNode(resNode);
            
            // Add node to front (most recently used position)
            addNode(resNode);

            // Update hashmap with new node position
            cache.put(key, head.next);
            return ans;
        }
        return -1;    
    }

    public void put(int key, int value) {
        // If key exists, remove it first
        if (cache.containsKey(key)) {
            Node curr = cache.get(key);
            cache.remove(key);
            deleteNode(curr);
        }

        // If cache is full, remove least recently used item (tail.prev)
        if (cache.size() == cap) {
            cache.remove(tail.prev.key);
            deleteNode(tail.prev);
        }

        // Add new node at front (most recently used position)
        addNode(new Node(key, value));
        // Update hashmap with new node
        cache.put(key, head.next);
    }
}
```
</details>




<details id="460. LFU Cache">
<summary> 
<span style="color:red;font-size:16px;font-weight:bold">460. LFU Cache 
</span>
</summary>

https://leetcode.com/problems/lfu-cache/

460. LFU Cache
Solved
Hard
Topics
Companies
Design and implement a data structure for a Least Frequently Used (LFU) cache.

Implement the LFUCache class:

LFUCache(int capacity) Initializes the object with the capacity of the data structure.
int get(int key) Gets the value of the key if the key exists in the cache. Otherwise, returns -1.
void put(int key, int value) Update the value of the key if present, or inserts the key if not already present. When the cache reaches its capacity, it should invalidate and remove the least frequently used key before inserting a new item. For this problem, when there is a tie (i.e., two or more keys with the same frequency), the least recently used key would be invalidated.
To determine the least frequently used key, a use counter is maintained for each key in the cache. The key with the smallest use counter is the least frequently used key.

When a key is first inserted into the cache, its use counter is set to 1 (due to the put operation). The use counter for a key in the cache is incremented either a get or put operation is called on it.

The functions get and put must each run in O(1) average time complexity.

 

Example 1:

Input
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

Explanation
// cnt(x) = the use counter for key x
// cache=[] will show the last used order for tiebreakers (leftmost element is  most recent)
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // return 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 2 is the LFU key because cnt(2)=1 is the smallest, invalidate 2.
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1.
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // return 4
                 // cache=[4,3], cnt(4)=2, cnt(3)=3
 

Constraints:

1 <= capacity <= 104
0 <= key <= 105
0 <= value <= 109
At most 2 * 105 calls will be made to get and put.

```java
class LFUCache {
    // Class to represent each node in the doubly linked list
    class Node {
        int key;
        int value;
        int frequency;
        Node prev;
        Node next;
        
        Node(int key, int value) {
            this.key = key;
            this.value = value;
            this.frequency = 1;
        }
    }
    
    // Class to manage doubly linked list operations
    class DLList {
        Node head;
        Node tail;
        int size;
        
        DLList() {
            head = new Node(-1, -1);
            tail = new Node(-1, -1);
            head.next = tail;
            tail.prev = head;
            size = 0;
        }
        
        // Add node right after head
        void addNode(Node node) {
            Node temp = head.next;
            node.next = temp;
            node.prev = head;
            head.next = node;
            temp.prev = node;
            size++;
        }
        
        // Remove a node from the list
        void removeNode(Node node) {
            node.prev.next = node.next;
            node.next.prev = node.prev;
            size--;
        }
        
        // Remove and return the least recently used node
        Node removeLRUNode() {
            if (size > 0) {
                Node node = tail.prev;
                removeNode(node);
                return node;
            }
            return null;
        }
    }
    
    int capacity;
    int minFrequency;
    int curSize;
    // Map to store key to Node mapping
    HashMap<Integer, Node> cache;
    // Map to store frequency to DLList mapping
    HashMap<Integer, DLList> frequencyMap;
    
    public LFUCache(int capacity) {
        this.capacity = capacity;
        this.minFrequency = 0;
        this.curSize = 0;
        this.cache = new HashMap<>();
        this.frequencyMap = new HashMap<>();
    }
    
    private void updateNode(Node node) {
        // Get the current frequency list
        DLList oldList = frequencyMap.get(node.frequency);
        oldList.removeNode(node);
        
        // If this list becomes empty and was the min frequency list, increment min frequency
        if (node.frequency == minFrequency && oldList.size == 0) {
            minFrequency++;
        }
        
        // Move node to the next frequency list
        node.frequency++;
        DLList newList = frequencyMap.computeIfAbsent(node.frequency, k -> new DLList());
        newList.addNode(node);
    }
    
    public int get(int key) {
        Node node = cache.get(key);
        if (node == null) {
            return -1;
        }
        
        updateNode(node);
        return node.value;
    }
    
    public void put(int key, int value) {
        if (capacity == 0) {
            return;
        }
        
        Node node = cache.get(key);
        
        if (node != null) {
            // Update existing node
            node.value = value;
            updateNode(node);
        } else {
            // Create new node
            node = new Node(key, value);
            
            // If cache is full, remove LFU (and LRU in case of tie)
            if (curSize == capacity) {
                DLList minFreqList = frequencyMap.get(minFrequency);
                Node lruNode = minFreqList.removeLRUNode();
                cache.remove(lruNode.key);
                curSize--;
            }
            
            // Add new node
            curSize++;
            minFrequency = 1;
            DLList list = frequencyMap.computeIfAbsent(1, k -> new DLList());
            list.addNode(node);
            cache.put(key, node);
        }
    }
}
```

</details>




<details id="895. Maximum Frequency Stack">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">895. Maximum Frequency Stack 
</span>
</summary>

https://leetcode.com/problems/maximum-frequency-stack/description/

Design a stack-like data structure to push elements to the stack and pop the most frequent element from the stack.

Implement the FreqStack class:

FreqStack() constructs an empty frequency stack.
void push(int val) pushes an integer val onto the top of the stack.
int pop() removes and returns the most frequent element in the stack.
If there is a tie for the most frequent element, the element closest to the stack's top is removed and returned.
 

Example 1:

Input
["FreqStack", "push", "push", "push", "push", "push", "push", "pop", "pop", "pop", "pop"]
[[], [5], [7], [5], [7], [4], [5], [], [], [], []]
Output
[null, null, null, null, null, null, null, 5, 7, 5, 4]

Explanation
FreqStack freqStack = new FreqStack();
freqStack.push(5); // The stack is [5]
freqStack.push(7); // The stack is [5,7]
freqStack.push(5); // The stack is [5,7,5]
freqStack.push(7); // The stack is [5,7,5,7]
freqStack.push(4); // The stack is [5,7,5,7,4]
freqStack.push(5); // The stack is [5,7,5,7,4,5]
freqStack.pop();   // return 5, as 5 is the most frequent. The stack becomes [5,7,5,7,4].
freqStack.pop();   // return 7, as 5 and 7 is the most frequent, but 7 is closest to the top. The stack becomes [5,7,5,4].
freqStack.pop();   // return 5, as 5 is the most frequent. The stack becomes [5,7,4].
freqStack.pop();   // return 4, as 4, 5 and 7 is the most frequent, but 4 is closest to the top. The stack becomes [5,7].
 

Constraints:

0 <= val <= 109
At most 2 * 104 calls will be made to push and pop.
It is guaranteed that there will be at least one element in the stack before calling pop.


```java
//Using Stack
class FreqStack {
    Map<Integer, Integer> freqMap;
    Map<Integer, Stack<Integer>> freqStack;
    int maxFreq;
    public FreqStack() {
        freqMap = new HashMap<>();
        freqStack = new HashMap<>();
        maxFreq = 0;
    }
    //Increment value in freqMap, 
    //updating the maxFreq
    //Adding value in dfreqStack
    public void push(int x) {
        int freq = freqMap.getOrDefault(x,0)+1;
        freqMap.put(x, freq);
        if(freq > maxFreq) maxFreq = freq;
        freqStack.computeIfAbsent(freq, f->new Stack()).push(x);
    }
    
    //Return and remove the top of maxFreq
    //Update maxFreq (Decrementing, if applicable)
    //Update freqMap
    public int pop() {
        Stack<Integer> s = freqStack.get(maxFreq);
        int top = s.pop();
        if(s.isEmpty()) maxFreq--;
        freqMap.put(top, freqMap.get(top)-1);
        return top;
    }
}

//Using ArrayList
class FreqStack {
    Map<Integer, Integer> freqMap;
    ArrayList<ArrayList<Integer>> freqStack;
    int maxFreq;
    public FreqStack() {
        freqMap = new HashMap<>();
        freqStack = new ArrayList<>();
        freqStack.add(new ArrayList<Integer>());
        maxFreq = 0;
    }
    //Increment value in freqMap, 
    //updating the maxFreq
    //Adding value in dfreqStack
    public void push(int x) {
        int freq = freqMap.getOrDefault(x,0)+1;
        freqMap.put(x, freq);
        if(freq > maxFreq) maxFreq = freq;
        //freqStack.computeIfAbsent(freq, f->new Stack()).push(x);
        if(freqStack.size() <= freq) freqStack.add(new ArrayList());
        freqStack.get(freq).add(x);
    }
    
    //Return and remove the top of maxFreq
    //Update maxFreq (Decrementing, if applicable)
    //Update freqMap
    public int pop() {
        ArrayList<Integer> s = freqStack.get(maxFreq);
        int top = s.remove(s.size()-1);
        if(s.isEmpty()) maxFreq--;
        freqMap.put(top, freqMap.get(top)-1);
        return top;
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

