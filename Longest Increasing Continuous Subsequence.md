#  Longest Increasing Continuous Subsequence

 Longest Increasing Continuous Subsequence  ( [leetcode]()  [lintcode] )

```
Description
Give an integer array，find the longest increasing continuous subsequence in this array.

An increasing continuous subsequence:
- Can be from right to left or from left to right.
- Indices of the integers in the subsequence should be continuous.

Notice
O(n) time and O(1) extra space.
```

### 解题思路

使用动态规划来考虑本题目，需要从左到右、从右到左遍历两次数组。

易错点：

> 1. `maxLen` 的初始值是 1，因为最少有一个数字，长度为 1。

Java 实现

```java
public class Solution {
    /**
     * @param A an array of Integer
     * @return  an integer
     */
    public int longestIncreasingContinuousSubsequence(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        
        // status
        int[] f = new int[A.length + 1];
        f[0] = 0;
        f[1] = 1;
        
        int maxLen = 1;
        for (int i = 2; i <= A.length; i++) {
            if (A[i - 1] > A[i - 2]) {
                f[i] = f[i - 1] + 1;
            } else {
                f[i] = 1;
            }
            maxLen = Math.max(maxLen, f[i]);
        }
        
        f[A.length] = 1;
        for (int i = A.length; i >= 2; i--) {
            if (A[i - 2] > A[i - 1]) {
                f[i - 1] = f[i] + 1;
            } else {
                f[i - 1] = 1;
            }
            maxLen = Math.max(maxLen, f[i - 1]);
        }
        
        return maxLen;
    }
}
```

考虑到状态 `f[i]` 只与 `f[i - 1]` 有关，使用滚动数组优化空间，有如下实现：

```java
public class Solution {
    /**
     * @param A an array of Integer
     * @return  an integer
     */
    public int longestIncreasingContinuousSubsequence(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        
        int n = A.length;
        int length = 1;
        int maxLen = 1;
        for (int i = 1; i < A.length; i++) {
            if (A[i] > A[i - 1]) {
                length++;
            } else {
                length = 1;
            }
            maxLen = Math.max(maxLen, length);
        }
        
        length = 1;
        for (int i = A.length - 2; i >= 0; i--) {
            if (A[i] > A[i + 1]) {
                length++;
            } else {
                length = 1;
            }
            maxLen = Math.max(maxLen, length);
        }
        
        return maxLen;
    }
}
```



