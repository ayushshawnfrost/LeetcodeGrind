# Cheetsheet

## 1. Int array to ArrayList



## 2. To print all the subset (not permutations) of a String/Array of all length
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
