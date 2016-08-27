# Permutations

Permutations ( leetcode [lintcode](http://www.lintcode.com/en/problem/permutations/))

```
Description
Given a list of numbers, return all possible permutations.

Notice
You can assume that there is no duplicate numbers in the list.

Example
For nums = [1,2,3], the permutations are:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

Challenge 
Do it without recursion.
```

### 解题思路

##### 一、递归（DFS）

Permutation 要求遍历得到所有长度为 `n` 的排列，与 Subsets 的实现相比较，有两点不一样的地方：一是只有在长度为 `n` 时才会记录；二是不再要求升序，所有排序都必须遍历到。

**算法复杂度分析：**

- 时间复杂度：

  以状态数来分析，全排列的个数为 `n!`，一个排列中的每个元素被遍历的次数为`(n - 1)!`（**这是为何？**），故一个排列所有 `n` 个元素被遍历的状态数一共为 `O(n * (n - 1)!) = O(n!)`，此为时间复杂度的下界，因为这里只算了合法条件下的遍历状态数。如果不对 `list` 中是否包含 `nums[i]` 进行检查，则总的状态数应为 `n^n` 种。

  由于最终每个排列的长度都为 `n` ，各排列的相同元素并不共享，故时间复杂度的下界为 `O(n * n!)`，上届为 `n * n^n` 。这里的时间复杂度并未考虑查找列表中是否包含重复元素的 `contain` 操作。

Java 实现

```java
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
    public ArrayList<ArrayList<Integer>> permute(ArrayList<Integer> nums) {
        // write your code here
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        if (nums == null || nums.size() == 0) {
            return result;
        }
        
        ArrayList<Integer> list = new ArrayList<Integer>();
        dfs(result, list, nums);
        return result;
    }
    
    private void dfs (ArrayList<ArrayList<Integer>> result,
                      ArrayList<Integer> list, 
                      ArrayList<Integer> nums) {
        if (list.size() == nums.size()) {
            result.add(new ArrayList<Integer>(list));
        }
        
        for (int i = 0; i < nums.size(); i++) {
            if (list.contains(nums.get(i))) {
                continue;
            }
            
            list.add(nums.get(i));
            dfs(result, list, nums);
            list.remove(list.size() - 1);
        }
    }
}

```





### 参考

1. [Permutations](http://algorithm.yuanbin.me/zh-hans/exhaustive_search/permutations.html)
2. [九章算法解答](http://www.jiuzhang.com/solutions/permutations/)
3. ​













