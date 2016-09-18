#  Single Number

 Single Number  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/single-number/) )

 ```
Description
Given 2*n + 1 numbers, every numbers occurs twice except one, find it.

Example
Given [1,2,2,1,3,4,3], return 4

Challenge 
One-pass, constant extra space.
 ```



### 解题思路

使用位操作，任意两个相同的数做异或运算（不进位加法），结果为 0，也即 `A ^ A = 0` ，而 `A ^ 0 = A` 。

Java 实现

```java
public class Solution {
    /**
      *@param A : an integer array
      *return : a integer 
      */
    public int singleNumber(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        
        int res = A[0];
        for (int i = 1; i < A.length; i++) {
            res = res ^ A[i];
        }
        
        return res;
    }
}
```





