# Backpack III

Backpack III  ( [lintcode]() )

```
Description
Given n kind of items with size Ai and value Vi( each item has an infinite number available) 
and a backpack with size m. 
What's the maximum value can you put into the backpack?

Notice
You cannot divide item into small pieces and the total size of items you choose should smaller or equal to m.

Example
Given 4 items with size [2, 3, 5, 7] and value [1, 5, 2, 4], and a backpack with size 10. 
The maximum value is 15.
```

### 解题思路

动态规划。

1. 定义状态：定义 `f[j]` 为前 `i` 个数填满大小为 `j` 的背包，最大值是多少。
2. 定义状态转移函数：对于当前状态 `f[j]` ，
   - 如果不计入 `A[i]` ，则 `f[j]` 保持状态不变；
   - 如果计入 `A[i]` ，需满足条件 `j > A[i]` ，那么 `f[j]` 上一个状态是 `f[j - A[i]]` 
   - 对于当前物品 `i`，若 `j` 从小到大的话，很可能在 `j` 之前的 `j - A[i]` 时已经放过第 `i` 件物品了，在 `j` 时再放就是重复放入；若 `j` 从大到小，则 `j` 之前的所有情况都没有更新过，不可能放过第 `i` 件物品，所以不会重复放入。【DH：想得不是很明白...】
3. 定义起点：
   - `f[0]` 状态为 `0` 。
4. 定义终点：最终结果即为 `f[m]` 。

Java 实现

```java
public class Solution {
    /**
     * @param A an integer array
     * @param V an integer array
     * @param m an integer
     * @return an array
     */
    public int backPackIII(int[] A, int[] V, int m) {
	    if (A == null || A.length == 0 ||
  			V == null || V.length == 0 ||
  			m <= 0) {
  			return 0;		
  	    }
        int[] f = new int[m + 1];

        for(int i = 0; i < A.length; i++){
            for(int j = 1; j <= m; j++){
                if (j >= A[i]) {
  				f[j] = Math.max(f[j], f[j - A[i]] + V[i]);
  			   }
            }
        }
        return f[m];
    }
}
```



### 参考

1. [[LintCode] Backpack I II III IV V VI [背包六问]](https://segmentfault.com/a/1190000006325321)
2. [Backpack III 440 | LintCode题解](https://zhengyang2015.gitbooks.io/lintcode/content/backpack_iii_440.html)

