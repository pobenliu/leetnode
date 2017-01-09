#  Count 1 in Binary

 Count 1 in Binary   ( [lintcode](http://www.lintcode.com/en/problem/count-1-in-binary/) )

```
Description
Count how many 1 in binary representation of a 32-bit integer.

Example
Given 32, return 1
Given 5, return 2
Given 1023, return 9

Challenge 
If the integer is n bits with m 1 bits. Can you do it in O(m) time?
```

### 解题思路

将 `num` 和 `num - 1` 求与操作，可消除最右边一位 `1`，如此反复操作，可以在 `O(m)` 时间完成计数。

Java 实现

```java
public class Solution {
    /**
     * @param num: an integer
     * @return: an integer, the number of ones in num
     */
    public int countOnes(int num) {
        // write your code here
        int count = 0;
        while (num != 0) {
            num = num & (num - 1);
            count++;
        }
        return count;
    }
};
```

