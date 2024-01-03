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
217. Contains Duplicate 
</span></summary>

https://leetcode.com/problems/contains-duplicate/description/



    Given an integer array nums, return true if any value appears 
    at least twice in the array, and return false if every element is distinct.

    

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
The sorting approach sorts the array in ascending order and then checks for 
adjacent elements that are the same. If any duplicates are found, it returns
 true. Sorting helps in bringing duplicates together, simplifying the check. 
 However, sorting has a time complexity of O(n log n).

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
<summary> <span style="color:green;font-size:16px;font-weight:bold">242. Valid Anagram 
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
<span style="color:green;font-size:16px;font-weight:bold">1. Two Sum 
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
<span style="color:yellow;font-size:16px;font-weight:bold">49. Group Anagrams 
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
<span style="color:yellow;font-size:16px;font-weight:bold">347. Top K Frequent Elements 
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
<span style="color:yellow;font-size:16px;font-weight:bold">238. Product of Array Except Self 
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
<span style="color:yellow;font-size:16px;font-weight:bold">128. Longest Consecutive Sequence 
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


<details id="125. Valid Palindrome">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">125. Valid Palindrome 
</span></summary>

https://leetcode.com/problems/valid-palindrome/description/

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string s, return true if it is a palindrome, or false otherwise.

 

Example 1:

Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.

Example 2:

Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.

Example 3:

Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
 

Constraints:

1 <= s.length <= 2 * 105
s consists only of printable ASCII characters.

<span style="color:green;">Solution:</span>

Approach: Simply iterate over the array using 2 pointers and compare the characters
if find any non letter continue the loop.

```java
public boolean isPalindrome(String s) {
        
    int i = 0;
    int j = s.length() - 1;
    while (i < j) {
        
        Character start = s.charAt(i);
        Character end = s.charAt(j);
        
        if (!Character.isLetterOrDigit(start)) {
            i++;
            continue;
        }
        
        if (!Character.isLetterOrDigit(end)) {
            j--;
            continue;
        }
        
        if (Character.toLowerCase(start) != Character.toLowerCase(end)) {
            return false;
        }
        
        i++;
        j--;    
    }
    
    return true;
}
```
    Time: O(n)

</details>

<details id="15. 3Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">15. 3Sum 
</span></summary>

https://leetcode.com/problems/3sum/description/

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

 

Example 1:

Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
Example 2:

Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
Example 3:

Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
 

Constraints:

3 <= nums.length <= 3000
-105 <= nums[i] <= 105

<span style="color:green;">Solution:</span>

Approach:
Iterate over the array and in each iteration pick ith element and try to find the 2 other
element that has sum equal to the negative of this number. We need to ignore the
duplicate elements in the outer loop as well as inner 2 pointer loop in order to get the unique triplets. 

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> sol = new ArrayList<List<Integer>>();

        for (int i = 0; i < nums.length - 2; i++) {
            //Only consider non-duplicate elements for i
            if (i == 0 || (i > 0 && nums[i] != nums[i - 1])) {
                int target = 0 - nums[i];
                int left = i + 1;
                int right = nums.length - 1;

                while (left < right) {
                    if (nums[left] + nums[right] == target) {
                        ArrayList<Integer> miniSol = new ArrayList<>(Arrays.asList(nums[i],nums[left],nums[right]));
                        sol.add(miniSol);
                        //Consider duplicate numbers only once
                        while (left < right && nums[left] == nums[left + 1]) {
                            left++;
                        }
                        while (left < right && nums[right] == nums[right - 1]) {
                            right--;
                        }
                        left++;
                        right--;
                    } else if (nums[left] + nums[right] > target) {
                        right--;
                    } else {
                        left++;
                    }
                }
            }
        }
        return sol;
    }
}
```

    Time: O(n^2)
</details>

<details id="11. Container With Most Water">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">11. Container With Most Water 
</span></summary>

https://leetcode.com/problems/container-with-most-water/description/


You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).
![Alt text](image-3.png)
Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

 

Example 1:


Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

Example 2:

Input: height = [1,1]
Output: 1
 

Constraints:

n == height.length
2 <= n <= 105
0 <= height[i] <= 104

<span style="color:green;">Solution:</span>

Approach:

```java
class Solution {

    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int res = 0;
        while (left < right) {
            int containerLength = right - left;
            int area = containerLength * Math.min(height[left], height[right]);
            res = Math.max(res, area);
            // Get the next bar of the shortest bar of the two
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return res;
    }
}
```
    Time: O(n)




</details>

## <h1 style="text-align: center;">3. Sliding Window </h1>



<details id="121. Best Time to Buy and Sell Stock">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">121. Best Time to Buy and Sell Stock 
</span></summary>

https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/

ou are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

 

Example 1:

Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
Example 2:

Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
 

Constraints:

1 <= prices.length <= 105
0 <= prices[i] <= 104

<span style="color:green;">Solution:</span>


Approach: Keep on checking the difference between the element starting from index 0 and
1.if the left index is bigger then right, make right new left . The reason why made right our new left is that, we have already checked the element sin between they were bigger then the left and the current right is less then left hence the current right is a promissing candidate for buying stock.

```java
class Solution {

    public int maxProfit(int[] prices) {
        int left = 0;
        int right = 1;
        int maxProfit = 0;
        while (right < prices.length) {
            if (prices[left] < prices[right]) {
                maxProfit = Math.max(maxProfit, prices[right] - prices[left]);
            } else {
                left = right;
            }
            right++;
        }
        return maxProfit;
    }
}

```

    Time: O(n)
</details>


<details id="3. Longest Substring Without Repeating Characters">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">3. Longest Substring Without Repeating Characters 
</span></summary>

https://leetcode.com/problems/longest-substring-without-repeating-characters/description/

Given a string s, find the length of the longest 
substring
 without repeating characters.

 

Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
Example 2:

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
 

Constraints:

0 <= s.length <= 5 * 10^4
s consists of English letters, digits, symbols and spaces.

<span style="color:green;">Solution:</span>

Approach: Use 2 pointers starting at index 0. in every iteration push the current character into a HashSet sequentially, if it is already present, this means that current length is the max length for non-repeating sequence.

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s.length()<2)return s.length();
        int ans=0;
        int currlen=0;
        int left=0;
        int right=0;
        HashSet<Character> hs=new HashSet<>();
        while(left<=right && right<s.length()){
            if(hs.contains(s.charAt(right))){
                ans=Math.max(ans,currlen);
                left+=1;
                right=left;
                hs=new HashSet<>();
                currlen=0;
            }else{
                hs.add(s.charAt(right));right++;currlen++;
            }
        }
        return Math.max(ans,currlen);
    }
}
```

    Time: O(n)

</details>



<details id="424. Longest Repeating Character Replacement">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">424. Longest Repeating Character Replacement 
</span></summary>

https://leetcode.com/problems/longest-repeating-character-replacement/description/

You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

 

Example 1:

Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.
Example 2:

Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
There may exists other ways to achieve this answer too.
 

Constraints:

1 <= s.length <= 105
s consists of only uppercase English letters.
0 <= k <= s.length

<span style="color:green;">Solution:</span>


Aproach: We use a 2 pointer approach. left pointer is "i" and right pointer is "j". We
want to keep track of the element whaich has the maximum frequency in a given window. for that make a character map and maintain the frequency. Whenever "right-left+1-max>k"
which means we cannot replace more characters in order to make the sequence repeating. we will increase the left pointer and reduce the frequency of the left char.

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int[] arr = new int[26];
        int ans = 0;
        int max = 0;
        int i = 0;
        for (int j = 0; j < s.length(); j++) {
            arr[s.charAt(j) - 'A']++;
            max = Math.max(max, arr[s.charAt(j) - 'A']);
            if (j - i + 1 - max > k) {
                arr[s.charAt(i) - 'A']--;
                i++;
            }
            ans = Math.max(ans, j - i + 1);
        }
        return ans;
    }
}

```

    Time: O(n)
    Space: O(1)
</details>


<details id="76. Minimum Window Substring">
<summary> 
<span style="color:red;font-size:16px;font-weight:bold">76. Minimum Window Substring (Hard)
</span></summary>
Given two strings s and t of lengths m and n respectively, return the minimum window 
substring
 of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

 

Example 1:

Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

Example 2:

Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.

Example 3:

Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
 

Constraints:

m == s.length
n == t.length
1 <= m, n <= 105
s and t consist of uppercase and lowercase English letters.
 

Follow up: Could you find an algorithm that runs in O(m + n) time?

<span style="color:green;">Solution:</span>

Approach:

```java
class Solution {
    public String minWindow(String s, String t) {
        
        HashMap<Character, Integer> map = new HashMap<>();
        for (char x : t.toCharArray()) {
            map.put(x, map.getOrDefault(x, 0) + 1);
        }

        // Taking count of number of character matched so far
        int matched = 0;
        // Starting index of the window
        int start = 0;
        // min length of answer found sofar (initially making it bigger then s length)
        int minLen = s.length() + 1;
        // starting index of substring 
        int subStr = 0;

        for (int endWindow = 0; endWindow < s.length(); endWindow++) {
            char right = s.charAt(endWindow);
            if (map.containsKey(right)) {
                map.put(right, map.get(right) - 1);
                // one character matched
                if (map.get(right) == 0) {
                    matched++;
                }
            }
            // While the window has all the character matched, try to shorten the window from start 
            while (matched == map.size()) {
                if (minLen > endWindow - start + 1) {
                    minLen = endWindow - start + 1;
                    subStr = start;
                }
                // Delete the character from begining
                char deleted = s.charAt(start);
                start++;
                if (map.containsKey(deleted)) {
                    // If the deleted character count is 0, then that character is unmatched
                    if (map.get(deleted) == 0) {
                        matched--;
                    }
                    map.put(deleted, map.get(deleted) + 1);
                }
            }
        }
        return minLen > s.length() ? "" : s.substring(subStr, subStr + minLen);
    }
}
```

    Time: O(n)
</details>


## <h1 style="text-align: center;">4. Stack </h1>

<details id="20. Valid Parentheses">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">20. Valid Parentheses 
</span></summary>

https://leetcode.com/problems/valid-parentheses/description/

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.
 

Example 1:

Input: s = "()"
Output: true
Example 2:

Input: s = "()[]{}"
Output: true
Example 3:

Input: s = "(]"
Output: false
 

Constraints:

1 <= s.length <= 104
s consists of parentheses only '()[]{}'.

<span style="color:green;">Solution:</span>


```java
class Solution {

    public boolean isValid(String s) {
        if (s.length() % 2 != 0) return false;
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            if (
                stack.isEmpty() &&
                (s.charAt(i) == ')' || s.charAt(i) == '}' || s.charAt(i) == ']')
            ) return false; else {
                    if (
                        s.charAt(i) == ')' && stack.peek() == '('
                    ) stack.pop(); else if (
                        s.charAt(i) == '}' && stack.peek() == '{'
                    ) stack.pop(); else if (
                        s.charAt(i) == ']' && stack.peek() == '['
                    ) stack.pop(); else stack.add(s.charAt(i));
            }
        }
        return stack.isEmpty();
    }
}
```

    Time:O(n)

</details>


## <h1 style="text-align: center;">5. Binary Search </h1>

<details id="704. Binary Search">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">704. Binary Search 
</span></summary>
Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4

Example 2:

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1


Solution:

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0; // initialize left pointer to 0
        int right = nums.length - 1; // initialize right pointer to the last index of the array
        
        while (left <= right) { // continue the loop till left pointer is less than or equal to right pointer
            int mid = left + (right - left) / 2; // calculate the middle index of the array
            
            if (nums[mid] == target) { // check if the middle element is equal to target
                return mid; // return the middle index
            } else if (nums[mid] < target) { // check if the middle element is less than target
                left = mid + 1; // move the left pointer to the right of middle element
            } else { // if the middle element is greater than target
                right = mid - 1; // move the right pointer to the left of middle element
            }
        }
        return -1; // target not found in the array
    }
}
```
</details>



<details id="153. Find Minimum in Rotated Sorted Array">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">153. Find Minimum in Rotated Sorted Array 
</span></summary>

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.
[0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

 

Example 1:

Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
Example 2:

Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
Example 3:

Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 
 

Constraints:

n == nums.length
1 <= n <= 5000
-5000 <= nums[i] <= 5000
All the integers of nums are unique.
nums is sorted and rotated between 1 and n times.

Solution:

Approach: If a array is sorted and rotated, then it has 2 sorted portions. First we want to determine where does the mid pointer lies, in which sorted portion. 
eg) 5,6,7:1,2,3,4  
if at anytime nums[mid] >= nums[l] this means that mid is in 1,2,3,4. now left is mid + 1.

```java
class Solution {
    public int findMin(int[] nums) {
        int l = 0;
        int r = nums.length - 1;
        while (l <= r) {
            if (nums[l] <= nums[r]) {
                return nums[l];
            }
            int mid = (l + r) / 2;
            if (nums[mid] >= nums[l]) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return 0;
    }
}

```
</details>

<details id="33. Search in Rotated Sorted Array">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">33. Search in Rotated Sorted Array 
</span></summary>
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

Approach:
They key here is to judge in which sorted portiondoes the mid lies.

1. if we know the mid lies in the left sorted portion (means mid>=left) then we can check the target. If the target is greater then mid, then we are sure that we will search on right since there is no elements (e>mid) present in left portion.
but if the target is less then mid, then ans could be present in left as well as right. Then we will check if left value is less than the target then we go left else we go in the right portion to search. 

2. similarly on the right sorted array

```java
class Solution {
    public int search(int[] nums, int target) {
        int l=0;
        int r=nums.length-1;

        while(l<=r){
            int mid = (l+r)/2;
            if(nums[mid]==target)return mid;
            //Left sorted portion
            if(nums[l]<= nums[mid]){
                //Always eliminate the possibility of having the elment in the other sorted portion first.
                if(target > nums[mid] || target < nums[l]){
                    l=mid+1;
                }else{
                    r=mid-1;
                }
            }
            //Right sorted portion
            else{
                //Always eliminate the possibility of having the elment in the other sorted portion first.
                if(target < nums[mid] || target > nums[r]){
                    r=mid-1;
                }else{
                    l=mid+1;
                }
            }
        }
        return -1;
    }
}

Time: O(log(n))
```

</details>


## <h1 style="text-align: center;">6. LinkedList </h1>
<details id="206. Reverse Linked List">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">206. Reverse Linked List 
</span></summary>
Given the head of a singly linked list, reverse the list, and return the reversed list.

![Alt text](image-4.png)

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

```java
class Solution {

    public ListNode reverseList(ListNode head) {
        ListNode current = head;
        ListNode previous = null;
        ListNode nextCurrent = null;
    
        while (current != null) {
            nextCurrent = current.next;
            current.next = previous;
            previous = current;
            current = nextCurrent;
        }

        return previous;
    }
}
```
 
</details>


<details id="21. Merge Two Sorted Lists">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">21. Merge Two Sorted Lists 
</span></summary>
You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

 

Example 1:
![Alt text](image-5.png)

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
 public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        final ListNode root = new ListNode();
        ListNode prev = root;
        while (list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                prev.next = list1;
                list1 = list1.next;
            } else {
                prev.next = list2;
                list2 = list2.next;
            }
            prev = prev.next;
        }
        prev.next = list1 != null ? list1 : list2;
        return root.next;
    }
```
</details>


<details id="143. Reorder List">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">143. Reorder List 
</span></summary>
You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln
Reorder the list to be on the following form:

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
You may not modify the values in the list's nodes. Only nodes themselves may be changed.

 

Example 1:

![Alt text](image-6.png)

Input: head = [1,2,3,4]
Output: [1,4,2,3]


Example 2:

![Alt text](image-7.png)

Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
 

Constraints:

The number of nodes in the list is in the range [1, 5 * 104].
1 <= Node.val <= 1000

```java 
  
        //Find middle of list using a slow and fast pointer approach
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        //Reverse the second half of the list using a tmp variable
        ListNode second = slow.next;
        ListNode prev = slow.next = null;
        while (second != null) {
            ListNode tmp = second.next;
            second.next = prev;
            prev = second;
            second = tmp;
        }

        //Re-assign the pointers to match the pattern
        ListNode first = head;
        second = prev;
        while (second != null) {
            ListNode tmp1 = first.next;
            ListNode tmp2 = second.next;
            first.next = second;
            second.next = tmp1;
            first = tmp1;
            second = tmp2;
        }
```

</details>


<details id="19. Remove Nth Node From End of List">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">19. Remove Nth Node From End of List 
</span></summary>
Given the head of a linked list, remove the nth node from the end of the list and return its head.

 ![Alt text](image-8.png)

Example 1:


Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
Example 2:

Input: head = [1], n = 1
Output: []
Example 3:

Input: head = [1,2], n = 1
Output: [1]

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null || head.next == null) return null;
       
        ListNode temp = new ListNode(0);
        temp.next = head;
        ListNode first = temp, second = temp;

        while (n > 0) {
            second = second.next;
            n--;
        }

        while (second.next != null) {
            second = second.next;
            first = first.next;
        }

        first.next = first.next.next;
        return temp.next;
    }
}

```
</details>

<details id="141. Linked List Cycle">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">141. Linked List Cycle 
</span></summary>
Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

 

Example 1:


Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
Example 2:


Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
Example 3:


Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
 

Constraints:

The number of the nodes in the list is in the range [0, 104].
-105 <= Node.val <= 105
pos is -1 or a valid index in the linked-list.
 

Follow up: Can you solve it using O(1) (i.e. constant) memory?


```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) return true;
        }
        return false;
        }
    }
}
```
</details>

<details id="3. Merge k Sorted Lists">
<summary> 
<span style="color:red;font-size:16px;font-weight:bold">3. Merge k Sorted Lists 
</span></summary>
You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

 

Example 1:

Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
Example 2:

Input: lists = []
Output: []
Example 3:

Input: lists = [[]]
Output: []
 

Constraints:

k == lists.length
0 <= k <= 104
0 <= lists[i].length <= 500
-104 <= lists[i][j] <= 104
lists[i] is sorted in ascending order.
The sum of lists[i].length will not exceed 104.

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }

        PriorityQueue<ListNode> queue = new PriorityQueue<>((a, b) -> a.val - b.val);
        for (ListNode node : lists) {
            if (node != null) {
                queue.offer(node);
            }
        }

        ListNode dummy = new ListNode(0);
        ListNode current = dummy;

        while (!queue.isEmpty()) {
            ListNode node = queue.poll();
            current.next = node;
            current = current.next;

            if (node.next != null) {
                queue.offer(node.next);
            }
        }

        return dummy.next;
    }
}
```
</details>

## <h1 style="text-align: center;">7. Trees </h1>
<details id="226. Invert Binary Tree">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">226. Invert Binary Tree 
</span></summary>
Given the root of a binary tree, invert the tree, and return its root.

 

Example 1:
![Alt text](image-9.png)

Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
Example 2:
![Alt text](image-11.png)

Input: root = [2,1,3]
Output: [2,3,1]
Example 3:

Input: root = []
Output: []
 

Constraints:

The number of nodes in the tree is in the range [0, 100].
-100 <= Node.val <= 100



```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        TreeNode node = new TreeNode(root.val);
        node.right = invertTree(root.left);
        node.left = invertTree(root.right);
        return node;
    }
}
```


</details>


<details id="104. Maximum Depth of Binary Tree">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">104. Maximum Depth of Binary Tree 
</span></summary>
Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

 

Example 1:
![Alt text](image-12.png)

Input: root = [3,9,20,null,null,15,7]
Output: 3
Example 2:

Input: root = [1,null,2]
Output: 2
 

Constraints:

The number of nodes in the tree is in the range [0, 104].
-100 <= Node.val <= 100


```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root ==null)return 0;
        return 1 + Math.max(maxDepth(root.left),maxDepth(root.right));
    }
}
```
</details>

<details id="100. Same Tree">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">100. Same Tree 
</span></summary>
Given the roots of two binary trees p and q, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

 

Example 1:
![Alt text](image-13.png)

Input: p = [1,2,3], q = [1,2,3]
Output: true


Example 2:
![Alt text](image-14.png)

Input: p = [1,2], q = [1,null,2]
Output: false


Example 3:
![Alt text](image-15.png)

Input: p = [1,2,1], q = [1,1,2]
Output: false
 

Constraints:

The number of nodes in both trees is in the range [0, 100].
-104 <= Node.val <= 104

```java
class Solution {
   public boolean isSameTree(TreeNode p, TreeNode q) {
        return dfs(p, q);
    }

    private boolean dfs(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }

        if (p == null || q == null) {
            return false;
        }
        if (p.val != q.val) return false;
        return dfs(p.left, q.left) && dfs(p.right, q.right);
    }
}
```
</details>

<details id="235. Lowest Common Ancestor of a Binary Search Tree">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">235. Lowest Common Ancestor of a Binary Search Tree 
</span></summary>
Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

 

Example 1:

![Alt text](image-16.png)
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.

Example 2:

![Alt text](image-17.png)
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
Example 3:

Input: root = [2,1], p = 2, q = 1
Output: 2
 

Constraints:

The number of nodes in the tree is in the range [2, 105].
-109 <= Node.val <= 109
All Node.val are unique.
p != q
p and q will exist in the BST.

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root!=null && p.val > root.val && q.val > root.val){
            return lowestCommonAncestor(root.right, p,q);
        }
        if(root!=null && p.val < root.val && q.val < root.val){
            return lowestCommonAncestor(root.left, p,q);
        }
        return root;
    }
}
```
</details>

<details id="102. Binary Tree Level Order Traversal">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">102. Binary Tree Level Order Traversal 
</span></summary>
Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

 

Example 1:


Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
Example 2:

Input: root = [1]
Output: [[1]]
Example 3:

Input: root = []
Output: []
 

Constraints:

The number of nodes in the tree is in the range [0, 2000].
-1000 <= Node.val <= 1000

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> arr=new ArrayList<>();
        Queue<TreeNode> q=new LinkedList<>();
        q.add(root);
        while(!q.isEmpty()){
            List<Integer> sub =new ArrayList<>();
            int len=q.size();
            for(int i=0;i<len;i++){
                TreeNode temp=q.poll();
                if(temp!=null){
                    sub.add(temp.val);
                    q.add(temp.left);
                    q.add(temp.right);
                }
            }
           if(!sub.isEmpty())arr.add(sub);
        }
        return arr;
    }
}
```
</details>

<details id="98. Validate Binary Search Tree">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">98. Validate Binary Search Tree 
</span></summary>
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

The left 
subtree
 of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
 

Example 1:


Input: root = [2,1,3]
Output: true

Example 2:

![Alt text](image-18.png)

Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
 

Constraints:

The number of nodes in the tree is in the range [1, 104].
-231 <= Node.val <= 231 - 1

```java
class Solution {
    public boolean helper(TreeNode root, Integer min, Integer max) {
        if(root == null)return true;
        if(( min!=null && root.val<=min) || (max!=null &&root.val>=max) ){
            return false;
        }
        return helper(root.left,min, root.val) && helper(root.right, root.val, max);
    }
    public boolean isValidBST(TreeNode root) {
        return helper(root,null, null);
    }
}
```
</details>

<details id="230. Kth Smallest Element in a BST">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">230. Kth Smallest Element in a BST 
</span></summary>
Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

 

Example 1:

![Alt text](image-19.png)

Input: root = [3,1,4,null,2], k = 1
Output: 1

Example 2:

![Alt text](image-20.png)

Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
 

Constraints:

The number of nodes in the tree is n.
1 <= k <= n <= 104
0 <= Node.val <= 104
 

Follow up: If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?


```java
class Solution {
    public void helper(TreeNode root, List<Integer> arr) {
        if(root==null)return;
        helper(root.left,arr);
        arr.add(root.val);
        helper(root.right,arr);
    }
    public int kthSmallest(TreeNode root, int k) {
        List<Integer> arr=new ArrayList<>();
        helper(root,arr);
        return arr.get(k-1);
    }
}
```
</details>




<details id="105. Construct Binary Tree from Preorder and Inorder Traversal">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">105. Construct Binary Tree from Preorder and Inorder Traversal 
</span></summary>
Given two integer arrays preorder and inorder where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree.

 

Example 1:

![Alt text](image-21.png)

Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
Example 2:

Input: preorder = [-1], inorder = [-1]
Output: [-1]
 

Constraints:

1 <= preorder.length <= 3000
inorder.length == preorder.length
-3000 <= preorder[i], inorder[i] <= 3000
preorder and inorder consist of unique values.
Each value of inorder also appears in preorder.
preorder is guaranteed to be the preorder traversal of the tree.
inorder is guaranteed to be the inorder traversal of the tree.

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder.length==0 || inorder.length==0)return null;
        TreeNode head=new TreeNode(preorder[0]);
        int mid=-1;
        for(int i=0;i<inorder.length;i++){
            if(preorder[0]==inorder[i]){
                mid=i;
            }
        }
        // there are 1 to mid+1 number of elements we need in preorder.
        head.left=buildTree(
            Arrays.copyOfRange(preorder, 1,mid+1),
            Arrays.copyOfRange(inorder, 0,mid));
        head.right=buildTree(
            Arrays.copyOfRange(preorder, mid+1,preorder.length),
            Arrays.copyOfRange(inorder, mid+1,inorder.length));
        return head;
    }
}
```
</details>


<details id="124. Binary Tree Maximum Path Sum">
<summary> 
<span style="color:red;font-size:16px;font-weight:bold">124. Binary Tree Maximum Path Sum 
</span></summary>
A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.

 

Example 1:

![Alt text](image-22.png)

Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.

Example 2:

![Alt text](image-23.png)


Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
 

Constraints:

The number of nodes in the tree is in the range [1, 3 * 104].
-1000 <= Node.val <= 1000


```java
class Solution {

    public int maxPathSum(TreeNode root) {
        int[] res = { Integer.MIN_VALUE };
        maxPathSum(root, res);
        return res[0];
    }

    public int maxPathSum(TreeNode root, int[] res) {
        // return the maximum sum without split
        if (root == null) return 0;
        // maximum sum from the left
        int left = Math.max(0, maxPathSum(root.left, res));
        // maximum sum from the right
        int right = Math.max(0, maxPathSum(root.right, res));
        // max sum so far with the split
        res[0] = Math.max(res[0], root.val + left + right);

        return root.val + Math.max(left, right);
    }
}
```

    Time: O(n)
    Space: log(n)
</details>



<details id="297. Serialize and Deserialize Binary Tree">
<summary> 
<span style="color:red;font-size:16px;font-weight:bold">297. Serialize and Deserialize Binary Tree 
</span></summary>

https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

Clarification: The input/output format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

 

Example 1:

![Alt text](image-24.png)

Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
Example 2:

Input: root = []
Output: []
 

Constraints:

The number of nodes in the tree is in the range [0, 104].
-1000 <= Node.val <= 1000


```java
public class Codec {
    private int i=0;
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        List<String> arr = new ArrayList<>();
        sHelper(root,arr);
        return String.join(",",arr);
    }
    // sHelper is converting a tree into an array of preorder string token, for null->N
    public void sHelper(TreeNode root, List<String> arr) {
        if(root==null){
            arr.add("N");
            return;
        }
        arr.add(Integer.toString(root.val));
        sHelper(root.left, arr);
        sHelper(root.right, arr);
    }
    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] splits=data.split(",");
        return dHelper(splits);
    }
    public TreeNode dHelper(String[] data) {
        String temp=data[i];
        if(temp.equals("N")){
            i++;
            return null;
        }
        TreeNode newNode=new TreeNode(Integer.parseInt(temp));
        i++;
        newNode.left=dHelper(data);
        newNode.right=dHelper(data);
        return newNode;
    }
}
```

    Time: O(n) for each function
</details>



## <h1 style="text-align: center;">8. Heap/Priority Queue </h1>
<details id="295. Find Median from Data Stream">
<summary> 
<span style="color:red;font-size:16px;font-weight:bold">295. Find Median from Data Stream 
</span></summary>

https://leetcode.com/problems/find-median-from-data-stream/


The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.

For example, for arr = [2,3,4], the median is 3.
For example, for arr = [2,3], the median is (2 + 3) / 2 = 2.5.
Implement the MedianFinder class:

MedianFinder() initializes the MedianFinder object.
void addNum(int num) adds the integer num from the data stream to the data structure.
double findMedian() returns the median of all elements so far. Answers within 10-5 of the actual answer will be accepted.
 

Example 1:

Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
 

Constraints:

-105 <= num <= 105
There will be at least one element in the data structure before calling findMedian.
At most 5 * 104 calls will be made to addNum and findMedian.
 

Follow up:

If all integer numbers from the stream are in the range [0, 100], how would you optimize your solution?
If 99% of all integer numbers from the stream are in the range [0, 100], how would you optimize your solution?

![Alt text](image-25.png)
```java
class MedianFinder {
    // maxheap can have 1 more element then min heap
        PriorityQueue<Integer> maxheap=new PriorityQueue<Integer>((a,b)->b-a);
        PriorityQueue<Integer> minheap=new PriorityQueue<Integer>((a,b)->a-b);
    public MedianFinder() {

    }
    
    public void addNum(int num) {
        if(maxheap.isEmpty() || maxheap.peek()>=num){
            maxheap.offer(num);
        }else{
            minheap.offer(num);
        }
        // size adjustments| maxheap can have 1 more element then minheap
        if(maxheap.size()>1+minheap.size()){
            minheap.offer(maxheap.poll());
        }else if(maxheap.size()<minheap.size()){
            maxheap.offer(minheap.poll());
        }
    }
    
    public double findMedian() {
        if(maxheap.size()==minheap.size()){
            // even number of elements
            return maxheap.peek()/2.0 + minheap.peek()/2.0;
        }
        // odd number of elements
        return maxheap.peek();
    }
}

```
</details>

## <h1 style="text-align: center;">9. Backtracking </h1>

<details id="39. Combination Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">39. Combination Sum 
</span></summary>
Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the 
frequency
 of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

 

Example 1:

Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
Example 2:

Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
Example 3:

Input: candidates = [2], target = 1
Output: []
 

Constraints:

1 <= candidates.length <= 30
2 <= candidates[i] <= 40
All elements of candidates are distinct.
1 <= target <= 40

![Alt text](image-26.png)

![Alt text](image-27.png)

https://www.youtube.com/watch?v=OyZFFqQtu98&t=1409s


Approach: Try to start with the first element. We have 2 paths, 1. include current element 
and try to include it again. 2. not include current element and try next element. By doing this kind of approch we will never get duplicate solution.

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans=new ArrayList<>();
        List<Integer> sub=new ArrayList<>();
        dfs(ans,sub,candidates,target,0,0);
        return ans;
    }

    public void dfs(List<List<Integer>> ans, List<Integer> sub, int[] candidates, int target, int currSum, int index){
        if(currSum==target){
            ans.add(sub);return;
        }
        if(currSum>target || index >=candidates.length){
            return;
        }
        // add current index element 
        sub.add(candidates[index]);
        // try to take same element again (by passing the same index)
        dfs(ans,new ArrayList<>(sub),candidates,target,currSum+candidates[index],index);
        // remove the added element 
        sub.remove(sub.size()-1);
        // choose nect index to include
        dfs(ans,new ArrayList<>(sub),candidates,target,currSum,index+1);
    }
}
```


</details>



<details id="79. Word Search">
<summary> 
<Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

 

Example 1:

![Alt text](image-28.png)

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true

Example 2:

![Alt text](image-29.png)

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true

Example 3:

![Alt text](image-30.png)

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
 

Constraints:

m == board.length
n = board[i].length
1 <= m, n <= 6
1 <= word.length <= 15
board and word consists of only lowercase and uppercase English letters.
 

Follow up: Could you use search pruning to make your solution faster with a larger board?


```java


//This is a typical backtracking problem similar to Number of Islands.
//In number of Isalnds, we sinked the islands. Here, we will do the same by adding changing the value of the char.
//I added 100 because it will exceed the ascii limit for characters and will change it to some ascii value which is not an alphabet.
class Solution {
   public boolean check(
        char[][] board,
        String word,
        int i,
        int j,
        int m,
        int n,
        int cur
    ) {
        if (cur >= word.length()) return true;
        if (
            i < 0 ||
            j < 0 ||
            i >= m ||
            j >= n ||
            board[i][j] != word.charAt(cur)
        ) return false;
        boolean exist = false;
        if (board[i][j] == word.charAt(cur)) {
            board[i][j] += 100;
            exist =
                check(board, word, i + 1, j, m, n, cur + 1) ||
                check(board, word, i, j + 1, m, n, cur + 1) ||
                check(board, word, i - 1, j, m, n, cur + 1) ||
                check(board, word, i, j - 1, m, n, cur + 1);
            board[i][j] -= 100;
        }
        return exist;
    }
    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (check(board, word, i, j, m, n, 0)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```
</details>


## <h1 style="text-align: center;">10. Graphs </h1>

<details id="200. Number of Islands">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">200. Number of Islands 
</span></summary>
Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

 

Example 1:

Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
Example 2:

Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 300
grid[i][j] is '0' or '1'.

```java
class Solution {
    int ans=0;
    public void dfs(char[][] grid, int i, int j) {
        if(i>=grid.length || j>=grid[0].length || i<0 || j<0 || grid[i][j] == '0')return;
        grid[i][j]='0';
        dfs(grid,i+1,j);
        dfs(grid,i,j+1);
        dfs(grid,i-1,j);
        dfs(grid,i,j-1);
    }
    public int numIslands(char[][] grid) {
        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){
                if(grid[i][j]=='1'){
                    dfs(grid,i,j);
                    ans++;
                }
            }
        }
        return ans;
    }
}
```

</details>

<details id="133. Clone Graph">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">133. Clone Graph 
</span></summary>
Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

class Node {
    public int val;
    public List<Node> neighbors;
}
 

Test case format:

For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with val == 1, the second node with val == 2, and so on. The graph is represented in the test case using an adjacency list.

An adjacency list is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with val = 1. You must return the copy of the given node as a reference to the cloned graph.

 

Example 1:

![Alt text](image-31.png)

Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).

Example 2:

![Alt text](image-32.png)

Input: adjList = [[]]
Output: [[]]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.
Example 3:

Input: adjList = []
Output: []
Explanation: This an empty graph, it does not have any nodes.
 

Constraints:

The number of nodes in the graph is in the range [0, 100].
1 <= Node.val <= 100
Node.val is unique for each node.
There are no repeated edges and no self-loops in the graph.
The Graph is connected and all nodes can be visited starting from the given node.

![Alt text](image-33.png)
```java
class Solution {
    Map<Integer,Node> map=new HashMap<>();
    public Node cloneGraph(Node node) {
        if(node==null)return null;
        // If that new node is already present return that 
        if(map.containsKey(node.val))return map.get(node.val);
        Node newNode=new Node(node.val,new ArrayList<Node>());
        map.put(node.val,newNode);
        // Recursively make and add all the neighbours
        for(Node neighbour:node.neighbors){
            newNode.neighbors.add(cloneGraph(neighbour));
        }
        return newNode;
    }
}
```

    Time: O(n+v)
</details>


<details id="695. Max Area of Island">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">695. Max Area of Island 
</span></summary>
You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The area of an island is the number of cells with a value 1 in the island.

Return the maximum area of an island in grid. If there is no island, return 0.

 

Example 1:

![Alt text](image-34.png)

Input: grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.
Example 2:

Input: grid = [[0,0,0,0,0,0,0,0]]
Output: 0
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 50
grid[i][j] is either 0 or 1.

```java
class Solution {
    public int helper(int[][] grid, int i, int j) {
        if(i<0 || j<0 || i>=grid.length || j>=grid[0].length || grid[i][j]!=1)return 0;
        grid[i][j]=0;
        return 1+helper(grid,i+1,j) + helper(grid,i,j+1) + helper(grid,i-1,j) + helper(grid,i,j-1);
    }
    public int maxAreaOfIsland(int[][] grid) {
        int ans=0;
        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){
                if(grid[i][j]==1){
                    ans=Math.max(ans,helper(grid,i,j));
                }
            }
        }
        return ans;
    }
}
```
</details>


<details id="417. Pacific Atlantic Water Flow">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">417. Pacific Atlantic Water Flow 
</span></summary>
There is an m x n rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an m x n integer matrix heights where heights[r][c] represents the height above sea level of the cell at coordinate (r, c).

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a 2D list of grid coordinates result where result[i] = [ri, ci] denotes that rain water can flow from cell (ri, ci) to both the Pacific and Atlantic oceans.

 

Example 1:

![Alt text](image-35.png)

Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
Explanation: The following cells can flow to the Pacific and Atlantic oceans, as shown below:
[0,4]: [0,4] -> Pacific Ocean 
       [0,4] -> Atlantic Ocean
[1,3]: [1,3] -> [0,3] -> Pacific Ocean 
       [1,3] -> [1,4] -> Atlantic Ocean
[1,4]: [1,4] -> [1,3] -> [0,3] -> Pacific Ocean 
       [1,4] -> Atlantic Ocean
[2,2]: [2,2] -> [1,2] -> [0,2] -> Pacific Ocean 
       [2,2] -> [2,3] -> [2,4] -> Atlantic Ocean
[3,0]: [3,0] -> Pacific Ocean 
       [3,0] -> [4,0] -> Atlantic Ocean
[3,1]: [3,1] -> [3,0] -> Pacific Ocean 
       [3,1] -> [4,1] -> Atlantic Ocean
[4,0]: [4,0] -> Pacific Ocean 
       [4,0] -> Atlantic Ocean
Note that there are other possible paths for these cells to flow to the Pacific and Atlantic oceans.
Example 2:

Input: heights = [[1]]
Output: [[0,0]]
Explanation: The water can flow from the only cell to the Pacific and Atlantic oceans.
 

Constraints:

m == heights.length
n == heights[r].length
1 <= m, n <= 200
0 <= heights[r][c] <= 105

```java
class Solution {
    // Applying the logic of count the no. of iceland will not give us the output (AtackOverFlowError) since we do not have anyting to track the visited nodes.
    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        List<List<Integer>> res = new ArrayList<>();
        // Lets work from outside->inside.
        int rows = heights.length, cols = heights[0].length;
        boolean[][] pacific = new boolean[rows][cols];
        boolean[][] atlantic = new boolean[rows][cols];

        // 1st and last row will reach to pac and alt ocean respectively. So, start from there and work inwards.
        for (int i = 0; i < cols; i++) {
            dfs(heights, 0, i, Integer.MIN_VALUE, pacific);
            dfs(heights, rows - 1, i, Integer.MIN_VALUE, atlantic);
        }
        // 1st and last column will reach to pac and alt ocean respectively. So, start from there and work inwards.
        for (int i = 0; i < rows; i++) {
            dfs(heights, i, 0, Integer.MIN_VALUE, pacific);
            dfs(heights, i, cols - 1, Integer.MIN_VALUE, atlantic);
        }
        // Push the indexs which are true for both pacific and atlantic 
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (pacific[i][j] && atlantic[i][j]) {
                    res.add(List.of(i, j));
                }
            }
        }
        return res;
    }

    private void dfs(
        int[][] heights,
        int i,
        int j,
        int prev,
        boolean[][] ocean
    ) {
        if (i < 0 || i >= ocean.length || j < 0 || j >= ocean[0].length || heights[i][j] < prev || ocean[i][j]) return;
        ocean[i][j] = true;
        dfs(heights, i + 1, j, heights[i][j], ocean);
        dfs(heights, i, j + 1, heights[i][j], ocean);
        dfs(heights, i - 1, j, heights[i][j], ocean);
        dfs(heights, i, j - 1, heights[i][j], ocean);
    }
}

```
</details>



<details id="130. Surrounded Regions">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">130. Surrounded Regions 
</span></summary>
Given an m x n matrix board containing 'X' and 'O', capture all regions that are 4-directionally surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

 

Example 1:

![Alt text](image-36.png)

Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
Explanation: Notice that an 'O' should not be flipped if:
- It is on the border, or
- It is adjacent to an 'O' that should not be flipped.
The bottom 'O' is on the border, so it is not flipped.
The other three 'O' form a surrounded region, so they are flipped.
Example 2:

Input: board = [["X"]]
Output: [["X"]]
 

Constraints:

m == board.length
n == board[i].length
1 <= m, n <= 200
board[i][j] is 'X' or 'O'.

```java
class Solution {
    public void dfs(char[][] grid, int i, int j) {
        if(i>=grid.length || j>=grid[0].length || i<0 || j<0 || grid[i][j] != 'O')return;
        grid[i][j]='A';
        dfs(grid,i+1,j);
        dfs(grid,i,j+1);
        dfs(grid,i-1,j);
        dfs(grid,i,j-1);
    }
    public void solve(char[][] board) {
        // This means that all the 'O' at the 1st and last row/column and its connected '0' will remain intact. All other '0' should be converted to 'X'.
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                if(i==0 || i==board.length-1 || j==0 || j==board[0].length-1 ){
                    // Find all the 'O' and convert all the connected 'O' to 'A'.
                    if(board[i][j]=='O'){
                        dfs(board,i,j);
                    }
                }
            }
        }
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                // All remaining 'O' should be "captured", hence flip it to 'X'
                if(board[i][j]=='O'){
                    board[i][j]='X';
                }
            }
        }
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                // All the 'A' should become 'O'
                if(board[i][j]=='A'){
                    board[i][j]='O';
                }
            }
        }
    }
}
```
</details>

<details id="994. Rotting Oranges">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">994. Rotting Oranges 
</span></summary>
You are given an m x n grid where each cell can have one of three values:

0 representing an empty cell,
1 representing a fresh orange, or
2 representing a rotten orange.
Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1.

 

Example 1:

![Alt text](image-37.png)

Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
Example 2:

Input: grid = [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
Explanation: The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.
Example 3:

Input: grid = [[0,2]]
Output: 0
Explanation: Since there are already no fresh oranges at minute 0, the answer is just 0.
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 10
grid[i][j] is 0, 1, or 2.

```java
class Cell{
    int x;int y;
    Cell(int x,int y){
        this.x=x;this.y=y;
    }
}
class Solution {
    // Multi source BFS algorithm. (DFS will fail to  solve this)
    public int orangesRotting(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        Queue<Cell> queue = new LinkedList<>();
        // we will keep fresh count, so that we can know when the algorithm ends
        int fresh = 0;

        for (int i = 0; i < m; i += 1) {
            for (int j = 0; j < n; j += 1) {
                if (grid[i][j] == 2) {
                    queue.offer(new Cell (i, j));
                    } 
                else if (grid[i][j] == 1) fresh += 1;
            }
        }

        int count = 0;
        int[][] dirs = { { 1, 0 }, { -1, 0 }, { 0, 1 }, { 0, -1 } };
        while (!queue.isEmpty() && fresh != 0) {
            count += 1;
            int sz = queue.size();
            for (int i = 0; i < sz; i += 1) {
                Cell rotten = queue.poll();
                int r = rotten.x, c = rotten.y;
                for (int[] dir : dirs) {
                    int x = r + dir[0], y = c + dir[1];
                    if (0 <= x && x < m && 0 <= y && y < n && grid[x][y] == 1) {
                        grid[x][y] = 2;
                        queue.offer(new Cell (x, y));
                        fresh -= 1;
                    }
                }
            }
        }
        return fresh == 0 ? count : -1;
    }
}
```

    Time: O(m+n)
    Space: O(m+n)
</details>

<details id="207. Course Schedule">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">207. Course Schedule 
</span></summary>
There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return true if you can finish all courses. Otherwise, return false.

 

Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
Example 2:

Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
 

Constraints:

1 <= numCourses <= 2000
0 <= prerequisites.length <= 5000
prerequisites[i].length == 2
0 <= ai, bi < numCourses
All the pairs prerequisites[i] are unique.

Sol:
![Alt text](image-38.png)

https://www.youtube.com/watch?v=EUDwWbvtB_Q

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Topological sort: by Khan's Algorithm
        int[] indegree = new int[numCourses];
        List<Integer>[] adj = new ArrayList[numCourses];
        
        // Prepare a adjcency list
        for (int i = 0; i < numCourses; i++) {
            adj[i] = new ArrayList<>();
        }
        // Prepare a indegree array for each node
        for (int[] prereq : prerequisites) {
            // Keep in mind for [1,0]: 0 should be taken then 1 hence 0->1
            adj[prereq[1]].add(prereq[0]);
            indegree[prereq[0]]++;
        }
        // Add all the nodes to queue which ahs indegree 0
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.add(i);
            }
        }
        
        int visited = 0;
        while (!queue.isEmpty()) {
            int node = queue.poll();
            visited++;
            // Check all its neighbours and decrement their indegree
            for (int neighbor : adj[node]) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) {
                    queue.add(neighbor);
                }
            }
        }
        
        return numCourses == visited;
    }
}
```
</details>

<details id="323. Number of Connected Components in an Undirected Graph">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">323. Number of Connected Components in an Undirected Graph 
</span></summary>
Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

Example 1:

Input: n = 5 and edges = [[0, 1], [1, 2], [3, 4]]

     0          3
     |          |
     1 --- 2    4

Output: 2Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

Example 1:

Input: n = 5 and edges = [[0, 1], [1, 2], [3, 4]]

     0          3
     |          |
     1 --- 2    4

Output: 2
Example 2:

Input: n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]]

     0           4
     |           |
     1 --- 2 --- 3

Output:  1
Note:
You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.
Example 2:

Input: n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]]

     0           4
     |           |
     1 --- 2 --- 3

Output:  1
Note:
You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.


</details>

## <h1 style="text-align: center;">11. 1-D Dynamic Programming </h1>

<details id="70. Climbing Stairs">
<summary> 
<span style="color:green;font-size:16px;font-weight:bold">70. Climbing Stairs 
</span></summary>
You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

 

Example 1:

Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
Example 2:

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
 

Constraints:

1 <= n <= 45


```java
// Bottom-up
class Solution {

    public int climbStairs(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;

        for (int i = 2; i < n + 1; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
}

```
```java
// Top down with memo[]
class Solution {

    public int climbStairs(int n) {
        int[] memo = new int[n + 1];
        Arrays.fill(memo, -1);
        // To reach n we can take 1 step from n-1 or 2 steps from n-2.
        return climbStairs(n - 1, memo) + climbStairs(n - 2, memo);
    }

    private int climbStairs(int n, int[] memo) {
        if (n < 0) return 0;
        if (n == 0 || n == 1) {
            memo[n] = 1;
            return memo[n];
        }
        if (memo[n] != -1) return memo[n];

        memo[n] = climbStairs(n - 1, memo) + climbStairs(n - 2, memo);
        return memo[n];
    }
}
```
</details>


<details id="198. House Robber">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">198. House Robber 
</span></summary>

https://leetcode.com/problems/house-robber/

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 

Example 1:

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
Example 2:

Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
 

Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 400

```java
// Bottom-up 
class Solution {
    public int rob(int[] nums) {
        if(nums.length==1)return nums[0];
        if(nums.length==2)return Math.max(nums[0], nums[1]);

        int[] dp=new int[nums.length+1];
        dp[0]=0;
        dp[1]=nums[0];
        dp[2]=nums[1]>nums[0]?nums[1]:nums[0];
        for(int i=2;i<nums.length+1;i++){
            dp[i]=dp[i-2]+nums[i-1]>dp[i-1]?dp[i-2]+nums[i-1]:dp[i-1];
        }
        return dp[nums.length];
    }
}
```
</details>

<details id="213. House Robber II">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">213. House Robber II 
</span></summary>
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 

Example 1:

Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
Example 2:

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
Example 3:

Input: nums = [1,2,3]
Output: 3
 

Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 1000


```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length==1)return nums[0];
        if(nums.length==2)return Math.max(nums[0],nums[1]);

        int[] dp0=new int[nums.length];
        dp0[1]=nums[0];
        dp0[2]=Math.max(nums[0],nums[1]);
        for(int i=3;i<nums.length;i++){
            dp0[i]=Math.max(dp0[i-2]+nums[i-1],dp0[i-1]);
        }     
        int[] dp1=new int[nums.length+1];
        dp1[2]=nums[1];
        dp1[3]=Math.max(nums[1],nums[2]);
        for(int i=4;i<nums.length+1;i++){
            dp1[i]=Math.max(dp1[i-2]+nums[i-1],dp1[i-1]);
        }  
        return Math.max(dp0[nums.length-1],dp1[nums.length]);      
    }
}
```
</details>





<details id="5. Longest Palindromic Substring">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">5. Longest Palindromic Substring 
</span></summary>
Given a string s, return the longest 
palindromic
 
substring
 in s.

 

Example 1:

Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
Example 2:

Input: s = "cbbd"
Output: "bb"
 

Constraints:

1 <= s.length <= 1000
s consist of only digits and English letters.

```java
class Solution {
    public String longestPalindrome(String s) {
        int longestPalLen=0;
        String longestPal="";
        for(int i=0;i<s.length();i++){
            //odd length palindrome
            int l=i; int r=i;
            while(l>=0 && r<s.length() && s.charAt(l)==s.charAt(r)){
                if(r-l+1>longestPalLen)
                { 
                    longestPalLen=r-l+1;
                    longestPal=s.substring(l,r+1);
                }
                l--;r++;
            }
            //even length palindrome
            l=i; r=i+1;
            while(l>=0 && r<s.length() && s.charAt(l)==s.charAt(r)){
                if(r-l+1>longestPalLen)
                {
                    longestPalLen=r-l+1;
                    longestPal=s.substring(l,r+1);
                }
                l--;r++;
            }
        }
        return longestPal;
    }
}
```
</details>





<details id="647. Palindromic Substrings">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">647. Palindromic Substrings 
</span></summary>
Given a string s, return the number of palindromic substrings in it.

A string is a palindrome when it reads the same backward as forward.

A substring is a contiguous sequence of characters within the string.

 

Example 1:

Input: s = "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
Example 2:

Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
 

Constraints:

1 <= s.length <= 1000
s consists of lowercase English letters.

```java
class Solution {
    // Similar approach as used in "5. Longest Palindromic Substring"
    public int helper(String s, int l, int r){
        int count=0;
        while(l>=0 && r<s.length()){
            if(s.charAt(l)==s.charAt(r)){
                count++;l--;r++;
            }else break;
        }
        return count;
    }
    public int countSubstrings(String s) {
        int ans=0;
        for(int i=0;i<s.length();i++){
            // check for odd length palindrome
            ans+=helper(s,i,i);
            // check for even length palindrome
            ans+=helper(s,i,i+1);
        }
        return ans;
    }
}
```
</details>





<details id="91. Decode Ways">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">91. Decode Ways 
</span></summary>
A message containing letters from A-Z can be encoded into numbers using the following mapping:

'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:

"AAJF" with the grouping (1 1 10 6)
"KJF" with the grouping (11 10 6)
Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06".

Given a string s containing only digits, return the number of ways to decode it.

The test cases are generated so that the answer fits in a 32-bit integer.

 

Example 1:

Input: s = "12"
Output: 2
Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).
Example 2:

Input: s = "226"
Output: 3
Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
Example 3:

Input: s = "06"
Output: 0
Explanation: "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").
 

Constraints:

1 <= s.length <= 100
s contains only digits and may contain leading zero(s).

```java
// Main Solution
// Bottom up dynamic prog
class Solution {

    public int numDecodings(String s) {
        int[] dp = new int[s.length() + 1];
        dp[0] = 1; // empty string
        dp[1] = s.charAt(0) == '0' ? 0 : 1;
        for (int i = 2; i < s.length() + 1; i++) {
            if (s.charAt(i - 1) != '0') {
                dp[i] += dp[i - 1];
            }
            if (
                s.charAt(i - 2) == '1' ||
                (s.charAt(i - 2) == '2' && s.charAt(i - 1) < '7')
            ) {
                dp[i] += dp[i - 2];
            }
        }
        return dp[s.length()];
    }
}
```

```java
//top down with memoization
class Solution {

    public int numDecodings(String s) {
        return numDecodings(s, 0, new Integer[s.length()]);
    }

    private int numDecodings(String s, int i, Integer[] dp) {
        if (i == s.length()) return 1;
        if (s.charAt(i) == '0') return 0;
        if (dp[i] != null) return dp[i];
        int count = 0;
        count += numDecodings(s, i + 1, dp);
        if (
            i < s.length() - 1 &&
            (s.charAt(i) == '1' || s.charAt(i) == '2' && s.charAt(i + 1) < '7')
        ) {
            count += numDecodings(s, i + 2, dp);
        }
        dp[i] = count;
        return dp[i];
    }
}
```
</details>





<details id="322. Coin Change">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">322. Coin Change 
</span></summary>
You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

 

Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
Example 2:

Input: coins = [2], amount = 3
Output: -1
Example 3:

Input: coins = [1], amount = 0
Output: 0
 

Constraints:

1 <= coins.length <= 12
1 <= coins[i] <= 231 - 1
0 <= amount <= 104

```java
// Top-Down dp
class Solution {
    public int coinChange(int[] coins, int amount) {
        if(coins.length==0 || amount==0)return 0;
        int[] dp= new int[amount+1];
        Arrays.fill(dp,amount+1);
        dp[0]=0;
        for(int i=1;i<=amount;i++){
            for(int coin:coins){
                if(i-coin>=0){
                    dp[i]=Math.min(dp[i],1+dp[i-coin]);
                }
            }
        }
        return dp[amount]==amount+1?-1:dp[amount];
    }
}
```
</details>





<details id="152. Maximum Product Subarray">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">152. Maximum Product Subarray 
</span></summary>
Given an integer array nums, find a 
subarray
 that has the largest product, and return the product.

The test cases are generated so that the answer will fit in a 32-bit integer.

 

Example 1:

Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
Example 2:

Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
 

Constraints:

1 <= nums.length <= 2 * 104
-10 <= nums[i] <= 10
The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.


```java
class Solution {
    public int maxProduct(int[] nums) {
        if(nums.length==1)return nums[0];
        int min = 1,max=1, ans=nums[0];
        for(int i:nums){
            // tmp-> so that we will not mistakely use updated max
            int tmp=max*i;
            max=Math.max(Math.max(tmp,min*i),i);
            min=Math.min(Math.min(tmp,min*i),i);
            ans=Math.max(ans,max);
        }
        return ans;
    }
}
```
</details>





<details id="139. Word Break">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">139. Word Break 
</span></summary>
Given a string s and a dictionary of strings wordDict, return true if s can be segmented into a space-separated sequence of one or more dictionary words.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

 

Example 1:

Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
Example 2:

Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
Example 3:

Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: false
 

Constraints:

1 <= s.length <= 300
1 <= wordDict.length <= 1000
1 <= wordDict[i].length <= 20
s and wordDict[i] consist of only lowercase English letters.
All the strings of wordDict are unique.


![Alt text](image-39.png)

![Alt text](image-40.png)

```java
// Top down DP solution
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] dp=new boolean[s.length()+1];
        dp[s.length()]=true;
        for(int i=s.length()-1;i>=0;i--){
            for(String word:wordDict){
                // s.startsWith(word,i)-> check for prefix word in the string s strating form index i
                if(word.length()+i<=s.length() && s.startsWith(word,i)){
                    dp[i]=dp[i+word.length()];
                }
                // Only 1 solution (true) is enough for every i
                if(dp[i])break;
            }
        }
        return dp[0];
    }
}
```
    Time:  O(s.length() * wordDict.size() * s.length())
</details>





<details id="300. Longest Increasing Subsequence">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">300. Longest Increasing Subsequence 
</span></summary>
Given an integer array nums, return the length of the longest strictly increasing 
subsequence
.

 

Example 1:

Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
Example 2:

Input: nums = [0,1,0,3,2,3]
Output: 4
Example 3:

Input: nums = [7,7,7,7,7,7,7]
Output: 1
 

Constraints:

1 <= nums.length <= 2500
-104 <= nums[i] <= 104
 

Follow up: Can you come up with an algorithm that runs in O(n log(n)) time complexity?

```java
class Solution {

    // Dynamic programming, O(n^2)
    public int lengthOfLIS(int[] nums) {
        if (nums.length == 1) return 1;

        int[] LIS = new int[nums.length];
        Arrays.fill(LIS, 1);
        int maximumSoFar = 1;

        for (int i = nums.length - 1; i >= 0; i--) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] < nums[j]) {
                    LIS[i] = Math.max(1 + LIS[j], LIS[i]);
                }
            }
            maximumSoFar = Math.max(maximumSoFar, LIS[i]);
        }
        return maximumSoFar;
    }

    // Binary search, O(nlogn)
    public int lengthOfLIS(int[] nums) {
        List<Integer> lis = new ArrayList<>(nums.length);

        for (int n : nums) {
            int i = Collections.binarySearch(lis, n);
            if (i < 0) i = -i - 1;

            if (i == lis.size())
                lis.add(n);
            else
                lis.set(i, n);
        }
        return lis.size();
    }
}
```
</details>


## <h1 style="text-align: center;">12. 2-D Dynamic Programming </h1>


<details id="62. Unique Paths">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">62. Unique Paths 
</span></summary>
There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to 2 * 109.

 

Example 1:

![Alt text](image-41.png)

Input: m = 3, n = 7
Output: 28
Example 2:

Input: m = 3, n = 2
Output: 3
Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
 

Constraints:

1 <= m, n <= 100

![Alt text](image-42.png)
Intution: the last row and column has only 1 path to reach the goal cell. For last row we can only go right and for last column we can only go down the cell. Hence assign all these cells to 1;
Now the number of paths to the goal cell from the current cell is the sum of poaths from the right cell and below cell.


```java
class Solution {

    // Dynamic programming: TC = O(m*n), SC = O(m*n)
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];

        // Fill out last row
        for (int j = 0; j < n; j++) {
            dp[m - 1][j] = 1;
        }

        // Fill out last column
        for (int i = 0; i < m; i++) {
            dp[i][n - 1] = 1;
        }

        for (int i = m - 2; i >= 0; i--) {
            for (int j = n - 2; j >= 0; j--) {
                dp[i][j] = dp[i][j + 1] + dp[i + 1][j];
            }
        }
        return dp[0][0];
    }

    // Dynamic programming: TC = O(m*n), SC = O(min(m,n))
    public int uniquePaths2(int m, int n) {
        if (m <= 0 || n <= 0) return 0;

        int[] dp = new int[n];
        for (int i = 0; i < n; i++)
            dp[i] = 1;

        for (int i = 1; i < m; i++)
            for (int j = 1; j < n; j++)
                dp[j] += dp[j - 1];

        return dp[n - 1];
    }

    // Combinatorics: TC = O(min(m,n)), SC = O(1)
    // result = C(m + n, n) = (m + n)! / (m! * n!)
    public int uniquePaths3(int m, int n) {
        if (m <= 0 || n <= 0) return 0;

        if (m < n) return uniquePaths3(n, m);

        m--;
        n--;
        long res = 1;
        for (int i = 1; i <= n; i++) {
            res *= (m + i);
            res /= i;
        }
        return (int)res;
    }
}
```
</details>





<details id="1143. Longest Common Subsequence">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">1143. Longest Common Subsequence 
</span></summary>
Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, "ace" is a subsequence of "abcde".
A common subsequence of two strings is a subsequence that is common to both strings.

 

Example 1:

Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
Example 2:

Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
Example 3:

Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
 

Constraints:

1 <= text1.length, text2.length <= 1000
text1 and text2 consist of only lowercase English characters.

![Alt text](image-43.png)



Intution: Focus on the iterative solution which is a bottom up solution.
Here we have assigned the last row and column to 0. We are starting from 
dp[ text1.length()-1][ text2.length()-1] diagonally updward to the corner tile. at every comparision we are determining the common subsequence b/w 2 strings where 1st string is from i----text1.length()-1 ans second string is from j----text2.length()-1



```java
// Iterative version (Bottom-up 2d dp)
class Solution {
    
    public int longestCommonSubsequence(String text1, String text2) {
        int[][] dp = new int[text1.length() + 1][text2.length() + 1];    

        for (int i = text1.length() - 1; i >= 0; i--) {
            for (int j = text2.length() - 1; j >= 0; j--) {
                if (text1.charAt(i) == text2.charAt(j)) {
                    // Increase the maximum sub-string length
                    dp[i][j] = 1 + dp[i + 1][j + 1];
                } else {
                    // Copy the maximum sub-string length so far
                    dp[i][j] = Math.max(dp[i][j + 1], dp[i + 1][j]);
                }
            }
        }

        return dp[0][0];
    }
}

// Iterative version
class Solution {
    
    public int longestCommonSubsequence(String text1, String text2) {
        int[][] dp = new int[text1.length() + 1][text2.length() + 1];    

        for (int i = text1.length() - 1; i >= 0; i--) {
            for (int j = text2.length() - 1; j >= 0; j--) {
                if (text1.charAt(i) == text2.charAt(j)) {
                    dp[i][j] = 1 + dp[i + 1][j + 1];
                } else {
                    dp[i][j] = Math.max(dp[i][j + 1], dp[i + 1][j]);
                }
            }
        }

        return dp[0][0];
    }
}
```
</details>

## <h1 style="text-align: center;">13. Greedy </h1>



<!-- <details id="Two Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Sample 
</span></summary>
This is the content that can be collapsed or expanded.
</details> -->





<!-- <details id="Two Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Sample 
</span></summary>
This is the content that can be collapsed or expanded.
</details> -->





<!-- <details id="Two Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Sample 
</span></summary>
This is the content that can be collapsed or expanded.
</details> -->





<!-- <details id="Two Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Sample 
</span></summary>
This is the content that can be collapsed or expanded.
</details> -->





<!-- <details id="Two Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Sample 
</span></summary>
This is the content that can be collapsed or expanded.
</details> -->





<!-- <details id="Two Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Sample 
</span></summary>
This is the content that can be collapsed or expanded.
</details> -->





<!-- <details id="Two Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Sample 
</span></summary>
This is the content that can be collapsed or expanded.
</details> -->






<!-- <details id="Two Sum">
<summary> 
<span style="color:yellow;font-size:16px;font-weight:bold">Sample 
</span></summary>
This is the content that can be collapsed or expanded.
</details> -->