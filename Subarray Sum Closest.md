#  Subarray Sum Closest

 Subarray Sum Closest  ( [leetcode]()  [lintcode]() )

```
Description
Given an integer array, find a subarray with sum closest to zero. 
Return the indexes of the first number and last number.

Example
Given [-3, 1, 1, -3, 5], return [0, 2], [1, 3], [1, 1], [2, 2] or [0, 4].

Challenge 
O(nlogn) time
```



### 解题思路

题目要求的是和的绝对值最小的子数组，结合数字前缀和，子数组和的绝对值最小，等价于两个前缀和的差（的绝对值）最小，结合题目对时间复杂度的要求，对前缀和进行排序，然后寻找差值最小的前缀和即可。

- 建立 `Pair` 数据结构，分别存储索引 `i` 及其对应的前缀和 `sum[i - 1]` 。
- 遍历求数组所有的前缀和，并将其及对应索引加入 `Pair` 。
- 对 `Pair` 中的前缀和排序，此处需定义比较运算符。
- 寻找差值最小的前缀和，并存储对应的索引值。此处需注意索引的大小可能是颠倒的。

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number 
     *          and the index of the last number
     */
    
    class Pair {
        int sum;
        int index;
        public Pair (int sum, int index) {
            this.sum = sum;
            this.index = index;
        }
    }
    
    public int[] subarraySumClosest(int[] nums) {
        int[] res = new int[2];
        if (nums == null || nums.length == 0) {
            return res;
        }
        
        int len = nums.length;
        // corner case : if there is only one element
        if (len == 1) {
            res[0] = res[1] = 0;
            return res;
        }
        
        Pair[] sums = new Pair[len + 1];
        int prev = 0;
        sums[0] = new Pair(0, 0);
        for (int i = 1; i <= len; i++) {
            sums[i] = new Pair(prev + nums[i - 1], i);
            prev = sums[i].sum;
        }
        
        Arrays.sort(sums, new Comparator<Pair> () {
            public int compare (Pair a, Pair b) {
                return a.sum - b.sum;
            }
        });
        
        int ans = Integer.MAX_VALUE;
        for (int i = 1; i <= len; i++) {
            if (ans > sums[i].sum - sums[i - 1].sum) {
                ans = sums[i].sum - sums[i - 1].sum;
                int[] temp = new int[]{sums[i].index - 1, sums[i - 1].index - 1};
                Arrays.sort(temp);
                res[0] = temp[0] + 1;
                res[1] = temp[1];
            }
        }
        
        return res;
    }
}
```



### 参考

1. [Subarray Sum Closest | 九章算法](http://www.jiuzhang.com/solutions/subarray-sum-closest/)