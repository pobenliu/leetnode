#  Trailing Zeros

 Trailing Zeros ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/trailing-zeros/) )

```
Description
Write an algorithm which computes the number of trailing zeros in n factorial.

Example
11! = 39916800, so the out should be 2

Challenge 
O(log N) time
```

### 解题思路

正整数 n 的阶乘有多少个尾随零，可以用以下公式求解：

![](http://ww1.sinaimg.cn/mw690/600e6311jw1f85xk6r7yzj20h802pdfw.jpg)

其中， `k` 需要满足 `5^(k+1) > n` 。

可以进一步简化为更容易实现的以下形式：

定义 `q_i = n / (5^i)` ，初始状态为 `q_0 = n`，迭代公式为 `q_i+1 = q_i / 5` 。

Java 实现

```java
class Solution {
    /*
     * param n: As desciption
     * return: An integer, denote the number of trailing zeros in n!
     */
    public long trailingZeros(long n) {
        if (n <= 0) {
            return 0;
        }
        
        long sum = 0;
        while (n != 0) {
            sum += n / 5;
            n /= 5;
        }
        return sum;
    }
}
```



### 参考

1.[Trailing Zeros | 九章算法](http://www.jiuzhang.com/solutions/trailing-zeros/)

2.[Trailing Zeros | Wikipedia](https://en.wikipedia.org/wiki/Trailing_zero)