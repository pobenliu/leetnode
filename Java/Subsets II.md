# Subsets II

Subsets II ( leetcode [lintcode](http://www.lintcode.com/en/problem/subsets-ii/) )

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



#### 解题思路

**一、递归解法（DFS）**

题目与Subsets有关，那么解题思路可以参考Subsets的解法，但是需要删除重复的子集。

思考哪些情况会出现重复。如`{1, 2(1), 2(2), 2(3)}`，`{1, 2(1)}`和`{1, 2(2)}`重复，`{1, 2(1), 2(2)}`和`{1, 2(2), 2(3)}`重复。其实，我们只关心取了几个`2`，不关心取哪几个。

实现方案：从第一个重复元素`2`开始连续取值。

Java实现：

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

