# Sort Colors

 Sort Colors  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/sort-colors/) )

```
Description
Given an array with n objects colored red, white or blue, 
sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.
Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Notice
You are not suppose to use the library's sort function for this problem. 
You should do it in-place (sort numbers in the original array).

Example
Given [1, 0, 1, 2], sort it in-place to [0, 1, 1, 2].

Challenge 
A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
Could you come up with an one-pass algorithm using only constant space?
```

### 解题思路

本题目“是 E. W. Dijkstra 的`荷兰国旗问题`引发的一道经典编程练习，因为这就好像用三种可能的主键值将数组排序一样，这三种主键值对应着荷兰国旗上的三种颜色”。

Dijkstra采用的解法为“三向切分”法，“从左向右遍历数组一次，维护一个指针 left 使得 nums[0, 1, …, left-1] 中的元素都小于 1，一个指针 right 使得 nums[right+1, … , len-1] 中的元素都大于 1 ，一个指针 i 使得 nums[left, …, i-1] 中的元素全部等于 1 ，nums[i, … right] 中的元素尚未确定”。开始时 i 和 left 相等，对 nums[i] 进行三向比较处理以下情况：

- nums[i] < 1 ，交换 nums[left] 和 nums[i] ，将 left 和 i 分别加一；
- nums[i] > 1 ，交换 nums[right] 和 nums[i] ，将 right 减一；
- nums[i] == 1 ， 将 i 加一。

借用 [[LeetCode] Sort Colors](http://bangbingsyb.blogspot.jp/2014/11/leetcode-sort-colors.html) 中的图来说明会更清楚

0......0   1......1   x1 x2 .... xm   2.....2

              |            |               |

            left           i             right

Java 实现

```java
Description
class Solution {
    /**
     * @param nums: A list of integer which is 0, 1 or 2 
     * @return: nothing
     */
    public void sortColors(int[] nums) {
        if (nums == null || nums.length == 0) {
            return;
        }
        
        int left = 0;
        int right = nums.length - 1;
        int i = left;
        while (i <= right) {
            if (nums[i] < 1) {
                exch(nums, left, i);
                left++;
                i++;
            } else if (nums[i] > 1) {
                exch(nums, i, right);
                right--;
            } else {
                i++;
            }
        }
        
        return;
    }
    
    private void exch(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```



### 参考

1. [《算法（第4版）》](https://book.douban.com/subject/10432347/) 2.3 快速排序 / 2.3.3.2 三取样切分 
2. [[LeetCode] Sort Colors | 喜刷刷](http://bangbingsyb.blogspot.jp/2014/11/leetcode-sort-colors.html) 