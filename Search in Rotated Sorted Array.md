# Search in Rotated Sorted Array

Search in Rotated Sorted Array  ( [leetcode][] [lintcode](http://www.lintcode.com/en/problem/search-in-rotated-sorted-array/) )

```
Description
Suppose a sorted array is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
You are given a target value to search. 
If found in the array return its index, otherwise return -1.
You may assume no duplicate exists in the array.

Example
For [4, 5, 1, 2, 3] and target=1, return 2.
For [4, 5, 1, 2, 3] and target=0, return -1.

Challenge
O(logN) time
```

### 解题思路

`O(logN)` 时间实现需要考虑使用二分查找。旋转排序（假设为上升）数组画出来就是一个锯齿的形状，分别是两段上升序列。`mid` 一定在较长的上升序列上，画图可以看出，`mid` 切分数组后，分别是一个上升序列，一个旋转上升序列。所以先判断 `mid` 在哪一段上升序列上面，然后先分析 `target` 位于上升序列的情况， `target` 位于旋转上升序列的情况交给 `else` 处理。

文字说的太绕，回头找时间补张图。

Java 实现

```java
public class Solution {
    /**
     *@param A : an integer rotated sorted array
     *@param target :  an integer to be searched
     *return : an integer
     */
    public int search(int[] A, int target) {
        // write your code here
        if (A == null || A.length == 0) {
            return -1;
        }

        int start = 0, end = A.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (A[mid] == target) {
                return mid;
            } else if (A[start] < A[mid]) {     // 根据mid所在位置划分不同情况
                if ((A[start] <= target) && (target <= A[mid])) {
                    end = mid;
                } else {
                    start = mid;
                }
            } else if (A[mid] < A [end]) {
                if ((A[mid] <= target) && (target <= A[end])) {
                    start = mid;
                } else {
                    end = mid;
                }
            }
        }  // while end
        if (A[start] == target) {
            return start;
        }
        if (A[end] ==  target) {
            return end;
        }
        return -1;
    }
}
```



### 参考



