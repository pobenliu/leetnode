# Subsets II

Subsets II ( [leetcode](https://leetcode.com/problems/subsets-ii/)  [lintcode](http://www.lintcode.com/en/problem/subsets-ii/) )

```
Description
Given a list of numbers that may has duplicate numbers, return all possible subsets

Notice
- Each element in a subset must be in non-descending order.
- The ordering between two subsets is free.
- The solution set must not contain duplicate subsets.

Example
If S = [1,2,2], a solution is:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

Challenge 
Can you do it in both recursively and iteratively?
```

### 解题思路

#### 一、DFS

题目与 Subsets 有关，解题思路可以参考 Subsets 的解法，但是需要考虑如何删除重复的子集。

以`{1, 2(1), 2(2), 2(3)}`为例，`{1, 2(1)}`和`{1, 2(2)}`重复，`{1, 2(1), 2(2)}`和`{1, 2(2), 2(3)}`重复。可以看到，对于重复元素，我们只关心取了几个，不关心取哪几个。

解决方案是从重复元素的第一个持续向下添加列表，而不能取第二个或之后的重复元素。换句话讲就是重复的元素必须连续选取。该方法很巧妙，建议使用图示以及函数运行的堆栈图理解。

算法复杂度

- 时间复杂度：在本题中解的个数为 `2^n`，产生一个解的复杂度最坏可以认为是 `n`，故算法时间复杂度的上界可以认为是 `O(n* 2^n)`。
- 空间复杂度：使用临时空间 `list` 保存中间结果，`list` 最大长度为数组长度，故空间复杂度近似为 `O(n)` 。

Java 实现：

```java
class Solution {
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    public ArrayList<ArrayList<Integer>> subsetsWithDup(ArrayList<Integer> S) {
        // write your code here
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        if (S == null || S.size() == 0) {
            return result;
        }
        
        Collections.sort(S);
        ArrayList<Integer> list = new ArrayList<Integer>();
        
        dfs(result, list, S, 0);
        return result;
    }
    
    private void dfs(ArrayList<ArrayList<Integer>> result,
                     ArrayList<Integer> list, ArrayList<Integer> S, int pos) {
        
        result.add(new ArrayList(list));
        
        for (int i = pos; i < S.size(); i++) {
            // 遇到重复的元素（已排序），必须从第一个开始连续取
            // 使用i-1是为了防止索引越界
            if (i != pos && S.get(i) == S.get(i - 1)) {
                continue;
            }
            list.add(S.get(i));
            dfs(result, list, S, i + 1);
            list.remove(list.size() - 1);
        }
        
    }
}
```

### 参考

1. [Unique Subsets | 数据结构与算法/leetcode/lintcode题解](http://algorithm.yuanbin.me/zh-hans/exhaustive_search/unique_subsets.html)
2. [Subsets II | 九章算法](http://www.jiuzhang.com/solutions/subsets-ii/)