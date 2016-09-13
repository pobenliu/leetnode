#  Longest Consecutive Sequence

 Longest Consecutive Sequence  ( [leetcode]() [lintcode]() )

```
Description
Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Clarification
Your algorithm should run in O(n) complexity.

Example
Given [100, 4, 200, 1, 3, 2],
The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.
```



### 解题思路

题目为求最长的连续序列，容易想到动态规划，但本题是一个特例，并不是考察动规，而是考察数据结构。

如果不考虑算法复杂度，直观的想法是先将数组排序，然后遍历寻找，然而基于比较的排序复杂度至少是 `O(nlogn)` ，所以无法使用。考虑使用哈希表。

- 将无序数组中的每个数存入 `HashSet` 中。
- 对于序列中的任一个数 `num[i]` ，
  - 判断 `num[i - 1], num[i - 2], num[i - 3]... ` 是否在序列中（循环），搜索到的数从 `HashSet` 中删除以避免重复搜索。
  - 判断 `num[i + 1], num[i + 2], num[i + 3]... ` 是否在序列中（循环），搜索到的数从 `HashSet` 中删除以避免重复搜索。
  - 进而找到整个连续序列。

**算法复杂度**

- 时间复杂度：`HashSet` 的操作 `add(), remove(), contains()` 的时间复杂度为 `O(1)` ，每个数的只有一次 `add(), remove()` 操作，所以复杂度为 `O(n)` 。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return an integer
     */
    public int longestConsecutive(int[] num) {
        if (num == null || num.length == 0) {
            return 0;
        }
        
        HashSet<Integer> set = new HashSet<Integer>();
        for (int i = 0; i < num.length; i++) {
            set.add(num[i]);
        }
        
        int longest = 0;
        for (int i = 0; i < num.length; i++) {
            int down = num[i] - 1;
            while (set.contains(down)) {
                set.remove(down);
                down--;
            }
            
            int up = num[i] + 1;
            while (set.contains(up)) {
                set.remove(up);
                up++;
            }
            // between down and up are the longest consecutive sequence
            longest = Math.max(longest, up - down - 1);
        }
        
        return longest;
    }
}
```



### 参考

1. [[LeetCode] Longest Consecutive Sequence | 喜刷刷](http://bangbingsyb.blogspot.jp/2014/11/leetcode-longest-consecutive-sequence.html)



