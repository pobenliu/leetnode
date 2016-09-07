# Jump Game II

Jump Game II ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/jump-game-ii/) )

```
Description
Given an array of non-negative integers, you are initially positioned at the first index of the array.
Each element in the array represents your maximum jump length at that position.
Your goal is to reach the last index in the minimum number of jumps.

Example
Given array A = [2,3,1,1,4]
The minimum number of jumps to reach the last index is 2. 
(Jump 1 step from index 0 to 1, then 3 steps to the last index.)
```



### 解题思路

#### 一、动态规划

1. 定义状态：题目的问题——从起点跳到终点的最小跳数，定义 `minSteps[i]` 为从起点 `[0]` 走到 `[i]` 的最小跳数。
2. 定义状态转移函数： `i` 之前的位置 `j` ，如果可以跳到 `i` ，那么 `minSteps[i] = minSteps[j] + 1` ，由于可能存在多条路径，所以对 `j` 的所有可能取值，应该找到 `minSteps[j]` 最小的那个位置。
3. 定义起点：从数组第一个元素开始，所以起点是 `[0]` ，初始化为 `0` 。
4. 定义终点：也就是最终的结果，是数组最后一个元素 `minSteps[n - 1]` 。



Java 实现

```java
public class Solution {
    /**
     * @param A: A list of lists of integers
     * @return: An integer
     */
    public int jump(int[] A) {
        // write your code here
        if (A == null || A.length == 0) {
            return -1;
        }
        
        // state
        int n = A.length;
        int[] minSteps = new int[n];
        
        // initialize
        minSteps[0] = 0;
        
        // top down
        for (int i = 1; i < n; i++) {
            minSteps[i] = Integer.MAX_VALUE;
            for (int j = 0; j < i; j++) {
                if (minSteps[j] != Integer.MAX_VALUE && A[j] + j >= i) {
                    minSteps[i] = minSteps[j] + 1;
                    break;
                }
            }
        }
        
        // end
        return minSteps[n - 1];
    }
}

```



#### 二、贪心

从左往右扫描，维护一个覆盖区间，每到达一个位置，重新计算覆盖区间的边界。

- 覆盖区间为 `[start, end]` 。
- 计算覆盖区间内每个位置 `A[i] + i` 的值，并将得到的最大值保存为 `farthest` 。
- 将 `farthest`设为 `end` ，将 `end + 1` 设为 `start` 。

Java 实现

```java
public class Solution {
    public int jump(int[] A) {
        if (A == null || A.length == 0) {
            return -1;
        }
        int start = 0, end = 0, jumps = 0;
        while (end < A.length - 1) {
            jumps++;
            int farthest = end;
            for (int i = start; i <= end; i++) {
                if (A[i] + i > farthest) {
                    farthest = A[i] + i;
                }
            }
            start = end + 1;
            end = farthest;
        }
        return jumps;
    }
}
```



### 参考

1. [Jump Game II | 九章算法](http://www.jiuzhang.com/solutions/jump-game-ii/)
2. [LeetCode: Jump Game II 解题报告 | Yu's garden](http://www.cnblogs.com/yuzhangcmu/p/4148858.html)

