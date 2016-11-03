# Backpack

Backpack  ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/backpack/) )

```
Description
Given n items with size Ai, an integer m denotes the size of a backpack. 
How full you can fill this backpack?

Notice
You can not divide any item into small pieces.

Example
If we have 4 items with size [2, 3, 5, 7], the backpack size is 11, we can select [2, 3, 5], 
so that the max size we can fill this backpack is 10. 
If the backpack size is 12. we can select [2, 3, 7] so that we can fulfill the backpack.
You function should return the max size we can fill in the given backpack.

Challenge 
O(n x m) time and O(m) memory.
O(n x m) memory is also acceptable if you do not know how to optimize memory.
```



### 解题思路

背包问题：单次选择+最大体积。

#### 一、二维状态

1. 定义状态：定义 `f[i][j]` 为 `A` 的前 `i` 个数是否能填满大小为 `j` 的背包。
2. 定义状态转移函数：对于当前状态 `f[i][j]` ，
   - 如果不计入 `A[i - 1]` ，则 `f[i][j]` 对应的上一个状态是 `f[i - 1][j]` ；
   - 如果计入 `A[i - 1]` ，满足条件 `j >= A[i - 1]` ，那么 `f[i][j]` 上一个状态是 `f[i - 1][j - A[i - 1]]` 
   - 以上两种情况满足其一即可。
3. 定义起点：
   - <u>**对于二维的动规问题，`f[i][0]` 和 `f[0][j]` 都是边界条件，需要进行初始化，初始化时要同时注意边界的状态转移。**</u>
   - 当 `i != 0` ，`j == 0` 时，如果一个数字都不计入，即取前 `i` 个数字中的零个，那么可得结果为零，所以 `f[i][0]` 的状态为 `true` 。
   - 当 `i == 0` ，`j != 0` 时，前零个元素的和显然无法得到值 `j`， `f[0][j]` 状态为 `false` 。
   - `f[0][0]` 状态为 `true` 。
4. 定义终点：最终结果即为 `f[n][m]` 。<u>**注：初始化时也需要注意取值范围是数组长度加一。**</u>

易错点： 

> 1. 状态函数 `f[][]` 的初始化范围为 `boolean[][] f = new boolean[n + 1][m + 1];` 。
> 2. 双重循环的起始值，结束值。
> 3. `j` 和 `A[i - 1]` 之间的关系，可以等于。

##### 算法复杂度

- 时间复杂度：`O(n*m)`。
- 空间复杂度：`O(n*m)`。

Java 实现

```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        
        int n = A.length;
        // state
        boolean[][] f = new boolean[n + 1][m + 1];
        
        // initialize
        f[0][0] = true;
        
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= m; j++) {
                f[i][j] = f[i - 1][j] || (j >= A[i - 1] && f[i - 1][j - A[i - 1]]) ;
            }
        }
        
        // answer
        for (int j = m; j >= 0; j--) {
            if (f[n][j]) {
                return j;
            }
        }
        return 0;
    }
}
```

使用滚动数组优化空间，可使空间复杂度降至 `O(m)`。

```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        if (m <= 0 || A == null || A.length == 0) {
            return 0;
        }
        
        int n = A.length;
        // status
        boolean[][] f = new boolean[2][m + 1];
        // initialize
        f[0][0] = true;
        for (int i = 1; i <= m; i++) {
            f[0][i] = false;
        }
        
        for (int i = 1; i <= n; i++) {
            f[i % 2][0] = true;
            for (int j = 1; j <= m; j++) {
                f[i % 2][j] = f[(i - 1) % 2][j] || 
                                ((A[i-1] <= j) && f[(i - 1) % 2][j - A[i - 1]]);
            }
        }
        
        int res = 0;
        for (int i = m; i >= 0; i--) {
            if(f[n % 2][i]) {
                res = i;
                break;
            }
        }
        return res;
    }
}
```



#### 二、一维状态

1. 定义状态：定义 `f[j]` 为 `A` 的前 `i` 个数是否能填满大小为 `j` 的背包。
2. 定义状态转移函数：对于当前状态 `f[j]` ，
   - 如果不计入 `A[i - 1]` ，则 `f[j]` 保持状态不变；
   - 如果计入 `A[i - 1]` ，满足条件 `j > A[i - 1]` ，那么 `f[j]` 上一个状态是 `f[j - 1]` 
   - 注意题目中的<u>**隐含条件为每个数只能使用一次**</u>。在使用二维数组定义状态是，其中一个维度避免了同一个数字的重复使用。所以在使用一维状态转移函数时，要避免出现这种情况，在内循环的取值需要从大到小。
3. 定义起点：
   - `f[0]` 状态为 `true` 。
4. 定义终点：最终结果即为 `f[m]` 。

##### 算法复杂度

- 时间复杂度：`O(nm)`。
- 空间复杂度：`O(m)`。

Java 实现

```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        
        int n = A.length;
        // state
        boolean[] f = new boolean[m + 1];
        
        // initialize
        f[0] = true;
        
        for (int i = 1; i <= n; i++) {
            // j starts from m to avoid repeating use of iterm A[i - 1]
            for (int j = m; j >= 0; j--) {
                if (j >= A[i - 1] && f[j - A[i - 1]]) {
                    f[j] = f[j - A[i - 1]];
                }
            }
        }
        
        // answer
        for (int j = m; j >= 0; j--) {
            if (f[j]) {
                return j;
            }
        }
        return 0;
    }
}
```



### 参考

1. [Lintcode: Backpack | neverlandly](http://www.cnblogs.com/EdwardLiu/p/4269149.html)
2. [Backpack | lintcode题解](https://lefttree.gitbooks.io/leetcode/content/dynamicProgramming2/backpack.html)



