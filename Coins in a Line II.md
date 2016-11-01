#  Coins in a Line II

 Coins in a Line II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/coins-in-a-line-ii/) )

```
Description
There are n coins with different value in a line. 
Two players take turns to take one or two coins from left side until there are no more coins left. 
The player who take the coins with the most value wins.

Could you please decide the first player will win or lose?
```

### 解题思路

#### 一、记忆化搜索

1. 定义状态： `f[i]` 为现在还剩 `i` 个硬币，先手取硬币最终可得的最大价值。
2. 定义状态转移函数： 依旧参考图示，在先手取过硬币之后，后手的策略一定是让先手取得尽可能少的硬币值，所以对应的，先手下一次取硬币面临的状态一定是 `a` 和 `b` 之间的最小值，或 `c` 和 `d` 之间的最小值。此时回过头来考虑先手在剩 `i` 个硬币时，选取策略是最大化硬币价值。所以对应状态转移方程为 `f[i] = Max{Min{f[i - 2], f[i - 3]} + values[n - i], Min{f[i - 3], f[i - 4]} + values[n - i] + values[n - i + 1]}`。
3. 定义起点：初始化状态转移函数无法涉及的状态`f[1], f[2], f[3], f[4]` 。
4. 定义终点：为 `f[n]`，当先手取得的最大值比所有硬币总价值的一半大时胜利。

![](http://ww2.sinaimg.cn/mw690/600e6311jw1f9cgghur63j20gi0aemy3.jpg)

具体实现过程使用了记忆化搜索的方法。

Java 实现

```java
public class Solution {
    /**
     * @param values: an array of integers
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int[] values) {
        int[] dp = new int[values.length + 1];
        boolean[] flag = new boolean[values.length + 1];
        
        int sum = 0;
        for (int now : values) {
            sum += now;
        }
        return sum < 2 * MemorySearch(values.length, dp, flag, values);
    }
    
    private int MemorySearch (int n, int[] dp, boolean[] flag, int[] values) {
        if (flag[n] == true) {
            return dp[n];
        }
        flag[n] = true;
        if (n == 0) {
            dp[n] = 0;
        } else if (n == 1) {
            dp[n] = values[values.length - 1];
        } else if (n == 2) {
            dp[n] = values[values.length - 1] + values[values.length - 2];
        } else if (n == 3) {
            dp[n] = values[values.length - 2] + values[values.length - 3];
        } else {
            dp[n] = Math.max(
                Math.min(MemorySearch(n-2, dp, flag, values), MemorySearch(n-3, dp, flag, values)) + values[values.length-n],
                Math.min(MemorySearch(n-3, dp, flag, values), MemorySearch(n-4, dp, flag, values)) + values[values.length-n] + values[values.length - n + 1]
                );
        }
        
        return dp[n];
    }
}
```



#### 二、动规

记忆化搜索实现起来比较麻烦，在网上看到另一种解法要简便很多。关键点在于状态的定义。

1. 定义状态： `f[i]` 表示先手从第 `i` 个硬币开始取，直到硬币取完，可得的最大价值。
2. 定义状态转移函数： 当先手开始准备取第 `i` 个硬币时，有两种方法
   - 取一个硬币 `values[i]`。那么对手接着可以取一个 `values[i + 1]` 或者两个 `values[i + 1] + values[i + 2]`，那么先手下一次取硬币面临的是 `f[i + 2]` 或 `f[i + 3]` ，而且是两者之中的最小值。
   - 取两个硬币 `values[i] + values[i + 1]`。同理对手接着可以取一个或两个，先手下一次取硬币面临的是 `f[i + 3]` 或 `f[i + 4]` 中的最小值。
   - 所以对应状态转移方程为 `f[i] = Max{Min{f[i + 2], f[i + 3]} + values[i], Min{f[i + 3], f[i + 4]} + values[i] + values[i + 1]}`。
3. 定义起点：初始化状态转移函数无法涉及的状态`f[n], f[n - 1], f[n - 2], f[n - 3]` 。
4. 定义终点：为 `f[0]`，当先手取得价值比后手多时胜利。

Java 实现

```java
public class Solution {
    /**
     * @param values: an array of integers
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int[] values) {
        int len = values.length;
        if (len < 3) {
            return true;
        }
        // status
        int[] f = new int[len + 1];
        // initialize
        f[len] = 0;
        f[len - 1] = values[len - 1];
        f[len - 2] = values[len - 1] + values[len - 2];
        f[len - 3] = values[len - 2] + values[len - 3];
        for (int i = len - 4; i >= 0; i--) {
            f[i] = Math.max(
                Math.min(f[i + 2], f[i + 3]) + values[i],
                Math.min(f[i + 3], f[i + 4]) + values[i] + values[i + 1]
                );
        }
        
        int sum = 0;
        for (int i : values) {
            sum += i;
        }
        
        return f[0] > sum - f[0];
    }
    
}
```



### 参考

1. [Coins in a Line II | 九章算法](http://www.jiuzhang.com/solutions/coins-in-a-line-ii/)
2. [lintcode ：Coins in Line II 硬币排成线 II | 水滴失船](http://www.cnblogs.com/theskulls/p/4963317.html) 
3. [Coins in a Line II | lintcode题解](https://lefttree.gitbooks.io/leetcode/content/dynamicProgramming2/coinsInLine2.html)