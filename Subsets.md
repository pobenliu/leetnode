# Subsets

Subsets (leetcode [lintcode](http://www.lintcode.com/en/problem/subsets/))

```
Given a set of distinct integers, return all possible subsets.

Notice
- Elements in a subset must be in non-descending order.
- The solution set must not contain duplicate subsets.

Example
If S = [1,2,3], a solution is:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

Challenge 
Can you do it in both recursively and iteratively?
```

### 解题思路

#### 一、DFS

要求一是子集中元素为升序，故先对原数组进行排序。要求二是子集不能重复，于是将原题转化为数学中的组合问题，使用深度优先搜索（`DFS`）进行穷举求解。

借用参考链接 1 中的解释，具体穷举过程可以用图示和函数运行的堆栈图理解，以数组 `[1, 2, 3]` 为例进行分析，下图所示为 `list` 及 `result` 动态变化的过程，箭头向下表示 `list.add` 及 `result.add` 操作，箭头向上表示`list.remove`操作。为了确保所有的情况都能够遍历到，在 `list` 加入一个元素后，还需要删除该元素以恢复原状态。

函数 `dfs(result, list, nums, 0)` 则表示将以 `list` 开头的所有组合全部加入 `result` 。当 `list` 是 `[1]` 时，对应图中步骤 2~7 ，依次将 `[1, 2], [1, 2, 3], [1, 3]` 全部添加到 `result` 中。

![](http://ww3.sinaimg.cn/mw690/600e6311jw1f8a8qisz8ij218g0xcn1y.jpg)

##### 算法复杂度

- 时间复杂度：在本题中解的个数为 `2^n`，产生一个解的复杂度最坏可以认为是 `n`，故算法时间复杂度的上界可以认为是 `O(n* 2^n)`。
- 空间复杂度：使用临时空间 `list` 保存中间结果，`list` 最大长度为数组长度，故空间复杂度近似为 `O(n)` 。

Java 实现：

```java
class Solution {
    /**
     * @param nums: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    public ArrayList<ArrayList<Integer>> subsets(int[] nums) {
        
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        // 注意：此处int型数组取长度无需括号()
      	if (nums == null || nums.length == 0) {
            result.add(new ArrayList<Integer>());  // when nums = [], return [[]]
            return result;
        }
        ArrayList<Integer> list = new ArrayList<Integer>();
        Arrays.sort(nums);
        
        dfs(result, list, nums, 0);
        return result;
    }
    
    private void dfs(ArrayList<ArrayList<Integer>> result,
                     ArrayList<Integer> list, int[] nums, int pos) {
        
        result.add(new ArrayList<Integer>(list));
        
        for (int i = pos; i < nums.length; i++) {
            list.add(nums[i]);
            dfs(result, list, nums, i + 1);
            list.remove(list.size() - 1);   //移除最后一个元素
        }
        
    }
}
```





#### 二、Bit Map

一共 `2^n` 个子集，每个子集对应 `0 … 2^n - 1` 之间的一个二进制整数，该整数一共 `n` 个 `bit` 位，用第`i`个 `bit` 位的取值 `1` 或 `0` 表示 `nums[i]` 在或不在集合中，我们只需遍历完所有的数字——对应所有的 `bit` 位可能性（外循环），然后转化为对应的数字集合——判断数字每一个 `bit` 的取值（内循环）——即可。

```
以nums = [1,2,3]举例
0 -> 000 -> null                    -> []
1 -> 001 -> nums[0]                 -> [1]
2 -> 010 -> nums[1]                 -> [2]
3 -> 011 -> nums[0],nums[1]         -> [1,2]
4 -> 100 -> nums[2]                 -> [3]
5 -> 101 -> nums[1],nums[2]         -> [1,3]
6 -> 110 -> nums[1],nums[2]         -> [2,3]
7 -> 111 -> nums[0],nums[1],nums[2] -> [1,2,3]
```



Java 实现：

```java
class Solution {
    /**
     * @param nums: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    public ArrayList<ArrayList<Integer>> subsets(int[] nums) {
        // write your code here
        if (nums == null || nums.length == 0) {
            return new ArrayList<ArrayList<Integer>>();
        }
        
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        int n =  nums.length;
        Arrays.sort(nums);  // 注意函数使用是Arrays.sort()
        // 1 << n = 100...0 , i的取值范围是0 ~ 2^n -1
        // i用来遍历所有的比特位可能性
        for (int i = 0; i < (1 << n); i++) {
            ArrayList<Integer> subset = new ArrayList<Integer>();
            for (int j = 0; j < n; j++) {
                // j的取值范围是0 ~ n
                // j用来判断n个bit位的取值是否为1
                if ((i & (1 << j)) != 0) {
                    subset.add(nums[j]);
                }
            } // for j 
            result.add(subset);
        } // for i
        return result;
    }
    
}
```

### 参考

1. [Subsets - 子集 | 数据结构与算法/leetcode/lintcode题解](http://algorithm.yuanbin.me/zh-hans/exhaustive_search/subsets.html#)
2. [Subsets | 九章算法](http://www.jiuzhang.com/solutions/subsets/)
3. [LeetCode: Subsets 解题报告 | Yu's garden](http://www.cnblogs.com/yuzhangcmu/p/4211815.html)