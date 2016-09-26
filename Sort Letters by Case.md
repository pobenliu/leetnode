#  Sort Letters by Case

 Sort Letters by Case  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/sort-letters-by-case/) )

```
Description
Given a string which contains only letters. Sort it by lower case first and upper case second.

Notice
It's NOT necessary to keep the original order of lower-case letters and upper case letters.

Example
For "abAcD", a reasonable answer is "acbAD"

Challenge 
Do it in one-pass and in-place.
```

### 解题思路

思路与 Partition Array 相同，使用指向数组首尾的双指针，向中间遍历。

Java 实现

```java
public class Solution {
    /** 
     *@param chars: The letter array you should sort by Case
     *@return: void
     */
    public void sortLetters(char[] chars) {
        if (chars == null || chars.length == 0) {
            return;
        }
        
        int left = 0;
        int right = chars.length - 1;
        while (left <= right) {
            while (left <= right && Character.isLowerCase(chars[left])) {
                left++;
            }
            while (left <= right && Character.isUpperCase(chars[right])) {
                right--;
            }
            
            if (left <= right) {
                char temp = chars[left];
                chars[left] = chars[right];
                chars[right] = temp;
            }
        }
        return;
    }
}
```



### 参考

