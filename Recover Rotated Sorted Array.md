# Recover Rotated Sorted Array

Recover Rotated Sorted Array  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/recover-rotated-sorted-array/#) )

```
Description
Given a rotated sorted array, recover it to sorted array in-place.

Clarification
What is rotated array?
For example, the orginal array is [1,2,3,4], 
The rotated array of it can be [1,2,3,4], [2,3,4,1], [3,4,1,2], [4,1,2,3]

Example
[4, 5, 1, 2, 3] -> [1, 2, 3, 4, 5]

Challenge 
In-place, O(1) extra space and O(n) time.
```

### 解题思路

本题目用到一个小技巧叫做“**三步翻转法**”，以题目中的数组为例。

```
原始数组：	 [4, 5, 1, 2, 3]
先找到转折点： [4, 5] [1, 2, 3]
第一步翻转： 	 [5, 4]
第二步翻转：          [3, 2, 1]
第三步翻转：   [1, 2, 3, 4, 5]
```

##### 算法复杂度

- 时间复杂度：对整个数组做翻转，复杂度为 `O(n)` 。
- 空间复杂度： `O(1)` 。

##### 易错点

> 1. ArrayList 的相关操作。设置第 `i` 个元素的值， `nums.set(i, value)` 。
> 2. 需要考虑输入数组没有翻转的情况，将三步翻转放在 `for` 循环中可以避免单独处理该情况，也即如果循环判断所有元素都小于下一个元素，那么不会进行任何操作。

Java 实现：

```java
public class Solution {
    /**
     * @param nums: The rotated sorted array
     * @return: void
     */
    public void recoverRotatedSortedArray(ArrayList<Integer> nums) {
        if (nums == null || nums.size() == 0) {
            return;
        }

        for (int i = 0; i < nums.size() - 1; i++) {
            if (nums.get(i) > nums.get(i + 1)) {
                reverse(nums, 0, i);
                reverse(nums, i + 1, nums.size() - 1);
                reverse(nums, 0, nums.size() - 1);
                return;
            }
        }
        
    }
    
    private void reverse (ArrayList<Integer> nums, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            int temp = nums.get(i);
            nums.set(i, nums.get(j));
            nums.set(j, temp);
        }
    }
}
```



### 参考

1. [Recover Rotated Sorted Array | 九章算法]()