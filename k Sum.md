# k Sum

 k Sum  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/k-sum/) )

```
Description
Given n distinct positive integers, integer k (k <= n) and a number target.
Find k numbers where sum is target. Calculate how many solutions there are?

Example
Given [1,2,3,4], k = 2, target = 5.
There are 2 solutions: [1,4] and [2,3].
Return 2.
```

### 解题思路

#### 一、动态规划

1. 定义状态：由于问题涉及的自变量比较多，前 `i` 个数字，取其中 `j` 个，其和为 `t` 的方案数量，所以定义三维状态变量 `f[i][j][t]` ，完成状态定义是非常关键的一步，如果定义准确，问题基本已经得到解决了。
2. 定义状态转移函数：对于当前状态 `f[i][j][t]` ，详细讨论如下
   - 如果不计入 `A[i - 1]` ，则 `f[i][j][t]` 对应的上一个状态是 `f[i - 1][j][t]` ；
   - 如果计入 `A[i - 1]` ，需满足条件 `j >= A[i - 1]` ，那么 `f[i][j][t]` 上一个状态是  `f[i - 1][j][t]` 或 `f[i - 1][j - 1][t - A[i - 1]]` ；
3. 定义起点：
   - `f[i][0][0]` 从前 `i` 个数字，取其中 `0` 个，其和为 `0` 的方案数量为1。
   - `f[0][j][0], j != 0` 从前 `0` 个数字，取其中 `j` 个，其和为 `0` 的方案数量为0。
   - `f[0][0][t], t != 0` 从前 `0` 个数字，取其中 `0` 个，其和为 `t` 的方案数量为0。
4. 定义终点：最终结果即为 `f[n][k][target]` 。注：**初始化时也需要注意取值范围是数组长度加一。**

##### 算法复杂度

- 时间复杂度：`O(n*k*target)`
- 空间复杂度：`O(n*k*target)`

易错点：

> 1. 多重循环需确保自变量的取值范围不会造成数组越界，这里的越界包含两部分，一是自变量本身的左右边界，二是自变量之间的边界限定。如本题中变量 `j` 的起始点从 1 开始，因为内部循环出现了 `j - 1` 。

Java 实现

```java
public class Solution {
    /**
     * @param A: an integer array.
     * @param k: a positive integer (k <= length(A))
     * @param target: a integer
     * @return an integer
     */
    public int kSum(int A[], int k, int target) {
        // write your code here
        if (A == null || A.length == 0) {
            return 0;
        }
        
        int n = A.length;
        // state
        // f[i][j][t] take j numbers from the first i numbers, how many combinations' sum is t
        int[][][] f = new int[n + 1][k + 1][target + 1];
        
        // initialize
        for (int i = 0; i <= n; i++) {
            f[i][0][0] = 1;
        }
        
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= k && j <= i; j++) {
                for (int t = 1; t <= target; t++) {
                    f[i][j][t] = f[i - 1][j][t];
                    if (t >= A[i - 1]) {
                        f[i][j][t] = f[i - 1][j][t] + f[i - 1][j - 1][t - A[i - 1]];
                    }
                }
            }
        }
        
        return f[n][k][target];
    }
}
```

观察不难得到，`f[i]` 仅和 `f[i - 1]` 有关，所以可使用滚动数组进行优化，将空间复杂度降至 `O(k*target)`。

```java
public class Solution {
    /**
     * @param A: an integer array.
     * @param k: a positive integer (k <= length(A))
     * @param target: a integer
     * @return an integer
     */
    public int kSum(int A[], int k, int target) {
        if (A == null || A.length < k || k <= 0) {
            return 0;
        }
        
        int n = A.length;
        // status
        int[][][] f = new int[2][k + 1][target + 1];
        for (int i = 0; i < 2; i++) {
            f[i][0][0] = 1;
        }
        
        for (int i = 1; i <= n; i++) {
            f[i % 2][0][0] = 1;
            for (int j = 1; j <= k && j <= i; j++) {
                for (int m = 1; m <= target; m++) {
                    f[i % 2][j][m] = f[(i - 1)%2][j][m];
                    if (m >= A[i - 1]) {
                        f[i % 2][j][m] += f[(i - 1) % 2][j - 1][m - A[i - 1]];
                    }
                }
            }
        }
        
        return f[n % 2][k][target];
    }
}

```



#### 二、动态规划 II

考虑到输入是单序列，每次最多增加一个元素，使用二维数组定义状态函数。实现思路参考 Backpack 的解法二。

需要注意的是：循环的嵌套顺序、变量的遍历顺序都需要进行调整，目的是为了避免重复计算。具体细节思考的不是很清楚。

Java 实现

```java
public class Solution {
    /**
     * @param A: an integer array.
     * @param k: a positive integer (k <= length(A))
     * @param target: a integer
     * @return an integer
     */
    public int kSum(int A[], int k, int target) {
        // write your code here
        if (A == null || A.length == 0) {
            return 0;
        }
        
        int n = A.length;
        // state
        // f[i][j][t] indicates the number of solutions for selecting j numbers from A[0 … i – 1] with the sum of t.
        int[][] f = new int[k + 1][target + 1];
        
        // initialize
        f[0][0] = 1;
        
        for (int i = 1; i <= n; i++) {
            for (int t = target; t >= 0; t--) {
                for (int j = 1; j <= k && j <= i; j++) {
                    if (t >= A[i - 1]) {
                        f[j][t] = f[j][t] + f[j - 1][t - A[i - 1]];
                    }
                }
            }
        }
        
        return f[k][target];
    }
}
```



### 参考

1. [lintcode: k Sum 解题报告 | Yu's garden](http://www.cnblogs.com/yuzhangcmu/p/4279676.html)