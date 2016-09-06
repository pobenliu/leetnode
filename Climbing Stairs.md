# Climbing Stairs

Climbing Stairs  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/climbing-stairs/) )

```
Description
You are climbing a stair case. It takes n steps to reach to the top.
Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Example
Given an example n=3 , 1+1+1=2+1=1+2=3
return 3
```



### 解题思路

#### 一、动态规划

按照正常的动态规划思路实现

Java 实现

```java
public class Solution {
    /**
     * @param n: An integer
     * @return: An integer
     */
    public int climbStairs(int n) {
        // write your code here
        if (n == 0) {
            return 1;
        }
        
        // state
        int[] ways = new int[n + 1];
        
        // initialize
        ways[0] = 1;
        ways[1] = 1;
        
        // top down
        for (int i = 2; i < n + 1; i++) {
            ways[i] = ways[i - 1] + ways[i - 2];
        }
        
        // end
        return ways[n];
    }
}
```

​	

#### 二、动态规划 II

第一个解法使用了长为 n 的数组，空间复杂度为 `O(n)` ，观察可得，只要保存当前位置的前两步的路径数量即可。此时空间复杂度为常数。

```java
public class Solution {
    /**
     * @param n: An integer
     * @return: An integer
     */
    public int climbStairs(int n) {
        // write your code here
        if (n <= 1) {
            return 1;
        }
        
        int last = 1, lastlast = 1;
        int now = 0;
        
        for (int i = 2; i <= n; i++) {
            now = last + lastlast;
            lastlast = last;
            last = now;
        }
        
        return now;
    }
}
```



### 参考

1. [Climbing Stairs | 九章算法](http://www.jiuzhang.com/solutions/climbing-stairs/)

