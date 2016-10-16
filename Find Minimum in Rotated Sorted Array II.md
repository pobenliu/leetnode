#  Find Minimum in Rotated Sorted Array II

 Find Minimum in Rotated Sorted Array II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/find-minimum-in-rotated-sorted-array-ii/#) )

```
Description
Suppose a sorted array is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
Find the minimum element.

Notice
The array may contain duplicates.

Example
Given [4,4,5,6,7,0,1,2] return 0.
```

### 解题思路

本题目要考虑重复元素，那么考虑一个极端的情况原数组 `[0, 1, 1, 1, 1, … , 1]` ，其旋转数组为  `[1, 1, ..., 1, 0, 1, ... , 1, 1]` ，该情况使用二分法无法确保得到正确结果，需要结合遍历来考虑。

Java 实现

```java
public class Solution {
    /**
     * @param num: a rotated sorted array
     * @return: the minimum number in the array
     */
    public int findMin(int[] nums) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int start = 0, end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] == nums[end]) {
                end--;
            } else if (nums[mid] < nums[end]){
                end = mid;
            } else {
                start = end;
            }
        }
        if (nums[start] <= nums[end]) {
            return nums[start];
        }
        return nums[end];
    }
}
```



### 参考

1. [Find Minimum in Rotated Sorted Array II | 九章算法](http://www.lintcode.com/en/problem/find-minimum-in-rotated-sorted-array-ii/#)