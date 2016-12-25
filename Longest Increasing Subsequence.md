# Longest Increasing Subsequence

 Longest Increasing Subsequence  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/longest-increasing-subsequence/) )

```
Description
Given a sequence of integers, find the longest increasing subsequence (LIS).
You code should return the length of the LIS.

Clarification
What's the definition of longest increasing subsequence?
- The longest increasing subsequence problem is to find a subsequence of a given sequence 
in which the subsequence's elements are in sorted order, lowest to highest, 
and in which the subsequence is as long as possible. 
This subsequence is not necessarily contiguous, or unique.
https://en.wikipedia.org/wiki/Longest_increasing_subsequence

Example
For [5, 4, 1, 2, 3], the LIS is [1, 2, 3], return 3
For [4, 2, 4, 5, 3, 7], the LIS is [2, 4, 5, 7], return 4

Challenge 
Time complexity O(n^2) or O(nlogn)
```

### 解题思路

最长递增子序列的定义（[维基百科](https://zh.wikipedia.org/zh-cn/%E6%9C%80%E9%95%BF%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97)）：

> 在计算机科学中，**最长递增子序列**（ longest increasing subsequence, LIS ）问题是指，在一个给定的数值序列中，找到一个子序列，使得这个子序列元素的数值依次递增，并且这个子序列的长度尽可能地大。最长递增子序列中的元素在原序列中不一定是连续的。

#### 一、动态规划

1. 定义状态：定义一维状态变量 `f[i]` ，表示以第 `i` 个数字为结尾的 LIS 的长度。
2. 定义状态转移函数：考虑到 <u>**LIS 是非连续的**</u>，对当前状态 `f[i]` ，上一个状态可能为 `f[j], j < i` ，而且此时需要满足 `nums[i-1] > nums[j-1]` ，在所有满足此条件的状态中取最大值 `f[i] = MAX{f[j] + 1, j < i, nums[j-1] < nums[i-1]}`；如果所有 `nums[i-1] <= nums[j-1], j = 1...i-1` ，那么 `f[i] = 1` 。
3. 定义起点： `f[0]` 为前 `0` 个数字中 LIS 的长度，为 `0` 。考虑到对于每一个 `i` ， `f[i]` 至少为 `1` ，所以初始化  `f[i] = 1, i = 1,2...,n-1`。
4. 定义终点：最终结果为 `f[i], i = 1,2...,n-1` 的最大值。

##### 算法复杂度

- 时间复杂度：双重循环，为 `O(n^2)` 。
- 空间复杂度： `O(n)` 。

##### 易错点

> 1. 状态 `f[i]` 的索引和数组的索引  `nums[i - 1]` 有差值，需要注意。
> 2. 需要根据实际意义初始化，可以假设一个极端的情况，一个逆序的排序数组，那么其每个状态都是 `1` 。

Java 实现

```java
public class Solution {
    /**
     * @param nums: The integer array
     * @return: The length of LIS (longest increasing subsequence)
     */
    public int longestIncreasingSubsequence(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int len = nums.length;
        // status
        int[] f = new int[len + 1];
        
        // initialize
        // default : f[1...len] = 1

        for (int i = 1; i <= len; i++) {
            f[i] = 1;
            for (int j = 1; j < i; j++) {
                if (nums[i - 1] > nums[j - 1]) {
                    f[i] = Math.max(f[i], f[j] + 1);
                }
            }
        }
        
        // result
        int max = 0;
        for (int i = 1; i <= len; i++) {
            if (f[i] > max) {
                max = f[i];
            }
        }
        return max;
    }
}
```



#### 二、二分法

实现思路是，建立一个数组 `minLast` ，依次把数组 `nums` 中的元素 `nums[i]` 放入，放入的规则是：在 `minLast` 中找到第一个比 `nums[i]` 大的元素，然后替换之，这样构造的数组是一个排序数组，所以可以用二分查找。最终 `minLast` 数组长度就是 LIS 长度，需要注意的是 `minLast` 中的数并不是 `nums` 的一个 LIS ，只是长度相等。

具体到实现可能会出现这么两种情况：

- 1） `nums[i]` 比 `minLast` 首元素小，直接替换首元素；
- 2） `nums[i]` 比 `minLast` 所有元素都大，会放在数组尾部，数组的长度增加一；
- 2） `nums[i]` 比 `minLast` 首元素小，比尾元素大，找到第一个比 `nums[i]` 的元素然后替换之。

为什么要这么做？以下是推理过程，主要参考了[最长递增子序列(LIS)解法详述](http://www.cppblog.com/jaysoon/articles/148382.html)，理解起来稍微有点绕。

```
           0 1 2 3 4 5 6 7 
nums[i]:   2 5 6 2 3 4 7 4
f[i-1]:  0 1 2 3 1 2 3 4 3 
```

我们来分析一下方法一动态规划的计算过程，在求解 `f[i]` 时，将其与每一个 `f[j], j < i && f[j] < f[i]` ，这导致每个 `f[i]` 的求解复杂度是 `O(n)` ，这里是可以改进的。

通过观察可得，在求解 `f[7]` 时，无需和 `nums[3], nums[4]` 比较，只需与 `nums[5]` 比较即可，也就是说我们考察第 `i` 个元素 `nums[i-1]` 时，如果前 `i - 1` 个元素中的任一个递增子序列，其最后一个元素比 `nums[i-1]` 小，那么就可以把 `nums[i-1]` 放在子序列后面，构成一个更长的递增子序列，不需要再和子序列前面的元素比较。

此为启发一，考察第 `i` 个元素 `nums[i-1]` 时，我们只关心前 `i - 1` 个元素中递增子序列的最后一个元素值。

在求解 `f[7]` 时，前 `6` 个元素中有两个长度为 `3` 的递增子序列 `2, 5, 6` 和 `2, 3, 4` ，而 `nums[6] > 6` 且 `nums[6] > 4` ，所以可将 `nums[6]` 放在任一序列后，构成长度为 `4` 的递增子序列。其实，只要 `nums[6] > 4` 即可接在子序列后面，无需和 `6` 比较。

此外启发二，考察第 `i` 个元素 `nums[i-1]` 时，对于同样长度的递增子序列，我们只关心尾元素中的最小值。

##### 算法复杂度

- 时间复杂度： `n` 次二分查找，为 `O(nlogn)` 。
- 空间复杂度： `O(n)` 。

Java 实现

```java
public class Solution {
    /**
     * @param nums: The integer array
     * @return: The length of LIS (longest increasing subsequence)
     */
    public int longestIncreasingSubsequence(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int len = nums.length;
        int[] minLast = new int[len + 1];
        minLast[0] = -1;
        for (int i = 1; i <= len; i++) {
            minLast[i] = Integer.MAX_VALUE;
        }
        
        for (int i = 0; i < len; i++) {
            // find the first number in minLast > nums[i]
            int index = binarySearch(minLast, nums[i]);
            minLast[index] = nums[i];
        }
        
        for (int i = len; i >= 1; i--) {
            if (minLast[i] != Integer.MAX_VALUE) {
                return i;
            }
        }
        return 0;
    }
    
    // find the first number > num
    private int binarySearch (int[] minLast, int num) {
        int start = 0, end = minLast.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (minLast[mid] < num) {
                start = mid;
            } else {
                end = mid;
            }
        }
        
        if (minLast[start] > num) {
            return start;
        }
        return end;
    }
}
```



### 参考

1. [ Longest Increasing Subsequence | 九章算法](http://www.jiuzhang.com/solutions/longest-increasing-subsequence/)
2. [[LeetCode] Longest Increasing Subsequence 最长递增子序列 | Grandyang](http://www.cnblogs.com/grandyang/p/4938187.html)
3. [最长递增子序列(LIS)解法详述 | 杰 & C++ & Python & DM](http://www.cppblog.com/jaysoon/articles/148382.html)