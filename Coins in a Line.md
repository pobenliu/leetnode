# Coins in a Line

 Coins in a Line  ( [leetcode]()  [lintcode]() )

```
Description
There are n coins in a line. 
Two players take turns to take one or two coins from right side until there are no more coins left. 
The player who take the last coin wins.

Could you please decide the first play will win or lose?

Example
n = 1, return true.
n = 2, return true.
n = 3, return false.
n = 4, return true.
n = 5, return true.

Challenge 
O(n) time and O(1) memory
```

### 解题思路

#### 一、动态规划（记忆化搜索）

1. 定义状态： `f[i]` 为现在还剩 `i` 个硬币，先手取硬币最终的输赢状况。
2. 定义状态转移函数： 可参考以下图示，在还剩 `i` 个硬币的时候，先手有两种取法，取 1 个或者取 2 个硬币，在这两种情况，后手也分别有两种取法，取 1 个或者取 2 个，所以在轮到先手下一次取硬币时，一共有四种情况，只有当情况 a 和 b 一定能赢，或者 c 和 d 一定能赢时，先手才能获得胜利，对应状态转移为 `f[i] = (f[n - 2] && f[n - 3]) || (f[n - 3]&&f[n - 4])`。
3. 定义起点：初始化状态转移函数无法涉及的状态 `f[1] = true, f[2] = true, f[3] = false, f[4] = true` 。
4. 定义终点：为 `f[n]`。

![](http://ww2.sinaimg.cn/mw690/600e6311jw1f9cgghur63j20gi0aemy3.jpg)

Java 实现

```java
public class Solution {
    /**
     * @param n: an integer
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int n) {
        int[] dp = new int[n + 1];
        
        return MemorySearch(n, dp);
    }
    
    private boolean MemorySearch (int n, int[] dp) {  // 0 is empty, 1 is false, 2 is true
        if (dp[n] != 0) {
            if (dp[n] == 1) {
                return false;
            } else {
                return true;
            }
        }
        
        if (n <= 0) {
            dp[n] = 1;
        } else if (n == 1) {
            dp[n] = 2;
        } else if (n == 2) {
            dp[n] = 2;
        } else if (n == 3) {
            dp[n] = 1;
        } else {
            if ((MemorySearch(n - 2, dp) && MemorySearch(n - 3, dp)) || 
                (MemorySearch(n - 3, dp) && MemorySearch(n - 4, dp) )) {
                dp[n] = 2;        
            } else {
                dp[n] = 1;
            }
        }
        
        if (dp[n] == 2) {
            return true;
        }
        return false;
    }
}
```



#### 二、数学推理

可以通过观察找规律，只有 `n` 为 3 的倍数时，先手会输。

```java
public class Solution {
    /**
     * @param n: an integer
     * @return: a boolean which equals to true if the first player will win
     */
    public boolean firstWillWin(int n) {
        if (n <= 0) {
            return false;
        }
        
        if (n % 3 == 0) {
            return false;
        }
        return true;
    }

}
```



### 参考

1. [Coins in a Line | 九章算法](http://www.jiuzhang.com/solutions/coins-in-a-line/)
2. [Coins in a Line | lintcode题解](https://lefttree.gitbooks.io/leetcode/content/dynamicProgramming2/coinsInLine.html)