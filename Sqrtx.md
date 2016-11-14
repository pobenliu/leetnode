# Sqrt(x)

 Sqrt(x)  ( [lintcode](http://www.lintcode.com/en/problem/sqrtx/) )

```
Description
Implement int sqrt(int x).
Compute and return the square root of x.

Example
sqrt(3) = 1
sqrt(4) = 2
sqrt(5) = 2
sqrt(10) = 3

Challenge 
O(log(x))
```

### 解题思路

#### 一、二分法

将该题转化为寻找第一个平方不大于目标值 `x` 的数，即可使用二分法。为了防止溢出，需使用 `long` 类型作为中间变量。

Java 实现

```java
class Solution {
    /**
     * @param x: An integer
     * @return: The sqrt of x
     */
    public int sqrt(int x) {
        if (x < 0) {
            return -1;
        }
        if (x == 0) {
            return 0;
        }
        // find the last number which square of it <= x
        long start = 1, end = x;
        while (start + 1 < end) {
            long mid = start + (end - start) / 2;
            if (mid * mid <= x) {
                start = mid;
            } else {
                end = mid;
            }
        }
        
        if (end * end <= x) {
            return (int) end;
        }
        return (int) start;
    }
}
```

#### 二、牛顿法

牛顿法偏数学计算，是一种在实数域/复数域近似求解方程的方法，使用函数 `f(x)` 的泰勒级数的前面几项寻找方程 `f(x) = 0` 的根，具体原理不在此详述，感兴趣同学可以查阅参考链接。这里直接给出求解的迭代公式，其中 `f'(x)` 是函数 `f(x)` 的导数。

```
x_(n+1) = x_n - f(x_n)/f'(x_n)
```

具体到本题目中，对应函数为 `f(y)` （考虑到 `x` 是常数，用 `y` 作自变量）， 有如下推导：

```
y_(n+1) = y_n - f(y_n) / f'(y_n) 
        = y_n - ((y_n)^2 - x)/2y_n 
        = ((y_n)^2 + x)/2y_n 
        = (y_n + x/y_n) / 2
```

 

Java 实现

```java
class Solution {
    /**
     * @param x: An integer
     * @return: The sqrt of x
     */
    public int sqrt(int x) {
        if (x == 0) {
            return 0;
        }
        
        double lastY = 0;
        double y = 1;
        while (y != lastY) {
            lastY = y;
            y = (y + x / y) / 2;
        }
        
        return (int) y;
    }
}
```



### 参考

1. [Sqrt(x) | 九章算法](http://www.jiuzhang.com/solutions/sqrtx/)
2. [Sqrt(x) — LeetCode | Code Ganker](http://blog.csdn.net/linhuanmars/article/details/20089131)
3. [牛顿法 | 维基百科](https://zh.wikipedia.org/wiki/%E7%89%9B%E9%A1%BF%E6%B3%95)