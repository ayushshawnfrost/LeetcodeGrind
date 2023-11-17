# Blind 75


<style>
/* Style to hide the native disclosure triangle in some browsers */
details summary::-webkit-details-marker {
    display: none;
}
</style>


## <h1 style="text-align: center;">1. Arrays & Hashing </h1>

<details id="section1">
<summary> <span style="color:green;font-size:16px;font-weight:bold">
217. Contains Duplicate (Easy)
</span></summary>

https://leetcode.com/problems/contains-duplicate/description/



    Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

    

    Example 1:

    Input: nums = [1,2,3,1]
    Output: true
    Example 2:

    Input: nums = [1,2,3,4]
    Output: false
    Example 3:

    Input: nums = [1,1,1,3,3,4,3,2,4,2]
    Output: true
    

    Constraints:

    1 <= nums.length <= 10^5
    -10^9 <= nums[i] <= 10^9


<span style="color:green;">Solution:</span>

Approach 1: Sorting


Intuition:
The sorting approach sorts the array in ascending order and then checks for adjacent elements that are the same. If any duplicates are found, it returns true. Sorting helps in bringing duplicates together, simplifying the check. However, sorting has a time complexity of O(n log n).

Explanation:
Another approach is to sort the array and then check for adjacent elements that are the same. If any duplicates are found, return true, otherwise return false.
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        for (int i = 1; i < n; i++) {
            if (nums[i] == nums[i - 1])
                return true;
        }
        return false;
    }
}
```

    Time: O(nlog(n))
    Space: O(n)



Approach 2: HashSet
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> hm=new HashSet<>();
        for(int i:nums){
            if(hm.contains(i)){
                return true;
            }
            hm.add(i);
        }
        return false;
    }
}
```
    Time: O(n)
    Space: O(n)

</details>


<details id="Valid Anagram">
<summary> <span style="color:green;font-size:16px;font-weight:bold">242. Valid Anagram (Easy)
</span></summary>

https://leetcode.com/problems/valid-anagram/description/


Given two strings s and t, return true if t is an anagram of s, and false otherwise.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 

Example 1:

Input: s = "anagram", t = "nagaram"
Output: true
Example 2:

Input: s = "rat", t = "car"
Output: false
 

Constraints:

1 <= s.length, t.length <= 5 * 104
s and t consist of lowercase English letters.
 

Follow up: What if the inputs contain Unicode characters? How would you adapt your solution to such a case?


<span style="color:green;">Solution:</span>

Approach 1: Sorting

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        char[] sChars = s.toCharArray();
        char[] tChars = t.toCharArray();
        
        Arrays.sort(sChars);
        Arrays.sort(tChars);
        
        return Arrays.equals(sChars, tChars);
    }
}
```

    Time: O(nlog(n)) n-> length of greater string
    Space: O(n+m)

Approach 2: Hash Table
    
```java
    class Solution {
    public boolean isAnagram(String s, String t) {
        Map<Character, Integer> count = new HashMap<>();
        
        // Count the frequency of characters in string s
        for (char x : s.toCharArray()) {
            count.put(x, count.getOrDefault(x, 0) + 1);
        }
        
        // Decrement the frequency of characters in string t
        for (char x : t.toCharArray()) {
            count.put(x, count.getOrDefault(x, 0) - 1);
        }
        
        // Check if any character has non-zero frequency
        for (int val : count.values()) {
            if (val != 0) {
                return false;
            }
        }
        
        return true;
    }
}
```
    Time: O(n) 
    Space: O(n)
</details>

<details id="Two Sum">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">1. Two Sum (Easy)
</span></summary>

https://leetcode.com/problems/two-sum/description/

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

 

Example 1:

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
Example 2:

Input: nums = [3,2,4], target = 6
Output: [1,2]
Example 3:

Input: nums = [3,3], target = 6
Output: [0,1]
 

Constraints:

2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
Only one valid answer exists.
 

Follow-up: Can you come up with an algorithm that is less than O(n2) time complexity?

<span style="color:green;">Solution:</span>

Solution 1: (Brute Force)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[]{}; // No solution found
    }
}
```
    Time: O(n^2)
    Space: O(1)

Solution 2: (Two-pass Hash Table)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> numMap = new HashMap<>();
        int n = nums.length;

        // Build the hash table
        for (int i = 0; i < n; i++) {
            numMap.put(nums[i], i);
        }

        // Find the complement
        for (int i = 0; i < n; i++) {
            int complement = target - nums[i];
            if (numMap.containsKey(complement) && numMap.get(complement) != i) {
                return new int[]{i, numMap.get(complement)};
            }
        }

        return new int[]{}; // No solution found
    }
}
```
    Time: O(n)
    Space: O(n)    

</details>


<details id="49. Group Anagrams">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">49. Group Anagrams (Medium)
</span></summary>

https://leetcode.com/problems/group-anagrams/description/

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 

Example 1:

Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
Example 2:

Input: strs = [""]
Output: [[""]]
Example 3:

Input: strs = ["a"]
Output: [["a"]]
 

Constraints:

1 <= strs.length <= 104
0 <= strs[i].length <= 100
strs[i] consists of lowercase English letters.


<span style="color:green;">Solution:</span>

```java

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        
        for (String word : strs) {
            char[] chars = word.toCharArray();
            Arrays.sort(chars);
            String sortedWord = new String(chars);
            
            map.putIfAbsent(sortedWord, new ArrayList<>());
            
            map.get(sortedWord).add(word);
        }
        
        return new ArrayList<>(map.values());
    }
}
```

    Time: O(n * k * log(k)), where:

    n is the number of strings in the input array strs.
    k is the maximum length of a string in the input array.
</details>


<details id="347. Top K Frequent Elements">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">347. Top K Frequent Elements (Medium)
</span></summary>

https://leetcode.com/problems/top-k-frequent-elements/description/

Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

 

Example 1:

Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
Example 2:

Input: nums = [1], k = 1
Output: [1]
 

Constraints:

1 <= nums.length <= 105
-104 <= nums[i] <= 104
k is in the range [1, the number of unique elements in the array].
It is guaranteed that the answer is unique.
 

Follow up: Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

<span style="color:green;">Solution:</span>

Approach 1:HashMap and Priority Queue


```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int i:nums){
            map.put(i, map.getOrDefault(i, 0)+1);
        }

        int[] ans = new int[k];
        // Max Heap on the basis of HashMap
        PriorityQueue<Integer> queue = new PriorityQueue<>((a,b)->map.get(b)-map.get(a));

        for(int i:map.keySet()){
            //log(n) operation
            queue.add(i);
        }
        for(int i=0; i<k; i++){
            ans[i]=queue.poll();
        }
        return ans;
    }
}
```

    Time complexity: O( n*log(n) ) where n=Length of nums

    Space complexity: O(n)

Approach 2: Bucket Sort

```java
public List<Integer> topKFrequent(int[] nums, int k) {

	List<Integer>[] bucket = new List[nums.length + 1];
	Map<Integer, Integer> frequencyMap = new HashMap<Integer, Integer>();

	for (int n : nums) {
		frequencyMap.put(n, frequencyMap.getOrDefault(n, 0) + 1);
	}

	for (int key : frequencyMap.keySet()) {
		int frequency = frequencyMap.get(key);
		if (bucket[frequency] == null) {
			bucket[frequency] = new ArrayList<>();
		}
		bucket[frequency].add(key);
	}

	List<Integer> res = new ArrayList<>();

	for (int pos = bucket.length - 1; pos >= 0 && res.size() < k; pos--) {
		if (bucket[pos] != null) {
			res.addAll(bucket[pos]);
		}
	}
	return res;
}
```
    Time complexity: O(n)



</details>

<details id="238. Product of Array Except Self">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">238. Product of Array Except Self (Medium)
</span></summary>

https://leetcode.com/problems/product-of-array-except-self/description/


Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.

 

Example 1:

Input: nums = [1,2,3,4]
Output: [24,12,8,6]
Example 2:

Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
 

Constraints:

2 <= nums.length <= 105
-30 <= nums[i] <= 30
The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.
 

Follow up: Can you solve the problem in O(1) extra space complexity? (The output array does not count as extra space for space complexity analysis.)

<span style="color:green;">Solution:</span>

Approach 1: prefix ans suffix sum

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int pre[] = new int[n];
        int suff[] = new int[n];
        pre[0] = 1;
        suff[n - 1] = 1;
        
        for(int i = 1; i < n; i++) {
            pre[i] = pre[i - 1] * nums[i - 1];
        }
        for(int i = n - 2; i >= 0; i--) {
            suff[i] = suff[i + 1] * nums[i + 1];
        }
        
        int ans[] = new int[n];
        for(int i = 0; i < n; i++) {
            ans[i] = pre[i] * suff[i];
        }
        return ans;
    }
}
```

    Time: O(n)
    Space:O(n)

Approach 2:

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int ans[] = new int[n];
        Arrays.fill(ans, 1);
        int curr = 1;
        for(int i = 0; i < n; i++) {
            ans[i] *= curr;
            curr *= nums[i];
        }
        curr = 1;
        for(int i = n - 1; i >= 0; i--) {
            ans[i] *= curr;
            curr *= nums[i];
        }
        return ans;
    }
}
```
    Time: O(n)
    Space:O(1)
</details>

<details id="128. Longest Consecutive Sequence">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">128. Longest Consecutive Sequence (Medium)
</span></summary>

https://leetcode.com/problems/longest-consecutive-sequence/description/


Given an unsorted array of integers nums, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.

 

Example 1:

Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
Example 2:

Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
 

Constraints:

0 <= nums.length <= 105
-109 <= nums[i] <= 109

<span style="color:green;">Solution:</span>
Approach: check if the element is the starting number in the sequence. Staring no. predecessor will not be present in the set. check the whole sequence if it is the starting number.

```java
class Solution {
    public int longestConsecutive(int[] nums) {
       if (nums.length == 0) return 0;
       HashSet<Integer> hs = new HashSet<>();
       for(int num:nums) hs.add(num);
       int longest =1;
       for(int num: nums ){
           //check if the num is the start of a sequence by checking if left exists
           if(!hs.contains(num-1)){ // start of a sequence
                int count =1;
                while(hs.contains(num + 1)){ // check if hs contains next no.
                    num++;
                    count++;
                }
                longest = Math.max(longest, count);
                
           }
           if(longest > nums.length/2) break;

       }
       return longest;
    }
}

```

    Time: O(n)

</details>

## <h1 style="text-align: center;">2. Two Pointers </h1>


<!-- <details id="Two Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Sample (Medium)
</span></summary>
This is the content that can be collapsed or expanded.
</details> -->