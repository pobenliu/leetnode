#  Jump Game

 Jump Game ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/jump-game/) )

```
Description
Given an array of non-negative integers, you are initially positioned at the first index of the array.
Each element in the array represents your maximum jump length at that position.
Determine if you are able to reach the last index.

Notice
This problem have two method which is Greedy and Dynamic Programming.
The time complexity of Greedy method is O(n).
The time complexity of Dynamic Programming method is O(n^2).

We manually set the small data set to allow you pass the test in both ways.
This is just to let you learn how to use this problem in dynamic programming ways. 
If you finish it in dynamic programming ways, you can try greedy method to make it accept again.

Example
A = [2,3,1,1,4], return true.
A = [3,2,1,0,4], return false.
```



### 解题思路

#### 一、动态规划

1. 定义状态：题目的问题——是否存在一条从起点跳到终点的路径，那么可以定义 `canJump[i]` 为从起点 `[0]` 走到 `[i]` 的路径状态， `true` 为存在， `false` 为不存在。
2. 定义状态转移函数：画图观察，是否能够跳到位置 `[i]` ，依赖于 `i` 之前的位置 `j` 的状态  `canJump[j]` ，如果存在 `canJump[j]` 为 `true` 且 `A[j] + j >= i` ，那么说明可以跳到位置 `[i]` 。
3. 定义起点：从数组第一个元素开始，所以起点是 `[0]` ，初始化为 `true` 。
4. 定义终点：也就是最终的结果，是数组最后一个元素 `canJump[n - 1]` 。



Java 实现

```java
public class Solution {
    /**
     * @param A: A list of integers
     * @return: The boolean answer
     */
    public boolean canJump(int[] A) {
        // wirte your code here 
        if (A == null || A.length == 0) {
            return false;
        }
        
        // state
        // boolean 初始化的默认值是 false
        int n =  A.length;
        boolean[] canJump = new boolean[n];
        
        // initialize
        canJump[0] = true;
        
        // top down
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (canJump[j] && (A[j] + j >= i)) {
                    // 此处只需考虑 true 的情况，因为 boolean 默认初始化为 false
                    canJump[i] = true;  
                }
            }
        }
        
        // end
        return canJump[n - 1];
    }
    
}
```



#### 二、贪心

使用变量 `farthest` 表示能够跳到的最远的点，从左到右扫描，根据当前的位置不断更新 `farthest` ，最后对 `farthest` 与数组长度做比较。

Java 实现

```java
public class Solution {
    public boolean canJump(int[] A) {
        // think it as merging n intervals
        if (A == null || A.length == 0) {
            return false;
        }
        int farthest = A[0];
        for (int i = 1; i < A.length; i++) {
            if (i <= farthest && A[i] + i >= farthest) {
                farthest = A[i] + i;
            }
        }
        return farthest >= A.length - 1;
    }
}
```





### 参考

1. [Jump Game | 九章算法](http://www.jiuzhang.com/solutions/jump-game/)
2. [LeetCode: Jump Game Total 解题报告 | Yu's garden](http://www.cnblogs.com/yuzhangcmu/p/4039840.html)

