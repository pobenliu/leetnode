# Permutations II

Permutations II ( [leetcode](https://leetcode.com/problems/permutations-ii/)  [lintcode](http://www.lintcode.com/en/problem/permutations-ii/) )

```
Description
Given a list of numbers with duplicate number in it. Find all unique permutations.

Example
For numbers [1,2,2] the unique permutations are:
[
  [1,2,2],
  [2,1,2],
  [2,2,1]
]

Challenge 
Using recursion to do it is acceptable. If you can do it without recursion, that would be great!
```



### 解题思路

在 Permutation 题目的基础上，增加了对重复元素的处理，此处可参考 Subsets II 题目中对重复元素的处理。即，最终要达到的目的是，出现重复的数字，原来排在前面的，最终结果中也排在前面，没有顺序上的变化也就不会产生重复的子集。

由于重复元素的存在，且每次遍历都会从第一个元素开始，所以需要给每个数组元素增加一个标志位，表示是否已经加入到 `list` 中。两种情况需要剪枝，即略过不符合要求的结果。一是该元素已经在 `list` 中，通过标志位判断；二是该元素和前一个元素相等，但前一个元素不在 `list` 中，如果加入该元素会打乱重复元素的选取顺序。

Java 代码

```java
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of unique permutations.
     */
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (nums == null || nums.length == 0) {
            result.add(new ArrayList<Integer>()); 
            return result;
        }
        
        Arrays.sort(nums);
        ArrayList<Integer> sol = new ArrayList<Integer>();
        boolean[] visited = new boolean[nums.length];
        dfs(result, sol, visited, nums);
        
        return result;
    } 
    
    private void dfs(List<List<Integer>> result, 
                        ArrayList<Integer> sol, 
                        boolean[] visited,
                        int[] nums) {
        if (sol.size() == nums.length ) {
            result.add(new ArrayList<Integer>(sol));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            // 相同的数字，原来排在前面的，在结果中也应该排在前面，这样就保证了唯一性
            // 所以当前面的数字未使用时，后面的数字也不应被使用
            if (visited[i] || (i != 0 && nums[i] == nums[i - 1] && !visited[i - 1])) {
                continue;
            }
            visited[i] = true;
            sol.add(nums[i]);
            dfs(result, sol, visited, nums);
            sol.remove(sol.size() - 1);
            visited[i] = false;
        }
    }
}
```

### 参考

1. [Permutations II | 九章算法](http://www.jiuzhang.com/listutions/permutations-ii/)