#  Fast Power

 Fast Power  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/fast-power/) )

```
Description
Calculate the a^n % b where a, b and n are all 32bit integers.

Example
For 231 % 3 = 2
For 1001000 % 1000 = 0

Challenge 
O(logn)
```

### 解题思路

本题目主要考察快速幂的实现，以及 int 类型数据溢出的处理。

快速幂实现的思路是 `a ^ n = (a ^ n/2) * (a ^ n/2)` ，这里需要注意的是当 `n` 为奇数时需要再多乘一个 `a` 。在具体实现时，对于 `n == 0` 和 `n == 1` 这两种情况也需要单独处理。

题目中提到 `a` 和 `b` 都是 `32` 位整数，所以在计算过程中中间变量使用 `long` 类型存储以免溢出。另，在本体中假定 `a` 和 `b` 都是正数。 

本题目还用到了整数取模操作的以下特点：`(a * b) % p =((a % p) * (b % p)) % p` 。

**算法复杂度**

- 时间复杂度：`O(logn)` 。

Java 实现

```java
class Solution {
    /*
     * @param a, b, n: 32bit integers
     * @return: An integer
     */
    public int fastPower(int a, int b, int n) {
        if (n == 1) {
            return a % b;
        }
        if (n == 0) {
            return 1 % b;
        }
        
        long product = fastPower(a, b, n / 2);
        product = (product * product) % b;
        if (n % 2 == 1) {
            product = (product * a) % b;
        }
        
        return (int) product;
    }
    
};
```



### 参考

1. [Lintcode: Fast Power 解题报告 | Yu's garden](http://www.cnblogs.com/yuzhangcmu/p/4174781.html)

