#  House Robber

 House Robber  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/house-robber/) )

```
Description
You are a professional robber planning to rob houses along a street. 
Each house has a certain amount of money stashed, 
the only constraint stopping you from robbing each of them is that 
adjacent houses have security system connected and 
it will automatically contact the police 
if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, 
determine the maximum amount of money you can rob tonight without alerting the police.
```



### 解题思路

求解最大值，使用动态规划。

1. 定义状态：可定义 `f[i]` 为抢劫前 `i` 个房子可得到的最大值（最多的钱）。
2. 定义状态转移函数：对于当前状态 `f[i]` ，如果不抢劫第 `i` 个房子，那么上一个状态对应 `f[i - 1]` ；如果抢劫第 `i` 个房子，那么不能抢第 `i - 1` 个房子，上一个状态对应 `f[i - 2]`。
3. 定义起点：初始化状态转移方程无法计算的情况。
4. 定义终点：最终结果 `f[n]`。

Java 实现

```java
public class Solution {
    /**
     * @param A: An array of non-negative integers.
     * return: The maximum amount of money you can rob tonight
     */
    public long houseRobber(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        
        int n = A.length;
        // status
        long[] f = new long[n + 1];
        // initialize
        f[0] = 0;
        f[1] = A[0];
        
        for (int i = 2; i <= n; i++) {
            f[i] = Math.max(f[i - 1], f[i - 2] + A[i - 1]);
        }
        
        return f[n];
    }
}
```

空间优化（滚动数组）：观察状态转移方程，可以发现 `f[i]` 只依赖于 `f[i - 1]` 和 `f[i - 2]` ，所以大小为 3 的数组就够了。然后通过取模来不断更新数组，求得结果。

Java 实现

```java
public class Solution {
    /**
     * @param A: An array of non-negative integers.
     * return: The maximum amount of money you can rob tonight
     */
    public long houseRobber(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        
        int n = A.length;
        // status
        long[] f = new long[3];
        // initialize
        f[0] = 0;
        f[1] = A[0];
        
        for (int i = 2; i <= n; i++) {
            f[i % 2] = Math.max(f[(i - 1) % 2], f[(i - 2) % 2] + A[i - 1]);
        }
        
        return f[n % 2];
    }
}
```

