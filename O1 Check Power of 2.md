#  O(1) Check Power of 2

 O(1) Check Power of 2  ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/o1-check-power-of-2/) )

```
Description
Using O(1) time to check whether an integer n is a power of 2.

Example
For n=4, return true;
For n=5, return false;

Challenge 
O(1) time
```

### 解题思路

如果一个数 `n` 是 `2` 的整数幂，那么以二进制表示的话只有一位是 `1` ，其余每一位都是 `0` ，而 `n - 1` 则全是 `1` ，且比 `n` 少一位最高位，两者相与为零。

以 `n = 4` 为例

```
n 	: 4 ---> 0100
n-1 : 3 ---> 0011
```



Java 实现

```java
class Solution {
    /*
     * @param n: An integer
     * @return: True or false
     */
    public boolean checkPowerOf2(int n) {
        if (n <= 0) {
            return false;
        }
        
        return (n & (n - 1)) == 0;
    }
};
```

### 参考

1. [O(1) Check Power of 2 | 数据结构与算法/leetcode/lintcode题解](https://algorithm.yuanbin.me/zh-hans/math_and_bit_manipulation/o1_check_power_of_2.html)

