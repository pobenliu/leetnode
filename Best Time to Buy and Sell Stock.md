#  Best Time to Buy and Sell Stock

 Best Time to Buy and Sell Stock ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock/) )

```
Description
Say you have an array for which the ith element is the price of a given stock on day i.
If you were only permitted to complete at most one transaction 
(ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Example
Given array [3,2,3,1,2], return 1.
```

### 解题思路

记录当前的最大利润，以及最小买入值。

```java
public class Solution {
    /**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length < 2) {
            return 0;
        }
        
        int profit = 0;
        int min = prices[0];
        for (int i = 1; i < prices.length; i++) {
            profit = Math.max(profit, prices[i] - min);
            min = Math.min(min, prices[i]);
        }
        return profit;
    }
}
```

