# Search for a Range

Search for a Range ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/search-for-a-range/) )

```
Given a sorted array of integers, find the starting and ending position of a given target value.
Your algorithm's runtime complexity must be in the order of O(log n).
If the target is not found in the array, return [-1, -1].

For example,
Given [5, 7, 7, 8, 8, 10] and target value 8,
return [3, 4].
```



### 解题思路

数组中含重复元素，要寻找某个目标值的范围（起始位置），先找第一次出现的位置，再找最后一次出现的位置。具体实现可参考题目 First Position of Target 。

易错点：

> 1. 在函数返回值时，建立数组并直接进行初始化：`return new int[] {-1, -1};`。
>
>    直接声明初始化数组：`int[] a = {1, 2, 3, 4};`。

Java 实现

```java
public class Solution {
    /**
     *@param A : an integer sorted array
     *@param target :  an integer to be inserted
     *return : a list of length 2, [index1, index2]
     */
    public int[] searchRange(int[] A, int target) {
        
        if (A == null || A.length == 0) {
            return new int[] {-1, -1};     // attention: how Java function return array
        }

        int start = 0, end = A.length - 1;
        int first, last;
        // find first target
        while (start + 1 < end) {
            int mid = start + (end -  start) / 2;
            if (A[mid] >= target) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if (A[start] == target) {
            first = start;
        } else if (A[end] == target){
            first = end;
        } else {
            first = last = -1;
            return new int[] {first, last};     // return two dimension array
        }

        // find last target
        start = 0;
        end = A.length - 1;
        while (start + 1 < end) {
            int mid = start + (end -  start) / 2;
            if (A[mid] <= target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if (A[end] == target) {
            last = end;
        } else if (A[start] == target) {
            last = start;
        } else {
            last = first = -1;
            return new int[] {first, last};
        }
        return new int[] {first, last};    // return the final position
    }
}
```

