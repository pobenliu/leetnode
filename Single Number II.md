#  Single Number II

 Single Number II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/single-number-ii/) )

```
Description
Given 3*n + 1 numbers, every numbers occurs triple times except one, find it.

Example
Given [1,1,2,3,3,3,2,2,4,1] return 4

Challenge 
One-pass, constant extra space.
```

### 解题思路

#### 一、位运算

可以参考 Single Number 的解决思路，采用位运算。由于所有数字都出现奇数次，所以无法直接使用异或操作。考虑到计算机使用二进制存储数字，可以建立一个32位的数字，统计每一位1出现的次数，如果一个整数出现了三次，那么三个0或者三个1对3取余都为0，对每个数的对应位都加起来对3取余，剩下的就是 Single Number 。

Java 实现

```java
public class Solution {
	/**
	 * @param A : An integer array
	 * @return : An integer 
	 */
    public int singleNumberII(int[] A) {
        if (A == null || A.length == 0) {
            return -1;
        }
        
        int result = 0;
        int[] bits = new int[32];
        for (int i = 0; i < 32; i++) {
            for (int j = 0; j < A.length; j++) {
                bits[i] += A[j] >> i & 1;
                bits[i] %= 3;
            }
            result |= (bits[i] << i);
        }
        
        return result;
    }
}
```



### 参考

1. [[LeetCode] Single Number II 单独的数字之二 | Grandyang](http://www.cnblogs.com/grandyang/p/4263927.html)

