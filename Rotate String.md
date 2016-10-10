#  Rotate String

 Rotate String  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/rotate-string/#) )

```
Description
Given a string and an offset, rotate string by offset. (rotate from left to right)

Example
Given "abcdefg".
offset=0 => "abcdefg"
offset=1 => "gabcdef"
offset=2 => "fgabcde"
offset=3 => "efgabcd"

Challenge 
Rotate in-place with O(1) extra memory.
```

### 解题思路

参考题目 Recover Rotated Sorted Array ，使用三步翻转法。

##### 算法复杂度

- 时间复杂度： `O(n)` 。
- 空间复杂度： `O(1)` 。

##### 易错点

> 1. 在写交换函数时，注意临时变量的类型，这里是 `char` 类型。

Java 实现

```java
public class Solution {
    /**
     * @param str: an array of char
     * @param offset: an integer
     * @return: nothing
     */
    public void rotateString(char[] str, int offset) {
        if (str == null || str.length == 0) {
            return;
        }
        
        offset %= str.length;
        if (offset == 0) {
            return;
        }
        
        reverse(str, 0, str.length - offset - 1);
        reverse(str, str.length - offset, str.length - 1);
        reverse(str, 0, str.length - 1);
    }
    
    private void reverse (char[] str, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            char temp = str[i];
            str[i] = str[j];
            str[j] = temp;
        }
    }
}
```

