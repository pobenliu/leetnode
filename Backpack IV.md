# Backpack IV

Backpack IV  ()

```
Description
Given n items with size nums[i] which an integer array and all positive numbers, no duplicates. 
An integer target denotes the size of a backpack. 
Find the number of possible fill the backpack.
Each item may be chosen unlimited number of times

Example
Given candidate items [2,3,6,7] and target 7,

A solution set is:
[7]
[2, 2, 3]
return 2
```

### 解题思路

动态规划。

1. 定义状态：定义 `f[j]` 为前 `i` 个数填满大小为 `j` 的背包的方法数。
2. 定义状态转移函数：对于当前状态 `f[j]` ，
   - 如果不计入 `A[i]` ，则 `f[j]` 保持状态不变；
   - 如果计入 `A[i]` ，需满足条件 `j > A[i]` ，那么 `f[j]` 上一个状态是 `f[j - A[i]]` 
   - 对于当前物品 `i`，若 `j` 从小到大的话，很可能在 `j` 之前的 `j - A[i]` 时已经放过第 `i` 件物品了，在 `j` 时再放就是重复放入；若 `j` 从大到小，则 `j` 之前的所有情况都没有更新过，不可能放过第 `i` 件物品，所以不会重复放入。【DH：想得不是很明白...】
3. 定义起点：
   - `f[0]` 状态为 `1` ，这是根据问题的含义来定义的。
4. 定义终点：最终结果即为 `f[m]` 。

##### 易错点

> 1. 初始状态的定义，注意题目的具体含义。

Java 实现

```java
public class Solution {
    /**
     * @param nums an integer array and all positive numbers, no duplicates
     * @param target an integer
     * @return an integer
     */
    public int backPackIV(int[] A, int m) {
		if (A == null || A.length == 0 || m <= 0) {
  			return 0;		
  		}
        int n = A.length;
        // status
        int[] f = new int[m + 1];
        // initialize
        f[0] = 1;

        for(int i = 0; i < n; i++){
            for(int j = A[i]; j <= m; j++){
                f[j] += f[j - A[i]];
            }
        }

        return f[m];
    }
}
```



### 参考

1. [Backpack IV 562 | LintCode题解](https://zhengyang2015.gitbooks.io/lintcode/content/backpack_iv_562.html)

