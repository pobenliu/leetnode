#  Best Time to Buy and Sell Stock IV

 Best Time to Buy and Sell Stock IV  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock-iv/) )

```
Description
Say you have an array for which the ith element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. You may complete at most k transactions.

Notice
You may not engage in multiple transactions at the same time 
(i.e., you must sell the stock before you buy again).

Example
Given prices = [4,4,6,1,1,4,2,5], and k = 2, return 6.
```
 
### 解题思路

本题使用动态规划求解，关键在于如何定义状态。

#### 一、常规解法

1. 定义状态：定义二维状态变量 `f[i][j]` 为前 `i` 天至多进行 `j` 次交易，能够获得的最大收益。
2. 定义状态转移函数：对于 `f[i][j]` 上一个状态可能是 `f[x][j - 1], 0 < x < i`，第 `j` 次交易的利润为 `profit(x + 1, i)`，那么状态转移函数为 `f[i][j] = max{f[x][j - 1] + profit(x + 1, i)}, 0 < x < i`。
3. 定义起点：`f[i][0] = 0, f[0][j] = Integer.MIN_VALUE` 。
4. 定义终点：最终结果为 `f[n][k]`。

##### 算法复杂度

- 时间复杂度：上述状态定义下，需要三重循环 `i:0~n, j:0~k, x:0~(i-1)`，所以是 `O((n^2)*k)`。
- 空间复杂度：`O(nk)`。

#### 二、优化时间解法

常规解法中定义的状态可以视为全局变量，可以增加一个变量保存局部变量，这样不必每次都对之前的状态进行 遍历。

1. 定义状态：
   - `mustsell[i][j]` 表示前 `i` 天，至多进行 `j` 次交易，第 `i` 天必须卖出，获得的最大收益。
   - `globalbest[i][j]` 表示前 `i` 天，至多进行 `j` 次交易，第 `i` 天可以不卖出，获得的最大收益。
2. 定义状态转移函数：
   - 定义 `gain` 为第 `i - 1` 天买入，第 `i` 天卖出的收益，`gain = prices[i] - prices[i - 1]`。
   - `mustsell[i][j] = Max{globalbest[i - 1][j - 1] + gain, mustsell[i - 1][j] + gain}`。
     - 其中 `globalbest[i - 1][j - 1] + gain = a. mustsell[i - 1][j - 1] + gain; b. mustsell[x][j - 1] + gain, x < i - 1`。
     - 对于情况 `a`，相当于 `i - 1` 天卖出，又买入，也就是把卖出拖延到第 `i` 天。所以可以是 `mustsell[i][j]` 的最优解，题目要求是至多交易 `k` 次，符合题意。
     - 对于情况 `b`，本来其实要 `x` 遍历一遍 `0 ~ i-1` 找到 `mustsell[x][j - 1]` 的最优解。考虑到 `globalbest[i][j]` 已保存之前所有的最优解，所以时间得到优化。
   - `globalbest[i][j] = Max{globalbest[i - 1][j], mustsell[i][j]}`。
3. 定义起点：`mustsell[0][i] = globalbest[0][i] = 0` 。
4. 定义终点：`globalbest[n - 1][k]`。

Java 实现

```java
class Solution {
    /**
     * @param k: An integer
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    public int maxProfit(int k, int[] prices) {
        if (prices == null || prices.length == 0 || k == 0) {
            return 0;
        }
        
        if (k >= prices.length / 2) {
            int profit = 0;
            for (int i = 1; i < prices.length; i++) {
                if (prices[i] > prices[i - 1]) {
                    profit += prices[i] - prices[i - 1];
                }
            }
            return profit;
        }
        
        int n = prices.length;
        // mustsell[i][j] 表示前i天，至多进行j次交易，第i天必须sell的最大获益 
        int[][] mustsell = new int[n + 1][k + 1];
        // globalbest[i][j] 表示前i天，至多进行j次交易，第i天可以不sell的最大获益
        int[][] globalbest = new int[n + 1][k + 1];
        
        mustsell[0][0] = globalbest[0][0] = 0;
        for (int i = 1; i <= k; i++) {
            mustsell[0][i] = globalbest[0][i] = 0;
        }
        
        for (int i = 1; i < n; i++) {
            int gain = prices[i] - prices[i - 1];
            mustsell[i][0] = 0;
            for (int j = 1; j <= k; j++) {
                mustsell[i][j] = Math.max(globalbest[i - 1][j - 1] + gain,
                                            mustsell[i - 1][j] + gain);
                globalbest[i][j] = Math.max(globalbest[i - 1][j], mustsell[i][j]);
            }
        }
        return globalbest[n - 1][k];
    }
};
```



### 参考

1. [Best Time to Buy and Sell Stock IV | 九章算法](http://www.jiuzhang.com/solutions/best-time-to-buy-and-sell-stock-iv/)