#  Best Time to Buy and Sell Stock III

 Best Time to Buy and Sell Stock III  ( [lintcode](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock-iii/) )

```
Description
Say you have an array for which the ith element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. You may complete at most two transactions.

Notice
You may not engage in multiple transactions at the same time 
(ie, you must sell the stock before you buy again).

Example
Given an example [4,4,6,1,1,4,2,5], return 6.
```

### 解题思路

使用隔板法，将数组分为两部分，然后分别求一次交易的最大利润。考虑到题目要求是最多可以进行两次交易，也即可以进行一次交易，所以在求最终结果时，隔板两侧的数组可以有重合。

```java
class Solution {
    /**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    public int maxProfit(int[] prices) {
        // write your code here
        if (prices == null || prices.length < 2) {
            return 0;
        }
        
        int n = prices.length;
        int[] left = new int[n];
        int min = prices[0];
        left[0] = 0;
        for (int i = 1; i < n; i++) {
            left[i] = Math.max(left[i-1], prices[i] - min);
            min = Math.min(min, prices[i]);
        }
        
        int[] right = new int[n];
        int max = prices[n-1];
        right[n-1] = 0;
        for (int i = n-2; i >= 0; i--) {
            right[i] = Math.max(right[i+1], max - prices[i]);
            max = Math.max(max, prices[i]);
        }
        
        int ans = Integer.MIN_VALUE;
        for (int i = 0; i < n - 1; i++) {
            ans = Math.max(left[i] + right[i], ans);
        }
        
        return ans;
    }
};
```



