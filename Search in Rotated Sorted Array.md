# Search in Rotated Sorted Array

Search in Rotated Sorted Array  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/search-in-rotated-sorted-array/) )

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

直观的解法是对数组进行遍历，这样需要 `O(n)` 的时间复杂度。如果要在 `O(logN)` 时间内实现需要考虑使用二分查找。

假设原始数组为升序，如图所示。

![](http://ww4.sinaimg.cn/mw690/600e6311jw1f8m0q12l21j207q05pmx6.jpg)

在旋转之后，可能有以下两种情况：

- 当 `A[mid] > A[start]` 时，对应情况 (a) 。
- 当 `A[mid] < A[end]` 时，对应情况 (b) 。

 ![](http://ww4.sinaimg.cn/mw690/600e6311jw1f8m0uxfvc6j20gg06474r.jpg)

不难看出， `mid` 将旋转序列分为两段，一段是正常的排序数组，另一段是旋转数组，目标值 `target` 可能在任一段。以情况 (a) 为例，当 `A[start] <= target <= A[mid]` 时，当目标值在左侧排序数组上，否则目标值在右侧旋转数组上，以此进行二分查找。

<u>总结一下本题目的思路就是</u>：先判断旋转数组的形状（左边/右边的上升序列更长？），然后判断目标值在哪一侧（正常的排序数组部分上，还是旋转数组部分）。

##### 算法复杂度

- 时间复杂度： `O(log n)` 。
- 空间复杂度： `O(1)` 。

##### 易错点

> 1. 在判断目标值的范围时，考虑到第一个元素 `A[start]` 和最后一个元素 `A[mid]` 可能等于目标值，所以要用大于等于、小于等于。

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

1. [Search in Rotated Sorted Array | 九章算法](http://www.jiuzhang.com/solutions/search-in-rotated-sorted-array/)