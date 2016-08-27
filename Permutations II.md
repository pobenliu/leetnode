# Permutations II

Permutations II ( leetcode [lintcode](http://www.lintcode.com/en/problem/permutations-ii/))

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

#### 一、递归法

在 Permutation 题目的基础上，增加了对重复元素的处理，此处可参考 unique subsets 题目中对重复元素的处理。即，最终要达到的目的是，出现重复的数字，原来排在前面的，最终结果中也排在前面，没有顺序上的变化也就不会产生重复的子集。



Java 代码

```java
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of unique permutations.
     */
    public ArrayList<ArrayList<Integer>> permuteUnique(ArrayList<Integer> nums) {
        // write your code here
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        // ArrayList 对象取长度是  nums.size()
        if (nums == null || nums.size() == 0) {
            return result;
        }
        
        ArrayList<Integer> list = new ArrayList<Integer>();
        int[] visited = new int[nums.size()];
        
        Collections.sort(nums);
        dfs(result, list, visited, nums);
        return result;
    }
    
    public void dfs(ArrayList<ArrayList<Integer>> result,
                    ArrayList<Integer> list,
                    int[] visited,
                    ArrayList<Integer> nums) {
        if (list.size() == nums.size()) {
            result.add(new ArrayList<Integer>(list));
            return;
        }
        
        for (int i = 0; i < nums.size(); i++) {
            // 相同的数字，原来排在前面的，在结果中也应该排在前面，这样就保证了唯一性
            // 所以当前面的数字未使用时，后面的数字也不应被使用
            if (visited[i] == 1 || (i != 0 && nums.get(i) == nums.get(i - 1) && visited[i - 1] == 0)) {
                continue;
            }
            visited[i] = 1;
            list.add(nums.get(i));
            dfs(result, list, visited, nums);
            list.remove(list.size() - 1);
            visited[i] = 0;
        }
    }
}

```















