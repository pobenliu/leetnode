#  Best Time to Buy and Sell Stock II

 Best Time to Buy and Sell Stock II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock-ii/) )

```
Description
Say you have an array for which the ith element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. 
You may complete as many transactions as you like 
(ie, buy one and sell one share of the stock multiple times). 
However, you may not engage in multiple transactions at the same time 
(ie, you must sell the stock before you buy again).

Example
Given an example [2,1,2,0,1], return 2
```

### 解题思路

#### 一、动态规划

1. 定义状态： `f[i]` 第 `i` 天卖出股票能获得的最大收益。
2. 定义状态转移函数： 如果第 `i` 天卖出，那么可以是第 `j + 1, j < i` 天买入，这样 `f[i]` 对应的上一个状态是 `f[j]`，`f[i] = MAX{f[j] + prices[i-1] - prices[j]}`。
3. 定义起点：`f[0] = 0`。
4. 定义终点：为 `f[n]`。

Java 实现

```java
class Solution {
    /**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length < 2) {
            return 0;
        }
        
        int n = prices.length;
        // status
        int[] f = new int[n + 1];
        // initialize
        f[0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                f[i] = Math.max(f[i], f[j] + prices[i-1] - prices[j]);
            }
        }
        
        return f[n];
    }
}
```



#### 二、贪心

在方法一中，会出现较多的重复计算。从另外一个角度考虑，如果可以无限多次的交易，那么只要第二天的价格比第一天的价格高，就可以进行交易。

Java 实现

```java
class Solution {
    /**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length < 2) {
            return 0;
        }
        
        int n = prices.length;
        int profit = 0;
        for (int i = 1; i < n; i++) {
            int diff = prices[i] - prices[i - 1];
            if (diff > 0) {
                profit += diff;
            }
        }
        
        return profit;
    }
}
```



### 参考

1. [Best Time to Buy and Sell Stock II leetcode java | 爱做饭的小莹子](http://www.cnblogs.com/springfor/p/3877065.html)